# CI/CD Tool

## CI/CD 란?
* CI (Continuous Integration): `지속적인 통합`으로 빌드와 테스트 자동화
  * Build -> Test -> Merge
* CD (Continuous Delivery): `지속적인 배포`로 배포 자동화
  * Deploy

## Why?
CI/CD는 Agile 문화와 DevOps의 한 부분으로 `속도`와 `효율`을 위해 출현했다.

## Tools
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

### 현재 우리에게 필요한 Tool
1. 무료
2. SVN 연동
3. 복잡한 script를 사용할 수도 있음
4. CD 까지..?

==> Jenkins 혹은 TeamCity 사용



### 참고
* [CI/CD 기본개념과 가장 많이 쓰이는 도구 5가지](https://www.hanl.tech/blog/ci-cd-%EA%B8%B0%EB%B3%B8%EA%B0%9C%EB%85%90%EA%B3%BC-%EA%B0%80%EC%9E%A5-%EB%A7%8E%EC%9D%B4-%EC%93%B0%EC%9D%B4%EB%8A%94-%EB%8F%84%EA%B5%AC-5%EA%B0%80%EC%A7%80/)
* [jenkins bamboo](https://www.lesstif.com/software-architect/jenkins-bamboo-15269892.html)
* [teamcity](https://www.jetbrains.com/ko-kr/teamcity/integrations/version-control/)
