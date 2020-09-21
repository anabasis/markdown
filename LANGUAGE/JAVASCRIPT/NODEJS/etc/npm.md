# NPM

## npm(Node Package Manager)

```bash
# 버젼확인
npm --version

# 최신버젼 업그레이드 -g 글로벌(--global)
sudo npm install npm -g

# npm install <모듈 이름>
# var express = require('express');

# 모듈 제거
sudo npm uninstall express

# 모듈 업데이트
sudo npm update express

# 모듈 검색
sudo npm search express

# NODE설치
sudo npm install -g n
sudo n stable
```

- npm help : 메인 설명
- npm <명령어> -h : 명령어에 대한 설명
- npm list 또는 npm ls : npm에서 관리되는 모듈 리스트
- npm view <패키지명> : 모듈의 버전별 상세정보
- npm install <패키지명> : 모듈을 찾아서 설치
- npm list installed : 설치되어 있는 모듈의 목록을 봄
- npm update <패키지명> : 설치된 패키지를 최신버전으로 업데이트

## 의존성 명시

- --save : 패키지를 설치하고 package.json의 dependencies항목에 설치한 패키지 이름과 버젼을 명시
- --save-dev : 패키지를 설치하고 package.json의 devDependencies항목에 설치한 패키지의 이름과 버젼을 명시

## package.json 생성

```bash
npm init [--yes]
# --yes 모든값을 기본값으로 채움
# 대화형 인터페이스
```

## 설치관련

```bash
npm init #
npm install 패키지@버젼 # --save(-S), --save-dev(-D)
npm install 주소 # Github주소
npm update # 패키지 업데이트
npm dedupe # 중복패키지 정리
npm docs # 패키지에 대한 설명(홈페이지)
```

[NPM global Path 변경](https://stackoverflow.com/questions/19874582/change-default-global-installation-directory-for-node-js-modules-in-windows)

## 조회관련

```bash
npm root # node_module 위치 선언
npm outdated # 오래된 패키지 알려줌.(package.json 일치하면 빨간색, 일치하지 않으면 녹새)
npm ls 패키지명 # 패키지 조회, 트리구조 dependencies
npm ll 패키지명 # 패키지 상세조회
npm search # npm 저장소에서 패키지를 검색(홈페이지)
npm owner # 패키지 주인 검색
npm bugs # 버그 발생시에 주인 연락방법(주로 Github issues탭 오픈)
```

## 로그인 관련

```bash
npm adduser # 회원가입 명령, npm login
npm logout # 로그아웃 명령
npm whoami # 자신의 아이디
```

## 퍼블리싱 관련(로그인 필요)

```bash
npm publish # 패키지 퍼블리쉬하거나 업데이트(참고 .gitignore,.npmignore)
npm deprecate # 치명적인 버그등일때 deprecate
npm unpublish # 퍼블리쉬 취소
npm star # 좋아하는 패키지 표시
npm stars # star 목록
npm version # npm 버젼
```

## 실행관련

```bash
npm start # package.json의 start명령어 실행, (default) node server.js
npm stop # 로그아웃 명령
npm restart # 자신의 아이디
npm test # 자신의 아이디
npm run # script를 실행하는 명령어
```

```json
# package.json
"scripts": {
    "compile": "babel lib -d build --presets env",
    "lint": "eslint lib/**/*.js test/**/*.js --ignore-path .gitignore",
    "test": "npm run compile && mocha --require babel-register",
    "coverage": "istanbul cover mocha -- --require babel-register"
}
```

```bash
npm run compile
```

## 설정관련

```bash
npm cache # cache를 보여줌. npm cache clean
npm rebuild # npm 재설치(npm cache clean 한 이후)
npm config # npm config list, npm set 이름 값, npm get 이름
```

--------------------------------------------------------

## Node.js 업그레이드

```bash
$ node -v
# 강제로 캐시 삭제
$ sudo npm cache clean -f
# npm WARN using --force I sure hope you know what you are doing.

# n 모듈 설치
$ sudo npm install -g n
# n 모듈을 사용하여 Nodejs 설치 (다른 버전의 Node.js 를 설치하려면 sudo n 5.11.0 이런식으로 명령어를 입력하면 됩니다)
$ sudo n stable
$ node -v
v6.0.0

$ sudo ln -sf /usr/local/n/versions/node/6.0.0/bin/node /usr/bin/node
```

## NPM 업그레이드

```bash
$ npm -v
# npm 으로 npm 설치 (인셉션..)
$ sudo npm install -g npm
/usr/local/bin/npm -> /usr/local/lib/node_modules/npm/bin/npm-cli.js
# 새로운 npm 버전 확인하기
$ npm -v
```

## Can't find Python executable "python" after installing

```bash
npm install -g --production windows-build-tools
```