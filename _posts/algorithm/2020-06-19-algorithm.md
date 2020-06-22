---
layout: post
title: 200619_Algorithm
date: Sat June 19 2020 22:30:04 GMT+0900
author: Jang Taeyoung
categories: algorithm
tags: algorithm
---

> 나를 위한 알고리즘 정리 (2) - 기억에 남는 문제 (문자열 파트)

<br /><br />

## 문제 1

알파벳 소문자, 대문자, 숫자 0-9중 하나가 주어졌을 때, 주어진 글자의 아스키 코드값을 출력하는 프로그램을 작성하시오.

### 입력

알파벳 소문자, 대문자, 숫자 0-9 중 하나가 첫째 줄에 주어진다.

### 출력

입력으로 주어진 글자의 아스키 코드 값을 출력한다.

## 풀이 (python)

<hr>

```python

import sys

alpha = sys.stdin.readline().rstrip()

print(ord(alpha))

```

### 공부

Javascript로 풀듯이 생각했다가 대차게 틀렸다. Python에서 아스키 코드 변환하는 방법은

> chr(숫자) => 문자 <br />ord(문자) => 숫자

<br /><br />

## 문제 2

그룹 단어란 단어에 존재하는 모든 문자에 대해서, 각 문자가 연속해서 나타나는 경우만을 말한다. 예를 들면, ccazzzzbb는 c, a, z, b가 모두 연속해서 나타나고, kin도 k, i, n이 연속해서 나타나기 때문에 그룹 단어이지만, aabbbccb는 b가 떨어져서 나타나기 때문에 그룹 단어가 아니다. <br />
단어 N개를 입력으로 받아 그룹 단어의 개수를 출력하는 프로그램을 작성하시오.

### 입력

첫째 줄에 단어의 개수 N이 들어온다. N은 100보다 작거나 같은 자연수이다. 둘째 줄부터 N개의 줄에 단어가 들어온다. 단어는 알파벳 소문자로만 되어있고 중복되지 않으며, 길이는 최대 100이다.

### 출력

첫째 줄에 그룹 단어의 개수를 출력한다.

## 풀이 (python)

<hr>

```python

import sys

n = int(sys.stdin.readline().rstrip())

word_list = [sys.stdin.readline().rstrip() for x in range(n)]

count = 0

for word in word_list:
  split_word = list(word)
  if len(split_word) == 1 or len(split_word) == 2:
    count += 1
    continue
  is_group = False
  for idx, alpha in enumerate(split_word[0:len(split_word)-2]):
    if split_word[idx+1] != alpha and len(split_word)-1 != idx:
      if alpha not in split_word[idx+2:len(split_word)]:
        is_group = True
      else:
        is_group = False
        break
    else:
      is_group = True

  if is_group:
    count += 1

print(count)

```

### 공부

1글자 또는 2글자는 무조건 그룹 단어가 되기 때문에 앞에서 예외 처리를 해줬다. 연속으로 같은 알파벳이 오는 경우는 확인할 필요 없으니 다음 알파벳이 다른 알파벳이며 그 이후에 전에 있었던 알파벳이 다시 나오는 경우만 체크했다. 음 .. 뭔가 더 이쁘게 짤 수 있었던 것 같은데 아쉽다. 초반에 index가 넘어가거나 무조건 그룹 단어인 경우가 있다는 것을 생각하지 못하고 정답을 제출했다가 틀렸는데 다음엔 이런 경우를 더 고려할 수 있었으면 좋겠다.

<br /><br />
