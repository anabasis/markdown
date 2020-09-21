# NodeJS

NodeJS API : <https://nodejs.org/dist/latest-v8.x/docs/api/>

## HTTP

- <https://nodejs.org/dist/latest-v8.x/docs/api/http.html>

```js
// 객체선언
const server = http.createServer((req, res) => {
    //로직구현
});
// 서버리슨
server.listen(port, hostname, () => {
  // 로직구현
});
```

```js
// 객체선언
const server = http.createServer( function(req, res) {
    //로직구현
});
// 서버리슨
server.listen( function(port, hostname, ()) {
  // 로직구현
});
```

### http.createServer([requestListener])

- requestListener \<Function\>
- Returns: [<http.Server>](https://nodejs.org/dist/latest-v8.x/docs/api/http.html#http_class_http_server)

The requestListener is a function which is automatically added to the 'request' event.

### server.listen

- http.Server > net.Server --> server.listen

<https://nodejs.org/dist/latest-v8.x/docs/api/net.html#net_server_listen>

### uglifyjs 패키지

```bash
npm install uglify-js -g

uglifyjs pretty.js
uglifyjs pretty.js -m  # --mangle (짖이기다)
uglifyjs pretty.js -o uglified.js -m  # --ouput FILE
uglifyjs pretty.js -o pretty.min.js -m  # min
```

### npm init

```bash
npm init  ## package.json 생성
# npm install upderscore  # 디펜던씨 체크 안함.
npm install upderscore --save # 디펜던씨 체크하고 package.json에 추가
```

```json
// package.json
"dependencies": {
    "underscore": "^1.8.3"
  }
```