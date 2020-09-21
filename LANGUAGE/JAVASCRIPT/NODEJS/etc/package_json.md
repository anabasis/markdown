# package.json

<http://programmingsummaries.tistory.com/385>
<https://outofbedlam.gitbooks.io/npm-handbook/content/config/package-json.html>

## name

- package.json에서 가장 중요한 항목이 name과 version 필드(필수항목)
- name과 version으로 유일하면서 중복되지 않는 식별자를 구성
- 팩키지가 변경되면 버전도 같이 변경되어야 함

### 규칙

- 이름은 반드시 214자 이하(스코프 팩키지의 스코프까지 포함)
- 점이나 밑줄로 시작하면 안됨
- 이름에 대문자가 포함되면 안됨
- 이름은 URL의 일부로 구성되거나 명령행의 인자나 폴더 이름으로 사용되므로 URL에 사용될 수 없는 문자를 포함할 수 없음

### 팁

- Node의 코어 모듈과 같은 이름을 사용하지 않음
- "js"나 "node"를 이름에 포함시키지 않도록 함
- "require()"를 호출할 때 이름이 파라미터로 전달될 것이므로, 짧지만 어떤 팩키지인지 쉽게 알아 볼 수 있는 이름이 좋음
- npm registry를 확인하여 중복된 이름이 있는지 미리 확인하는 것이 좋음
- 이름은 스코프로 프리픽를 지정(예: @myorg/mypackage. 자세한 사항은 npm-scope를 참조)

### version

- package.json에서 가장 중요한 항목이 name과 version 필드(필수항목)
- name과 version으로 유일하면서 중복되지 않는 식별자를 구성
- 팩키지가 변경되면 버전도 같이 변경

버전은 node-semver가 인식할 수 있어야 함
node-semver는 npm에 함께 포함되어 있음
(npm install semver를 통해서 사용)

더 자세한 사항은 semver를 참조하기 바랍니다.

### description

설명을 추가합니다. 이 설명은 다른 사람들이 npm search로 검색할때 이 팩키지를 찾을 수 있도록합니다.

### keywords

키워드를 추가합니다. 다른 사람들이 npm search로 검색할 수 있도록 합니다.

### homepage

프로젝트의 홈페이지 주소를 기록합니다.
주의: This is not the same as "url". If you put a "url" field, then the registry will think it's a redirection to your package that has been published somewhere else, and spit at you. Literally. Spit. I'm so not kidding.

### bugs

프로젝트의 이슈 트랙커의 주소나 이슈를 보고할 수 있는 이메일 주소를 기록합니다. 이 팩키지를 사용하는 사람들이 이슈를 등록할 수 있도록 합니다.

```json
{ "url" : "https://github.com/owner/project/issues"
, "email" : "project@hostname.com"
}
```

한가지 항목만 기록하거나 두 가지 값을 모두 쓸 수도 있습니다. 만약 url만을 지정하려면 "bugs" 항목에 객체 대신에 그 값을 직접 문자열로 기록하면 됩니다.

제공된 url은 npm bugs 명령에 사용됩니다.

### license

사용자들이 사용상의 허용범위를 확인할 수 있도록 팩키지의 라이센스를 지정해야합니다.

BSD-2-Clause나 MIT와 같은 일반적인 라이센스를 사용한다면 SPDX 라이센스 식별자를 사용하면 됩니다.

```json
{ "license" : "BSD-3-Clause" }
```

SPDX 라이센스 아이디의 전체 목록은 <https://spdx.org/licenses/>에서 확인할 수 있습니다.

복수의 일반 라이센스를 적용하려면 SPDX 문법을 따릅니다. 이 문법에 대해서는 <https://www.npmjs.com/package/spdx>를 참조합니다.

```json
{ "license" : "(ISC OR GPL-3.0)" }
```

SPDX에서 지정되지 않은 라이센스를 사용하거나 수정된 라이센스를 사용하려면 아래와 같은 문자열 값을 지정합니다.

```json
{ "license" : "SEE LICENSE IN <filename>" }
```

그리고  \<filename\> 에 지정된 파일을 팩키지의 최상위 폴더에 포함시킵니다.

비공개 팩키지를 다른 사람들이 어떠한 조건에서도 사용하지 못하도록 하려면 다음과 같이 지정합니다.

```json
{ "license": "UNLICENSED"}
```

이 경우에는  "private": true  를 설정하여 실수로 퍼블리쉬되지 않도록 방지하는 것이 좋습니다.

### people fields: author, contributors

