# <img src="https://upload.wikimedia.org/wikipedia/commons/thumb/4/4c/Typescript_logo_2020.svg/512px-Typescript_logo_2020.svg.png" width=50 align=left> TypeScript

## TypeScript?

TypeScript 란 MS에 의해 개발/관리되고 있는 오픈소스 프로그래밍 언어로, JavaScript에 `타입`을 선언한 언어이다. 



## TypeScript vs. JavaScript

| 타입 시스템 기능            | JavaScript      | TypeScript           |
| --------------------------- | --------------- | -------------------- |
| 타입 결정 방식              | 동적            | 정적                 |
| 타입이 자동으로 변환되는가? | O               | X (대부분)           |
| 언제 타입을 확인하는가?     | 런타임          | 컴파일 타임          |
| 언제 에러를 검출하는가?     | 런타임 (대부분) | 컴파일 타임 (대부분) |

TypeScript는 **정적 타입**을 지원하므로 **컴파일 단계**에서 오류를 확인할 수 있는 장점이 있다.

```javascript
function sum(a, b) {
	return a + b;
}

sum(10, 20); // 30
sum('10', '20'); // 1020
```

```typescript
function sum(a: number, b: number): number {
	return a + b;
}

sum(10, 20); // 30
sum('10', 20); // error TS2345
```

> **TS2345**
>
> 앞으로 많이 보게 될 error code중 하나로, 이미 정해진 타입에 다른 타입을 할당하려할 때 발생하는 에러



## tsconfig.json

TypeScript프로젝트는 루트 디렉터리에 tsconfig.json 파일이 존재한다. 어떤 파일을 컴파일하고, 어떤 JavaScript 버전으로 방출하는지 등을 정의한다.

