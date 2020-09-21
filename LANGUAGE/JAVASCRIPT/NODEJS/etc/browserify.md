# Browserify

<https://ironhee.com/2015/06/17/browserify-%EC%86%8C%EA%B0%9C/>

안녕하세요! 오늘은 Javascript 코드를 깔쌈하게 빌드하는 방법에 대해서 얘기해 보려고 해요.
Browserify 는
Node.js 기반 javascript code 를 브라우저 환경에서도 실행 가능하도록 만들어주는 녀석이에요.
예를 들자면,

1
2
3
4
5
6
7
8
var path = require('path');
var foo = require('./helpers/foo')
 
// Some codes..
 
module.exports = function bar () {
  return 'bar'
};
이런 코드도 브라우저에서도 실행 가능하도록 만들어줘요.

반대로 생각해보자면 browser 에서 실행되는 코드를 node 스타일로 짤 수 있도록 해준다는거죠.
이건 정말 큰 장점인데 기존의 AMD 방식을 사용하게 되면, 코드를 AMD 스타일로 짜야 한다는 단점이 있어요.

1
2
3
4
5
6
7
8
9
define("alpha", ["require", "exports", "beta"],
  function (require, exports, beta) {
    exports.verb = function() {
      return beta.verb();
      //Or:
      return require("beta").verb();
    }
  }
);
이런 식으로요.

하지만 Browserify 는 Node.js 의 Module API 를 그대로 사용하기 때문에, 일단 node 코드로 작성하고 나중에 browserify 로 컴파일만 해주면
node 와 browser 환경에서 둘다 실행되요. 
또 끝내주는 특징 중 하나는, node 기반 package 를 그대로 사용할 수 있다는 부분이에요.
built-in package 들은 물론이고,
npm 으로 설치한 package 모두 컴파일을 통해 브라우저에서도 그대로 사용할 수 있어요.

1
2
3
var path = require('path');
var _ = require('underscore')
var foo = require('./foo')
이런 코드를 브라우저에서도 그대로 쓸 수 있다는거에요! 얏호!!

Bower 에서 받은 외부 라이브러리들도 browserify-shim 플러그인을 사용해서 그대로 사용할 수 있구요.

1
2
3
4
5
6
7
8
9
// package.json
// ...
"browserify": {
  "transform": ["browserify-shim"]
},
"browserify-shim": {
  "jquery": "global:$"
}
// ...
이런식으로요.

standalone, external 등 여러가지 옵션을 사용해서 라이브러리화 & 최적화도 가능해요!

Webpack 이라는 녀석도 있는데 이친구는 다음번에 알아보도록 해요.