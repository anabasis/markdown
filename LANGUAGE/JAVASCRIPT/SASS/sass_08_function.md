# Function (함수)

Built-in Function 과는 달리 이부분은 임의 함수입니다.

Function은 위에서 소개한 mixin 과도 사뭇 비슷한데요, 차이점은 mixin 은 style markup 을 반환하지만,  function 은 @return directive 를 통하여 값 을 반환합니다.

Function을 선언 할 때는,  예상하셨겠지만! @function directive 를 사용합니다.

```scss
/* SCSS */
@function calc-percent($target, $container) {
  @return ($target / $container) * 100%;
}
@function cp($target, $container) {
  @return calc-percent($target, $container);
}
.my-module {
  width: calc-percent(650px, 1000px);
}
```

```css
/* CSS */
.my-module {
  width: 65%;
}
```

꿀 팁: 자주 사용 할 것 같은 함수는 위와같이 단축함수를 만들어 사용하세요. 그런다고 해서 결과물의 용량이 늘어나지는 않으니까요.