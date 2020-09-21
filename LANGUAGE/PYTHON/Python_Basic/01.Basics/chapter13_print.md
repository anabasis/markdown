# print

## 예제1

- print함수로 출력할 문장(문자열)은 ‘ ’ 또는 “ ” 로 감싸야 한다.
- 문자열 속에 ' 기호가 있는 경우에는 “ ”로
- “ 기호가 있는 경우에는 ‘ ’를 사용하면 편리하게 출력할 수 있다.
- 콤마(,)로 문자열을 나열할 경우 공백(기본값)이 자동으로 추가 된다.
- 더하기(+) 기호로 문자열을 공백없이 연결할 수 있다.

```python
# '' "" , + 차이
print('Hello Python!')
print("Nice to meet you.")
print('Hello "Python"')
print("Hello 'Python'")
print('Hello', 'Python!')
print('Hello' + 'Python!')

Hello Python!
Nice to meet you.
Hello "Python"
Hello 'Python'
Hello Python!
HelloPython! (공백이 없음)

# 형식
print('variable',variable,end='')
print('This is string :', var_char, 'This is number :' ,var_num,var_num2)
print('This is string :' + var_char, 'This is number :', var_num+var_num2)
```

## 예제2

- 긴 문자열은 \ 기호로 사용하여 여러 줄에 작성할 수 있다.
- 개행(줄바꿈), 공백, 특수기호(이스케이프 문자)를 포함하여 입력한 그대로 화면에 출력하기 위해서는 ‘’‘ ’‘’ 또는 “”“ ”“” 기호를 사용하면 된다.

```python
# 긴문자열 ''' """ \
print('I like Python. \
But I do not like Ruby.')
print("""I like Python.
But I don't like C.""")
print('''I like Python.
But I don't like C.''')

I like Python. But I do not like Ruby. (개행 없이 연결됨)
I like Python.
But I don't like C.
I like Python.
But I don't like C.
```

## 예제3

- end 인자를 사용하면 print함수 마지막 효과를 변경할 수 있다. (기본값은 개행)
- sep 인자를 사용하면 콤마로 구분된 문자열을 다르게 결합할 수 있다. (기본값은 공백)
- file 인자를 사용하면 출력 결과를 파일, 표준에러처리로 보낼 수 있다.입력

```python
# end, sep, file
print('Hello', end=' ')
print('Python')
print('Hello', end='&&&')
print('Python')
print('Hello', 'Python', sep='#')

f = open('dump.txt', 'w')
print('Hello Python', file=f)
f.close()

import sys
print('Hello Python', file=sys.stderr)

Hello Python
Hello&&&Python
Hello#Python
(dump.txt 파일에 문자열이 저장됨)
Hello Python
```

## 예제4

- 이스케이프 문자(Escape character)는 \문자를 사용하여 출력한다.
- (\n 공백, \t 탭키, \\ \, \‘, \“ 등)

```python
# Excape Charater
print('My mother\'s house')
print('\', \", \\, \a, \t, \n')

My mother's house
', ", \, , ,
```

## 예제5

- % 기호를 활용하여 변수값을 출력할 수 있다. (오래된 방식)
- (%d 정수, %f 실수, %s 문자열)
- %5.2f처럼 출력하는 모양을 지정할 수 있다. (총 자릿수 5, 소수점 2자리까지)
- format 함수를 사용하면 좀 더 다양항 방식으로 출력을 할 수 있다.
- (format 함수에 대한 내용은 워낙 방대하여 나중에 따로 글을 쓰겠습니다. 지금은 예문으로만 감상해주세요. 설명하자면 긴데 사실 format함수도 쓰는 기능만 주로 쓰기 때문에 꼭 모든 것을 알아야할 필요는 없습니다.)

```python
# %기호, format
i = 123
f = 3.14
s = 'Hello'
print('i: %d, f: %f, s: %s' % (i, f, s))
print('i: %9d, f: %5.2f, s: %7s' % (i, f, s))
print('i: %09d, f: %05.2f, s: %7s' % (i, f, s))
print('i: {}, f: {}, s: {}'.format(i, f, s))
print('f: {1}, i: {0}, s: {2}'.format(i, f, s))
print('f: {ff}, i: {ii}, s: {ss}'.format(ii=i, ff=f, ss=s))
a = 'apple'
b = 'banana'
print('a is {0[a]}, b is {0[b]}'.format(locals()))
print('a is {a}, b is {b}'.format(**locals()))

i: 123, f: 3.140000, s: Hello
i:       123, f:  3.14, s:   Hello
i: 000000123, f: 03.14, s:   Hello
i: 123, f: 3.14, s: Hello
f: 3.14, i: 123, s: Hello
f: 3.14, i: 123, s: Hello
a is apple, b is banana
a is apple, b is banana
```

