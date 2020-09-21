# PYTHON

## CHAPTER01

참조 : 점프투파이썬 <https://wikidocs.net/26>
참조 : 예제로 배우는 파이썬 프로그래밍 <http://pythonstudy.xyz/Python/Basics>

### 기본특징

- 변수등에 대소문자 구분.
- C언어와 다르게 변수 지정할 경우에 "형" 지정이 없음.
- 들여쓰기로 구분함
- 파이썬2.x는 기본 인코딩이 ascii, 파이썬3.x는 Unicode
- 맨 위에 **#-*-encoding:utf-8-*\-** 을 입력해주어야 한글 주석이 가능함.

### 변수선언

- 변수선언 : 항상 왼쪽에 문자로 선언(=대입연산자)
- 값을 할당하는 것이 아니라 서로 다른 객체들을 서로 **연결**하는 것

### 변수의 자료형

- 숫자 - 정수형, 실수형
- 문자 - 문자/문자열
- 리스트 - 여러개 모아놓은 것 [1,2,3,4]
- 듀플 - 리스트와 비슷, 값변경 못함
- 딕셔너리 - 사전구조{"a",1234}

### 연산자

- +-*/
- //(몫) \**(승수)
- &(AND) |(OR)
- ==(같음) !=(다름)

### 문자열

