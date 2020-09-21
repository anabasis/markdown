# fs(FileSystem) 패키지

<http://nodejs.org/api/fs.html>
<https://opentutorials.org/module/938/7373>
<http://mudchobo.tistory.com/542>

## fs(FileSystem) 모듈

- fs 모듈은 FileSystem의 약자로 파일 처리와 관련된 모듈
- 메소드가 굉장히 많은데 가장 중요하고 기초라고 할 수 있는 파일 읽기와 쓰기 기능

### 파일 읽기

- filename의 파일을 [options]의 방식으로 읽은 후 callback으로 전달된 함수를 호출(비동기적)

```js
- fs.readFile(filename, [options], callback)
```

- filename의 파일을 [options]의 방식으로 읽은 후 문자열을 반환합니다. (동기적)

```js
- fs.readFileSync(filename, [options])
```

- Sync가 붙은 것은 동기적 읽기, 붙지 않은 것은 비동기적 읽기
- 동기적 읽기로 읽게 되면 파일을 읽으면서 다른 작업을 동시에 할 수 없음.
- 하지만 비동기적으로 읽으면 파일을 읽으면서 다른 작업도 동시에 수행할 수 있고 파일을 다 읽으면 매개변수 callback으로 전달한 함수가 호출
- [options]에는 보통 인코딩 방식이 오게 되며 웹에서는 utf8을 주로 사용합니다.

```txt
fs 모듈
파일 입출력 연습
```

- 예제

```js
// readFile.js

var fs = require('fs');

// 동기적 읽기
var text = fs.readFileSync('text.txt', 'utf8');
console.log(text);

// 비동기적 읽기
fs.readFile('text.txt', 'utf8', function(err, data) {
    console.log(data);
});
```

- 실행 결과

```bash
$node readFile.js
fs 모듈
파일 입출력 연습
fs 모듈
파일 입출력 연습
```

- 비동기적 읽기에서 callback으로 전달된 함수는 매개변수로 err와 data를 갖음
- 파일로부터 읽은 데이터 내용이 매개변수 data로 전달되어 함수 내에서 접근
- err와 관련해서는 아래 예외처리 부분 참고

### 파일 쓰기

- filename의 파일에 [options]의 방식으로 data 내용을 쓴 후 callback 함수를 호출합니다. (비동기적)

```js
fs.writeFile(filename, data, [options], callback)
```

- filename의 파일에 [options]의 방식으로 data 내용을 씁니다. (동기적)

```js
fs.writeFileSync(filename, data, [options])
```

- 예제

```js
// writeFile.js
var fs = require('fs');
var data = 'Hello FileSystem';
fs.writeFile('text.txt', data, 'utf8', function(err) {
    console.log('비동기적 파일 쓰기 완료');
});

fs.writeFileSync('text2.txt', data, 'utf8');
console.log('동기적 파일 쓰기 완료');
```

- 실행 결과

```bash
$node writeFile.js
동기적 파일 쓰기 완료
비동기적 파일 쓰기 완료

$cat text.txt
Hello FileSystem

$cat text2.txt
Hello FileSystem
```

### 예외 처리

- 파일 입출력은 매우 다양한 원인으로 예외가 발생
- 권한이 없다거나 존재하지 않는 파일을 읽는다거나 심지어 하드디스크 용량을 초과할 수도 있음.
- 동기적인 방식과 비동기적인 방식에서 예외를 처리하는 방법이 조금 다름

#### 동기적 방식의 예외처리

동기적 방식에서는 자바스크립트의 일반적인 예외처리 방식인 try ~ catch 구문으로 처리합니다.

- 예제

```js
// exceptionHandleSync.js
var fs = require('fs');
// 파일 읽기
try {
    // 존재하지 않는 파일을 읽으려 합니다.
    var data = fs.readFileSync('nonExist.txt', 'utf8');
    console.log(data);
}
catch(err) {
    console.log(err);
}

// 파일 쓰기
try {
    // 루트 위치에 파일을 쓰려고 합니다.(권한 거부)
    fs.writeFileSync('/unauthorized.txt', 'Hello World', 'utf8');
    console.log('파일 쓰기 성공');
}
catch(err) {
    console.log(err);
}
```

- 실행 결과

```bash
$node exceptionHandleSync.js
{ [Error: ENOENT, no such file or directory 'nonExist.txt']
  errno: 34,
  code: 'ENOENT',
  path: 'nonExist.txt',
  syscall: 'open' }
{ [Error: EACCES, permission denied '/unauthorized.txt']
  errno: 3,
  code: 'EACCES',
  path: '/unauthorized.txt',
  syscall: 'open' }
```

