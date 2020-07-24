# JPA(Java Persistent API)

## Repository

JPA에서 데이터베이스에 접근하기 위한 객체로 인터페이스로 생성





## JPA Query

* find...By...
* read...By...
* query...By...
* count...By...



## JPA QueryDSL

MyBatis에서 동적 쿼리를 만들어 사용하듯이 JPA에서 동적 쿼리를 사용하기 위해서는 QueryDSL을 사용

> **QuerDSL**
>
> 오픈소스로서 자바 코드로 작성된 일종의 JPQL(Java Persistence Query Language) 빌더
>
> JPA, JDBCLucene, Hibernate Search, MongoDB, 자바 컬렉션 등을 지원



### 실행

``` 
com.querydsl.apt.jpa.JPPAAnnotationProcessor
```



### 예제 코드

DynamicRepository 생성

```java
public interface DynamicBoardRepository extends CrudRepository<Board, Long>, QuerydslPredicateExecutor<Board> {

}
```



DynamicQueryTest 생성

: MyBatis의 동적 쿼리처럼 searchCondition의 값에 따라 서로 다른 builder를 저장하고 Pageable에서 paging 설정을 해 다음과 같이 쿼리 실행

```java
@RunWith(SpringRunner.class)
@SpringBootTest
public class DynamicQueryTest {
	@Autowired
	private DynamicBoardRepository boardRepo;

	@Test
	public void testDynamicQuery() {
		String searchCondition = "CONTENT";
		String searchKeyword = "Test Content 10";

		
		BooleanBuilder builder = new BooleanBuilder();
		
		QBoard qboard = QBoard.board;
		
		if(searchCondition.equals("TITLE")) {
			builder.and(qboard.title.like("%" + searchKeyword + "%"));
		} else if(searchCondition.equals("CONTENT")) {
			builder.and(qboard.content.like("%" + searchKeyword + "%"));
		}		
		
		Pageable paging = PageRequest.of(0, 5);
		
		Page<Board> boardList = boardRepo.findAll(builder, paging);
				
		System.out.println("검색 결과");
		for (Board board : boardList) {
			System.out.println("---> " + board.toString());
		}
	}
}
```







## 참고

* [https://velog.io/@jayjay28/JPA%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-%EB%8B%A8%EC%88%9C-%EA%B2%8C%EC%8B%9C%EB%AC%BC-%EC%B2%98%EB%A6%AC](https://velog.io/@jayjay28/JPA를-이용한-단순-게시물-처리)
* [https://medium.com/@ish453525/springboot-jpa%EC%9D%98-querydsl-b7ec9b4d19a8](https://medium.com/@ish453525/springboot-jpa의-querydsl-b7ec9b4d19a8)