"author"는 한 개인을 나타내고 "contributors"는 여러 사람들의 배열을 기록합니다. "person"은 "name" 필드를 가진 객체로써 "url"과 "email"필드는 옵션이며 다음의 예와같습니다.

```json
{ "name" : "Barney Rubble"
, "email" : "b@rubble.com"
, "url" : "http://barnyrubble.tumblr.com/"
}
```

또는 이 내용을 모두 단일 문자열로 아래와 같이 기록하면 npm이 각 내용을 파싱할 것입니다.

```json
"Barney Rubble <b@rubble.com> (http://barnyrubble.tumblr.com/)"
```

두 가지 방법 모두에서 email과 url은 옵셔널 필드입니다.

### files

"files" 필드는 프로젝트에 포함시킬 파일들의 배열입니다. 이 배열에 폴더 이름을 기록하면 (별도의 다른 규칙에 의해 제외하도록 하지 않았다면) 해당 폴더내의 모든 파일들이 포함됩니다.

팩키지 최상위 폴더나 하위 폴더에 ".npmignore" 파일이 있고 여기에 기록된 파일들은 files 배열에 포함되어 있다하더라고 팩키지에서 제외됩니다. .npmignore 파일은 .gitignore파일과 동일한 방법으로 작용합니다.

아래의 파일들은 설정에 관련없이 항상 포함됩니다.

- package.json
- README (및 여기에 해당되는 여러가지 변형파일들)
- CHANGELOG (와 여기에 해당되는 변형 파일들)
- LICENSE / LICENCE

여기에 대비해서 다음의 파일들은 항상 제외됩니다.

- .git
- CVS
- .svn
- .hg
- .lock-wscript
- .wafpickle-N
- *.swp
- .DS_Store
- ._*
- npm-debug.log

### main

main 필드는 프로그램의 시작 포인트를 가리키는 모듈 ID입니다. 팩키지의 이름이 foo 라고 한다면 이 팩키지를 설치한 사용자가 <b>require("foo")</b>를 호출하면 팩키지의 메인 모듈이 익스포트(export)한 객체를 반환받게 됩니다.

모듈 ID는 팩키지 폴더의 최상위를 기준으로한 상대 경로이어야합니다.

### bin

많은 팩키지들이 PATH에 추가되어야하는 하나 이상의 실행 파일을 포함하고 있습니다. npm은 이런 작업을 손쉽게 합니다. (실제로 npm 실행 파일 자체도 이 기능을 사용합니다.)

이 기능을 사용하려면 package.json내에 bin 필드를 작성하여 명령어 이름과 로컬 파일 이름과의 맵핑을 해야합니다. 설치과정에서 npm은 글로벌 설치일 경우 prefix/bin 내에, 로컬 설치의 경우 ./node_modules/.bin 폴더 내에 심볼릭 링크 파일을 생성합니다.

예를 들어 myapp에 다음 처럼 bin 필드를 작성하면

```json
{ "bin" : { "myapp" : "./cli.js" } }
```

myapp을 설치하면 npm은 cli.js 스크립트를 /usr/local/bin/myapp으로 심볼릭 링크를 생성합니다.

하나의 실행파일만 있고, 이 실행 파일의 이름이 팩키지 이름과 동일하다면 간단히 문자열로 지정할 수도 있습니다.

```json
{ "name": "my-program"
, "version": "1.2.5"
, "bin": "./path/to/program" }
```

이런 경우 실제로 다음과 동일한 결과입니다.

```json
{ "name": "my-program"
, "version": "1.2.5"
, "bin" : { "my-program" : "./path/to/program" } }
```

## man

man 프로그램이 찾을 수 있는 위치에 설치할 하나의 파일 이름이나 파일이름의 배열을 지정합니다.

하나의 파일만 지정하고 이 파일이 man 의 결과이어야 한다면 이 파일의 실제 파일명은 어떤 이름으든 상관이 없습니다. 예를 들어

```json
{ "name" : "foo"
, "version" : "1.2.3"
, "description" : "A packaged foo fooer for fooing foos"
, "main" : "foo.js"
, "man" : "./man/doc.1"
}
```

이 경우에  ./man/doc.1  파일은  man foo  의 대상이 됩니다.

만약 파일명이 아래와 같이 팩키지명으로 시작되지 않으면 팩키지명이 자동으로 접두어(prefix)로 붙게 됩니다.

```json
{ "name" : "foo"
, "version" : "1.2.3"
, "description" : "A packaged foo fooer for fooing foos"
, "main" : "foo.js"
, "man" : [ "./man/foo.1", "./man/bar.1" ]
}
```