#### 비동기적 방식의 예외처리

비동기적 방식에서 예외가 발생하면 callback 함수의 매개변수 err에 전달되므로 따로 try ~ catch 구문을 사용할 필요가 없음

- 예제

```javascript
// exceptionHandle.js
var fs = require('fs');

// 파일 읽기
// 존재하지 않는 파일을 읽으려 합니다.
fs.readFile('nonExist.txt', 'utf8', function(err, data) {
    if(err) {
        // 파일 읽기 실패
        console.log(err);
    }
    else {
        // 파일 읽기 성공
        console.log(data);
    }
});

// 파일 쓰기
// 루트 위치에 파일을 쓰려고 합니다.(권한 거부)
fs.writeFile('/unauthorized.txt', 'Hello World', 'utf8', function(err, data) {
    if(err) {
        // 파일 쓰기 실패
        console.log(err);
    }
    else {
        // 파일 쓰기 성공
        console.log('파일 쓰기 성공');
    }
});
```

- 실행 결과

```js
$node exceptionHandle.js
{ [Error: EACCES, open '/unauthorized.txt'] errno: 3, code: 'EACCES', path: '/unauthorized.txt' }
{ [Error: ENOENT, open 'nonExist.txt'] errno: 34, code: 'ENOENT', path: 'nonExist.txt' }
```

---------------------------------------------------------------------------

11. File system

이 챕터는 파일시스템 모듈에 대해서 설명한다.

파일시스템 함수들은 파일I/O와 디렉토리I/O함수에 관한 것이다. 모든 파일싀스템 함수들은 동기/비동기 함수를 제공한다. 이 둘의 차이점을 설명하면 동기는 값을 함수안에서 바로 리턴하며(함수이름에 Sync가 붙음), I/O작업이 수행하는 동안 다른 코드가 실행되는 것을 막는다.
var fs = require('fs');
var data = fs.readFileSync('./index.html', 'utf8');
// wait for the result, then use it
console.log(data);

비동기 함수는 리턴값을 콜백함수를 통해서 제공한다.
var fs = require('fs');
fs.readFile('./index.html', 'utf8', function(err, data) {
  // the data is passed to the callback in the second argument
  console.log(data);
});

아래 목록은 FS API의 비동기 함수들 전부이다. 이 함수들은 동기함수를 가지고 있지만, 읽기 쉽게 하기 위해 목록에서 뺐다.

Read & write a file (fully buffered)
fs.readFile(filename, [encoding], [callback])
fs.writeFile(filename, data, encoding='utf8', [callback])
Read & write a file (in parts)
fs.open(path, flags, [mode], [callback])
fs.read(fd, buffer, offset, length, position, [callback])
fs.write(fd, buffer, offset, length, position, [callback])
fs.fsync(fd, callback)
fs.truncate(fd, len, [callback])
fs.close(fd, [callback])
Directories: read, create & delete
fs.readdir(path, [callback])
fs.mkdir(path, mode, [callback])
fs.rmdir(path, [callback])
Files: info
fs.stat(path, [callback])
fs.lstat(path, [callback])
fs.fstat(fd, [callback])
fs.realpath(path, [callback])
Readable streams
fs.ReadStream
Event: 'open'
fs.createReadStream(path, [options])
Writable streams
fs.WriteStream
Event: 'open'
file.bytesWritten
fs.createWriteStream(path, [options])
Files: rename, watch changes & change timestamps
fs.rename(path1, path2, [callback])
fs.watchFile(filename, [options], listener)
fs.unwatchFile(filename)
fs.watch(filename, [options], listener)
fs.utimes(path, atime, mtime, callback)
fs.futimes(path, atime, mtime, callback)
Files: Owner and permissions
fs.chown(path, uid, gid, [callback])
fs.fchown(path, uid, gid, [callback])
fs.lchown(path, uid, gid, [callback])
fs.chmod(path, mode, [callback])
fs.fchmod(fd, mode, [callback])
fs.lchmod(fd, mode, [callback])
Files: symlinks
fs.link(srcpath, dstpath, [callback])
fs.symlink(linkdata, path, [callback])
fs.readlink(path, [callback])
fs.unlink(path, [callback])

아마도 주로 비동기 형태를 많이 사용하겠지만, 동기 형태가 더 적절한 케이스도 있다(예를 들어 서버 시작 시 설정 파일을 읽는 작업). 비동기 작업 시에는 작업이 시작되고 금방 끝날 수 있기 때문에 조금 더 생각을 하고 작업할 필요가 있다.
fs.readFile('./file.html', function (err, data) {
  // ...
});
fs.readFile('./other.html', function (err, data) {
  // ..
});

