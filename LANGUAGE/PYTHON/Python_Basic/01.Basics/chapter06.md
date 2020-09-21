# PYTHON

모듈 정리 추가 : <http://pythonstudy.xyz/python/article/16-%ED%95%A8%EC%88%98>
클래스 정리 추가 : <https://wikidocs.net/28>

## 클래스

### 1. 패키지

- 파이썬은 객체지향 프로그래밍(OOP, Object Oriented Programming)을 기본적으로 지원
- 파이썬에서 객체지향 프로그래밍의 기본 단위인 클래스를 만들기 위해서는 아래와 같이 "class 클래스명" 을 사용하여 정의
- 클래스명은 PEP 8 Coding Convention에 가이드된 대로 각 단어의 첫 문자를 대문자로 하는 CapWords 방식으로 명명

```python
class MyClass:
    pass
```

### 2. 클래스 멤버

- 클래스는 메서드(method), 속성(property), 클래스 변수(class variable), 인스턴스 변수(instance variable), 초기자(initializer), 소멸자(destructor) 등의 여러 맴버들을 가질 수 있는데, 크게 나누면 데이타를 표현하는 필드와 행위를 표현하는 메서드로 구분
- 파이썬에서 이러한 필드와 메서드는 모두 그 객체의 attribute
- 다른 OOP 언어와 달리 파이썬은 Dynamic Language로서 새로운 attribute를 동적으로 추가할 수 있고, 메서드도 일종의 메서드 객체로 취급하여 attribute에 포함

#### 메서드

- 메서드는 클래스의 행위를 표현하는 것으로 클래스 내의 함수
- 파이썬에서 메서드는 크게 인스턴스 메서드(instance method), 정적 메서드(static method), 클래스 메서드(class method)
- 가장 흔히 쓰이는 인스턴스 메서드는 인스턴스 변수에 엑세스할 수 있도록 메서드의 첫번째 파라미터에 항상 객체 자신을 의미하는 "self"라는 파라미터를 갖음
- 인스턴스 메서드는 여러 파라미터를 가질 수 있지만, 첫번째 파라미터는 항상 self 를 갖음.

```python
class Rectangle:
    count = 0  # 클래스 변수

    # 초기자(initializer)
    def __init__(self, width, height):
        # self.* : 인스턴스변수
        self.width = width
        self.height = height
        Rectangle.count += 1

    # 메서드
    def calcArea(self):
        area = self.width * self.height
        return area
```

#### 클래스 변수

- 클래스 정의에서 메서드 밖에 존재하는 변수를 클래스 변수(class variable)- 해당 클래스를 사용하는 모두에게 공용으로 사용되는 변수
- 클래스 변수는 클래스 내외부에서 "클래스명.변수명" 으로 엑세스

#### 인스턴스 변수

- 하나의 클래스로부터 여러 객체 인스턴스를 생성해서 사용
- 클래스 변수가 하나의 클래스에 하나만 존재하는 반면, 인스턴스 변수는 각 객체 인스턴스마다 별도로 존재
- 클래스 정의에서 메서드 안에서 사용되면서 "self.변수명"처럼 사용되는 변수를 인스턴스 변수(instance variable)라 하는데, 이는 각 객체별로 서로 다른 값을 갖는 변수
- 인스턴스 변수는 클래스 내부에서는 self.width 과 같이 "self." 을 사용하여 엑세스하고, 클래스 밖에서는 "객체변수.인스턴스변수"와 같이 엑세스

- Python은 다른 언어에서 흔히 사용하는 public, protected, private 등의 접근 제한자 (Access Modifier)를 갖지 않음
- Python 클래스는 기본적으로 모든 멤버가 public
- Python 코딩 관례(Convention)상 내부적으로만 사용하는 변수 혹은 메서드는 그 이름 앞에 하나의 밑줄(_) 을 붙임
- 실제 밑줄 하나를 사용한 멤버도 public 이므로 필요하면 외부에서 엑세스할 수 있음
- 만약 특정 변수명이나 메서드를 private으로 만들어야 한다면 두개의 밑줄(__)을 이름 앞에 붙이면 됨.

```python
def __init__(self, width, height):
    self.width = width
    self.height = height

    # private 변수 __area
    self.__area = width * height

# private 메서드
def __internalRun(self):
    pass
```

#### Initializer (초기자)