```python
# 형식에 맞추어 출력하기

# 파이썬(Python) 3 포맷팅 방식
print('나의 이름은 {}입니다'.format('한사람'))
print('나이는 {1}세이고 성별은 {2}입니다. 나의 이름은 "{0}"입니다. '.format('한사람',33,'남성'))
print('나이는 {age}세이고 성별은 {gender}입니다. 나의 이름은 "{name}"입니다. '
         .format(name='한사람',age=33,gender='남성'))
print('만세삼창 :  {0}!!! {0}!!! {0}!!! '.format('만세'))
print('삼삼칠 박수 :  {0}!!! {0}!!! {1}!!! '.format('짝'*3,'짝'*7))
print('-' * 40)

나의 이름은 한사람입니다
나의 이름은 "한사람"입니다. 나이는 33세이고 성별은 남성입니다.
나이는 33세이고 성별은 남성입니다. 나의 이름은 "한사람"입니다.
나이는 33세이고 성별은 남성입니다. 나의 이름은 "한사람"입니다.
만세삼창 :  만세!!! 만세!!! 만세!!!
삼삼칠 박수 :  짝짝짝!!! 짝짝짝!!! 짝짝짝짝짝짝짝!!!
----------------------------------------
```

## 예제6

- 문자열 객체의 다양한 메소드(함수)를 활용하여 출력 모양을 변경할 수 있다.
- (메소드 이름만으로도 동작을 짐작할 수 있기에 자세한 설명은 생략)

```python
# String 함수 사용
print('Hello Python!'.center(20))
print('Hello Python!'.rjust(20))
print('Hello Python!'.ljust(20))
print('Hello Python!'.zfill(20))
print('hello python!'.capitalize())
print('hello python!'.upper())

   Hello Python!
       Hello Python!
Hello Python!
0000000Hello Python! (앞쪽 여백을 0으로 채움)
Hello python! (첫글자만 대문자로)
HELLO PYTHON! (대문자로 변경)
```

## 예제7

- 정수, 실수, 불 자료형을 문자열로 변경할 필요없이 바로 출력할 수 있다.
- (명시적으로 변경하기 위해서는 str()함수를 사용하면 됨)
- 수식과 리스트 같은 복잡한 내용도 쉽게 출력할 수 있다.

```python
# 정수,실수,불 자동형변환, str()함수
print(123); print(3.14)
print(True)
print("3 * 2 = ", 3 * 2)
print(list(range(1, 10)))

123
3.14
True
3 * 2 =  6
[1, 2, 3, 4, 5, 6, 7, 8, 9]
```

## 예제8

- 길이와 정렬
- {:길이} : 출력할 데이터의 길이를 지정합니다. 문자열(왼쪽 정렬), 숫자(오른쪽 정렬)
- {:<길이} : 왼쪽 정렬
- {:>길이} : 오른쪽 정렬
- {:^길이} : 가운데 정렬

```python
print('Python is [{:15}]'.format('good'))
print('Python is [{:<15}]'.format('good'))
print('Python is [{:>15}]'.format('good'))
print('Python is [{:^15}]'.format('good'))
print('당신의 나이는 [{:15}]세'.format(22))
print('당신의 나이는 [{:<15}]세'.format(22))
print('당신의 나이는 [{:>15}]세'.format(22))
print('당신의 나이는 [{:<15}]세'.format(22))

Python is [good           ]
Python is [good           ]
Python is [           good]
Python is [     good      ]
당신의 나이는 [             22]세
당신의 나이는 [22             ]세
당신의 나이는 [             22]세
당신의 나이는 [22             ]세
```

## 예제9

- {}를 출력하려면 "{"과 "}"을 세번 연달아 입력해야 합니다.
- % 자체를 출력하려면 %%로 입력해야 합니다.

```python
# 형식에 맞추어 출력하기
print('{0}씨는 상위 {1}%안에 있는 사람입니다.'.format('한사람',10))
print('{{0}}씨는 상위 {{1}}%안에 있는 사람입니다.'.format('한사람',10))
print('{{{0}}}씨는 상위 {{{1}}}%안에 있는 사람입니다.'.format('한사람',10))
print('%s씨는 상위 %d%%안에 있는 사람입니다.'%('한사람',10))

한사람씨는 상위 10%안에 있는 사람입니다.
{0}씨는 상위 {1}%안에 있는 사람입니다.
{한사람}씨는 상위 {10}%안에 있는 사람입니다.
한사람씨는 상위 10%안에 있는 사람입니다.
```

## 출력형식 정리

표준 형식 지정자의 일반적인 형식

```properties
format_spec :: = [[ fill] align] [ sign] [#] [0] [ width] [ grouping_option] [. precision] [ type]
fill :: = <모든 문자>
align :: = "<"| ">"| "="| "^"
sign :: = "+"| "-"| ""
width :: = digit+ grouping_option :: = "_"| ","
precision :: = digit+
type :: = "b"| "c"| "d"| "e"| "E"| "f"| "F"| "g"| "G"| "n"| "o"| "s"| "x"| "X"
```

- fill(채움문자) : 어떠한 문자도 가능합니다.
- align(정렬) : <(왼쪽정렬, 문자열 기본값), >(오른쪽 정렬, 숫자 기본값), ^(가운데정렬)
=(숫자에만 사용, '+000000120'형식으로 필드를 인쇄하는 데 사용)
- sign(부호) : +(양수도 부호 표시), -(음수에 대해서만 부호를 사용,기본값)
- width(폭) : 전체 폭을 양의 정수로 지정합니다.
- `, | _` : 천단위마다 콤마(,) 또는 밑줄(_)을 붙여줍니다.
- precision(소수이하 자리수) : 소수이하 자리수를 정수로 지정합니다.
- type(형식) : 문자열(s), 정수(b, c, d, o, x, X, n), 실수(e, E, f, F, g, G, n)
