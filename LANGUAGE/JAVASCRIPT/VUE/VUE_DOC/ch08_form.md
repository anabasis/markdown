# 폼 입력 바인딩

- `v-model`은 모든 form 엘리먼트의 초기 `value`와 `checked` 그리고 `selected` 속성을 무시
- 항상 Vue 인스턴스 데이터를 원본 소스로 취급
- 컴포넌트의 `data` 옵션 안에 있는 JavaScript에서 초기값을 선언

## 기본

### 문자열

```html
<input v-model="message" placeholder="여기를 수정해보세요">
<p>메시지: {{ message }}</p>
```

### 여러줄 가진 문장

- 텍스트 영역의 보간 `(<textarea>{{ text }}</textarea>)`은 작동하지 않음. 대신 v-model를 사용

```html
<span>여러 줄을 가지는 메시지:</span>
<p style="white-space: pre-line">{{ message }}</p>
<br>
<textarea v-model="message" placeholder="여러줄을 입력해보세요"></textarea>
```

### 체크박스

```html
<input type="checkbox" id="checkbox" v-model="checked">
<label for="checkbox">{{ checked }}</label>

<div id='example-3'>
  <input type="checkbox" id="jack" value="Jack" v-model="checkedNames">
  <label for="jack">Jack</label>
  <input type="checkbox" id="john" value="John" v-model="checkedNames">
  <label for="john">John</label>
  <input type="checkbox" id="mike" value="Mike" v-model="checkedNames">
  <label for="mike">Mike</label>
  <br>
  <span>체크한 이름: {{ checkedNames }}</span>
</div>
```

```js
new Vue({
  el: '#example-3',
  data: {
    checkedNames: []
  }
})
```

### 라디오

```html
<input type="radio" id="one" value="One" v-model="picked">
<label for="one">One</label>
<br>
<input type="radio" id="two" value="Two" v-model="picked">
<label for="two">Two</label>
<br>
<span>선택: {{ picked }}</span>
```

### 셀렉트

```html
<select v-model="selected">
  <option disabled value="">Please select one</option>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<span>선택함: {{ selected }}</span>
```

```js
new Vue({
  el: '...',
  data: {
    selected: ''
  }
})
```

#### 다중 셀렉트

```html
<select v-model="selected" multiple>
  <option>A</option>
  <option>B</option>
  <option>C</option>
</select>
<br>
<span>Selected: {{ selected }}</span>
```

#### v-for를 이용한 동적 옵션 렌더링

```html
<select v-model="selected">
  <option v-for="option in options" v-bind:value="option.value">
    {{ option.text }}
  </option>
</select>
<span>Selected: {{ selected }}</span>
```

```js
new Vue({
  el: '...',
  data: {
    selected: 'A',
    options: [
      { text: 'One', value: 'A' },
      { text: 'Two', value: 'B' },
      { text: 'Three', value: 'C' }
    ]
  }
})
```

## 값 바인딩

```html
<!-- `picked` 는 선택시 문자열 "a" 입니다 -->
<input type="radio" v-model="picked" value="a">

<!-- `toggle` 는 true 또는 false 입니다 -->
<input type="checkbox" v-model="toggle">

<!-- `selected`는 "ABC" 선택시 "abc" 입니다 -->
<select v-model="selected">
  <option value="abc">ABC</option>
</select>
```

### 체크박스

- `true-value` 와 `false-value` 속성은 폼 전송시 체크되지 않은 박스를 포함하지 않기 때문에 입력의 `value` 속성에 영향을 미치지 않음
- 두 값 중 하나가 폼을 통해 전송 되려면 (예 : '예' 또는 '아니요') 라디오를 대신 사용

```html
<input
  type="checkbox"
  v-model="toggle"
  true-value="yes"
  false-value="no"
>
```

```js
// 체크된 경우
vm.toggle === 'yes'
// 체크 되지 않은 경우
vm.toggle === 'no'
```

### 라디오

```html
<input type="radio" v-model="pick" v-bind:value="a">
```

```js
// 체크 하면:
vm.pick === vm.a
```

### 셀렉트 옵션

```html
<select v-model="selected">
  <!-- inline object literal -->
  <option v-bind:value="{ number: 123 }">123</option>
</select>
```

```js
// 선택 하면:
typeof vm.selected // -> 'object'
vm.selected.number // -> 123
```

## 수식어

### `.lazy`

- v-model은 각 입력 이벤트 후 입력과 데이터를 동기화(단 앞에서 설명한 IME 구성은 제외)
- `.lazy` 수식어를 추가하여 change 이벤트 이후에 동기화

```html
<!-- "input" 대신 "change" 이후에 동기화 됩니다. -->
<input v-model.lazy="msg" >
```

### `.number`

- 사용자 입력이 자동으로 숫자로 형변환 되기를 원하면, v-model이 관리하는 input에 number 수식어를 추가

```html
<input v-model.number="age" type="number">
```

### `.trim`

- `v-model`이 관리하는 input을 자동으로 trim 하기 원하면, trim 수정자를 추가

```html
<input v-model.trim="msg">
```

## `v-model`과 컴포넌트
