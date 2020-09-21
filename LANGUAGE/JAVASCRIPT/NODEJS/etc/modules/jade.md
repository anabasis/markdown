# jade 패키지

- Node.js를 위한 빠르고 개방적인 간결한 웹 프레임워크

<https://www.npmjs.com/package/jade>
<http://jade-lang.com/>
<https://www.npmjs.com/package/pug>

<http://blog.doortts.com/223>
<https://opentutorials.org/course/2136/11915>
<https://wayhome25.github.io/nodejs/2017/02/18/nodejs-10-express-template-engine-jade/>

```json
"dependencies": {
    "jade": "^1.11.0",
    "pug": "^2.0.1",
  }
```

## jade/pug 설치

```bash
npm install jade --save
npm install pug --save
```

```js
var express = require('express');
var app = express();
```

----------------------------------------------------------------------

## Jade - template engine

### 기능들

- 클라이언트 사이드 지원
- 뛰어난 가독성
- 유연한 띄어쓰기
- 블럭 확대(block-expansion)
- 믹스인(mixins, 섞어쓰는 것)
- 스태틱 인클루드(static includes)
- 속성 인터폴레이션(attribute interpolation)
- 보안을 위해 코드는 기본적으로 이스케이프 처리된다.
- 컴파일과 런타임시에 문맥에 맞는 에러 출력
- 커맨드 라인을 통해 jade 템플릿을 컴파일 할 수 있음
- html 5 모드 (!!! 5 doctype)
- 선택적인 메모리 캐싱
- 다이내믹과 스태틱 태그 클래스 조합
- 필터를 이용한 트리구조 파싱 parse tree manipulation via filters
- 템플릿 상속
- 외부 모듈로 Express JS 지원
- 객체, 배열, 그리고 열거형이 아닌것들에 대해서도  each 를 이용하여 투명하게 이터레이션 지원
- 블럭 코멘트
- tag를 이용한 접두어처리 없음
- AST 필터
- 필터들
  - :stylus 는 stylus가 인스톨 되어 있어야 한다.
  - :sass 는 sass.js가 인스톨 되어 있어야 한다.
  - :less 는 less.js가 인스톨 되어 있어야 한다.
  - :markdown 은 markdown-js 혹은 node-discount가 설치되어 있어야 한다.
  - :cdata
  - :coffeescript 는 coffee-script가 인스톨 되어 있어야 한다.
- Vim Syntax
- TextMate Bundle
- 스크린캐스트 보기(추천!!)
- html2jade converter

### 구현체들

- php
- scala
- ruby

### 설치

```bash
npm install jade
```

### Public API

```js
var jade = require('jade');

// Compile a function
var fn = jade.compile('string of jade', options);
fn(locals);
```

#### Options

- self 로컬 값을 포함하기 위해  self  네임스페이스를 사용(기본값 false)
- locals 로컬 변수 객체
- filename Used in exceptions, and required when using includes
- debug Outputs tokens and function body generated
- compiler Compiler to replace jade's default
- compileDebug When false no debug instrumentation is compiled

### 문법

#### Line Endings

- CRLF 와 CR 은 파싱되기 전에 LF로 변환

#### Tags

```jade
html --> <html></html\>
div#container --> <div id="container"></div>
div.user-details --> <div class="user-details"></div>
div#foo.bar.baz --> <div id="foo" class="bar baz"></div>
#foo
.bar --> <div id="foo"></div><div class="bar"></div>
```

#### 태그 텍스트(Tag Text)

```jade
p wahoo! --> <p>wahoo!</p>
p
  | foo bar baz
  | rawr rawr
  | super cool
  | go jade go
--> <p>foo bar baz rawr.....</p>
```

- 인터폴레이션(interpolation, 중간중간 끼워넣어 완성시키는 것)
- 다음과 같은 값 { name: 'tj', email: 'tj@vision-media.ca' } 을 컴파일 함수로 넘김

```jade
#user #{name} <#{email}> --> <div id="user">tj <tj@vision-media.ca></div>
```

