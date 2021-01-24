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