각각 파일을 읽는데 걸리는 시간이 순서에 상관없이 완료될 수 있다. 가장 쉬운 해결책으로 콜백체인 방식으로 해결할 수 있다.
fs.readFile('./file.html', function (err, data) {
   // ...
   fs.readFile('./other.html', function (err, data) {
      // ...
   });
});

그렇지만, control flow챕터에서 좀 더 나은 패턴을 연구해볼 필요가 있다.




11.1. 파일: 읽기 쓰기

한번에 읽고 쓰는 방법은 매우 간단하다. 함수 호출할 때 문자나 버퍼로 쓰고, 콜백 리턴값을 확인하면 된다.

레시피: 파일 읽기(Fully buffered)
fs.readFile('./index.html', 'utf8', function(err, data) {
  // the data is passed to the callback in the second argument
  console.log(data);
});

레시피: 파일 쓰기(Fully buffered)
fs.writeFile('./results.txt', 'Hello World', function(err) {
  if(err) throw err;
  console.log('File write completed');
});

파일의 일부의 쓰기 읽기 작업을 할 때에는 open()함수가 필요로 하는데, 이것을 통해 파일디스크립터를 얻고 이것과 함께 작업할 수 있다.

fs.open(path, flags, [mode], [callback]) 아래와 같은 flag를 지원한다.

'r' - 읽기로 열기. 파일이 존재하지 않으면 에러발생.
'r+' - 읽기/쓰기로 열기. 파일이 존재하지 않으면 에러발생.
'w' - 쓰기로 열기. 파일이 존재하지 않으면 만들어지고, 파일이 존재하면 지우고 처음부터 씀.
'w+' - 읽기/쓰기로 열기. 파일이 존재하지 않으면 만들어지고, 파일이 존재하면 처음부터 씀.
'a' - 추가 쓰기로 열기. 파일이 존재하지 않으면 만들어짐.
'a+' - 파일을 읽고/추가쓰기모드로 열기. 파일이 존재하지 않으면 만들어짐.

새로 생성되는 퍼미션에 대해서는 기본값 0666으로 지정됨.

레시피: 파일을 열고 쓰기를 한 뒤 닫기(in parts)
fs.open('./data/index.html', 'w', function(err, fd) {
  if(err) throw err;
  var buf = new Buffer('bbbbb\n');
  fs.write(fd, buf, 0, buf.length, null, function(err, written, buffer) {
    if(err) throw err;
    console.log(err, written, buffer);
    fs.close(fd, function() {
      console.log('Done');
    });
  });
});

read()와 write()함수는 버퍼에서 돌아가는 함수라서, 문자열을 new Buffer()로 생성했다. 명심할 것은 리턴값이 buffer객체로 리턴된다.

레시피: 파일 열고, 특정위치 탐색 후 읽고 닫기.
fs.open('./data/index.html', 'r', function(err, fd) {
    if(err) throw err;
    var str = new Buffer(3);
    fs.read(fd, buf, 0, buf.length, null, function(err, bytesRead, buffer) {
      if(err) throw err;
      console.log(err, bytesRead, buffer);
      fs.close(fd, function() {
        console.log('Done');
      });
    });
  });




11.2 디렉토리: 읽기 생성 삭제

레시피: 디렉토리 읽기

디렉토리에 있는 아이템(파일, 디렉토리 등)의 이름들을 리턴해준다.
var path = './data/';
fs.readdir(path, function (err, files) {
  if(err) throw err;
  files.forEach(function(file) {
    console.log(path+file);
    fs.stat(path+file, function(err, stats) {
      console.log(stats);
    });
  });
});

fs.stat()함수는 아이템에 대한 정보를 제공한다. stat객체는 아래와 같은 정보를 제공한다.
{ dev: 2114,
  ino: 48064969,
  mode: 33188,
  nlink: 1,
  uid: 85,
  gid: 100,
  rdev: 0,
  size: 527,
  blksize: 4096,
  blocks: 8,
  atime: Mon, 10 Oct 2011 23:24:11 GMT,
  mtime: Mon, 10 Oct 2011 23:24:11 GMT,
  ctime: Mon, 10 Oct 2011 23:24:11 GMT }

atime, mtime, ctime은 Date인스턴스다. stat객체는 또한 아래와 같은 함수를 제공한다.

