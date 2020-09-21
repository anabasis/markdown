# Decorators

참고사이트

- <http://www.hanbit.co.kr/media/channel/view.html?cms_code=CMS5689111564>

## Closure

데코레이터의 본질은 클로저입니다. 이미 알고 계시다면 과감하게 더 이상 읽지 않으셔도 됩니다!
클로저가 뭐냐 하면 함수에 함수

```python
# 하스스톤의 안녕로봇
def annoy_o_tron(message):

    # 안녕로봇이 인사할 수 있도록 해주는 내부 구현
    def greeting(name):
      print(f'{message} - {name}!')
    return greeting

    # annoy_o_tron 함수의 반환값은 def greeting(name): print(...) 입니다
    # print(f'Hello - {name}') 함수를 값처럼 가지고 있는 상태라고 할 수 있겠네요
    hello_o_tron = annoy_o_tron('Hello')

    # print('Hello - Doondoony!') 함수를 호출합니다
    hello_o_tron('Doondoony') # Hello - Doondoony!
    hello_o_tron('velog') # Hello - velog!
```

## 데코레이터의 종류

1. 함수 데코레이터
2. 클래스 데코레이터

### 함수 데코레이터

```python
def logger(func):
    def wrapper(*args, **kwargs):
        result = func(*args, **kwargs)
        print(f'Result: {result}')
        return result
    return wrapper

    @logger
    def add(a, b):
        return a + b

    result = add(20, 22) # Result: 42
    print(result) # 42
```

### 클래스 데코레이터

```python
class Logger(object):
    def __init__(self, func):
        self.func = func

    def __call__(self, *args, **kwargs):
        result = self.func(*args, **kwargs)
        print(f'Result: {result}')
        return result

    @Logger
    def add(a, b):
        return a + b

    result = add(20, 22) # Result: 42
    print(result) # 42
```

### 람다 표현식 활용한 데코레이터

```python
logger = lambda fn: lambda *args, **kwargs: print(f'Result: {fn(*args, **kwargs)}')
## 함수 실행 결과만 반환, print는 그대로 실행
## logger = lambda fn: lambda *args, **kwargs: (print(f'Result: {fn(*args, **kwargs)}'), fn(*args, **kwargs))[1]

@logger
def add(a, b):
    return a + b

result = add(20, 22) # Result: 42
print(result) # None...
```

## 데코레이터 문법

위의 예제에서도 알 수 있지만 함수 또는 클래스, 메서드 등 callable한 객체의 선언부 상단에 @로 시작하는 함수명을 적어주는 형태를 띄어야 합니다!
(Pycharm 에서 데코레이터 뒤에 함수나 다른 데코레이터가 오지 않을 경우 Syntax Error: @ or def expected 에러를 표시합니다)

```python
@timer # 함수의 정의부 상단에 적용할 데코레이터를 작성합니다
def foo():
    pass

@timer # 클래스 정의부 상단에 적용할 데코레이터를 작성해도 됩니다
class Bar(object):
    pass

class Baz(object):
    @timer # 클래스의 메서드 당연하게도 됩니다
    def qux(self):
        pass

@timer
doondoon = 'Doondoony' # [X] 이런건 안됩니다...

# callable 한 객체에는 모두 사용 가능합니다
callable(foo) # True
callable(Bar) # True
callable(Baz().qux) # True
callable(doondoon) # False
```

## 클러저의 축약문법

```python
def synchronized(lock):
    # codes below...
    pass

def classmethod(arg):
    # codes below...
    pass

# 1안 일반 ######################
def foo(cls):
    # codes below...
    pass

foo = synchronized(lock)(foo)
foo = classmethod(foo)


# 2안 축약문법 ######################
@classmethod
@synchronized(lock)
def foo(cls):
    pass
```

## 데코레이터 실행 순서

- 가장 중요한 건 함수 위에 @decorator 를 적용할 때 greeting 함수가 decorator 함수 안으로 들어감
- 중첩된 데코레이터는 바로 위에서 설명드렸다시피 아래에서 위로 실행

```python
def decorator(func):
    print('in decorator function') #1
    # 2번과 3번의 실행 순서는 비즈니스 로직에 달려있습니다 (데코레이터와는 순서 무관)
    def wrapper(*args, **kwargs):
        print('in wrapper function') #2
        func(*args, **kwargs) #3
        return wrapper

@decorator
def greeting():
    print('in greeting')

if __name__ == '__main__':
    greeting()
```

