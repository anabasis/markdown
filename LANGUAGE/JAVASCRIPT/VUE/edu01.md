# VUE

## 설치

```js
<script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script><!-- 개발버전, 도움되는 콘솔 경고를 포함. -->
<script src="https://cdn.jsdelivr.net/npm/vue"></script><!-- 상용버전, 속도와 용량이 최적화됨. -->
```

```bash
npm install vue

npm install -g @vue/cli
yarn global add @vue/cli

vue create 프로젝트명
vue init webpack 프로젝트명 (local)
vue init username/repo 프로젝트명 (github)

cd 프로젝트명
npm install (모듈 업데이트)
npm run serve(npm run dev, npm run build)
```

## 기본템플릿

```js
<template lang="html">
    <div id="app">
      {{ message }}
    </div>
</template>

<script>
// @ ROOT부터 페키지 검색
export default {
    data() : {

    },
    components: {
    }
}
</script>

<style lang="css" scoped>
#app {

}
</style>
```

- 예제 1

```html
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script><!-- 개발버전, 도움되는 콘솔 경고를 포함. -->
  </head>
  <body>
      <div id="app">
          {{person.name}}  {{person.age}}<br/>
          {{ nextYear('안녕하세요.') }}<br/>
          <input v-bind:type="type" v-bind:value="inputData"><br/>
          <input :type="type" :value="inputData"><!-- v-bind 생략 가능 --><br/>
          <a :href='link'> InfoRect-Portal</a>
          <button v-on:click="alert" >Click Me</button><br/>
          <form v-on:submit.prevent="submit">
              <!--<input type="text" :value="text" @keyup="updateText" ><br/>-->
              <input type="text" v-model="text" ><br/>
              {{ text }} <br/>
              <button type="submit">버튼</button>
          </form>
      </div>
      <script>
      new Vue({
          el:'#app',
          data:{
              person : {
                  name : 'InfoRect-Portal',
                  age : 1
              },
              inputData: 'hello',
              type:'text',
              text:'text',
              link:'https://www.youtube.com/'
          },
          methods : {
              alert(){
                 alert("TEST TEST");
              },
              submit(){
                  alert("submitted!!");
                  console.log("hello");
              }
          }
      })
      </script>
  </body>
</html>
```

- 예제 2

```html
<html lang="en">
  <head>
    <meta charset="utf-8">
    <meta http-equiv="X-UA-Compatible" content="IE=edge">
    <meta name="viewport" content="width=device-width,initial-scale=1.0">
    <title>Document</title>
    <script src="https://cdn.jsdelivr.net/npm/vue/dist/vue.js"></script><!-- 개발버전, 도움되는 콘솔 경고를 포함. -->
  </head>
  <body>
      <div id="app">
          {{person.name}}  {{person.age}}<br/>
          {{ nextYear('안녕하세요.') }}<br/>
          <input v-bind:type="type" v-bind:value="inputData"><br/>
          <input :type="type" :value="inputData"><!-- v-bind 생략 가능 --><br/>
          <a :href='link'> InfoRect-Portal</a>
          <button v-on:click="alert" >Click Me</button><br/>
          <form v-on:submit.prevent="submit">
              <!--<input type="text" :value="text" @keyup="updateText" ><br/>-->
              <input type="text" v-model="text" ><br/>
              {{ text }} <br/>
              <button type="submit">버튼</button>
          </form>
      </div>
      <script>
      new Vue({
          el:'#app',
          data:{
              person : {
                  name : 'InfoRect-Portal',
                  age : 1
              },
              inputData: 'hello',
              type:'text',
              text:'text',
              link:'https://www.youtube.com/'
          },
          methods : {
              alert(){
                 alert("TEST TEST");
              },
              submit(){
                  alert("submitted!!");
                  console.log("hello");
              }
          }
      })
      </script>
  </body>
</html>
```

## 소스코드 분석

- /public/index.html

```html
...
<div id="app"></div>
...
```

- /src/main.js

```js
import Vue from 'vue'
import App from './App.vue'
import router from './router'
import store from './store'

Vue.config.productionTip = false
new Vue({
  router,
  store,
  render: h => h(App)
}).$mount('#app')
```

- /src/App.vue

```js
<template>
  <div id="app">
    <div id="nav">
      <router-link to="/">Home</router-link> |
      <router-link to="/about">About</router-link>
    </div>
    <router-view/>
  </div>
</template>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
#nav {
  padding: 30px;
}
#nav a {
  font-weight: bold;
  color: #2c3e50;
}
#nav a.router-link-exact-active {
  color: #42b983;
}
</style>
```

- /router/index.js

```js
import Vue from 'vue'
import VueRouter from 'vue-router'
import Home from '../views/Home.vue'

Vue.use(VueRouter)

const routes = [
  {
    path: '/',
    name: 'Home',
    component: Home
  },
  {
    path: '/about',
    name: 'About',
    // route level code-splitting
    // this generates a separate chunk (about.[hash].js) for this route
    // which is lazy-loaded when the route is visited.
    component: () => import(/* webpackChunkName: "about" */ '../views/About.vue')
  }
]

const router = new VueRouter({
  mode: 'history',
  base: process.env.BASE_URL,
  routes
})

export default router
```

- /store/index.js

```js
import Vue from 'vue'
import Vuex from 'vuex'

Vue.use(Vuex)

export default new Vuex.Store({
  state: {
  },
  mutations: {
  },
  actions: {
  },
  modules: {
  }
})

```

- /views/Home.vue

```js
<template>
  <div class="home">
    <img alt="Vue logo" src="../assets/logo.png">
    <HelloWorld msg="Welcome to Your Vue.js App"/>
  </div>
</template>

<script>
// @ is an alias to /src
import HelloWorld from '@/components/HelloWorld.vue'

export default {
  name: 'Home',
  components: {
    HelloWorld
  }
}
</script>
```

- /views/About.vue

```js
<template>
  <div class="about">
    <h1>This is an about page</h1>
  </div>
</template>
```

- views 일반 페이지 컴포넌트
- components 페이지내 기능 컴포넌트

![라이프사이클 다이어그램](./images/lifecycle.png)

![1단계emit](./images/1단계.PNG)

![2단계store-vuex](./images/2단계.PNG)
