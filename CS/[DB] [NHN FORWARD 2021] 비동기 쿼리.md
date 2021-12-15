# [NHN FORWARD 2021] Java에서 MySQL 비동기 쿼리 사용하기

* 2021.12.14 NHN FORWARD `Java에서 MySQL 비동기 쿼리 사용하기` session 정리
* https://forward.nhn.com/2021/sessions/11 

## 전통적인 동기 방식의 쿼리와 문제

* 쿼리 요청 (Thread) => 쿼리 작업 (DB) => 결과 응답
  * ![image-20211215233703478](C:\Users\ksb94\AppData\Roaming\Typora\typora-user-images\image-20211215233703478.png)
* 쿼리 작업 시 **응답이 올 때까지 대기**해야하는 문제 발생
* 이를 위해서 `Connection Pool`, `Thread Pool`이 사용되었다.

### Connection Pool & Thread Pool

* 미리 DB와 Connection을 연결해 둔 다음
* 요청이 오는 경우 쿼리 작업 후 결과 응답

**BUT**

* 커넥션 풀, 스레드 풀의 사이즈를 적절하게 사용해야하는 문제
* 동시 접속자가 많을 경우 connection이 없을 경우 반환될 때까지 대기
* 서버가 여유가 있어도 데이터베이스 병목(처리 대기 시간)으로 인해 서버를 늘리게 되는 경우 발생



* [참고: Spring-3-커넥션-풀이란](https://linked2ev.github.io/spring/2019/08/14/Spring-3-%EC%BB%A4%EB%84%A5%EC%85%98-%ED%92%80%EC%9D%B4%EB%9E%80/)
* [참고: DB Connection Pool](https://hyuntaeknote.tistory.com/12)

* [참고: Thread Pool(스레드 풀) 이란](https://limkydev.tistory.com/55)
* [참고: MySQL을 시작하기 전에 3](https://dung-beetle.tistory.com/68)

## 비동기 처리

* 동기 방식의 문제를 처리하기 위해 비동기 처리 방식을 사용

### 비동기 처리?

* 응답을 대기하지 않고 계속적으로 처리
  * ![image-20211215233945745](C:\Users\ksb94\AppData\Roaming\Typora\typora-user-images\image-20211215233945745.png)

## MySQL 비동기 지원

* X DevAPI
* Kotlin jasync SQL

### X DevAPI

![image-20211216003211953](C:\Users\ksb94\AppData\Roaming\Typora\typora-user-images\image-20211216003211953.png)

* 새로 추가된 부분
  * X Protocol
  * NoSQL을 몰라도 개발 가능

* 사용

  * ```java
    import com.mysql.cj.xdevapi.*;
    
    // Connect to server on localhost using a connection URI
    Session mySession = new SessionFactory.getSession("mysqlx://localhost:33060/...");
    
    Schema myDb = mySession.getSchema("test");
    Table employees = myDb.getTable("employee");
    
    // execute the query asynchronously, obtain a future
    CompletableFuture<RowResult> rowsFuture = employees.select("name", "age")
    .where("name like: name")
    .orderBy("name")
    .bind("name". "m?%").executeAsync();
    
    // 비동기 요청 후 필요한 작업 수행
    
    // wait until it's ready
    RowResult rows = rowsFuture.get();	// 결과 얻기
    
    ```

  * getSession => CompletableFuture => executeAsync() => 로직 수행 => rowsFuture.get()

* **ISSUE**
  * session이 많아질수록 성능이 떨어짐 (session이 1개일 경우 가장 성능이 좋음)
  * ![image-20211215234437651](C:\Users\ksb94\AppData\Roaming\Typora\typora-user-images\image-20211215234437651.png)
  * 원인 분석
    * executeAsync()를 요청할 때 마다 dispatchingThreadMonitor에 lock을 걸고있음
    * ListenerDispatcher => while문 마다 lock을 걸고있음

### jasync SQL

* 사용

  * ```java
    PoolConfiguration poolConfiguration = new PoolConfiguration(
    	100,							// maxObjects
    	TimeUnit.MINUTES.toMillis(15),	// maxIdle
    	10_000,							// maxQueueSize
    	TimeUnit.SECONDS.toMillis(30)	// validationInterval
    );
    
    Connection connection = new ConnectionPool<>(
    	new MySQLConnectionFactory(new Configuration("username", "host.com", 3306, "password", "schema")), poolConfiguration
    );
    
    connection.connect().get();
    
    CompletableFuture<QueryResult> future = connection.sendPreparedStatement("select * from table limit 2");
    
    // 비동기 요청 후 필요한 작업을 수행
    
    QueryResult queryResult = future.get();
    ```

  * ConnectionPool => connection.sendPreparedStatement() => 로직 수행 => future.get();

* X DevAPI와 같은 성능문제는 발생하지 않고 있음

### X DevAPI vs. jasync-SQL

![image-20211215234934537](C:\Users\ksb94\AppData\Roaming\Typora\typora-user-images\image-20211215234934537.png)

* R2DBC 내부적으로 jasync-SQL 사용

## 비동기로 얻을 수 있는 것

* 코드 생산성
* 성능

### 코드 생산성

첫번째 row 출력

#### 동기 방식

```java
Executors.newFixedThreadPool(250).execute(() -> {
	SqlSession sqlSession = GameSqlSessionFactory.getSqlSession();
	try{
		UserDataMapper userDataMapper = sqlSession.getMapper(UserDataMapper.class);
		List<Map<String, Object>> userList = userDataMapper.selectUser();
		user.forEach(logger::info);
	} finally {
		sqlSession.close();
	}
});
```

#### 비동기 방식

```java
CompletableFuture<QueryResult> future = connection.sendPreparedStatement(sql)
	.thenApplyAsync(r -> new ArrayList<>(r.getRows().get(0).columns()))
	.thenAccept(r -> r.foreache(logger::info));
```

=> 라이브러리가 대신 처리해주므로 code를 blocking할 필요가 없음

* Redis: jedis (동기) -> lettuce (비동기)
* [참고: Jedis 보다는 Lettuce를 쓰자](https://abbo.tistory.com/107)

### 성능

#### 테스트

![image-20211215235500538](C:\Users\ksb94\AppData\Roaming\Typora\typora-user-images\image-20211215235500538.png)

#### Stored Procedure

API방식으로 Transaction을 처리할 때 성능이 낮아지는 문제를 어느정도 줄일 수 있다.

![image-20211215235525361](C:\Users\ksb94\AppData\Roaming\Typora\typora-user-images\image-20211215235525361.png)

## 정리

* 동기: MyBatis, HIBERNATE
* 비동기: X DevAPI, jasync SQL



* 비동기 방식의 경우 **코드 생산성과 가독성, 성능이 우수**하다.
* API방식으로 Transaction을 처리할 때 성능이 낮아지는 문제를 Stored Procedure에서 Transaction을 처리하는 방식을 통해 성능을 높일 수 있다.
* 그러나, X DevAPI는 세션이 늘어나면 성능이 더 안좋아지는 issue가 있다.



### 개인적으로 더 알아봐야 할 것

* CompletableFuture
* Jedis, Lettuce
* Zeko SQL 
* Stored Procedure
* 200만 동접 게임을 위한 MySQL 샤딩 (NHN FORWARD 2019)