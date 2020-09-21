# NodeJS

<http://expressjs.com/ko/>
<http://expressjs.com/ko/starter/static-files.html>
<http://expressjs.com/ko/4x/api.html>

<https://m.blog.naver.com/PostList.nhn?blogId=azure0777&categoryNo=18&listStyle=style1>

```bash
npm install express --save
```

## Express

```bash
mkdir myapp
cd myapp
npm init
npm install --save express
```

### Entry File/Entry Point

```js
var express = require('express');
var app = express();
```

- Arrow함수 Express

```js
app.get('/', (req, res) => {
  res.send('Arrow Hello World!');
});
app.get('/login', (req, res) => {
  res.send('Arrow Hello World! Login');
});
app.listen(3000, () => {
  console.log('Arrow Example app listening on port 3000!');
});
```

- Express

```js
app.get('/', function (req, res) {
  res.send('Hello World!');
});
app.listen(3000, function () {
  console.log('Example app listening on port 3000!');
});
```

### Express 정적파일 제공

<http://expressjs.com/ko/starter/static-files.html>

```js
app.use(express.static(__dirname + '/public'));
app.use('/static', express.static(__dirname + '/public'));

app.get('/route', (req,res) => {
    res.send('Hello Router, <img src="/Periodic-Table.png">');
});
```

### Express 정적/동적처리

- 정적처리 /public/test.html
- <http://localhost:3000/test.html>

```html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    Static Hello, Static !!
    <ul>
      <li>coding</li>
      <li>coding</li>
      <li>coding</li>
      <li>coding</li>
    </ul>
  </body>
</html>
```

- 동적처리
- <<http://localhost:3000/dynamic>>

```js
app.get('/dynamic', (req,res) => {
  var lis = '';
  for(var i = 0 ; i < 5 ; i++){
    lis = lis + '<li>coding</li>';
  }
  var time = Date();

  var output = `
  <!DOCTYPE html>
  <html>
    <head>
      <meta charset="utf-8">
      <title></title>
    </head>
    <body>
      Dynamic Hello, Static !!
      <ul>
      ${lis}
      </ul>
      ${time}
    </body>
  </html>
  `;
  res.send(output);
});
```

### 템플릿

<http://jade-lang.com/>
<https://www.npmjs.com/package/pug>

```bash
npm install jade --save
npm install pug --save  ## jade의 다른 이름
```

```js
// Template 설정
//express에게 template view engine을 알려주는 설정
app.set('view engine', 'jade');
//express에게 template 파일이 모여있는 디렉토리를 알려주는 설정
app.set('views', './views');

// Template 랜더링
app.get('/template', (req,res) => {
  //res.render('temp');  // temp.jade랜더링
  res.render('temp', {time:Date(), _title:'Jade 랜더링'});  // 파라메터 전달
});
```

- jade express code pretty

```js
// Express 4.x
if(app.get('env') === 'developement'){
    app.locals.pretty = true;
}
```
