# 클래스와 CSS바인딩

## HTML 클래스 바인딩(class)

### 객체 구문

```html
<div v-bind:class="{ active: isActive }"></div>
<div
  class="static"
  v-bind:class="{ active: isActive, 'text-danger': hasError }"
></div>
```

```json
data: {
  isActive: true,
  hasError: false
}
```

- `<div class="static active"></div>`
- isActive 또는 hasError 가 변경되면 클래스 목록도 그에 따라 업데이트
- `<div class="static active text-danger"></div>`

#### 인라인이 아닌 경우

```html
<div v-bind:class="classObject"></div>
```

```json
data: {
  classObject: {
    active: true,
    'text-danger': false
  }
}
```

#### computed 속성에 바인딩

```html
<div v-bind:class="classObject"></div>
```

```json
data: {
  isActive: true,
  error: null
},
computed: {
  classObject: function () {
    return {
      active: this.isActive && !this.error,
      'text-danger': this.error && this.error.type === 'fatal'
    }
  }
}
```

### 배열 구문

```html
<div v-bind:class="[activeClass, errorClass]"></div>
```

```json
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

- `<div class="active text-danger"></div>`

```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div>
```

```json
data: {
  activeClass: 'active',
  errorClass: 'text-danger'
}
```

- `<div class="active text-danger"></div>`

```html
<div v-bind:class="[isActive ? activeClass : '', errorClass]"></div> <!-- 삼항연산자 -->
<div v-bind:class="[{ active: isActive }, errorClass]"></div> <!-- 객체구문 -->
```

#### 컴포넌트와 함께 사용하는 방법

```html
<my-component class="baz boo"></my-component>
```

```js
Vue.component('my-component', {
  template: '<p class="foo bar">Hi</p>'
})
```

- `<p class="foo bar baz boo">Hi</p>`

```html
<my-component v-bind:class="{ active: isActive }"></my-component>
```

- `<p class="foo bar active">Hi</p>`

## 인라인 CSS 바인딩(style)

### 객체 구문

```html
<div v-bind:style="{ color: activeColor, fontSize: fontSize + 'px' }"></div>
```

```json
data: {
  activeColor: 'red',
  fontSize: 30
}
```

- 직접바인딩

```html
<div v-bind:style="styleObject"></div>
```

```json
data: {
  styleObject: {
    color: 'red',
    fontSize: '13px'
  }
}
```

### 배열 구문

```html
<div v-bind:style="[baseStyles, overridingStyles]"></div>
```

### 자동접두사

- `v-bind:style` 에 브라우저 벤더 접두어가 필요한 CSS 속성 (예: transform)을 사용하면 Vue는 자동으로 해당 접두어를 감지하여 스타일을 적용

### 다중값 제공

```html
<div v-bind:style="{ display: ['-webkit-box', '-ms-flexbox', 'flex'] }"></div>
```
