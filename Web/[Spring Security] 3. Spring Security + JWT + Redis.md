# Spring Security + JWT + Redis

Redis를 이용해 사용자를 관리해보자 :thinking:



## Redis Cache

spring-data-redis lib 이용해 Redis를 캐시 storage로 사용



## 실행

### build.gradle에 lib 추가

``` 
implementation 'org.springframework.boot:spring-boot-starter-data-redis'
implementation 'it.ozimov:embedded-redis:0.7.2'
```



### Embedded Redis 설정 추가

```java
@Profile("local")
@Configuration
public class EmbeddedRedisConfig {

    @Value("${spring.redis.port}")
    private int redisPort;

    private RedisServer redisServer;

    @PostConstruct
    public void redisServer() {
        redisServer = new RedisServer(redisPort);
        redisServer.start();
    }

    @PreDestroy
    public void stopRedis() {
        if (redisServer != null) {
            redisServer.stop();
        }
    }
}
```



### application-local.yml에 redis 설정 추가

```
spring:
  profiles: local
  .... 생략
  redis:
    host: localhost
    port: 6379
```



### application-alpha.yml에 서버 구축한 redis 정보 설정

```
spring:
  profiles: alpha
  .... 생략
  redis:
    host: 서버에 설치한 Redis의 호스트 정보
    port: 서버에 설치한 Redis의 port 정보
```



### RedisConfig.java

> @EnableCaching: Redis 캐시 사용 활성화
>
> **캐시 key별로 유효시간 설정**, 캐시 key에 유효시간을 설정하지 않은 경우에도 default 유효시간으로 캐시가 생성되도록 세팅 (default가 없다면 캐시가 지워지지 않으므로 메모리 부족)

```java
@RequiredArgsConstructor
@EnableCaching
@Configuration
public class RedisConfig {

    @Bean(name = "cacheManager")
    public RedisCacheManager cacheManager(RedisConnectionFactory connectionFactory) {

        RedisCacheConfiguration configuration = RedisCacheConfiguration.defaultCacheConfig()
                .disableCachingNullValues() // null value 캐시안함
                .entryTtl(Duration.ofSeconds(CacheKey.DEFAULT_EXPIRE_SEC)) // 캐시의 기본 유효시간 설정
                .computePrefixWith(CacheKeyPrefix.simple())
                .serializeKeysWith(RedisSerializationContext.SerializationPair.fromSerializer(new StringRedisSerializer())); // redis 캐시 데이터 저장방식을 StringSeriallizer로 지정

// 캐시키별 default 유효시간 설정
        Map<String, RedisCacheConfiguration> cacheConfigurations = new HashMap<>();
        cacheConfigurations.put(CacheKey.USER, RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(CacheKey.USER_EXPIRE_SEC)));
        cacheConfigurations.put(CacheKey.BOARD, RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(CacheKey.BOARD_EXPIRE_SEC)));
        cacheConfigurations.put(CacheKey.POST, RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(CacheKey.POST_EXPIRE_SEC)));
        cacheConfigurations.put(CacheKey.POSTS, RedisCacheConfiguration.defaultCacheConfig()
                .entryTtl(Duration.ofSeconds(CacheKey.POST_EXPIRE_SEC)));

        return RedisCacheManager.RedisCacheManagerBuilder.fromConnectionFactory(connectionFactory).cacheDefaults(configuration)
                .withInitialCacheConfigurations(cacheConfigurations).build();
    }
}
```



### CacheKey.java

> 캐시 세팅시 사용할  key와 expire 등 정적 정보 저장

```java
public class CacheKey {
    public static final int DEFAULT_EXPIRE_SEC = 60; // 1 minutes
    public static final String USER = "user";
    public static final int USER_EXPIRE_SEC = 60 * 5; // 5 minutes
    public static final String BOARD = "board";
    public static final int BOARD_EXPIRE_SEC = 60 * 10; // 10 minutes
    public static final String POST = "post";
    public static final String POSTS = "posts";
    public static final int POST_EXPIRE_SEC = 60 * 5; // 5 minutes
}
```



