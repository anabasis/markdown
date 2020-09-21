# OrientJS 패키지

- Node.js.에 대한 공식 orientdb 드라이버 빠르고 가볍고 바이너리 프로토콜을 사용

<https://www.npmjs.com/package/orientjs>

```json
"dependencies": {
    "orientjs": "^2.2.9"
  }
```

## OrientJS 설치

```bash
npm install orientjs --save
```

```js
var OrientDB = require('orientjs');
var server = OrientDB({
  host:'localhost',
  port:2424,
  username:'root',
  password:'11111'
});
var db = server.use('o2');
db.record.get('#20:0')
.then((record) =>{
  console.log('Loaded Record', record);
});
```
