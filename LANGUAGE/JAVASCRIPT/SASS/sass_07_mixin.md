# Mixin (믹스인)

- Mixin 은 Sass 의 아주 유용한 기능 중 하나인데요, extend 와 비슷하지만 argument (인수) 를 받을 수 있음
- mixin 을 선언 할 떄는 @mixin directive 를 사용하며, 이를 사용 할 때는 @include directive 를 사용

```scss
/* SCSS */
@mixin headline ($color, $size) {
  color: $color;
  font-size: $size;
}
h1 {
  @include headline(green, 12px);
}
```

```css
/* CSS */
h1 {
  color: green;
  font-size: 12px;
}
```

Mixin 을 응용하면 이런식으로도 사용 가능

```scss
/* SCSS */
@mixin media($queryString){
    @media #{$queryString} {
      @content;
    }
}
.container {
    width: 900px;
    @include media("(max-width: 767px)"){
        width: 100%;
    }
}
```

```css
/* CSS */
.container {
  width: 900px;
}
@media (max-width: 767px) {
  .container {
    width: 100%;
  }
}
```

\#{ } 표현은 특정 문자열을 따로 처리하지않고 그대로 출력 할 때 사용됩니다.
@content directive 를 사용하면 나중에 @include 하였을 때, 그 선택자 내부의 내용들이 @conent 부분에 나타나게 됩니다.

## 아래와 같은 코드를 보신적이 있을겁니다

```css
.ellipsis{overflow:hidden;white-space:nowrap;text-overflow:ellipsis}
.title{color:#1b1f2a} 
.btn{display:inline-block;overflow:hidden;width:12px;height:8px; ... line-height:999px;vertical-align:top} 
.ico{display:inline-block;overflow:hidden;width:4px;height:4px; ... line-height:999px;vertical-align:top}
```

반복되는 말줄임 적용 스타일을 클래스로 만들어놓고 필요한 위치에서 클래스를 추가로 넣어주거나, background-image가 들어가는 각각의 요소에 글자를 숨기기 위해 일부 동일한 속성을 반복해서 써야하는 번거로움.

sass에서는 mixin기능을 이용하여 이런 번거로움을 줄일 수 있도록 해줍니다.

* 작성방법

선언 – @mixin mixin명{}
호출 – @include mixin명{}

<table>
<tr><td>SCSS</td><td>CSS</td></tr>
<tr><td>

```scss
$overflow_hid:hidden;
@mixin ellipsis{
  overflow:$overflow_hid;
  white-space:nowrap;
  text-overflow:ellipsis;
}
@mixin hide_txt{
  display:inline-block;
  overflow:$overflow_hid;
  line-height:9999px;
  vertical-align:top;
}
.title{
  @include ellipsis;
  color:#1b1f2a;
}
.sub_tit{
  @include ellipsis;
}
.btn{
  @include hide_txt;
  width:12px;
  height:8px;
}
.ico{
  width:4px;
  height:4px;
  @include hide_txt;
}
```

</td><td>

```css
.title {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  color: #1b1f2a;
}
.sub_tit {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
}
.btn {
  display: inline-block;
  overflow: hidden;
  line-height: 9999px;
  vertical-align: top;
  width: 12px;
  height: 8px;
}
.ico {
  width: 4px;
  height: 4px;
  display: inline-block;
  overflow: hidden;
  line-height: 9999px;
  vertical-align: top;
}
```
</td></tr>
</table>

예제를 보면, mixin기능을 사용하여 말줄임과 숨김텍스트 기능을 상단에 선언해놓고, 실제로 적용되는 style 코드에서는 include로 mixin명만 호출하였습니다. 이렇게 하면, Sass는 상단에 선언된 속성들을 불러와 css로 출력을 해줘서 반복되는 코드를 여러번 작성하는 번거로움을 덜어줍니다! 그리고 .btn과 .ico를 비교해보시면 서로 mixin을 호출한 위치에서 출력을 해주는 것을 알 수 있습니다

또한 mixin은 특정 속성값을 인자로 지정하여 여러가지의 스타일을 만들 수도 있습니다!

* 작성방법

선언 – @mixin mixin명(변수명){}
호출 – @include mixin명(변수명){}

<table>
<tr><td>SCSS</td><td>CSS</td></tr>
<tr><td>

```scss
@mixin title_style($size, $color){
  overflow:hidden;
  white-space:nowrap;
  text-overflow:ellipsis;
  font-size:$size;
  color:$color;
}
h1{
  @include title_style(16px, #000)
}
```

</td><td>

```css
h1 {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  font-size: 16px;
  color: #000;
}
```
</td></tr>
</table>

* 인자값은 하나만 넣을수도 있고, (,)쉼표구분으로 여러개를 넣을 수도 있습니다.

