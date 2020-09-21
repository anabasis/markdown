# NODE 개발환경 <https://steemit.com/kr-dev/@nhj12311/nodejs-github>

1. NODE설치
2. ATOM설치 <https://atom.io/>
3. Github 가입 <https://github.com/>
4. Node.js개발을 위한 Atom 설정과 Github 연동
    (윈도우 서버 설정 <https://git-scm.com/download/win>)
5. git plus, git plus toolbar github-plus git-control 설치

ATOM 명령어 실행 Ctrl+Shift+P

## ATOM GIT package 구성

git plus, git plus toolbar github-plus git-control

## ATOM NODE package 구성

atom-runner, linter, linter-csslint, linter-jshint

---------------------------------------------------

ATOM - Markdown(<http://futurecreator.github.io/2016/06/14/atom-as-markdown-editor/>)

ATOM install

- markdown-preview-enhanced
- markdown-format

ATOM Thems
(<http://atomthemes.io/>)

---------------------------------------------------

## W104 - 'const' is available in ES6 (JSHint Warning발생할 때)

JSHint Option <http://jshint.com/docs/options/#esversion>

### ATOM

- 각소스에 주석넣기

  ```js
  /*jshint esversion: 6 */
  const Suites = {
      Spade: 1,
      Heart: 2,
      Diamond: 3,
      Club: 4
  };
  ```

- 프로젝트 ROOT에 .jshintrc넣기

  ```json
  {
      "esversion": 6
  }
  ```

### VSCODE

- User Settings, JSHint config, "jshint.options":{} 수정

  ```json
  "jshint.options":{"esversion":6}
  ```

## 환경변수

- NodeJS는 환경변수로 Path만 잡아주면 됨.

## ATOM node.js 실행

- atom-runner : Alt+R

- File - Config

```yml
'runner':
    'scopes':
      'coffee': 'coffee'
      'js': 'node'
      'ruby': 'ruby'
      'python': 'python'
      'go': 'go run'
      'shell': 'bash'
      'powershell': 'powershell -noninteractive -noprofile -c -'
    'extensions':
      'spec.coffee': 'mocha'
      'ps1': 'powershell –file'
```
