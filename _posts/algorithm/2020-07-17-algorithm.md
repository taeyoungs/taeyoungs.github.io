---
layout: post
title: 200717_Algorithm
date: Fri July 17 2020 18:05:04 GMT+0900
author: Jang Taeyoung
categories: algorithm
tags: algorithm
---

> 나를 위한 알고리즘 정리 (4) - 기억에 남는 문제 (재귀 파트)

<br /><br />

## 문제

세 개의 장대가 있고 첫 번째 장대에는 반경이 서로 다른 n개의 원판이 쌓여 있다. 각 원판은 반경이 큰 순서대로 쌓여있다. 이제 수도승들이 다음 규칙에 따라 첫 번째 장대에서 세 번째 장대로 옮기려 한다.
<br />

1. 한 번에 한 개의 원판만을 다른 탑으로 옮길 수 있다.
2. 쌓아 놓은 원판은 항상 위의 것이 아래의 것보다 작아야 한다.

<br />
이 작업을 수행하는데 필요한 이동 순서를 출력하는 프로그램을 작성하라. 단, 이동 횟수는 최소가 되어야 한다.

## 입력

첫째 줄에 첫 번째 장대에 쌓인 원판의 개수 N (1 ≤ N ≤ 20)이 주어진다.

## 출력

첫째 줄에 옮긴 횟수 K를 출력한다.
<br />
두 번째 줄부터 수행 과정을 출력한다. 두 번째 줄부터 K개의 줄에 걸쳐 두 정수 A B를 빈칸을 사이에 두고 출력하는데, 이는 A번째 탑의 가장 위에 있는 원판을 B번째 탑의 가장 위로 옮긴다는 뜻이다.

## 풀이 (python)

<hr>

```python

import sys

N = int(sys.stdin.readline().rstrip())

# hanoi(N, start, to, via)
# move(N, start, to)

move_list = []

def move(N, start, to):

  move_list.append((start, to))

def hanoi(N, start, to, via):
  if N == 1:
    move(N, start, to)
    return
  else:
    hanoi(N-1, start, via, to)
    move(N, start, to)
    hanoi(N-1, via, to, start)

hanoi(N, 1, 3, 2)

print(len(move_list))
for item in move_list:
  print(item[0], item[1])

```

### 공부

3가지 과정이 존재

1. 시작에서 경유점으로 모든 원판을 옮기는 과정 (제일 아래있는 원판을 옮기기 위해)
2. 제일 아래 있던 원판을 목표로 옮기는 과정
3. 1번에서 옮긴 경유점을 시작점, 원래 시작점을 경유점으로 스위치 후 제일 아래 있는 원판 꺼내는 과정을 반복
