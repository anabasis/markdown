# MySQL 패키지

- Node.js.에 대한 공식 MySQL드라이버

<https://github.com/mysqljs/mysql>

```json
"dependencies": {
    "mysql": "^2.15.0",
    ...
  }
```

## MySQL 설치

```bash
npm install mysql --save
```

```js
var mysql = require('mysql');
var connection = mysql.createConnection({
  host:'localhost',
  username:'node',
  password:'1',
  database:'o2'
});
connection.connect();

connection.query('', (error, rows, fields) => {
    if(error) throw error;
    console.log('The Solution is : ', rows[0].solution);
});

connection.end();
```