이 경우 man foo와 man foo-bar에 대응하는 파일이 생성됩니다.

man 파일명은 숫자로 끝나야만 하며 옵션사항으로 압축된 파일의 경우 .gz 접미사(suffix)가 가능합니다. 이때 숫자는 이 파일이 설치되는 man 섹션을 뜻합니다.

```json
{ "name" : "foo"
, "version" : "1.2.3"
, "description" : "A packaged foo fooer for fooing foos"
, "main" : "foo.js"
, "man" : [ "./man/foo.1", "./man/foo.2" ]
}
```

이 경우는 man foo와 man 2 foo 엔트리를 생성합니다.

## directories

CommonJS 팩키지 스펙에 규정된 팩키지에 적용할 수 있는 다수의 directories 객체의 구조가 설명되어 있습니다.

### directories.lib

사용자들에게 라이브러라가 존재하는 장소를 알려줍니다. 특별한 작업을 수행하지 않으며 단순히 메타 정보를 나타냅니다.

### directories.bin

directories.bin에 bin 디렉터리를 지정하면 해당 폴더에 들어 있는 모든 파일들이 추가됩니다.

bin 지시자가 동작하는 방식 때문에 bin과 dictories.bin이 동시에 사용하는 것은 오류입니다. 개별 파일들을 각 각 지정하려면 bin을 사용하고 이미 존재하는 bin 디렉터리의 모든 파일을 추가하려면 directories.bin을 사용하도록 하십시오.

### directories.man

전체 man 페이지를 담고 있는 폴더입니다.

### directories.doc

마크다운(markdown) 파일을 저장하는 폴더입니다.

### directories.example

예제 스크립트를 저장하는 곳입니다.

## repository

코드를 저장하는 저장소를 지정합니다. 도움을 제공하고자하는 사람들에게 소스 저장소 위치를 알려주게 됩니다. 만약 이 레포지토리가 GitHub라면 npm docs 명령어가 이 저장소의 소유자를 찾을 수 있습니다.

아래와 같이 합니다.

```json
"repository" :
 { "type" : "git"
 , "url" : "https://github.com/npm/npm.git"
 }

"repository" :
 { "type" : "svn"
 , "url" : "https://v8.googlecode.com/svn/trunk/"
 }
```

이 URL은 (읽기 전용이라도) 공개되어 있어서(pulbic) VCS 프로그램에서 별도의 수정없이 접근 가능해야합니다. 브라우저에 입력하는 HTML로된 프로젝트 페이지 URL이 아닙니다.

GitHub, GitHub gist, Bitbucket, GitLab 레포지토리의 경우에는 npm install에 사용되는 단축 문법과 동일한 방식을 사용할 수 있습니다.

```json
"repository": "npm/npm"
"repository": "gist:11081aaa281"
"repository": "bitbucket:example/repo"
"repository": "gitlab:another/repo"
```

## scripts

"scripts" 프로퍼티는 딕셔너리 형태로 팩키지의 라이프 사이클상에서 다양한 순간에 실행되는 스크립트 명령어들을 담고 있습니다. 이때 키는 라이프사이클 이벤트이고, 값은 이 때 실행할 명령어입니다.

## config

"config" 객체를 통해서 팩키지 스크립트에서 사용되는 설정 파라미터들을 지정할 수 있습니다. 다음의 예를 들면...

```json
{ "name" : "foo"
, "config" : { "port" : "8080" } }
```

"start" 명령에서 npm_package_config_port를 참조한다고 가정하면, 이 값은 npm config set foo:port 8001 명령으로 오버라이드할 수 있습니다.

## dependencies

팩키지 이름과 버전 범위를 맵핑한 오브젝트를 통해서 의존성을 지정합니다. 버전 범위는 하나 또는 공백으로 구분되는 문자열들로 나타냅니다. 의존성은 압축파일 형태이거나 git URL로 식별됩니다.

여기에 시험목적이거나 코드 변환이나 컴파일을 위한 도구는 dependencies 객체에 포함시키지 말도록 하고, 대신에 devDependencies를 참조하기 바랍니다.

버전 범위를 지정하는 것이 자세한 사항들은 semver를 참조하기 바랍니다.

