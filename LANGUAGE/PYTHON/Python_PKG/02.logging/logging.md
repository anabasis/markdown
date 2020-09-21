# logging

- logging.conf 파일을 통해서 외부에서 설정 가능 
- 레벨을 자유롭게 설정하고 추가 할 수 있어야 한다. (CRITICAL, ERROR,INFO, DEBUG )
- 파일,스트림에 동시에 출력 할 수 있어야 한다.
- 다양한 목적에 따라 다양한 파일에 출력 할 수 있어야 한다.
- 로깅 시간 출력 및 다양한 정보에 대한 추가가 가능해야 한다.
- 하루에 한번씩 파일을 생성 해야하며, 지난 파일은 압축하여 보관 해야한다.
- 하루에 한번씩 파일을 생성 해야하며, 오래된 파일을 삭제 할 수 있어야 한다.
- 파일 용량이 너무 커질 경우에는 자동으로 분리 시켜야 한다.
- 멀티프로세스를 활용해야 하는 파이썬의 특성상 멀티프로세스에서도 로그를 취득할 수 있어야 한다.
- 핸들러를 커스터마이징 할 수 있어야 한다. 즉 콘솔,파일 출력 뿐 만 아니라, DB 및 소켓을 통하여도 발송 할 수 있어야 한다.
- 사용하기 쉬워야 한다. (문서화가 잘 되 있어야 한다) , 기본 로깅 모듈을 사용해야한다.

## 초급

### 로깅 모듈 사용하기

파이썬에서 로깅 모듈은 기본 라이브러리에 포함되어 있기 때문에, 굳이 따로 설치 할 필요는 없습니다.

```python
import logging

if __name__ == '__main__':
    logging.error("something wrong!!")

##결과: ERROR:root:somthing wrong!!
```

### 로깅 레벨

보통 DEBUG, INFO, WARNING, ERROR, CRITICAL 의 5가지 등급이 기본적으로 사용됩니다.

```python
import logging

if __name__ == '__main__':
    logging.info("hello world")

# 응?  출력이 안되네요? 
```

보통 DEBUG 레벨에서 굉장히 많은 정보들을 출력하게 되는데, 개발시에만 DEBUG 레벨의 정보를 보고 싶고, 실제 서비스를 할 경우는 DEBUG 레벨은 보고싶지 않을 수 있습니다.
INFO 레벨보다 심각한 것만 출력하게 하라!! 라고 로깅 시스템을 사용하면 설정 가능합니다.

파이썬 로깅의 기본 설정은 WARNING 입니다. 따라서  DEBUG < INFO < WARNING < ERROR < CRITICAL  
이 순서에서 더 높은 레벨인 ERROR 는 출력이 되지만, 하위레벨 (INFO,DEBUG) 은 출력이 안됩니다.
즉 이 레벨을 DEBUG 나 INFO 로 낮추어 설정 한후에 사용해야 합니다. 설정하는 방법은 조금 있다가 살펴보죠.

### 나의 로깅 모듈 사용하기 - 1 (생성)

제일 처음에  logging.error("somthing wrong") 이렇게 error 함수를 바로 사용 했었는데요.
근데 이렇게 직접 함수를 호출하기도 하지만, 자신만의 특정한 로거를 따로 만들어서 사용하는게 보통입니다.

```python
import logging

if __name__ == '__main__':
    mylogger = logging.getLogger("my")
```

이렇게 호출하면 "my" 라는 특정 로거를 생성하게 됩니다.

### 나의 로깅 모듈 사용하기 - 2 (레벨 설정)

```python
import logging

if __name__ == '__main__':
    mylogger = logging.getLogger("my")
    mylogger.setLevel(logging.INFO)
```

setLevel 메소드를 통해서 INFO 레벨 이상은 출력 하도록 설정 하였습니다. (DEBUG 출력 안함)

### 나의 로깅 모듈 사용하기 - 3 (핸들러 설정)

```python
import logging

if __name__ == '__main__':
    mylogger = logging.getLogger("my")
    mylogger.setLevel(logging.INFO)

    stream_hander = logging.StreamHandler()
    mylogger.addHandler(stream_hander)

    mylogger.info("server start!!!")

#결과: server start!!!
```

핸들러란 내가 로깅한 정보가 출력되는 위치 설정하는 것을 말합니다. 위에서는 콘솔을 통해 출력하도록 설정 했지만, 파일,DB,소켓,큐등을 통해 출력 하도록 설정 할 수도 있습니다.

### 나의 로깅 모듈 사용하기 - 4 (파일 핸들러 설정)

Logging Handler : <https://docs.python.org/ko/3.7/library/logging.handlers.html#>
Logging Handler : <https://docs.python.org/ko/2.7/library/logging.handlers.html#>

```python
#!/usr/bin/python
import logging

if __name__ == '__main__':
    mylogger = logging.getLogger("my")
    mylogger.setLevel(logging.INFO)

    stream_hander = logging.StreamHandler()
    mylogger.addHandler(stream_hander)

    file_handler = logging.FileHandler('my.log')
    mylogger.addHandler(file_handler)

    mylogger.info("server start!!!")
```

logging.FileHandler 클래스를 통해 객체를 만들어서 나의 로거에 추가해주면 됩니다. 현재 디렉토리에 파일(my.log)이 만들어 질 것 입니다.

노트:  파일은 a 모드로 열리는게 디폴트입니다. a 모드란 같은 이름의 파일이 이미 있다면, 파일 끝에 추가하여 쓰고, 없다면 쓰기용으로 파일 만들라는 뜻입니다.



나의 로깅 모듈 사용하기 - 5 (출력 포매팅 설정)