```js
"#{animals} on a #{transport}".interpolate({ animals: "Pigs", transport: "Surfboard" });
// 결과
"Pigs on a Surfboard"
```

```jade
// 특정 이유로  #{} 를 출력하고 싶다면? 이스케이프 처리
p \#{something}
// 결과
<p>#{something}</p>
```

```js
//이스케이프 처리하지 않길 바랄때 다음과 같이 #대신 !를 사용 !{html}
- var html = "<script></script>"
| !{html}
```

```jade
//중첩된 태그들 또한 선택적으로 텍스트 블럭을 사용
label
  | Username:
  input(name='user[name]')
//바로 태그 텍스트
label Username:
  input(name='user[name]')
```

```jade
//오직 텍스트만 쓸 수 있는  script, style, 그리고 textarea 같은 태그들은 긴 문장이라도 앞에  | 문자를 붙일 필요 없음
html
  head
    title Example
    script
      if (foo) {
        bar();
      } else {
        baz();
      }
```

```jade
// 텍스트 블럭을 나타내기 위해 마지막에 '.'
  p.
    foo asdf
    asdf
     asdfasdfaf
     asdf
    asd
```

```html
출력:
  <p>foo asdf
  asdf
    asdfasdfaf
    asdf
  asd
  </p>
```

```jade
// 끝에 '.'을 붙일 때 공백을 주면 Jade파서는 무시
// 일반 문자로 간주
p .
```

```html
출력:
<p>.</p>
```

```jade
// 이스케이프 문자(\)를 찍고 싶으면 두개
p.
  foo\\bar
```

```html
결과 :
</p>foo\bar</p>
```

#### 주석(Comments)

```jade
// just some paragraphs
p foo
p bar
```

```html
출력 :
<!-- just some paragraphs -->
<p>foo</p>
<p>bar</p>
```

```jade
// 하이픈을 붙이면 파싱할 때 제외한다.
//- will not output within markup
p foo
p bar
```

```html
출력
<p>foo</p>
<p>bar</p>
```

#### 블럭 주석(Block Comments)

```jade
  body
    //
      #content
        h1 Example
```

```html
출력 :
<body>
  <!--
  <div id="content">
    <h1>Example</h1>
  </div>
  -->
</body>
```

```jade
//Jade는 조건절 주석도 잘 지원한다.
head
  //if lt IE 8
    script(src='/ie-sucks.js')
```

```html
출력:
<body>
  <!--[if lt IE 8]>
    <script src="/ie-sucks.js"></script>
  <![endif]-->
</body>
```

#### 중첩(Nesting)

```jade
// Jade는 일반적인 방식으로 태그 정의에 중첩을 지원
ul
  li.first
    a(href='#') foo
  li
    a(href='#') bar
  li.last
    a(href='#') baz
```

```html
<ul>
  <li id="first"><a href="#">foo</a>
  </li>
  <li><a href="#">bar</a>
  </li>
  <li class="last"><a href="#">baz</a>
  </li>
</ul>
```

#### 블럭 확장(Block Expansion)

```jade
// 블럭 확장은 한줄짜리 중첩 태그 `:`
ul
  li.first: a(href='#') foo
  li: a(href='#') bar
  li.last: a(href='#') baz
```

#### 속성들(Attributes)

```jade
// Jade는 현재 속성 구분자로 '(' 와 ')'를 사용
a(href='/login', title='View login page') Login

// 속성의 값이 undefined 나 null 이면 표시되지 않음
div(something=null)

// boolean 속성도 지원한다.
input(type="checkbox", checked)

// 코드가 붙은 boolean 속성은 코드의 값이  true 일 때만 속성이 표시
input(type="checkbox", checked=someValue)

// 여러 라인에 걸쳐서도 잘 동작한다.
input(type='checkbox',
  name='agreement',
  checked)

// 콤마가 없는 여러 라인일 경우도 잘 된다.
input(type='checkbox'
  name='agreement'
  checked)

// 공백도 가능
input(
  type='checkbox'
  name='agreement'
  checked)

// 콜론도 됨
rss(xmlns:atom="atom")


 // { id: 12, name: 'tobi' } 값을 나타내는 user라는 로컬 변수
 // 앵커 태그가 "/user/12" 지정
 // 자바스크립트의 문자열 연결방식을 사용
a(href='/user/' + user.id)= user.name

// jade은 인터폴레이션을 쓸 수도 있음
// 루비를 쓰거나 커피스크립트를 쓰는 사람들이 일반적으로 쓰는 것
a(href='/user/#{user.id}')= user.name

// class 속성은 배열을 받는 특별 케이스
// bodyClasses = ['user', 'authenticated']
body(class=bodyClasses)
```

