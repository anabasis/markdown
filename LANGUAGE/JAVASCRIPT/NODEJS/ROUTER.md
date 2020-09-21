# Node JS Router

## 1. 디렉토리 구조 이해하기

```txt
express_tutorial/
├── package.json
├── public
│   └── css
│   └── style.css
├── router
│   └── main.js
├── server.js
└── views
 ├── about.html
 └── index.html
```

## 라우터 셈플

- 서버파일 생성 server.js

```js
var express = require('express');
var app = express();

// 라우터 파일
var router = require('./router/main')(app);  // 예제 1
//var router = require('./router/main');  // 예제 2

app.set('views', __dirname + '/views');
app.set('view engine', 'pug');

var server = app.listen(3000, function(){
    console.log("Express server has started on port 3000")
});
```

- 라우터 파일 분리
- router디렉토리 , main.js 생성

### 예제 1

```js
module.exports = function(app)
{
    app.get('/',(req,res) => {
        res.render('index.html')
    });
    app.get('/about',(req,res) => {
        res.render('about.html');
    });
}
```

### 예제 2

```js
var express = require('express');
var router = express.Router();

router.get('/', (req,res) => {
    res.render('index.html')
});
router.get('/about',(req,res) => {
    res.render('about.html');
});
module.exports = router;
```

- html 파일 생성

```html
<html>
  <head>
    <title>Main</title>
    <link rel="stylesheet" type="text/css" href="css/style.css">
  </head>
  <body>
    Hey, this is index page
  </body>
</html>
```

```html
<html>
  <head>
    <title>About</title>
    <link rel="stylesheet" type="text/css" href="css/style.css">
  </head>
  <body>
    About... what?
  </body>
</html>
```

## 라우트 핸들러

- 미들웨어와 비슷하게 작동하는 여러 콜백 함수를 제공하여 요청을 처리
- 이러한 콜백은 next('route')를 호출하여 나머지 라우트 콜백을 우회할 수도 있다는 점
- 이러한 메커니즘을 이용하면 라우트에 대한 사전 조건을 지정한 후, 현재의 라우트를 계속할 이유가 없는 경우에는 제어를 후속 라우트에 전달

### 하나의 콜백 함수는 하나의 라우트를 처리

```js
app.get('/example/a', function (req, res) {
  res.send('Hello from A!');
});
```

### 2개 이상의 콜백 함수는 하나의 라우트를 처리(next 오브젝트를 반드시 지정해야 함)

```js
app.get('/example/b', function (req, res, next) {
  console.log('the response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from B!');
});
```

### 하나의 콜백 함수 배열은 하나의 라우트를 처리

```js
var cb0 = function (req, res, next) {
  console.log('CB0');
  next();
}

var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}

var cb2 = function (req, res) {
  res.send('Hello from C!');
}

app.get('/example/c', [cb0, cb1, cb2]);
```

### 독립적인 함수와 함수 배열의 조합은 하나의 라우트를 처리

```js
var cb0 = function (req, res, next) {
  console.log('CB0');
  next();
}

var cb1 = function (req, res, next) {
  console.log('CB1');
  next();
}

app.get('/example/d', [cb0, cb1], function (req, res, next) {
  console.log('the response will be sent by the next function ...');
  next();
}, function (req, res) {
  res.send('Hello from D!');
});
```

## 응답 메소드

|메소드|설명|
|:--:|:--|
|res.download()|파일이 다운로드되도록 프롬프트합니다.|
|res.end()|응답 프로세스를 종료합니다.|
|res.json()|JSON 응답을 전송합니다.|
|res.jsonp()|JSONP 지원을 통해 JSON 응답을 전송합니다.|
|res.redirect()|요청의 경로를 재지정합니다.|
|res.render()|보기 템플리트를 렌더링합니다.|
|res.send()|다양한 유형의 응답을 전송합니다.|
|res.sendFile|파일을 옥텟 스트림의 형태로 전송합니다.|
|res.sendStatus()|응답 상태 코드를 설정한 후 해당 코드를 문자열로 표현한 내용을 응답 본문으로서 전송합니다.|

## app.route()

- app.route()를 이용하면 라우트 경로에 대하여 체인 가능한 라우트 핸들러를 작성
- 경로는 한 곳에 지정되어 있으므로, 모듈식 라우트를 작성하면 중복성과 오타가 감소하여 도움
- 라우트에 대한 자세한 정보는 Router() 문서를 참조

```js
app.route('/book')
  .get(function(req, res) {
    res.send('Get a random book');
  })
  .post(function(req, res) {
    res.send('Add a book');
  })
  .put(function(req, res) {
    res.send('Update the book');
  });
```

## express.Router

- express.Router 클래스를 사용하면 모듈식 마운팅 가능한 핸들러를 작성
- Router 인스턴스는 완전한 미들웨어이자 라우팅 시스템
- 따라서 “미니 앱(mini-app)”이라고 불리는 경우가 많음

```js
var express = require('express');
var router = express.Router();

// middleware that is specific to this router
router.use(function timeLog(req, res, next) {
  console.log('Time: ', Date.now());
  next();
});
// define the home page route
router.get('/', function(req, res) {
  res.send('Birds home page');
});
// define the about route
router.get('/about', function(req, res) {
  res.send('About birds');
});

module.exports = router;
```

```js
var birds = require('./birds');
...
app.use('/birds', birds);
```

앱은 이제 /birds 및 /birds/about에 대한 요청을 처리할 수 있게 되었으며, 해당 라우트에 대한 특정한 미들웨어 함수인 timeLog를 호출
