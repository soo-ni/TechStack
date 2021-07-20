# Spring boot + MySQL + Docker + AWS

docker-compose를 사용해보자 :thinking:



## docker-compose란?

Docker compose yaml 파일로 **여러 개**의 도커컨테이너의 정의를 작성하여 한 번에 많은 컨테이너들을 작동시키고 관리할 수 있는 툴



> **TIP**
>
> Microservice architecture 란?
>
> 서버들을 공통성이 있는 기능을 기반으로 모듈화하고 서로의 디펜던시를 최소화시켜 decouple된 (분리된) 서버 구조
>
> 이런 아키텍처를 추구할 경우 서비스 하나하나가 굉장히 라이트 웨이트하고 stateless 구조라고 볼 수 있기 때문에 서버 로드를 효율적으로 분산시키고 테스트를 분리하기에 굉장히 유용
>
> [여기]([https://medium.com/withj-kr/docker-%EB%B6%80%ED%84%B0-kubernetes-%EA%B9%8C%EC%A7%80-5-docker-compose-%EC%9E%85%EB%AC%B8-ece1a6721775](https://medium.com/withj-kr/docker-부터-kubernetes-까지-5-docker-compose-입문-ece1a6721775))에 더 자세히 나와있음!



## docker-compose 적용

### docker-compose.yml

Dockerfile과 같은 위치에 있도록 설정

```
version: '3.7'

services:
    mysql:
        # 컨테이너 이름을 주고 싶다면 작성한다
        container_name: mysql
        image: mysql:latest
        restart: always
        ports:
          - 3306:3306        
        environment:
          MYSQL_ROOT_PASSWORD: root
          MYSQL_ROOT_HOST: '%'
          MYSQL_USER: ssafy
          MYSQL_PASSWORD: ssafy
          MYSQL_DATABASE: dbtest

    springdockerdemo:
        container_name: springdockerdemo
        build:
          context: .
          dockerfile: Dockerfile
        environment:
          MYSQL_ADDRESS: mysql
          MYSQL_USERNAME: ssafy
          MYSQL_PASSWORD: ssafy
        ports:
          - 8080:8080
        restart: always
        links:
          - mysql
        depends_on:
          - mysql
```



### application.yml

> 바뀔 수 있는 mysql_address, mysql_username, mysql_password는 환경변수로 처리

```
spring:
  datasource:
    driver-class-name: com.mysql.cj.jdbc.Driver
    url : jdbc:mysql://${mysql_address}:3306/dbtest?serverTimezone=Asia/Seoul&createDatabaseIfNotExist=true&useUnicode=true&characterEncoding=UTF-8&characterSetResults=UTF-8&useSSL=true
    username : ${mysql_username}
    password : ${mysql_password}
    
  jpa:
    database-platform: org.hibernate.dialect.MySQL5InnoDBDialect
    hibernate:
      ddl-auto: update
    generate-ddl: true
    show-sql: true
      
logging:
  level:
    root: info
```



### Spring boot war 파일 만들기

> java -jar [filename].war이 잘 실행돼야한다

```
$ set mysql_address=localhost
$ set mysql_username=root
$ set mysql_password=root

$ gradlew -DskipTests=true build

$ cd builds/libs
$ java -jar [filename].war

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.3.2.RELEASE)
 // 생략...
```



### EC2 보안정책 설정

![security_mysql](https://user-images.githubusercontent.com/19357410/89738298-644ca580-dab2-11ea-867f-c6db68160d1d.PNG)



### FileZilla - EC2

FileZilla로 서버 연결은 [여기](https://ithub.tistory.com/48) 참고하기

* [filename].war
* Dockerfile
* docker-compose.yml



### EC2에 docker-compose 설치

[여기](https://megazonedsg.github.io/1-Make-Docker/) 참고

```
$ sudo curl -L https://github.com/docker/compose/releases/download/1.25.0\
-rc2/docker-compose-`uname -s`-`uname -m` -o \
/usr/local/bin/docker-compose

$ sudo chmod +x /usr/local/bin/docker-compose

$ docker-compose -v
docker-compose version 1.25.0-rc2, build 661ac20e
```



### docker-compose 실행

1. mysql의 최신버전이 pull
2. spring boot의 image가 생성

```
$ docker-compose up
Pulling mysql (mysql:)...
latest: Pulling from library/mysql
bf5952930446: Pull complete
8254623a9871: Pull complete
938e3e06dac4: Pull complete
ea28ebf28884: Pull complete
f3cef38785c2: Pull complete
894f9792565a: Pull complete
1d8a57523420: Pull complete
6c676912929f: Pull complete
ff39fdb566b4: Pull complete
fff872988aba: Pull complete
4d34e365ae68: Pull complete
7886ee20621e: Pull complete
Digest: sha256:c358e72e100ab493a0304bda35e6f239db2ec8c9bb836d8a427ac34307d074ed
Status: Downloaded newer image for mysql:latest
Building springdockerdemos
Step 1/6 : FROM openjdk:8-jre
 ---> 2e2653debbe9
Step 2/6 : VOLUME /tmp
 ---> Running in baa94b668f80
Removing intermediate container baa94b668f80
 ---> 651d663f7fd0
Step 3/6 : EXPOSE 8080
 ---> Running in 7ab5fadc9932
Removing intermediate container 7ab5fadc9932
 ---> ce3d9525cd7b
Step 4/6 : ARG JAR_FILE=./demo-0.0.1-SNAPSHOT.war
 ---> Running in 8934ba749a3b
Removing intermediate container 8934ba749a3b
 ---> 65a7b8b8bdf4
Step 5/6 : ADD ${JAR_FILE} demo-0.0.1-SNAPSHOT.war
 ---> 898a17b24e41
Step 6/6 : ENTRYPOINT ["java", "-jar", "/demo-0.0.1-SNAPSHOT.war"]
 ---> Running in ffdfcb4af5c7
Removing intermediate container ffdfcb4af5c7
 ---> 8f90a97859b1

Successfully built 8f90a97859b1
Successfully tagged apps_springdockerdemos:latest
WARNING: Image for service springdockerdemos was built because it did not already exist. To rebuild this image you must use `docker-compose build` or `docker-compose up --build`.
Creating mysql ... done
Creating springdockerdemos ... done
Attaching to mysql, springdockerdemos
mysql                | 2020-08-09 17:29:00+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.21-1debian10 started.
mysql                | 2020-08-09 17:29:01+00:00 [Note] [Entrypoint]: Switching to dedicated user 'mysql'
mysql                | 2020-08-09 17:29:01+00:00 [Note] [Entrypoint]: Entrypoint script for MySQL Server 8.0.21-1debian10 started.
mysql                | 2020-08-09 17:29:01+00:00 [Note] [Entrypoint]: Initializing database files
mysql                | 2020-08-09T17:29:01.335877Z 0 [Warning] [MY-010139] [Server] Changed limits: max_open_files: 1024 (requested 8161)
mysql                | 2020-08-09T17:29:01.335884Z 0 [Warning] [MY-010142] [Server] Changed limits: table_open_cache: 431 (requested 4000)
mysql                | 2020-08-09T17:29:01.336256Z 0 [System] [MY-013169] [Server] /usr/sbin/mysqld (mysqld 8.0.21) initializing of server in progress as process 42
mysql                | 2020-08-09T17:29:01.351219Z 1 [System] [MY-013576] [InnoDB] InnoDB initialization has started.
mysql                | 2020-08-09T17:29:02.264132Z 1 [System] [MY-013577] [InnoDB] InnoDB initialization has ended.
mysql                | 2020-08-09T17:29:04.755499Z 6 [Warning] [MY-010453] [Server] root@localhost is created with an empty password ! Please consider switching off the --initialize-insecure option.
springdockerdemos    |
springdockerdemos    |   .   ____          _            __ _ _
springdockerdemos    |  /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
springdockerdemos    | ( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
springdockerdemos    |  \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
springdockerdemos    |   '  |____| .__|_| |_|_| |_\__, | / / / /
springdockerdemos    |  =========|_|==============|___/=/_/_/_/
springdockerdemos    |  :: Spring Boot ::        (v2.3.2.RELEASE)

// 생략...
```



### Postman으로 확인

#### 1. User 등록

![createUser](https://user-images.githubusercontent.com/19357410/89738623-18e7c680-dab5-11ea-9681-1046ac445397.png)

#### 2. User 전체 조회

![findAllUser](https://user-images.githubusercontent.com/19357410/89738628-1b4a2080-dab5-11ea-9fa2-be015d86b7fe.png)

#### 3. User 상세 조회

![findByUid](https://user-images.githubusercontent.com/19357410/89738631-1d13e400-dab5-11ea-8490-eb8c48e2bd60.png)





---

### 실행이 안될 때 :sob:

#### 1. 모든 컨테이너 확인 후 삭제

```
$ docker ps -a
$ docker rm [container id]
```

#### 2. 모든 이미지 확인 후 삭제

```
$ docker images
$ docker rmi [image id]
$ docker rmi -f [image id]	//안된다면 -f 추가
```

#### 3. 다시 처음부터 하기 ㅎ..ㅎ

