# [CI/CD] 1. CI/CD Tool

## CI/CD 란?
* CI (Continuous Integration): `지속적인 통합`으로 빌드와 테스트 자동화
  * Build -> Test -> Merge
* CD (Continuous Delivery): `지속적인 배포`로 배포 자동화
  * Deploy

## CI/CD 가 필요한 이유?
CI/CD는 Agile 문화와 DevOps의 한 부분으로 `속도`와 `효율`을 위해 출현

## 대표적인 Tools
Jenkins, Buddy, TeamCity, Bamboo, Gitlab CI, Circle CI 등 여러 tool이 존재

### 1. Jenkins
- 가장 많이 쓰이고 관련 문서가 많음
- 무료
- 직접 호스팅 해야함 > 관리 리소스가 많이 들어감
- java 기반
- **svn**, git
- https://www.jenkins.io/

### 2. Buddy
- 유료
- 직관적인 UI
- git, bitbucket
- https://buddy.works/

### 3. TeamCity
- 무료
- JetBrains에서 개발 IDE와 통합되어 편리
- Docker, k8 기반 프로젝트와 통합이 편리
- java 기반
- **svn**, git
- https://www.jetbrains.com/teamcity/

### 4. Bamboo
- 유료
- 직관적인 UI, atlassian 제품과 통합 가능 (ex. JIRA)
- 프로젝트 권한 설정이나 분산 빌드 간편
- 기존 Jenkins migration 가능
- **svn**, git
- https://www.atlassian.com/software/bamboo

### 5. Gitlab CI
- gitlab에서 제공하는 CI tool
- 권한설정이 간편
- git
- https://about.gitlab.com/

### 6. CircleCI
- 유료
- github에서 제공
- 비동기 CI/CD 가능
- git, bitbucket
- https://circleci.com/

## 현재 우리에게 필요한 Tool
아래와 같은 기능이 필요했고, 여러 CI/CD tool 중 Jenkins와 TeamCity가 가장 적합
1. 무료
2. SVN 연동
3. 복잡한 script 사용까지 가능
4. 현재 CI까지 진행이지만 CD까지 가능

### Jenkins
* `Plugin issue`: 다양한 plugin 지원하지만 plugin 설치 시 재시작 필요, 방화벽 설정 (정확하지 않음), 추후에 CD까지 생각한다면 아찔할듯
* `Docker container`: 추후에 docker 사용 시 간헐적으로 freezing 현상이 발생하고 Jenkins에 Docker 적용 이슈가 있다고함

### TeamCity
* `편리한 인터페이스`: 사용하기 편리한 UI
* `적은 Library`: Jenkins보다는 적은 라이브러리
* `자동화 파이프 라인 구성`
* [build runners](https://www.jetbrains.com/help/teamcity/supported-platforms-and-environments.html#Build+Runners)

TeamCity는 Jenkins보다는 상대적으로 적은 library를 보유하고 있지만 build에 필요한 library는 다 보유하고 있으며, docker 사용에도 무리가 없고, 편리한 인터페이스를 가지고 있어 **TeamCity**를 선택