server start!!! 라는 메세지 말고도, 이 메세지가 언제 쓰여졌는지, 어떤 모듈에서 쓰여졌는지 등 기타 정보를 같이 출력하고 싶을 것입니다. 이때 포매팅이란 것을 하게 되는데요.

import logging


if __name__ == '__main__':
    mylogger = logging.getLogger("my")
    mylogger.setLevel(logging.INFO)

    formatter = logging.Formatter('%(asctime)s - %(name)s - %(levelname)s - %(message)s')

    stream_hander = logging.StreamHandler()
    stream_hander.setFormatter(formatter)
    mylogger.addHandler(stream_hander)

    file_handler = logging.FileHandler('my.log')
    mylogger.addHandler(file_handler)

    mylogger.info("server start!!!")


결과: 2017-08-07 12:00:29,141 - my - INFO - server start!!!
포매팅에 추가한 정보를 모든 로그 데이터에 추가해서 출력하게 됩니다.  

asctime 시간
name 로거이름
levelname 로깅레벨 
message 메세지 

이것 말고도 모듈이름, 파일이름, 출력라인넘버등 다앙하게 많으니, 더 많은 정보는 메뉴얼을 참고 하시구요. 위에서는 포매터를 콘솔출력에 한정해서 적용했기에 파일은 기존 그대로 출력 될 것입니다. 파일도 포매팅되게 하고 싶으면 역시 포매터를 파일 핸들러에도 추가하세요.



중급 
이제 중급부터는 좀 더 인사이드에 대한 고찰이 시작됩니다. 사실 파이썬이라는 언어는 읽고 이해하기 간결한 언어이기 때문에 실제 로깅 소스 모듈을 직접 읽어 보시는게 가장 명쾌하게 이해 할 수 있을 것입니다. 저도 내용 검증은 직접 소스를 확인 하였으며 참고로 파이참을 쓰실 경우 컨트롤 + 좌클릭으로 손쉽게 소스를 확인 할 수 있습니다. 

로깅 모듈 생성에 관한 고찰 

자 위에서 나만의 로깅 객체를 만들 때 logging.getLogger("my") 라고 만들었었는데요.  
그냥 매개변수 없이 logging.getLogger() or logging.getLogger("") 이렇게 만들 수도 있습니다. 

이렇게 하면 루트로거 를 리턴하게 되며, 루트로거는 로깅시스템의 기본로거이고 기본레벨은 warning 을 가지고 있습니다. 우리가 젤 처음에 로거를 만들지 않고 그냥 logging.error("...") 식으로 사용했었죠? 내부에서는 루트로거를 사용하고 있답니다.

로깅 생성에 대한 특성을 정리 해 보면 아래와 같습니다. 

* logging.getLogger() 를 통해 루트 로거 얻어서 사용 할수 있습니다.(기본레벨은 warning)

* logging.error() 를 직접 호출하면 내부에서 루트로거를 이용합니다.

* Logger 인스턴스는 이름으로 식별됩니다.

* 동일 모듈 뿐만 아니라, 여러 모듈에서 동일한 인스턴스이름 logging.getLogger('someLogger')를 여러 번 호출해도 동일한 로거 객체에 대한 참조가 반환됩니다.

* 빈 문자열인 "" 라는 이름의 루트 로거가 있으며, 다른 Logger 들은 모두 루트 Logger 의 자식들입니다. 따라서 루트 로거에 기본적인 설정을 해두고 사용하면 자식들은 모두 동일한 설정을 사용하게 됩니다.(핸들러,포매터등) 

*이름은 마침표(.) 로 구분되는 문자열이며 계층 구조를 형성한다. 즉 a 라는 로거를 만들고, a.b 라는 로거를 만들면 a.b는 a 로거의 자식입니다. 

* 하나의 모듈에서 상위 로거를 정의 및 구성하고 별도의 모듈에서 하위 로거를 생성 할 수 있으며 하위에 대한 모든 로거 호출은 상위 노드로 전달됩니다.

* 가능하면 루트 로거를 사용하기 보다는 각 클래스나 모듈마다 별도의 로거를 만드는 것을 추천합니다.

* 로깅 메세지는 부가정보가 합쳐져서 LogRecords 로 생성되며, 이 객체를 가지고 각각의 handler 는 파일,콘솔,TCP 등으로 로 emit (출력) 합니다



로깅 모듈 생성 실제 - 로거 범위 

보통 main 에서 로깅 시스템 (대표설정) 을 만들어서 사용하게 됩니다. 이것을 디폴트로 자식 로거들에서 사용하게 되며, 자식별로 필요하면 특정 설정을 하게 됩니다. 또한 핸들러는 버퍼링을 하고 있기 때문에 어플리케이션이 갑자기 중지되기 전에 logging.shutdown() 을 호출해서 모두 출력해 주면 좋습니다. (fianllly 절에서 수행) 

이제 로거를 만드는 대표적인 범위를 살펴보시죠.

@ 모듈명 : 많은 함수와 클래스들을 포함하는 모듈의 전역 Logger 인스턴스로 생성 

import logging
logger = logging.getLogger(__name__)

@객체인스턴스 : __init__() 메소드 내에서 Logger 를 생성. 인스턴스 마다 고유하다. 

def __init__(self, name)
self.logger = logging.getLogger("{0}.{1}".format(self.__class__.qualname__, name))

@클래스명 : __class__.__qualname__  만으로 생성 

@함수명 :  잘 사용되지 않는 큰 함수라면 함수 내에서 로그를 생성 할 수도 있다.

def main():
    logger = logging.getLogger("main") 

파이썬에서는 하나의 파일이 하나의 모듈과 일치합니다. 따라서 파일의 제일 위쪽에서 모듈명으로 로거객체를 만들 수 있으며, 함수별/클래스별/객체별로도 만들 수 있습니다. 자신의 로깅 전략에 따라서 만들면 될 거 같습니다.


