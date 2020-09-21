# Numpy

<https://aruie.github.io/2019/06/19/numpy.html>
<http://pythonstudy.xyz/python/article/402-numpy-%EC%82%AC%EC%9A%A9%ED%95%98%EA%B8%B0>
<https://ponyozzang.tistory.com/499>
<https://engkimbs.tistory.com/664>

```bash
# 설치
pip install numpy
```

## numpy 배열

행 2, 열3 -> rank 2 shape (2,3)

## 생성 함수

- np.array()

```python
li = [1, 2, 3, 4, 5, 6]  # li 리스트
arr = np.array(li)     # ndarray로 변환
display(arr)
array([1, 2, 3, 4, 5, 6])
```

- np.arange()

```python
> np.arange(1,7)  
array([1, 2, 3, 4, 5, 6])
```

- np.zeros(), np.full(), np.eye()

```python
np.zeros((2,3))
array([[0., 0.],
       [0., 0.],
       [0., 0.]])

np.full((2,3), 55)  # 자매품 0대신 다른값 넣는 함수
array([[55, 55, 55],
       [55, 55, 55]])

np.eye(2)   # 단위행렬(주대각성분이 1이고 나머진 0) 생성 함수
array([[1., 0.],
        0., 1.])
```

```python
import numpy as np

list1 = [1, 2, 3, 4]
a = np.array(list1)
print(a.shape) # (4, )

b = np.array([[1,2,3],[4,5,6]])
print(b.shape) # (2, 3)
print(b[0,0])  # 1
```

```python
import numpy as np

a = np.zeros((2,2))
print(a)
# 출력:
# [[ 0.  0.]
#  [ 0.  0.]]

a = np.ones((2,3))
print(a)
# 출력:
# [[ 1.  1.  1.]
#  [ 1.  1.  1.]]

a = np.full((2,3), 5)
print(a)
# 출력:
# [[5 5 5]
#  [5 5 5]]

a = np.eye(3)
print(a)
# 출력:
# [[ 1.  0.  0.]
#  [ 0.  1.  0.]
#  [ 0.  0.  1.]]

a = np.array(range(20)).reshape((4,5))
print(a)
# 출력:
# [[ 0  1  2  3  4]
#  [ 5  6  7  8  9]
#  [10 11 12 13 14]
#  [15 16 17 18 19]]
```

## 연산 함수

- np.add(), np.substract(), np.mulyiply(), np.divide()
사칙연산입니다. 각 해당하는 위치의 원소끼리 연산

- np.dot()
내적을 연산하는 것이라지만, 행렬곱

- np.sum(), np.prod()
원소의 합, 혹은 곱을 반환

- np.max(), np.min()
최대와 최소값 반환하는 함수

- np.argmax(), np.argmin()
최대와 최소값을 가진 위치를 반환하는 함수
np.argmax(data) 가 2라면 2번째 위치에 최대값이 존재한단 뜻

- numpy를 사용하면 배열간 연산을 쉽게 실행
- 연산은 +, -, *, / 등의 연산자를 사용할 수도 있고, add(), substract(), multiply(), divide() 등의 함수를 사용
- 예)배열 a 와 b 가 있을때, a + b를 하면 각 배열요소의 합을 구하는데 즉 a[0]+b[0], a[1]+b[1], ... 과 같은 방식으로 결과를 리턴

```python
import numpy as np

a = np.array([1,2,3])
b = np.array([4,5,6])

# 각 요소 더하기
c = a + b
# c = np.add(a, b)
print(c)  # [5 7 9]

# 각 요소 빼기
c = a - b
# c = np.subtract(a, b)
print(c)  # [-3 -3 -3]

# 각 요소 곱하기
# c = a * b
c = np.multiply(a, b)
print(c)  # [4 10 18]

# 각 요소 나누기
# c = a / b
c = np.divide(a, b)
print(c)  # [0.25 0.4 0.5]
```

- numpy에서 vector와 matrix의 product를 구하기 위해서 dot() 함수를 사용
- 예) 두 matrix의 product를 구한 예

```python
import numpy as np

lst1 = [
    [1,2],
    [3,4]
]

lst2 = [
    [5,6],
    [7,8]
]
a = np.array(lst1)
b = np.array(lst2)

c = np.dot(a, b)
print(c)
# 출력:
# [[19 22]
#  [43 50]]
```

- numpy은 배열간 연산을 위한 위해 많은 함수들을 제공
- 예) 각 배열 요소들을 더하는 sum() 함수, 각 배열 요소들을 곱하는 prod() 함수 등을 사용
- 이들 함수에 선택옵션으로 axis 을 지정할 수 있는데, 예를 들어 sum()에서 axis가 1 이면 행끼리 더하는 것이고, axis가 0 이면 열끼리 더하는 것

```python
import numpy as np

a = np.array([[1,2],[3,4]])

s = np.sum(a)
print(s)   # 10

# axis=0 이면, 컬럼끼리 더함
# axis=1 이면, 행끼리 더함
s = np.sum(a, axis=0)
print(s)   # [4 6]

s = np.sum(a, axis=1)
print(s)   # [3 7]

s = np.prod(a)
print(s)   # 24
```

## 형상 관련 함수

- ndarray.shape

```python
data = np.arange(1,7)
data.shape
(6,)
```