- version  : version에 정확히 일치해야합니다.
- \>version : 지정된 version보다 더 커야합니다.
- \>\=version
- \<version
- \<\=version
- \~version  : 해당 버전에 대략적으로 일치해야합니다.
- \^version  : 해당 버전과 호환이되어야 합니다.
- 1.2.x  : 1.2.0, 1.2.1 등등, 1.3.0은 안됩니다.
- <http://...>  : URL 의존성 참고
- \*  : 모든 버전와 매칭됩니다.
- ""  : (빈 문자열) \*과 동일합니다.
- version1 - version2  :  \>\=version1 \<\=version2 와 동일합니다.
- range1 || range2  : range1 또는 range2중에 하나를 만족하면 통과합니다.
- git...  : Git URL 의존성 참고
- tag  : 태그되었거나 tag로 퍼블리쉬된 버전을 지정합니다.
- path/path/path  : 로컬 경로를 참고하기 바랍니다.

아래의 예는 모두 유효한 dependencies 정의입니다.

```json
{ "dependencies" :
 { "foo" : "1.0.0 - 2.9999.9999"
 , "bar" : ">=1.0.2 <2.1.2"
 , "baz" : ">1.0.2 <=2.3.4"
 , "boo" : "2.0.1"
 , "qux" : "<1.0.0 || >=2.3.1 <2.4.5 || >=2.5.2 <3.0.0"
 , "asd" : "http://asdf.com/asdf.tar.gz"
 , "til" : "~1.2"
 , "elf" : "~1.2.3"
 , "two" : "2.x"
 , "thr" : "3.3.x"
 , "lat" : "latest"
 , "dyl" : "file:../dyl"
 }
}
```

## URL 의존성

버전 범위 대신에 압축 파일의 URL을 지정할 수 있습니다. 이 압축파일은 팩키지가 인스톨될 때 다운로드되어 함께 로컬에 설치될 것입니다.

## Git URL 의존성

Git URL은 다음과 같은 형태입니다.

```git
git://github.com/user/project.git#commit-ish
git+ssh://user@hostname:project.git#commit-ish
git+ssh://user@hostname/project.git#commit-ish
git+http://user@hostname/project/blah.git#commit-ish
git+https://user@hostname/project/blah.git#commit-ish
```

여기서 commit-ish 부분은  git checkout 의 인자가 될 수 있는 어떤 태그나 브랜치가 될 수도 있습니다. 디폴트는 master입니다.

## GitHub URL

