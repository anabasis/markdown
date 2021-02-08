# 템플릿

## 보간법

### 문자열

```html
# Mustache 구분(이중 중괄호) 사용
<span>메시지: {{ msg }}</span>

# v-once 디렉티브를 사용하여 데이터 변경 시 업데이트 되지 않는 일회성 보간을 수행
# 같은 노드의 바인딩에도 영향을 미친다는 점을 유의
<span v-once>다시는 변경하지 않습니다: {{ msg }}</span>
```

### 원시 HTML

```html
# v-html 디렉티브 사용
<p>Using mustaches: {{ rawHtml }}</p>
<p>Using v-html directive: <span v-html="rawHtml"></span></p>


# 결과
Using mustaches: <span style="color:red">This should be red.</span>
Using v-html directive: This should be red.
```

### 속성

```html
# Mustache는 html 태그 속성으로 사용 안됨, v-bind 사용
<div v-bind:id="dynamicId"></div>

# 
<button v-bind:disabled="isButtonDisabled">Button</button>
```

### Javascript 표현식 사용

- 가능(하나의 단일 표현식만 포함)

```js
{{ number + 1 }}
{{ ok ? 'YES' : 'NO' }}
{{ message.split('').reverse().join('') }}
<div v-bind:id="'list-' + id"></div>
```

- 불가능

```js
<!-- 아래는 구문입니다, 표현식이 아닙니다. -->
{{ var a = 1 }}

<!-- 조건문은 작동하지 않습니다. 삼항 연산자를 사용해야 합니다. -->
{{ if (ok) { return message } }}
```

## 디렉티브

- `v-` 접두사
- 단일 Javascript 표현식(`v-for` 예제)
- `v-if` 디렉티브는 seen 표현의 진실성에 기반하여 `<p>` 엘리먼트를 제거 또는 삽입

```js
<p v-if="seen">이제 나를 볼 수 있어요</p>
```

### 전달인자

```js
<a v-bind:href="url"> ... </a>
<a :href="url"> ... </a>

<a v-on:click="doSomething"> ... </a>
<a @click="doSomething"> ... </a>
```

### 동적 전달인자

- 동적 전달인자는 "동적 전달인자의 형식 제약"의 부분에서 후술되는바와 같이, 조금의 제약이 있는 점에 주의

```js
<!-- href 라는 값을 가진 attributeName 데이터 속성을 가진 경우, 이 바인딩은 `v-bind:href`와 동등 -->
<a v-bind:[attributeName]="url"> ... </a>

<!-- eventName의 값이 "focus" 라고 한다면 v-on:[EventName]은 v-on:focus와 동등 -->
<a v-on:[eventName]="doSomething"> ... </a>
```

#### 동적 전달인자 값의 제약

- 동적 전달인자는, null을 제외하고는 string으로 변환될 것으로 예상
- 특수 값인 null은 명시적으로 바인딩을 제거하는데 사용
- 그 외의 경우, string이 아닌 값은 경고를 출력

#### 동적 전달인자 형식의 제약

```js
<!-- 컴파일러 경고, computed로 대체-->
<a v-bind:['foo' + bar]="value"> ... </a>

<!--
in-DOM 탬플릿에서는 이 부분이 v-bind:[someattr]로 변환됩니다.
인스턴스에 "someattr"속성이 없는 경우, 이 코드는 동작하지 않습니다.
-->
<a v-bind:[someAttr]="value"> ... </a>
```

### 수식어

- 수식어는 점으로 표시되는 특수 접미사(디렉티브를 특별한 방법으로 바인딩 해야 함을 나타냄)
- `.prevent` 수식어는 트리거된 이벤트에서 event.preventDefault()를 호출하도록 v-on 디렉티브에게 알려줌
