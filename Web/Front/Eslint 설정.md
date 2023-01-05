# Eslint

현재 Vue.js 사용

## 패키지 설치
Vue 팀에서 공식적으로 제공하는 esling-plugin-vue package 다운로드

```shell
$ npm install --save-dev eslint @typescript-eslint/parser @typescript-eslint/eslint-plugin
$ npm install --save-dev eslint-config-prettier eslint-plugin-vue 
```

## .eslintrc.js

root 하위에 `.eslintrc.js` 파일 생성

```javascript
module.exports = {
  root: true,
  env: {
    node: true,
  },
  extends: [
    'plugin:vue/essential',         // eslint-plugin-vue 패키지에서 제공
    'eslint:recommended',           // eslint 에서 권장하는 javascript 코드 규칙
    '@vue/typescript/recommended',
    '@vue/prettier',
    '@vue/prettier/@typescript-eslint',
  ],
  parserOptions: {
    ecmaVersion: 2020,  // ECMAScript Version
  },
  rules: {
    // add your custom rules here
  }
};
```

## eslint-plugin-vue 제공 rules

[eslint-plugin-yue](https://eslint.vuejs.org/rules/) 에서 필요한 rules 검색 가능

### vue/multiline-html-element-content-newline

`plugin:vue/vue3-strongly-recommended`, `plugin:vue/strongly-recommended`, `plugin:vue/recommended` 에 포함된 rule

현재 `plugin:vue/essential` 사용중이어서 적용되지 않고 있었음

#### default options

```javascript
{
    "vue/multiline-html-element-content-newline": ["error", {
        "ignoreWhenEmpty": true,
        "ignores": ["pre", "textarea", ...INLINE_ELEMENTS],
        "allowEmptyLines": false
    }]
}
```

#### rules 적용

```javascript
rules: {
    "vue/multiline-html-element-content-newline": 'warn',
}
```

