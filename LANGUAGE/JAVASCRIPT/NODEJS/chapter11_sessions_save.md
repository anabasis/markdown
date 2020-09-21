# NodeJS

## Sessions 정보저장

<https://www.npmjs.com/package/session-file-store>
<https://www.npmjs.com/package/express-mysql-session>
<https://www.npmjs.com/package/connect-oriento>

## Sessions 파일저장

### Sessions 파일저장 모듈 설치

```bash
npm install session-file-store --save
```

```json
"dependencies": {
    "session-file-store": "^1.2.0",
    ...
  }
```

### Sessions 파일저장 Example

```js
// 세션 선언 및 사용(파일저장)
var session = require('express-session');
var FileStore = require('session-file-store')(session);

//  __dirname + '/sessions' 쓰기권한
// Error: EPERM: operation not permitted, rename '.\sessions\odH7G5BqOmbXSU0hKNQnoDzat_O_I74s.json.2381778732' -> '.\sessions\odH7G5BqOmbXSU0hKNQnoDzat_O_I74s.json'
app.use(session({
  secret : 'keyboard cat',
  resave : false ,
  saveUninitialized : true,
  store : new FileStore({path:__dirname + '/sessions'})
}));
```

## Sessions MySQL 저장

### Sessions MySQL저장 모듈 설치

```bash
npm install express-mysql-session --save
```

```json
"dependencies": {
    "express-mysql-session": "^1.2.3",
    ...
  }
```

### Sessions MySQL저장 Example

```js
// 세션 선언 및 사용(파일저장)
var session = require('express-session');
var MySQLStore = require('express-mysql-session')(session);
app.use(session({
  secret : 'keyboard cat',
  resave : false ,
  saveUninitialized : true,
  store : new MySQLStore({
    host: 'localhost',
    port: 3306,
    user: 'node',
    password: '1',
    database: 'o2',
  })
}));
```

### Sessions MySQL Table

```sql
CREATE TABLE `sessions` (
  `session_id` varchar(128) CHARACTER SET utf8mb4 COLLATE utf8mb4_bin NOT NULL,
  `expires` int(11) unsigned NOT NULL,
  `data` text CHARACTER SET utf8mb4 COLLATE utf8mb4_bin,
  PRIMARY KEY (`session_id`)
) ENGINE=InnoDB DEFAULT CHARSET=utf8;
```

## Sessions OrintDB 저장

### Sessions OrintDB 저장 모듈 설치

```bash
npm install connect-oriento --save
```

```json
"dependencies": {
    "connect-oriento": "^0.1.4",
    ...
  }
```

### Sessions OrintDB 저장 Example

```js
// 세션 선언 및 사용(파일저장)
var session = require('express-session');
var OrientoStore = require('connect-oriento')(session);
app.use(session({
  secret : 'keyboard cat',
  resave : false ,
  saveUninitialized : true,
  store : new OrientoStore({
    server:'host=localhost&port=2424&username=root&password=11111&db=o2'
  })
}));
```

### Sessions OrintDB Table

- Schema > Vertex Class : Session 생성됨