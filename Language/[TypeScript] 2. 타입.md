# [TypeScript] 타입

타입스크립트가 지원하는 각각의 타입을 알아본다. 각 타입이 무엇을 포함할 수 있는지, 어떤 동작을 수행할 수 있는지 알 수 있다.

## 타입스크립트 타입 계층

<img src="https://media.vlpt.us/images/winbigcoms/post/cbaf6cf0-a5cf-4254-907c-41ec7cbac718/image.png">

## 타입

### any

any를 사용했을 때 예외를 발생시키고 싶다면 tsconfig.json 파일에서 **noImplicitAny** = true로 변경한다.

### unknown

```typescript
let a: unknown = 30	// unknown
let b = a === 123	// boolean
let c = a + 10		// 에러 TS2571: 객체의 타입이 'unknown'
if (typeof a === 'number') {
    let d = a + 10	// number
}
```

### boolean

### number

### bigint

###  string

### symbol

실무에서 자주 사용하지 않는 편이며 객체와 맵에서 문자열 키를 대신하는 용도로 사용한다.

### object

### Array

타입스크립트는 T[] 혹은 Array<T> 두가지 문법을 지원한다.

### Tuple

```typescript
let friends: [numbre, string, ...string[]] = [1, 'A', 'B', 'C', 'D']
```

### null, undefined, void, never

void는 명시적으로 아무것도 반환하지 않는 함수의 반환 타입을 가리키며 never는 절대 반환하지 않는 (예외를 던지거나 영원히 실행되는) 함수를 가리킨다. unknown이 모든 타입의 상위 타입이라면 never은 모든 타입의 서브타입이다.

### enum

## 그 외 타입

### 타입 별칭

```typescript
type Age = number
type Person = {
    name: 'soo-ni',
    age: Age
}

let age: Age = 55
let driver: Person = {
    name: 'soo-ni',
    age: age
}
```

### 유니온(|)과 인터섹션(&) 타입

```typescript
type Cat = {}
type Dog = {}
type CatOrDog = Cat | Dog
type CatAndDog = Cat & Dog
```



### :books: 참고

타입스크립트 프로그래밍