### 캐싱 객체에 대한 Serializable / @Proxy(lazy=false)

> Redis에 객체를 저장: 내부적으로 직렬화되어 저장
>
> 모델 class에 Serializable 선언 필요
>
> JPA를 사용한다면 entity 객체 내에서 연관관계 매핑이 있을 수 있어 Lazy(지연) 로딩으로 처리되는 경우 Redis 캐싱시 오류 발생할 수 있어 해당 entity에 @Proxy(lazy=false) 선언 필요

```java
@Entity
@Getter
@NoArgsConstructor
public class Board extends CommonDateEntity implements Serializable {
    ................ 생략
}

@Entity
@Getter
@NoArgsConstructor
public class Post extends CommonDateEntity implements Serializable {
    ................ 생략
}

// User클래스는 다른 class에서 연관관계 매핑을 통해 Lazy로딩되므로 캐싱시 문제가 발생하지 않도록 proxy false 설정을 한다.
@Proxy(lazy = false)
public class User extends CommonDateEntity implements UserDetails {
    ................ 생략
}
```



### 빈번히 호출되는 Method Cache 처리

> CustomUserDetailService의 경우 회원정보를 빈번히 호출해 캐싱 적용 필요
>
> * @Cacheable
>   * 해당 메소드가 호출될 때
>     * 캐시X: DB접근해 캐시 생성 및 데이터 반환
>     * 캐시O: 바로 캐시 데이터 반환
>   * Option
>     * value: 저장 시 key 값
>     * key: key 생성 시 추가로 덧붙일 파라미터 정보 (ex. user::1004)
>     * unless = "#result==null": 메소드 결과가 null이 아닌 경우에만 캐싱

```java
@RequiredArgsConstructor
@Service
public class CustomUserDetailService implements UserDetailsService {

    private final UserJpaRepo userJpaRepo;

    @Cacheable(value = CacheKey.USER, key = "#userPk", unless = "#result == null")
    public UserDetails loadUserByUsername(String userPk) {
        return userJpaRepo.findById(Long.valueOf(userPk)).orElseThrow(CUserNotFoundException::new);
    }
}
```



### CRUD 메소드에 캐싱 처리

> 게시판 게시물 CRUD에 따라 캐싱 처리
>
> 캐싱을 위한 annotation 
>
> * @Cacheable 
>   * 캐시O: 메소드 실행하지 않고 캐시된 값 반환
>   * 캐시X: 메소드 실행 후 반환된 값 캐시에 저장
> * @CachePut
>   * 캐시에 데이터 넣거나 수정
>   * 캐시O: 갱신
>   * 캐시X: 저장
> * @CacheEvict
>   * 캐시 삭제
> * @Caching
>   * 여러개 캐시 annotation 실행할 때 사용



#### 단일 데이터 캐시 처리

```java
// 게시판 정보 조회
@Cacheable(value = CacheKey.BOARD, key = "#boardName", unless = "#result == null")
public Board findBoard(String boardName) {
        return Optional.ofNullable(boardJpaRepo.findByName(boardName)).orElseThrow(CResourceNotExistException::new);
}
```

#### 다중 데이터 캐시 처리

```java
// 게시글 리스트 조회.
@Cacheable(value = CacheKey.POSTS, key = "#boardName", unless = "#result == null")
public List<Post> findPosts(String boardName) {
    return postJpaRepo.findByBoard(findBoard(boardName));
}
```

#### 데이터 등록 캐시 처리

> 데이터 등록은 캐시 처리가 대부분 필요하지 않음
>
> 아래의 경우는 게시글 리스트 캐시를 초기화 해야하므로 CacheEvict를 사용해 캐시 삭제
>
> ex. "posts::freeboard" 형식의 캐시키 삭제됨

