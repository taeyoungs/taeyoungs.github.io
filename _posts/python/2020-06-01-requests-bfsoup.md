---
layout: post
title: "requests, BeautifulSoup"
date: Mun June 1 2020 23:40:20 GMT+0900
author: Jang Taeyoung
categories: python
tags: python module
cover: "/assets/python.png"
---

> 나를 위한 Python 개념 정리 (1)

Python Code Challenge를 진행하면서 사용했고 앞으로 Webscrapper 만들 때 사용할 requests, BeautifulSoup 사용법에 대해서 간단하게 짚고 넘어가자.

# requests

---

Python에서 HTTP 요청을 보내는 모듈

```python

import requests

r = requests(url)
r.text
r.status_code

```

requests 모듈을 import 해서 사용할 수 있으며 .status_code로 응답 상태, .text로 응답의 내용으로 온 html을 text 형태로 볼 수도 있다.
<br /><br />

### GET 요청 params

```python

params = {"key_A": "value_A", "args_B": "value_B"}
r = requests.get(url, params=params)

```

key, value인 dict 형태로 params에 담아 보내주면 된다.
<br /><br />

### POST 요청 data

```python

data = {"key_A": "value_A", "args_B": "value_B"}
r = requests.post(url, data=data)

```

GET 요청 때 params와 동일하게 dict 형태로 이번엔 data에 담아서 보내주면 된다.
<br /><br />

### Custom Header, Cookies

요청 보낼 때

```python

headers = {'user-agent': 'my-app/0.0.1'}
cookies = dict(cookies_are='working')
r = requests.get(url, headers=headers, cookies=cookies)

```

응답 받고 꺼내서 사용할 때

```python

r = requests.get(url)
r.headers['Content-Type']
r.cookies['cookies_are']

```

<br /><br />

# BeautifulSoup

---

HTML, XML 파일에서 데이터를 뽑아내는 Python 모듈

```python

url = "http://www.blah.com"
r = requests.get(url)
soup = BeautifulSoup(r.text, "html.parser")

```

soup을 통해서 원하는 tag 또는 tag 안에 텍스트를 뽑아낼 수 있다.
<br /><br />

### find(), find_all()

```python

soup = BeautifulSoup(r.text, "html.parser")

job = soup.find("div", class_="something")
job = soup.find("div", id="something")
job_list = find_all("div", class_="list")

```

- .find()에 원하는 태그명을 첫 번째 인자값으로 입력하면 되고 추가적인 attributes를 통해서 더 자세한 조건을 입력할 수 있다.
- .find_all(), .find()에서 더 나아가 조건에 맞는 모든 태그를 찾으며 결과값을 리스트로 반환한다.
  ++ value=True, string=True로 해당 attribute가 존재할 경우에만 검색하도록 설정 가능
  ++ ["value"]를 뒤에 붙이면 속성값을 가져온다.
  <br /><br />

### .select_one()

```python

soup = BeautifulSoup(r.text, "html.parser")

result = job.select_one("td.regDate > strong").get_text()

```

find와 비슷하지만 사용하는 방식이 조금 다르다. attr를 넘겨주는 대신 css 작성하듯이 검색 조건 설정
<br /><br />

### .string, get_text()

- get_text()는 현재 태그를 포함하여 모든 하위 태그를 제거하고 유니코드 텍스트만 들어있는 문자열을 반환한다. 따라서, 마지막 태그에 사용하지 않으면 결과값이 복잡하게 나올 수도 있다.

- .string은 태그 내에 문자열을 반환하는데 태그 내에 자식 태그가 둘 이상이라면, 반환할 것을 정확히 모르기 때문에 None을 반환한다. 또한, 태그 내에 태그 + 문자열이 존재할 때 .string으로 문자열만 가져오지 못하고 None을 반환한다.
