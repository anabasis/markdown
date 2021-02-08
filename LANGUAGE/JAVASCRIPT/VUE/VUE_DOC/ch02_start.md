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

## 선택적 렌더링

```html
<div id="app">
  {{ message }}
</div>
```

```js
var app = new Vue({
  el: '#app',
  data: {
    message: '안녕하세요 Vue!'
  }
})
```

## 엘리먼트 속성 바인딩

- `v-bind:` 대신 `:`사용 가능

```html
<div id="app-2">
  <span v-bind:title="message">
  <!-- <span :title="message"> 생략가능 -->
    내 위에 잠시 마우스를 올리면 동적으로 바인딩 된 title을 볼 수 있습니다!
  </span>
</div>
```

```js
var app2 = new Vue({
  el: '#app-2',
  data: {
    message: '이 페이지는 ' + new Date() + ' 에 로드 되었습니다'
  }
});
```

## 조건문

```html
<div id="app-3">
  <p v-if="seen">이제 나를 볼 수 있어요</p>
</div>
```

```js
var app3 = new Vue({
  el: '#app-3',
  data: {
    seen: true
  }
})
```
## 반복문

```html
<div id="app-4">
  <ol>
    <li v-for="todo in todos">
      {{ todo.text }}
    </li>
  </ol>
</div>
```

```js
var app4 = new Vue({
  el: '#app-4',
  data: {
    todos: [
      { text: 'JavaScript 배우기' },
      { text: 'Vue 배우기' },
      { text: '무언가 멋진 것을 만들기' }
    ]
  }
})
```

## 이벤트 핸들러

- `v-on:` 대신 `@`사용 가능

```html
<div id="app-5">
  <p>{{ message }}</p>
  <button v-on:click="reverseMessage">메시지 뒤집기</button>
  <!-- <button @click="reverseMessage">메시지 뒤집기</button> -->
</div>
```

```js
var app5 = new Vue({
  el: '#app-5',
  data: {
    message: '안녕하세요! Vue.js!'
  },
  methods: {
    reverseMessage: function () {
      this.message = this.message.split('').reverse().join('')
    }
  }
})
```

## 양방향 바인딩

- `v-on:input` 보다 적은 코드로 구현가능
- 상세한 부문은 `v-on:input`로 구현

```html
<div id="app-6">
  <p>{{ message }}</p>
  <input v-model="message">
</div>
```

```js
var app6 = new Vue({
  el: '#app-6',
  data: {
    message: '안녕하세요 Vue!'
  }
})
```

## 컴포넌트

```html
<ol>
  <!-- todo-item 컴포넌트의 인스턴스 만들기 -->
  <todo-item></todo-item>
</ol>
```

```js
// todo-item 이름을 가진 컴포넌트를 정의합니다
Vue.component('todo-item', {
  template: '<li>할일 항목 하나입니다.</li>'
})

var app = new Vue(...)
```

### props

```html
<div id="app-7">
  <ol>
    <!--
      이제 각 todo-item 에 todo 객체를 제공합니다.
      화면에 나오므로, 각 항목의 컨텐츠는 동적으로 바뀔 수 있습니다.
      또한 각 구성 요소에 "키"를 제공해야합니다 (나중에 설명 됨).
     -->
    <todo-item
      v-for="item in groceryList"
      v-bind:todo="item"
      v-bind:key="item.id"
    ></todo-item>
    <!--
    <todo-item
      v-for="item in groceryList"
      :todo="item"
      :key="item.id"
    ></todo-item>
    -->
  </ol>
</div>
```

```js
Vue.component('todo-item', {
  props: ['todo'],
  template: '<li>{{ todo.text }}</li>'
})

var app7 = new Vue({
  el: '#app-7',
  data: {
    groceryList: [
      { id: 0, text: 'Vegetables' },
      { id: 1, text: 'Cheese' },
      { id: 2, text: 'Whatever else humans are supposed to eat' }
    ]
  }
})
```
