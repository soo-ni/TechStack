# TeamCity 적용

## TeamCity 구축
### ⚙ TeamCity 구축 환경
- 개인PC (OS: Windows10)

### 📂Project
- agent, build configurations 를 설정할 수 있는 가장 큰 단위
- build configuration 실행 (run)

### 📦 Agent
- build 실행할 환경 설정
- 3개까지 가능

### 📃 Build Configurations
- 실행할 agent 선택
- SVN 연결
- build 실행할 script 설정: maven, nodejs, script etc...

## SVN 연결
svn 연결이 제대로 안될 경우 `Advanced Options` 클릭 후 `HTTPS connections:` >  `Accept non-trusted SSL certificates` 체크박스 선택 필요
* Type of VCS: Subversion
* SVN Connection Settings
  * URL: 연결할 SVN
  * Username
  * Password
  * HTTPS connections: `Accept non-trusted SSL certificates` 체크박스 선택
 
`Text connection` 으로 svn에 제대로 연결이 되는지 확인

## Maven Project
BE 적용

### #1 maven download 실패

아래와 같은 error log 발생
```shell
Failed to execute goal on project ...(생략)
09:56:19   
```
* 기존에 .m2에 설정해주던 settings.xml이 제대로 설정이 안된 것 같음
* maven setting에 settings.xml 추가해주었는데도 안되는 것 같음 확인 필요

=> default setting 뿐만 아니라 build configuration > maven settings에서도 settings.xml 추가 필요

### #2 지정된 경로 찾을 수 없음

아래와 같은 error log 발생
```shell
java.lang.IllegalStateException: Failed to load ApplicationContext
10:43:02       java.lang.IllegalStateException: Failed to load ApplicationContext
```

test 실행하지 않도록 build configuration 수정<br>
=> `Goals`: clean install<br>
=> `Additional Maven command line parameters`: -Dmaven.test.skip=true

### #3 bhp 파일 생성
- bhp 파일 생성 필요
=> env.ROOT_LIB=C:/TeamCity<br>
==> **환경 변수 설정** > Project > Parameter 추가<br>
==> /target에 생성되는데, 환경변수 추가해서 다른 폴더에 생성하게 할 수도 있음

## Nodejs Project
FE 적용

### #1 docker 설치 필요
- 조건 만족하는 agent 가 없어서 실행불가능<br>
=> Unknown, there are no idle compatible agents which can run this build
- agent 조건<br>
=> docker.server.osType, docker.server.version<br>
=> nodejs를 teamcity 옵션 중 npm:lts 로 실행함<br>
=> 이 때, docker image가 필요한 것 같음<br>
==> docker 설치완료

### #2 npm run build FAILED
- build configuration에서 node:lts -> node:12.22.7 로 버전 변경 필요<br>
=> docker에서 알아서 image pull 실행
- script > `npm i @vue/cli-service` 추가 필요 (npm run build)
- infosafer front build 대략 15m 소요


## 추가 확인 필요
- 한번에 여러개의 build가 돌아간다면 memory limit이 날 것 같은데 어떤 전략을 짜야할지?
- 환경 변수 설정 규칙