- 클래스로부터 새 객체를 생성할 때마다 실행되는 특별한 메서드로 \_\_init\_\_() 이라는 메서드가 있는데, 이를 흔히 클래스 Initializer (주: 파이썬에서 두개의 밑줄 (\_\_) 시작하고 두개의 밑줄로 끝나는 레이블은 보통 특별한 의미).
- Initializer는 클래스로부터 객체를 만들 때, 인스턴스 변수를 초기화하거나 객체의 초기상태를 만들기 위한 문장들을 실행하는 곳
- 위의 \_\_init\_\_() 예제를 보면, width와 height라는 입력 파라미터들을 각각 self.width와 self.height라는 인스턴스변수에 할당하여 객체 내에서 계속 사용할 수 있도록 준비
- (주: Python의 Initializer는 C#/C++/Java 등에서 일컫는 생성자(Constructor)와 약간 다름. Python에서 클래스 생성자(Constructor)는 실제 런타임 엔진 내부에서 실행되는데, 이 생성자(Constructor) 실행 도중 클래스 안에 Initializer가 있는지 체크하여 만약 있으면 Initializer를 호출하여 객체의 변수 등을 초기화한다.).

#### 정적 메서드와 클래스 메서드

- 인스턴스 메서드가 객체의 인스턴스 필드를 self를 통해 엑세스할 수 있는 반면, 정적 메서드는 이러한 self 파라미터를 갖지 않고 인스턴스 변수에 엑세스할 수 없다
- 따라서, 정적 메서드는 보통 객체 필드와 독립적이지만 로직상 클래스내에 포함되는 메서드에 사용
- 정적 메서드는 메서드 앞에 @staticmethod 라는 Decorator를 표시하여 해당 메서드가 정적 메서드임을 표시

- 클래스 메서드는 메서드 앞에 @classmethod 라는 Decorator를 표시하여 해당 메서드가 클래스 메서드임을 표시
- 클래스 메서드는 정적 메서드와 비슷한데, 객체 인스턴스를 의미하는 self 대신 cls 라는 클래스를 의미하는 파라미터를 전달받음
- 정적 메서드는 이러한 cls 파라미터를 전달받지 않음
- 클래스 메서드는 이렇게 전달받은 cls 파라미터를 통해 클래스 변수 등을 엑세스
- 일반적으로 인스턴스 데이타를 엑세스 할 필요가 없는 경우 클래스 메서드나 정적 메서드를 사용
- 보통 클래스 변수를 엑세스할 필요가 있을 때는 클래스 메서드를, 이를 엑세스할 필요가 없을 때는 정적 메서드를 사용
- isSquare() 메서드는 정적 메서드로서 cls 파라미터를 전달받지 않고 메서드 내에서 클래스 변수를 사용하지 않고 있다. 
- printCount() 메서드는 클래스 메서드로서 cls 파라미터를 전달받고 메서드 내에서 클래스 변수 count 를 사용하고 있다.

```python
class Rectangle:
    count = 0  # 클래스 변수

    def __init__(self, width, height):
        self.width = width
        self.height = height
        Rectangle.count += 1

    # 인스턴스 메서드
    def calcArea(self):
        area = self.width * self.height
        return area

    # 정적 메서드
    @staticmethod
    def isSquare(rectWidth, rectHeight):
        return rectWidth == rectHeight

    # 클래스 메서드
    @classmethod
    def printCount(cls):
        print(cls.count)

# 테스트
square = Rectangle.isSquare(5, 5)
print(square)   # True

rect1 = Rectangle(5, 5)
rect2 = Rectangle(2, 5)
rect1.printCount()  # 2
```

#### 그 밖의 특별한 메서드들

- 파이썬에는 Initializer 이외에도 객체가 소멸될 때 (Garbage Collection 될 때) 실행되는 소멸자(\_\_del\_\_) 메서드, 두 개의 객체를 ( + 기호로) 더하는 \_\_add\_\_ 메서드, 두 개의 객체를 ( - 기호로) 빼는 \_\_sub\_\_ 메서드, 두 개의 객체를 비교하는 \_\_cmp\_\_ 메서드, 문자열로 객체를 표현할 때 사용하는 \_\_str\_\_ 메서드 등 많은 특별한 용도의 메서드들이 있다. 아래 예제는 이 중 \_\_add\_\_() 메서드에 대한 예이다.

```python
def __add__(self, other):
    obj = Rectangle(self.width + other.width, self.height + other.height)
    return obj

# 사용 예
r1 = Rectangle(10, 5)
r2 = Rectangle(20, 15)
r3 = r1 + r2  # __add__()가 호출됨
```

### 3. 객체의 생성과 사용

- 클래스를 사용하기 위해서는 먼저 클래스로부터 객체(Object)를 생성
- 파이썬에서 객체를 생성하기 위해서는 "객체변수명 = 클래스명()"과 같이 클래스명을 함수 호출하는 것처럼 사용
- 만약 \_\_init\_\_() 함수가 있고, 그곳에 입력 파라미터들이 지정되어 있다면, "클래스명(입력파라미터들)"과 같이 파라미터를 괄호 안에 전달
- 전달된 파라미터들은 Initializer 에서 사용

```python
# 객체 생성
r = Rectangle(2, 3)

# 메서드 호출
area = r.calcArea()
print("area = ", area)

# 인스턴스 변수 엑세스
r.width = 10
print("width = ", r.width)

# 클래스 변수 엑세스
print(Rectangle.count)
print(r.count)
```

클래스로부터 생성된 객체(Object)로부터 클래스 멤버들을 호출하거나 엑세스할 수 있다. 인스턴스 메서드는 "객체변수.메서드명()"과 같이 호출할 수 있는데, 위의 예제에선 r.calcArea() 이 메서드 호출에 해당된다. 인스턴스 변수는 "객체변수.인스턴스변수" 으로 표현되며, 값을 읽거나 변경하는 일이 가능하다. 위의 예제 r.width = 10 은 인스턴스변수 width 에 새 값을 할당하는 예이다.

파이썬에서 특히 클래스 변수를 엑세스할 때, "클래스명.클래스변수명" 혹은 "객체명.클래스변수명"을 둘 다 허용하기 때문에 약간의 혼란을 초래할 수 있다. 예를 들어, 위의 예제에서 Rectangle.count 혹은 r.count은 모두 클래스 변수 count를 엑세스하는 경우로서 이 케이스에는 동일한 값을 출력한다.
하지만, 아래 예제와 같이 Rectangle 클래스의 클래스 변수 count를 Rectangle.count로 할당하지 않고 객체 r 로부터 할당하면 혼돈스러운 결과를 초래하게 된다.
파이썬에서 한 객체의 attribute에 값이 할당되면 (예를 들어, r.count = 10), 먼저 해당 객체에 이미 동일한 attribute가 있는지 체크해서 있으면 새 값으로 치환하고, 만약 그 attribute가 없으면 객체에 새로운 attribute를 생성하고 값을 할당한다. 즉, r.count = 10 의 경우 클래스 변수인 count를 사용하는 것이 아니라 새로 그 객체에 추가된 인스턴스 변수를 사용하게 되므로 클래스 변수값은 변경되지 않는다. 
파이썬에서 한 객체의 attribute를 읽을 경우에는 먼저 그 객체에서 attribute를 찾아보고, 없으면 그 객체의 소속 클래스에서 찾고, 다시 없으며 상위 Base 클래스에서 찾고, 그래도 없으면 에러를 발생시킨다. 따라서, 위 예제에서 클래스 변수값이 출력된 이유는 값을 할당하지 않고 읽기만 했기 때문에, r 객체에 새 인스턴스 변수를 생성하지 않게 되었고, 따라서 객체의 attribute가 없어서 클래스의 attribute를 찾았기 때문이다.
이러한 혼돈을 피하기 위해 클래스 변수를 엑세스할 때는 클래스명을 사용하는 것이 좋다.

```python
r = Rectangle(2, 3)

Rectangle.count = 50
r.count = 10   # count 인스턴스 변수가 새로 생성됨

print(r.count, Rectangle.count)  # 10  50 출력
```

## 4. 클래스 상속과 다형성

파이썬은 객체지향 프로그래밍의 상속(Inheritance)을 지원하고 있다. 클래스를 상속 받기 위해서는 파생클래스(자식클래스)에서 클래스명 뒤에 베이스클래스(부모클래스) 이름을 괄호와 함께 넣어 주면 된다. 즉, 아래 예제에서 Dog 클래스는 Animal 클래스로부터 파생된 파생클래스이며, Duck 클래스도 역시 Animal 베이스클래스로부터 파생되고 있다. (주: 파이썬은 복수의 부모클래스로부터 상속 받을 수 있는 Multiple Inheritance를 지원하고 있다.)

```python
class Animal:
    def __init__(self, name):
        self.name = name
    def move(self):
        print("move")
    def speak(self):
        pass

class Dog (Animal):
    def speak(self):
        print("bark")

class Duck (Animal):
    def speak(self):
        print("quack")
```

파생클래스는 베이스클래스의 멤버들을 호출하거나 사용할 수 있으며, 물론 파생클래스 자신의 멤버들을 사용할 수 있다.

```python
dog = Dog("doggy") # 부모클래스의 생성자
n = dog.name # 부모클래스의 인스턴스변수
dog.move()   # 부모클래스의 메서드
dog.speak()  # 파생클래스의 멤버
```

파이썬은 객체지향 프로그래밍의 다형성(Polymorphism)을 또한 지원하고 있다. 아래 예제는 animals 라는 리스트에 Dog 객체와 Duck 객체를 넣고 이들의 speak() 메서드를 호출한 예이다. 코드 실행 결과를 보면 객체의 타입에 따라 서로 다른 speak() 메서드가 호출됨을 알 수 있다.

```python
animals = [Dog('doggy'), Duck('duck')]

for a in animals:
    a.speak()
```