# Mock Service Worker

## MSW란?
MSW는 서비스 워커를 사용해 HTTP 요청을 가로채 모의 응답을 보내는 API 도구이다. 백엔드 개발이 안됐을 때 유용하게 사용 가능하다.

### 서비스 워커란?
브라우저가 백그라운드에서 실행하는 스크립트를 의미한다.

1. 웹사이트와 별개로 동작, 웹페이지가 닫히더라도 비활성화되지 않는다.
2. 웹사이트 요청을 가로챌 수 있다.
3. 웹사이트와 네트워크 중간다리 역할을 할 수 있다. (proxy 서버 역할)
4. 리소스를 캐싱할 수 있다.
   
## 사용하기
### #1 msw 설치
1. msw 설치

   `npm install msw` / `npm install --save-D msv`

2. public/ folder에 service worker script 생성 (mockServiceWorker.js)

   `npx msw init public/`

### #2 폴더 추가
`src/mocks` 폴더 추가

### #3 API 동작을 정의

handlers.ts 파일에 API 동작을 정의할 수 있다 (rest 사용). 이 때, path는 full path를 작성해야한다.

```typescript
import { rest } from 'msw';

export default [
  rest.get(
    'https://192.168.107.252:8087/infosafer_manager/api/v1/system-settings/backup',
    (req, res, ctx) => {
      return res(
        ctx.json([
          { name: 'backup.auto.scheduling_time', val: '03:00' },
          { name: 'backup.expired_period', val: '12' },
          { name: 'backup.manual.data_type', val: 'ALL' },
          { name: 'backup.manual.end_date', val: '2022-11-03' },
          { name: 'backup.manual.scheduling_time', val: '2022-11-17 11:53:00' },
          { name: 'backup.manual.start_date', val: '2022-11-03' },
          { name: 'backup.manual.type', val: '0' },
          { name: 'backup.max_disk_space', val: '90' },
          { name: 'backup.path', val: '/home/test,/home/pnpweb/data/backup' },
          { name: 'backup.type', val: '0' },
        ])
      );
    }
  ),
];

```

### #4 worker 생성
browser.ts 파일에 handlers.ts 에 선언한 handler를 worker에 setting 해준다.
```typescript
import { setupWorker } from 'msw';
import handlers from '@/mocks/handlers';

export const worker = setupWorker(...handlers);
```


### #5 worker 실행

main.ts 에서 worker를 실행시킨다.
```typescript
if (process.env.NODE_ENV === 'development') {
  worker.start();
}
```



### 참고
* [Mock Service Worker](https://mswjs.io/)
* [Service Worker](https://developer.mozilla.org/en-US/docs/Web/API/Service_Worker_API)
* [Vue.js test using msw](https://www.vuemastery.com/blog/mock-service-worker-api-mocking-for-vuejs-development-testing/)
