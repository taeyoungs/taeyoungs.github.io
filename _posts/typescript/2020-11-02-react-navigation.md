---
layout: post
title: Type checking with TypeScript (RN Navigation) [정리]
date: Mon November 2 2020 21:10:04 GMT+0900
author: Jang Taeyoung
categories: typescript
tags: typescript react-native navigation
---

React Navigation is written with TypeScript and exports type definitions for TypeScript projects.

> 출처: https://reactnavigation.org/docs/typescript

## Type checking the navigator

route 이름과 파라미터들의 타입 체크를 위해서 첫 번째 할 일은 route 이름을 route 파라미터들에 매핑하는 타입 객체를 만드는 것이다. 예를 들어 root navigator 안에 Profile이라는 route가 있으며 파라미터값으로 userId 값을 가진다고 가정하면 다음과 같이 작성할 수 있다.

> To type check our route name and params, the first thing we need to do is to create an object type with mappings for route name to the params of the route. For example, say we have a route called Profile in our root navigator which should have a param userId:

```typescript

type RootStackParamList = {
  Profile: { userId: string };
};

```

이를 다른 route들에게도 동일하게 적용해주면

```typescript

type RootStackParamList = {
  Home: undefined;
  Profile: { userId: string };
  Feed: { sort: 'latest' | 'top' } | undefined;
};

```

**undefined**는 아무런 값도 파라미터로 갖지 않는다는 뜻이다. **undefined**와 유니온 타입을 함께 사용할 경우 파라미터가 들어올 수도 있고 들어오지 않을 수도 있다는 것을 표현할 수 있다. 맵핑을 정의하고 난 후에는 Navigator에게 해당 맵핑을 사용할 거라고 알려줘야 하는데 이를 위해 다음과 같이 createXNavigator 함수의 제네릭으로 설정해준다.

> Specifying undefined means that the route doesn't have params. A union type with undefined (e.g. SomeType | undefined) means that params are optional. <br /> After we have defined the mappings, we need to tell our navigator to use it. To do that, we can pass it as a generic to the createXNavigator functions:

```typescript
import { createStackNavigator } from '@react-navigation/stack';

const RootStack = createStackNavigator<RootStackParamList>();
```

그리고 나서 다음과 같이 사용하자.

```typescript
<RootStack.Navigator initialRouteName="Home">
  <RootStack.Screen name="Home" component={Home} />
  <RootStack.Screen
    name="Profile"
    component={Profile}
    initialParams={{ userId: user.id }}
  />
  <RootStack.Screen name="Feed" component={Feed} />
</RootStack.Navigator>
```

이는 Navigator와 Screen 컴포넌트들의 props에게 타입 체킹과 자동 완성 기능을 제공해줄 것이다.

<br /><br />

## Type checking screens

Screen 컴포넌트들의 타입 체크를 위해선, navigation과 route prop를 받는 Screen에게 해당 props의 설명이 필요하다. 이를 위해 Navigator에 해당하는 타입을 import 해줄 필요가 있다. 예를 들어 다음과 같이 StackNavigator를 위한 StackNavigationProp가 있다.

> To type check our screens, we need to annotate the navigation prop and the route prop received by a screen. <br />To annotate the navigation prop, we need to import the corresponding type from the navigator. For example, StackNavigationProp for @react-navigation/stack:

```typescript
Copy
import { StackNavigationProp } from '@react-navigation/stack';

type ProfileScreenNavigationProp = StackNavigationProp<
  RootStackParamList,
  'Profile'
>;

type Props = {
  navigation: ProfileScreenNavigationProp;
};
```

Navigation prop 타입은 2가지 제네릭을 갖는데, 하나는 이전에 타입 객체로 만들어놓은 파라미터 목록이고 다른 하나는 현재 route명이다. 이를 통해서 navigate를 사용해서 이동 또는 push할 때 route명과 파라미터를 타입 체크할 수 있다. 현재 route명은 setParams를 호출할 때 타입 체크를 위해서 필요하다.

> The type for the navigation prop takes 2 generics, the param list object we defined earlier, and the name of the current route. This allows us to type check route names and params which you're navigating using navigate, push etc. The name of the current route is necessary to type check the params when you call setParams.

Drawer, BottomTab Navigator를 사용할 때도 비슷하게 import 하여 사용할 수 있다. route prop를 위해서는 RouteProp import하여 다음과 같이 사용해야한다.

> Similarly, you can import DrawerNavigationProp from @react-navigation/drawer, BottomTabNavigationProp from @react-navigation/bottom-tabs etc. <br />To annotate the route prop, we need to use the RouteProp type from @react-navigation/native:

```typescript
import { RouteProp } from '@react-navigation/native';

type ProfileScreenRouteProp = RouteProp<RootStackParamList, 'Profile'>;

type Props = {
  route: ProfileScreenRouteProp;
};
```

이는 route.params 같은 route 객체를 타입 체크 해준다.
<br />

정리하면:

```typescript
import { RouteProp } from '@react-navigation/native';
import { StackNavigationProp } from '@react-navigation/stack';

type RootStackParamList = {
  Home: undefined;
  Profile: { userId: string };
  Feed: { sort: 'latest' | 'top' } | undefined;
};

type ProfileScreenRouteProp = RouteProp<RootStackParamList, 'Profile'>;

type ProfileScreenNavigationProp = StackNavigationProp<
  RootStackParamList,
  'Profile'
>;

type Props = {
  route: ProfileScreenRouteProp;
  navigation: ProfileScreenNavigationProp;
};
```

