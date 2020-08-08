# Spring boot + Docker + AWS

Spring boot + Docker + AWS 배포 :thinking:

## Spring boot

gradle 설치

https://ict-nroo.tistory.com/29

https://gunju-ko.github.io/spring/docker/2018/12/22/SpringAndDocker.html



### .war file

```
$ gradle build
```

```
// war file 실행
multicampus@DESKTOP-KVCQHCD MINGW64 /f/SSAFY/Docker/demo/build/libs
$ java -jar demo-0.0.1-SNAPSHOT.war
  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.3.2.RELEASE)

// 생략..
```



### Dockerfile

```
FROM openjdk:8-jre
ADD ./build/libs/demo-0.0.1-SNAPSHOT.war demo-0.0.1-SNAPSHOT.war
ENTRYPOINT ["java", "-jar", "demo-0.0.1-SNAPSHOT.war"]
```



### Docker image 배포

```
$ docker build -t springdockerdemo .
Sending build context to Docker daemon  55.57MB
Step 1/3 : FROM openjdk:8-jre
 ---> 2e2653debbe9
Step 2/3 : ADD ./build/libs/demo-0.0.1-SNAPSHOT.war demo-0.0.1-SNAPSHOT.war
 ---> Using cache
 ---> d6243077f6af
Step 3/3 : ENTRYPOINT ["java", "-jar", "demo-0.0.1-SNAPSHOT.war"]
 ---> Running in c714e1966221
Removing intermediate container c714e1966221
 ---> 7f0a9415c9b9
Successfully built 7f0a9415c9b9
Successfully tagged springdockerdemo:latest
SECURITY WARNING: You are building a Docker image from Windows against a non-Windows Docker host. All files and directories added to build context will have '-rwxr-xr-x' permissions. It is recommended to double check and reset permissions for sensitive files and directories.
```

```
$ docker images
REPOSITORY          TAG                 IMAGE ID            CREATED             SIZE
springdockerdemo    latest              1117d25f7439        24 minutes ago      285MB
<none>              <none>              0d66a95a8c6f        24 minutes ago      285MB
<none>              <none>              d6ef00d1cb39        25 minutes ago      285MB
openjdk             8-jre               2e2653debbe9        3 days ago          265MB
<none>              <none>              3ae62b78cf59        3 weeks ago         105MB
<none>              <none>              6ea20499e1d6        3 weeks ago         643MB
mysql               latest              6e447ce4863d        3 weeks ago         544MB
docker101tutorial   latest              dec8df1decfb        3 weeks ago         26.8MB
<none>              <none>              aa15c41622be        3 weeks ago         122MB
<none>              <none>              2bf76d756e12        3 weeks ago         108MB
<none>              <none>              a00ebeaa9443        3 weeks ago         224MB
nginx               alpine              ecd67fe340f9        4 weeks ago         21.6MB
tomcat              8.5                 34d28186c789        4 weeks ago         529MB
node                12-alpine           057fa4cc38c2        5 weeks ago         89.3MB
python              alpine              8ecf5a48c789        2 months ago        78.9MB
openjdk             8-jdk-alpine        a3562aa0b991        15 months ago       105MB
java                8                   d23bdf5b1b1b        3 years ago         643MB
```

```
$ docker run -p 80:8888 springdockerdemo

  .   ____          _            __ _ _
 /\\ / ___'_ __ _ _(_)_ __  __ _ \ \ \ \
( ( )\___ | '_ | '_| | '_ \/ _` | \ \ \ \
 \\/  ___)| |_)| | | | | || (_| |  ) ) ) )
  '  |____| .__|_| |_|_| |_\__, | / / / /
 =========|_|==============|___/=/_/_/_/
 :: Spring Boot ::        (v2.3.2.RELEASE)
 
 // 생략...
```

```
$ docker login
Username:
Password:
Login Succeeded
```

```
$ docker push springdockerdemo
The push refers to repository [docker.io/library/springdockerdemo]
ba7f09dad016: Preparing
ef5928eda1bf: Preparing
09fd8e7eb053: Preparing
1c91e89645b0: Preparing
7a9460d53218: Preparing
b2765ac0333a: Waiting
0ced13fcf944: Waiting
denied: requested access to the resource is denied
```

> **TIP**
>
> login한 username이 image 이름에 없으면 push되지 않음

```
$ docker image tag springdockerdemo ksb940925/springdockerdemo
```

```
$ docker push ksb940925/springdockerdemo
The push refers to repository [docker.io/ksb940925/springdockerdemo]
ba7f09dad016: Pushed
ef5928eda1bf: Pushed
09fd8e7eb053: Pushed
1c91e89645b0: Pushed
7a9460d53218: Pushed
b2765ac0333a: Pushed
0ced13fcf944: Pushed
latest: digest: sha256:91367569ed01bf315226387cfbe46a74dfaf031dc8c6bfbafe95634b7b44b747 size: 1793
```

![dockerimages](https://user-images.githubusercontent.com/19357410/89716702-32700c00-d9ea-11ea-8053-b87c632bb951.PNG)



### AWS EC2 - Docker

https://victorydntmd.tistory.com/62

https://gunju-ko.github.io/spring/docker/2018/12/22/SpringAndDocker.html

```
$ sudo yum update -y
```

```
$ sudo yum install -y docker
```

```
$ sudo service docker start
```

```
$ sudo usermod -a -G docker ec2-user
```

```
$ docker info 
// 로 확인하기
```



### Get Docker image

```
$ docker login
```

```
$ docker pull ksb940925/springdockerdemo
```

```
$ docker images
```

```
$ docker run -d -p 8080:8080 ksb940925/springdockerdemo
```



### AWS EC2 server로 확인

http://{AWS address}:8080/

![awsDocker](https://user-images.githubusercontent.com/19357410/89716703-356afc80-d9ea-11ea-992f-1bc95f3268d9.png)