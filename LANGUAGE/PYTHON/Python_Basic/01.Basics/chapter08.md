# Closure

- 클로저란 퍼스트클래스 함수를 지원하는 언어의 네임 바인딩 기술
- 클로저는 어떤 함수를 함수 자신이 가지고 있는 환경과 함께 저장한 레코드
- 함수가 가진 프리변수(free variable)를 클로저가 만들어지는 당시의 값과 레퍼런스에 맵핑하여 주는 역할
- 일반 함수와는 다르게, 자신의 영역 밖에서 호출된 함수의 변수값과 레퍼런스를 복사하고 저장한 뒤, 이 캡처한 값들에 액세스

```python
# -*- coding: utf-8 -*-

def outer_func(): #1
    message = 'Hi' #3

    def inner_func(): #4
        print message #6

    return inner_func() #5

outer_func() #2
```

```bash
python closure.py
Hi
```

```python
# -*- coding: utf-8 -*-

def outer_func(): #1
    message = 'Hi' #3

    def inner_func(): #4
        print message #6

    return inner_func #5 <-- ()를 지웠습니다.

outer_func() #2
my_func = outer_func() #2 <-- 리턴값인 inner_func를 변수에 할당합니다.
print my_func #7 <-- 추가
my_func() #7
my_func() #8
```

```bash
python closure.py


<function inner_func at 0x1020dfed8>
Hi
Hi
```

```python
# -*- coding: utf-8 -*-

def outer_func():  #1
    message = 'Hi'  #3

    def inner_func():  #4
        print message  #6

    return inner_func  #5

my_func = outer_func()  #2

print my_func  #7
print dir(my_func)  #8
print type(my_func.__closure__) #9
print my_func.__closure__  #10
print my_func.__closure__[0]  #11
print dir(my_func.__closure__[0])  #12
print my_func.__closure__[0].cell_contents  #13
```

```bash
$ python closure.py
<function inner_func at 0x1019dfed8>
['__call__', '__class__', '__closure__', '__code__', '__defaults__', '__delattr__', '__dict__', '__doc__', '__format__', '__get__', '__getattribute__', '__globals__', '__hash__', '__init__', '__module__', '__name__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'func_closure', 'func_code', 'func_defaults', 'func_dict', 'func_doc', 'func_globals', 'func_name']
<type 'tuple'>
(<cell at 0x1019e14b0: str object at 0x1019ea788>,)
<cell at 0x1019e14b0: str object at 0x1019ea788>
['__class__', '__cmp__', '__delattr__', '__doc__', '__format__', '__getattribute__', '__hash__', '__init__', '__new__', '__reduce__', '__reduce_ex__', '__repr__', '__setattr__', '__sizeof__', '__str__', '__subclasshook__', 'cell_contents']
Hi
```

```python
# -*- coding: utf-8 -*-

def outer_func(tag):  #1
    text = 'Some text'  #5
    tag = tag  #6

    def inner_func():  #7
        print '<{0}>{1}<{0}>'.format(tag, text)  #9

    return inner_func  #8

h1_func = outer_func('h1')  #2
p_func = outer_func('p')  #3

h1_func()  #4
p_func()  #10
```

```bash
$ python closure.py
<h1>Some text<h1>
<p>Some text<p>
```

```python
# -*- coding: utf-8 -*-

def outer_func(tag):  #1
    tag = tag  #5

    def inner_func(txt):  #6
        text = txt  #8
        print '<{0}>{1}<{0}>'.format(tag, text)  #9

    return inner_func  #7

h1_func = outer_func('h1')  #2
p_func = outer_func('p')  #3

h1_func('h1태그의 안입니다.')  #4
p_func('p태그의 안입니다.')  #10
```

```bash
$ python closure.py
<h1>h1태그의 안입니다.<h1>
<p>p태그의 안입니다.<p>
```