navigation과 route props 타입 정의를 따로 하지 않고 같이 하는 방법도 있다. 사용하고자 하는 Navigator에 맞게 import 하여 다음과 같이 사용하면 된다.

> Alternatively, you can also import a generic type to define types for both the navigation and route props from the corresponding navigator:

```typescript
import { StackScreenProps } from '@react-navigation/stack';

type RootStackParamList = {
  Home: undefined;
  Profile: { userId: string };
  Feed: { sort: 'latest' | 'top' } | undefined;
};

type Props = StackScreenProps<RootStackParamList, 'Profile'>;
```

그리고 나서 정의해놓은 Props를 컴포넌트에서 사용하려면 다음과 같이 사용하면 된다.

- 함수형 컴포넌트
```typescript
function ProfileScreen({ route, navigation }: Props) {
  // ...
}
```

- 클래스형 컴포넌트
```typescript
Copy
class ProfileScreen extends React.Component<Props> {
  render() {
    // ...
  }
}
```

공홈에서는 컴포넌트 파일에서 Props 타입을 반복해서 만들기 보다는 types.tsx라는 파일을 따로 만들어서 원하는 곳에 import 하여 사용하는 방법을 추천하고 있다.

> We recommend creating a separate types.tsx file where you keep the types and import them in your component files instead of repeating them in each file.


## Nesting navigators

2개 이상의 Navigator를 사용할 때, Screen의 navigation prop은 여러 개의 navigation prop 집합이다. 예를 들어, StackNavigation 안에 TabNavigiontion이 위치한 구조가 있다고 가정해보자. 이때, navigation prop은 **jumpTo** (TabNavigator에 존재하는)와 **push** (StackNavigator에 존재하는) 모두를 갖고 있을 것이다. 여러 개의 navigator 타입을 좀 더 쉽게 정의하기 위해서 CompositeNavigationProp를 사용하자.

> When we nest navigators, the navigation prop of the screen is a combination of multiple navigation props. For example, if we have a tab inside a stack, the navigation prop will have both jumpTo (from the tab navigator) and push (from the stack navigator). To make it easier to combine types from multiple navigator, you can use the CompositeNavigationProp type:

```typescript
import { CompositeNavigationProp } from '@react-navigation/native';
import { BottomTabNavigationProp } from '@react-navigation/bottom-tabs';
import { StackNavigationProp } from '@react-navigation/stack';

type ProfileScreenNavigationProp = CompositeNavigationProp<
  BottomTabNavigationProp<TabParamList, 'Profile'>,
  StackNavigationProp<StackParamList>
>;
```

CompositeNavigationProp은 2가지 파라미터값을 갖는데, 첫 번째 파라미터로는 본래의 navigation type (Screen을 위한 Navigator, 위 가정대로라면 Profile screen을 포함하고 있는 TabNavigator가 된다.) 그리고 두 번째 파라미터로는 상위의 Navigator이다.(primary navigator를 감싸고 있는 navigator). 하위 navigation 타입은 항상 Screen의 route명을 두 번째 파라미터값으로 갖고 있어야만 한다.

> The CompositeNavigationProp type takes 2 parameters, first parameter is the primary navigation type (type for the navigator that owns this screen, in our case the tab navigator which contains the Profile screen) and second parameter is the secondary navigation type (type for a parent navigator). The primary navigation type should always have the screen's route name as it's second parameter.

감싸고 있는 Navigator가 여러 개라면 다음과 같이 nested 형식으로 만들어주어야 한다.

> For multiple parent navigators, this secondary type should be nested:

```typescript
type ProfileScreenNavigationProp = CompositeNavigationProp<
  BottomTabNavigationProp<TabParamList, 'Profile'>,
  CompositeNavigationProp<
    StackNavigationProp<StackParamList>,
    DrawerNavigationProp<DrawerParamList>
  >
>;
```

## Annotating useNavigation, useRoute

hooks를 이용하여 navigation과 route prop를 가져와서 사용할 수 있는데 이때 타입 체크를 위해 다음과 같이 사용한다.

```typescript
const navigation = useNavigation<ProfileScreenNavigationProp>();
const route = useRoute<ProfileScreenRouteProp>();
```

사용하는 타입 매개 변수가 정확하지 않을 수 있고 정적으로 확인할 수 없기 때문에 이 방법을 통해서는 타입 체크가 안전하지는 않다는 점에 유의해야한다.

> It's important to note that this isn't completely type-safe because the type parameter you use may not be correct and we cannot statically verify it.


## Annotating options and screenOptions

Navigator 컴포넌트에 Screen 옵션 또는 screenOptions prop를 보내서 사용할 때, 옵션들은 이미 타입 체크가 된 상태라서 추가적으로 뭘 해줄 필요는 없다. 단, 해당 옵션들을 추출하여 다른 객체에서 사용하고 싶을 때가 있을 텐데 그때는 다음과 같이 사용하면 된다. (사용하고자 하는 Navigator에 맞게 import 하여 사용)

> When you pass the options to a Screen or screenOptions prop to a Navigator component, they are already type-checked and you don't need to do anything special. However, sometimes you might want to extract the options to a separate object, and you might want to annotate it.

```typescript
import { StackNavigationOptions } from '@react-navigation/stack';

const options: StackNavigationOptions = {
  headerShown: false,
}
```