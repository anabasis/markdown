# Logging 데코레이션 생성

```python
# exception_decor.py
import functools
import logging

def create_logger():
    """
    Creates a logging object and returns it
    """
    logger = logging.getLogger("example_logger")
    logger.setLevel(logging.INFO)
    # create the logging file handler
    fh = logging.FileHandler("/path/to/test.log")
    fmt = '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
    formatter = logging.Formatter(fmt)
    fh.setFormatter(formatter)
    # add handler to logger object
    logger.addHandler(fh)
    return logger

def exception(function):
    """
    A decorator that wraps the passed in function and logs
    exceptions should one occur
    """

    @functools.wraps(function)
    def wrapper(*args, **kwargs):
        logger = create_logger()
        try:
            return function(*args, **kwargs)
        except:
            # log the exception
            err = "There was an exception in  "
            err += function.__name__
            logger.exception(err)
            # re-raise the exception
            raise
    return wrapper
```

- 로깅 오브젝트 생성하여 리턴
- 데코레이터 기능

```python
from exception_decor import exception

@exception
def zero_divide():
    1 / 0

if __name__ == '__main__':
    zero_divide()
```

```python
2016-06-09 08:26:50,874 - example_logger - ERROR - There was an exception in  zero_divide
Traceback (most recent call last):
  File "/home/mike/exception_decor.py", line 29, in wrapper
    return function(*args, **kwargs)
  File "/home/mike/test_exceptions.py", line 5, in zero_divide
    1 / 0
ZeroDivisionError: integer division or modulo by zero
```

1. Logger

    ```python
    # exception_logger.py
    import logging
    def create_logger():
        """
        Creates a logging object and returns it
        """
        logger = logging.getLogger("example_logger")
        logger.setLevel(logging.INFO)
        # create the logging file handler
        fh = logging.FileHandler(r"/path/to/test.log")
        fmt = '%(asctime)s - %(name)s - %(levelname)s - %(message)s'
        formatter = logging.Formatter(fmt)
        fh.setFormatter(formatter)
        # add handler to logger object
        logger.addHandler(fh)
        return logger
    logger = create_logger()
    ```

2. Exception

    ```python
    # exception_decor.py
    import functools
    def exception(logger):
        """
        A decorator that wraps the passed in function and logs
        exceptions should one occur
        @param logger: The logging object
        """

        def decorator(func):
            def wrapper(*args, **kwargs):
                try:
                    return func(*args, **kwargs)
                except:
                    # log the exception
                    err = "There was an exception in  "
                    err += func.__name__
                    logger.exception(err)
                # re-raise the exception
                raise
            return wrapper
        return decorator
    ```

3. 사용

    ```python
    from exception_decor import exception
    from exception_logger import logger
    @exception(logger)
    def zero_divide():
        1 / 0
    if __name__ == '__main__':
        zero_divide()
    ```
