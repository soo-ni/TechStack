# Redis

## Redis란

> Remote Dictionary Server의 약자로, 오픈소스 소프트웨어
>
> 휘발성이면서 영속성을 가진 **key-value** 저장소

### 휘발성

* **Cache**
* 메모리에 데이터를 read/write해 빠른 속도
* 데이터가 메모리 안에 있어 캐시 관점에서 유용
* Cache 방식을 통한 DB 부하 감소

### 영속성

* 지속성을 보장하기 위해 데이터를 disk에 저장
* snapshotting (RDB)
  * 순간적으로 메모리에 있는 내용을 disk에 전체를 옮겨 담음
* AOF (Append On File)
  * redis의 모든 write/update 연산 자체를 모두 log 파일에 기록



## Spring Boot 적용

### Dependency

```
compile("org.springframework.boot:spring-boot-starter-data-redis")
```

* spring-data-redis: 스프링에서 공식 지원하는 dependency로 Redis Client와 연동 가능한 높은 레벨의 **RedisTemplate** 추상화를 제공
* jeids: Java Client로 가장 활발한 오픈소스로, redis에서 공식 추천

### Auto Configuration

```
spring.redis.host = 
```

* JedisConnectionFactory : Redis 연동
* RedisTemplate : Redis Command 를 도와주는 Template





### 참조

* https://kingbbode.tistory.com/25