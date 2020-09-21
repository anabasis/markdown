# NodeJS

## Password 보안

<https://www.npmjs.com/package/md5>
<https://www.npmjs.com/package/sha256>
<https://www.npmjs.com/package/pbkdf2-password>

## Password md5 보안

### Password md5 보안 모듈 설치

```bash
npm install md5 --save
```

```json
"dependencies": {
    "md5": "^2.2.1",
    ...
  }
```

### Password md5 보안 Example

```bash
node
var md5 = require('md5');
md5('11111');
'b0baee9d279d34fa1dfd71aadb908c3f'
```

```js
// MD5 설정
var md5 = require('md5');
...
app.post('/auth/login', (req, res) => {
  var user ={
    username : 'node',
    password : 'b0baee9d279d34fa1dfd71aadb908c3f',  // '11111'
    displayName : 'Node Master'
  };
  ...
  if(uname === user.username && md5(pwd) === user.password){
    ...
  }else{
    ...
  }
});
```

### Password md5 보안 Salt Example

## Password sha256 보안

### Password sha256 보안 모듈 설치

```bash
npm install sha256 --save
```

```json
"dependencies": {
    "sha256": "^0.2.0",
    ...
  }
```

### Password sha256 보안 Example

```bash
node
var sha256 = require('sha256');
sha256('11111');
'b0baee9d279d34fa1dfd71aadb908c3f'
```

```js
// MD5 설정
var sha256 = require('sha256');
...
app.post('/auth/login', (req, res) => {
  var user ={
    username : 'node',
    password : 'b0baee9d279d34fa1dfd71aadb908c3f',  // '11111'
    displayName : 'Node Master'
  };
  ...
  if(uname === user.username && sha256(pwd) === user.password){
    ...
  }else{
    ...
  }
});
```

## Password pbkdf2 보안

### Password pbkdf2 보안 모듈 설치

```bash
npm install pbkdf2-password --save
```

```json
"dependencies": {
    "pbkdf2-password": "^1.2.1",
    ...
  }
```

### Password pbkdf2 보안 Example

```bash
node
var kbdf2Password = require('pbkdf2-password');
var hasher = kbfd2Password();
hasher({password:'11111'}, (error, pass, salt, hash ) => {

});
```

```js

```
