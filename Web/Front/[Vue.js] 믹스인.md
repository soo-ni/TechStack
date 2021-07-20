# [Vue.js] Mixin

## Mixin이란?

믹스인(Mixins)이란 뷰에서 나온 용어가 아닌 `객체 지향 프로그래밍 언어`에서 다른 클래스의 부모클래스가 되지 않으면서 다른 클래스에서 사용할 수 있는 메서드를 포함하는 `클래스`이다. 구현된 메서드가 포함된 `인터페이스`로 볼 수도 있으며 이 패턴은 SOLID 중 D(dependency inversion)를 적용하는 예이다. 믹스인은 때때로 **상속**이 아니라 **포함**으로 설명된다.

믹스인은 여러 컴포넌트 간에 공통으로 사용하고 있는 로직, 기능들을 **재사용**하는 방법이다. 믹스인에 정의할 수 있는 재사용 로직은 data, methods, created 등과 같은 컴포넌트 옵션이다. 컴포넌트에 mixin을 사용하면 해당 mixin의 모든 옵션이 컴포넌트의 고유 옵션에 **혼합**된다.

* 공식 문서: https://kr.vuejs.org/v2/guide/mixins.html

### Mixin 장점

* 코드 재사용성
* 오버라이딩을 통한 커스텀 및 확장 가능

### 예시

```javascript
// mixin 객체 생성
var myMixin = {
  created: function () {
    this.hello()
  },
  methods: {
    hello: function () {
      console.log('hello from mixin!')
    }
  }
}

// mixin을 사용할 컴포넌트 정의
var Component = Vue.extend({
  mixins: [myMixin]
})

var component = new Component() // => "hello from mixin!"
```

### 옵션 병합

mixin과 컴포넌트 자체에 중첩 옵션이 포함되어 있으면 `병합`된다.

```javascript
var mixin = {
  data: function () {
    return {
      message: 'hello',
      foo: 'abc'
    }
  }
}

new Vue({
  mixins: [mixin],
  data: function () {
    return {
      message: 'goodbye',
      bar: 'def'
    }
  },
  created: function () {
    console.log(this.$data)
    // => { message: "goodbye", foo: "abc", bar: "def" }
  }
})
```





### :books: 참고

* [Cracking Vue.js - 믹스인](https://joshua1988.github.io/vue-camp/reuse/mixins.html#%EB%AF%B9%EC%8A%A4%EC%9D%B8)
* [Vue.js Mixins: 믹스인은 왜 필요한가? :: 마이구미](https://mygumi.tistory.com/266)
* [[Vue] vue.js Mixin을 활용해서 재사용성을 늘리자!](https://webruden.tistory.com/224)

