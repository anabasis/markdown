# WebPack

<https://github.com/joshua1988/LearnWebpack>

모듈 번들러
![모듈 번들러](./images/webpack.png)

기존 Web Task Manager(Gulp, Grunt)의 기능 + 모듈 의존성 관리

## 자바스크립트 Code based Modules 관리

- 자바스크립트 모듈화의 필요성 : AMD, Common JS, ES6(Modules)
- 기존 모듈 로더들과의 차이점 : 모듈 간의 관계를 청크(Chunk) 단위로 나눠 필요할 때 로딩
- 현대의 웹에서 JS역할이 커짐에 따라, Client Side에 들어가는 코드량이 많아지고 복잡해짐
- 복잡한 웹을 관리하기 위해 모듈 단위로 관리하는 Common JS, AMD, ES6 Modules등이 등장
- 가독성이나 다수 모듈 미병행 처리등의 약점을 보완하기 위해 Webpack이 등장

## 자바스크립트 모듈화 문제

```js
<script src="module1.js"></script>
<script src="module2.js"></script>
<script src="library1.js"></script>
<script src="module3.js"></script>
```

- 상기모듈로딩 방식의 문제점 : 전역변수 충돌, 스크립트 로딩 순서, 복잡도에 따른 관리상의 문제

## 철학

- Everything is Module (js,css,html 모두 모듈)
- Load only "what" you need and "when" you need(초기에 불필요한 것들을 로딩하지 않고, 필요할때 필요한 것만 로딩하여 사용)

## 설치

```bash
# 1. Webpack 설치
npm install webpack -g

# 2. 패키지 생성
npm init -y

# 3. index.html, index.js

# 4. lodash 추가
npm i lodash --save

# 5. package 추가
import _ from 'lodash';


```

## Entry, Output, Loader, Plugins, Resolve
