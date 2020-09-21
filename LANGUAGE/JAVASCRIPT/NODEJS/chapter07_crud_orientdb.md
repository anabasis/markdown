# NodeJS

## Custom App 소스

```bash
npm install supervisor -g  # 즉시반영
npm install express --save  # express모듈
npm install pug -save  # pug/jade 템플릿 엔진 모듈
npm install body-parser --save  # POST방식 전송 모듈
```

<http://expressjs.com/en/4x/api.html#req.body>

```js
// 패키지 선언
var express = require('express');
var bodyParser = require('body-parser');
var fs = require('fs');

// 사용선언 및 설정
var app = express();
app.use(bodyParser.urlencoded({ extended: false}));
app.locals.pretty = true;

// pug(구 jade) 선언
app.set('view engine', 'pug');
app.set('views', __dirname + '/view_files');

// 신규생성 페이지
app.get('/topic/new', (req,res) => {
  fs.readdir( __dirname + '/data', (err, files) => {
    if(err){
      console.log(err);
      res.status(500).send('Custom Internal Server Error !!!!');
    }
    res.render("new",{topics:files});
  });
});

// 목록 페이지, 상세페이지
app.get(['/topic','/topic/:id'], (req,res) => {

  fs.readdir( __dirname + '/data', (err, files) => {
    if(err){
      console.log(err);
      res.status(500).send('Custom Internal Server Error !!!!');
    }
    var id = req.params.id;
    if(id){
      fs.readFile(__dirname + '/data/'+id, 'utf8', (err, data) => {
        if(err){
          console.log(err);
          res.status(500).send('Custom Internal Server Error !!!!');
        }
        //res.send(data);
        res.render('view', {topics:files, title:id, description:data });
      });
    }else{
      res.render('view', {topics:files, title:'Welcome', description:'Hello Javascript for Server'});
    }
  });
});

// 등록페이지
app.post('/topic', (req,res) => {
  var title = req.body.title;
  var description = req.body.description;
  fs.writeFile(__dirname + '/data/'+title, description, (err) => {
    if(err){
      console.log(err);
      res.status(500).send('Custom Internal Server Error');
    }
    res.redirect('/topic/'+title);
  });
});

// 미들웨어 기동
app.listen(3000,(req,res) => {
  console.log('Connected listen on port 3000 !!!');
});
```

```jade
//[new.pug]
doctype html
html
  head
    meta(charset='utf-8')
  body
    hi
      a(href='/topic') Server Side Javascript
    ul
      each topic in topics
        li
          a(href='/topic/'+topic)= topic
    article
    form(action='/topic' method='post')
      p
        input(type='text' name='title' placeholder='title')
      p
        textarea(name='description')
      p
        input(type='submit')

```

```jade
//[view.pug]
doctype html
html
  head
    meta(charset='utf-8')
  body
    hi
      a(href='/topic') Server Side Javascript
    ul
      each topic in topics
        li
          a(href='/topic/'+topic)= topic
    article
      h2= title
      = description
    div
      a(href='/topic/new') NEW
```

## OrientDB설치

```bash
npm install orientjs --save
npm install orientjs -g
```

## OrientDB Example

- DATABASE BOARD PLAN

```js
get('/topic/') : view.pug
get('/topic/:id') : view.pug
get('/topic/add') : add.pug
  post('/topic/add')
  get('/topic/:id')
get('/topic/:id/edit') : edit.pug
  post('/topic/:id/edit')
  get('/topic/:id')
get('/topic/:id') : delete.pug
  post('/topic/:id/delete')
  get('/topic/')
```

- app_orientdb.js

