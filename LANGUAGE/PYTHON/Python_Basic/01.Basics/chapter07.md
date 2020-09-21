# First Class Function

- 퍼스트클래스 함수란 프로그래밍 언어가 함수 (function) 를 first-class citizen으로 취급하는 것
- 함수 자체를 인자 (argument) 로써 다른 함수에 전달하거나 다른 함수의 결과값으로 리턴 할수도 있고, 함수를 변수에 할당하거나 데이터 구조안에 저장할 수 있는 함수

## First Class Function 예제

- First Class Function 예

```python
# -*- coding: utf-8 -*-
def square(x):
    return x * x

print(str(square(5)))

f = square

print(str(square))
print(str(f))
```

```bash
$ python first_class_function.py
25
<function square at 0x1018dfe60>
<function square at 0x1018dfe60>
```

- 인자로 넘기 함수 호출

```python
# first_class_function.py
# -*- coding: utf-8 -*-
def square(x):
    return x * x

def my_map(func, arg_list):
    result = []
    for i in arg_list:
        result.append(func(i)) # square 함수 호출, func == square
    return result

num_list = [1, 2, 3, 4, 5]

squares = my_map(square, num_list)

print(str(squares))
```

```bash
$ python first_class_function.py
[1, 4, 9, 16, 25]
```

- 함수를 변수에 할당,인자로 전달, 함수 리턴값으로 사용

```python
# first_class_function.py
# -*- coding: utf-8 -*-
def square(x):
    return x * x

num_list = [1, 2, 3, 4, 5]

def simple_square(arg_list):
    result = []
    for i in arg_list:
        result.append(i * i)
    return result

simple_squares = simple_square(num_list)

print(str(simple_squares))
```

```bash
$ python first_class_function.py
[1, 4, 9, 16, 25]
```

- 퍼스트클래스 함수를 사용하면 이미 정의된 여러 함수를 간단히 재활용

```python
first_class_function.py
# -*- coding: utf-8 -*-
def square(x):
    return x * x

def cube(x):
    return x * x * x

def quad(x):
    return x * x * x * x

def my_map(func, arg_list):
    result = []
    for i in arg_list:
        result.append(func(i)) # square 함수 호출, func == square
    return result

num_list = [1, 2, 3, 4, 5]

squares = my_map(square, num_list)
cubes = my_map(cube, num_list)
quads = my_map(quad, num_list)

print(squares)
print(cubes)
print(quads)
```

```bash
$ python first_class_function.py
[1, 4, 9, 16, 25]
[1, 8, 27, 64, 125]
[1, 16, 81, 256, 625]
```

- 함수의 결과값으로 또 다른 함수를 리턴하는 방법

```python
# first_class_function2.py
# -*- coding: utf-8 -*-
def logger(msg):

    def log_message(): #1
        print('Log: '+ msg)

    return log_message

log_hi = logger('Hi')
print(log_hi) # log_message 오브젝트가 출력됩니다.
log_hi() # "Log: Hi"가 출력됩니다.
```

```bash
$ python first_class_function.py
<function log_message at 0x1029ca140>
Log:  Hi
```

- log_message가 정말 기억을 하고 있는지 msg 변수를 지역변수로 가지고 있는 logger 함수를 글로벌 네임스페이스에서 완전히 지운 후, log_message를 호출

```python
# first_class_function2.py
# -*- coding: utf-8 -*-
def logger(msg):

    def log_message(): #1
        print('Log: ' + msg)

    return log_message

log_hi = logger('Hi')
print(log_hi) # log_message 오브젝트가 출력됩니다.
log_hi() # "Log: Hi"가 출력됩니다.

del logger # 글로벌 네임스페이스에서 logger 오브젝트를 지웁니다.

# logger 오브젝트가 지워진 것을 확인합니다.
try:
    print(logger)
except NameError:
    print('NameError: logger는 존재하지 않습니다.')

log_hi() # logger가 지워진 뒤에도 Log: Hi"가 출력됩니다.
```

```bash
$ python first_class_function2.py
<function log_message at 0x1007ca1b8>
Log:  Hi
logger는 존재하지 않습니다.
Log:  Hi
```

- 활용

```python
# first_class_function3.py
# -*- coding: utf-8 -*-
# 단순한 일반 함수
def simple_html_tag(tag, msg):
    print '<{0}>{1}<{0}>'.format(tag, msg)

simple_html_tag('h1', '심플 헤딩 타이틀')

print '-'*30

# 함수를 리턴하는 함수
def html_tag(tag):

    def wrap_text(msg):
        print '<{0}>{1}<{0}>'.format(tag, msg)

    return wrap_text

print_h1 = html_tag('h1') #1
print print_h1 #2
print_h1('첫 번째 헤딩 타이틀') #3
print_h1('두 번째 헤딩 타이틀') #4

print_p = html_tag('p')
print_p('이것은 패러그래프 입니다.')
```

```bash
$ python first_class_function.py
<h1>심플 헤딩 타이틀<h1>
------------------------------
<function wrap_text at 0x1007dff50>
<h1>첫 번째 헤딩 타이틀<h1>
<h1>두 번째 헤딩 타이틀<h1>
<p>이것은 패러그래프 입니다.<p>
```
