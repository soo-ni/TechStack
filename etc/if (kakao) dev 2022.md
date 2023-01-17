# if (kakao) dev 2022

## MFA, 누구냐 너: 공통 플랫폼 파트의 MFA 도입기

### MFA, why ?

* App이 커짐에 따라 프론트앤드의 중복 개발이 많아짐
  => 동일한
* 업무 결합도 증가
  => 다양한 운영주체 (협의가 많아짐)

### MFA 란

* 독립적으로 작동하는 SPA
* injection하여 조립
* 응집력 / 유지보수와 배포 등 업무의 분리

### MFA 구현 방법

[martinFowler.com](https://martinfowler.com/) 참고

1. Iframe을 통한 통합
   - Container App 에서 Micro App을 ifrmae 방식으로 표출
2. Web-component를 통한 runtime 통합
   - Mricro App을 하나의 custom-element로 만들어 container app에서 runtime 환경에서 injection
3. NGINX 통한 routing
   - NGINX에서 유입된 URL에 따라 표출할 HTML 결정
   - 단순한 routing 정도 수준이지만 각 app이 분리되어 개발, 배포돼있어야 하므로 MFA 방식 중 하나
   - 완전한 조각은 아님
4. NPM 화를 통한 Build time 통합
   - 각 micro app들을 npm화 하여 container app에서 install 하여 이용
   - build time에 조합되기 때문에 각 조직간의 배포를 완전히 분리할 순 없다
5. **Runtime JS, CSS injection**
   - build된 micro pp의 js를 container app에서 runtime에 다운로드하여 injection
   - 과도한 payload 크기가 단점

### MFA 구현 방법

1. Container app => component로 injection하기 위해서 manifest.json 파일을 요청
2. css, js 위치를 담은 manifest.json 파일 응답
3. 요청 후 bundling된 파일을 다운받은 후 container app에 injection
4. constainer app과 micro app 모두 NPM Module 이용
5. NPM Module => appName, renderFunction 필요
6. function 실행 시 micro app object 생성됨
7. render하는 시점에 render function 실행 micro app 실행
8. unmount 시점에는 delete가 이뤄짐 (unmount callback stack push) 
   => cache time 이후 실행됨 (default: 5분)
   => 중복된 static file download 막음
9. 필요한 값을 props로 전달해서 mount하도록
   => micro app이 존재하는지 확인 후 
   => 없다면 다운로드 후 injection
10. 

### MFA 장단점

* 장점
  * 업무 결합도 감소
    * ownership 증가
  * 컴포넌트 재사용성 증가
* 단점
  * 과도한 payload 크기
    * 공통적으로 사용하는 library가 bundling된 js마다 존재하므로 payload 크기 과도
    * monolithic 보다 좋다하지만 과도함
  * 운영 복잡도 증가
    * micro app마다 다른 빌드, 배포 파이프라인을 갖고 있어야하므로 관리 포인트가 늘어남
  * CSS 오염 문제
  * Global window 공유
    * container에 injection 된 이후에 원인 debugging 하기 어려움

=> 얼마 되지 않은 기술로 여전히

## 마이크로 프론트엔드 실무에 쓸만할까?

### MFA 언제 필요한가?

* UX적으로 괜찮은가?
* 배포 주기의 문제
* dependency의 업데이트
  * 신규 기능은 필요한데 시간이 없어 리팩토링을 할 순 없었다

### HOW?

* 쪼개서 해야한다

  * frontend는 티가나면 안된다
  * 부분부분 쪼개서 순차적으로

* 구조 분리

  * NPM + webpack
  * pnpm + turborepo + vite![image-20221211215648659](https://user-images.githubusercontent.com/19357410/206906840-6b29d72e-05a9-4ff4-8dc6-0cb1242c86cb.png)

* 서버 사이드 라우팅

  * **runtime** 시 통합한다

* 컨테이너

  * 웹 컴포넌트 + 워크스페이스 환경 앱![image-20221211215826874](https://user-images.githubusercontent.com/19357410/206906842-7d0224d6-71b3-4bb5-8570-cbc194e3794f.png)

  * 웹 컴포넌트

    * 프레임워크로부터 독립적

    * ```vue
      <work-container routes={devRoutes}>
      	<WorkSpace/>
      </work-container>
      ```

    * => container

### 왜 이제 시작?

1. 속도

   - 개발 속도

     => 사람이 많아진다면 좋다

   - 로딩 속도

     => 불필요한 

     => `관리자`  서비스이기 때문에 적절했다

2. 늙지 않는 코드

## 웹 반응성 개선하기

### 웹 성능이란

* 보통 로딩 속도를 얘기함 (로드 성능)

#### 성능 측정

1. Synthetic Monitoring
2. Real User Monitoring
3. 성능 개선 가이드, 지원

#### 페이지 로드 이후

* 로드 보다는 interaction이 중요해지고 있는 경우도 있음 (지도)

### 웹 반응성

![image-20221211220610914](https://user-images.githubusercontent.com/19357410/206906843-452ea5ea-24cf-4ac5-83ac-d96d1e52a465.png)

### 웹 반응성 지표

1. **TBT (Total Blocking Time)**
   - 모든 blocking time
   - Long Task: 메인 스레드에서 50ms 이상 실행되는 작업
   - Blocking Time: Long Task 중 50ms 를 제외한 메인 스레드 점유 시간
   - ![image-20221211220751416](https://user-images.githubusercontent.com/19357410/206906844-1ca32c68-605e-4657-a5d4-6023c56d698f.png)

2. INP (Interaction to Next Paint)
   - 사용자 입력에 대한 이전 event가 끝내는 시간 + event handler + dom에 반영되어 나타나는 시간
   - ![image-20221211220857174](https://user-images.githubusercontent.com/19357410/206906846-bb1ec01f-7bf5-4d7f-95f9-61495bc3e639.png)

3. CLS (Cumulative Layout Shift)
   - 예기치 않은 레이아웃 이동 점수

### 측정 방법

1. Lighthouse Userflow

   - 기존 lighthouse

   - Navigation

     - 단일 페이지 로드 분석
     - LCP, Speed Index 와 같은 페이지 로드 성능 점수 제공
     - Performance, Accessibility, Best Practice, SEO, PWA 등 과 같은 카테고리

   - Snapshot

     - 사용자 인터렉션 이후 특정 상태의 페이지 분석
     - SPA나 복잡한 폼의 접근성 이슈 확인

   - **Timespan**

     - 임의의 시간동안 사용자 인터렉션 분석
     - 수명이 긴 페이지, SPA에서 성능 개선 포인트 제공

   - ```javascript
     import puppeteer,lighthout;
     ```

2. chrome에서 recorde하고 export (chrome devtools recoder)
   => Puppeteer 등 자동화 도구로 확인 가능

### 웹 반응성 개선 사례

#### TBT 개선 사례

* 챗봇 주문
  * callstack 확인 시 강제 reflow가 발생되었음
  * Render: Javascript > Style > Layout > Paint > Composite
  * 개선되지 않은 element 의 style 을 

#### INP 개선 사례

* WPM (Web Performance Monitoring)

  * chart를 바꿨을 때

    * 긴급 업데이트 (사용자가 직접)

    * 전환 업데이트 (긴급하지 않은)

      => 사용자가 직접 변경한 부분을 우선 적용하고 그 이후에 전환하였음

#### CLS 개선 사례

* 카카오 페이 구매
  * 미리 영역에 height 값을 주었음 (반응형이면.. how..?)

> **요약**
>
> 1. 로드 성능은 여전히 중요하지만 반응성도 중요해진다.
>
> 2. Lighthouse userflow 로 측정
>
> 3. 반응성 지표별 개선 사례
>    - 블로킹 타임 발생시 강제 리플로우 확인
>    - 전체화면 업데이트 발생시 중요한 부분 우선 업데이트 진행
>    - 레이아웃 시프트는 간단한 작업만으로 쉽게 개선 가능


### 참고
* https://if.kakao.com/