로깅 모듈 생성 실제 - 로거 부모/자식 관계 

위에서 로거는 부모/자식관계를 갖는다고 말했는데요. 즉 루트로거는 모든 로거의 부모이며,  a.b 라는 로거는 a 라는 로거의 자식입니다. ( a라는 로거가 없는데 a.b 로거를 만들 수도 있습니다. 내부적으로 a 로거는 자동으로 place holding 됩니다) 

저런 관계에 따른 이상 행동을 예측 하기 위해서는 부모/자식간의 행동을 파악할 필요가 있는데요.
다음 코드를 보시죠. 
# 루트 로거 생성 
rootlogger = logging.getLogger()
rootlogger.setLevel(logging.INFO)
stream_hander = logging.StreamHandler()
rootlogger.addHandler(stream_hander)

# "my" 로거 생성 
mylogger = logging.getLogger("my")
mylogger.setLevel(logging.INFO)
stream_hander2 = logging.StreamHandler()
mylogger.addHandler(stream_hander2)

# "my" 로거 생성 
mylogger.info("message from mylogger")


루트로거를 a 모듈에서 만들고, "my" 로거를 다른 곳에서 만들었다고 칩시다.( 위에 소스에서는 보기 쉽게 같은 곳에 두었음)  "my" 로거에서 "message from mylogger" 를 출력 했는데, 결과는 어떻게 될까요? 

message from mylogger
message from mylogger
 
이렇게 2개의 메세지가 출력되는데 상위로 전파되었기 때문입니다.
이것을 막으려면, 간단하게는 루트로거에서 핸들러를 추가해 주지 않아도 되며, 혹은
mylogger.propagate = 0
propagate 를 0 으로 세팅하면 상위로 전파하지 않습니다.



로깅 모듈 생성 실제 - 파일을 통한 설정 

외부 파일을 통해서, 로깅레벨, 핸들러 (파일로 할것인지, 콘솔로 할 것인지 등) 와 포매터등을 설정 할 수도 있습니다. 이렇게 되면 굳이 소스를 건드리지 않고도 간편히 설정가능하겠지요? 
파일 포맷은 JSON, INI, YAML 등 다양하게 할 수 있습니다만 여기서는 JSON을 살펴보겠습니다.

logging.json 파일은 아래와 같습니다.
{
    "version": 1,
    "formatters": {
        "simple": {
            "format": "%(asctime)s - %(name)s - %(levelname)s - %(message)s"
        }
    },

    "handlers": {
        "console": {
            "class": "logging.StreamHandler",
            "level": "INFO",
            "formatter": "simple",
            "stream": "ext://sys.stdout"
        },

        "info_file_handler": {
            "class": "logging.FileHandler",
            "level": "DEBUG",
            "formatter": "simple",
            "filename": "info.log"
        }
    },

    "root": {
        "level": "DEBUG",
        "handlers": ["console", "info_file_handler"]
    }
}

포매터를 작성했으며, 핸들러를 두개 만들었습니다. root 로거에 대한 설정도 했네요.
파일핸들러를 통해서 info.log 파일에 DEBUG 레벨 이상일 경우 출력합니다. 저것을 실제 서비스시 INFO로 바꾸면 디버깅 출력은 disable 되겠죠. 

루트 로거의 레벨이 DEBUG 이기 때문에 모든 레벨을 다 출력하게 됩니다만, 자식로거에서 레벨을 높히게 되면 자식이 우선순위를 갖게 됩니다.

소스에서는 아래와 같이 logging.json 을 가져다가 사용합니다.

import logging
import logging.config
import json
import os



if __name__ == '__main__':

    with open('logging.json', 'rt') as f:
        config = json.load(f)

    logging.config.dictConfig(config)

    logger = logging.getLogger()
    logger.info("test!!!")

파일을 읽어서 logging.config.dictConfig 를 통해서 설정을 세팅하고, 루트로거를 가져다가 사용하게 됩니다.
만약 설정 파일에 아래와 같은 특정 로거를 세팅해 주게되면 
"loggers": {
    "my_module": {
        "level": "ERROR",
        "handlers": ["console"],
        "propagate": "no"
    }
},

logging.getLogger("my_module") 식으로 가져와서 사용하면 저 속성을 갖게 됩니다.


로깅 모듈 생성 실제 - 부가정보 출력하기

로깅 호출에 전달 된 매개 변수 외에도 컨텍스트 정보를 포함하도록 로깅 출력을 원하는 경우가 있습니다.
이때 LoggerAdapters 를 사용 할 수 있는데요.
class LoggerAdapter(logging.LoggerAdapter):
    def __init__(self, prefix, logger):
        super(LoggerAdapter, self).__init__(logger, {})
        self.prefix = prefix

    def process(self, msg, kwargs):
        return '[%s] %s' % (self.prefix, msg), kwargs

if __name__ == '__main__':

    with open('logging.json', 'rt') as f:
        config = json.load(f)

    logging.config.dictConfig(config)

    logger = logging.getLogger("")

    logger = LoggerAdapter("SAS", logger)
    logger.info("test!!!")


결과: 2017-08-07 12:54:55,911 - root - INFO - [SAS] test!!!!

logging.LoggerAdapter 를 상속받아서 만든 어댑터를 통해서 SAS 라는 문구를 메세지 앞에 고정 시킬 수 있게 됩니다. process 메소드에 prefix 를 추가한것을 확인하세요. 


로깅 모듈 생성 실제 - 커스텀 로그레벨 설정하기 

