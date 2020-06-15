---
layout: post
title: 200615_Algorithm
date: Mon June 15 2020 18:10:04 GMT+0900
author: Jang Taeyoung
categories: algorithm
tags: algorithm
---

> 나를 위한 알고리즘 정리 (1) - 기억에 남는 문제

<br /><br />

## 문제 1

<hr>

### 입력

첫 줄에 테스트케이스의 개수 T가 주어진다. T는 최대 1,000,000이다. 다음 T줄에는 각각 두 정수 A와 B가 주어진다. A와 B는 1 이상, 1,000 이하이다.

### 출력

각 테스트케이스마다 A+B를 한 줄에 하나씩 순서대로 출력한다.
<br /><br />

## 풀이 (python)

<hr>

```python

import sys

num = int(sys.stdin.readline())
a = [sys.stdin.readline().rstrip() for i in range(int(num))]

for case in a:
    a, b = case.split()
    print(int(a)+int(b))
```

<br /><br />

## 공부

input 대신 사용한 sys.stdin.readline(), Java로 알고리즘을 풀 때 BufferedReader를 사용하듯 Python으로 여러 줄의 입력을 받을 경우에는 **sys.stdin.readline()**을 사용한다.

> 입출력 속도 : sys.stdin.readline() > raw_input() > input()

하나의 라인 또는 여러 라인을 **sys.stdin.readline()**과 for문을 사용하여 리스트나 튜플에 바로 저장하는 방법

> import sys<br /> a = map(int, sys.stdin.readline())<br /> # a = [1, 2, 3, 4, 5]

개행 문자도 입력 받아서 저장하기 때문에 필요한 경우에는 **rstrip()** 이용

<br /><br />

## 문제 2

<hr>

### 입력

첫째 줄에 테스트 케이스의 개수 T가 주어진다. 각 테스트 케이스는 한 줄로 이루어져 있으며, 각 줄에 A와 B가 주어진다. (0 < A, B < 10)

### 출력

각 테스트 케이스마다 "Case #x: "를 출력한 다음, A+B를 출력한다. 테스트 케이스 번호는 1부터 시작한다.
<br /><br />

## 풀이 (python)

<hr>

```python
import sys

num = sys.stdin.readline().rstrip()

a = [sys.stdin.readline().rstrip() for idx in range(int(num))]

for idx, case in enumerate(a):
    a, b = case.split()
    print(f"Case #{idx+1}:", int(a)+int(b))
```

<br /><br />

## 공부

**enumerate()** : for문에서 index와 value를 모두 사용있게 해주는 Python 내장 함수, 첫 번째 인자에 index가 들어오고 두 번째 인자값에 value가 들어온다.

**map()** : 리스트의 요소를 지정된 함수로 처리해주는 함수 ex. numbers = list(map(int, sys.stdin.readline().split()))
