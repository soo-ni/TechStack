# JPA

## Persistence context

@Entity: DB에 보관되는 대상

JPA는 엔티티를 영속 컨텍스트로 관리한다.

영속 컨텍스트에 보관된 객체를 영속 객체라 부른다.

1. EntityManager 생성
2. 트랜잭션 시작
3. EntityManager 통해 영속 컨텍스트에 객체를 추가하거나 구함
4. 트랜잭션 커밋
5. EntityManager 닫음