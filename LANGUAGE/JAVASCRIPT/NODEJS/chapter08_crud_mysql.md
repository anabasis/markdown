# NodeJS

## MySQL 연동

### 1. Download

### 2. 환경변수 등록

- MYSQL_HOME
- PATH : bin

### 3. 설정파일

- my-default.ini --> my.ini

```ini
basedir : MySQL이 설치된 경로
datadir : DB 데이터가 저장 될 경로
port : 일반적으로 3306을 이용.
```

```bash
$mysqld -initialize-insecure
```

### 4. Window에 MySQL서비스 등록

```bash
$mysqld --install
"Service successfully installed"
## 삭제
$mysqld --remove
```

## MariaDB 설치

- 인스톨파일 설치

## DB 생성

```sql
-- 데이터페이스 보기
show databases;

-- 데이터베이스 생성
create database o2;

-- 사용자 추가 , 패스워드 생성
create user node;
create user node@localhost identified by '1';
create user 'node'@'%' identified by '1';  -- 외부접근 아이디 생성
insert into user (host,user,password) values ('%','node',password('1'));

-- 사용자 삭제
drop user 'node';
delete from user where user = 'node';

-- 사용자 권한 부여
grant all privileges on o2.topic to node@'%' identified by '1';
grant select, insert, update on o2.topic to node@'%' identified by '1';
grant all privileges on o2.* to node@'%' identified by '1';


-- 변경된 권한을 적용
flush privileges;

-- 권한 삭제
revoke all on o2.topic from node@'%';

-- 권한 확인
show grants for node@'%'

-- 데이터베이스 사용
use o2;

-- 테이블 생성
CREATE TABLE `o2`.`topic` (
  `id` INT NOT NULL AUTO_INCREMENT,
  `title` VARCHAR(50) NOT NULL,
  `description` TEXT NOT NULL,
  `author` VARCHAR(50) NOT NULL,
  PRIMARY KEY (`id`));


-- 테이블 확인
show tables;

-- 데이터 입력
INSERT INTO topic (title, description, author) VALUES ('Javascript', 'Computer language for web', 'jscho');
INSERT INTO topic (title, description, author) VALUES ('NPM', 'NPM Package Manager', 'jscho');
INSERT INTO topic (title, description, author) VALUES ('JSON', 'JSON Computer language for web', 'jscho');
```

## Node MySQL 설치

```bash
npm install mysql --save
```

```json
"dependencies": {
    "mysql": "^2.15.0",
    ...
  }
```

## MySQL Example

- app_mysql.js

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
app.set('views', __dirname + '/view_mysql');

// Database 설정
var mysql = require('mysql');
var conn = mysql.createConnection({
  host:'localhost',
  user:'node',
  password:'1',
  database:'o2'
});
conn.connect();

// 신규생성 페이지
app.get('/topic/add', (req, res) => {
  var sql = 'SELECT * FROM topic';
  conn.query(sql, (error, topics, fields) => {
      if(error) throw error;
      res.render("add",{topics:topics});
  });
});

// 신규생성 페이지 저장
app.post('/topic/add', (req,res) => {
  var title = req.body.title;
  var description = req.body.description;
  var author = req.body.author;
  var sql = 'INSERT INTO topic (title, description, author) VALUES ( ?, ?, ?)';
  var params = [title, description, author];
  conn.query(sql, params, (error, result, fields) => {
    // result = {fieldCount, affectedRows,insertId,serverStatus,warningCount,message,protocol41,changedRows}
    if(error) {
      console.log(error);
      throw error;
    }else {
      console.log('/topic/'+encodeURIComponent(result.insertId));
      res.redirect('/topic/'+encodeURIComponent(result.insertId));
    }
  });
});

// 내용변경 페이지
app.get('/topic/:id/edit', (req,res) => {
  var sql = 'SELECT * FROM topic';
  conn.query(sql, (error, topics, fields) => {
    var id = req.params.id;
    if(id){
      var sql = 'SELECT * FROM topic WHERE id= ? ';
      conn.query(sql, [id] ,(error,topic,fields) => {
        if(error) {
          console.log(error);
          throw error;
        }else {
          res.render('edit',{topics:topics, topic:topic[0]});
        }
      });
    }else{
      console.log(error);
      res.status(500).send('Custom Internal Server Error');
    }
  });
});

// 내용변경 페이지 저장
app.post('/topic/:id/edit', (req,res) => {
  var sql = 'UPDATE topic SET title= ? , description= ? , author= ?  WHERE id= ? ';
  var id = req.params.id;
  var params = [req.body.title, req.body.description, req.body.author, req.params.id ];
    conn.query(sql, params, (error, results, fields) => {
      res.redirect('/topic/'+encodeURIComponent(id));
    });
});

// 삭제 페이지
app.get('/topic/:id/delete', (req,res) => {
  var sql = 'SELECT * FROM topic';
  var id = req.params.id;
  if(id){
    conn.query(sql, (error, topics, fields) => {
      var sql = 'SELECT * FROM topic WHERE id= ? ';
      conn.query(sql, [id], (error, topic, fields) => {
        res.render('delete',{topics:topics, topic:topic[0]});
      });
    });
  }
});

// 목록 및 상세 정보
app.get(['/topic','/topic/:id'], (req,res) => {
  var sql = "SELECT * FROM topic";
  conn.query(sql, (error, topics ,fields) => {
    if(error){
      console.log(error);
      throw error;
    } else {
      var id = req.params.id;
      if(id){
        var params = [id];
        var sql = 'SELECT * FROM topic WHERE id= ? ';
        conn.query(sql, params, (error, topic, fields) => {
          res.render('view',{topics:topics, topic:topic[0]});
        });
      } else {
        console.log(topics);
        res.render('view', {topics:topics});
      }
    }
  });
});

// 삭제처리 저장
app.post('/topic/:id/delete', (req,res) => {
  var sql = 'DELETE FROM topic WHERE id= ? ';
  var id = req.params.id;
  conn.query(sql, [id], (error, results, fields) => {
    if(error){
      console.log(error);
      throw error;
    } else {
      console.log(results);
      res.redirect('/topic/');
    }
  });
});

//conn.end();

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
      a(href='/topic') Server Side Javascript(MySQL)
    ul
      each topic in topics
        li
          a(href='/topic/'+ topic.id)= topic.title
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
          a(href='/topic/'+topic.id+'/edit') Edit
        li
          a(href='/topic/'+topic.id+'/delete') Delete
```

- add.pug

```jade
doctype html
html
  head
    meta(charset='utf-8')
  body
    hi
      a(href='/topic') ADD Server Side Javascript(MySQL)
      ul
        each topic in topics
          li
            a(href='/topic/'+ topic.id)= topic.title
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
      a(href='/topic') EDIT Server Side Javascript(MySQL)
      ul
        each topic in topics
          li
            a(href='/topic/'+ topic.id)= topic.title
    article
    form(action='/topic/'+encodeURIComponent(topic.id)+'/edit' method='post')
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
      a(href='/topic') DELETE Server Side Javascript(MySQL)
      ul
        each topic in topics
          li
            a(href='/topic/'+ topic.id)= topic.title
    article
    form(action='/topic/'+encodeURIComponent(topic.id)+'/delete' method='post')
    // 항상 UPDATE,DELETE는 post방식으로 해야 중복 Click이 안됨
      p [#{topic.title}] Delete??
      p
        input(type='submit' value='삭제')
      p
        a(href='/topic/'+encodeURIComponent(topic.id)) NO
```