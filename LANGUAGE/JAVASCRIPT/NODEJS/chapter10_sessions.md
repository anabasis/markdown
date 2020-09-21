# NodeJS

## Sessions

<https://www.npmjs.com/package/express-session>

## Sessions 설치

```bash
npm install express-session --save
```

```json
"dependencies": {
    "express-session": "^1.15.6",
    ...
  }
```

## Sessions Example

```js
// 세션 선언 및 사용
var session = require('express-session');
app.use(session({
  secret : 'keyboard cat',
  resave : false ,
  saveUninitialized : true
}));
```

- resave

```txt
요청 중에 세션이 수정되지 않은 경우에도 세션이 세션 저장소에 다시 저장되도록합니다. 저장소에 따라 필요할 수도 있지만 클라이언트가 서버에 두 개의 병렬 요청을하고 한 요청에서 세션에 대한 변경 사항이 변경되지 않은 경우에도 다른 요청이 끝날 때 덮어 쓸 수있는 경쟁 조건이 만들어 질 수 있습니다 (이 동작은 사용중인 상점에 따라 다릅니다.)

기본값은 true이지만 나중에 기본값이 변경되므로 기본값을 사용하는 것이 더 이상 사용되지 않습니다. 이 설정을 조사하고 유스 케이스에 적합한 것을 선택하십시오. 일반적으로 거짓을 원할 것입니다.

이것이 내 상점에 필요한지 어떻게 알 수 있습니까? 가장 좋은 방법은 터치 방법을 구현하는 경우 상점에 문의하는 것입니다. 그렇다면 안전하게 다시 설정 : false를 설정할 수 있습니다. touch 메소드를 구현하지 않고 상점에서 저장된 세션에 만기 날짜를 설정하면 resave : true가 필요할 수 있습니다.

Forces the session to be saved back to the session store, even if the session was never modified during the request. Depending on your store this may be necessary, but it can also create race conditions where a client makes two parallel requests to your server and changes made to the session in one request may get overwritten when the other request ends, even if it made no changes (this behavior also depends on what store you're using).

The default value is true, but using the default has been deprecated, as the default will change in the future. Please research into this setting and choose what is appropriate to your use-case. Typically, you'll want false.

How do I know if this is necessary for my store? The best way to know is to check with your store if it implements the touch method. If it does, then you can safely set resave: false. If it does not implement the touch method and your store sets an expiration date on stored sessions, then you likely need resave: true.
```

- saveUninitialized

```txt

"초기화되지 않은"세션을 강제로 저장소에 저장합니다. 새 세션이지만 수정되지 않은 세션은 초기화되지 않습니다. false를 선택하면 로그인 세션을 구현하거나 서버 저장 영역 사용을 줄이거 나 쿠키 설정 전에 권한이 필요한 법률을 준수하는 데 유용합니다. false를 선택하면 클라이언트가 세션없이 여러 개의 병렬 요청을하는 경쟁 조건에도 도움이됩니다.

기본값은 true이지만 나중에 기본값이 변경되므로 기본값을 사용하는 것이 더 이상 사용되지 않습니다. 이 설정을 조사하고 유스 케이스에 적합한 것을 선택하십시오.

PassportJS와 함께 Session을 사용하는 경우 Passport는 사용자가 인증 된 후에 사용할 빈 Passport 개체를 세션에 추가합니다.이 개체는 세션 수정으로 간주되어 저장됩니다. 이 문제는 PassportJS 0.3.0에서 수정되었습니다.

Forces a session that is "uninitialized" to be saved to the store. A session is uninitialized when it is new but not modified. Choosing false is useful for implementing login sessions, reducing server storage usage, or complying with laws that require permission before setting a cookie. Choosing false will also help with race conditions where a client makes multiple parallel requests without a session.

The default value is true, but using the default has been deprecated, as the default will change in the future. Please research into this setting and choose what is appropriate to your use-case.

Note if you are using Session in conjunction with PassportJS, Passport will add an empty Passport object to the session for use after a user is authenticated, which will be treated as a modification to the session, causing it to be saved. This has been fixed in PassportJS 0.3.0


```

```js
var express = require('express');
var app = express();

// POST 방식 전송 모듈 선언 및 사용
var bodyParser = require('body-parser');
app.use(bodyParser.urlencoded({extended:false}));

// 세션 선언 및 사용
var session = require('express-session');
app.use(session({
  secret : 'keyboard cat',
  resave : false ,
  saveUninitialized : true
}));

// 세션 Count
app.get('/count', (req, res) => {
  if(req.session.count)
    req.session.count ++;
  else
    req.session.count = 1 ;
  res.send('count : ' + req.session.count);
});

// 로그인 페이지
app.get('/auth/login', (req, res) => {
  var output = `
  <form action="/auth/login" method="post">
    <p><input type="text" name="username" placeholder="username" ></p>
    <p><input type="password" name="password" placeholder="password" ></p>
    <p><input type="submit" ></p>
  </form>
  `;
  res.send(output);
});

// 로그인 요청처리 페이지
app.post('/auth/login', (req, res) => {
  var user ={
    username : 'node',
    password : '11111',
    displayName : 'Node Master'
  };
  var uname = req.body.username;
  var pwd = req.body.password;
  if(uname === user.username && pwd === user.password){
    req.session.displayName = user.displayName;
    res.redirect('/welcome');
  }else{
    res.send('Who are you? <a href="/auth/login">login</a>');
  }
});

// 웰콤 페이지
app.get('/welcome', (req,res) => {
  if(req.session.displayName){
    res.send(`
      <h1>Hello, ${req.session.displayName}</h1>
      <a href="/auth/logout">Logout</a>
      `);
  }else{
    res.send(`
      <h1>Welcome</h1>
      <a href="/auth/login">Login</a>
      `);
  }
  //res.send(JSON.stringify(req.session));
});

app.get('/auth/logout', (req, res) => {
  delete req.session.displayName;
  res.redirect('/welcome');
});

// 미들웨어 기동
app.listen(3000,(req,res) => {
  console.log('Cookie Connected listen on port 3000 !!!');
});
```