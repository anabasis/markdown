# Props

Props
이 페이지는 여러분이 이미 컴포넌트 기초를 읽었다고 가정하고 쓴 내용입니다. 컴포넌트가 처음이라면 기초 문서를 먼저 읽으시기 바랍니다.

Prop 대소문자 구분 (camelCase vs kebab-case)
HTML 어트리뷰트는 대소문자 구분이 없기 때문에 브라우저는 대문자를 소문자로 변경하여 읽습니다. 그렇기 때문에 카멜 케이스(대소문자 혼용)로 prop의 이름을 정한 경우에 DOM 템플릿 안에서는 케밥 케이스(하이픈으로-연결된-구조)를 사용하여야 올바르게 동작합니다.

Vue.component('blog-post', {
  // JavaScript에서의 camelCase
  props: ['postTitle'],
  template: '<h3>{{ postTitle }}</h3>'
})
<!-- HTML에서의 kebab-case -->
<blog-post post-title="hello!"></blog-post>
물론, 문자열 템플릿을 사용하는 경우에는 무관합니다.

Prop 타입
이때까지 우리는 문자열 배열 형태로 작성된 prop의 리스트를 보아 왔습니다.

props: ['title', 'likes', 'isPublished', 'commentIds', 'author']
일반적으로 생각해 보면, prop에 특정 타입의 값을 넣고싶은 경우가 있을 수 있습니다. 이 때, 다음과 같이 prop을 속성 이름과 타입을 포함하는 오브젝트로 선언함으로써 타입이 지정된 prop의 리스트를 구현할 수 있습니다.

props: {
  title: String,
  likes: Number,
  isPublished: Boolean,
  commentIds: Array,
  author: Object,
  callback: Function,
  contactsPromise: Promise // or any other constructor
}
이는 컴포넌트를 읽기 좋게 문서화할 뿐 아니라 브라우저의 자바스크립트 콘솔에서도 잘못된 타입이 전달된 경우 경고를 띄워줄 수 있도록 해줍니다. 페이지 아래에 있는 타입 체크와 prop 유효성 검사 에서 조금 더 자세한 내용을 확인할 수 있습니다.

정적 & 동적 prop 전달하기
아래와 같이 정적인 prop을 전달할 수 있습니다.

<blog-post title="My journey with Vue"></blog-post>
혹은 아래와 같이 v-bind 를 이용하여 동적인 prop을 전달할 수도 있습니다.

<!-- 변수에 담긴 값을 동적으로 할당 -->
<blog-post v-bind:title="post.title"></blog-post>

<!-- 복잡한 표현식의 값을 동적으로 할당 -->
<blog-post
  v-bind:title="post.title + ' by ' + post.author.name"
></blog-post>
위 두 가지 경우는 모두 문자열 형태의 변수를 전달하지만 실제로는 모든 타입의 변수가 prop으로 전달될 수 있습니다.

숫자형(Number) 전달
<!-- `42`는 정적인 값이지만, Vue에서 해당 값이 숫자라는 것을 알 수 있도록 하기 위해 -->
<!-- v-bind를 이용해 문자열이 아닌 JavaScript 표현식이라는 것을 알려줍니다.      -->
<blog-post v-bind:likes="42"></blog-post>

<!-- 변수의 값을 동적으로 할당할 수도 있습니다. -->
<blog-post v-bind:likes="post.likes"></blog-post>
논리 자료형(Boolean) 전달
<!-- 값이 없는 prop은 `true` 를 전달합니다.. -->
<blog-post is-published></blog-post>

<!-- `false`는 정적인 값이지만, Vue에서 해당 값이 논리 자료형이라는 것을 알 수 있도록 하기 위해 -->
<!-- v-bind를 이용해 문자열이 아닌 JavaScript 표현식이라는 것을 알려줍니다.                -->
<blog-post v-bind:is-published="false"></blog-post>

<!-- 변수의 값을 동적으로 할당할 수도 있습니다. -->
<blog-post v-bind:is-published="post.isPublished"></blog-post>
배열(Array) 전달
<!-- 해당 배열은 정적인 값이지만, Vue에서 해당 값이 배열이라는 것을 알 수 있도록 하기 위해 -->
<!-- v-bind를 이용해 문자열이 아닌 JavaScript 표현식이라는 것을 알려줍니다.          -->
<blog-post v-bind:comment-ids="[234, 266, 273]"></blog-post>

<!-- 변수의 값을 동적으로 할당할 수도 있습니다. -->
<blog-post v-bind:comment-ids="post.commentIds"></blog-post>
오브젝트(Object) 전달
<!-- 해당 오브젝트는 정적인 값이지만, Vue에서 해당 값이 배열이라는 것을 알 수 있도록 하기 위해 -->
<!-- v-bind를 이용해 문자열이 아닌 JavaScript 표현식이라는 것을 알려줍니다.             -->
<blog-post
  v-bind:author="{
    name: 'Veronica',
    company: 'Veridian Dynamics'
  }"
