# Python2 와 Python3의 차이

- Python3에서는 모든 변수가 객체(object)로 처리됨

## int 나누기 결과가 float

```python
# Python 2
print( 1/2 )
print( type(1/2) )
# 0
# <type 'int'>


# Python 3
print( 1/2 )
print( type(1/2) )
# 0.5
# <class 'float'>
```

## print문 괄호 필수

```python
# Python 2
print( 'hello' )
# hello
print 'hello'
# hello

# Python 3
print( 'hello' )
# hello
print 'hello'
# Error! invalid syntax
```

## str과 unicode 통일 ★

```python
# Python 2
print( type('hello') )
print( type(u'hello') )
# <type 'str'>
# <type 'unicode'>

# Python 3
print( type('hello') )
print( type(u'hello') )
# <class 'str'>
# <class 'str'>
```

→ 모든 문자열은 유니코드인 str 클래스(의 인스턴스)이다.

## long → int로 통일

```python
# Python 2
print( 2**30 )
print( type(2**30) )
print( 2**100 )
print( type(2**100) )
# 1073741824
# <type 'int'>
# 1267650600228229401496703205376
# <type 'long'>

# Python 3
print( 2**30 )
print( type(2**30) )
print( 2**100 )
print( type(2**100) )
# 1073741824
# <class 'int'>
# 1267650600228229401496703205376
# <class 'int'>
```

## 인코딩

소스 코드의 인코딩이 기본적으로 ‘utf-8’

```python
# Python 2
# -*- coding: utf-8 -*-

# Python 3
#(생략가능) -*- coding: utf-8 -*-
```

## input

```python
# Python 2
name = raw_input("input name:")

# Python 3
name = input("input name:")
```

## Exception Handling

```python
# Python 2
print 'Exception Handling'
try:
    let_us_cause_a_NameError
except NameError, err:
    print err, '--> our error message'

# Python 3
print 'Exception Handling'
try:
    let_us_cause_a_NameError
except NameError as err:
    print(err, '--> our error message')
```