## 사용방법

- 반복을 줄임
- 메소드나 함수의 책임을 확장
- 가독성이 떨어짐

### 데코레이터를 사용하지 않는 경우

```python
import functools
import time
  
# 시작, 종료 시간을 체크해서 프린트 하는 코드를 함수안에 반영합니다.
def huge_add(a, b):
    start = time.time()
    result = a + b
    time.sleep(1)
    print(f'add Elapsed time: {round((time.time() - start), 4)} seconds')
    return result

# huge_add 와 같은 일을 하는 코드가 반복됩니당
def huge_subtract(a, b):
    start = time.time()
    result = a - b
    time.sleep(1)
    print(f'subtract Elapsed time: {round((time.time() - start), 4)} seconds')
    return result

# 같은 일을 하는 코드가 반복됩니당
def huge_multiply(a, b):
    start = time.time()
    result = a * b
    time.sleep(1)
    print(f'multiply Elapsed time: {round((time.time() - start), 4)} seconds')
    return result
  
if __name__ == '__main__':
    huge_number = 10e8
    huge_add(huge_number, huge_number)
    huge_subtract(huge_number, huge_number)
    huge_multiply(huge_number, huge_number)
  
# 출력
# add Elapsed time: 1.0039 seconds
# subtract Elapsed time: 1.0048 seconds
# multiply Elapsed time: 1.0048 seconds
```

### 데코레이터를 사용하는 경우

```python
import functools
import time

def timer(func):

    @functools.wraps(func)
    def wrapper(*args, **kwargs):
        start = time.time()
        result = func(*args, **kwargs)
        print(f'{func.__name__} took {round((time.time() - start), 4)} seconds')
        return result
    return wrapper

# timer 데코레이터 적용
@timer
def huge_add(a, b):
    result = a + b
    time.sleep(1)
    return result

@timer
def huge_subtract(a, b):
    result = a - b
    time.sleep(1)
    return result

@timer
def huge_multiply(a, b):
    result = a * b
    time.sleep(1)
    return result

if __name__ == '__main__':
    huge_number = 10e8
    huge_add(huge_number, huge_number)
    huge_subtract(huge_number, huge_number)
    huge_multiply(huge_number, huge_number)

# 출력, 이제는 간편하게 호출된 함수 이름도 확인할 수 있습니다
# huge_add took 1.0037 seconds
# huge_subtract took 1.0044 seconds
# huge_multiply took 1.0028 seconds

```

## 예제

```python
from urllib.parse import urljoin

import urllib3
from urllib3.exceptions import HTTPError


def response_ok(func):
    def validate(*args, **kwargs):
        result = func(*args, **kwargs)
        if result.status != 200:
            raise HTTPError(f'Request Failed with status code {result.status}')
            return result
        return validate

@response_ok
def fetch(path):
    with urllib3.PoolManager() as http:
        return http.request('GET', path)

if __name__ == '__main__':
    api = 'http://httpbin.org'
    url = urljoin(api, 'ip')  # 존재하는 URL 입니다. 200 응답을 기대합니다
    wrong_url = urljoin(api, 'pi')  # 존재하지 않는 URL 입니다. 404 응답을 기대합니다
    fetch(url).data.decode()  
    fetch(wrong_url).data.decode()
    # urllib3.exceptions.HTTPError: Request Failed with status code 404

# 출력
# 올바른 URL로 접속 시도시
# '{\n  "origin": "1.2.3.4"\n}\n'
# 잘못된 URL로 접속 시도시
# urllib3.exceptions.HTTPError: Request Failed with status code 404
```

## 데코레이터 장단점

### 장점

1. 중복을 제거하고, 코드가 간결해짐
2. 클로저를 직접 작성하는 것 보다 문법적으로 간결함
3. 부수 효과 없는 함수로 작성된 데코레이터의 경우, 조합해서 사용하기 용이

### 단점

1. 가독성이 떨어짐
2. 특정 메소드/함수 한정적인 데코레이터는 재사용성 떨어짐
3. 디버깅이 어려움

## 사용예제

