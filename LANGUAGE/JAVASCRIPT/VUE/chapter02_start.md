# VUE

<https://blog.storyg.co/vue-js-posts/todos-tutorial>

인터넷 개발환경 : <https://jsfiddle.net/chrisvfritz/50wL7mdz/>

```txt
src
├── assets
│   └── logo.png
├── components
│   └── HelloWorld.vue
├── router
│   └── index.js
├── App.vue
└── main.js
```

- assets : 외부에서 가지고 온 이미지, 파일, css, js 파일 등을 넣어두는 폴더
- components : VueJs 에서 사용하는 확장명 vue 파일 들을 생성하고 구현하는 곳,프로젝트 생성 후에는 ‘HelloWorld.vue’ 파일
- router/index.js : Vue 에서는 서버사이드에서 제공하는 라우팅을 사용하지 않아도 라우팅을 할 수 있도록 도와주는 Vue Router. 이것을 가지고 페이지를 서버에 요청하지 않아도 새롭운 페이지로 이동
- App.vue : 프로젝트가 다루는 컴포넌트가 표시되는 Root 컴포넌트
- main.js : 프로젝트의 Base 파일입니다. 전역 설정을 하려면 main.js를 수정