- np.reshape()

행렬의 차원 바꾸는데 사용, -1을 넣어 쭉 피는 용도로도 자주 사용
여러 차원을 넣을땐 꼭 ()로 묶어줘야합니다

```python
data = np.arange(1,7)
data.reshape((2,3))   # 2,3 행렬로 변경
# np.reshape(data, (2,3))   이렇게도 가능
array([[1, 2, 3],
       [4, 5, 6]])

data.reshape(-1)      # 쭉 필때
array([1, 2, 3, 4, 5, 6])

data.reshape((3,-1))    # 3행으로(열은 알아서)
array([[1, 2],
       [3, 4],
       [5, 6]])
```

- np.transpose()
행렬의 전치행렬을 구하는 것

```python
data = np.arange(1,7)
data = data.reshape((2,3,1))
print(data.shape)
data = data.transpose((0,2,1)) # 두번째와 세번째의 순서 변경
print(data.shape)
(2, 3, 1)
(2, 1, 3)
```

## 슬라이스

- numpy 배열은 파이썬 리스트와 마찬가지로 슬라이스(Slice)를 지원한다. numpy 배열을 슬라이싱하기 위해서는 각 차원별로 슬라이스 범위를 지정

```python
import numpy as np

lst = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
arr = np.array(lst)

# 슬라이스
a = arr[0:2, 0:2]
print(a)
# 출력:
# [[1 2]
#  [4 5]]

a = arr[1:, 1:]
print(a)
# 출력:
# [[5 6]
#  [8 9]]
```

## numpy 정수 인덱싱 (integer indexing)

- numpy 슬라이싱이 각 배열 차원별 최소-최대의 범위를 정하여 부분 집합을 구하는 것이라면, 정수 인덱싱은 각 차원별로 선택되어지는 배열요소의 인덱스들을 일렬로 나열하여 부분집합을 구하는 방식
- 즉, 임의의 numpy 배열 a 에 대해 a[[row1, row2], [col1, col2]] 와 같이 표현하는 것인데, 이는 a[row1, col1] 과 a[row2, col2] 라는 두 개의 배열요소의 집합을 의미
- 예) a[[0, 2], [1, 3]] 은 정수 인덱싱으로서 이는 a[0, 1] 과 a[2, 3] 등 2개의 배열요소

```python
import numpy as np

lst = [
    [1, 2, 3, 4],
    [5, 6, 7, 8],
    [9, 10, 11, 12]
]
a = np.array(lst)

# 정수 인덱싱
s = a[[0, 2], [1, 3]]

print(s)
# 출력
# [2 12]
```

## numpy 부울린 인덱싱 (boolean indexing)

- numpy 부울린 인덱싱은 배열 각 요소의 선택여부를 True, False로 표현하는 방식
- 만약 배열 a 가 2 x 3 의 배열이이라면, 부울린 인덱싱을 정의하는 numpy 배열도 2 x 3 으로 만들고 선택할 배열요소에 True를 넣고 그렇지 않으면 False
- 예) 3 x 3 배열 a 중 짝수만 뽑아내는 부울린 인덱싱 배열(numpy 배열)을 사용하여 짝수 배열 n 을 만드는 예

```python
import numpy as np

lst = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
a = np.array(lst)

bool_indexing_array = np.array([
    [False,  True, False],
    [True, False,  True],
    [False,  True, False]
])

n = a[bool_indexing_array];
print(n)
```

- 부울린 인덱싱 배열에 True/False 값을 일일이 지정하는 방법 이외에 표현식을 사용하여 부울린 인덱싱 배열을 생성하는 방법
- 예) 배열 a 에 대해 짝수인 배열요소만 True로 만들고 싶다면, bool_indexing = (a % 2 == 0) 와 같이 표현
- 마지막에 a[ a % 2 == 0 ] 와 같이 부울린 인덱싱 표현식을 배열 인덱스안에 넣어 간단하게 표현

```python
import numpy as np

lst = [
    [1, 2, 3],
    [4, 5, 6],
    [7, 8, 9]
]
a = np.array(lst)

# 배열 a 에 대해 짝수면 True, 홀수면 False
bool_indexing = (a % 2 == 0)

print(bool_indexing)
# 출력: 부울린 인덱싱 배열
# [[False  True False]
#  [ True False  True]
#  [False  True False]]

# 부울린 인덱스를 사용하여 True인 요소만 뽑아냄
print(a[bool_indexing])
# 출력:
# [2 4 6 8]

# 더 간단한 표현
n = a[ a % 2 == 0 ]
print(n)
```

## 난수 발생 함수들

- np.random.seed()
기본 시드지정함수로 랜덤이 들어간 함수에서 항상 같은 결과를 표출하기 위해 사용

```python
np.random.seed(42)
np.random.randn()
# 표준 정규분포에 따른 n 개의 무작위 숫자 생성

np.random.randn(5) # 5개의 숫자 생성
array([ 0.69406812, -1.15425813,  0.98864607,  0.73682549, -0.57181785])
```

- np.random.randint()
랜덤한 정수를 생성해줍니다. min 과 sup 지정도 가능

```python
np.random.randint(0, 10, 5)  # 0 이상 10 미만의 숫자 5개 랜덤 생성
array([7, 0, 7, 7, 2])
```