저같이 센서로 부터 데이터를 받는 IoT 서비스에서 로깅을 출력 할 때 DATA 만을 확인해 보고 싶을 경우가 있습니다. 이 경우 DEBUG 보다 한단계 아래에 로깅 설정을 할 수 있게 되는데요 
CRITICAL = 50
FATAL = CRITICAL
ERROR = 40
WARNING = 30
WARN = WARNING
INFO = 20
DEBUG = 10
NOTSET = 0
위처럼 각 레벨에는 숫자가 10단위로 적용되어 있습니다. 예상 할 수 있다시피, 내부에서는

if level > 세팅된 레벨:
   출력 

식으로 되어 있어서 세팅된 레벨보다 출력레벨이 클 경우에 한 해서만 출력하고 있습니다.

if ERROR  > INFO :   세팅이 INFO 면 ERROR 는 출력
if DEBUG > INFO :    세팅이 INFO 면 DEBUG 는 출력하지 않음 

따라서 DATA = 15 이렇게 설정해 두면, DEBUG 정보는 출력하지 않고, DATA 레벨만  출력하게 됩니다.

사용예는 아래와 같습니다. 

logging.addLevelName(15, "DATA")
logging.DATA = 15

logger.log(logging.DATA, "message")



멀티쓰레드에서의 로깅 

스레드에서 로깅하는 것에 대해서도 특별한 노력이 필요하지는 않습니다. 각각의 쓰레드에서 동일한 이름의 로거를 가져 다 사용 해도 됩니다. 포매팅을 통해 쓰레드별로 출력 할 수 있습니다.

다음 예제는 기본 스레드와 멀티 스레드에서 로깅을 보여줍니다. 둘이 같은 루터 로거를 사용했습니다.
import logging
import threading
import time

def worker(arg):
    while not arg['stop']:
        logging.debug('Hi from myfunc')
        time.sleep(0.5)

def main():
    logging.basicConfig(level=logging.DEBUG, format='%(relativeCreated)6d %(threadName)s %(message)s')
    info = {'stop': False}
    thread = threading.Thread(target=worker, args=(info,))
    thread.start()
    while True:
        try:
            logging.debug('Hello from main')
            time.sleep(0.75)
        except KeyboardInterrupt:
            info['stop'] = True
            break
    thread.join()

if __name__ == '__main__':
    main()
실행하면 스크립트는 다음과 같이 출력 합니다.

  1 MainThread Hello from main
     1 Thread-1 Hi from myfunc
   503 Thread-1 Hi from myfunc
   751 MainThread Hello from main
  1003 Thread-1 Hi from myfunc
  1503 MainThread Hello from main
  1503 Thread-1 Hi from myfunc
  2004 Thread-1 Hi from myfunc


블럭되는 핸들러 유연하게 다루기 

파이썬 로깅 시스템에서 핸들러를 사용 하여 출력을 하게 되는데, 중간에 버퍼링을 하고 있지 않기 때문에, 쓰기에서 오랜 지연시간이 걸리는 행동을 하게 된다면,  전체 시스템은 느려지게 마련입니다. 주로 이메일 전송의 경우 이런 행동이 자주 나타나곤 하는데, 이 경우 중간 큐를 사용하여 버퍼링을 해 줄 필요가 있습니다. 
 
이때 QueueHandler 와 QueueListener 를 사용하면 됩니다. 문제는 파이썬 2.7에서는 이것을 지원하지 않기 때문에 직접 비스무리하게 만들어야 합니다. 

import Queue
import threading
import traceback
import logging
import sys

class QueueListener(threading.Thread):

    def __init__(self, queue, stream_h):
        threading.Thread.__init__(self)
        self.queue = queue
        self.daemon = True
        self.logger = logging.getLogger("main")
        self.logger.addHandler(self.handler)

    def run(self):
        while True:
            try:
                record = self.queue.get()
                self.logger.callHandlers(record)
            except (KeyboardInterrupt, SystemExit):
                raise
            except EOFError:
                break
            except:
                traceback.print_exc(file=sys.stderr)



class QueueHandler(logging.Handler):

    def __init__(self, queue):
        logging.Handler.__init__(self)
        self.queue = queue

    def emit(self, record):
        self.queue.put(record)


if __name__ == '__main__':

    # 리스터 부분
    logging_q = Queue.Queue(-1)
    stream_h = logging.StreamHandler()
    log_queue_reader = QueueListener(logging_q,stream_h)
    log_queue_reader.start()

    # 핸들러 부분 
    handler = QueueHandler(logging_q)
    root = logging.getLogger()
    root.addHandler(handler)
    
    # 사용 
    root.error("queue handler test!!")


어떻게 활용하는지 설명드리면 

큐 생성)
먼저 버퍼로 사용할 큐를 -1 을 매개변수로 만들고, 

데이터 입력)
큐에 데이터를 담아줄 QueueHandler 를 만듭니다. 핸들러는 기본적으로 emit 메소드를 가지고 있어서 이 메소드를 통해서 log 정보를 (LogRecord 객체) 콘솔로 쏴주거나, 파일로 쏘게 됩니다만, 여기서는 큐에 데이터를 넣습니다.

데이터 처리 ) 
큐에서 데이터 (LogRecord 객체) 를 가져와서, 핸들러에게 넘겨주는 역할을 하는 QueueListener 를 만듭니다.
실제 처리를 담당할 핸들러를 위에서는 스트림 핸들러를 만들어서 매개변수로 넣어주었습니다.

그 후에는 그냥 로거 사용하듯이 사용하면 됩니다. 그러면 중간에 큐를 거쳐서 출력하게 되겠지요. 
위에서는 쓰레드를 사용했지만, 멀티프로세싱을 통해서 할 수도 있으며, 이 방식은 파이썬에서 문제가 되는 멀티프로세싱 로깅을 해결 할 수 있는 실마리를 제공해줍니다. 이 부분은 고급에서 배우게 될 것입니다.