></blog-post>

<!-- 변수의 값을 동적으로 할당할 수도 있습니다. -->
<blog-post v-bind:author="post.author"></blog-post>
오브젝트의 속성(Properties) 전달
오브젝트의 모든 속성을 전달하길 원하는 경우, v-bind:prop-name 대신 v-bind 만 작성함으로써 모든 속성을 prop으로 전달할 수 있습니다. 예를 들어, 아래와 같은 post 라는 오브젝트가 있다면:

post: {
  id: 1,
  title: 'My Journey with Vue'
}
아래와 같이 작성하는 것은

<blog-post v-bind="post"></blog-post>
아래와 같이 동작합니다.

<blog-post
  v-bind:id="post.id"
  v-bind:title="post.title"
></blog-post>
단방향 데이터 흐름
모든 prop들은 부모와 자식 사이에 단방향으로 내려가는 바인딩 형태를 취합니다: 부모의 속성이 변경되면 자식 속성에게 전달되지만, 반대 방향으로는 전달되지 않습니다. 자식의 데이터가 부모에게 전달되는 것을 막는 것은 자식 요소가 의도치 않게 부모 요소의 상태를 변경함으로써 앱의 데이터 흐름을 이해하기 어렵게 만드는 일을 막기 위해서입니다.

또한, 부모 컴포넌트가 업데이트될 때 마다 자식 요소의 모든 prop들이 최신 값으로 새로고침됩니다. 이는 곧 사용자가 prop을 자식 컴포넌트 안에서 수정해서는 안된다는 것이기도 합니다. 만약 수정을 시도하는 경우, Vue는 콘솔에 경고를 표시합니다.

아래 두 경우가 주로 prop을 직접 변경하고 싶을 수 있는 상황의 예시입니다:

prop은 초기값만 전달하고, 자식 컴포넌트는 그 초기값을 로컬 데이터 속성으로 활용하고 싶은 경우
해당 경우에는 로컬 데이터 속성을 따로 선언하고 그 속성의 초기값으로써 prop을 사용하는 것이 가장 바람직합니다.

props: ['initialCounter'],
data: function () {
  return {
    counter: this.initialCounter
  }
}
전달된 prop의 형태를 바꾸어야 하는 경우
해당 경우에는 computed 속성을 사용하는 것이 가장 바람직합니다.

props: ['size'],
computed: {
  normalizedSize: function () {
    return this.size.trim().toLowerCase()
  }
}
자바스크립트 오브젝트나 배열을 prop으로 전달하는 경우, 객체를 복사하는 것이 아니라 참조하게 됩니다. 즉, 전달받은 오브젝트나 배열를 수정하게 되는 경우, 자식 요소가 부모 요소의 상태를 **변경하게 될 것**입니다.

Prop 유효성 검사
앞에서 보신 것 처럼, 컴포넌트는 prop의 유효성 검사를 위해 요구사항을 특정할 수 있습니다. 요구사항이 충족되지 않은 경우 Vue는 브라우저의 자바스크립트 콘솔을 통해 경고를 표시합니다. 이는 다른 사람들도 사용하는 컴포넌트를 개발하는 경우에 특히 유용합니다.

Prop들의 유효성 검사를 위해서 prop 의 값에 배열이나 문자열 대신 오브젝트를 삽입할 수 있습니다. 예를 들어:

Vue.component('my-component', {
  props: {
    // 기본 타입 체크 (`Null`이나 `undefinded`는 모든 타입을 허용합니다.)
    propA: Number,
    // 여러 타입 허용
    propB: [String, Number],
    // 필수 문자열
    propC: {
      type: String,
      required: true
    },
    // 기본값이 있는 숫자
    propD: {
      type: Number,
      default: 100
    },
    // 기본값이 있는 오브젝트
    propE: {
      type: Object,
      // 오브젝트나 배열은 꼭 기본값을 반환하는
      // 팩토리 함수의 형태로 사용되어야 합니다. 
      default: function () {
        return { message: 'hello' }
      }
    },
    // 커스텀 유효성 검사 함수
    propF: {
      validator: function (value) {
        // 값이 항상 아래 세 개의 문자열 중 하나여야 합니다. 
        return ['success', 'warning', 'danger'].indexOf(value) !== -1
      }
    }
  }
})
Prop 유효성 검사가 실패하는 경우, Vue는 콘솔에 주의 메세지를 띄웁니다. (개발용 빌드를 사용중인 경우에)

