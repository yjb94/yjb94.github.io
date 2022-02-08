---
title: React hooks에 대하여
date: 2020-12-30 12:00:00 +0900
categories: react
---

# 개요

## Hooks는 어떻게 탄생했는가?

일단 리액트에는 크게 두 가지 종류의 컴포넌트 선언 방식이 있다. 하나는 클래스 컴포넌트, 하나는 함수 컴포넌트이다. 최근엔 클래스 컴포넌트를 사용하지 않고 함수 컴포넌트만 사용하는 추세이지만 그래도 장단점을 살펴보자.

클래스 컴포넌트는 상태관리나 라이프 사이클 관련 기능을 사용할 수 있었다. 함수형은 반면에 상태관리나 라이프 사이클을 사용할 수 없었고 선언이 간편하여 프레젠테이션 컴포넌트나 UI작업 등에 활용되었다.

하지만 React 16.8이 등장하면서 함수형에서도 상태와 라이프사이클 관련된 기능을 사용할 수 있게 되면서 큰 의미가 없어졌고 함수형이 대세가 되기 시작했다. 이 React 16.8이 등장할 때 새롭게 추가된 것이 **React Hook**이다.

```jsx
class AwesomeComponent extends React.Component {
  constructor(props) {
    suoer(props);

    this.state = {};
  }

  render() {
    return <div>hello class component!</div>;
  }
}
```

```jsx
const AwesomeComponent = (props) => {
  return <div>hello function component!</div>;
};
```

## Hook은 무엇일까?

> _Hook이 React 버전 16.8에 새로 추가되었습니다. Hook을 이용하여 Class를 작성할 필요 없이 상태 값과 여러 React의 기능을 사용할 수 있습니다._

```jsx
import React, { useState } from "react";

function Example() {
  // "count"라는 새로운 상태 값을 정의합니다.
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

공식문서에서 Hook은 Class를 작성할 필요 없이 상태 값과 여러 React의 기능을 사용할 수 있게 해준다고 한다.

### 컴포넌트 사이의 상태 로직 재사용

React에서는 컴포넌트에 재사용 가능한 행동을 붙이는 방법을 제공하지 않습니다. 그래서 이를 해결하기 위해서 render props나 HOC등 같은 패턴을 사용했습니다. 그러나 이런 패턴들을 사용하려면 컴포넌트를 재구성해야되고, 코드 추적이 어려워집니다. 개발자 도구에서 전형적인 React 어플리케이션을 본다면 provider, consumer, HOC, render props, abstraction... 레이어로 둘러쌓인 Wrapper 지옥을 볼 가능성이 높습니다. 하지만 Hook을 사용하면 **계층 변화 없이 상태를 추상화하여 재사용할 수** 있습니다

### 복잡한 컴포넌트는 이해하기 어렵다

가볍게 시작한 컴포넌트가 나중에는 결국 `componentDidMount` 그리고 `componentDidUpdate`로 데이터를 가져오고, `componentDidMount` 에서 이벤트 리스너를 만들고, `componentWillUnmount` 에서 clean up을 수행하는 등 버그가 쉽게 발생하고 무결성을 해치기 쉬워집니다. 이런 사례에서 상태로직이 모두 한 공간에 있기 때문에 컴포넌트를 작게 만드는 것이 불가능해지고 테스트하기 어려워집니다. 이것을 해결하기 위해 life cycle을 기반으로 쪼개기 보다는 **Hook을 통해서 로직에 기반을 둔 작은 함수로 컴포넌트를 나눌 수 있습니다.**

### Class는 사람과 기계를 혼동시킨다

[Hook의 개요 - React](https://ko.reactjs.org/docs/hooks-intro.html#classes-confuse-both-people-and-machines)

크게 유의미한 내용은 아니니 링크를 참고하도록 하자

# Hook 살펴보기

## Hook의 규칙

- 오직 React 함수 내에서만 호출이 가능하다
  일반적인 JS나 클래스에서 호출이 불가능합니다
- 최상위에서만 호출해야한다.
  **반복문, 조건문 혹은 중첩된 함수 내에서 Hook을 호출하지 말고 항상 함수의 최상위에서 호출해야한다.** 이 규칙을 따라야만 항상 동일한 순서로 Hook이 호출되는 것이 보장된다. [자세한 이유](https://ko.reactjs.org/docs/hooks-rules.html#explanation)

## State Hook

```jsx
import React, { useState } from "react";