노트: 
고급을 진행하기 전에 멀티프로세스에 관해서 알아보자. 아시다시피 파이썬에서는 멀티쓰레드가 I/O 를 제외하고 동시계산하는 곳에서는 그닥 효율적이지 않기 때문에, 멀티프로세스를 사용하게 된다. 이때 로깅도 프로세스 각각에서 사용하게 되는데, 이때 동일한 파일이 두 프로세스에 의해 열려져 있기 도 한다.이런 상황에서 여러 문제가 발생하고 해결 해야 하는 문제가 있다.
이전 로그파일을 압축하려고 할 때 우리는 , 압축 할 파일을 잠시 close 하고  압축을 하게 되는데, 윈도우즈에서는 압축이 안되는 현상 및 기존 로그 파일을 날짜가 붙은 새로운 이름으로 변경 하러 할 때도 안되는 현상이 있다.이것은 다른 프로세스에서 그 로그파일을 열고 있기 때문인데 우분투에서는 잘되었다. 아마 파일 디스크립터는 다르기 때문으로 추정한다.  아무튼 운영체제별 이런 차이들이 있어서 좀 골치 아프다. 마지막으로 멀티프로세스 환경의 로깅에서는 각각 따로 쓰면 파일에 써지지 않기 때문에  멀티프로세스용 큐를 이용하여 하나로 모아서 파일에 쓰게 되는 것을 보게 될 것이다. 


윈도우) 
- 2개의 프로세스에서 하나의 파일에 쓰기를 할 경우 윈도우는 예측 불가능이고 (하나의 프로세스 것만 써질 수도 있고, 부분 누락이 되며 순서대로 써질 수도 있고)
- 파일 삭제는 불가능
- 압축파일에 파일 옮겨 쓰는것은 예측 불가하다.
- 다만 하나의 프로세스에서 파일을 close 하면 ,그 이후로 다른프로세스의 데이터는 파일에 써지며, close 한 프로세스에서 쓴 데이터의 일부는 압축도 된다.   

리눅스) 
- 2개의 프로세스에서 하나의 파일에 쓰기를 할 경우 리눅스는 하나의 프로세스 것만 써진다.  
- 파일 삭제는 가능하다.
- 압축파일로 옮겨 쓰는 것도 가능하다. 다만 파일을 close 해주고 해야한다. 물론 메인프로세스에서 쓴 내용만 압축된다.



고급
고급은 아래와 같은 주제를 다룹니다.  조금 숨 좀 돌리고 작성 하도록 할께요~다음 기회에 봐요 ~:-) 

* 추가 컨텍스트 정보 출력 하기 / 필터 사용하기 
* 소켓핸들러 작성 
* 멀티프로세싱을 위한 로깅 작성
* 용량 넘어선 로깅파일 나누기 
* 어제로깅파일을 압축하기
* 한달전 로깅파일을 삭제하기  




[markdown]# 파이썬 로깅모듈에 대해서

나는 개발자 경력을 자바개발자로 시작했다.

제일 먼저 배운 메서드는 `main` 메서드이고 그 다음으로 배운건 `System.out.println` 이다.
그러다가 `log4j`라는 고마운 녀석을 알게되어서 별 생각없이 `log4j`만 열심히 쓰다가, 여러 로깅모듈을 하나의 인터페이스로 모아주는 `slf4j`를 살짝 만져보다가 `nodejs`로 전향해서 엄청나게 삽질을 해댄 경험이 있다.

