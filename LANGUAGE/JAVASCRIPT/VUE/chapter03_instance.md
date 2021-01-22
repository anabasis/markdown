# Vue Instance

## 형식

### 기본형식

```js
var vm = new Vue({
  // 옵션
})
```

### 대이터객체 형식

```js
// 데이터 객체
var data = { a: 1 }

// Vue인스턴스에 데이터 객체를 추가합니다.
var vm = new Vue({
  data: data
})

// 인스턴스에 있는 속성은
// 원본 데이터에 있는 값을 반환합니다.
vm.a === data.a // => true

// 인스턴스에 있는 속성값을 변경하면
// 원본 데이터에도 영향을 미칩니다.
vm.a = 2
data.a // => 2

// ... 반대로 마찬가지입니다.
data.a = 3
vm.a // => 3
```

## 컴포넌트 설계

```bash
Root Instance
└─ TodoList
   ├─ TodoItem
   │  ├─ DeleteTodoButton
   │  └─ EditTodoButton
   └─ TodoListFooter
      ├─ ClearTodosButton
      └─ TodoListStatistics
```