#### HTML

```jade
html
  body
    | <h1>Title</h1>
    | <p>foo bar baz</p>

// 파이프를 생략하기 위해 끝에 .을 붙여서 Jade에게 텍스트 블럭임을 알려줌
html
  body.
    <h1>Title</h1>
    <p>foo bar baz</p>
```

```html
결과 :
<html><body><h1>Title</h1>
<p>foo bar baz</p>
</body></html>
```

```jade
html
  body
    h1 User <em>#{name}</em>
```

#### Doctypes

```jade
// 독타입을 추가하려면  !!!, 혹은 선택적으로 doctype 을 붙임
// transitional doctype
!!!

!!! 5
!!! html
doctype html

//독타입은 대소문자를 가리지 않음
doctype Basic
doctype basic

// 독타입 문자열을 전달
doctype html PUBLIC "-//W3C//DTD XHTML Basic 1.1//EN
```

```html
결과:
<!DOCTYPE html PUBLIC "-//W3C//DTD XHTML Basic 1.1//EN>
```

```js
//바꾸고 싶으면 아래처럼 간단히 된다.
jade.doctypes.default = 'whatever you want';
```

### 필터(Filters)

- 필터들은 앞에 :를 붙임
- Filter List <https://www.npmjs.com/browse/keyword/jstransformer>

```jade
body
  :markdown
    Woah! jade _and_ markdown, very **cool**
    we can even link to [stuff](http://google.com)
```

```html
결과 :
<body><p>Woah! jade <em>and</em> markdown, very <strong>cool</strong> we can even link to <a href="http://google.com">stuff</a></p></body>
```

### 코드(Code)

- Jade에서는 세 가지 종류의 실행 코드를 지원
- 첫번째 '-' 를 앞에 붙이는 것

```jade
- var foo = 'bar';

// 조건문이나 반복문
- for (var key in obj)
  p= obj[key]

// Jade의 버퍼링 기술
- if (foo)
  ul
    li yay
    li foo
    li worked
- else
  p oh no! didnt work

// 복잡한 이터레이션
- if (items.length)
  ul
    - items.forEach(function(item){
      li= item
    - })
```

- 두 번째는 이스케이프된 버퍼 코드 앞에 반환값으로 쓰이는 버퍼값에 사용되며 앞에 =를 사용

```jade
- var foo = 'bar'
= foo
h1= foo
```

```html
결과
bar<h1>bar</h1>
```

```jade
// 보안상의 이유로 = 가 붙은 버퍼 코드는 기본적으로 이스케이프 처리
// 이스케이프 처리하고 싶지 않으면  != 사용
p!= aVarContainingMoreHTML
```

- Jade는 또한 디자이너 친화적인 변형
- 자바스크립트 리터럴을 좀 더 표현적이고 선언적으로 만들어 줌
- 다음 할당문은 동등

```jade
 - var foo = 'foo ' + 'bar'
 foo = 'foo ' + 'bar'
```

- 비슷하게 Jade는 퍼스트 클래스(first-class) if, else if, else, until, while, unless 명령어
- 자바스크립트 표현식

```jade
 if foo == 'bar'
   ul
     li yay
     li foo
     li worked
 else
   p oh no! didnt work  
```

### 반복문(Iteration)

- 디자이너 친화적인 템플릿을 만드는 것 가능하게 해주는 생성자

