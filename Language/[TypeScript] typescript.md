# TypeScript

## :computer: TypeScript란?

TypeScript란 Microsoft에서 개발 및 유지 관리 하는 오픈소스 프로그래밍 언어이다.

TypeScript는 ES5의 Superset이므로 기존의 자바스크립트(ES5) 문법을 그대로 사용할 수 있다. 쉽게 말해 JavaScript에 `타입`을 부여한 언어이다.
<img src="https://blog.hax0r.info/assets/images/posts/2020-12-15-from-the-grave-to-the-cradle-with-typescript/typescript-superset.png">

### JavaScript vs. TypeScript

* JavaScript
    * `동적 타이핑 (Dynamic Typing)`
    * 변수나 반환값의 타입을 사전에 지정하지 않는다.
    * 타입 결정 이후에도 같은 변수에 여러 타입의 값을 교차하여 할당 가능하다.
    * `런타임` 시 에러를 발생시킨다.
* TypeScript
    * `정적 타이핑 (Static Typing)`
    * 타입을 명시적으로 선언한다.
    * 타입이 선언된 이후에는 변경할 수 없다.
    * `컴파일` 시 에러를 발생시킨다.
    * JavaScript와 달리 브라우저에서 실행하려면 compile해야한다.

## 🤔 TypeScript를 쓰는 이유?

크게 에러의 사전 방지 및 코드 가이드 및 자동완성을 통한 개발 생산성 향상을 위해 사용한다.

### 에러의 사전 방지

``` javascript
// js
function sum(a, b) {
	return a + b;
}
```

``` typescript
// ts
function sum(a: number, b:number) {
	return a + b;	
}
```

``` javascript
sum(10, 20);	// ex1
sum('10', '20');	// ex2
```

`sum` 함수를 이용하여 ex1과 같이 숫자 10과 20을 더하게 되면 js, ts 모두 20을 얻을 수 있다.

ex2와 같이 숫자 대신 문자열을 더하게 될 경우 js는 `1020` 이라는 결과를 반환한다. 그러나, ts의 경우 컴파일 단계에서 오류를 발생시켜 의도하지 않은 코드의 동작을 예방할 수 있다.

### 코드 자동 완성과 가이드

TypeScript를 사용하게되면 `Visual Studio Code` 와 같은 개발 툴의 기능을 최대로 활용할 수 있다.

JavaScript로 코드를 작성하게 되면 작성 시점에 변수의 타입을 모르기 때문에 API를 사용할 수 없다. 그러나 TypeScript로 코드를 작성하게 되면 타입이 지정되어 있기 때문에 해당 타입에 대한 API를 미리 보기로 볼 수 있고 빠르고 정확하게 작성할 수 있다.

## :bookmark_tabs: 기본 타입

JavaScript에서 추가된 데이터 타입이 있다.

* tuple
* enum
* any
* void
* never

### Boolean

``` typescript
let isTrue: boolean = false;
```

### Number

``` typescript
let num: number = 6;
```

### String

``` typescript
let name: string = "typescript";
```

### Array

``` typescript
let arr: number[] = [1, 2, 3];	// []
let arr: Array<number> = [1, 2, 3];	// 제네릭 배열 타입
```

### Tuple

요소의 타입과 개수가 고정된 배열을 표현 가능하다.

``` typescript
let arr: [string, number];
arr = ["hello", 1];

console.log(arr[0].substring(1));	// 정해진 인덱스에 위치한 요소에 접근 가능
```

### Enum

특정 값이 어떤 `Color` enum멤버와 매칭되는지를 알고 싶을 때, 이 값을 이용하면 일치하는 enum 멤버의 이름을 알아낼 수 있다.

``` typescript
enum Color {Red, Green, Blue}
let c: Color = Color.Green;
```

### Any

알지 못하는 타입을 표현해야 할 경우 타입 검사를 하지 않고, 그 값들이 컴파일 시간에 검사를 통과시킬 때 사용한다.

``` typescript
let notSure: any = 4;
notSure = "maybe a string instead";
notSure = false; // success
```

또한, any 타입은 타입의 일부만 알고 전체는 알지 못할 때 사용한다. 예를 들어, 여러 다른 타입이 섞인 배열을 다룰 수 있다.

``` typescript
let list: any[] = [1, true, "free"];

list[1] = 100;
```

### Void

변수에는 `undefined`와 `null`만 할당하고, 함수에는 반환 값을 설정할 수 없는 타입이다. 보통 함수에서 반환 값이 없을 때 반환 타입을 표현하기 위해 쓰인다.

``` typescript
let unuseful: void = undefined;
function warnUser(): void {
    console.log("This is my warning message");
}
```

### Null and Undefined

``` typescript
// 이 밖에 이 변수들에 할당할 수 있는 값이 없다!
let u: undefined = undefined;
let n: null = null;
```