```python
import datetime

def datetime_decorator(func):
    def decorated():
        print datetime.datetime.now()
        func()
        print datetime.datetime.now()
        return decorated

@datetime_decorator
def main_function_1():
    print "MAIN FUNCTION 1 START"

@datetime_decorator
def main_function_2():
    print "MAIN FUNCTION 2 START"

@datetime_decorator
def main_function_3():
    print "MAIN FUNCTION 3 START"
```

```python
import datetime

class DatetimeDecorator:
    def __init__(self, f):
        self.func = f

    def __call__(self, *args, **kwargs):
        print datetime.datetime.now()
        self.func(*args, **kwargs)
        print datetime.datetime.now()

class MainClass:

    @DatetimeDecorator
    def main_function_1():
        print "MAIN FUNCTION 1 START"

    @DatetimeDecorator
    def main_function_2():
        print "MAIN FUNCTION 2 START"

    @DatetimeDecorator
    def main_function_3():
        print "MAIN FUNCTION 3 START"

my = MainClass()
my.main_function_1()
my.main_function_2()
my.main_function_3()
```

## 데코레이터 구조

- ‘데코레이터(Decorator)는 호출할 함수를 다른 함수에 매개변수로 전달한 후에 호출할 함수를 감싸는 함수(wrapper)에서 전달받은 함수를 이용하여 추가 작업을 한 후에 반환한다.’

### 함수형 데코레이터(데코레이터에 인수전달이 없는 경우)

```python
def product_buying(function): # 1
    def wrapper(arg):
        arg = arg-5000
        return function(arg) # 5
    return wrapper # 6

@product_buying # 2-1
def money_eating_machine(money): # 2
    money_you_have = money
    return '거스름 돈 : {}'.format(money_you_have)

print money_eating_machine(10000) # 3

# -----
결과 : 거스름 돈 : 5000 # 4
```

### 함수형 데코레이터(데코레이터에 인수전달이 있는 경우)

```python
def product_buying(coffee, bread): # 3
    def machine(function): # 4
        def wrapper(money, list): # 5
            buy_list = []
            if list:
            for lt in list:
                if lt == '커피':
                    money = money - coffee
                    buy_list.append('커피')
                    if lt == '빵':
                        money = money - bread
                        buy_list.append('빵')
                    else:
                        buy_list = None

                return function(money, buy_list) # 6
            return wrapper
        return machine

@product_buying(coffee=6000, bread=4500) # 1
def money_eating_machine(money, select): # 2
    money_you_have = money
    return '구매목록 : {}, 거스름 돈 : {}'.format(','.join(select), money_you_have)

print money_eating_machine(15000, ['커피','빵']) # 7

# -----
결과 : 구매목록 : 커피,빵, 거스름 돈 : 4500
```

### Class형 decorator(데코레이터에 인수가 없는 경우)

```python
class readingTheBook:

    def __init__(self, function):
        self.function = function
        self.total_page = 513
        self.amount_read = []
        self.add_read = 3

    def __call__(self, reading, days):
        if self.total_page > 0: # 3
            self.total_page = self.total_page - reading
            days += 1 # 하루가 지났네..
            self.amount_read.append([reading, days]) # 얼마나 읽었는지 기록해놔야지..
            reading += self.add_read  # 오늘 이렇게나 읽었다!
            self.add_read += 3  # 더욱 열심히 읽어보자.
            self.__call__(reading, days)
        else:
        return self.function(self.amount_read, days)

@readingTheBook # 2
def read_books(page, days):
    for page_read, day in page:
        print '{}일째 {} 만큼 읽었네'.format(day, page_read)
    print '걸린 날짜 : {}일'.format(days)
    return True

read_books(37, 0) # 1

# -----
결과 :
1일째 37 만큼 읽었네
2일째 40 만큼 읽었네
3일째 46 만큼 읽었네
4일째 55 만큼 읽었네
5일째 67 만큼 읽었네
6일째 82 만큼 읽었네
7일째 100 만큼 읽었네
8일째 121 만큼 읽었네
걸린 날짜 : 8일
```

### Class형 decorator(데코레이터에 인수가 있는 경우)

```python
class fruit_counting:
    def __init__(self, operator):
        self.formula = operator

    def __call__(self, function):
        def wrapper(fruit1, fruit2):
            return function([fruit1, fruit2], eval(str(fruit1) + self.formula + str(fruit2)))
        return wrapper

@fruit_counting('+')
def fruit_basket(fruit, result):
    return '총 {0}개가 있네~'.format(result)

print fruit_basket(2,3)
```