[흑역사 링크](https://github.com/wapj/loggyu)

지금 개발이 메인언어는 `nodejs`이고 프로젝트 빌드 및 배포는 `chef` + `fabric`으로 하고 있고, 서브 스크립트 언어로 `python`과 `shell`을 사용하고 있다. 그중에 스케줄러로 돌아가는 파이썬 스크립트를 만들게 되었는데, 이 녀석이 돌다가 에러가 났을 때 `print` 메서드 만으로는 이게 실행이 됐는지 죽었는지 확인할 길이 없었다. 그래서 파이썬 로깅모듈을 찾아봤다.

![python-logging1.png](http://gyus.me/wp-content/uploads/2014/09/python-logging1-1-1.png)

어라?! 제일 위에 표준 라이브러리가 나온다.

뭔가 내용이 많은데, 표준라이브러리 내용이 읽기 부담 스러운 분은 이 글을 읽으면 조금 도움이 될지도 모르겠다.

일단, 로깅 라이브러리라고 하면 쉽게 말하면 로그를 찍는 기능이 기본이고, 두번째는 여러군데에 찍는 것이고 (예를 들어 아웃풋 스트림에 찍고, 파일로 찍고), 세번째는 이쁘게 찍는것 이다.

그래서 내가 원하는 기능이 있는지 찬찬히 살펴보았다. 필요한 기능은 아래와 같았다.

1. 스트림과 파일에 동시에 로그를 남긴다.
2. 로그를 찍은 시간과 어디에서 로그를 남겼는지 남아야한다.
3. 테스트 환경과 프로덕션 환경에서 남기는 로깅 레벨이 달라야한다.
4. 파일에 남기는 경우, 파일의 크기가 너무 크면 자동으로 하나 더 만들어 주면 좋겠다.
5. 확장이 쉬우면 좋겠다.

일단 결론만 말하면 위에 말한거 전부 다 된다. 기본 모듈이 이정도라니 정말 놀랍다.

차근 차근 한번 알아보자.

### 스트림과 파일에 동시에 로그를 남기기

`print` 메서드로만 로그를 찍어왔다면, 이제 기본 탑재된 `logging` 모듈을 한번 사용해 보자.

“`python
import logging

logging.info(“I told you so”)
logging.warning(“Watch out!”)
“`

위의 코드를 실행하면 아래와 같이 나오는데, 그 이유는 logging의 기본 로그 레벨이 WARNING으로 되어 있기 때문이다.

“`shell
WARNING:root:Watch out!
“`

로그를 전부다 `WARNING`으로 찍을 수는 없으니 살짝만 건드려 보자.

“`python
import logging
logging.basicConfig(level=logging.DEBUG)

logging.debug(“디버깅용 로그~~”)
logging.info(“도움이 되는 정보를 남겨요~”)
logging.warning(“주의해야되는곳!”)
logging.error(“에러!!!”)
logging.critical(“심각한 에러!!”)
“`

그러면 아래와 같이 나올 것이다.

“`shell
DEBUG:root:디버깅용 로그~~
INFO:root:도움이 되는 정보를 남겨요~
WARNING:root:주의해야되는곳!
ERROR:root:에러!!!
CRITICAL:root:심각한 에러!!
“`

로깅 레벨도 지정해 봤으니 파일로도 남겨보자.

“`python
import logging
logging.basicConfig(filename=’./test.log’,level=logging.DEBUG)

logging.info(“=========================================”)
logging.info(“파일에다가 남겨봐요~”)
logging.info(“=========================================”)
logging.debug(“디버깅용 로그~~”)
logging.info(“도움이 되는 정보를 남겨요~”)
logging.warning(“주의해야되는곳!”)
logging.error(“에러!!!”)
logging.critical(“심각한 에러!!”)
“`

위의 코드를 실행하면 실행한 폴더에 test.log라는 파일이 생기고 그 파일을 열어보면 아래와 같이 로그가 파일로 남는다. `여러번 실행하면 파일을 덮어 쓰는 것이 아니라, 기존의 로그에 이어서 붙이기를 하게된다.`

“`shell
INFO:root:=========================================
INFO:root:파일에다가 남겨봐요~
INFO:root:=========================================
DEBUG:root:디버깅용 로그~~
INFO:root:도움이 되는 정보를 남겨요~
WARNING:root:주의해야되는곳!
ERROR:root:에러!!!
“`

그럼 이제 원래 하고 싶었던 걸 해보자.
아웃풋 스트림에도 찍어봤고, 파일로도 남겨 봤는데 둘다 동시에 남길려면 어떻게 해야되지? 라는 질문이 생기는데, 둘다 로그를 남길려면 `logging.getLogger(“로거이름”)` 이라는 메서드로 얻을 수 있는 logger라는 녀석을 사용해야한다.

logger를 써서 여러군데로 로그를 남기는 것에 대해 간단하게 단계를 설명하면 아래와 같다.

1. 로거 인스턴스를 만든다.
2. 스트림과 파일로 로그를 출력하는 핸들러를 각각 만든다.
3. 1번에서 만든 로거 인스턴스에 스트림 핸들러와 파일핸들러를 붙인다.
4. 로거 인스턴스로 로그를 찍는다.

말로 설명해 봤으니 코드를 보자.

“`python
import logging
import logging.handlers

# 1. 로거 인스턴스를 만든다
logger = logging.getLogger(‘mylogger’)

# 2. 스트림과 파일로 로그를 출력하는 핸들러를 각각 만든다.
fileHandler = logging.FileHandler(‘./myLoggerTest.log’)
streamHandler = logging.StreamHandler()

# 3. 1번에서 만든 로거 인스턴스에 스트림 핸들러와 파일핸들러를 붙인다.
logger.addHandler(fileHandler)
logger.addHandler(streamHandler)

# 4. 로거 인스턴스로 로그를 찍는다.
logger.setLevel(logging.DEBUG)
logger.debug(“===========================”)
logger.info(“TEST START”)
logger.warning(“스트림으로 로그가 남아요~”)
logger.error(“파일로도 남으니 안심이죠~!”)
logger.critical(“치명적인 버그는 꼭 파일로 남기기도 하고 메일로 발송하세요!”)
logger.debug(“===========================”)
logger.info(“TEST END!”)
“`

위의 코드를 실행시켜 보면 콘솔과 파일에 각각 아래와 같은 로그가 남는다.

“`shell
===========================
TEST START
스트림으로 로그가 남아요~
파일로도 남으니 안심이죠~!
치명적인 버그는 꼭 파일로 남기기도 하고 메일로 발송하세요!
===========================
TEST END!
“`

### 로그를 찍은 시간과 어느 파일의 어느 라인에 심어 놓은 로그인지 남기기

로그의 포매팅에 관한 이야기 인데, 이 부분도 파이썬을 개발하는 분들이 이미 표준 logging모듈에 심어두셨다. 위에서는 핸들러를 알아봤다면 이번에 알아볼 녀석은 포매터라는 녀석이다. 내가 알고 싶은건 날짜와 시간, 파일명, 로그레벨, 메세지 이정도가 되겠다. 코드를 만드는 단계를 설명안 해도 될정도로 엄청 간단하므로 그냥 코드로 바로 알아보도록하자.

“`python
import logging
import logging.handlers

# 로거 인스턴스를 만든다
logger = logging.getLogger(‘mylogger’)

# 포매터를 만든다
fomatter = logging.Formatter(‘[%(levelname)s|%(filename)s:%(lineno)s] %(asctime)s > %(message)s’)

# 스트림과 파일로 로그를 출력하는 핸들러를 각각 만든다.
fileHandler = logging.FileHandler(‘./myLoggerTest.log’)
streamHandler = logging.StreamHandler()

# 각 핸들러에 포매터를 지정한다.
fileHandler.setFormatter(fomatter)
streamHandler.setFormatter(fomatter)

# 로거 인스턴스에 스트림 핸들러와 파일핸들러를 붙인다.
logger.addHandler(fileHandler)
logger.addHandler(streamHandler)

# 로거 인스턴스로 로그를 찍는다.
logger.setLevel(logging.DEBUG)
logger.debug(“===========================”)
logger.info(“TEST START”)
logger.warning(“스트림으로 로그가 남아요~”)
logger.error(“파일로도 남으니 안심이죠~!”)
logger.critical(“치명적인 버그는 꼭 파일로 남기기도 하고 메일로 발송하세요!”)
logger.debug(“===========================”)
logger.info(“TEST END!”)
“`

위의 코드를 실행하면 아래와 같이 나온다.

“`shell
[DEBUG|loggingFormatter.py:24] 2014-09-02 20:39:46,630 > ===========================
[INFO|loggingFormatter.py:25] 2014-09-02 20:39:46,630 > TEST START
[WARNING|loggingFormatter.py:26] 2014-09-02 20:39:46,630 > 스트림으로 로그가 남아요~
[ERROR|loggingFormatter.py:27] 2014-09-02 20:39:46,630 > 파일로도 남으니 안심이죠~!
[CRITICAL|loggingFormatter.py:28] 2014-09-02 20:39:46,631 > 치명적인 버그는 꼭 파일로 남기기도 하고 메일로 발송하세요!
[DEBUG|loggingFormatter.py:29] 2014-09-02 20:39:46,631 > ===========================
[INFO|loggingFormatter.py:30] 2014-09-02 20:39:46,631 > TEST END!
“`

이처럼 포매터의 값만 이리저리 바꿔주면, 내가 원하는 대로 로그를 남길 수 있다!
포매터에 들어가는 변수의 문자열은 아래 링크에서 확인하길 바란다.

[logrecord-attributes](https://docs.python.org/3/library/logging.html#logrecord-attributes)

다음으로 가보자.

### 테스트 환경과 프로덕션 환경에서 로그 레벨을 다르게 하고 싶을경우

나는 테스트환경과 프로덕션 환경을 구분하기 위해서 처음에 서버를 세팅할 때 환경변수를 심어놓는다. nodejs모듈에서 사용되는(expressjs) NODE_ENV라는 환경변수명이 있는데 이 값을 미리 테스트 서버와 알파서버, 프로덕션 서버에 각각 다른 값으로 설정을 해둔다. 여기서는 로컬 개발 머신과 테스트 서버만 있다고 가정하고 예제를 만들어봤다.

“`python
import os
import logging
import logging.handlers

# 로거 인스턴스를 만든다
logger = logging.getLogger(‘mylogger’)

# 포매터를 만든다
fomatter = logging.Formatter(‘[%(levelname)s|%(filename)s:%(lineno)s] %(asctime)s > %(message)s’)

# 환경변수를 읽어서 로깅 레벨과 로그를 남길 파일의 경로를 변수에 저장한다
if (os.environ[‘NODE_ENV’] == ‘local’):
loggerLevel = logging.DEBUG
filename = ‘/tmp/test.log’
elif(os.environ[‘NODE_ENV’] == ‘test’):
loggerLevel = logging.DEBUG
filename = ‘/home/www/log/testServer.log’
else:
loggerLevel = logging.INFO
filename = ‘/home/www/log/server.log’

# 스트림과 파일로 로그를 출력하는 핸들러를 각각 만든다.
fileHandler = logging.FileHandler(filename)
streamHandler = logging.StreamHandler()

# 각 핸들러에 포매터를 지정한다.
fileHandler.setFormatter(fomatter)
streamHandler.setFormatter(fomatter)

# 로거 인스턴스에 스트림 핸들러와 파일핸들러를 붙인다.
logger.addHandler(fileHandler)
logger.addHandler(streamHandler)

# 로거 인스턴스로 로그를 찍는다.
logger.setLevel(loggerLevel)
logger.debug(“===========================”)
logger.info(“TEST START”)
logger.warning(“파일 명과 로깅 레벨을 각각 환경마다 다르게 남도록 했어요.”)
logger.debug(“디버그 로그는 테스트 환경과 로컬 피씨에서남 남는 답니다.”)
logger.critical(“치명적인 버그는 꼭 파일로 남기기도 하고 메일로 발송하세요!”)
logger.debug(“===========================”)
logger.info(“TEST END!”)
“`

환경변수 `NODE_ENV`의 값에 따라 로그가 남는 파일의 경로와 로깅 레벨이 달라졌습니다~

### 파일로 로그를 남기는 경우 파일이 너무 커지면 자동으로 새로운 파일을 만들어 줬으면…

보통 이런거는 `shell` 스크립트를 스케줄러로 돌려서 자동으로 돌리거나 하는데, 파이썬에는 `RotatingFileHandler`라는 놈이 이미 만들어져 있다. 그냥 가져다 쓰면 된다. 정말 감동적인 모듈인듯!

이전에 만들어둔 파일 핸들러를 `RotatingFileHandler`로 교체해보자.

위에 있는 fileHandler부분만 아래 코드로 교체하면 된다.
“`python
fileMaxByte = 1024 * 1024 * 100 #100MB
fileHandler = logging.handlers.RotatingFileHandler(filename, maxBytes=fileMaxByte, backupCount=10)
“`

maxBytes 파라메터는 한개의 파일의 최대 바이트 수 이고, backupCount는 몇개까지 백업파일을 남길것인지 세팅하는 파라메터이다.
위의 세팅 대로라면 100MB 짜리 파일을 10개까지 남기겠다. 라는 의미가 된다. 이제 로그 파일의 용량이 엄청나게 커져서 서버에 용량이 부족할까 걱정하지 않아도 된다~ 야호~~!

눈치가 빠른 사람이라면 `logging.handlers` 아래에 다른 핸들러들도 많겠구나~ 라는 생각이들것이다.

[logging.handlers](https://docs.python.org/3/library/logging.handlers.html#module-logging.handlers) 링크를 타고 가보면 많은 핸들러들을 볼 수가 있다.

어지간한 기능은 다 넣어본것 같은데 기존에 없는 기능을 추가할려면 어떻게 하지?!

### 확장이 쉬우면 좋겠다!

에러가 났을 때 mongodb에 그 정보를 저장했으면 좋겠다! 어떻게 하지?

일단 쉬운 방법은 나보다 똑똑한 사람이 만들어 놓은 것을 쓰면 된다. 요즘 세상이 참 좋은 세상이라 구글로 찾으면 내가 생각한건 다있다. ㅎㅎ 근데 가끔 이렇게 찾아도 내 마음에 쏙~ 안들 수도 있다. 그럴 때는 한번 만들어보는 것도 힘들긴 하지만, 도움이 될 때가 많다.

그런 의미에서 다른분들도 이미 뜬 삽이겠지만, 나도 한삽을 더 해보려고 한다. 진짜 기본기능만 되는걸 하나 만들어보자.

참고로, mongodb 모듈로 pymongo가 설치되어 있어야 한다. `pip3 install pymongo`로 간단히 설치가능하다.
mongodb도 물론 설치가 되어있어야한다. 해당 내용은 이글과는 크게 관계없으므로 생략하겠다.

핸들러를 만드는 순서는 아래와 같다.

1. mongodb에 로그를 저장할 수 있도록 handler를 만든다.
2. handler는 logging.Handler를 상속하고 emit 메서드를 구현하면된다.

간단히 만들어본 소스는 아래와 같다.
“`python
import logging
from pymongo.connection import Connection
from bson import InvalidDocument

class MongoHandler(logging.Handler):

def __init__(self, db=’mongolog’, collection=’log’, host=’localhost’, port=None, level=logging.NOTSET):
logging.Handler.__init__(self, level)
self.collection = Connection(host, port)[db][collection]

def emit(self, record):
data = record.__dict__.copy()

try:
self.collection.save(data)
except InvalidDocument as e:
logging.error(“Unable save log to mongodb: %s”, e.message)

if __name__ == ‘__main__’:
MongoHandler(‘mongolog’, ‘test’)
“`

테스트용 소스도 만들어보자. 간단히 핸들러를 추가하고 로그를 찍어본다.

“`python
import logging
from mongoLogger import MongoHandler

if __name__ == ‘__main__’:
logger = logging.getLogger(‘mongoTest’)
logger.setLevel(logging.WARNING)
logger.addHandler(MongoHandler(‘mongolog’, ‘log’))

logger.debug(“test debug”)
logger.info(“test info”)
logger.warning(“test warning”)
logger.error(“test error”)
logger.critical(“test critical”)
“`

실행 후 mongodb에 들어가서 확인을 해보면 아래와 같이 WARNING이상의 로그가 저장되어 있다.

“`shell
> db.log.find().pretty();
{
“_id” : ObjectId(“5405c2cc1626051dcf238cfa”),
“stack_info” : null,
“exc_text” : null,
“exc_info” : null,
“processName” : “MainProcess”,
“lineno” : 11,
“msecs” : 891.3910388946533,
“relativeCreated” : 50.26507377624512,
“process” : 7631,
“name” : “mongoTest”,
“pathname” : “mongoTest.py”,
“created” : 1409663692.891391,
“filename” : “mongoTest.py”,
“funcName” : ““,
“threadName” : “MainThread”,
“msg” : “test warning”,
“args” : [ ],
“module” : “mongoTest”,
“levelno” : 30,
“thread” : NumberLong(“140735296762640”),
“levelname” : “WARNING”
}
{
“_id” : ObjectId(“5405c2cc1626051dcf238cfb”),
“stack_info” : null,
“exc_text” : null,
“exc_info” : null,
“processName” : “MainProcess”,
“lineno” : 12,
“msecs” : 891.618013381958,
“relativeCreated” : 50.492048263549805,
“process” : 7631,
“name” : “mongoTest”,
“pathname” : “mongoTest.py”,
“created” : 1409663692.891618,
“filename” : “mongoTest.py”,
“funcName” : ““,
“threadName” : “MainThread”,
“msg” : “test error”,
“args” : [ ],
“module” : “mongoTest”,
“levelno” : 40,
“thread” : NumberLong(“140735296762640”),
“levelname” : “ERROR”
}
{
“_id” : ObjectId(“5405c2cc1626051dcf238cfc”),
“stack_info” : null,
“exc_text” : null,
“exc_info” : null,
“processName” : “MainProcess”,
“lineno” : 13,
“msecs” : 891.7689323425293,
“relativeCreated” : 50.642967224121094,
“process” : 7631,
“name” : “mongoTest”,
“pathname” : “mongoTest.py”,
“created” : 1409663692.891769,
“filename” : “mongoTest.py”,
“funcName” : ““,
“threadName” : “MainThread”,
“msg” : “test critical”,
“args” : [ ],
“module” : “mongoTest”,
“levelno” : 50,
“thread” : NumberLong(“140735296762640”),
“levelname” : “CRITICAL”
}
“`

### 결론

파이썬에서는 로그를 남기기 위해서 뭘쓸까 고민할 필요가 전혀 없다. 표준 라이브러리가 워낙에 잘되어 있고, 확장 또한 쉽기 때문에 별다른 고민없이 `logging` 모듈만 잘 공부하면 된다. 나도 필요해서 찾아보고 공부해본 것이지만, 위에서 소개한 것 이외에도 많은 기능들을 가지고 있으므로 아마 거의 대부분의 경우에는 표준 logging모듈로도 충분할 것으로 생각된다.

관심이 있는 사람은 [logging-cookbook](https://docs.python.org/3/howto/logging-cookbook.html#logging-cookbook) 페이지를 참고하도록 하자.

[/markdown]


