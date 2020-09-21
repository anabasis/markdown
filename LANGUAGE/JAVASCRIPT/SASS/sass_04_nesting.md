# Nesting (중첩)

Sass 의 매우 유용한 기능중 하나는 선언을 중첩

```css
/* CSS */
.container {
    width: 100%;
}

.container h1 {
    color: red;
}
```

간단한 CSS 면, 큰 문제는 없지만, CSS 파일이 커지면 유지보수가 어려워지죠..
Sass 에선, 이런식으로 작성하면 위와 같은 결과물을 얻을 수 있게 됩니다!

```scss
/*  SCSS */
.container {
    width: 100%;
    h1 {
        color: red;
    }
}
```

부모 선택자를 리퍼런스 할떄는 & 문자를 사용합니다. (내장함수 예제에서도 이 문자가 사용됐었죠?)

```scss
/* SASS */
a {
  color: black;
  &:hover {
    text-decoration: underline;
    color: gray;
  }
  &:visited {
    color: purple;
  }
}
```

```css
/* CSS */
a {
  color: black;
}
a:hover {
  text-decoration: underline;
  color: gray;
}
a:visited {
  color: purple;
}
```

코드 중첩을 하는건 위와같이 하면 됩니다. 하지만 중첩에서 벗어나려면 (de-nest)

```scss
/* SCSS */
.container {
  .child {
    color: blue;
  }
  .sibling {
    color: gray;
  }
}
```

sibling 클래스가 container 클래스 밖에서도 사용되는것을 알게되었을땐, @at-root directive (지시자) 를 사용

```scss
/* SSCC */
.container {
  .child {
    color: blue;
  }
  @at-root .sibling {
    color: gray;
  }
}
```

```css
/* CSS */
.container .child {
  color: blue;
}
.sibling {
  color: gray;
}
```

보통 @at-root 는 Sass 코드를 깔끔하게 정리하면서 작성 할 때 사용됨
(참조링크: [Writing modular CSS (BEM/OOCSS) selectors with Sass 3.3](https://benfrain.com/writing-modular-css-bemoocss-selectors-sass-3-3/))

## 인셉션 규칙

- Sass 코드 중첩을 할 때, 4 레벨 보다 깊게 들어가지 말 것

Default

```css
.info{background:#fff}
.info:before{display:block;content:''}
.info .tit{font-size:16px}
.info .lnk{display:block;margin-top:9px}
```

.info라는 클래스 안에 있는 .tit에는 이런 속성을 주고, 또 .info라는 클래스 안에 .lnk에는 저런 속성을 주고 …
.info라는 클래스를 쓰고 또 쓰고 하는 그런 상황!
sass를 사용하면 중첩기능을 통해 .info를 한번만 쓰고 적용

<table>
<tr><td>SCSS</td><td>CSS</td></tr>
<tr><td>

```scss
.info{
    background:#fff;
    &:before{
        display:block;
        content:””;
    };
    .tit{
          font-size:16px;
    }
    .lnk{
          display:block;
          margin-top:9px;
    }
}
```

</td><td>

```css
.info {
      background: #fff;
}
.info:before {
      display: block;
      content: “”;
}
.info .tit {
      font-size: 16px;
}
.info .lnk {
      display: block;
      margin-top: 9px;
}
```

</td></tr>
</table>

그리고 적용이 필요한 코드들은 .클래스명{속성:값}; 으로 .info내에 선언해주었습니다.
예제를 보면, .info는 한 번 밖에 쓰이지 않았습니다.
이렇게하면, sass는 중첩된 내용들에 대해서 알아서 최상단에 선언해 준 클래스명을 적용해줍니다.(ex. .info .tit, .info .lnk…)

그런데, 예제를 보다보니 이상한 애가 하나 있습니다.
& ← 너 누구니? 너 이름이 모니?
안녕하세요? 저는 Referencing Parent Selectors(부모참조선택자) 라고 해요!!
&(부모참조선택자)의 등장입니다. sass에서는 &기호로 부모 선택자를 참조할 수 있습니다.

위의 예제에서는 .info가 부모 선택자가 됩니다. 이게 왜 필요하냐..!라고 하시면 아래의 예제를 볼까요?

<table>
<tr><td>SCSS</td><td>CSS</td></tr>
<tr><td>

```scss
.info{
    background:#fff;
    &.tit{
      font-size:16px;
    }
    .tit{
      font-size:11px;
    }
}
```

</td><td>

```css
.info {
    background: #fff;
}
.info.tit {
    font-size: 16px;
}
.info .tit {
    font-size: 11px;
}
```

</td></tr>
</table>

예제를 보면, &선택자를 사용한 .tit클래스는 .info의 복합선택자 속성값이 적용되지만, 그렇지 않은 .tit클래스는 .info 하위에 있는 자식선택자로 속성값이 적용
&선택자의 사용여부에 따라 부모 친구선택자(복합선택자)가 되느냐, 부모를 따르는 자식선택자가 되느냐로 구분

<table>
<tr><td>SCSS</td><td>CSS</td></tr>
<tr><td>

```scss
.info{
  background:#fff;
  &.tit{
    font-size:16px;
  }
  body.test &{
    font-size:30px;
  }
}
```

</td><td>

```css
.info {
background: #fff;
}
.info.tit {
font-size: 16px;
}
body.test .info {
font-size: 30px;
}
```

</td></tr>
</table>

body가 .test라는 클래스를 가질 때 .info클래스의 속성값이 변하는 것에 대해 body.test{};으로 스타일을 새롭게 작성하는 것이 아니라 작성중인 스타일내에서 body.test뒤로 &선택자만 활용하여 선언해주면 CSS에서는 부모선택자를 참조하여 예제와 같은 결과값을 출력해주게 됩니다. 연관된 스타일을 여러 줄을 만드는 것이 아닌, 하나로 그룹핑 할 수 있다는 장점