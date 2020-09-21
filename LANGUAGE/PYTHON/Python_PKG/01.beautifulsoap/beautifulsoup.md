# BeautifulSoup

## 설치 및 선언

```bash
# 설치
pip install beautifulsoup4
```

```python
# 선언
from bs4 import BeautifulSoup

url = "http://www.naver.com/"
html = urlopen(url).read()
soup = BeautifulSoup(html, "html.parser")
```

## 파서 정의

1. html.parser (괜찮은 속도와 관대함)
2. lxml (매우 빠르고 관대함)

    ```bash
    pip install lxml
    ```

3. html5lib (매우 느리지만 극도로 관대)

    ```bash
    pip install html5lib
    ```

## 해더추가

```python
url = "http://www.naver.com/"
req = Request(url)
req.add_header('User-Agent', 'Mozilla/5.0')
html = urlopen(req).read()
```

## 사용법

### select 요소

- soup.select("상위태그 또는 id 또는 클래스 > 하위태그 또는 id 또는 클래스")
- id일 경우 #idname
- class일 경우 .classname
- 태그일 경우 tagname
- soup.select("p > #idname > .classname")

```python
# a태그이면서 classname인 항목을 찾고 싶을때
soup.select("a .classname")
# a태그 면서 href요소가 있는 항목만 list로 묶어줌
res = soup.select('a[href]')
# a 태그 면서 href요소도 있고 특정 class만 묶고 싶을 때
res = soup.select("a[href] + .classname")
```

### 모든 오브젝트

- 형식 `find_all(name, attrs, recursive, string, limit, **kwargs)`

```python
# 모든 a태그
soup.find_all("a")
soup("a")

# string이 있는 title태그 모두 검색 2개만
soup.title.find_all(string=True,limit=2)
soup.title(string=True,limit=2)

# String 검색
soup.find_all(string="Elsie") # string 이 Elsie 인 것 찾기
soup.find_all(string=["Tillie", "Elsie", "Lacie"]) # or 검색
soup.find_all(string=re.compile("Dormouse")) # 정규식 이용

# p 태그와 속성 값이 title 이 있는거
soup.find_all("p", "title")
# <p class="title"></p>

# a태그와 b태그 찾기
soup.find_all(["a", "b"])

# class 는 파이썬에서 예약어이므로 class_ 로 쓴다.
soup.find_all("a", class_="sister")

# a태그 모두 찾기
res_list = bs.find_all('a')

# 클래스가 일치하는 모든 a태그 찾기
res_list = bs.find_all('a', {'class':'test_class'})

# 클래스가 일치하는 2개의 a태그 찾기
res_list = bs.find_all('a', {'class':'test_class'}, limit=2)

# 위에서 찾은 것 중 두번째 요소
res_list[1]

# string값 검색
soup.find_all(string="searchzz")
```

### find

특정 조건에 일치하는 것 하나만 찾으면 되는 경우 find
find는 조건에 일치하는 첫 번째 태그를 찾아줌

- find(name, attrs, recursive, string, **kwargs)

```python
find()
find_next()
find_all()

# find 할 때 확인
if soup.find("div", title=True) is not None:
i = soup.find("div", title=True)

# 태그명 얻기
soup.find("div").name

# data-로 시작하는 속성 find
soup.find("div", attrs={"data-value": True})

# 속성 얻기
soup.find("div")['class'] # 만약 속성 값이 없다면 에러
soup.find("div").get('class') # 속성 값이 없다면 None 반환

# class명이 일치하는 a태그 찾기
res_list = bs.find('a', class_=:test)
# OR
res = bs.find('a', {'class':'test'})
# class는 예약어라 class_라고 써줘야함.

# id일치 하는 태그 찾기
res = bs.find(id='test_id')
# OR
res = bs.find('', {'id'='test_id'})
```

### 속성이 있는지 확인

```python
tag.has_attr('class')
tag.has_attr('id')
## 있으면 True, 없으면 False
```

### 속성 값 가져오기

```python
soup.p['class']
soup.p['id']
```

### string을 다른 string으로 교체

```python
tag.string.replace_with("새로운 값")

# 속성 값 모두 출력
tag.attrs
```

### 보기 좋게 출력

```python
soup.b.prettify()
```

### 간단한 검색

```python
soup.body.b # body 태그 아래의 첫번째 b 태그
soup.a # 첫번째 a 태그
```

### 태그 삭제

```python
a_tag.img.unwrap()
```

### 태그 추가

```python
soup.p.string.wrap(soup.new_tag("b"))
soup.p.wrap(soup.new_tag("div")
```

### 태그안에 특정 값만 가져오면 되는 경우

특정 속성 값만 가져오고 싶을 때 get사용

```python
res.get('href')

# <title> 태그까지 같이 가져옴
soup.title
soup.find(‘title’)

# <title>태그 안에 있는 내용만 가져오고 싶을 때는 string 이나 get_text()이용
soup.title.string
soup.find(‘title’) .get_text()

```

## html 파일 열기

```python
with open("example.html") as fp:
    soup = BeautifulSoup(fp, 'html.parser')
```

## 웹에서 소스 가져오기

1. urllib를 통해서 웹에 있는 소스 가져오기

```python
import urllib.request
import urllib.parse

# web_url에 원하는 웹의 URL을 넣어주시면 됩니다.
with urllib.request.urlopen(web_url) as response:
    html = response.read()
    soup = BeautifulSoup(html, 'html.parser')


2. requests를 통해서 웹에 있는 소스 가져오기

```python
import requests

# web_url에 원하는 웹의 URL을 넣어주시면 됩니다.
r = requests.get(web_url)
r.status_code
200
r.headers['content-type']
'text/html; charset=UTF-8'
r.encoding
'UTF-8'
r.text
<!DOCTYPE html>
<html class="client-nojs" lang="en" dir="ltr">
```