mixin을 선언할 때 괄호안에 변수로 인자를 지정하고, 적용되는 코드에서 필요한 값을 넣어주면 sass는 전달받은 인자값으로 css를 호출해주게 됩니다!
이렇게하면, 동일한 스타일의 하나의 값 또는 몇개의 다른 값을 가지는 것에 대해서 좀 더 편리하게 작성할 수 있습니다.

또, 인자에 기본값을 지정할 수도 있습니다 . *기본값은 css처럼 (:)콜론으로 정의합니다.

<table>
<tr><td>SCSS</td><td>CSS</td></tr>
<tr><td>

```scss
@mixin title_style($size, $color:#080){
  overflow:hidden;
  white-space:nowrap;
  text-overflow:ellipsis;
  font-size:$size;
  color:$color;
}
h1{
  @include title_style(16px)
}
h2{
  @include title_style(14px, #333)
}
```

</td><td>

```css
h1 {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  font-size: 16px;
  color: #080;
}
h2 {
  overflow: hidden;
  white-space: nowrap;
  text-overflow: ellipsis;
  font-size: 14px;
  color: #333;
}
```

</td></tr>
</table>

이렇게 하면 인자값을 특별히 지정해주지 않을 경우, 기본값을 출력하게 됩니다.
주의! 기본값이 없는 인자에 대해 값을 넣어주지 않으면 에러가 납니다.

mixin은 특히 css3에서 브라우저별로 다른 벤더 프리픽스를 입력할 때 좋습니다. 그 예제는 <http://sass-lang.com/guide#topic-6>에서 확인해보시면 될 것 같습니다

## [Mixin vs Extend]

정리를 하다보니, mixin도 공통된 스타일을 반영할 때 쓰고, extend도 공통된 스타일을 쓰는데… 뭐가 다른거지? 결국엔 공통된 거 써주는거잖아?! 라는 의문이 듭니다. 코드를 살펴보니 차이는 간단합니다. 공통된 속성을 클래스마다 넣어줄 것이냐.. [mixin], (,)콤마 선택자로 클래스들을 하나로 묶어서 한번만 넣어줄 것이냐.. [extend] 입니다. 결국에는 작업자의 스타일에 따라.. 작업상황에 따라.. mixin을 쓸 지 extend를 쓸 지를 결정하는거라고 볼 수 있을 것 같네요~ 때에 따라서는 mixin 안에 extend를 조합할 수도 있습니다

[CSS 출력 style지정]

Sass 기능에 대한 내용들을 잠깐씩 접해보았습니다. 이제 이런 기능들을 사용한 .scss 파일을 .css 파일로 출력해주기만 하면 되는데요~ Sass 명령어로 CSS의 출력 스타일을 지정해 줄 수 있습니다!

### 명령어 작성

sass –style style종류 (작성된)파일명.scss (변환될)파일명.css
ex) sass –style compact test.scss test.css

### style 종류

중첩(nested) : sass의 기본스타일로 html문서처럼 부모요소에 대한 하위요소는 들여쓰기 되는 형식입니다.
확장(expanded) : 일반적인 css 스타일로 선택자에 따라 속성이 들여쓰기 되는 형식입니다.
축약(compact) : 업무에서 가장 많이 사용되는 스타일로 한줄씩 출력되는 스타일입니다.

각 style에 따른 출력화면은 아래와 같습니다.

<table>
<tr><td>중첩(nested)</td><td>확장(expanded)</td><td>축약(compact)</td></tr>
<tr><td>

```scss
.info {
background: #fff; }
info .tit {
font-size: 11px; }
.test2 {
font-color: #000; }
```

</td><td>

```scss
.info {
background: #fff;
}
.info .tit {
font-size: 11px;
}
.test2 {
font-color: #000;
}
```

</td><td>

```scss
.info { background: #fff; }
.info .tit { font-size: 11px; }
.test2 { font-color: #000; }
```
</td></tr>
</table>

그리고 수정할 때마다 명령어를 실행하기 번거롭다! 하시면 아래와 같이 –watch 명령어를 활용하시면 됩니다!

```bash
sass --watch (작성된)파일명.scss 
sass --watch --style종류 (작성된)파일명.scss
```

Sass의 기능은 이 외에도 연산기능이나 @if{}, @for{}, @each{} 등 다양한 기능이 있는데.. 자세한 내용들은 http://sass-lang.com/을 참고하시면 좋을 것 같습니다.

## 참고자료

- http://sass-lang.com/
- http://sassmeister.com/ ← Sass를 설치하지 않고도 scss를 쓸 경우, css로 어떻게 출력되는지 확인할 수 있는 사이트
- 도서 [댄 시더홈 웹디자이너를 위한 SASS]