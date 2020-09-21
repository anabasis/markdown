# NodeJS

<http://pyrasis.com/nodejs/nodejs-HOWTO>
<https://opentutorials.org/module/938/7190>
<http://poiemaweb.com/nodejs-module>

## Nodejs 언어

### exports객체와 모듈의 기본

#### exports : 사용자 모듈 만들기(여러 속성과 메소드), require()

- 만들고자 하는 모듈을 파일로 만들고 exports 객체의 속성이나 메소드를 정의
- 만들어진 모듈을 전역 함수 require()를 이용하여 추출

```js
// cicle.js
var PI = Math.PI;

exports.area = function (r) {
    return PI * r * r;
};

exports.circumference = function (r) {
    return 2 * PI * r;
};

// foo.js
var circle = require('./circle.js');
console.log( 'The area of a circle of radius 4 is ' + circle.area(4));
```

```bash
## 실행 결과
$ node foo.js
The area of a circle of radius 4 is 50.26548245743669
```

- circle.js에서 area와 circumference를 exports 객체의 메소드로 정의
- foo.js에서 require() 함수를 통해 circle.js 파일을 불러와 결과 값을 변수 circle에 대입
- foo.js에서 변수 circle을 통해 circle.js의 exports 객체에 추가한 속성이나 메소드를 사용

#### module.exports : 사용자 모듈 만들기(하나의 속성이나 메소드)

<http://www.nodejs.org/api/globals.html#globals_module>

- exports는 속성이나 메소드를 여러 개 정의
- module.exports는 하나만 정의

```js
// square.js
module.exports = function(width) {
    return {
        area: function() {
            return width * width;
        }
    };
}

// bar.js
var square = require('./square.js');
var mySquare = square(2);
console.log('The area of my square is ' + mySquare.area());
```

```bash
## 실행 결과
$ node bar.js
The area of my square is 4
```

- square.js에서 module.exports를 함수로 정의
- bar.js에서 square.js 파일을 require()를 통해 변수로 불러와 모듈로 사용
- 함수 뿐 아니라 마찬가지로 속성도 지정

```js
//예제
// mymodule.js
module.exports = "사용자 모듈입니다.";

// mymain.js
var mymodule = require('./mymodule.js');
console.log(mymodule);
```

```bash
## 실행 결과
$ node mymain.js
```

#### index.js 파일

```js
var module = require('./mymodule');
```

- 먼저 mymodule.js 파일을 찾습니다. 있다면 그 파일을 추출
- mymodule.js 파일이 없다면 mymodule 이라는 폴더를 찾음
- 그 폴더의 index.js 파일을 찾아 추출

----------------------------------------------------------

### Node.js 모듈

#### 1. 모듈

<https://nodejs.org/api/modules.html#modules_modules>

- 브라우저 상에서 동작하는 JavaScript는 script tag로 로드하며 복수의 JavaScript 파일을 로드할 경우 하나의 파일로 merge되며 동일한 유효범위
- ES6에서는 Client-side JavaScript에서도 동작하는 모듈 기능을 추가
- 단 현재 대부분의 브라우저가 ES6의 모듈을 지원하지 않고 있으므로 ES6 모듈을 현재의 브라우저에서 사용하기 위해서는 SystemJS, RequireJS 등의 모듈 로더 또는 Webpack 등의 모듈번들러를 사용
- ES6 모듈은 키워드 export, import를 제공
- JavaScript를 Client-side에 국한하지 않고 범용적으로 사용 제안된 것이 CommonJS와 AMD(Asynchronous Module Definition)
- Node.js는 사실상 모듈 시스템의 사실상 표준(de facto standard)인 CommonJS를 채택하였고 현재는 독자적인 진화를 거쳐 CommonJS 사양과 100% 동일하지는 않지만 기본적으로 CommonJS 방식
- Node.js는 module 단위로 각 기능을 분할
- module은 파일과 1대1의 대응 관계를 가지며 하나의 모듈은 자신만의 독립적인 실행 영역(Scope)
- 클라이언트 사이드 JavaScript와는 달리 전역변수의 중복 문제가 발생하지 않음
- 모듈은 module.exports 또는 exports 객체를 통해 정의하고 외부로 공개
- 공개된 모듈은 require 함수를 사용하여 임포트

#### 2. exports

- 모듈은 독립적인 파일 스코프를 갖기 때문에 모듈 안에 선언한 모든 것들은 기본적으로 해당 모듈 내부에서만 참조 가능
- 모듈 안에 선언한 항목을 외부에 공개하여 다른 모듈들이 사용할 수 있게 하고 싶다면 exports 객체를 사용해야 한다.
- 모듈을 파일로 작성하고 외부에 공개할 대상을 exports 객체의 프로퍼티 또는 메소드를 정의
- 모듈을 전역 함수 require()를 이용하여 추출

```js
// circle.js
const { PI } = Math;
exports.area = (r) => PI * r * r;
exports.circumference = (r) => 2 * PI * r;
```

