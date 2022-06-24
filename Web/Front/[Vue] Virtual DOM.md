# Virtual DOM

## #1. DOM

###  DOM 이란?

**D**ocument **O**bject **M**odel, 웹문서를 **객체화** (`console.log(document)` 로 확인 가능)

### 객체화가 필요한 이유

* 인터페이스를 위해서

* script 언어로 document를 조작하기 위해서

  ```javascript
  const div = document.querySelect('#div');
  div.innerHeight = 10;
  ```

* 참고: 최상위 객체는 window

### 객체 모델 생성

DOM 객체 생성 과정

![image-20220623094244711](C:\Users\sbkim\AppData\Roaming\Typora\typora-user-images\image-20220623094244711.png)

## #2. 렌더링

### 렌더링이란?

브라우저가 서버로 부터 받은 HTML, CSS, Javascript 코드를 그래픽으로 그리는 작업

오픈소스 렌더링 엔진: [webkit](https://webkit.org/) (크롬, 사파리)

### 렌더링 과정

![image-20220623094830696](C:\Users\sbkim\AppData\Roaming\Typora\typora-user-images\image-20220623094830696.png)

webkit 기준의 렌더링

1. 서버 요청 (HTML, 정적 파일)

2. 렌더링 엔진이 HTML, CSS 파싱

3. DOM Tree (HTML), CSSOM Tree (CSS) 생성

4. DOM Tree, CSSOM Tree 두 가지를 결합하여 렌더링 트리 생성

   > **렌더링 트리**?
   >
   > 화면에 표시되는 모든 노드의 컨텐츠 및 스타일 정보들을 포함하는 구조
   >
   > 
   >
   > 스크립트 태그나 메타 태그 같은 요소는 렌더링에 반영되지 않고, css의 display: none; 속성도 렌더링 트리에 포함되지 않는다.
   >
   > visibility: hidden;은 **보이지는 않지만 공간을 차지하기 때문에 렌더링 트리에 포함**

5. 렌더링 트리 배치 (위치, 크기 계산 필요)

   1. Layout
   2. Paint

### 💥 BUT!! It’s expensive to update the DOM

* 사용자가 액션이나 이벤트 발생 => Layout, Paint 작업이 반복

* 데이터 변경, 요소 추가 => DOM Tree부터 생성

==> 브라우저 연산량이 늘어남! 사용자는 늦게 웹페이지를 봄!

## #3. Virtual DOM

### Virtual DOM이란?

실제 DOM을 업데이트하는 연산을 줄이기 위해 만들어진 가상의 DOM으로, 메모리에 저장되어 사용

#### 일반적인 DOM 노드

```html
<div id="main">
    <h1>Header</h1>
    <p>Description</p>
</div>
```

#### 일반적으로 DOM API를 사용하여 DOM을 변경한다는 것

```javascript
document.getElementById('main').appendChild('<p>Hello</p>');
```

#### Virtual DOM 노드 (Javascript 객체 형식)

```javascript
let domNode = {
    tag: 'div',
    attributes: { id: 'main' },
    children: []
};
```

#### Virtual DOM으로 변경하는 방법

```javascript
domNode.children.push('<p>Hello</p>');
```

=> DOM 조작을 위해서 getElementById와 같은 DOM API를 사용한 코드가 아닌 가상 돔을 사용한다면, `코드는 단지 자바스크립트 객체`로 변경되는 적은 비용만이 발생

#### Virtual DOM 과 실제 DOM의 동기화

```javascript
// This function would call the DOM API and make changes
// to the "real" DOM. It would do it in batches and with
// more efficiency than it would with arbitrary updates.
sync(originalDomNode, domNode);
```



## #4. Reactivity

Reactivity 반응형이란 자동적으로 DOM에 반응해주는 것

![image-20220623131102851](C:\Users\sbkim\AppData\Roaming\Typora\typora-user-images\image-20220623131102851.png)

1. 첫 Render

   - 데이터에 접근(touched)하게되면 getter 함수가 불려짐

   - getter는 dependency 데이터를 모으기 위해 watcher를 부름 (intent 이용)

     > **Intent**
     >
     > 구성 요소(Component) 간에 작업 수행을 위한 정보를 전달하는 역할
     >
     > 명시적: 목적지를 정확하게 지정
     >
     > 암시적: URI 값의 유형에 따라 action이 달라지는 것 처럼 action을 수행

   - watcher는 component render function을 호출

   - Component render function은 Virtual DOM Tree를 생성

2. 데이터 변경

   - dependency로 모아진 데이터를 바꾸게 되면 setter 함수가 불려짐
   - setter는 watcher에게 모든 변화를 전달
   - watcher는 component render function을 호출
   - Component render function은 Virtual DOM Tree 형성

=> Reactivity System은 created 전에 vue 인스턴스에 주입된다.

=> Component의 data property들을 Object.defineProperty()를 사용해 getter()와 setter() 메소드로 정의한다.

=> 초기에 컴포넌트 생성 시, getter와 setter로 정의되어 Reactivity System이 데이터를 인지할 수 있고, watcher가 데이터를 가져다 줄 수 있다.

## #5. Vue가 브라우저에 렌더링되는 원리





### 참고

* [렌더링 트리 생성 과정](https://hyojin96.tistory.com/m/entry/%EB%B8%8C%EB%9D%BC%EC%9A%B0%EC%A0%80%EC%9D%98-%EB%A0%8C%EB%8D%94%EB%A7%81-%EA%B3%BC%EC%A0%95-Render-Tree%EC%99%80-DOM-Tree%EC%9D%98-%EC%B0%A8%EC%9D%B4%EC%A0%90)

* [웹 렌더링 개념](https://pinokio0702.tistory.com/362)

* [Reactivity in vue](https://deepsource.io/blog/reactivity-in-vue/)
* [what is babel](https://bravenamme.github.io/2020/02/12/what-is-babel/)