```jade
each VAL[, KEY] in OBJ

//배열

- var items = ["one", "two", "three"]
each item in items
  li= item
```

```html
출력 :
<li>one</li>
<li>two</li>
<li>three</li>
```

```jade
// 인덱스로 반복처리
items = ["one", "two", "three"]
each item, i in items
  li #{item}: #{i}
```

```html
출력 :
<li>one: 0</li>
<li>two: 1</li>
<li>three: 2</li>
```

```jade
// 객체의 키와 값으로 반복처리를 하려면:
obj = { foo: 'bar' }
each val, key in obj
  li #{key}: #{val}
```

```html
출력 :
<li>foo: bar</li>
```

- 내부적으로 Jade는 이런 문장들을 users.forEach(function(user){와 같은 일반적인 자바스크립트 반복문으로 변환
- 렉시컬 스코프와 중첩적용은 일반적인 자바스크립트 규칙을 따름

```jade
each user in users
  each role in user.roles
    li= role

// for
for user in users
  for role in user.roles
    li= role
```

### 조건절(Conditionals)

- Jade의 조건절은  (-) 접두어 붙은 코드를 사용하는 방법
- 디자이너 친화적인 방식을 제공

```jade
for user in users
  if user.role == 'admin'
    p #{user.name} is an admin
  else
    p= user.name

// 자바스크립트 리터럴를 사용하는 것과 같음
for user in users
  - if (user.role == 'admin')
    p #{user.name} is an admin
  - else
    p= user.name

// Jade는  unless 라는 키워드를 가지고 있는데, 이는 자바스크립트의 if (!(expr)) 에 해당
for user in users
  unless user.isAnonymous
    p
      | Click to view
      a(href='/users/' + user.id)= user.name 
```

### 템플릿 상속(Template inheritance)

- Jade는  block 와 extends 키워드를 이요해서 템플릿 상속을 지원
- 블럭은 말그대로 Jade의 'block'
- 자식 템플릿으로 교체되며, 재귀적으로 적용

- Jade의 블럭은 기본 내용을 갖고 있을 수 도 있음(선택사항)
- block scripts, block content, 그리고 block foot 옵션으로 사용

```jade
html
  head
    h1 My Site - #{title}
    block scripts
      script(src='/jquery.js')
  body
    block content
    block foot
      #footer
        p some footer content
```

그리고 레이아웃을 확장하고 싶다면 새로운 파일을 만들어서  extends 지시어를 아래 코드처럼 사용한다. 지시어 옆에는 경로를 쓸 수 있으며 .jade 확장자를 붙이는 건 선택사항이다. 하나 혹은 그 이상의 블럭도 정의할 수 있는 데 해당 블럭은 부모 블럭 컨텐트를 덮어쓰게된다. 아래 코드에서 foot 블럭은 정의되지 않았기 때문에 결과는 "some footer content"라고 출력될 것이다.

```jade
extends extend-layout

block scripts
  script(src='/jquery.js')
  script(src='/pets.js')

block content
  h1= title
  each pet in pets
    include pet
```

- 추가적인 블럭들을 제공하기 위해 어떤 블럭을 덮어쓰는 것도 가능
- content 가  sidebar 와primary 를 덮어쓰기용 블럭으로 표현
- 자식 템플릿은 필요에 따라서는 content를 통으로 덮어쓸 수도 있음

```jade
extends regular-layout

block content
  .sidebar
    block sidebar
      p nothing
  .primary
    block primary
      p nothing
```

### 인클루드(Includes)

- 인클루드는 정적으로 Jade 덩어리들, 혹은 css, html 같이 별도로 분리되어 있는 파일들을 포함
- 헤더와 풋터를 인클루딩 하는 것

```bash
  ./layout.jade
  ./includes/
    ./head.jade
    ./tail.jade
```

```jade
html
  include includes/head
  body
    h1 My Site
    p Welcome to my super amazing site.
    include includes/foot
```

- includes/head and includes/foot 둘다 layout.jade의 filename 옵션에 맞춰 상대 경로로 포함
- 절대 경로도 쓸 수 있음
- 해당 파일들을 파싱해서 기대한 대로 AST에 끼워넣기(옮긴이주. AST: Abstract syntax tree)

```jade
<html>
  <head>
    <title>My Site</title>
    <script src="/javascripts/jquery.js">
    </script><script src="/javascripts/app.js"></script>
  </head>
  <body>
    <h1>My Site</h1>
    <p>Welcome to my super lame site.</p>
    <div id="footer">
      <p>Copyright>(c) foobar</p>
    </div>
  </body>
</html>
```

- 확장자를 보고 Jade가 해당 파일이 Jade 소스가 아니라고 판단하면 문자 그대로 포함만 함

```jsde
html
  body
    include content.html
```

- 인클루드 지시자는 블럭에 쓸 수 도 있음
- 주어진 블럭은 파일에서 정의된 블럭의 마지막에 추가

```jade
// head.jade
head
  script(src='/jquery.js')
```

- 아래 처럼 블럭에  include head 을 써서 두 개의 스크립트를 추가

```jade
html
  include head
    script(src='/foo.js')
    script(src='/bar.js')
  body
    h1 test
```

### 믹스인(Mixins)


- 믹스인은 일종의 메타 코드 모듈
- Jade에서는 믹스인을 그냥 '템플릿 안의 템플릿'
- 믹스인은 컴파일된 템플릿 내에서 일반적인 자바스크립트로 변환된다. 
- 믹스인은 필요에 따라서는 인자도 가질 수도 있음

```jade
mixin list
  ul
    li foo
    li bar
    li baz

// 믹스인을 인자없이 쓰면 블럭이랑 비슷하게 보임
h2 Groceries
mixin list
```

- 믹스인은 하나 이상의 인자를 가짐
- 인자는 일반적인 자바스크립트 표현식처럼 사용

```jade
mixin pets(pets)
  ul.pets
    - each pet in pets
      li= pet

mixin profile(user)
  .user
    h2= user.name
    mixin pets(user.pets)
```

```html
<div class="user">
  <h2>tj</h2>
  <ul class="pets">
    <li>tobi</li>
    <li>loki</li>
    <li>jane</li>
    <li>manny</li>
  </ul>
</div>
```


- 일종의 반복해서 나타나는 구조를 템플릿으로 만들어서 간단히 캡슐화 시키는거죠.

```jade
//믹스인 정의. 같은 파일내에 있어도 무방
mixin field(type, name, label)
  .field(class='field-' + type)
    label #{label}:
    input(type=type, name='user[#{name}]', value=user[name])

//실제 템플릿 코드
form
  mixin field('text', 'name', 'Username')
  mixin field('text', 'email', 'Email')
  mixin field('password', 'pass', 'Password')
  input(type='submit', value='Sign Up')
```

```html
결과 :
<form>
  <div class="field field-text">
    <label>Username:</label>
    <input type="text" name="user[name]" value="TJ Holowaychuk"/>
  </div>
  <div class="field field-text">
    <label>Email:</label>
    <input type="text" name="user[email]" value="tj@learnboost.com"/>
  </div>
  <div class="field field-password">
    <label>Password:</label>
    <input type="password" name="user[pass]"/>
  </div>
  <input type="submit" value="Sign Up"/>
</form>
```

### jade 명령행

```bash
Usage: jade [options] [dir|file ...]

Options:

  -h, --help         output usage information
  -v, --version      output the version number
  -o, --obj <str>    javascript options object
  -O, --out <dir>    output the compiled html to <dir>
  -p, --path <path>  filename used to resolve includes over stdio

Examples:

  # translate jade the templates dir
  $ jade templates

  # create {foo,bar}.html
  $ jade {foo,bar}.jade

  # jade over stdio
  $ jade < my.jade > my.html

  # jade over stdio specifying filename to resolve include directives
  $ jade < my.jade -p my.jade > my.html

  # jade over stdio
  $ echo "h1 Jade!" | jade

  # foo, bar dirs rendering to /tmp
  $ jade foo bar --out /tmp
```