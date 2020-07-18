---
layout: post
title: 200702_Algorithm
date: Thu July 2 2020 14:05:04 GMT+0900
author: Jang Taeyoung
categories: algorithm
tags: algorithm
---

> 나를 위한 알고리즘 정리 (2) - 기억에 남는 문제 (수학 1 파트)

<br /><br />

## 문제

우현이는 어린 시절, 지구 외의 다른 행성에서도 인류들이 살아갈 수 있는 미래가 오리라 믿었다. 그리고 그가 지구라는 세상에 발을 내려 놓은 지 23년이 지난 지금, 세계 최연소 ASNA 우주 비행사가 되어 새로운 세계에 발을 내려 놓는 영광의 순간을 기다리고 있다.
<br /><br />
그가 탑승하게 될 우주선은 Alpha Centauri라는 새로운 인류의 보금자리를 개척하기 위한 대규모 생활 유지 시스템을 탑재하고 있기 때문에, 그 크기와 질량이 엄청난 이유로 최신기술력을 총 동원하여 개발한 공간이동 장치를 탑재하였다. 하지만 이 공간이동 장치는 이동 거리를 급격하게 늘릴 경우 기계에 심각한 결함이 발생하는 단점이 있어서, 이전 작동시기에 k광년을 이동하였을 때는 k-1 , k 혹은 k+1 광년만을 다시 이동할 수 있다. 예를 들어, 이 장치를 처음 작동시킬 경우 -1 , 0 , 1 광년을 이론상 이동할 수 있으나 사실상 음수 혹은 0 거리만큼의 이동은 의미가 없으므로 1 광년을 이동할 수 있으며, 그 다음에는 0 , 1 , 2 광년을 이동할 수 있는 것이다. ( 여기서 다시 2광년을 이동한다면 다음 시기엔 1, 2, 3 광년을 이동할 수 있다. )
<br /><br />
김우현은 공간이동 장치 작동시의 에너지 소모가 크다는 점을 잘 알고 있기 때문에 x지점에서 y지점을 향해 최소한의 작동 횟수로 이동하려 한다. 하지만 y지점에 도착해서도 공간 이동장치의 안전성을 위하여 y지점에 도착하기 바로 직전의 이동거리는 반드시 1광년으로 하려 한다.
<br /><br />
김우현을 위해 x지점부터 정확히 y지점으로 이동하는데 필요한 공간 이동 장치 작동 횟수의 최솟값을 구하는 프로그램을 작성하라.

## 입력

입력의 첫 줄에는 테스트케이스의 개수 T가 주어진다. 각각의 테스트 케이스에 대해 현재 위치 x 와 목표 위치 y 가 정수로 주어지며, x는 항상 y보다 작은 값을 갖는다. (0 ≤ x < y < 231)

## 출력

각 테스트 케이스에 대해 x지점으로부터 y지점까지 정확히 도달하는데 필요한 최소한의 공간이동 장치 작동 회수를 출력한다.

## 풀이 (python)

<hr>

```python

import sys
import math

t = int(sys.stdin.readline().rstrip())

xy = [sys.stdin.readline().rstrip() for c in range(t)]

for case in xy:
  item = list(map(int, case.split()))
  dist = item[1] - item[0]
  if dist <= 3:
    print(dist)
  else:
    n = int(math.sqrt(dist))
    if n**2 == dist:
      print(n*2-1)
    elif n**2 < dist and n**2+n >= dist:
      print(n*2)
    else:
      print(n*2+1)

```

### 공부

중심점인 N 값을 찾는 것이 주 목표였고 이를 찾기 위해 조건에 만족하는 N이 나올 때까지 반복문을 돌렸으나 시간 초과 에러가 발생했다. 이런 경우 반복문을 사용하지 않고도 충분히 N 값을 찾을 수 있다는 뜻이었는데 방법을 찾지 못해서 결국 힌트를 봤다. 😭
<br />
힌트를 보고 나니 루트를 왜 생각하지 못했나 싶었다. 생각하고도 이게 맞나 싶었던 것 같기도 하고 .. 아무튼 루트와 정수값 변경을 이용해서 N을 찾고 조건에 맞을 때 이동 횟수 출력을 진행했다.
<br />
위 코드에서 else 부분을 나중에 추가해서 정답을 맞췄는데 반복문에서 사용하던 조건을 이어서 사용하다 보니 dist를 루트와 정수값을 이용해 N으로 만드는 과정에서 값이 작아져 조건이 바뀔 수도 있다는 생각을 하지 못했다. 이 문제를 며칠 동안 잡고 있어서 그런지 아쉬운 느낌이 많이 들었다.

<br /><br />

## 문제

평소 반상회에 참석하는 것을 좋아하는 주희는 이번 기회에 부녀회장이 되고 싶어 각 층의 사람들을 불러 모아 반상회를 주최하려고 한다.
<br /><br />
이 아파트에 거주를 하려면 조건이 있는데, “a층의 b호에 살려면 자신의 아래(a-1)층의 1호부터 b호까지 사람들의 수의 합만큼 사람들을 데려와 살아야 한다” 는 계약 조항을 꼭 지키고 들어와야 한다.
<br /><br />
아파트에 비어있는 집은 없고 모든 거주민들이 이 계약 조건을 지키고 왔다고 가정했을 때, 주어지는 양의 정수 k와 n에 대해 k층에 n호에는 몇 명이 살고 있는지 출력하라. 단, 아파트에는 0층부터 있고 각층에는 1호부터 있으며, 0층의 i호에는 i명이 산다.

## 입력

첫 번째 줄에 Test case의 수 T가 주어진다. 그리고 각각의 케이스마다 입력으로 첫 번째 줄에 정수 k, 두 번째 줄에 정수 n이 주어진다. (1 <= k <= 14, 1 <= n <= 14)

## 출력

각각의 Test case에 대해서 해당 집에 거주민 수를 출력하라.

## 풀이 (python)

<hr>

```python

import sys

t = int(sys.stdin.readline().rstrip())

kn = [int(sys.stdin.readline().rstrip()) for x in range(t*2)]

k_list, n_list = [], []
f_list, b_list = [], []

for idx, value in enumerate(kn):
  if idx%2 == 0:
    k_list.append(value)
  else:
    n_list.append(value)

for idx, k_value in enumerate(k_list):
  for k_idx in range(k_value):
    if k_idx == 0:
      for x in range(1, n_list[idx]+1):
        f_list.append(x)
    else:
      sum_temp = 0
      if k_idx%2 == 1:
        b_list = []
        for idx, num in enumerate(f_list):
          sum_temp += num
          b_list.append(sum_temp)
      else:
        f_list = []
        for idx, num in enumerate(b_list):
          sum_temp += num
          f_list.append(sum_temp)
  if k_value%2 == 0:
    print(sum(b_list))
  else:
    print(sum(f_list))
  f_list, b_list = [], []

```

### 공부

> 내가 푼 방법이 기억에 남아서 포스트로 정리

여전히 내가 효율적으로 풀었는가에 대해서는 확신이 없다. 🤔 하지만 이런 식으로도 풀었었다고 기록으로 남기고 싶었다. 정답을 알기 위해서는 입력으로 들어온 층보다 아래의 층들에 대한 정보가 모두 필요하다고 생각했고 어떻게 하면 리스트로 저장할 수 있을까 고민했다.
<br />
결국 마지막에 필요로 하는 리스트 정보는 입력으로 들어온 층의 바로 전 층에 대한 정보이기에 2개의 리스트를 가지고 전 층과 다음 층을 번갈아가면서 구하는 방식을 선택했다.

<br /><br />
