# 디렉티브

## v-if

```html
<h1 v-if="awesome">Vue is awesome!</h1>

<h1 v-if="awesome">Vue is awesome!</h1>
<h1 v-else>Oh no !!</h1>
```

### `<template>`에 `v-if`을 갖는 조건부 그룹

```html
<template v-if="ok">
  <h1>Title</h1>
  <p>Paragraph 1</p>
  <p>Paragraph 2</p>
</template>
```

## v-else

```html
<div v-if="Math.random() > 0.5">
  이제 나를 볼 수 있어요
</div>
<div v-else>
  이제는 안보입니다
</div>
```

## v-else-if

```html
<div v-if="type === 'A'">
  A
</div>
<div v-else-if="type === 'B'">
  B
</div>
<div v-else-if="type === 'C'">
  C
</div>
<div v-else>
  Not A/B/C
</div>
```

## `key`를 이용한 재사용 가능 엘리먼트 제어

- 두 템플릿 모두 같은 요소를 사용하므로 `<input>`은 대체되지 않고 단지 `placeholder`만 변경

```html
<template v-if="loginType === 'username'">
  <label>사용자 이름</label>
  <input placeholder="사용자 이름을 입력하세요">
</template>
<template v-else>
  <label>이메일</label>
  <input placeholder="이메일 주소를 입력하세요">
</template>
```

- 입력이 처음부터 렌더링

```html
<template v-if="loginType === 'username'">
  <label>사용자 이름</label>
  <input placeholder="사용자 이름을 입력하세요" key="username-input">
</template>
<template v-else>
  <label>이메일</label>
  <input placeholder="이메일 주소를 입력하세요" key="email-input">
</template>
```

## v-show(조건부 표시)

```html
<h1 v-show="ok">안녕하세요!</h1>
```

## v-for

```html
<ul id="example-1">
  <li v-for="item in items">
    {{ item.message }}
  </li>
</ul>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

### 현재 항목의 인덱스에 대한 두 번째 전달인자 옵션을 제공

- in 대신에 of를 구분자로 사용

```html
<ul id="example-2">
  <li v-for="(item, index) in items">  
    {{ parentMessage }} - {{ index }} - {{ item.message }}
  </li>
</ul>
```

```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    parentMessage: 'Parent',
    items: [
      { message: 'Foo' },
      { message: 'Bar' }
    ]
  }
})
```

### 객체의 속성을 반복

```html
<ul id="v-for-object" class="demo">
  <li v-for="value in object">
    {{ value }}
  </li>
</ul>
```

```js
new Vue({
  el: '#v-for-object',
  data: {
    object: {
      title: 'How to do lists in Vue',
      author: 'Jane Doe',
      publishedAt: '2016-04-10'
    }
  }
})
```

```html
<div v-for="(value, name, index) in object">
  {{ index }}. {{ name }}: {{ value }}
</div>
```

### Maintaining State

- 객체나 배열처럼, 기본 타입(Primitive value)이 아닌 값을 키로 사용해서는 안됨. 대신 문자열이나 숫자를 사용

```html
<div v-for="item in items" v-bind:key="item.id">
  <!-- content -->
</div>
```

### 배열 변경 감지

#### 배열 변경 감지

- Vue는 감시중인 배열의 변이 메소드를 래핑하여 뷰 갱신을 트리거
- push(), pop(), shift(), unshift(), splice(), sort(), reverse()

#### 배열대체

- 호출된 원본 배열을 변형
- filter(), concat() 와 slice()
- 항상 새 배열을 반환

```js
example1.items = example1.items.filter(function (item) {
  return item.message.match(/Foo/)
})
```

#### 주의사항

- JavaScript의 제한으로 인해 Vue는 배열에 대해 다음과 같은 변경 사항을 감지할 수 없습니다.

1. 인덱스로 배열에 있는 항목을 직접 설정하는 경우, 예: `vm.items[indexOfItem] = newValue`
2. 배열 길이를 수정하는 경우, 예: `vm.items.length = newLength`

```js
var vm = new Vue({
  data: {
    items: ['a', 'b', 'c']
  }
})
vm.items[1] = 'x' // reactive하지 않음
vm.items.length = 2 // reactive하지 않음

// 1번상황 극복 ////////////////////////////
// Vue.set
Vue.set(vm.items, indexOfItem, newValue)
// Array.prototype.splice
vm.items.splice(indexOfItem, 1, newValue)

// 2번상황 극복 ////////////////////////////
vm.$set(vm.items, indexOfItem, newValue)
vm.items.splice(newLength)

