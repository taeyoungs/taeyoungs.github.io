---
layout: post
title: Typescript with React (1)
date: Mon June 8 2020 21:10:04 GMT+0900
author: Jang Taeyoung
categories: typescript
tags: typescript react
---

> 나를 위한 Typescript 개념 정리 (2)

<br /><br />

## 프로젝트 생성하기

> npx create-react-app project-name --typescript

React에서는 기본적으로 Typescript하고 같이 사용할 수 있기 때문에 프로젝트를 생성할 때 같이 사용한다고 옵션으로 알려주기만 하면 된다.
<br /><br />

## @type/

문제는 Typescript가 React가 사용하는 라이브러리들을 알지 못한다는 점이다. 따라서, 이에 대한 정보를 Typescript에게 알려줄 필요가 있는데 그게 바로 **definitelytyped/@type**이다.
<br />

똑똑한 분들이 Typescript가 알 수 있도록 React에서 주로 사용되는 라이브러리에 대한 type 정의를 이미 해놓았다. 따라서, yarn 또는 npm으로 사용하고자 하는 라이브러리를 **@type/name**으로 설치해주자.
<br />

혹시나 사용하고자 하는 라이브러리가 @type으로 정의되어 있지 않다면, tsconfig.json 옵션에서 **"noImplicitAny": true**를 설정해주어 Typescript가 import하는 라이브러리 체크를 자동으로 하지 않도록 해주자.
<br /><br />

## React State and Typescript
<hr>

### props와 state type 설정

typescript에겐 state가 처음부터 존재하지 않기 때문에 React state를 생성하게 되면 오류가 발생한다. 따라서, typescript에게 props와 state에 대한 interface 설정이 필요하다.

useState를 사용해서 state를 생성했다면 Generics이 필요한 경우를 제외하고는 알아서 잘 추론한다. <br />
> => Generics이 필요한 경우<br />1. state가 null일 수도 있는 경우<br />2. state 타입이 까다로운 구조를 가진 객체 또는 배열일 경우

{% highlight javascript %}

const [count, setCount] = useState<number>(0);

const [info, setInformation] = useState<Information | null>(null);

{% endhighlight %}

위에는 둘 다 <Generics>를 적어놨지만 null일 수도 있는 경우가 아니라면 작성하지 않아도 React + Typescript가 알아서 잘 추론해준다.

### 함수형 컴포넌트 props, event

{% highlight javascript %}

interface IProps {
  text: string;
  onChange: (event: React.InputHTMLAttributes<HTMLInputElement>) => void;
}

const Input: React.FunctionComponent<IProps> = ({ text, onChange }) => {
  return <input type="text" onChange={onChange} />;
};

{% endhighlight %}

Typescript에게 해당 컴포넌트는 함수형 컴포넌트라고 알려주고 (:React.FunctionComponent), <type>을 통해서 props에 대한 type을 정의해준다.
<br />
onChange, onClick과 같은 이벤트를 설정해줄 때 자동으로 들어오는 인자값인 event도 Typescript에게 어떤 type인지 알려주어야 한다. 위에서는 복잡하고 길게 나와있지만 해당 이벤트에 마우스 커서를 올리면 Vscode가 친절히 어떤 것을 적어주어야 하는지 알려주니 적극적으로 이용하도록 하자.

## style-components
<hr >

style-components도 Typescript에겐 존재하지 않았던 것이기 때문에 이에 대한 Type도 정의해주어야 한다. 하지만 style 특성상 엄청나게 많은 Type 정의(interface)를 작성하게 되기에 **style.d.ts** 파일을 만들어서 이를 이용하도록 하자.
<br />
사용 방식은 Context와 비슷하게 최상단 컴포넌트를 감싸서 사용한다. ThemeProvider를 호출해서 props인 theme에 style.d.ts 파일을 넣어서 사용하면 된다.

### style.d.ts

{% highlight javascript %}

// import original module declarations
import 'styled-components'

// and extend them!
declare module 'styled-components' {
  export interface DefaultTheme {
    borderRadius: string

    colors: {
      main: string
      secondary: string
    }
  }
}

{% endhighlight %}

<br /><br />