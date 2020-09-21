# Import (불러오기)

import 기능은 스타일들을 여러 파일들로 나누고, 다른 파일에서 불러와서 사용하는 기능
다음과 같이 @import directive 를 사용하여 특정.scss 파일을 불러 올 수 있습니다:

```scss
@import "layout.scss";
```

참고로, 확장자를 붙이지 않아도 됩니다.

```scss
@import "layout";
partial
```

partial 기능에 대하여 알아봅시다.
만약에 .sass 파일이나 .scss 파일의 파일이름을 underscore _ 로 시작하면 css 파일로 따로 컴파일되지 않음
html 에서 해당 css 파일을 불러올일이 없고, import만 되는경우에는이 기능을 사용

Sass도 css처럼 import가 가능합니다. 다른 점은, css는 import된 각각의 .css파일의 로딩을 http에 요청해야한다면, sass는 여러개의 .scss파일을 import해도 최종적으로는 하나의 .css로 변환해주기 때문에 http에 요청을 여러번 보낼 필요가 없음

* 작성방법 – @import “파일명.scss”; or @import “파일명”;

<http://sass-lang.com>에서는 .scss 파일을 import할 경우, .scss 확장자를 써주지 않아도 된다고 설명

<table>
<tr><td>SCSS(test.scss)</td><td>@import SCSS (test2.scss)</td><td>CSS</td></tr>

<tr><td>

```scss
@import ‘test2.scss’;
//@import ‘test2’;
$color-main:#333;
.test{
    font-color:$color-main;
}
.test2{
    font-color:$color-test;
}
```

</td><td>

```scss
$color-test:#000;
```

</td><td>

```scss
.test{
    font-color:#333;
}
.test2{
    font-color:#000;
}
```

</td></tr>
</table>

* sass에서는 // 한 줄 주석이 가능합니다!

   한 줄짜리 주석은 출력된 .css 파일에서는 삭제되기 때문에 자유롭게 쓸 수 있습니다!

예제를 보면, 변수가 두 번 ($color-main, $color-test) 사용된 것을 보실 수 있습니다. 첫번째 test.scss파일을 보면 $color-test가 변수 선언되어 있지 않습니다. 단지 $color-main 변수만 선언 되었을 뿐.. 그런데, 이 파일에 import된 test2.scss 파일을 보니 $color-test라는 변수가 있네요~ 그러면 sass는 test.scss파일이 css로 출력될 때 import된 파일의 내용도 함께 반영해서 출력해주게 됩니다.
CSS 결과물에 test2.scss에 반영된 \$color-test 변수의 속성값이 적용된 것