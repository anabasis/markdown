# Variable (변수)

- Sass 는 CSS에 변수 개념을 도입
- 변수로 사용 가능한 형태는 숫자, 문자열, 폰트, 색상, null, lists 와 maps
- 변수를 사용 할 떄는 $ 문자를 사용

```sass
$primary-color: #333;
```

변수를 만들어도, 사용하지 않으면 컴파일된 CSS 파일에는 아무것도 나타나지 않음

```scss
/* SCSS */
$primary-color: #333;

body {
  background-color: $primary-color;
}
```

```css
/* CSS */
body {
  background-color: #333;
}
```

## Variable Scope (변수 범위)

변수를 특정 selector (선택자) 에서 선언하면 해당 selector 에서만 접근이 가능

```scss
/* SCSS */
$primary-color: #333;

body {
  $primary-color: #eee;
  background-color: $primary-color;
}

p {
  color: $primary-color;
}
```

```css
/* CSS */
body {
  background-color: #eee;
}

p {
  color: #333;
}
```

변수를 선언 할 때, 변수를 global (전역) 하게 설정 할 때는 !global 플래그를 사용

```scss
/* SCSS */
$primary-color: #333;

body {
  $primary-color: #eee !global;;
  background-color: $primary-color;
}

p {
  color: $primary-color;
}
```

```css
/* CSS */
body {
  background-color: #eee;
}

p {
  color: #eee;
}
```

추가적으로, !default 플래그는 해당 변수가 설정되지 않았거나 값이 null 일떄 값을 설정
이 플래그는 나중에 mixin 을 작성 할 떄 유용하게 사용

```scss
# SCSS
$primary-color: #333;

$primary-color: $eee !default;

p {
  color: $primary-color;
}
```

```css
/* CSS */
p {
  color: #333;
}
```

작성방법 – $변수명 : 속성값;
<table>
<tr><td>SCSS</td><td>CSS</td></tr>
<tr><td>

```scss
$display-default:block;
$color-point:#080;
.lnk{
  display:$display-default;
  position:absolute;
  left:5px;
  color:$color-point;
}
```

</td><td>

```css
.lnk {
display: block;
position: absolute;
left: 5px;
color: #080;
}
```

</td></tr>
</table>

이렇게 작성한 후 css로 변환하면, sass는 자동으로 내가 선언한 변수가 가진 값을 출력
.lnk내에 display 속성값과 color 속성값을 위에 선언된 변수명