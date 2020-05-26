---
layout: post
title:  "Virtual Dom"
date: Wed May 27 2020 01:12:20 GMT+0900
author: Jang Taeyoung
categories: react
cover: "/assets/react.png"
---


> 나를 위한 React 개념 정리 (1)

# 왜 Virtual DOM 인가 ?
* * *

React, Vue 등 SPA 프레임워크에서 사용되는 Virtual DOM에 대해 알아보자.<br />
잠깐, Virtual DOM에 대한 이해를 돕기 위해 먼저 브라우저 렌더링 엔진이 일하는 과정을 살펴보자. 🚀<br /><br />

## 브라우저의 동작

1. DOM Tree 생성
2. Render Tree 생성 + Attachment
3. Layout (= reflow)
4. Paint

브라우저들은 대부분 4가지의 동작 과정을 거친다. (브라우저마다 미묘한 차이는 있을 수 있다.)<br /><br />

#### 1. DOM Tree 생성

브라우저가 HTML을 전달받으면 브라우저의 렌더링 엔진이 이를 파싱하여 노드로 이루어진 DOM 트리를 생성하고 CSS 파일 또한 파싱해서 CSSOM 트리를 만들게 된다.

#### 2. Render Tree 생성 + Attachment

파싱한 스타일 정보를 토대로 노드의 스타일을 처리하는 과정을 **Attachment**라고 한다. DOM 트리의 모든 노드들은 **attach**라는 메소드를 가지고 있으며, 브라우저가 노드들을 하나하나 찾아가서 이 메소드를 실행하고 스타일 정보를 계산한다.<br />
이 과정을 마치고 나면 DOM과 CSSOM이 결합된 Render Tree(모든 노드의 콘텐츠 및 스타일 정보를 포함하고 있는)가 생성된다.

#### 3. Layout

노드들의 정확한 위치와 크기를 계산하는 단계

#### 4. Paint

1, 2, 3번의 과정을 통해 노드들의 계산된 스타일 및 위치와 크기 등에 대해서 파악했으므로, 각 노드들을 실제 화면에 그리는 단계
<br /><br /> 

## DOM 조작

문제는 DOM을 조작할 때마다 1, 2, 3, 4번의 과정을 다시 밟게 된다는 점이다. 예를 들어 20번의 DOM 조작을 시도했다면 20번 레이아웃을 재계산하고 20번 다시 렌더링하는 과정을 거치게 된다. 

> 여기서 Virtual DOM이 어떤 역할인데?

Virtual DOM은 말 그대로 가상의 DOM, 렌더링 되지 않는 오프라인 DOM에 DOM 조작으로 생기는 변화를 적용시킨다. 렌더링 되지 않는 가상의 DOM이기 때문에 연산의 비용이 적다. Layout과 Paint의 규모가 커질지언정 **모든 변화를 하나로 묶어서 처리하는 것**이 포인트이다.

> 그럼 이건 Virtual DOM만 할 수 있겠네?

No, 이 과정은 Virtual DOM 없어도 가능하다. 변화가 생기면, 생기는 변화들을 모두 묶어서 한번에 DOM Fragment에 전달해주면 된다. 
<br /><br />

문제는 이를 스스로 하나하나 작업을 한다고 가정하면, DOM 조작으로 어떤 것엔 변화가 생겼고 어떤 것엔 변화가 생기지 않았는지 계속해서 파악한 상태에서 작업을 해야한다는 것인데 이게 쉽지 않다는 것이다.<br /><br />

바로 이 과정을 **Virtual DOM**이 자동화하고 추상화하여 사용자가 DOM에 일일이 접근하지 않아도 어떤 것이 변했고 어떤 것이 변하지 않았는지를 알아낸다. 결과적으로 DOM 관리를 Virtual DOM에게 맡김으로써 Virtual DOM이 변화들을 캐치해서 이를 자동으로 DOM에 반영해준다.
* * *
### 여담

React(Virtual DOM)는 DOM보다 빠르지 않다.

단, 유지 보수 가능한 어플리케이션을 만들 수 있게 도와주고, 대부분의 경우에 충분히 빠르기 때문에 <br />사용된다.
