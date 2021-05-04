**Table of Contents**

- [Opening](#opening)
- [Array](#array)
- [Array Math](#array-math)
- [Array Indexing](#array-indexing)
- [Numpy Data Handling](#numpy-data-handling)
- [Matrix Algebra with Numpy](#matrix-algebra-with-numpy)

# Opening

(https://docs.scipy.org/doc/numpy/index.html)  
Numpy is the core library for scientific computing in Python, especially "Array" generator.  
It provides a high-performance multidimensional array object, and tools for working with these arrays.  
If you are already familiar with MATLAB, you might find this tutorial useful to get started with Numpy.  

# Array

- A numpy array is a grid of values, all of the same type, and is indexed by a tuple of nonnegative integers.  
- Rank of the array: the number of dimensions  
- Shape of an array: a tuple of integers giving the size of the array along each dimension.

```python
# Functions for generating scalar, vector, and matrix
import numpy as np
a = np.array([1,2,3])
print(a)
print(type(a))
print(a.shape)
a[0] = 5
print(a)
print(np.rank(a))
print(a.shape, '\n')

b = np.array([[1,2,3], [4,5,6]])
print(b)
print(type(b))
print(np.rank(b))
print(b.shape)
```

    [1 2 3]
    <class 'numpy.ndarray'>
    (3,)
    [5 2 3]
    1
    (3,) 
    
    [[1 2 3]
     [4 5 6]]
    <class 'numpy.ndarray'>
    2
    (2, 3)
    

    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:9: VisibleDeprecationWarning: `rank` is deprecated; use the `ndim` attribute or function instead. To find the rank of a matrix see `numpy.linalg.matrix_rank`.
      if __name__ == '__main__':
    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:15: VisibleDeprecationWarning: `rank` is deprecated; use the `ndim` attribute or function instead. To find the rank of a matrix see `numpy.linalg.matrix_rank`.
      from ipykernel import kernelapp as app
    

| 함수 | 내용 |
|------------|-----------------------------------------------------------------------------------------------------|
| np.array | 입력된 데이터를 ndarray로 변환. dtype을 명시하면 자료형을 설정할 수 있다 |
| np.asarray | 입력 데이터를 ndarray로 변환하나 이미 ndarray일 경우에는 새로 메모리에 ndarray가 생성되지는 않는다 |
| np.arange | range 함수와 유사하나 ndarray를 반환 |
| np.ones | 전달인자로 전달한 dtype과 모양(행,렬)으로 배열을 생성하고 모든 내용을 1로 초기화하여 ndarray를 반환 |
| np.zeros | ones와 같으나 초기값이 0이다 |
| np.empty | ones와 zeros와 비슷하나 값을 초기화하지는 않는다 |


```python
# Functions for generating data
a = np.zeros((3,3))
print(a)

b = np.ones((1,3))
print(b)

c = np.full((3,3), 7)
print(c)

d = np.eye(3)
print(d)

e = np.random.random((2,2))
print(e)

```

    [[0. 0. 0.]
     [0. 0. 0.]
     [0. 0. 0.]]
    [[1. 1. 1.]]
    [[7 7 7]
     [7 7 7]
     [7 7 7]]
    [[1. 0. 0.]
     [0. 1. 0.]
     [0. 0. 1.]]
    [[0.15184718 0.54838015]
     [0.75311903 0.32889525]]
    


```python
print(np.array(np.random.random((2,2)), dtype=np.float64))
print(np.array(np.random.random((2,2)), dtype=np.complex))
print(np.array(np.random.random((2,2)), dtype=np.int))
print(np.array(np.random.random((2,2)), dtype=np.bool))
print(np.array(np.random.random((2,2)), dtype=np.object))
```

    [[0.72038126 0.13581294]
     [0.7290755  0.89247687]]
    [[0.88379784+0.j 0.32741343+0.j]
     [0.26558431+0.j 0.8743355 +0.j]]
    [[0 0]
     [0 0]]
    [[ True  True]
     [ True  True]]
    [[0.91699367500707 0.6695660621906848]
     [0.1841214730339662 0.0931911999371059]]
    

# Array Math
Basic mathematical functions operate elementwise on arrays, and are available both as operator overloads and as functions in the numpy built-in function module(Numpy Universal Functions):
- A universal function (or ufunc for short) is a function that operates on ndarrays in an element-by-element fashion, supporting array broadcasting, type casting, and several other standard features.  

| 함수 | 설명 |
|------------------------|--------------------------------------------------------------------------|
| abs, fabs | 각 원소의 절대값을 구한다. 복소수가 아닌 경우에는 fabs로 빠르게 연산가능 |
| sqrt | 제곱근을 계산 arr ** 0.5와 동일 |
| square | 제곱을 계산 arr ** 2와 동일 |
| Exp | 각 원소에 지수 ex를 계산 |
| Log, log10, log2, logp | 각각 자연로그, 로그10, 로그2, 로그(1+x) |
| sign | 각 원소의 부호를 계산 |
| ceil | 각 원소의 소수자리 올림 |
| floor | 각 원소의 소수자리 버림 |
| rint | 각 원소의 소수자리 반올림. dtype 유지 |
| modf | 원소의 몫과 나머지를 각각 배열로 반환 |

| 함수 | 설명 |
|------------------------------------------------------------|----------------------------------------------------------------------|
| add | 두 배열에서 같은 위치의 원소끼리 덧셈 |
| subtract | 첫번째 배열 원소 - 두번째 배열 원소 |
| multiply | 배열의 원소끼리 곱셈 |
| divide | 첫번째 배열의 원소에서 두번째 배열의 원소를 나눗셈 |
| power | 첫번째 배열의 원소에 두번째 배열의 원소만큼 제곱 |
| maximum, fmax | 두 원소 중 큰 값을 반환. fmax는 NaN 무시 |
| minimum, fmin | 두 원소 중 작은 값 반환. fmin는 NaN 무시 |
| mod | 첫번째 배열의 원소에 두번째 배열의 원소를 나눈 나머지 |
| greater, greater_equal, less, less_equal, equal, not_equal | 두 원소 간의 >, >=, <, <=, ==, != 비교연산 결과를 불리언 배열로 반환 |
| logical_and, logical_or, logical_xor | 각각 두 원소 간의 논리연산 결과를 반환 |


```python
a2d = np.array([[1,2],[3,4]], dtype=np.float64)
b2d = np.array([[5,6],[7,8]], dtype=np.float64)
print(a2d)
print(b2d)
print('\n')

# Elementwise sum; both produce the array
print(a2d + b2d)
print(np.add(a2d, b2d))
print('\n')

# Elementwise difference; both produce the array
print(a2d - b2d)
print(np.subtract(a2d, b2d))
print('\n')

# Elementwise product; both produce the array
print(a2d * b2d)
print(np.multiply(a2d, b2d))
print('\n')

# Elementwise division; both produce the array
print(a2d / b2d)
print(np.divide(a2d, b2d))
print('\n')

# Elementwise square root; produces the array
print(np.sqrt(a2d))
```

    [[1. 2.]
     [3. 4.]]
    [[5. 6.]
     [7. 8.]]
    
    
    [[ 6.  8.]
     [10. 12.]]
    [[ 6.  8.]
     [10. 12.]]
    
    
    [[-4. -4.]
     [-4. -4.]]
    [[-4. -4.]
     [-4. -4.]]
    
    
    [[ 5. 12.]
     [21. 32.]]
    [[ 5. 12.]
     [21. 32.]]
    
    
    [[0.2        0.33333333]
     [0.42857143 0.5       ]]
    [[0.2        0.33333333]
     [0.42857143 0.5       ]]
    
    
    [[1.         1.41421356]
     [1.73205081 2.        ]]
    


```python
a2d = np.array([[-1,2,3],[-4,5,6],[7,8,-9]])
print(a2d)
print(np.abs(a2d))
print(np.fabs(a2d))
print(np.sqrt(a2d))
# is same as
print(a2d**0.5)
print(np.sign(a2d))
print(np.ceil(a2d))
print('\n')

b2d = np.array([[1,2,3],[4,5,6],[7,8,9]])
print(b2d)
print(np.add(a2d, b2d))
print(np.multiply(a2d, b2d))
print(np.power(a2d, b2d))
print(np.greater(b2d, a2d))
print(np.logical_and(a2d, a2d+b2d))
```

    [[-1  2  3]
     [-4  5  6]
     [ 7  8 -9]]
    [[1 2 3]
     [4 5 6]
     [7 8 9]]
    [[1. 2. 3.]
     [4. 5. 6.]
     [7. 8. 9.]]
    [[       nan 1.41421356 1.73205081]
     [       nan 2.23606798 2.44948974]
     [2.64575131 2.82842712        nan]]
    [[       nan 1.41421356 1.73205081]
     [       nan 2.23606798 2.44948974]
     [2.64575131 2.82842712        nan]]
    [[-1  1  1]
     [-1  1  1]
     [ 1  1 -1]]
    [[-1.  2.  3.]
     [-4.  5.  6.]
     [ 7.  8. -9.]]
    
    
    [[1 2 3]
     [4 5 6]
     [7 8 9]]
    [[ 0  4  6]
     [ 0 10 12]
     [14 16  0]]
    [[ -1   4   9]
     [-16  25  36]
     [ 49  64 -81]]
    [[        -1          4         27]
     [       256       3125      46656]
     [    823543   16777216 -387420489]]
    [[ True False False]
     [ True False False]
     [False False  True]]
    [[False  True  True]
     [False  True  True]
     [ True  True False]]
    

    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:5: RuntimeWarning: invalid value encountered in sqrt
      """
    C:\ProgramData\Anaconda3\lib\site-packages\ipykernel_launcher.py:7: RuntimeWarning: invalid value encountered in power
      import sys
    

# Array Indexing
Numpy offers several ways to index into arrays.  
(http://cs231n.github.io/python-numpy-tutorial/#scipy-image)
- **Slicing:** Similar to Python lists, numpy arrays can be sliced. Since arrays may be multidimensional, you must specify a slice for each dimension of the array.  
- **Integer array indexing:** When you index into numpy arrays using slicing, the resulting array view will always be a subarray of the original array. In contrast, integer array indexing allows you to construct arbitrary arrays using the data from another array.  
- **Boolean array indexing:** Boolean array indexing lets you pick out arbitrary elements of an array. Frequently this type of indexing is used to select the elements of an array that satisfy some condition.  



```python
a = np.arange(10)
print(a)
print(a[5])
print(a[5:8])
print('\n')

a = np.array([[1,2,3,4], [5,6,7,8], [9,10,11,12]])
print(a)
print(a[1,:])
print(a[1:2,:])
print(a[1:3,:])
print(a[:,1:3])
print('\n')

a = np.array([[1,2], [3, 4], [5, 6]])
print(a)
print(a[[0, 1, 2], [0, 1, 0]])
print(np.array([a[0, 0], a[1, 1], a[2, 0]]))
print('\n')

a = np.array([[1,2,3], [4,5,6], [7,8,9], [10, 11, 12]])
print(a)
b = np.array([0, 2, 0, 1])
print(a[np.arange(4), b])
print(a[np.arange(4), b] + 10)
print('\n')

a = np.array([[1,2], [3, 4], [5, 6]])
print(a)
print(a>2)
print(a[a>2])
```

    [0 1 2 3 4 5 6 7 8 9]
    5
    [5 6 7]
    
    
    [[ 1  2  3  4]
     [ 5  6  7  8]
     [ 9 10 11 12]]
    [5 6 7 8]
    [[5 6 7 8]]
    [[ 5  6  7  8]
     [ 9 10 11 12]]
    [[ 2  3]
     [ 6  7]
     [10 11]]
    
    
    [[1 2]
     [3 4]
     [5 6]]
    [1 4 5]
    [1 4 5]
    
    
    [[ 1  2  3]
     [ 4  5  6]
     [ 7  8  9]
     [10 11 12]]
    [ 1  6  7 11]
    [11 16 17 21]
    
    
    [[1 2]
     [3 4]
     [5 6]]
    [[False False]
     [ True  True]
     [ True  True]]
    [3 4 5 6]
    


```python
# is similar as "Python List", but..
b = list(range(10))
print(b)
print(b[5:8])

a[5:8] = 100
print(a)
b[5:8] = 100
print(b)
```

    [0, 1, 2, 3, 4, 5, 6, 7, 8, 9]
    [5, 6, 7]
    [[1 2]
     [3 4]
     [5 6]]
    


    ---------------------------------------------------------------------------

    TypeError                                 Traceback (most recent call last)

    <ipython-input-7-2deac86225b3> in <module>
          6 a[5:8] = 100
          7 print(a)
    ----> 8 b[5:8] = 100
          9 print(b)
    

    TypeError: can only assign an iterable



```python
names = np.array(['철수', '영희', '말자', '숙자'])
print(names)
names == '철수'
```

    ['철수' '영희' '말자' '숙자']
    




    array([ True, False, False, False])




```python
data = np.random.randn(4,4)
print(data)
data[data<0] = 0
print(data)
data[names == '철수']
```

    [[ 1.57285632 -0.74975911  1.01052522 -0.11128709]
     [-1.59985091 -0.92836754  0.81365947 -0.62043759]
     [-1.32212764 -2.13044909  0.40301989  0.38829275]
     [-2.19182118  0.09649667 -1.16889098 -1.4226416 ]]
    [[1.57285632 0.         1.01052522 0.        ]
     [0.         0.         0.81365947 0.        ]
     [0.         0.         0.40301989 0.38829275]
     [0.         0.09649667 0.         0.        ]]
    




    array([[1.57285632, 0.        , 1.01052522, 0.        ]])



# Numpy Data Handling


```python
# Statistics
a2d = np.array([[-1,2,3],[-4,5,6],[7,8,-9]])
print(a2d)
print(a2d + 2)
print(a2d * 2)
print(np.sum(a2d, axis=0))
print(np.sum(a2d, axis=1))
print(np.mean(a2d, axis=0))
print(np.std(a2d, axis=0))
print(np.var(a2d, axis=0))
print(np.min(a2d, axis=0))
print(np.max(a2d, axis=0))
print(np.argmin(a2d, axis=0))
print(np.argmax(a2d, axis=0))
print(np.cumsum(a2d, axis=0))
print(np.cumprod(a2d, axis=0))
```

    [[-1  2  3]
     [-4  5  6]
     [ 7  8 -9]]
    [[ 1  4  5]
     [-2  7  8]
     [ 9 10 -7]]
    [[ -2   4   6]
     [ -8  10  12]
     [ 14  16 -18]]
    [ 2 15  0]
    [4 7 6]
    [0.66666667 5.         0.        ]
    [4.64279609 2.44948974 6.4807407 ]
    [21.55555556  6.         42.        ]
    [-4  2 -9]
    [7 8 6]
    [1 0 2]
    [2 2 1]
    [[-1  2  3]
     [-5  7  9]
     [ 2 15  0]]
    [[  -1    2    3]
     [   4   10   18]
     [  28   80 -162]]
    


```python
a2d = np.array([[-1,2,3],[-4,5,6],[7,8,-9]])
b2d = np.array([[1,2,3],[4,5,6],[7,8,9]])

c2d = np.concatenate((a2d, b2d), axis=0)
print(c2d)
c2d = np.concatenate((a2d, b2d), axis=1)
print(c2d)

c2d.reshape((9,2))
```

    [[-1  2  3]
     [-4  5  6]
     [ 7  8 -9]
     [ 1  2  3]
     [ 4  5  6]
     [ 7  8  9]]
    [[-1  2  3  1  2  3]
     [-4  5  6  4  5  6]
     [ 7  8 -9  7  8  9]]
    




    array([[-1,  2],
           [ 3,  1],
           [ 2,  3],
           [-4,  5],
           [ 6,  4],
           [ 5,  6],
           [ 7,  8],
           [-9,  7],
           [ 8,  9]])




```python
a2d = np.array([[1, 2, 3, 4], [5, 6, 7, 8], [9, 10, 11, 12], [13 ,14 ,15, 16]])
print(a2d)
print(np.split(a2d, 2, axis=0))
print(np.split(a2d, 2, axis=1))
print(np.split(a2d, [2,3], axis=0))
print(np.split(a2d, [2,3], axis=1))
```

    [[ 1  2  3  4]
     [ 5  6  7  8]
     [ 9 10 11 12]
     [13 14 15 16]]
    [array([[1, 2, 3, 4],
           [5, 6, 7, 8]]), array([[ 9, 10, 11, 12],
           [13, 14, 15, 16]])]
    [array([[ 1,  2],
           [ 5,  6],
           [ 9, 10],
           [13, 14]]), array([[ 3,  4],
           [ 7,  8],
           [11, 12],
           [15, 16]])]
    [array([[1, 2, 3, 4],
           [5, 6, 7, 8]]), array([[ 9, 10, 11, 12]]), array([[13, 14, 15, 16]])]
    [array([[ 1,  2],
           [ 5,  6],
           [ 9, 10],
           [13, 14]]), array([[ 3],
           [ 7],
           [11],
           [15]]), array([[ 4],
           [ 8],
           [12],
           [16]])]
    

# Matrix Algebra with Numpy
- Note that unlike MATLAB, * is elementwise multiplication, not matrix multiplication. We instead use the dot function to compute inner products of vectors, to multiply a vector by a matrix, and to multiply matrices. dot is available both as a function in the numpy module and as an instance method of array objects.


```python
a = np.array([[1,2],[3,4]])
b = np.array([[5,6],[7,8]])
v = np.array([9,10])
w = np.array([11,12])
print(a)
print(b)
print(v)
print(w)
print('\n')

# Inner product of vectors; both produce 219
print(v.dot(w))
print(np.dot(v, w))
print('\n')

# Matrix / vector product; both produce the rank 1 array [29 67]
print(a.dot(v))
print(np.dot(a, v))
print('\n')

# Matrix / matrix product; both produce the rank 2 array
print(a.dot(b))
print(np.dot(a, b))
print('\n')

a2d = np.array([[1, 2, 3], [4, 5, 6]])
print(a2d)
print(a2d.transpose())
print(np.dot(a2d, a2d.transpose()))
print(np.dot(a2d, a2d.T))
print(np.linalg.inv(np.dot(a2d, a2d.T)))
```

    [[1 2]
     [3 4]]
    [[5 6]
     [7 8]]
    [ 9 10]
    [11 12]
    
    
    219
    219
    
    
    [29 67]
    [29 67]
    
    
    [[19 22]
     [43 50]]
    [[19 22]
     [43 50]]
    
    
    [[1 2 3]
     [4 5 6]]
    [[1 4]
     [2 5]
     [3 6]]
    [[14 32]
     [32 77]]
    [[14 32]
     [32 77]]
    [[ 1.42592593 -0.59259259]
     [-0.59259259  0.25925926]]
    

- Broadcasting two arrays together follows these rules:  
    - If the arrays do not have the same rank, prepend the shape of the lower rank array with 1s until both shapes have the same length.
    - The two arrays are said to be compatible in a dimension if they have the same size in the dimension, or if one of the arrays has size 1 in that dimension.
    - The arrays can be broadcast together if they are compatible in all dimensions.
    - After broadcasting, each array behaves as if it had shape equal to the elementwise maximum of shapes of the two input arrays.
    - In any dimension where one array had size 1 and the other array had size greater than 1, the first array behaves as if it were copied along that dimension


```python
a2d = np.array([[1,2,3], [4,5,6], [7,8,9], [10,11,12]])
b1d = np.array([1,0,1])
print(a2d)
print(b1d)
c2d = np.empty_like(a2d)
for i in range(4):
    c2d[i, :] = a2d[i, :] + b1d
print(c2d)
```

    [[ 1  2  3]
     [ 4  5  6]
     [ 7  8  9]
     [10 11 12]]
    [1 0 1]
    [[ 2  2  4]
     [ 5  5  7]
     [ 8  8 10]
     [11 11 13]]
    


```python
# When the matrix a2d is very large, computing an explicit loop in Python could be slow
bb = np.tile(b1d, (4, 1))
print(bb)
c2d = a2d + bb
print(c2d)
```

    [[1 0 1]
     [1 0 1]
     [1 0 1]
     [1 0 1]]
    [[ 2  2  4]
     [ 5  5  7]
     [ 8  8 10]
     [11 11 13]]
    


```python
c2d = a2d + b1d
print(c2d)
```

    [[ 2  2  4]
     [ 5  5  7]
     [ 8  8 10]
     [11 11 13]]
    


```python
# Compute outer product of vectors
v = np.array([1,2,3])
w = np.array([4,5])
print(v)
print(w)
# To compute an outer product, we first reshape v to be a column vector of shape (3, 1); 
# we can then broadcast it against w to yield an output of shape (3, 2), which is the outer product of v and w:
print(np.reshape(v, (3,1)) * w)
print('\n')

# Add a vector v to each row of a matrix x
x = np.array([[1,2,3], [4,5,6]])
print(x)
print(x + v)
print('\n')

# Add a vector w to each column of a matrix x
print(x)
print((x.T + w).T)
print(x + np.reshape(w, (2, 1)))
```

    [1 2 3]
    [4 5]
    [[ 4  5]
     [ 8 10]
     [12 15]]
    
    
    [[1 2 3]
     [4 5 6]]
    [[2 4 6]
     [5 7 9]]
    
    
    [[1 2 3]
     [4 5 6]]
    [[ 5  6  7]
     [ 9 10 11]]
    [[ 5  6  7]
     [ 9 10 11]]
    


```python

```