- ',"
- '," 이 데이터로 있어야 하는 경우 "',""" 사용
- 문자열 연결 : +
- str(1) : 확실하게 문자열로 인식

### 리스트

- []
- 추가 : append(뒤에 추가), insert(원하는 위치에 추가)
- 삭제 : pop(위치), remove(값)
- 리스트끼리 합치기 : +
- 리스트끼리 반복 : \*
- list = [n \** 2 for n in range(10) if n%3 == 0] range(10)까지 n%3==0 을 만족하는 n\**2 값

### Dictionary(딕셔너리)

- key + value
- 생성 : {}

### 조건문(if)

```python
if 조건문1 :
    내용
elif 조건문2 :
    내용
else :
    내용
```

### 반복문(for)

```python
for 변수 in (범위)
```

- range(숫자), range(시작점,끝점,간격)

```python
# range([start,][end][,step])
range(5) # 0,1,2,3,4
range(3,8) # 3,4,5,6,7
range(2,12,3) # 2,5,8,11
range(20,5,-5) # 20,15,10
range(len(seq)) # sequence of index of values in seq
```

### 정리

> **Base Type : integer, float, boolean, string, bytes**

```python
int : 783, 0(null), -192, 0b010(binary), 0o642(octal), 0xF3(hexa)
float : 9.23, 0.0, -1.7e-6(*10^-6)
bool : true, false
str : "One\nTwo"(escaped new line), """X\tY\tZ1\t2\t3""" (escaped tab), 'I\'m'(escaped ')
bytes : b"toto\xfe\775"
```

> **Container Type**

```python
# Ordered sequences : fast index access, repeatable values
list [] : [1, 5, 9], ["x", 11, 8.9], ["mot"]
tuple () : (1, 5, 9), , ("mot")
str bytes b"" : ordered sequences of chars/bytes

# Key containers : no a priori order, fast key access, each key is unique
dictionary dict {} : {"key":"value"}, dict(a=3,b=4,c="K"), {1:"one",3:"three",2:"two",3.14:"pi"}
collection set : {"value1","value2"}, {1,9,3,0}, set() empty --> hashtable values
```

> **변수선언(Variables assignment)**

```python
# assignment --> binding of a name with a value
x = 1.2 + 8 + sin(y)
a = b = c = 0 # assingment to same value
y,z,r = 9.2, -7, 0 # multiple assignments
a,b = b,a  # value swap
a, *b = seq # unpacking of sequence in item and list
*a, b = seq # 위 상동
x+=3 # increment --> x = x + 3  , *=, /=, %=
x-=2 # decrement --> x = x - 2
x=None # undefined >> constant value
del x # remove name x
```

> **변환(Conversions)**

```python
# type(expression)
int("15") # 15
int("3f",16) # 63 can specify integer number base in 2rd parameter
int(15.56) # 15
float("-11.24e8") # -1124000000.0
round(15.56,1) # 15.6
bool(x) # False for null x, empty container x, None or False x ; True for other x
str(x) # "..." representation string of x for display
chr(64) # '@' ord('@') --> 64, code <--> char
repr(x) # "..." literal representation string of x
bytes([72,9,64]) # b'H\t@'
list("abc") # ['a','b','c']
dict([(3,"three"),(1,"one")]) # {1:'one',3:'three'}
set(["one","two"]) # {'one','two'}

#assempled
':'.join(['toto','12','pswd']) # toto:12:pswd
# splitted on whitespaces
"words with spaces".split() # ["words", "with", "spaces"]
# splitted on separator
"1, 4, 8, 2".split(",") # ['1','4','8','2']
[int(x) for x in ('1', '29', '-3')] # [1, 29, -3]
```

> **Sequence Containers Indexing**

![셈플](../images/seq_container_indexing.png)

```python
# sub-sequences via lst[start slice:end slice:step]
lst[:-1]  # [10,20,30,40]
lst[1:-1]  # [20,30,40]
lst[::2]  # [10,30,50]

lst[::-1]  # [50,40,30,20,10]
lst[::-2]  # [50,30,10]
lst[:]  # [10,20,30,40,50]

lst[1:3]  # [20,30]
lst[-3:-1]  # [30,40]

lst[:3]  # [10,20,30]
lst[3:]  # [40,50]
```

> **Exception on Errors**

```python
# type(expression)
Signaling an error :
    raise ExcClass(...)
Error processing :
try
    normal processing block
except Exception:
    error processing block
except Exception as e :
    error processing block
finally :
    block for final processing in all cases
```

## 유용한 함수

### List명령어

```python
lst.append(val) # add item at end (item)
lst.extend(seq) # add sequence of items at end (list)
lst.insert(idx,val) # insert item at index (position, item)
lst.remove(val) # remove first item with value val (item)
lst.pop([idx]) #remove & return item at index idx(default last) (position)
lst.sort() # sort
lst.reverse() # reverse
lst.count(item)
lst.index(item)
```

### Dictionary사전 명령어

```python
# 생성 dic[key]=value, 삭제 dic.clear(), del dic[key]
dic.update(d2) # update/add associations
dic.keys() #
dic.values() #
dic.items() #
dic.pop(key[,default]) #
dic.popitem() # (key, value)
dic.get(key[,default]) # value
dic.setdefault(key[,default]) # value
```

### Set 명령어

```python
basket = {'apple', 'orange', 'apple', 'pear', 'orange', 'banana'}
print(basket) # show that duplicates have been removed
{'orange', 'banana', 'pear', 'apple'}
'orange' in basket # fast membership testing
True
'crabgrass' in basket
False

# Demonstrate set operations on unique letters from two words
a = set('abracadabra')
b = set('alacazam')
a # unique letters in a
{'a', 'r', 'b', 'c', 'd'}
a - b # letters in a but not in b
{'r', 'd', 'b'}
a | b # letters in a or b or both
{'a', 'c', 'r', 'd', 'b', 'm', 'z', 'l'}
a & b # letters in both a and b
{'a', 'c'}
a ^ b # letters in a or b but not both
{'r', 'd', 'b', 'm', 'z', 'l'}

#Similarly to list comprehensions, set comprehensions are also supported:
a = {x for x in 'abracadabra' if x not in 'abc'}
a
{'r', 'd'}

s.update(s2) s.copy()
s.add(key) s.remove(key)
s.discard(key) s.clear()
s.pop()
```

### String 명령어

```python
s.startswith(prefix[,start[,end]])
s.endswith(suffix[,start[,end]])
s.strip([chars])
s.count(sub[,start[,end]])
s.partition(sep),s.rpartition(sep) # (before,sep,after)
s.index(sub[,start[,end]]), s.rindex (sub, start, end)
s.find(sub[,start[,end]])
s.upper(),s.lower()
s.title()
s.swapcase(),s.casefold(),s.capitalize()
s.center([width,fill]),s.ljust([width,fill]),s.rjust([width,fill]),s.zfill([width])
s.encode(encoding),s.decode()
s.split([sep]),s.rsplit(sep),s.splitlines()
s.join(seq),s.replace(old, new)
s.rstrip(),s.lstrip()
s.rfind(sub, start ,end)
s.expandtabs()
s.is…() # tests on chars categories (ex. s.isalnum(),s.isalpha(),s.isdigit(),s.islower(),s.isspace() )
```

### File 명령어

```python
f = open("file.txt","w",encoding="utf8")
# file variable for operations
# name of file on disk(+path)
# opening mode r/w/a
# encoding of clars for text files(utf8,ascii,latin1...)

# Writing
f.write("test write!!")
f.writelines(list of lines)
f.close()

#Reading
f.read([n])
f.readlines([n])
f.readline()
f.close()

f.flush()
f.truncate([taille])
f.tell()
f.seek(position[,origin])

# General Common
with open() as f :
    for line in f :
        # processing of line
```
