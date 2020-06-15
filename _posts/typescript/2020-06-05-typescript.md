---
layout: post
title: Typescript (1)
date: Fri June 5 2020 18:30:04 GMT+0900
author: Jang Taeyoung
categories: typescript
tags: typescript
---

> 나를 위한 Typescript 개념 정리 (1)

# Typescript ?

쉽게 생각하면 Type + Javascript, Js 위에서 동작하며 production에 이르러야 알 수 있는 에러들을 사전에 <br />예방할 수 있도록 하는
즉, 개발자들의 실수를 줄여줄 수 있는 언어이다. (물론 그 외에도 더 많은 장점이 있다.)
<br /><br />

## Type

```typescript
let something: string = "Blah Blah";

const hello = (name: string, age: number): string => {
  return `Hello ${name} you are ${age} old`;
};
```

변수와 return 타입도 정의 가능하다. 위와 같이 정의해놓으면 올바르지 않은 인자값이 들어올 경우 컴파일 <br />전에 미리 사용자에게 에러로 알려준다.

## interface

인자값이 Object로 들어오는 경우, Object에 대해 Interface로 Object 타입 정의

```typescript

const youngs = {
	name: "Jang",
	age: 27,
	hungry: true
}

interface IHuman = {
	name: string,
	age: number,
	hungry: boolean
}

const helloToHuman = (human: IHuman) => {
	console.log(`Hello ${human.name} you are ${human.age}`)
}

helloToHuman(youngs);

```

> age? : '?'으로 optional chaning이 가능하다. 해당 인자값이 들어오지 않으면 undefined 반환한다.

## class

react, node, express 등과 같이 사용할 때는 interface 대신 class를 주로 사용한다. 결국은 Js로 컴파일 될 것이기 때문에 Js에서도 존재하는 class를 통해서 Object를 정의하기 위해 class를 사용하는 것 같다.<br />

> 이게 아니면 OOP에서 사용하는 interface, 추상 class의 목적으로 사용하는 것 같기도 ..?

```typescript
class Human {
  public name: string;
  public age: number;
  public gender: string;
  constructor(name: string, age: number, gender: string) {
    (this.name = name), (this.age = age), (this.gender = gender);
  }
}

const person = new Human("youngs", 27, "male");

const sayHi = (person: Human): string => {
  return `Hello, ${person.name} you are ${person.age} years old and ${person.gender}`;
};

console.log(sayHi(person));
```

위와 같이 사용하며, 접근제한자, 변수, return 타입에 대하여 미리 설정이 가능하다.

### 접근제한자 ( public, private, protected )

[Typescript\_홈페이지](https://www.typescriptlang.org/docs/handbook/classes.html)<br />

- private : class의 특정 속성을 숨기고 싶을 때 사용한다. class 내에서만 호출이 가능하며, class 밖에서 호출할 경우 에러가 발생한다. (컴파일 하기 전에 에러 발생)
- protected : 직접적인 인스턴스화는 불가능하지만 protected 생성자나 변수를 갖고 있는 클래스를 상속받아서 인스턴스화하는 것까지 허용된다.

둘 다 비슷하게 동작하지만 가장 큰 차이는 protected는 상속받은 클래스에 한하여 직접적인 변수나 생성자 호출이 가능하다. <br />
예를 들어, 상속받은 클래스에서 protected로 선언된 부모 클래스의 변수나 생성자(super)를 호출 가능하다. <br />( private와 동일하게 변수나 생성자의 직접적인 호출은 불가능 )
