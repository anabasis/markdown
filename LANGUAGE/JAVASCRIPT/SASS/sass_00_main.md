# SASS(Syntactically Awesome Style Sheets) : CSS pre-processor

<https://velopert.com/1712>
<http://wit.nts-corp.com/2015/01/09/2936>

## SCSS 컴파일

### RUBY SASS

```bash
gem install sass
sass style.scss style.css
```

### GUI 어플리케이션

Koala, Hammer, Compass

### libsass

### node-sass

node환경에 libsass사용

```bash
# NPM 을 통하여 node-sass 글로벌 설치
$ sudo npm install -g node-sass

# 컴파일하여 현재 디렉토리에 저장
$ node-sass style.scss -o .

# style.scss 파일에 변화가 있을 떄 마다 자동으로 리컴파일
$ node-sass style.scss -w -o .
```

### SASS를 CSS 변환 서비스 : [Sassmeister](http://www.sassmeister.com)

## Comment (주석)

- Sass의 주석이 CSS 와 다른점은 한 줄 주석이 추가
- 한 줄 주석은 // 로 표기하며,  CSS로 컴파일 되었을 때 나타나지 않음
- 여러 줄 주석은 CSS 와 동일하며 CSS 로 컴파일 되었을 때 나타남

```scss
//Sass
/* You can See me */
// You can't see me
/* You Can
   See Mee
*/

//CSS
/* You can See me */
/* You Can
   See Mee
*/
```