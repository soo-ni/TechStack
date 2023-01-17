# Javascript

## 자바스크립트 비동기 런타임 과정

![1.png](https://image.toast.com/aaaabcy/post/16093411084241.png)

* Call stack
  * 자바스크립트에서 수행해야 할 함수들을 순차적으로 스택에 담아 처리
* Web API
  * 웹 브라우저에서 제공하는 API
  * ajax나 timeout 등의 비동기 작업을 실행
* Task Queue
  * callback queue라고도 하며 Web API에서 넘겨받은 callback 함수를 저장
* Event Loop
  * Call stack이 비어있다면 Task Queue의 작업을 Call stack으로 옮김

### 실제 코드 예시

```javascript
setTimeout(() => console.log('async'));
console.log('not timeout');

// not timeout
// async
```

1. setTimeout 함수가 실행되며 Call stack에 setTimeout 함수가 추가
2. setTimeout 함수는 javascript 엔진이 처리하지 않고 Web API가 처리 (NodeJS의 경우 Timers 모듈이 처리) => 요청한 시간이 지나면 Task Queue로 callback 함수를 전달
3. `console.log('not timeout')` 이 call stack에 추가된 후 call stack의 `console.log('not timeout')` 가 실행
4. 자바스크립트의 Event loop는 call stack이 비어있는지 항상 확인, call stack이 비워진 것을 확인한 event loop는 task queue에 있던 callback 함수를 call stack으로 옮겨 작업 수행



### 참고

* https://chanyeong.com/blog/post/44
* https://chanyeong.com/blog/post/33