**기본적으로 `null` 과 `undefined`는 다른 모든 타입의 하위 타입이다.**

하지만, `--strictNullChecks`를 사용하면, `null`과 `undefined`는 오직 `any`와 각자 자신들 타입에만 할당 가능하다.

### Never

함수의 끝에 절대 도달하지 않는다는 의미를 지닌 타입이다.

``` typescript
// 이 함수는 절대 함수의 끝까지 실행되지 않는다는 의미
function neverEnd(): never {
  while (true) {

  }
}
```

### Object

`object`는 원시 타입(Primitive Type)이 아닌 타입을 나타낸다.

### 변수 선언 시 let을 사용하는 이유

값이 변하지 않는 변수는 `const`를, 값이 변하는 변수는 `let`을 사용하여 선언한다. `var`는 절대로 사용하지 않도록 한다.

> * let: 중복 선언 불가능 > 블럭 내부의 변수는 외부에서 사용 불가능
> * var: 중복 선언 가능 > 블럭 내부의 변수를 외부에서 사용 가능

``` javascript
var foo = 123;
console.log(foo); //123

{
    var foo=456;  // 중복선언이 가능, 위에 선언된 애를 블럭내에서도 참조가 되버림
}
console.log(foo); //456 , 블럭밖에서도 참조가 됩니다. 

let foo2_ = 789;
console.log(foo2_); //789
{
    let foo2_:number = 456; 
    // 중복선언이 불가능. 같은 변수를 두번 선언 할 수 없음. 
    // /위에걸 쓰는게 아니고 블럭내의 변수하나를 새로 만드는것
    let bar:number = 456;
    console.log(foo2_);
    console.log(bar); //456
}
foo2_ = 567;
console.log(foo2_); //789
```

## :gear: TypeScript 개발 환경 설정

1. TypeScript 컴파일러 설치

``` shell
$ npm install -g typescript
```

2. ts 파일 작성 (test.ts)

``` typescript
let helloWorld = "Hello World!!";
const user = {
	name: "TypeScript",
	id: 0,
};
```

3. ts 파일 컴파일

``` shell
$ tsc test.ts
```

4. JavaScript 코드로 변환 (test.js)

``` javascript
var helloWorld = "Hello World!!";
var user = {
	name: "TypeScript",
	id: 0,
};
```

* `tsconfig.json`에서 컴파일 옵션 수정이 가능하다.

- - -

추가사항 업데이트 예정

## :books: 참고

* [타입스크립트 - 번역본](https://typescript-kr.github.io/)
* [이제는 타입스크립트를 배워야합니다. (to 자바스크립트 개발자)](https://blog.hax0r.info/2019-03-12/typescript-in-fastcampus/)
* [Typescript 와 함께 요람에서 무덤까지 1부](https://blog.hax0r.info/2020-12-15/from-the-grave-to-the-cradle-with-typescript/)
* [TypeScript의 소개와 개발 환경 구축](https://poiemaweb.com/typescript-introduction)
* [Typescript를 이용한 Node.js API 서버 프로그래밍](https://nashorn.tistory.com/entry/Typescript%EB%A5%BC-%EC%9D%B4%EC%9A%A9%ED%95%9C-Nodejs-API-%EC%84%9C%EB%B2%84-%ED%94%84%EB%A1%9C%EA%B7%B8%EB%9E%98%EB%B0%8D)
* [8장. 리액트 프로젝트에서 타입스크립트 사용하기](https://react.vlpt.us/using-typescript/)
* [리액트 + 타입스크립트](https://codingmoondoll.tistory.com/entry/%EC%B8%84%EB%9D%BC%EC%9D%B4-%EC%B8%84%EB%9D%BC%EC%9D%B4-%EB%A6%AC%EC%95%A1%ED%8A%B8-%ED%83%80%EC%9E%85%EC%8A%A4%ED%81%AC%EB%A6%BD%ED%8A%B8%EB%B6%80%EC%A0%9C-%EB%82%98%EB%A7%8C-%EA%B3%A0%ED%86%B5-%EB%B0%9B%EC%9D%84-%EC%88%98%EB%8A%94-%EC%97%86%EC%A7%80)
* [뷰에 타입스크립트 적용하기](https://joshua1988.github.io/vue-camp/ts/with-vue.html)
* [TS 환경설정](https://velog.io/@hopsprings2/TypeScript-%EC%8B%9C%EC%9E%91%ED%95%98%EA%B8%B0-%ED%94%84%EB%A1%9C%EC%A0%9D%ED%8A%B8-%ED%99%98%EA%B2%BD%EC%84%A4%EC%A0%95)
* [타입스크립트 핸드북](https://joshua1988.github.io/ts/guide/basic-types.html#tuple)
* [컴파일옵션 정리](https://geonlee.tistory.com/214)
