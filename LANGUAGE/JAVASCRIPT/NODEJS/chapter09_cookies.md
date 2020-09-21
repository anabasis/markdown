# NodeJS

## Cookies

<http://expressjs.com/ko/4x/api.html#req.cookies>
<http://expressjs.com/en/4x/api.html#req.signedCookies>

## Cookies 설치

```bash
npm install cookie-parser --save
```

```json
"dependencies": {
    "cookie-parser": "^1.4.3",
    ...
  }
```

## Cookies Example

```js
var express = require('express');
var app = express();

// Cookie Parser 선언
var cookieParser = require('cookie-parser');
// 암호화 키 2345678anabasis
app.use(cookieParser('2345678anabasis'));

var products = {
  1:{title:'The History of Web 1 '},
  2:{title:'The Next of Web 2 '},
  3:{title:'The Third of Web 3 '}
};

app.get('/products', (req, res) => {
  var output = '';
  for(var name in products){
    output += `<li><a href="/cart/${name}">${products[name].title}</a></li>`;
  }
  res.send(`<h1>Products</h1><ul>${output}</ul><a href="/cart">Cart</a>`);
});

app.get('/cart', (req,res) => {
  var cart = req.signedCookies.cart;
  if(!cart){
    res.send('Empty');
  }else{
    var output = '';
    for(var id in cart){
      output += `<li>${products[id].title} (${cart[id]}) </li>`;
    }
    res.send(`
      <h1>Cart</h1>
      <ul>${output}</ul>
      <a href="/products">Product List</a>`);
  }
});

app.get('/cart/:id', (req,res) => {
  var id = req.params.id;
  var cart;
  // req.signedCookies.cart 쿠키 읽기
  if(req.signedCookies.cart)
    cart = req.signedCookies.cart;
  else
    cart = {};

  if(!cart[id]) cart[id] = 0;
  cart[id] = parseInt(cart[id]) + 1;  // 쿠키는 스트링

  // 쿠키 설정
  res.cookie('cart', cart, {signed:true});
  res.redirect('/cart'); //res.send(JSON.stringify(cart));
});

app.get('/count', (req, res) => {
  var count;
  if(req.signedCookies.count){
    count = parseInt(req.signedCookies.count);
  }else{
    count = 0;
  }
  count = count + 1;

  // 쿠키 설정
  res.cookie('count', count, {signed:true});
  res.send('count : ' + count);
});

// 미들웨어 기동
app.listen(3000,(req,res) => {
  console.log('Cookie Connected listen on port 3000 !!!');
});

```