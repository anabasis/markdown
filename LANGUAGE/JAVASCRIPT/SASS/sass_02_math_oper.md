# Math Operators (수학 연산자)

Sass 에서는 수학 연산자들을 사용

|Operator|Description|
|:--:|:--:|
|+|addition|
|-|subtraction|
|/|division|
|*|multiplication|
|%|modulo|
|==|equality|
|!=|inequality|

주의하실점은, +, - operator 를 사용 할 떄는 단위를 언제나 통일
예를들어, 다음과 같은 코드는 오류가 발생하게됩니다: $box-width: 100% - 20px
이런 작업을 해야한다면 css 의 calc() 함수를 사용

다음과 같은 식은 오류 없이 작동합니다: $box-width: 300px / 960px * 100%

```scss
/*  SCSS */
.container { width: 100%; }

article[role="main"] {
  float: left;
  width: 600px / 960px * 100%;
}

aside[role="complementary"] {
  float: right;
  width: 300px / 960px * 100%;
}
```

```css
/* CSS */
.container {
  width: 100%;
}

article[role="main"] {
  float: left;
  width: 62.5%;
}

aside[role="complementary"] {
  float: right;
  width: 31.25%;
}
```