```

### 객체 변경 감지에 관한 주의사항

- Vue는 속성 추가 및 삭제를 감지하지 못함

```js
var vm = new Vue({
  data: {
    a: 1
  }
})
// `vm.a` 는 반응형입니다.

vm.b = 2
// `vm.b` 는 반응형이 아닙니다.
```

#### 중첩된 객체에 반응형 속성 추가 가능

```js
var vm = new Vue({
  data: {
    userProfile: {
      name: 'Anika'
    }
  }
})

// 1안 ////
Vue.set(vm.userProfile, 'age', 27);
// 2안 ////
vm.$set(vm.userProfile, 'age', 27);
// 3안 ////
Object.assign(vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
// 4안 ////
vm.userProfile = Object.assign({}, vm.userProfile, {
  age: 27,
  favoriteColor: 'Vue Green'
})
```

### 필터링/정렬

```html
<li v-for="n in evenNumbers">{{ n }}</li>
```

```json
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
computed: {
  evenNumbers: function () {
    return this.numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

```html
<li v-for="n in even(numbers)">{{ n }}</li>
```

```json
data: {
  numbers: [ 1, 2, 3, 4, 5 ]
},
methods: {
  even: function (numbers) {
    return numbers.filter(function (number) {
      return number % 2 === 0
    })
  }
}
```

### Range v-for

```html
<div>
  <span v-for="n in 10">{{ n }} </span>
</div>
```

### v-for 템플릿

```html
<ul>
  <template v-for="item in items">
    <li>{{ item.msg }}</li>
    <li class="divider" role="presentation"></li>
  </template>
</ul>
```

### v-for와 v-if

- `v-for`가 `v-if`보다 높은 우선순위

```html
<li v-for="todo in todos" v-if="!todo.isComplete">
  {{ todo }}
</li>
```

```html
<ul v-if="todos.length">
  <li v-for="todo in todos">
    {{ todo }}
  </li>
</ul>
<p v-else>No todos left!</p>
```

### v-for와 컴포넌트

```html
<my-component v-for="item in items" :key="item.id"></my-component>
```

```html
<my-component
  v-for="(item, index) in items"
  v-bind:item="item"
  v-bind:index="index"
  v-bind:key="item.id"
></my-component>
```

### 할일 목록

```html
<div id="todo-list-example">
  <form v-on:submit.prevent="addNewTodo">
    <label for="new-todo">Add a todo</label>
    <input
      v-model="newTodoText"
      id="new-todo"
      placeholder="E.g. Feed the cat"
    >
    <button>Add</button>
  </form>
  <ul>
    <li
      is="todo-item"
      v-for="(todo, index) in todos"
      v-bind:key="todo.id"
      v-bind:title="todo.title"
      v-on:remove="todos.splice(index, 1)"
    ></li>
  </ul>
</div>
```

```js
Vue.component('todo-item', {
  template: '\
    <li>\
      {{ title }}\
      <button v-on:click="$emit(\'remove\')">Remove</button>\
    </li>\
  ',
  props: ['title']
})

new Vue({
  el: '#todo-list-example',
  data: {
    newTodoText: '',
    todos: [
      {
        id: 1,
        title: 'Do the dishes',
      },
      {
        id: 2,
        title: 'Take out the trash',
      },
      {
        id: 3,
        title: 'Mow the lawn'
      }
    ],
    nextTodoId: 4
  },
  methods: {
    addNewTodo: function () {
      this.todos.push({
        id: this.nextTodoId++,
        title: this.newTodoText
      })
      this.newTodoText = ''
    }
  }
})
```

## v-on(이벤트 핸들링)

```html
<div id="example-1">
  <button v-on:click="counter += 1">Add 1</button>
  <p>위 버튼을 클릭한 횟수는 {{ counter }} 번 입니다.</p>
</div>
```

```js
var example1 = new Vue({
  el: '#example-1',
  data: {
    counter: 0
  }
})
```

### 메소드 이벤트 핸들러

```html
<div id="example-2">
  <!-- `greet`는 메소드 이름으로 아래에 정의되어 있습니다 -->
  <button v-on:click="greet">Greet</button>
</div>
```

```js
var example2 = new Vue({
  el: '#example-2',
  data: {
    name: 'Vue.js'
  },
  // 메소드는 `methods` 객체 안에 정의합니다
  methods: {
    greet: function (event) {
      // 메소드 안에서 사용하는 `this` 는 Vue 인스턴스를 가리킵니다
      alert('Hello ' + this.name + '!')
      // `event` 는 네이티브 DOM 이벤트입니다
      if (event) {
        alert(event.target.tagName)
      }
    }
  }
})

// 또한 JavaScript를 이용해서 메소드를 호출할 수 있습니다.
example2.greet() // => 'Hello Vue.js!'
```

### 인라인 메소드 핸들러

```html
<div id="example-3">
  <button v-on:click="say('hi')">Say hi</button>
  <button v-on:click="say('what')">Say what</button>
</div>
```

```js
new Vue({
  el: '#example-3',
  methods: {
    say: function (message) {
      alert(message)
    }
  }
})
```

### $event변수를 사용해 메소드에 전달

```html
<button v-on:click="warn('Form cannot be submitted yet.', $event)">
  Submit
</button>
```

```js
// ...
methods: {
  warn: function (message, event) {
    // 이제 네이티브 이벤트에 액세스 할 수 있습니다
    if (event) event.preventDefault()
    alert(message)
  }
}
```

### 이벤트 수식어

- `.stop`
- `.prevent`
- `.capture`
- `.self`
- `.once`
- `.passive`

```html
<!-- 클릭 이벤트 전파가 중단됩니다 -->
<a v-on:click.stop="doThis"></a>

<!-- 제출 이벤트가 페이지를 다시 로드 하지 않습니다 -->
<form v-on:submit.prevent="onSubmit"></form>

<!-- 수식어는 체이닝 가능합니다 -->
<a v-on:click.stop.prevent="doThat"></a>

<!-- 단순히 수식어만 사용할 수 있습니다 -->
<form v-on:submit.prevent></form>

<!-- 이벤트 리스너를 추가할 때 캡처모드를 사용합니다 -->
<!-- 즉, 내부 엘리먼트를 대상으로 하는 이벤트가 해당 엘리먼트에서 처리되기 전에 여기서 처리합니다. -->
<div v-on:click.capture="doThis">...</div>


<!-- event.target이 엘리먼트 자체인 경우에만 트리거를 처리합니다 -->
<!-- 자식 엘리먼트에서는 안됩니다 -->
<div v-on:click.self="doThat">...</div>
```

- `v-on:click.prevent.self`를 사용하면 **모든 클릭**을 방지
- `v-on:click.self.prevent`는 엘리먼트 자체에 대한 클릭만 방지

```html
<!-- 클릭 이벤트는 최대 한번만 트리거 됩니다. -->
<a v-on:click.once="doThis"></a>

<!-- 스크롤의 기본 이벤트를 취소할 수 없습니다. -->
<div v-on:scroll.passive="onScroll">...</div>
```

### 키 수식어

- .enter, .tab,.delete (“Delete” 와 “Backspace” 키 모두를 캡처합니다), .esc, .space, .up, .down, .left, .right

```html
<!-- only call `vm.submit()` when the `key` is `Enter` -->
<input v-on:keyup.enter="submit">

<input v-on:keyup.page-down="onPageDown">

<input v-on:keyup.13="submit">
```

```js
// `v-on:keyup.f1`을 사용할 수 있습니다.
Vue.config.keyCodes.f1 = 112
```

### 시스템 수식어

- .ctrl, .alt, .shift, .meta
- 매킨토시 키보드에서 meta는 command 키(⌘)
- Windows 키보드에서 meta는 windows 키(⊞)
- Sun Microsystems 키보드에서 meta는 단색의 다이아몬드 (◆)로 표시
- 특정 키보드의 경우, 특히 MIT 및 Lisp 시스템 키보드와 Knight 키보드, space-cadet 키보드와 같은 제품에는 “META” 레이블이 지정
- Symbolics 키보드에서 메타는 “META” 또는 “Meta”로 표시
- 수식어 키는 일반 키와 다르며 `keyup` 이벤트와 함께 사용되면 이벤트가 발생할 때 수식어 키가 눌려있어야 함(`keyup.ctrl`는 `ctrl`을 누른 상태에서 키를 놓으면 트리거)
- `ctrl` 키만 놓으면 트리거되지 않음

```html
<!-- Alt + C -->
<input @keyup.alt.67="clear">

<!-- Ctrl + Click -->
<div @click.ctrl="doSomething">Do something</div>
```

#### `.exact` 수식어

```html
<!-- Alt 또는 Shift와 함께 눌린 경우에도 실행됩니다. -->
<button @click.ctrl="onClick">A</button>

<!-- Ctrl 키만 눌려있을 때만 실행됩니다. -->
<button @click.ctrl.exact="onCtrlClick">A</button>

<!-- 아래 코드는 시스템 키가 눌리지 않은 상태인 경우에만 작동합니다. -->
<button @click.exact="onClick">A</button>
```

#### 마우스 수식어

- .left, .right, .middle

