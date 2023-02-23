# Jenkins vs. TeamCity

## Jenkins
* `Plugin issue`: 다양한 plugin 지원하지만 plugin 설치 시 재시작 필요, 방화벽 설정 (정확하지 않음), 추후에 CD까지 생각한다면 아찔할듯
* `Docker container`: 추후에 docker 사용 시 간헐적으로 freezing 현상이 발생하고 Jenkins에 Docker 적용 이슈가 있다고함

## TeamCity
* `편리한 인터페이스`: 사용하기 편리한 UI
* `적은 Library`: Jenkins보다는 적은 라이브러리
* `자동화 파이프 라인 구성`
* [build runners](https://www.jetbrains.com/help/teamcity/supported-platforms-and-environments.html#Build+Runners)
