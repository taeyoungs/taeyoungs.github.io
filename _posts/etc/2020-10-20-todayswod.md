---
layout: post
title: 오늘의 와드 앱 - RN
date: Fri September 25 2020 18:39:20 GMT+0900
author: Jang Taeyoung
categories: etc
---

### 프로젝트 생성

- expo init -t expo-template-blank-typescript

### Storybook

- 현재 내 환경에서 storybook-server 활용이 불가능 (노트북 환경이 윈도우)
- RN으로 개발 중이니 device에서 storybook을 활용


## Atomic Design

### 도입하는 목적

Airbnb clone app을 만들면서 내내 느꼈던 점 중에 하나는 거의 비슷하지만 디자인 요소가 조금 다른 컴포넌트들이 계속해서 만들어진다는 점이다. 분명 다른 스크린에서 거의 흡사하게 만들었던 View인데 현재 작업하고 있는 컴포넌트에서 재사용하지 못하는 점이 너무 불편했다. Container, Presenter 구조로 데이터 로직과 레이아웃 로직의 분리는 성공했으나 Presenter에서 계속해서 늘어나는 레이아웃 구조를 내부적으로 분리해 재사용할 수 있는 방법이 없는지 고민하는 와중에 Atomic Design을 발견했다.

> 출처 : https://vallista.kr/2020/03/29/Component-%EB%B6%84%EB%A6%AC%EC%9D%98-%EB%AF%B8%ED%95%99/

Atomic Design은 5가지 구성요소로 나뉩니다.

- atoms (원자) : 레이아웃을 구성하는 가장 작은 단위입니다. p, span등 가장 작은 elements 요소를 일컫습니다.
- molecules (분자) : 레이아웃을 조립하는 단위입니다. p, span등을 뭉쳐서 하나의 분자를 만듭니다.
- organisms (유기체) : 원자, 분자를 조합해 만든 커다란 하나의 단위입니다.
- templates (템플릿) : 원자, 분자, 유기체를 조합해 만든 하나의 페이지 틀입니다. 데이터를 전달받기 전 페이지 레이아웃을 말합니다.
- pages (페이지) : templates에 데이터를 주입해서 유저에게 최종적으로 보여주는 페이지입니다.

### 장점과 단점

- 장점

1. 디자인 요소에 대한 변경을 컴포넌트 스타일 변경으로 한번에 처리 가능하다.
2. 스타일을 최소 단위로 구현하기 때문에 거의 비슷하지만 다른 스타일들을 제거할 수 있다.
3. 높은 재사용률을 확립할 수 있다.


- 단점

1. Top to Bottom 설계가 아니라 Bottom to Top 설계로 변경된다.
2. atoms 단위의 설계가 잘못 된다면 기존에 작업하던 방식보다 더 복잡해질 수 있다.
3. 컴포넌트 분리가 제대로 되지 않는다면 유지보수가 어려워진다.