stats.isFile()
stats.isDirectory()
stats.isBlockDevice()
stats.isCharacterDevice()
stats.isSymbolicLink() (only valid with fs.lstat())
stats.isFIFO()
stats.isSocket()

레시피: 디렉토리 생성과 삭제
fs.mkdir('./newdir', 0666, function(err) {
  if(err) throw err;
  console.log('Created newdir');
  fs.rmdir('./newdir', function(err) {
    if(err) throw err;
    console.log('Removed newdir');
  });
});

stream객체를 이용해서 읽고 쓰기

stream객체는 chapter9에서 설명한다.

레시피: 파일을 읽어서 다른 파일에 쓰기
var file = fs.createReadStream('./data/results.txt', {flags: 'r'} );
var out = fs.createWriteStream('./data/results2.txt', {flags: 'w'});
file.on('data', function(data) {
  console.log('data', data);
  out.write(data);
});
file.on('end', function() {
  console.log('end');
  out.end(function() {
    console.log('Finished writing to file');
    //test.done();
  });
});

다른 방법으로 pipe()함수를 이용해서 할 수 있다.
var file = fs.createReadStream('./data/results.txt', {flags: 'r'} );
var out = fs.createWriteStream('./data/results2.txt', {flags: 'w'});
file.pipe(out);

레시피: 파일에 추가하기
var file = fs.createWriteStream('./data/results.txt', {flags: 'a'} );
file.write('HELLO!\n');
file.end(function() {
  test.done();
});




연습용 예제

Node는 여러 파일에 대한 비동기 접근이 자주 일어나기 때문에 큰 용량의 파일I/O처리에 대해서 주의해야 한다. 사용 가능한 파일핸들을 다 써버릴 수도 있다(제한된 운영 시스템에서 파일을 사용하는 경우). 또한 비동기 처리로 동작하면서 끝나는 순서가 보장되지 않은 상황이 오기 때문에 이전 챕터인 CONTROL FLOW에서 실행 순서를 조정하는 패턴에 대해서 논의를 했었다. 아래 예를 보자.

예제: 특정 디렉토리를 재귀적으로 탐색해서 파일을 찾기

이 예제에서는 주어진 경로에 대해서 재귀적으로 파일을 검색할 것이다. 검색할 경로, 검색할 파일이름, 찾기가 끝났을 때 호출할 콜백함수, 이렇게 3개의 파라메터를 받는다.
var fs = require('fs');

function findFile(path, searchFile, callback) {
  fs.readdir(path, function (err, files) {
    if(err) { return callback(err); }
    files.forEach(function(file) {
      fs.stat(path+'/'+file, function(err, stats) {
        if(err) { return callback(err); }
        if(stats.isFile() && file == searchFile) {
          callback(undefined, path+'/'+file);
        } else if(stats.isDirectory()) {
          findFile(path+'/'+file, searchFile, callback);
        }
      });
    });
  });
}

findFile('./test', 'needle.txt', function(err, path) {
  if(err) { throw err; }
  console.log('Found file at: '+path);
});



함수를 분리하여 좀 이해하기 쉽게 만들었다.
var fs = require('fs');

function findFile(path, searchFile, callback){
    function isMatch(err, stats, file){
        if(err) { return callback(err); }
        if(stats.isFile() && file == searchFile) {
            callback(undefined, path + '/' + file);
        } else if (stats.isDirectory()){
            statDirectory(path + '/' + file, isMatch);
        }
    }
    statDirectory(path, isMatch);
}

function statDirectory(path, callback){
    fs.readdir(path, function(err, files){
        if(err) { return callback(err); }
        files.forEach(function(file){
            fs.stat(path + '/' + file, function(err, stats){
                callback(err, stats, file);
            });
        });
    });
}

findFile('./test', 'needle.txt', function(err, path){
    if(err) { throw err; }
    console.log('Found file at: ' + path);
});

함수를 작은 파트별로 분리했다.

findFile: 이 코드는 전체 프로세스의 시작점이며, 입력값에 의해서 결과값과 함께 콜백을 리턴한다.
isMatch: stat()함수의 결과값을 가져오는 함수이고, 매칭이 됐는지 체크하는 필수로직이 있는 부분이다.
statDirectory: 이 함수는 경로를 읽고, 각각의 파일의 stat정보를 파라메터로 받는 콜백함수로 리턴한다.

PathIterator: EventEmitter를 이용하여 재사용을 향상시키기

EventEmitter를 이용하여 디렉토리를 탐색하는 방법의 모듈을 생성해서 재사용하는 방식으로 만들 것이다.
var fs = require('fs'),
    EventEmitter = require('events').EventEmitter,
    util = require('util');

