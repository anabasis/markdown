# VUE 설치

<https://kr.vuejs.org/>
<https://router.vuejs.org/kr/>

모바일 :
<https://medium.com/@JonUK/creating-a-mobile-web-app-with-vue-vuetify-typescript-dc69bed4cd2d>

## 개발 환경

<https://ict-nroo.tistory.com/84>

## DevTool

Chrome 확장설치 + F12

## 직접 Script 추가

최신버젼 CDN : <https://unpkg.com/vue>

## NPM

```bash
# vue-cli 설치
$ npm install -g vue-cli

# "webpack" 템플릿을 이용해서 새 프로젝트 생성
$ vue init webpack vue-project

# 의존성을 설치하고 실행하세요!
$ cd vue-project

$ npm install
$ npm run dev
```

```bash
# 최신 안정화 버전
npm install vue

npm install -g @vue/cli
# OR
yarn global add @vue/cli

vue create test1

npx @vue/cli create test2

# "webpack" 템플릿을 이용해서 새 프로젝트 생성
$ vue init webpack vue-project

# 의존성을 설치하고 실행하세요!
$ cd vue-project

$ npm install
$ npm run dev
```

|                           |        UMD         |       CommonJS        |     ES Module      |
| :-----------------------: | :----------------: | :-------------------: | :----------------: |
|           Full            |       vue.js       |     vue.common.js     |     vue.esm.js     |
|       Runtime-only        |   vue.runtime.js   | vue.runtime.common.js | vue.runtime.esm.js |
|     Full (production)     |     vue.min.js     |           -           |         -          |
| Runtime-only (production) | vue.runtime.min.js |           -           |         -          |

- Full : 컴파일러와 런타임을 포함.
- Compiler : 템플릿 문자열을 JavaScript 렌더링 함수로 컴파일하는 코드입니다.
- Runtime : Vue 인스턴스 생성, 가상 DOM 렌더링 및 패치 등을 담당하는 코드. 본적으로 모든 컴파일러를 제외한 것입니다.
- UMD: UMD 빌드는 \<script\>태그를 통해 브라우저에서 직접 사용할 수 있습니다. <https://unpkg.com/vue>의 Unpkg CDN의 기본 파일은 Runtime + Compiler UMD 빌드 (vue.js)입니다.
- CommonJS : CommonJS 빌드는 browserify 또는 webpack 1와 같은 이전 번들과 함께 사용하기 위한 것입니다. 이러한 번들(pkg.main)의 기본 파일은 런타임 전용 CommonJS 빌드(vue.runtime.common.js)입니다.
- ES Module : ES 모듈 빌드는 webpack 2 또는 rollup과 같은 최신 번들과 함께 사용하기 위한 것입니다. 이러한 번들(pkg.module)의 기본 파일은 런타임 전용 ES 모듈 빌드(vue.runtime.esm.js)입니다.

## Runtime + Compiler vs. Runtime-only

```js
// 아래 내용은 컴파일러가 필요합니다
new Vue({
  template: "<div>{{ hi }}</div>",
});

// 아래는 필요하지 않습니다
new Vue({
  render(h) {
    return h("div", this.hi);
  },
});
```

- 런타임 전용 빌드는 전체 빌드보다 약 30% 정도 작아서 언제든지 사용할 수 있습니다. 하지만 전체 빌드를 사용하려면 번들러에서 별칭을 구성해야합니다.

```js
## Webpack
module.exports = {
  // ...
  resolve: {
    alias: {
      'vue$': 'vue/dist/vue.esm.js' // 'vue/dist/vue.common.js'  webpack 1용 입니다
    }
  }
}

## Rollup
const alias = require('rollup-plugin-alias')

rollup({
  // ...
  plugins: [
    alias({
      'vue': 'vue/dist/vue.esm.js'
    })
  ]
})

## Browserify package.json
{
  // ...
  "browser": {
    "vue": "vue/dist/vue.common.js"
  }
}
```