버전 1.1.65부터 GitHub URL을 `"foo":"user/foo-project"와 같은 간단한 방법으로 참조할 수 있게 되었습니다. Git URL에서의 commit-ish와 같은 방법이 그대로 적용됩니다.

```json
{
 "name": "foo",
 "version": "0.0.0",
 "dependencies": {
 "express": "visionmedia/express",
 "mocha": "visionmedia/mocha#4727d357ea"
 }
}
```

### 로컬 경로

버전 2.0.0에서 부터 팩키지를 저장하고 있는 로컬 디렉터리 경로를 사용할 수 있습니다. 로컬 경로는 npm install -S 또는 npm install --save 명령을 통해서 저장될 수 있습니다. 아래와 같은 형태로 사용가능합니다.

```bash
../foo/bar
~/foo/bar
./foo/bar
/foo/bar
```

어떤 방법이든 경로는 상대경로로 노말라이즈 되어 package.json에 추가됩니다.

```json
{
 "name": "baz",
 "dependencies": {
 "bar": "file:../foo/bar"
 }
}
```

이 기능은 공개 레지스트리에 퍼블리쉬하면 안되는 팩키지를 사용해서 로컬에서 오프라인상태로 개발이 가능하도록 합니다.

## devDependencies

어떤 사람이 우리의 모듈을 이용하여 다른 프로그램을 작성할 때 우리 모듈에서 사용하고 있는 외부 테스트 프레임워크나 문서화 프레임워크를 같이 다운로드 받고 싶지 않을 것입니다. 이러한 프레임워크는 우리 모듈을 개발할 때만 필요한 것이지 우리 모듈을 사용할 때에는 필요하지 않기 때문입니다.

이런 경우, 이런 추가적인 항목들을 devDependencies 객체에 넣는게 최선입니다.

이러한 추가 항목들은 팩키지의 최상위 경로에서 npm link 또는 npm install을 실행할 때 설치되며, 다른 npm 설정 파라미터와 동일한 방법으로 다뤄집니다.

CoffeeScript와 같은 JavaScript로 컴파일되는 플랫폼 독립적인 빌드 과정들은 prepublish 스크립트를 이용하도록 하며 필요한 팩키지들은 devDependency에 추가합니다.

```json
{ "name": "ethopia-waza",
 "description": "a delightfully fruity coffee varietal",
 "version": "1.2.3",
 "devDependencies": {
 "coffee-script": "~1.6.3"
 },
 "scripts": {
 "prepublish": "coffee -o lib/ -c src/waza.coffee"
 },
 "main": "lib/waza.js"
}
```

prepublish 스크립트는 퍼블리쉬되기 전에 실행되므로 퍼블리쉬하기 전에 매번 직접 컴파일하지 않아도 됩니다. 개발 모드 (npm install을 로컬에서 실행하는)에서도, 이 스크립트가 실행되므로 쉽게 테스트할 수 있습니다.

## peerDependencies

## bundledDependencies

## optionalDependencies

## engines

호환되는 node의 버전을 지정할 수 있습니다.

```json
{ "engines" : { "node" : ">=0.10.3 <0.12" } }
```

dependencies처럼 버전을 지정하지 않거나 (또는  * 을 지정하면) 모든 버전의 node를 뜻합니다.

"engines" 필드를 지정하면 npm은 "node"가 리스트에 반드시 포함되어야합니다. "engine"이 생략되면 npm은 디폴트로 node를 사용하는 것으로 가정합니다.

"engines"필드를 통해서 우리의 프로그램이 정상적으로 동작할 수 있는 npm의 버전을 지정할 수도 있습니다.

```json
{ "engines" : { "npm" : "~1.0.20" } }
```

사용자가 engine_strict 설정 플래그를 지정하지 않는한 이 필드는 참고사항으로만 사용됩니다.

## engineStrict

이 기능은 npm 3.0.0에서 디프리케이트 되었습니다.

npm 3.0.0 이전에는 사용자가 engine-strict 플래그를 지정한 것과 동일하게 간주되었습니다.

## os

우리의 모듈이 동작할 수 있는 운영 체제를 지정할 수 있습니다.

```json
"os" : [ "darwin", "linux" ]
```

운영체제를 화이트리스트 방식 대신에  !  문자를 사용해서 블랙리스트 방식으로도 지정할 수 있습니다.

```json
"os" : [ "!win32" ]
```

호스트의 운영 체제는 process.platform에 의해서 결정됩니다.

## cpu

동작 가능한 cpu 아키텍쳐를 지정합니다.

```json
"cpu" : [ "x64", "ia32" ]
```

os 옵션과 마찬가지로 블랙리스트 방식이 가능합니다.

```json
"cpu" : [ "!arm", "!mips" ]
```

호스트의 아키텍쳐는 process.arch로 결정됩니다.

## preferGlobal

작성하는 팩키지가 명령행 애플리케이션으로 반드시 글로벌로 설치되어야만 한다면, 이 값을 true로 설정해서 로컬로 설치되는 경우에 경고를 출력할 수 있습니다.

이 것을 설정한다고 사용자가 로컬로 설치하는 것을 금지할 수는 없지만 혼란이 발생하는 것을 방지할 수는 있습니다.

## private

package.json에서 이 값을  "private": true 로 설정하면 npm이 이 팩키지를 퍼블쉬할 수 없게됩니다.

프라이빗 레포지토리를 실수로 공개하는 것을 방지할 수 있습니다. 만약 이 팩키지가 특정 레지스트리 (예를 들어 회사 내부의 레지스트리)에만 퍼블리쉬되어야한다면 publishConfig 딕셔너리의 registry 설정 파라미터를 사용해야합니다.

## publishConfig

퍼블리쉬 과정에서 사용되는 설정 값들입니다. 태그나 레지스트리와 접근 권한을 지정할 때 사용하여 팩키지가 디폴트로 "latest"로 태그 되거나, 글로벌 공개 레지스트리로 퍼블리쉬되거나, 스코프 지정된 모듈이 프라이빗이 되는 것을 방지할 수 있습니다.

어떠한 config 값들도 오버라이드할 수 있으나, "tag", "registry", "access" 가 퍼블리쉬 과정에서 가장 중요한 사항일 것입니다.

## 디폴트 값들

npm은 팩키지의 내용물에 따라 디폴트 값을 사용합니다.

- "scripts": {"start": "node server.js"}

server.js 파일이 팩키지의 최상위 경로에 존재하면 npm은 start 명령으로  node server.js 를 디폴트로 사용합니다.

- "scripts":{"preinstall": "node-gyp rebuild"}

binding.gyp 파일이 팩키지 최상위 경로에 존재하면 npm은 preinstall 명령에 node-gyp을 사용하여 컴파일합니다.

- "contributors": [...]

AUTHORS 파일이 팩키지 최상위 경로에 존재하면 npm은 파일의 각 라인을 Name  (url) 형태로 간주합니다. #으로 시작하는 라인이나 공백라인은 무시합니다.