var PathIterator = function() { };

// augment with EventEmitter
util.inherits(PathIterator, EventEmitter);

// Iterate a path, emitting 'file' and 'directory' events.
PathIterator.prototype.iterate = function(path) {
    var self = this;
    this.statDirectory(path, function(fpath, stats) {
        if(stats.isFile()) {
            self.emit('file', fpath, stats);
        } else if(stats.isDirectory()) {
            self.emit('directory', fpath, stats);
            //self.iterate(path+'/'+file);
            self.iterate(fpath);
        }
    });
};

// Read and stat a directory
PathIterator.prototype.statDirectory = function(path, callback) {
    fs.readdir(path, function (err, files) {
        if(err) { self.emit('error', err); }
            files.forEach(function(file) {
            var fpath = path+'/'+file;
            console.log(fpath);
            fs.stat(fpath, function (err, stats) {
                if(err) { self.emit('error', err); }
                callback(fpath, stats);
            });
        });
    });
};
module.exports = PathIterator;

보시다시피 EventEmitter를 확장해서 새로운 클래스를 만들었다. 그리고, 아래와 같이 발생시킬 수 있는 이벤트를 정의했다.

error - function(error): 에러 발생 시 발생.
file - function(filepath, stats): 파일의 전체경로와 fs.stat()의 결과
directory - function(dirpath, stats): 디렉토리의 전체경로와 fs.stat()의 결과

이제 디렉토리를 탐색하기 위해 이 유틸리티 클래스를 사용할 수 있다.
var PathIterator = require('./pathiterator.js');
function findFile(path, searchFile, callback) {
    var pi = new PathIterator();
    pi.on('file', function(file, stats) {
        if(file.indexOf(searchFile) > 0) {
            callback(undefined, file);
        }
    });
    pi.on('error', callback);
    pi.iterate(path);
}

findFile('./test', 'needle.txt', function(err, path){
    if(err) { throw err; }
    console.log('Found file at: ' + path);
});

이 방식이 순수 콜백 방식보다 좀 더 나은 결과와 확장성을 보여준다(예를 들어 file이벤트에서 file목록을 쉽게 찾을 수 있다).

경로를 탐색하는 코드 작성을 완료했다면, PathIterator EventEmitter는 코드를 단순화시킬될 것이다. 넌블로킹I/O가 이벤트 루프를 통해서 콜백하는 것이 남아있지만, 인터페이스는 매우 이해하기 쉽게 되었을 것이다. 아마 findFile()함수를 프로세스 일부에 쓰일 것이라면, 같은 모듈로 일부 로직을 몰라도 경로를 탐색하는 코드를 작성할 수 있다.

특화된 모듈 사용: async.js

EventEmitter를 이용한 I/O처리의 캡슐화는 도움이 되었지만, 좀 더 나은 모듈인 fjacob의 async.js모듈을 사용하여서도 처리할 수 있다. 이것은 FileSystem모듈에 정의된 것을 체인 방식으로 잘 캡슐화한 것이다. async.js를 이용한 findFile()함수를 보자:
var async = require('asyncjs');

function findFile(path, searchFile, callback){
    async.readdir(path)
        .stat()
        .filter(function(file){
            if (!file.stat.isFile()){
                findFile(file.path, searchFile, callback);
                return false;
            }
            return file.name == searchFile;
        })
        .end(function(err, file){
            if (!file){
                return;
            }
            callback(err, file.path);
        });
}

findFile('./test', 'needle.txt', function(err, path){
    if(err) { throw err; }
    console.log('Found file at: ' + path);
});

async.js에서는 다양한 기술들로 구성되어 있지만(Control flow챕터 참조), 체인 인터페이스를 사용한 작동만 사용했다. 

포인트 - 노드에서 비동기I/O로 구성하는 것은 다른 환경이나 언어에서 보다 복잡할 수 있다. 이것은 파일 시스템을 다룰 때 매우 중요하다. 왜냐하면 어떤 것을 얻기 위해서는 긴 비동기 호출을 수행할 수 있기 때문이다.

그렇지만, 메인 코드로부터 더 나은 해결책을 찾아낼 가능성이 있다. 어떤 경우에는 매우 간결한 방법을 제공하는 라이브러리를 찾아서 처리할 수 있다(예를 들어 async.js 같은 것). 그리고, 다른 경우에는 모듈을 분리해서 프로세스의 파트를 나눠서 작업하는 경우도 있다(PathIterator 같은 것).