아래 파일과 같이 사용되며, 프로젝트 설정 시 중요한 옵션으로는 `noImplicitAny`, `strictNullChecks`, 등이 있다. (지원되는 전체 목록은 [상세 설명](https://typescript-kr.github.io/pages/tsconfig.json.html) 참고)

// @TODO 1

```json
{
  "compilerOptions": {
    "target": "esnext",
    "module": "esnext",
    "strict": true,
    "jsx": "preserve",
    "moduleResolution": "node",
    "experimentalDecorators": true,
    "skipLibCheck": true,
    "esModuleInterop": true,
    "allowSyntheticDefaultImports": true,
    "forceConsistentCasingInFileNames": true,
    "useDefineForClassFields": true,
    "sourceMap": true,
    "noImplicitAny": true,
    "strictNullChecks": true,
    "baseUrl": ".",
    "types": [
      "webpack-env"
    ],
    "paths": {
      "@/*": [
        "src/*"
      ]
    },
    "lib": [
      "esnext",
      "dom",
      "dom.iterable",
      "scripthost"
    ]
  },
  "include": [
    "src/**/*.ts",
    "src/**/*.tsx",
    "src/**/*.vue",
    "tests/**/*.ts",
    "tests/**/*.tsx"
  ],
  "exclude": [
    "node_modules"
  ]
}

```



## TypeScript's types

### 기본 타입

* boolean
* number
* string
* array (2가지 표현방법)
  * T[]
  * Array\<T\>
* tuple
  * 요소의 타입과 개수가 고정된 배열 표현
  * `let x: [string, number]`
    `x = ["hello", 10];`
  * 서버와 통신할 때 배열 내에서 타입, 개수를 지정해 보내는 경우가 많지 않아 보통 잘 사용하지 않는다.
* enum
* void
* null and undefined
  * tsconfig.json에 `strictNullChecks` 옵션을 사용한다면 null, undefined은 오직 any와 null, undefined 각자에만 할당 가능하다.
* **any**
  * 타입 검사를 하지 않고 컴파일 시 모든 검사를 통과한다.
  * 어떤 값이든 이 변수에 할당할 수 있다는 의미로, 타입을 사용하지 않는 것과 똑같다. 
  * 타입 오류를 사전에 방지하기 위해선 사용하는 것을 지양한다.
* never
  * 절대 발생할 수 없는 타입을 의미한다.

### 유니언 타입

* union

  * 어떤 메소드의 파라미터가 number나 string를 매개변수로 사용할 때 유니언 타입을 사용한다. (즉, **OR**)

  * ```typescript
    function concat(orgVal: string, newVal: string | number): string {
        return orgVal.concat(newVal);
    }
    
    concat("a", 1);  // a1
    concat("a", "1");  // a1
    ```

### 유틸리티 타입

자세한 사항은 [링크](https://typescript-kr.github.io/pages/utility-types.html)에서 확인 가능하다.

* Partial\<T\>

  * 주어진 타입의 모든 하위 집합을 나타낸다.

  * ```typescript
    interface Todo {
        title: string;
        description: string;
    }
    
    function updateTodo(todo: Todo, fieldsToUpdate: Partial<Todo>) {
        return { ...todo, ...fieldsToUpdate };
    }
    
    const todo1 = {
        title: 'organize desk',
        description: 'clear clutter',
    };
    
    const todo2 = updateTodo(todo1, {
        description: 'throw out trash',
    });
    ```

* Pick<T, K>

  * `T`에서 프로퍼티 `K`의 집합을 선택해 타입을 구성한다.

  * ```typescript
    interface Todo {
        title: string;
        description: string;
        completed: boolean;
    }
    
    type TodoPreview = Pick<Todo, 'title' | 'completed'>;
    
    const todo: TodoPreview = {
        title: 'Clean room',
        completed: false,
    };
    ```

* Record<K, T>

  * 타입 `T`의 프로퍼티의 집합 `K`로 타입을 구성한다.

  * ```typescript
    interface PageInfo {
        title: string;
    }
    
    type Page = 'home' | 'about' | 'contact';
    
    const x: Record<Page, PageInfo> = {
        about: { title: 'about' },
        contact: { title: 'contact' },
        home: { title: 'home' },
    };
    ```

  * // @TODO 3

#### Q. 현재 프로젝트에서는 Partial과 Pick을 사용하지 않고 있는데 해당 타입을 도입해도 좋지 않을까?

1. [Partial] 메소드의 파라미터에 `?` (optional) 만으로 대체되지 않는다면 Partial을 사용해도 나쁘진 않을 것 같지만, 필수값이 undefined | null인 경우에도 타입 오류가 나지 않기 때문에 사용하기 전 side effect를 고려해야할 것 같다.

2. [Pick] 사용하려고 한다면 따로 타입을 선언해주는 것이 필요할 것 같다.

   - ```typescript
     interface Todo {
         title: string;
         description: string;
         completed: boolean;
     }
     
     type TodoPreview = Pick<Todo, 'title' | 'completed'>; 
     ```

   - 타입이 너무 많아지는 것을 지양하기 위해서는 Partial, Pick을 사용하는 것도 지양하는게 좋지 않을까 생각한다. 

### 🤔그럼 이런 타입을 모두 다 선언해줘야하는가?

TypeScript는 **타입 추론**이 가능하므로 기본 타입의 경우 변수에 타입을 선언해줄 필요가 없다. 변수를 생성하며 초기값을 선언해준다면 해당 값으로 타입을 적절하게 추론한다 (하지만 한번 설정된 타입을 바꾸려고 한다면 JavaScript와 다르게 오류 발생: `error TS2322`). [자세한 설명](https://typescript-kr.github.io/pages/type-inference.html)



## TypeScript default value?

TypeScript에서 default value를 설정할 수 있는가? 에 대한 답은 '없다'이다. TypeScript에서 default value를 설정하고 싶다면 아래와 같은 방법을 사용할 수 있다.

1. default value를 미리 지정해두고 선언 시 사용

   ```typescript
   // x, y값을 기본으로 가지는 Point interface 생성
   interface Point {
   	x: number;
   	y: number;
   }
   
   // defaultPoint 선언
   const defaultPoint = {
       x: 0,
       y: 0,
   } as Point;
   ```

2. 함수의 parameter의 경우 default value 설정 가능

   ```typescript
   // 직접 선언
   const getPoint1 = (point = { x: 10, y: 10 } as Point): Point => {
   	return point;
   };
   
   // defaultPoint로 선언
   const getPoint2 = (point = defaultPoint): Point => {
       return point;
   }
   
   getPoint1(); // { x: 10, y: 10 }
   getPoint2(); // { x: 0, y: 0 }
   getPoint1({ x: 6, y: 6 }); // { x: 6, y: 6 }
   getPoint2({ x: 6, y: 6 }); // { x: 6, y: 6 }
   
   getPoint1({ y: 6 }); // error TS2345 (interface 필수값에 따라)
   getPoint2({ x: 6 }); // error TS2345 (defaultPoint 필수값에 따라)
   ```



## 그래서 왜 TypeScript ?

직접 개발을 통해 TypeScript, JavaScript를 사용해보니 infosafer v1에서 js를 사용할 때, 

* 변수명이 제대로 설정되지 않는 경우
* 해당 기능에 대한 insight가 없는 경우 
* 1000줄 이상의 코드가 있는 경우

=> 어떤 코드를 해석하고 있는지 중간에 놓치는 경우가 많았고, 모두 var, let으로만 이루어진 변수의 type은 무엇인지 어떤것을 의미하는지 잘 모르는 문제가 있었다.

TypeScript를 사용하게 된다면 메소드 파라미터의 `타입`을 선언할 수 있고, 해당 변수가 어떤 값인지 추론가능하므로 코드 해석이 훨씬 쉬워진다. 물론, 모든 메소드와 변수에 대해 타입을 설정하고 선언한다는 것이 처음엔 귀찮을 수 있지만 추후 유지 보수를 하거나 추가 기능을 개발할 때 훨씬 편하다는 장점이 있다.



---

## 참고

### Compiler

1. 개발자가 다수의 텍스트 파일을 작성
2. 컴파일러가 **파싱**하여 **추상 문법 트리**(ASTs, abstract syntax trees) 자료구조로 변환
3. AST를 바이트코드(bytecode)로 변환
4. Runtime이 바이트코드를 평가

TypeScript의 컴파일러는 코드를 바이트코드 대신 자바스크립트 코드로 변환한다. 이후로는 일반적인 자바스크립트 코드를 실행하는 것과 마찬가지로 브라우저, Node.js 등으로 실행한다.

TypeScript컴파일러는 AST를 만들어 결과 코드를 내놓기 전에 **타입 확인**을 거친다. 즉, **컴파일 단계**에서 오류를 확인할 수 있는 장점이 있다.

> TS
>
> 1. 타입스크립트 소스 => 타입스크립트 AST
> 2. 타입검사기가 AST를 확인
> 3. 타입스크립트 AST => 자바스크립트 소스
>
> JS
>
> 1. 자바 스크립트 소스 => 자바스크립트 AST
> 2. AST => 바이트코드
> 3. 런타임이 바이트코드를 평가



---

**TODO**

* tsconfig.json에 현재 프로젝트 설정 확인
* d.ts 파일 만들기 https://typescript-kr.github.io/pages/declaration-files/creating-dts-files-from-js.html
* Record<K, T> 실제 사례 확인 필요

---

## Plugins

Register globally

Register locally