Prop의 유효성 검사는 컴포넌트 인스턴스가 생성되기 전에 일어난다는 것을 기억하세요. 즉, 인스턴스의 값(e.g. `data`, `computed`, 등등)은 `default`나 `validator` 함수 안에 사용될 수 없습니다.

Type Checks
type 은 아래에 있는 네이티브 생성자중 하나가 될 수 있습니다.

String
Number
Boolean
Array
Object
Date
Function
Symbol
또한, type 에는 커스텀 생성자가 사용될 수도 있습니다. 확인은 instanceof 를 통해 이루어집니다. 예를 들어, 아래와 같은 생성자 함수가 선언되어 있다면:

function Person (firstName, lastName) {
  this.firstName = firstName
  this.lastName = lastName
}
아래와 같이 작성함으로써:

Vue.component('blog-post', {
  props: {
    author: Person
  }
})
author prop이 new Person으로 생성된 값인지 확인할 수 있습니다.

Prop이 아닌 속성
Prop이 아닌 속성은 컴포넌트에 전달되긴 하지만 대응되는 prop이 정의되지 않습니다.

명확하게 정의된 prop을 통해서 자식 컴포넌트에 정보를 전달하는 것이 권장되긴 하지만, 컴포넌트 라이브러리를 만드는 등의 경우 어떤 맥락에서 해당 컴포넌트가 사용될지를 확실히 상정할 수 없는 경우가 있습니다. 이러한 경우에 대응하기 위해서 컴포넌트는 임의의 속성값을 받아와 컴포넌트의 루트 엘리먼트에 추가해 줄 수 있습니다.

예를 들어, data-date-picker 속성이 필요한 부트스트랩의 서드파티 컴포넌트인 bootstrap-date-input 을 사용하는 상황을 가정해 봅시다. 이 경우, 아래와 같이 컴포넌트 인스턴스에 속성을 추가해 줄 수 있습니다.

<bootstrap-date-input data-date-picker="activated"></bootstrap-date-input>
data-date-picker="activated"속성은 bootstrap-date-input의 루트 엘리먼트에 자동으로 추가됩니다.

기존 속성의 대체 및 병합
아래 코드가 bootstrap-date-input의 템플릿이라고 생각해 봅시다:

<input type="date" class="form-control">
날짜 선택 플러그인의 테마를 설정하기 위해서는 아래와 같이 특정 클래스를 작성해 주어야 합니다.

<bootstrap-date-input
  data-date-picker="activated"
  class="date-picker-theme-dark"
></bootstrap-date-input>
이 경우, 두 개의 각각 다른 값이 class에 선언됩니다.

form-control: 컴포넌트 템플릿으로부터 부여됩니다.

date-picker-theme-dark: 컴포넌트의 부모로부터 전달받아 부여됩니다.

대부분 속성의 경우, 전달받은 속성이 기존에 선언된 속성을 대체합니다. 예를 들어, type="text"를 type="date"가 선언된 컴포넌트에 전달하는 경우에는 속성이 대체되고 문제를 일으키게 될 가능성이 생깁니다. 하지만 다행히도 class와 style 속성의 경우에는 조금 더 똑똑하게 반응합니다. 즉, 앞의 form-control과 date-picker-theme-dark의 예제와 같이 두 개의 값이 합쳐져서 적용됩니다.

속성 상속 비활성화
컴포넌트의 루트 엘리먼트가 상속된 속성을 갖지 않기를 원하는 경우, 컴포넌트에 inheritAttrs: false 옵션을 줄 수 있습니다. 예를 들어:

Vue.component('my-component', {
  inheritAttrs: false,
  // ...
})
이는 컴포넌트에 전달된 속성의 이름과 값을 가지고 있는 인스턴스 속성인 $attrs와 조합하면 유용하게 사용할 수 있습니다.

{
  required: true,
  placeholder: 'Enter your username'
}
inheritAttrs: false 와 $attrs를 이용하면 수동으로 전달할 속성을 선택할 수 있으며, 이는 스타일 규칙 중 base components 를 지키는 방법으로써 꽤 바람직합니다.

Vue.component('base-input', {
  inheritAttrs: false,
  props: ['label', 'value'],
  template: `
    <label>
      {{ label }}
      <input
        v-bind="$attrs"
        v-bind:value="value"
        v-on:input="$emit('input', $event.target.value)"
      >
    </label>
  `
})
`inheritAttrs: false` 옵션은 `style`과 `class`의 바인딩에 영향을 주지 **않는다는** 것을 기억하세요!

이러한 패턴을 이용하면 기본 컴포넌트의 실제 루트를 크게 신경쓰지 않고도 기본 HTML 엘리먼트에 가깝게 사용할 수 있습니다.

<base-input
  v-model="username"
  required
  placeholder="Enter your username"
></base-input>