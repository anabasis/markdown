# PYTHON

## 모듈(Module)

### Modules/Names Import

```python
import 모둘1[, 모듈2[,... 모듈2]]
import math
a = math.factorial(5)

# 모듈에서 특정 함수만
from 모듈명 import 함수명
from math import factorial
a = factorial(5)

# 모듈에서 여러 함수만
from 모듈명 import (함수명1, 함수명2)
from math import (factorial,acos)
n = factorial(5) + acos(1)

# 모듈에서 모든 함수
from 모듈명 import *
from math import *
n = sqrt(5) + fabs(-12.5)

# Alias선언
from 모듈명 import 함수명1 as ALIAS1
from math import factorial as fac
n = fac(5)
```

### Modules 위치

- 현재디렉토리
- 환경변수 PYTHONPATH
- Python설치된 경로 및 하위 라이브러리 경로

```python
import sys
sys.path
```

### Modules 작성

- 현재디렉토리
- 환경변수 PYTHONPATH
- Python설치된 경로 및 하위 라이브러리 경로

```python
import sys
sys.path
```

### 동적 Modules 작성

#### \_\_import\_\_()

```python
def load_module_func(module_name) :
    # 방법1
    tmp = __import__(module_name)
    instance = getattr(모듈경로, module_name)

    # 방법2
    tmp = __import__(module_name, fromlist=[module_name])

    return tmp
```

#### getattr()

- getattr()은 지정한 object의 속성을 문자열 형태로 접근이 가능하도록 하는 함수
- 모듈의 class에 변수를 이용하여 접근이 가능함

```python
def load_module_func(module_name) :

    tmp = __import__(module_name, fromlist=[module_name])
    tmp_cls = getattr(tmp,module_class_name)
    return tmp_cls

tmp_cls.func1()
```