```js
// 패키지 선언
var express = require('express');
var bodyParser = require('body-parser');
var fs = require('fs');

// 사용선언 및 설정
var app = express();
app.use(bodyParser.urlencoded({ extended: false}));
app.locals.pretty = true;

// Database 설정
var OrientDB = require('orientjs');
var server = OrientDB({
  host:'localhost',
  port:2424,
  username:'root',
  password:'11111'
});
var db = server.use('o2');

// pug(구 jade) 선언
app.set('view engine', 'pug');
app.set('views', __dirname + '/view_orientdb');

// 신규생성 페이지
app.get('/topic/add', (req,res) => {
  var sql = 'SELECT FROM topic';
  db.query(sql).then((topics) => {
    res.render("add",{topics:topics});
  });
});

// 신규생성 페이지 저장
app.post('/topic/add', (req,res) => {
  var title = req.body.title;
  var description = req.body.description;
  var author = req.body.author;
  var sql = 'INSERT INTO topic (title, description, author) VALUES ( :title, :description, :author)';
  db.query(sql,{
    params:{
      title:title,
      description:description,
      author:author
    }
  }).then((topics) => {
    res.redirect('/topic/'+encodeURIComponent(topics[0]['@rid']));
  });
});

// 내용변경 페이지
app.get('/topic/:id/edit', (req,res) => {
  var sql = 'SELECT FROM topic';
  var id = req.params.id;
  db.query(sql).then((topics) => {
    var sql = 'SELECT FROM topic WHERE @rid=:rid';
    db.query(sql, {params:{rid:id}}).then((topic) => {
      res.render('edit',{topics:topics, topic:topic[0]});
    });
  });
});

// 내용변경 페이지 저장
app.post('/topic/:id/edit', (req,res) => {
  var sql = 'UPDATE topic SET title=:title , description=:description , author=:author  WHERE @rid=:rid';
  var id = req.params.id;
  db.query(sql,{
    params:{
      title:req.body.title,
      description:req.body.description,
      author:req.body.author,
      rid:req.params.id
    }
  }).then( (results) => {
    console.log(results);
    res.redirect('/topic/'+encodeURIComponent(id));
  });
});

// 목록 및 상세 정보
app.get(['/topic','/topic/:id'], (req,res) => {
  var sql = "SELECT FROM topic";
  db.query(sql).then((topics) => {
    var sql = 'SELECT FROM topic WHERE @rid=:rid';
    var id = req.params.id;
    if(id){
      db.query(sql, {params:{rid:id}}).then((topic) => {
        console.log(topic[0]);
        res.render('view',{topics:topics, topic:topic[0]});
      });
    } else {
      res.render('view', {topics:topics});
    }
  });
});

// 삭제 페이지
app.get('/topic/:id/delete', (req,res) => {
  var sql = 'SELECT FROM topic';
  var id = req.params.id;
  db.query(sql).then((topics) => {
    var sql = 'SELECT FROM topic WHERE @rid=:rid';
    db.query(sql, {params:{rid:id}}).then((topic) => {
      res.render('delete',{topics:topics, topic:topic[0]});
    });
  });
});

// 삭제처리 저장
app.post('/topic/:id/delete', (req,res) => {
  var sql = 'DELETE FROM topic WHERE @rid=:rid';
  var id = req.params.id;
  db.query(sql,{
    params:{
      rid:id
    }
  }).then( (results) => {
    console.log(results);
    res.redirect('/topic/');
  });
});

// 미들웨어 기동
app.listen(3000,(req,res) => {
  console.log('Connected listen on port 3000 !!!');
});

```

- view.pug

```jade
doctype html
html
  head
    meta(charset='utf-8')
  body
    hi
      a(href='/topic') Server Side Javascript(OrientDB)
    ul
      each topic in topics
        li
          - rid = encodeURIComponent(topic['@rid'])
          a(href='/topic/'+ rid)= topic.title
          // topic.title = topic['title']
    article
      if topic
        h2= topic.title
        p description :
          = topic.description
        p author :
          = topic.author
      else
        h2 Welcome
        | This is server side javascript tutorial.
    ul
      li
        a(href='/topic/add') Add
      if topic
        li
          - rid = encodeURIComponent(topic['@rid'])
          a(href='/topic/'+rid+'/edit') Edit
        li
          a(href='/topic/'+rid+'/delete') Delete
```

- add.pug

```jade
doctype html
html
  head
    meta(charset='utf-8')
  body
    hi
      a(href='/topic') ADD Server Side Javascript(OrientDB)
      ul
        each topic in topics
          li
            - rid = encodeURIComponent(topic['@rid'])
            a(href='/topic/'+ rid)= topic.title
            // topic.title = topic['title']
    article
    form(action='/topic/add' method='post')
      p
        input(type='text' name='title' placeholder='title')
      p
        textarea(name='description' placeholder='description')
      p
        input(type='text' name='author' placeholder='author')
      p
        input(type='submit' value='추가')
```

- edit.pug

```jade
doctype html
html
  head
    meta(charset='utf-8')
  body
    hi
      a(href='/topic') EDIT Server Side Javascript(OrientDB)
      ul
        each topic in topics
          li
            - rid = encodeURIComponent(topic['@rid'])
            a(href='/topic/'+ rid)= topic.title
            // topic.title = topic['title']
    article
    form(action='/topic/'+encodeURIComponent(topic['@rid'])+'/edit' method='post')
      p
        input(type='text' name='title' placeholder='title' value=topic.title)
      p
        textarea(name='description' placeholder='description' )
          =topic.description
      p
        input(type='text' name='author' placeholder='author' value=topic.author)
      p
        input(type='submit' value='수정')
```

- delete.pug

```jade
doctype html
html
  head
    meta(charset='utf-8')
  body
    hi
      a(href='/topic') DELETE Server Side Javascript(OrientDB)
      ul
        each topic in topics
          li
            - rid = encodeURIComponent(topic['@rid'])
            a(href='/topic/'+ rid)= topic.title
            // topic.title = topic['title']
    article
    form(action='/topic/'+encodeURIComponent(topic['@rid'])+'/delete' method='post')
      p [#{topic.title}] Delete??
      p
        input(type='submit' value='삭제')
      p
        a(href='/topic/'+encodeURIComponent(topic['@rid'])) NO
``````