- circle.js는 독립적인 파일 스코프를 갖는 모듈
- circle 모듈에서 area와 circumference를 exports 객체의 메소드로 정의
- 변수 PI는 circle 모듈에서만 유효한 private 변수
- area와 circumference는 외부에 공개
- require 함수를 사용하여 임의의 이름으로 circle 모듈을 import
- 모듈의 확장자는 생략 가능

```js
// app.js
const circle = require('./circle.js'); // == require('./circle')
console.log(`지름이 4인 원의 면적: ${circle.area(4)}`);
console.log(`지름이 4인 원의 둘레: ${circle.circumference(4)}`);
```

```bash
## 실행
$ node app
지름이 4인 원의 면적: 50.26548245743669
지름이 4인 원의 둘레: 25.132741228718345
```

#### 3. module.exports

- exports 객체는 프로퍼티 또는 메소드를 여러 개 정의
- odule.exports에는 하나의 값(기본자료형, 함수, 객체)을 할당

```js
// circle.js
// circle 모듈의 module.exports에는 하나의 함수를 할당
const { PI } = Math;
module.exports = function (r) {
  return {
    area() { return PI * r * r; },
    circumference() { return 2 * PI * r}
  };

// app.js
const circle = require('./circle');
const myCircle = circle(4);
console.log(`지름이 4인 원의 면적: ${myCircle.area()}`);
console.log(`지름이 4인 원의 둘레: ${myCircle.circumference()}`);
```

- require 함수를 통해 circle 모듈을 임포트하여 circle 변수에 할당
- circle 변수는 circle 모듈에서 module.exports에 할당한 값 자체 즉 객체를 반환하는 함수

```js
// primitive.js
const pv = 'primitive value';
module.exports = pv;

// app.js
const value = require('./primitive');
console.log(value); // => 'primitive value'
```

- exports는 module.exports에의 참조이며 module.exports의 alias
- exports는 module.exports와 같다고 보아도 무방
- Node.js의 document에는 만약 exports와 module.exports의 관계가 어렵게 느껴진다면 exports를 무시하고 module.exports만을 사용하라고 권유

|구분|모듈 정의 방식|require 함수의 호출 결과|
|:--:|:--|:--|
|exports|exports 객체에는 값을 할당할 수 없고 공개할 대상을 exports 객체에 프로퍼티 또는 메소드로 추가|exports 객체에 추가한 프로퍼티와 메소드가 담긴 객체가 전달|
|module.exports|module.exports 객체에 하나의 값(기본자료형, 함수, 객체)만을 할당|module.exports 객체에 할당한 값이 전달|

##### 3.1 module.exports에 함수를 할당하는 방식

```js
// foo.js
module.exports = function(a, b) {
  return a + b;
};
// app.js
const add = require('./foo');
const result = add(1, 2);
console.log(result); // => 3
```

- module.exports는 1개의 값만을 할당(모듈에서 1개의 값만을 공개하는 것은 불편)
- 다음과 같이 객체를 사용하여 복수의 기능을 하나로 묶어 공개하는 방식을 사용

##### 3.2 exports에 객체를 할당하는 방식

```js
// foo.js
module.exports = {
  add (v1, v2) { return v1 + v2 },
  minus (v1, v2) { return v1 - v2 }
};

// app.js
const calc = require('./foo');
const result1 = calc.add(1, 2);
console.log(result1); // => 3
const result2 = calc.minus(1, 2);
console.log(result2); // => -1
```

#### 4. require

- require 함수의 인수에는 파일뿐만 아니라 디렉터리를 지정

project/
├── app.js
└── module/
    ├── index.js
    ├── calc.js
    └── print.js

```js
//모듈을 명시하지 않고 require 함수를 호출하면 해당 디렉터리의 index.js을 로드
const myModule = require('./module');
```

```js
// module/index.js
//로드되는 index.js 내에서 calc.js과 print.js를 require하면 한번의 require로 alc.js과 print.js의 모든 기능을 사용
module.exports = {
  calc: require('./calc'),
  print: require('./print')
};

// module/calc.js
module.exports = {
  add (v1, v2) { return v1 + v2 },
  minus (v1, v2) { return v1 - v2 }
};

// module/print.js
module.exports = {
  sayHello() { console.log('Hi!') }
};

// app.js
const myModule = require('./module');

// module/calc.js의 기능
const result = myModule.calc.add(1, 2);

console.log(result);

// module/print.js의 기능
myModule.print.sayHello();
```

#### 5. 코어 모듈과 파일 모듈

Node.js는 기본으로 포함하고 있는 모듈이 있다. 이를 코어 모듈이라 한다. 코어 모듈을 로딩할 때에는 패스를 명시하지 않아도 무방하다.

```js
const http = require('http');

//npm을 통해 설치한 외부 패키지 또한 패스를 명시하지 않아도 무방하다.
const mongoose = require('mongoose');

// 코어 모듈과 외부 패키지 이외는 모두 파일 모듈
// 파일 모듈을 로딩할 때에는 패스를 명시
const foo = require('./lib/foo');
```
