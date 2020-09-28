# Export&Import

<https://beomy.tistory.com/22>

## export

- 파일이나 모듈안의 함수나, 객체를 export 하는데 사용
- ES6에서 정의된 API로, 브라우저에서 기본으로 제공되지 않기 때문에, Traceurur Complier, Babel, Rollup, Webpack과 같은 별로의 트랜스파일러와 함께 사용되어야 함

### 1. 문법

```js
export { name1, name2, ..., nameN };
export { variable1 as name1, variable2 as name2, ..., nameN };
export let name1, name2, ..., nameN; // 또는 var
export let name1 = ..., name2 = ..., ..., nameN; // 또는 var, const

export expression;
export dafault expression;
export default function (...) { ... } // 또는 class, function*
export default function name1(...) { ... } // 또는 class, function*
export { name1 as default, ... };

export * from ...;
export { name1, name2, ..., nameN } from ...;
export { import1 as name1, import2 as name2, ..., nameN } from ...;
```

nameN : export된 식별자, 다른 스크립트에서 이 식별자를 통해 import 가능

### 2. 설명

export에는 Named exports와 Default exports 두가지 방법

#### Named exports

```js
export { myFunction };
export const foo = Math.sqrt(2);
```

- Named exports는 여러값을 export 하는데 유용
- export 된 이름을 사용하여 import 하여 사용 가능

#### Default exports

```js
export default myFunctionOrClass // 여기에는 세미콜론이 없음
```

- 모듈 당 딱 한 개의 default export만 있어야 함
- default export로 객체, 함수 클래스 등이 될 수 있음
- 가장 간단하게 export
- 딱 한개만 default export를 할 수 있기 때문에, "메인" 이라고 할 수 있는 것을 default export 
- 단일 값이나 모듈에 대한 대체 값을 export 하는데 default export를 사용

### 3. 예제

#### Named exports

- 아래와 같이 export 가능

```js
// module "my-module.js"
function cube(x) {
    return x * x * x;
}
const foo = Math.PI + Math.SQRT2;
export { cube, foo };
```

- 위의 export된 값들을 import하여 사용할 때 아래와 같이 사용 가능

```js
import { cube, foo } from 'my-module';
console.log(cube(3)); // 27
console.log(foo);    // 4.555806215962888
```

#### Default exports

아래와 같이 Default export 가능

```js
// module "my-module.js"
let cube = function cube(x) {
    return x * x * x;
}
export default cube;
```

- default exports된 값을 import 하는 방법

```js
// module "my-module.js"
import myFunction from 'my-module';
console.log(myFunction(3)); // 27
```

## import

- 외부 스크립트 또는 외부 모듈의 export된 함수, 객체를 가져오는데 사용
- export와 마찬가지로, ES6에서 정의된 API
- import는 모든 브라우저에서 기본으로 제공되지 있지 않기 때문에, Traceur Compiler, Babel, Rollup, Webpack과 같은 별도의 트랜스파일러와 함께 사용되어야 함

### 1. 문법

```js
import name form "module-name";
import * as name from "module-name";
import { member } from "module-name";
import { member as alias } from "module-name";
import { member1, member2 } from "module-name";
import { member1, member2 as alias2, [...] } from "module-name";
import defaultMember, { member [, [...]] } from "module-name";
import defaultMember, * as alias from "module-name";
import defaultMember from "module-name";
import "module-name";
```

- name : 가져온 값을 받을 객체 이름.
- member, memberN : export 된 모듈에서 멤버의 이름
- defaultMember : export 된 모듈의 default 이름
- alias, aliasN : export된 멤버의 이름을 지정한 이름
- module-name : 가져올 모듈 이름 (파일명)

### 2. 설명

- name 파라미터는 export 되는 모든 멤버를 저장 할 객체의 이름
- member 파라미터는 각각의 멤버를 지정하지만, name 파라미터는 name이라는 이름의 객체에 모든 export된 값을 가져옮

### 기본구문

#### 모듈 전체 가져오기

- export 된 모든 것들을 현재 범위(scope) 내에 myModule로 바인딩 됨(scope 관련 내용은 [자바스크립트] 유효범위(Scope)와 호이스팅(Hoisting) 참고)

```js
import * as myModule from "my-module.js";
```

#### 하나의 멤버 가져오기

- 현재 범위 내의 myMember를 import

```js
import {myMember} from "my-module.js";
```

#### 여러개의 멤버 가져오기

- 현재 범위 내의 foo와 bar를 import

```js
import {foo,bar} from"my-module.js";
```

#### 다른 이름으로 멤버 가져오기

- 멤버를 가져올 때 다른 이름으로 멤버를 지정 가능.
- export 된 멤버 이름이 길거나, 복잡할 때, 임의의 이름으로 멤버를 지정 가능

```js
import {reallyReallyLongModuleMemberName as shortName} from "my-module.js";
```

- 여러개의 멤버를 다른 이름으로 지정하여 import 할 수도 있습니다.

```js
import {reallyReallyLongModuleMemberName as shortName, anotherLongModuleName as short} from "my-module.js";
```

#### 바인딩 없이 모듈만 실행하기

- 단순히 특정 모듈을 불러와 실행만 할 목적이라면, import만 사용

```js
import "my-module.js";
```

#### default export 값 가져오기

- default export를 통해 export 된 값들을 가져올 수 있음.

```js
import myModule from "my-module.js";
```

- 위에서 설명한 기본 구문과 함께 사용 가능.
- 기본 값(default export 된 값)을 가져오는 부분이 먼저 선언

```js
import myDefault, * as myModule from "my-module.js";
// 또는
import myDefault, {foo, bar} from "my-module.js";
```

#### 예제

```js
// --file.js--
function getJSON(url, callback) {
    let xhr = new XMLHttpRequest();
    xhr.onload = function () { 
        callback(this.responseText) 
    };
    xhr.open("GET", url, true);
    xhr.send();
}

export function getUsefulContents(url, callback) {
    getJSON(url, data => callback(JSON.parse(data)));
}

// --main.js--
import { getUsefulContents } from "file.js";
getUsefulContents("http://www.example.com", data => {
    doSomethingUseful(data);
});
```

## require vs import 차이

- 코드 가장 위에 패키지를 읽어들이는 놈이 require와 import

require : 자바스크립트 자체가 지원하는 패키지 읽는 방법(CommonJS)
import : ES6의 사양으로 새로운 패키지 읽는 방법(ES6)

### 불러오기

#### require

```js
var foo = require("foo");
var bar = requre("foo").bar;
```

#### import

```js
import foo from "foo";
import { bar } from "foo";
```

### 내보내기

#### module.exports

```js
module.exports = foo;
exports.bar = bar;
```

#### export

```js
export default foo;
export { bar };
```

### 예제 차이

- 웹팩의 tree shaking으로 인해 필요한 패키지만 읽어옮

```js
// file1.js
export {
  a: "a",
  b: "b",
  c: "c",
};
// file2.js
import { a } from './file1';
console.log(a);
```

- require을 사용하면 동적으로 모듈을 불러올 수 있지만, 불필요한 코드들까지 불러오기 때문에 실제로 어떤 코드가 사용되었는지 명확히 알 수가 없음

```js
import React from 'react';
import ReactDOM from 'react-dom';
import ReloadRoot from './components/ReloadRoot';
const render = () => {
  ReactDOM.render(
    <ReloadRoot />,
    document.getElementById('root')
  );
}
render();
if (module.hot) {
  module.hot.accept('./components/ReloadRoot', () => {
    require('./components/ReloadRoot').default;
    render();
  });
}
```
