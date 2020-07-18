---
layout: post
title: 200708_Algorithm
date: Wed July 8 2020 18:05:04 GMT+0900
author: Jang Taeyoung
categories: algorithm
tags: algorithm
---

> 나를 위한 알고리즘 정리 (3) - 기억에 남는 문제 (수힉2 파트)

<br /><br />

## 문제

베르트랑 공준은 임의의 자연수 n에 대하여, n보다 크고, 2n보다 작거나 같은 소수는 적어도 하나 존재한다는 내용을 담고 있다.
<br />
이 명제는 조제프 베르트랑이 1845년에 추측했고, 파프누티 체비쇼프가 1850년에 증명했다.
<br />
예를 들어, 10보다 크고, 20보다 작거나 같은 소수는 4개가 있다. (11, 13, 17, 19) 또, 14보다 크고, 28보다 작거나 같은 소수는 3개가 있다. (17,19, 23)
<br />
n이 주어졌을 때, n보다 크고, 2n보다 작거나 같은 소수의 개수를 구하는 프로그램을 작성하시오.

## 입력

입력은 여러 개의 테스트 케이스로 이루어져 있다. 각 케이스는 n을 포함하며, 한 줄로 이루어져 있다. (n ≤ 123456)
<br />
입력의 마지막에는 0이 주어진다.

## 출력

각 테스트 케이스에 대해서, n보다 크고, 2n보다 작거나 같은 소수의 개수를 출력한다.

## 풀이 (python)

<hr>

```python

import sys

MAX_N = 123456*2
decimal_list = []
for x in range(MAX_N+1):
  decimal_list.append(x)

# 2부터 0번 자리에 위치
for num in range(2, MAX_N+1):
  if decimal_list[num] == 0:
    continue
  for t in range(num+num, MAX_N+1, num):
    decimal_list[t] = 0

while True:
  n = int(sys.stdin.readline().rstrip())
  if n == 0:
    break

  cnt = 0
  for num in range(n+1, n*2+1):
    if decimal_list[num] != 0:
      cnt += 1

  print(cnt)

```

### 공부

에라토스테네스의 체 (Sieve of Eratosthenes) 이용해서 문제를 해결했다. 이제껏 특정 숫자가 소수인지 아닌지 하나 씩 파악하는 방법으로 문제를 풀어갔었다. 문제를 풀어가면서도 이런 식이면 시간 초과와 같은 문제에 무조건 부딪힐 거라고 생각했고 다른 방법으로 푸는 방법이 없는지 찾아보다가 에라토스테네스의 체 방법을 이용하게 됐다.

### 에라토스테네스의 체 (Sieve of Eratosthenes)

간단하게 리스트에 1 ~ N까지의 수를 넣어놓고 리스트의 길이만큼 반복문을 돌린다. 반복문의 증가하는 변수 i의 배수는 0으로 교체하는 방법이며, 반복문을 모두 돌고 나서 0으로 교체되지 않고 남아있는 수들이 소수가 된다.
<br />
숫자를 하나 씩 소수인지 판별하는 반복문보다 에라토스테네스의 체로 문제에서 입력 N이 최대로 커질 경우의 소수 리스트를 구하는 반복문이 훨씬 효율적이기 때문에 미리 소수 리스트를 구하고 이를 이용해 소수인지 아닌지 파악했다.
