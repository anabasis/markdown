# NodeJS

<http://expressjs.com/ko/4x/api.html#req.query>

```js
app.set('view engine', 'jade');
app.set('views', __dirname + '/views');

app.use(express.static(__dirname + '/public'));

app.get('/topic', (req,res) => {
  var topics = [
    'Javascript is ...',
    'NodeJS is ...',
    'Express is ...'
  ];
  var str = `
  <a href="/topic?id=0">Javascript</a><br>
  <a href="/topic?id=1">NodeJS</a><br>
  <a href="/topic?id=2">Express</a><br>
  `;
  var output = str + topic[req.query.id];
  res.send(output);  // 파라메터 전달
});

app.get('/param/:module_id/:topic_id', (req,res) => {
  res.json(req.params);
});
```

## Express

### Semantic URL

```js
// 유형 1
app.get('/topic/:id/:mode', (req,res) => {
  res.send(req.params.id+ ',' + req.params.mode);
});

// 유형 2
app.get('/topic/:id', (req,res) => {
  res.send(req.params.id);
});
```

### FROM

```jade
doctype html
html
  head
    meta(charset='utf-8')
  body
    form(action='/form_receiver' method='get')
      p
        input(type='text' name='title')
      p
        textarea(name='description')
      p
        input(type="submit")
```

```js
app.get('/form_receiver', (req,res) => {
  console.log("GET 호출");
  var title = req.query.title;
  var description = req.query.description;
  res.send(title + ',' + description + " GET");
});
```

### POST

```jade
doctype html
html
  head
    meta(charset='utf-8')
  body
    form(action='/form_receiver' method='post')
      p
        input(type='text' name='title')
      p
        textarea(name='description')
      p
        input(type="submit")
```

```js
app.post('/form_receiver', (req,res) => {
  console.log("POST 호출");
  var title = req.body.title;
  var description = req.body.description;
  res.send(title + ',' + description + " POST");
});
```