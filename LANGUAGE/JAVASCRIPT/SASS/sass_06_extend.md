# Extend (상속)

Sass 에서 특정 선택자를 상속 할 때, @extend directive를 사용합니다.

```scss
/* SCSS */
.box {
  border: 1px solid gray;
  padding: 10px;
  display: inline-block;
}
.success-box {
  @extend .box;
  border: 1px solid green;
}
```

```css
/* CSS */
.box, .success-box {
  border: 1px solid gray;
  padding: 10px;
  display: inline-block;
}
.success-box {
  border: 1px solid green;
}
```

## Placeholder

Placeholder 선택자 % 를 사용하면 상속은 할 수 있지만 해당 선택자는 컴파일되지 않음

```scss
/* SCSS */
%box {
  padding: 0.5em;
}
.success-box {
  @extend %box;
  color: green;
}
.error-box {
  @extend %box;
  color: red;
}
```

```css
/* CSS */
.success-box, .error-box {
  padding: 0.5em;
}
.success-box {
  color: green;
}
.error-box {
  color: red;
}
```

mixin과 같이 중복되는 스타일에서 일부 속성값만 다른 클래스에 대해 좀 더 쉽게 작성하는 방법으로 extend 기능

* 작성방법 – @extend .클래스명; or @extend %클래스명;

<table>
<tr><td>SCSS</td><td>CSS</td></tr>
<tr><td>

```scss
.ico{
  display:block;
}
%bg{
  background:url(img/bg.gif) no-repeat;
}
.ico_new{
  @extend .ico;
  @extend %bg;
  background:red;
}
.ico_qna{
  @extend .ico;
  @extend %bg;
  background:black;
}
```

</td><td>

```scss
.ico, .ico_new, .ico_qna {
  display: block;
}
.ico_new, .ico_qna {
  background: url(img/bg.gif) no-repeat;
}
.ico_new {
  background: red;
}
.ico_qna {
  background: black;
}
```

</td></tr>
</table>

예제를 보면, 동일한 속성값을 가진 클래스들이 background 색상만 다를 경우, extend를 이용하면 공통되는 속성값에 대한 클래스를 (,)콤마구분으로 묶어서 출력해주고, 다른 속성값에 대해서만 따로 출력해주는 것을 볼 수 있습니다. 이렇게하면, html에서 클래스를 두 번 써야하는 번거로움을 줄일 수가 있습니다. 아래처럼요!

\<span class=”ico ico_new”\> → \<span class=”ico_new”\>

* % : Placeholder Selectors
* %(플레이스홀더 셀렉터) : extend를 사용할 때 ID나 class선택자처럼 %로 선택자를 만들 수 있음
* 단, %선택자는 css상에서는 출력되지 않음
