# 👊🤢 

## Vue Error
### Vue 무한 render 늪

window.opener['app']에 parent VueComponent 추가해서 해당 vue에 있는 내용 참조하는 Popup을 띄우고 부모 창에서 다른 페이지로 이동하려고 하면 무한 루프에 빠지는 오류 발생

```
vue.runtime.esm.js?2b0e:619 [Vue warn]: You may have an infinite update loop in a component render function.
```

반응형으로 참조해서 생기는 문제 발생 (window.opener['app']이 변경되면 무한 렌더링 발생하는 것 같다..아마..?) 

``` typescript
// 변경 전
const state = reactive({
  parent: '',
});
state.parent = window.opener['app'];

// 변경 후 
let parent = window.opener['app'];
```

> **렌더링** 관련해서 더 공부해볼 것

* 참고

https://stackoverflow.com/questions/43151265/you-may-have-an-infinite-update-loop-in-a-component-render-function

