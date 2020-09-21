# PYTHON

## CHAPTER02

- 함수의 매개변수 : <https://ychae-leah.tistory.com/65>
- 함수 인자 : <https://wikidocs.net/16053>

### 함수의 범위

```python
a = 1
def func(a)
    a = a + 10
    return a
print func(a)
print(a)
--> 11
--> 1
```

- 외부변수에 직접 함수의 결과를 대입해서 사용(1)

```python
a = 1
def func(a)
    a = a + 10
    return a
a = func(a)
print(a)
--> 11
```

- 외부변수에 직접 함수의 결과를 대입해서 사용(2)

```python
a = 1
def func()
    global a
    a = a + 10
    return a
print func()
print(a)
--> 11
--> 11
```

### Lambda함수(익명함수)

### 1. 함수

```python
def 함수명(입력파라미터):
    문장1
    문장2
    [return 리턴값]
```

두개의 입력파라미터를 받아들여 이를 더한 값을 리턴하는 함수를 정의하고 이를 사용하는 예

```python
def sum(a, b):
    s = a + b
    return s

total = sum(4, 7)
print(total)
```

### 2. 파라미터 전달방식

- 파이썬 함수에서 입력 파라미터는 Pass by Assignment에 의해 전달.
- 호출자(Caller)는 입력 파라미터 객체에 대해 레퍼런스를 생성하여 레퍼런스 값을 복사하여 전달
- 전달되는 입력파라미터는 Mutable일 수도 있고, Immutable일 수도 있으므로 각 경우에 따라 다른 결과

- 만약 입력파라미터가 Mutable 객체이고, 함수가 그 함수 내에서 해당 객체의 내용을 변경하면, 이러한 변경 사항은 호출자(caller)에게 반영
- 함수 내에서 새로운 객체의 레퍼런스를 입력파라미터에 할당한다면, 레퍼런스 자체는 복사하여 전달되었으므로, 호출자에서는 새로운 레퍼런스에 대해 알지 못하게 되고, 호출자 객체는 아무런 변화가 없음
- 입력파라미터가 Immutable 객체이면, 입력파라미터의 값이 함수 내에서 변경될 수 없으며, 함수 내에서 새로운 객체의 레퍼런스를 입력파라미터에 할당되어도 함수 외부(Caller)의 값은 변하지 않는다.

#### 3. 함수내에서 i, mylist 값 변경

```python
def f(i, mylist):
    i = i + 1
    mylist.append(0)

k = 10       # k는 int (immutable)
m = [1,2,3]  # m은 리스트 (mutable)

f(k, m)      # 함수 호출
print(k, m)  # 호출자 값 체크
# 출력: 10 [1, 2, 3, 0]
```

- 함수 f()는 하나의 정수(i)와 하나의 리스트(mylist)를 입력받아, 함수 내에서 그 값들을 변경
- 정수는 Immutable 타입이므로 함수 내에서 변경된 것이 호출자에 반영되지 않으며, 리스트는 Mutable 타입이므로 추가된 요소가 호출자에서도 반영

### 4. Default Parameter

함수에 전달되는 입력파라미터 중 호출자가 전달하지 않으면 디폴트로 지정된 값을 사용하게 할 수 있는데, 이를 디폴트 파라미터 혹은 Optional 파라미터라 부른다. 아래 예제에서 factor는 디폴트 파라미터로서 별도로 전달되지 않으면, 그 값이 1로 설정된다.

```python
def calc(i, j, factor = 1):
    return i * j * factor

result = calc(10, 20)
print(result)
```

### 5. Named Parameter

- 함수를 호출할 때 보통 함수에 정의된 대로 입력파라미터를 순서대로 전달
- 순서의 의한 전달 방식 외에 또 다른 호출 방식으로 "파라미터명=파라미터값" 과 같은 형식으로 파라미터를 전달할 수도 있는데, 이를 Named Parameter라 부름
- Named Parameter를 사용하면 어떤 값이 어떤 파라미터로 전달되는지 쉽게 파악할 수 있는 장점

```python
def report(name, age, score):
    print(name, score)

report(age=10, name="Kim", score=80)
```

### 6. 가변길이 파라미터

- 함수의 입력파라미터의 갯수를 미리 알 수 없거나, 0부터 N개의 파라미터를 받아들이도록 하고 싶다면, 가변길이 파라미터를 사용
- 가변길이 파라미터는 파라미터명 앞에 * 를 붙여 가변길이임을 표시
- *numbers는 가변길이 파라미터이므로, total()을 호출할 때, 임의의 숫자의 파라미터들을 지정

```python
def total(*numbers):
    tot = 0
    for n in numbers:
        tot += n
    return tot

t = total(1,2)
print(t)
t = total(1,5,2,6)
print(t)
```

### 7. 리턴값

- 함수로부터 호출자로 리턴하기 위해서는 return 문을 사용
- return문은 단독으로 쓰이면 아무 값을 호출자에게 전달하지 않으며, "return 리턴값" 처럼 쓰이면, 값을 호출자에게 전달
- 함수에서 리턴되는 값은 하나 이상일 수 있는데, 필요한 수만큼 return 키워드 다음에 콤마로 구분하여 적는다. 예를 들어, return a,b,c 는 3개의 값을 리턴
- 기술적으로 좀 더 깊이 설명하면, 이는 (a,b,c) 세개의 값을 포함하는 Tuple 하나를 리턴하는 것으로 함수는 항상 하나의 리턴값을 전달

```python
def calc(*numbers):
    count = 0
    tot = 0
    for n in numbers:
        count += 1
        tot += n
    return count, tot

count, sum = calc(1,5,2,6)  # (count, tot) 튜플을 리턴
print(count, sum)
```