function Example() {
  // "count"라는 새로운 상태 값을 정의합니다.
  const [count, setCount] = useState(0);

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

```jsx
class Example extends React.Component {
  constructor(props) {
    super(props);
    this.state = {
      count: 0,
    };
  }

  render() {
    return (
      <div>
        <p>You clicked {this.state.count} times</p>
        <button onClick={() => this.setState({ count: this.state.count + 1 })}>
          Click me
        </button>
      </div>
    );
  }
}
```

여기서 useState가 바로 Hook이다. use는 리액트에서 hook에 사용되는 네이밍 규칙인데, custom hook을 만들 때도 앞에 useCustomHook 이런식으로 붙인다. 앞에 use가 있으면 hook이라는 사실을 기억해두자.

useState는 상태관리 hook으로 현재의 state값과 이 값을 업데이트하는 함수를 쌍으로 제공한다. useState의 인자에 들어가는 값은 초기값으로, 예제의 경우에는 0이 들어갔다. 이 초기값은 첫 번째 랜더링 때 딱 한번만 사용된다.

## Effect Hook

```jsx
import React, { useState, useEffect } from "react";

function Example() {
  const [count, setCount] = useState(0);

  // componentDidMount, componentDidUpdate와 비슷합니다
  useEffect(() => {
    // 브라우저 API를 이용해 문서의 타이틀을 업데이트합니다
    document.title = `You clicked ${count} times`;
  });

  return (
    <div>
      <p>You clicked {count} times</p>
      <button onClick={() => setCount(count + 1)}>Click me</button>
    </div>
  );
}
```

Effect hook은 컴포넌트 내에서 데이터를 가져오거나 구독하는 역할을 합니다. 위의 예시는 React가 DOM을 업데이트 한 뒤에 문서의 타이틀을 바꾸는 예시이다.

Effect hook은 기본적으로 컴포넌트가 랜더링 될 때마다 실행된다.

### Clean up을 사용하지 않는 Effect

네트워크 리퀘스트, DOM 수동 조작, 로깅 등은 정리(clean-up)가 필요 없는 경우이고 이럴 때는 clean up을 해주지 않아도 된다.

위의 예시는 clean up이 필요하지 않은 경우이다. 실행 이후에 따로 신경 쓸 부분이 없기 때문이다.

### Clean up을 사용하는 Effect

```jsx
import React, { useState, useEffect } from "react";

function FriendStatus(props) {
  const [isOnline, setIsOnline] = useState(null);

  useEffect(() => {
    function handleStatusChange(status) {
      setIsOnline(status.isOnline);
    }
    ChatAPI.subscribeToFriendStatus(props.friend.id, handleStatusChange);
    // effect 이후에 어떻게 정리(clean-up)할 것인지 표시합니다.
    return function cleanup() {
      ChatAPI.unsubscribeFromFriendStatus(props.friend.id, handleStatusChange);
    };
  });

  if (isOnline === null) {
    return "Loading...";
  }
  return isOnline ? "Online" : "Offline";
}
```

이런 에시가 clean up을 사용하는 effect의 예시이다. effect에서 `return` 하여 반환하는 경우는 clean up을 실행하는 경우로, 구독 추가와 제거를 위한 로직을 가까이 묶어둘 수 있게 합니다.

clean up코드는 컴포넌트 마운트가 해제될 때 실행됩니다.

### Effect hook 성능 최적화하기

useEffect훅은 컴포넌트가 랜더링 될 때마다 실행된다. 따라서 지나친 호출을 막고자 prevProps와 prevState등의 비교처럼 기존에 사용하던 방식과 비슷한 기능을 제공한다.

```jsx
useEffect(() => {
  document.title = `You clicked ${count} times`;
}, [count]); // count가 바뀔 때만 effect를 재실행합니다.
```

아까의 코드에서 실제로 필요한 것은 count에 대한 데이터이기 때문에 count를 useEffect의 두 번째 인자로 넣어주면 count가 바뀔 때만 실행된다.

만약 빈 배열을 전달하게 되면 props나 state중 어떤 값에도 의존하지 않기 때문에 최초 랜더링시에만 실행되게 된다. (그래서 componentDidMount나 생성자처럼 쓰이기도 한다)

## Context Hook

```jsx
const value = useContext(MyContext);
```

이 개념을 알기 위해서는 [context API에](https://ko.reactjs.org/docs/context.html) 대해서 알고 있는 편이 좋은데, 지금 하다보면 러닝 커브가 너무 복잡해질 수도 있기에 생략하도록 한다.

간단하게 짚고 넘어가자면 useContext는 Context 객체를 해당 컴포넌트에서 사용하겠다는 의미로, context가 변경될 때마다 호출된다.

## useReducer

위에까지가 기본 hook이고 여기서부터는 추가 hook이다. (리액트에서 그렇게 정했다)

```jsx
const [state, dispatch] = useReducer(reducer, initialArg, init);
```

useState의 대체함수로서 `(state, action) => newState` 의 형태로 reducer를 받고 dispatch 메소드의 짝으로 현재 state를 반환한다. redux에 익숙하다면 어떤 개념인 지 알텐데, 사실 현업에서 그렇게 많이 사용되는 부분은 아니니 아, 이런게 있구나 하고 넘어가도 좋다.

## useCallback

memoized된 함수를 반환하는 훅이다.

```jsx
const memoizedCallback = useCallback(() => {
  doSomething(a, b);
}, [a, b]);
```

클래스 컴포넌트에 있던 `shouldComponentUpdate` 와 비슷한 기능인데, 일단 메모이제이션이 뭔지 살짝 짚고 넘어갈 필요가 있다

### Memoization

메모이제이션은 말 그대로 메모를 하는 것이다. 동일한 계산을 반복해야 할 때, 이전에 계산했던 값을 미리 기록하여 연산을 빠르게 하는 방법이다.

위의 예제의 경우를 좀 더 구체화해서, 장바구니에서 물건을 삭제하는 로직이 있다고 가정해보자.

```jsx
const App = () => {
  ...
  // Notice the dependency array passed as a second argument in useCallback
  const handleRemove = React.useCallback(
    (id) => setCartItem(cartItems.filter((cartItem) => cartItem.id !== id)),
    [cartItems]
  );

  ...
};
```

저 `setCartItem`은 복잡도가 아주 높은 함수이고 실행하는데 1초씩 소요된다. 이런 경우에 useCallback을 사용하면 같은 input에 대해서 미리 연산된 값으로 처리하기 때문에 속도를 단축시킬 수 있다

## useMemo

useMemo는 useCallback과 유사하다. 차이점은 useMemo는 메모이제이션된 **값**을 반환하고 useCallback은 **함수**를 반환한다. 따라서 `useCallback(fn, deps)`은 `useMemo(() => fn, deps)`와 같다.

기본적인 내용은 useCallback과 같아서 생략한다. 다만, 어차피 useCallback을 useMemo의 형태로 구현할 수 있기에(특별한 경우를 제외하고) 하나로 통일하여 사용하는 것이 좋다고 생각해서 대부분의 경우에 useMemo만을 사용하고 있다.

## useRef

```jsx
function TextInputWithFocusButton() {
  const inputEl = useRef(null);
  const onButtonClick = () => {
    // `current` points to the mounted text input element
    inputEl.current.focus();
  };
  return (
    <>
      <input ref={inputEl} type="text" />
      <button onClick={onButtonClick}>Focus the input</button>
    </>
  );
}
```

useRef는 `.current` 프로퍼티로 전달된 인자(initialValue)로 초기화된 변경 가능한 ref 객체를 반환한다.

말이 좀 어려운데 쉽게 얘기하자면 DOM의 노드에 접근... 말이 또 어려운데 그냥 저 input이라는 컴포넌트를 담을 수 있는 상자를 제공한다 정도로 기억하자.

유의할 점은 useRef는 값이 변경된다고 리렌더링을 발생시키지 않는다.

## useImperativeHandle

나도 오늘 처음봤는데, 앞으로 유용하게 쓸 것 같다.

```jsx
function FancyInput(props, ref) {
  const inputRef = useRef();
  useImperativeHandle(ref, () => ({
    focus: () => {
      inputRef.current.focus();
    }
  }));
  return <input ref={inputRef} ... />;
}
FancyInput = forwardRef(FancyInput);
```

forwardRef라고 하는 함수 컴포넌트를 ref로 사용하게 해주는 함수가 있는데, 이럴 경우에 부모에게 노출되는 함수들을 커스터마이징 할 수 있다. 항상 forwardRef와 짝을 이루어서 활동한다.

## useLayoutEffect

이 함수는 useEffect와 비슷한데, 차이점은 모든 DOM 변경 후에 동기적으로 발생한다. 따라서 DOM에서 레이아웃을 읽은 후에 동기적으로 리랜더링 되는 경우에 사용한다.

예를 들어서 텍스트로 이루어진 카드 컴포넌트가 있다고 가정하자. 서로 글자 수가 달라서 크기가 다른데, 가장 큰 카드 컴포넌트의 크기에 맞춰서 모두 리사이징 하고 싶을 때 useLayoutEffect를 사용할 수 있다. 자주 사용하지는 않으니 이런게 있구나 기억하고 나중에 써먹자

## useDebugValue

```jsx
function useFriendStatus(friendID) {
  const [isOnline, setIsOnline] = useState(null);

  // ...

  // Show a label in DevTools next to this Hook
  // e.g. "FriendStatus: Online"
  useDebugValue(isOnline ? "Online" : "Offline");

  return isOnline;
}
```

알쓸신잡이다. 디버그 값을 출력할 때 사용한다. 개인적으로 디버깅 할 때 사용하려고 import 하나를 추가하는 게 싫어서 그냥 console.log하거나 devtool 쓴다.

# Conclusion

사실 커스텀 훅을 제작하는 것 까지 다루고 싶었지만 이미 너무 뇌가 과포화일 것 같아서 실습은 제끼도록 하겠다.

리액트는 훅 이전과 이후로 나뉜다고 해도 과언이 아닐 정도로 많은 것들이 변했고, 기존에 인정받던 디자인 패턴들도 모두 교체되고 있으며 아직도 훅을 멋들어지게 쓰는 법은 정립중이다. 따라서 리액트를 공부하는 사람에게 좋은 정보가 무엇이고 좋다고 누가 하더라도 과연 그런가? 라고 생각해보는 비판적인 사고가 필요하다. 그럼 화이팅!

# Reference
