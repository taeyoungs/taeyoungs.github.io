---
layout: post
title: "VanillaJS + Redux"
date: Thu August 20 2020 18:20:25 GMT+0900
author: Jang Taeyoung
categories: react
tags: redux react
cover: "/assets/react.png"
---

> 나를 위한 React 개념 정리 (4) <br /> VanillaJS부터 React까지

<br />

### Redux가 필요한 이유

[사람들은 언제 Redux가 필요하다고 느낄까?](https://www.youngslog.kr/react/2020/05/27/redux.html)

<br />

### Redux를 구성하는 요소들

- Store
- Reducer
- Action
- Dispatch
- Subscribe
- ...

<br />

#### 1. Store

State를 관리하는 데이터 박스, 조금 더 풀어서 설명하면 앱 내에서 바뀌는 가변데이터들을 관리하고 상위 컴포넌트로부터 데이터를 넘겨받지 않고 하위 컴포넌트가 직접 접근하여 데이터를 얻기 위해 만들어진 장소 정도일까.. <br />
예를 들어, 앱 내에서 함수에 의해 변하는 데이터를 count라고 한다면 Store를 생성하고 count를 Store에 저장하여 관리를 하게 된다.

```javascript
import { createStore } from "redux";

const store = createStore(reducer);
```

store를 만드는 방법 자체는 어렵지 않다. createStore 메서드를 통해 손쉽게 생성할 수 있으며, 한 앱당 하나의 Store를 만든다. 인자값으로 Reducer를 넣어줌으로써 Store는 Reducer, 현재 state 정보 그리고 몇 가지 내장 함수(subscribe, dispatch ...)를 보유하고 있게 된다.

<br />

#### 2. Reducer

Store에 담긴 state 상태를 변경하는 역할을 담당한다. 내가 Store에 저장한 데이터를 수정하는 함수이며, Store에서의 예를 이어서 들면 Store에 저장된 count(현재 state)를 받아서 작업을 한 후 작업을 마친 count를 반환하여 새로운 state 상태를 만든다.

```javascript
import { createStore } from "redux";

const add = document.getElementById("add");
const minus = document.getElementById("minus");
const span = document.querySelector("span");

// 현재의 state 값이 인자값으로 들어온다.
const modifyCount = (state = 0) => {
  return state;
};

// Store 생성 및 Reducer 인자값
const countStore = createStore(modifyCount);

// .getState() : store의 현재 state 반환
console.log(countStore.getState());
```

<br />

#### 3. Action

Reducer와 소통하기 위한 인자값이다. 예를 들어, count라는 값을 Store에 저장 후 이를 증가시키거나 감소시킬 예정이다. Reducer 함수는 만들었지만 Reducer가 어떠한 값을 보고 count를 증가시킬지 감소시킬지 알 수 없기 때문에 이를 도와주기 위해 action 이라는 인자값을 활용한다.

```javascript
const ADD = "ADD";
const MINUS = "MINUS";

const modifyCount = (state = 0, action) => {
  switch (action.type) {
    case ADD:
      return state + 1;
    case MINUS:
      return state - 1;
    default:
      return state;
  }
};
```

action은 어떠한 작업인지를 명시하는 type과 데이터를 담아 보내기 위한 payload로 구성되어있다. type의 종류는 보통 Reducer 바로 위에 작성하며, type으로 경우를 나누기 때문에 if문 보다는 switch문을 사용한다.

#### 4. Action Creator

action을 만들어주는 함수인데, 단순히 인자값을 받아서 action 객체를 반환시켜주는 함수이다. 다음과 같이 만들어서 사용한다.

```javascript
const ADD_TODO = "ADD_TODO";
const DELETE_TODO = "DELETE_TODO";

const addToDo = (text) => {
  return {
    type: ADD_TODO,
    text,
  };
};

const deleteToDo = (id) => {
  return {
    type: DELETE_TODO,
    id,
  };
};
```

<br />

#### 5. Dispatch

Store가 가지고 있는 내장 함수 중 하나로 action을 발생시키는 역할을 담당한다. action을 인자값으로 받으며, dispatch를 통해 Store를 호출하면 Store는 Reducer를 실행시켜 action 타입에 맞는 로직이 있는지 확인하고 있다면 해당 작업을 진행한다.

```javascript
const dispatchDeleteToDo = (e) => {
  const id = e.target.parentNode.id;
  store.dispatch(deleteToDo(id));
  toDoPainting();
};

const dispatchAddToDo = (text) => {
  store.dispatch(addToDo(text));
};
```

<br />

#### 6. Subscribe

Store가 가지고 있는 내장 함수 중 하나로 subscribe에 인자값으로 특정 함수를 전달해주면, dispatch가 발생할 때마다 인자값으로 전달해준 함수를 호출한다.

```javascript
store.subscribe(() => console.log(store.getState()));
```

<br />