```java
// 게시글 등록후 캐시 삭제
@CacheEvict(value = CacheKey.POSTS, key = "#boardName")
public Post writePost(String uid, String boardName, ParamsPost paramsPost) {
        Board board = findBoard(boardName);
        Post post = new Post(userJpaRepo.findByUid(uid).orElseThrow(CUserNotFoundException::new), board, paramsPost.getAuthor(), paramsPost.getTitle(), paramsPost.getContent());
        return postJpaRepo.save(post);
}
```

#### 데이터 수정 캐시 처리

> 1. 기존 캐시 갱신
>    - 수정된 데이터의 캐시만 갱신하면 될 경우
>    - @CachePut
> 2. 캐시 삭제
>    - 연관된 캐시가 많아 갱신이 힘든 경우
>    - @Caching 사용

```java
// 1. 기존 캐시 갱신
@CachePut(value = CacheKey.POST, key = "#postId")
public Post updatePost(long postId, String uid, ParamsPost paramsPost) {
        Post post = getPost(postId);
        User user = post.getUser();
        if (!uid.equals(user.getUid()))
            throw new CNotOwnerException();
        post.setUpdate(paramsPost.getAuthor(), paramsPost.getTitle(), paramsPost.getContent());
        return post;
}
```

```java
// 2. 캐시 삭제
// 게시글 수정
public Post updatePost(long postId, String uid, ParamsPost paramsPost) {
        Post post = getPost(postId);
        User user = post.getUser();
        if (!uid.equals(user.getUid()))
            throw new CNotOwnerException();
        post.setUpdate(paramsPost.getAuthor(), paramsPost.getTitle(), paramsPost.getContent());
cacheSevice.deleteBoardCache(post.getPostId(), post.getBoard().getName());
        return post;
}
```

#### 데이터 삭제 캐시 처리

```java
// 게시글 삭제
public boolean deletePost(long postId, String uid) {
    Post post = getPost(postId);
    User user = post.getUser();
    if (!uid.equals(user.getUid()))
        throw new CNotOwnerException();
    postJpaRepo.delete(post);
    cacheSevice.deleteBoardCache(post.getPostId(), post.getBoard().getName());
    return true;
   }
}
```



### CacheService.java

```java
@Slf4j
@Service
public class CacheSevice {

    @Caching(evict = {
            @CacheEvict(value = CacheKey.POST, key = "#postId"),
            @CacheEvict(value = CacheKey.POSTS, key = "#boardName")
    })
    public boolean deleteBoardCache(long postId, String boardName) {
        log.debug("deleteBoardCache - postId {}, boardName {}", postId, boardName);
        return true;
    }
}
```



### Key 생성

> 1. 객체 변수값으로 캐시 key 생성
>    - post::1000
> 2. 여러개 값으로 캐시 key 생성
>    - post::1000::happy
> 3. 동일 key 형태 캐시 삭제
>    - User::1, User::2, ... 삭제

```java
// 1. 객체 변수값으로 캐시 key 생성
@CachePut(value = CACHE_KEY, key = "#post.postId")
public Post updatePost(Post post) {
     ..... 생략
}
```

```java
// 2. 여러개 값으로 캐시 key 생성
@Cacheable(value = CACHE_KEY, key = "{#postId, #title}")
public Post getPostMultiKey(long postId, String title) {
        ...... 생략
 }

@CachePut(value = CACHE_KEY, key = "{#post.postId, #post.title}")
public Post updatePostMultiKey(Post post) {
        ....... 생략
}

// 별도의 연산된 결과를 key값으로 사용하고 싶은경우 메서드를 사용가능
@CachePut(value = CACHE_KEY, key = "{#post.postId, #post.getTitle()}")
public Post updatePostMultiKey(Post post) {
        ....... 생략
}
```

```java
// 3. 동일 key 형태 캐시 삭제
@CacheEvict(cacheNames = {"User"}, allEntries = true)
public void clearCache(){}
```







### 참조

* https://daddyprogrammer.org/post/3870/spring-rest-api-redis-caching/