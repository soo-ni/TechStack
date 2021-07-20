# JPA(Java Persistent API)

## JPA란?

* ORM표준 기술로, 인터페이스의 모음
* JPA 2.1 표준 명세를 구현한 3가지 구현체: Hibernate, EclipseLink, DataNucleus
  <img src="https://gmlwjd9405.github.io/images/inflearn-jpa/implementation-of-jpa.png">



> **TIP**
>
> ORM(Object Relational Mapping)이란?
>
> * 객체 관계 매핑으로, 객체는 객체대로 설계하고 관계형 데이터베이스는 관계형 데이터베이스대로 설계
> * ORM 프레임워크가 중간에서 매핑
>
> EJB(Enterprise Java Bean) 이란?
>
> * 과거의 ORM
>
> Hibernate
>
> * ORM 프레임워크, Open Source SW
> * JPA 인터페이스를 구현한 대표적인 오픈소스

장점

* 객체 지향적으로 관리
* 테이블 생성, 변경, 관리가 쉬움
  * 간단한 CRUD
    * 저장: jpa.persist(entity)
    * 조회: jpa.find(entityID)
    * 수정: entity.setName("updateName")
    * 삭제: jpa.remove(entity)
* 로직을 쿼리보다는 객체 자체에 집중
* 빠른 개발

단점

* 데이터 손실이 있을 수 있음 (persistence context)
* 성능 문제



## JPA 동작 과정

<img src="https://gmlwjd9405.github.io/images/inflearn-jpa/jpa-basic-structure.png">

* 애플리케이션과 JDBC 사이에서 동작
  * 개발자가 JPA 사용 => JPA 내부에서 JDBC API를 사용하여 SQL호출해 DB와 통신



### 저장 과정

<img src="https://gmlwjd9405.github.io/images/inflearn-jpa/jpa-insert-structure.png">

* 개발자: JPA에 Entity를 넘김
* JPA
  1. Entity 분석
  2. Insert SQL 생성
  3. JDBC API를 사용하여 SQL를 DB에 날림



### 조회 과정

<img src="https://gmlwjd9405.github.io/images/inflearn-jpa/jpa-select-structure.png">

* 개발자: JPA에 Entity의 pk값을 넘김
* JPA
  1. Entity의 Mapping 정보를 바탕으로 Select SQL 생성
  2. JDBC API를 사용해 SQL를 DB에 날림
  3. DB에서 결과를 받아옴
  4. ResultSet을 객체에 모두 매핑





## 참고

* https://blog.woniper.net/255
* https://gmlwjd9405.github.io/2019/08/04/what-is-jpa.html