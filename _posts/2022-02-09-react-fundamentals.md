---
title: 근본있는 React 개발자가 되고싶다면 알아보자
date: 2021-04-07 12:00:00 +0900
categories: react
---

# 들어가며

이 글은 React에 대한 완전히 기초적인 내용을 다룬다. 문법이나 사용법보다는 리액트가 왜 탄생했고, 어떤 식으로 동작하는지에 대해서 다룬다. 문법적인 내용이나 추천하고 싶은 기술 등은 다른 글을 참조하길 바랍니당.

# 과거의 Web

## HTML CSS JS

이 때부터 웹개발을 한 것은 아니라 많고 디테일한 지식은 없지만, 아직도 통용되는 웹개발의 기본은 HTML과 CSS와 JavaScript이다. 최초의 HTML은 HyperText Markup Language로서, 인터넷에서 문서를 표기하기 위한 수단으로 개발되었다. 그렇기 때문에 웹으로 주로 서비스를 만들고 있는 현재의 웹은 HTML의 핵심적인 패러다임과 맞지 않았고, 이에 덕지덕지 하나씩 추가되었고, CSS도 그 중의 일부이다. CSS이전엔 HTML로만 디자인을 했지만 디자인 정보를 우겨넣다 보니까 아웃풋을 보지 않으면 HTML이 어떤 내용인지 알아볼 수가 없었고 기존의 구조화된 전략과 맞지 않았다. 따라서 W3C에서는 디자인과 HTML을 분리해보자는 아이디어로 CSS가 개발되었다.

JS는 웹 브라우저에서 제공하는 DOM(Document Object Model)에 접근하기 위해 사용되었다. 여기에서 DOM이 무엇인지, 그리고 리액트에서 흔히 말하는 Virtual DOM이 무엇인지 알 필요가 반드시 생긴다.

## DOM

DOM은 대체 무엇이길래 사람들이 중요하다고 강조하고 리액트에서도 가상 DOM을 만든 것 일까? 앞서 말했듯, 웹페이지는 문서이다. HTML은 문서를 웹브라우져에서 표시하기 위해서 개발된 언어이다. DOM의 뜻인 Document Object Model에서 Document가 지칭하는 것도 이 문서이다. 간단하게 DOM은 이런 HTML문서의 요소들을 구조화(Modeling)한 것이다. 는 어려우니, 실제로 웹페이지가 어떻게 만들어지는지 살펴봅니다.

### 웹 페이지는 어떻게 만들어지나

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04c2787f-5369-4178-aeb7-a6751a1c3412/-1.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/04c2787f-5369-4178-aeb7-a6751a1c3412/-1.png)

웹 브라우저가 원본 HTML을 읽어들이고 Style을 입히고 대화형 페이지로 만들어서 뷰포트에 표시하기까지의 과정을 Critical Rendering Path라고 합니다. 이 과정은 실제로 여러 단계로 나누어져 있지만, 두 단계로 줄여보자면 기본적으로

1. 브라우저가 읽어들인 문서를 파싱하여 어떤 내용을 랜더링할 지 결정합니다.
2. 결정된 내용을 랜더링합니다.

조금 더 자세하게 서술하자면, 첫 번째 과정을 거치면 Render Tree가 형성됩니다. 브라우저는 Render Tree를 생성하기위해서 DOM, CSSOM이라고 하는 두 가지 모델이 필요합니다.

- DOM(Document Object Model) – HTML 요소들의 구조화된 표현
- CSSOM(Cascading Style Sheets Object Model) – 요소들과 연관된 스타일 정보의 구조화된 표현

### DOM은 어떻게 표시될까?

DOM은 원본 HTML을 객체 기반으로 표시하는 형식입니다. 예를 들어서

```html
<html lang="en">
  <head>
    <title>Lucky pear study</title>
  </head>
  <body>
    <h1>hello world</h1>
    <p>welcome to luckypear</p>
  </body>
</html>
```

이러한 형태의 HTML문서가 있을 때, DOM은 아래와 같은 노드 트리로 표현됩니다.

```html
html ㄴ head ㄴ title ㄴ Lucky pear study ㄴ body ㄴ h1 ㄴ hello world ㄴ p ㄴ
welcome to luckypear
```

따라서 DOM은 HTML이다 라는 틀린 말이고, HTML로부터 DOM이 생성된다고 보면 됩니다. 가령 JS로 DOM을 수정하거나 형식에 맞지 않는 HTML을 DOM으로 변환하는 과정에서 DOM이 HTML과 다른 데이터를 가지게 됩니다.

이렇게 생성된 DOM의 노드 트리는 CSSOM(비슷한 개념으로 CSS를 모델링한 것)과 결합하여 Render Tree가 됩니다. 따라서 랜더 트리는 DOM과는 다릅니다. 예를 들어서

```html
<html lang="en">
  <head>
    <title>Lucky pear study</title>
  </head>
  <body>
    <h1>hello world</h1>

    <p style="display: none;">welcome to luckypear</p>
  </body>
</html>
```

이런식의 HTML이 있을 때,

```html
HTML ㄴ head ㄴ title ㄴ Lucky pear study ㄴ body ㄴ h1 ㄴ hello world ㄴ p ㄴ
welcome to luckypear
```

DOM 노드 트리는 이렇게 생성되지만, CSSOM과 결합하면서 `display:none`을 수행하며

```html
HTML ㄴ head ㄴ title ㄴ Lucky pear study ㄴ body ㄴ h1 ㄴ hello world
```

랜더 트리에서는 p태그의 내용이 사라지게 됩니다.

이제 DOM이 무엇인지 알겠나용?

## jQuery

DOM에 대해서 신나게 떠들고 있어서 어디를 공부하고있었는지 놓쳤을 수도 있는데, 아직 과거의 웹 기술이 어땠는지 배우고 있었습니다. 과거의 Web기술을 얘기할 때 빼놓을 수 없는게 바로 `jQuery`입니다.

2006년에 "write less, do more"라는 모토를 들고 발표된 이 기술은 다음과 같은 특징이 있습니다.

- 웹페이지 상에서 엘리먼트(Element)를 쉽게 찾고 조작할 수 있습니다.
- 거의 모든 웹브라우저에 대응할 정도로 호환성이 매우 뛰어납니다.
- 네트워크, 애니메이션 등 다양한 기능을 제공합니다.
- 메소드 체이닝(Method chaining) 등 짧고 유지관리가 용이한 코드 작성을 지원합니다.
- 관련 플러그인들이 웹상에 공개되어 있으며 플러그인을 직접 구현하거나 확장할 수 있습니다.
- 공식 웹사이트(www.jquery.com)와 수많은 레퍼런스를 통해 쉽게 접근 가능합니다.

핵심적인 내용을 말하자면, jQuery는 DOM을 조작할 수 있는 아주 효과적인 수단을 (아주 많이)제공합니다. 이에 따라 웹 개발에 빼놓을 수 없는 존재로 군림했습니다.

더 자세히 알고 가기엔 끝도 없지만, 짧게 jQuery가 효과적이었던 이유를 말하자면 HTML이 단순 문서가 아닌 서비스를 만드는 수단으로 변모하면서, js로 DOM을 조작하는 방식이 많이 늘어났고, 이는 굉장히 불편했습니다. 이 때 jQuery가 모토답게 적은 코드로 쉽게 개발을 할 수 있게 도와주었고 아주 효과적으로 자리잡은 것입니다.

# React의 등장

<aside>
🔥 느슨해진 힙합씬에 긴장을 주듯이 jQuery로 도배된 웹씬에 Virtual DOM이라는 것을 들고 React가 등장합니다.

</aside>

모두 알다시피, React는 Facebook이 만든 Javascript 라이브러리입니다. 그리고 순식간에 웹 개발 생태계를 점령해가고 있고, 이것이 여러분들이 리액트를 배우고 싶어하는 이유입니다.

그렇다면 어떤 것 때문에 리액트가 생태계를 쳐묵했을까요?

## Component

리액트로 개발하다보면 컴포넌트라는 말이 지속적으로 등장합니다. 리액트에서의 컴포넌트란 [SOLID](<https://ko.wikipedia.org/wiki/SOLID_(%EA%B0%9D%EC%B2%B4_%EC%A7%80%ED%96%A5_%EC%84%A4%EA%B3%84)>)한 독립적인 블록입니다. 마치 레고를 조립해서 자동차를 만들듯이 각 컴포넌트가 레고의 부품이 됩니다.

이런 블록 단위의 컴포넌트를 이용한 개발은 많은 장점이 있지만 대표적으로 유지보수가 쉬워집니다. 컴포넌트가 하나의 기능을 수행하기 때문에, 문제가 생긴 기능을 수정해주면 작은 단위의 수정이 전체를 매끄럽게 해줍니다.

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa5af3e6-d3fd-4e6c-8cd5-e2b4e10a2de2/Screen_Shot_2021-01-17_at_12.38.16_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/fa5af3e6-d3fd-4e6c-8cd5-e2b4e10a2de2/Screen_Shot_2021-01-17_at_12.38.16_PM.png)

![https://s3-us-west-2.amazonaws.com/secure.notion-static.com/406da23d-b909-4ae7-8d8e-41f1bf558968/Screen_Shot_2021-01-17_at_12.38.21_PM.png](https://s3-us-west-2.amazonaws.com/secure.notion-static.com/406da23d-b909-4ae7-8d8e-41f1bf558968/Screen_Shot_2021-01-17_at_12.38.21_PM.png)

## JSX

JSX는 React를 위해 만들어진 자바스크립트 문법으로, 사용해도 안해도 상관은 없지만 JSX를 사용하지 않으면 리액트의 많은 것을 포기하게 됩니다.

```jsx
class HelloMessage extends React.Component {
  render() {
    return React.createElement("div", null, "Hello ", this.props.name);
  }
}

ReactDOM.render(React.createElement(HelloMessage, { name: "Paul" }), mountNode);
```

위는 입력한 이름을 표시해주는 컴포넌트의 코드입니다. JSX를 사용하지 않으면 자바스크립트로 이런식으로(과거의 jQuery로 했던 것 처럼) 구현하게 됩니다.

하지만 JSX를 사용하게된다면

```jsx
class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}
ReactDOM.render(<HelloMessage name="Paul" />, mountNode);
```

이런식으로 코드를 간소화 할 수 있고, 단순히 코드가 간소화해지는 것 뿐만아니라 코드가 어떤 구조로 구성되어 있는 지 훨씬 수월하게 해석할 수 있습니다.

그리고 위의 jsx코드는 어차피 Babel이라고 하는 플러그인으로 트랜스파일되어 상위의 js코드처럼 바뀌게 됩니다.

## Virtual DOM

리액트의 가장 화끈한 특징입니다. 앞서 DOM에 대해서 설명으로 가상 DOM이 무엇인지 살짝 감이 올 수 있을 것 같습니다.

### DOM의 문제점

앞에서 DOM에 대해서 어떤 것인지 설명은 했지만 왜 Virtual DOM이 생겨야 할까? 단점이 무엇일까에 대해선 살펴보지 않았습니다. 여기서 다루려고 미뤄뒀기 때문입니다.

문제는 바로 **DOM을 효과적으로 다루는 것이 어렵다**는 것이었습니다. DOM의 성능이 좋지 않다 등등의 말이 나오는 이유는 사실 DOM 자체의 기능보다는 해석 과정에 문제가 있는 경우가 대부분입니다. 브라우저별로 DOM을 가지고 화면을 만드는 방식 또한 다르기 때문에 브라우저별로 동일한 화면을 그려내지도 못합니다.

그래서 앞서 말한 **jQuery**가 등장했습니다...만 jQuery에도 문제가 있었습니다.

### jQuery 너마저

jQuery는 DOM을 다루게 도와주는 멋진 도구였습니다. 하지만 DOM의 구조를 개선하며 사용할 수 있는 도구가 아니라 흔히 하는 비유로 **커터칼**같은 도구였습니다.

jQuery는 DOM을 말 그대로 잘라내서 사용합니다. 과거에 이것은 인기가 높았지만 프론트엔드의 전문성이 증가하고 대형 어플리케이션이 개발되면서 유지보수가 어려워지며 jQuery의 한계점이 드러납니다.

그리고 아래의 링크에서 볼 수 있듯이, 나중에 개발자들은 이 꼴이 됩니다.

[Needs More jQuery](http://needsmorejquery.com/)

### Virtual DOM

이에 등장한 Virtual DOM은 DOM에 대한 아예 새로운 접근을 제시합니다.

```jsx
class HelloMessage extends React.Component {
  render() {
    return <div>Hello {this.props.name}</div>;
  }
}

ReactDOM.render(<HelloMessage name="Paul" />, mountNode);
```

앞서 살펴봤던 JSX코드를 다시 가져왔습니다.

여기서 `name`이 Peter로 바뀐다면 어떻게 될까요? 뭐 당연한 말이지만 Hello Paul이 Hello Peter로 바뀌겠죠. 하지만 우리가 다룰 것은 실제로 어떤 일이 일어날까? 하는 것입니다.

리액트에는 **상태값과 속성값**이라고 불리는 존재가 있습니다. 여기에서 속성값은 this.props가 참조하고 있는 name이 됩니다. 리액트는 상태값이나 속상값에 변화가 일어나면 이를 감지하여 해당 컴포넌트의 render()를 다시 수행합니다. ~~그래서 이름이 React인 것 입니다.~~ 그리고 render함수가 실행되면서 리턴된 값은 바로 DOM에 반영되지 않습니다(랜더링 되지 않는다는 뜻입니다). 이 때 Virtual DOM이 일을 하는데, render함수의 리턴값으로 React는 새로운 가상의 DOM을 만들어냅니다. 그리고 브라우저의 DOM과 가상 DOM을 비교하면서 바뀐 부분만 DOM에 적용합니다. 그러고나서 브라우저가 DOM을 해석해서 새로운 화면을 랜더링합니다.

앞선 예시로 다시 풀자면,

1. `HelloMessage`에서 Hello Paul을 render에서 return하여 랜더링 중
2. props의 name이 Peter로 변경
3. 속성값이 변경되었기 때문에 render함수 재호출
4. `HelloMessage`에서 Hello Peter를 return
5. Virtual DOM에서 변경된 내용(this.props.name)을 감지, 해당 <div>안의 내용을 DOM에서 수정
6. 브라우저에서 변경 값을 감지하고 새로운 화면 랜더링

근데 뭔가 이렇게 동작하는건 이제 알 것 같은데, 왜 굳이 가상 DOM을 만들까? 그냥 비교하지 않고 바로 DOM을 업데이트하는게 더 빠르지 않을까 하는 의문이 듭니다.

그것은 간단하게 브라우저에서 **DOM을 업데이트 하는 비용이 비싸기 때문**에 가상 DOM에서 미리 최적화하고, 컴포넌트 단위로 묶어서 관리해주는 것입니다.

[DOM 업데이트 비용은 왜 비싼가?](https://velopert.com/3236)

## React Native

리액트 네이티브는 리액트보다 조금 나중에 등장했지만 역시 각광받았습니다. 웹 개발이랑은 크게 관련이 없지만, 리액트가 사랑받은 이유 중 큰 부분을 차지하고 있기 때문에 짧게 추가하겠습니다.

리액트의 사용방식을 그대로 모바일 환경에서 사용할 수 있다는 점은 큰 메리트가 되었고, "Learn once, write everywhere"라는 리액트의 모토를 탄생시켰습니다. 아무튼 요지는 한 번 배워서 여러 곳에 써먹을 수 있어서 리액트 인기가 높아졌다- 입니다.

# React는 어떻게 동작하는가

## React.createElement

앞서 말한 JSX를 사용하게되면 React.createElement의 사용 없이 Babel에 의해 파싱되고 트랜스파일링된다. 따라서 모르고 넘어갈 수도 있지만, 내 코드가 실제로 무슨 일을 하는 지는 알아둬야 하기에 설명하도록 합니다.

> _React 엘리먼트는 호스트 객체를 그릴 수 있는 일반적인 자바스크립트 객체입니다.
> React 엘리먼트는 가볍고 호스트 객체에 직접적으로 관여하지 않습니다. 단지 화면에 무엇을 그리고 싶은지에 대한 정보가 들어 있을 뿐입니다._

리액트는 `React.createElement(type, [props], [...children])` 라는 API를 통해 컴포넌트를 생성한다.(type에는 React Element, HTML tag, React Fragment 중에 하나가 올 수 있다)

`createElement`는 실행하게 되면 `ReactElement`라고 하는 객체를 생성한다. `ReactElement`는 다음과 같은 속성을 가진다

```jsx
const ReactElement = function (type, key, ref, self, source, owner, props) {
  const element = {
    $$typeof: REACT_ELEMENT_TYPE,

    type: type,
    key: key,
    ref: ref,
    props: props,

    _owner: owner,
  };

  return element;
};
```

위의 내용을 자세하게 뜯어볼 필요는 없고, 요지는 `React.createElement`를 하게되면 Element에 대한 정보를 가지는 Object를 생성해준다.

결과적으로 이렇게 생성된 Object를 In-Memory에 저장하고 `ReactDOM.render` 함수를 통해 Web API(`document.createElement`)를 이용해 실제 브라우저에 그려지는 것이다.

## React.Component

앞서 말한 Element들을 모아서 모듈화한 것이 Component라고 볼 수 있다.

```jsx
function Component(props, context, updater) {
  this.props = props;
  this.context = context;
  // If a component has string refs, we will assign a different object later.
  this.refs = emptyObject;
  // We initialize the default updater but the real one gets injected by the
  // renderer.
  this.updater = updater || ReactNoopUpdateQueue;
}
```

`React`의 [내부 코드](https://github.com/facebook/react/blob/9198a5cec0936a21a5ba194a22fcbac03eba5d1d/packages/react/src/ReactBaseClasses.js)에서 `Component`는 위와 같이 기술된다. 위의 코드를 보고 알 수 있을 지 모르지만, 컴포넌트는 props와 context를 받고, 자체적으로 state를 가지고있는 자바스크립트 객체이다.

```jsx
Component.prototype.setState = function (partialState, callback) {
  invariant(
    typeof partialState === "object" ||
      typeof partialState === "function" ||
      partialState == null,
    "setState(...): takes an object of state variables to update or a " +
      "function which returns an object of state variables."
  );
  this.updater.enqueueSetState(this, partialState, callback, "setState");
};
```

컴포넌트는 setState를 다음과 같이 정의하는데, Component의 prototype으로 구현되어있다.

코드를 해석하자면, updater안에 `state`를 업데이트 하는 `Queue` 구조의 `setState` 이벤트 큐가 있고 이 `state`를 변경하는 이벤트 큐에 변경된 `state`를 넣어 업데이트가 이루어진다고 볼 수 있다.

<aside>
⚠️ **Function Component**의 코드를 뒤져본 결과, 기본적으로 Class Component와 같이 위의 코드를 참조하며 사용되는 것 같았다

</aside>

## ReactDOM.render

CRA(Create React App)를 사용해서 앱을 만들게되면 index.js에 해당 코드가 생성된다

```jsx
ReactDOM.render(
  <React.StrictMode>
    <App />
  </React.StrictMode>,
  document.getElementById("root")
);
```

우리는 이 render의 내부를 뜯어보겠다.

```jsx
export function render(
  element: React$Element<any>,
  container: Container,
  callback: ?Function
) {
  invariant(
    isValidContainer(container),
    "Target container is not a DOM element."
  );

  return legacyRenderSubtreeIntoContainer(
    null,
    element,
    container,
    false,
    callback
  );
}
```

여기서 볼 수 있는 것은 `ReactElement`와 `container`(Dom Element)를 받아서 container의 서브트리로 엘리먼트를 render한다는 것을 확인할 수 있다.

따라서 위의 코드는 `'root'` id를 가진 컨테이너의 서브트리에 `<App/>`이라는 리액트 앨리먼트를 추가하는 것이다.

```jsx
function legacyRenderSubtreeIntoContainer(
  parentComponent: ?React$Component<any, any>,
  children: ReactNodeList,
  container: Container,
  forceHydrate: boolean,
  callback: ?Function
) {
  // TODO: Without `any` type, Flow says "Property cannot be accessed on any
  // member of intersection type." Whyyyyyy.
  let root: RootType = (container._reactRootContainer: any);
  let fiberRoot;
  if (!root) {
    // Initial mount
    root = container._reactRootContainer = legacyCreateRootFromDOMContainer(
      container,
      forceHydrate
    );
    fiberRoot = root._internalRoot;
    if (typeof callback === "function") {
      const originalCallback = callback;
      callback = function () {
        const instance = getPublicRootInstance(fiberRoot);
        originalCallback.call(instance);
      };
    }
    // Initial mount should not be batched.
    unbatchedUpdates(() => {
      updateContainer(children, fiberRoot, parentComponent, callback);
    });
  } else {
    fiberRoot = root._internalRoot;
    if (typeof callback === "function") {
      const originalCallback = callback;
      callback = function () {
        const instance = getPublicRootInstance(fiberRoot);
        originalCallback.call(instance);
      };
    }
    // Update
    updateContainer(children, fiberRoot, parentComponent, callback);
  }
  return getPublicRootInstance(fiberRoot);
}
```

컨테이너의 서브트리에 리액트 앨리먼트를 추가하는 코드이다.

좀 많이 복잡하지만 요약하자면

1. root 컨테이너가 없는 경우

   root 컨테이너가 없다는 것은 리액트가 최초로 랜더링 되는 시점이라는 의미이다. Dom Element에 React Element를 등록하고 초기의 랜더링은 [batch](https://ko.wikipedia.org/wiki/%EC%9D%BC%EA%B4%84_%EC%B2%98%EB%A6%AC)를 하지 않고 랜더링을 진행한다.

2. root 컨테이너가 있는 경우

   React element가 등록된 후에는 배치 처리를 통해 리액트의 자식 노드를 업데이트한다.

이제 이 `updateContainer`가 어떤 일을 해주는지 알아보러 가자

```jsx
export function updateContainer(
  element: ReactNodeList,
  container: OpaqueRoot,
  parentComponent: ?React$Component<any, any>,
  callback: ?Function
): Lane {
  const current = container.current;
  const eventTime = requestEventTime();
  const lane = requestUpdateLane(current);

  if (enableSchedulingProfiler) {
    markRenderScheduled(lane);
  }

  const context = getContextForSubtree(parentComponent);
  if (container.context === null) {
    container.context = context;
  } else {
    container.pendingContext = context;
  }

  const update = createUpdate(eventTime, lane);
  // Caution: React DevTools currently depends on this property
  // being called "element".
  update.payload = { element };

  callback = callback === undefined ? null : callback;
  if (callback !== null) {
    update.callback = callback;
  }

  enqueueUpdate(current, update);
  scheduleUpdateOnFiber(current, lane, eventTime);

  return lane;
}
```

위의 코드도 복잡하니 요약하자면

1. 현재의 데이터로 lane을 가져온다. lane은 현재 컨테이너의 업데이트를 처리해주는 이벤트 루프의 컨텍스트이다.
2. 상위 컴포넌트의 context를 가져온다
3. 현재의 컨텍스트가 없는 경우, 상위 컴포넌트의 context를 물려받는다.
4. 새로운 업데이트에 대한 정보를 생성한다
5. 업데이트의 payload에 element 정보들을 입력한다.
6. 업데이트 queue에 현재의 데이터를 입력하여 batch등록을 예약한다.

<aside>
⚠️ `React` 내부의 업데이트 큐는 **16ms**마다 window.requestIdleCallback()을 통해서 실행됩니다. window.requestIdleCallback() API가 없으면 내부적으로 구현한 로직으로 스케줄 작업을 합니다.

</aside>

조금 길게 `ReactDOM.render`에 대해서 살펴봤는데, 이렇게 길게 본 이유는 리액트와 Virtual DOM의 특성을 잘 알 수 있기 때문이다

1. 컨테이너의 DOM Element 안에 자체적인 가상 DOM tree를 만든다
2. 컴포넌트에 변경사항이 생기면, updateQueue에 등록한다
3. 해당 변경사항이 배치를 통해 실행된다

## React의 update

React의 update는 다음 3가지 과정을 반복한다.

1. Element를 DOM에 추가한다.
2. Element의 children(자식노드)을 추가하기 위한 Fiber(작업)를 생성한다.
3. 다음으로 진행되어야 할 작업을 선택한다.

여기서 다음으로 진행되어야 할 작업은 자식 노드가 있으면 이를 먼저 수행하고, 그 다음에 형제 노드로 다음 작업을 지정한다.

이 과정을 진행중에 root로 돌아오게 된다면, React의 render과정이 모두 끝난 것으로 판단한다.

### requestUpdateLane / enqueueUpdate

[updateContainer](https://www.notion.so/React-20db630f8e0847b6975a22f4403dfa0a) 에서 `requestUpdateLane`과 `enqueueUpdate`에 대한 내용을 얼추 생략했는데,

`requestUpdateLane`은 React의 update에 대한 우선순위를 설정해주는 함수이다.

`enqueueUpdate`는 리액트 업데이트 큐에 추가하는 함수인데, 업데이트 큐는 우선 순위가 지정된 업데이트의 Linked List이며 Current Queue, Work-In-Progress Queue의 이중 버퍼 구조를 가진다. 이중 버퍼를 가지는 이유는 업데이트 도중 랜더링이 discard된 경우에도 업데이트를 유지하기 위함이다.

### scheduleUpdateOnFiber

`scheduleUpdateOnFiber` 는 `enqueueUpdate` 에서 추가한 업데이트를 스케쥴러에 반영하는 기능을 한다. 업데이트 실무자 정도로 보면 되겠다

```jsx
export function scheduleUpdateOnFiber(
  fiber: Fiber,
  lane: Lane,
  eventTime: number
): FiberRoot | null {
  checkForNestedUpdates();
  warnAboutRenderPhaseUpdatesInDEV(fiber);

  const root = markUpdateLaneFromFiberToRoot(fiber, lane);
  if (root === null) {
    warnAboutUpdateOnUnmountedFiberInDEV(fiber);
    return null;
  }
  // Mark that the root has a pending update.
  markRootUpdated(root, lane, eventTime);

  if (enableProfilerTimer && enableProfilerNestedUpdateScheduledHook) {
    if (
      (executionContext & CommitContext) !== NoContext &&
      root === rootCommittingMutationOrLayoutEffects
    ) {
      if (fiber.mode & ProfileMode) {
        let current = fiber;
        while (current !== null) {
          if (current.tag === Profiler) {
            const { id, onNestedUpdateScheduled } = current.memoizedProps;
            if (typeof onNestedUpdateScheduled === "function") {
              if (enableSchedulerTracing) {
                onNestedUpdateScheduled(id, root.memoizedInteractions);
              } else {
                onNestedUpdateScheduled(id);
              }
            }
          }
          current = current.return;
        }
      }
    }
  }

  if (root === workInProgressRoot) {
    // Received an update to a tree that's in the middle of rendering. Mark
    // that there was an interleaved update work on this root. Unless the
    // `deferRenderPhaseUpdateToNextBatch` flag is off and this is a render
    // phase update. In that case, we don't treat render phase updates as if
    // they were interleaved, for backwards compat reasons.
    if (
      deferRenderPhaseUpdateToNextBatch ||
      (executionContext & RenderContext) === NoContext
    ) {
      workInProgressRootUpdatedLanes = mergeLanes(
        workInProgressRootUpdatedLanes,
        lane
      );
    }
    if (workInProgressRootExitStatus === RootSuspendedWithDelay) {
      // The root already suspended with a delay, which means this render
      // definitely won't finish. Since we have a new update, let's mark it as
      // suspended now, right before marking the incoming update. This has the
      // effect of interrupting the current render and switching to the update.
      // TODO: Make sure this doesn't override pings that happen while we've
      // already started rendering.
      markRootSuspended(root, workInProgressRootRenderLanes);
    }
  }
  // TODO: requestUpdateLanePriority also reads the priority. Pass the
  // priority as an argument to that function and this one.
  const priorityLevel = getCurrentPriorityLevel();

  if (lane === SyncLane) {
    if (
      // Check if we're inside unbatchedUpdates
      (executionContext & LegacyUnbatchedContext) !== NoContext &&
      // Check if we're not already rendering
      (executionContext & (RenderContext | CommitContext)) === NoContext
    ) {
      // Register pending interactions on the root to avoid losing traced interaction data.
      schedulePendingInteractions(root, lane);

      // This is a legacy edge case. The initial mount of a ReactDOM.render-ed
      // root inside of batchedUpdates should be synchronous, but layout updates
      // should be deferred until the end of the batch.
      performSyncWorkOnRoot(root);
    } else {
      ensureRootIsScheduled(root, eventTime);
      schedulePendingInteractions(root, lane);
      if (executionContext === NoContext) {
        // Flush the synchronous work now, unless we're already working or inside
        // a batch. This is intentionally inside scheduleUpdateOnFiber instead of
        // scheduleCallbackForFiber to preserve the ability to schedule a callback
        // without immediately flushing it. We only do this for user-initiated
        // updates, to preserve historical behavior of legacy mode.
        resetRenderTimer();
        flushSyncCallbackQueue();
      }
    }
  } else {
    // Schedule a discrete update but only if it's not Sync.
    if (
      (executionContext & DiscreteEventContext) !== NoContext &&
      // Only updates at user-blocking priority or greater are considered
      // discrete, even inside a discrete event.
      (priorityLevel === UserBlockingSchedulerPriority ||
        priorityLevel === ImmediateSchedulerPriority)
    ) {
      // This is the result of a discrete event. Track the lowest priority
      // discrete update per root so we can flush them early, if needed.
      if (rootsWithPendingDiscreteUpdates === null) {
        rootsWithPendingDiscreteUpdates = new Set([root]);
      } else {
        rootsWithPendingDiscreteUpdates.add(root);
      }
    }
    // Schedule other updates after in case the callback is sync.
    ensureRootIsScheduled(root, eventTime);
    schedulePendingInteractions(root, lane);
  }
  // We use this when assigning a lane for a transition inside
  // `requestUpdateLane`. We assume it's the same as the root being updated,
  // since in the common case of a single root app it probably is. If it's not
  // the same root, then it's not a huge deal, we just might batch more stuff
  // together more than necessary.
  mostRecentlyUpdatedRoot = root;

  return root;
}
```

코드가 좀 긴데, 참고용으로 삽입해보았다. 결국 업데이트를 진행하는 것으로 생각하면 된다.

# 이제 다시 React

## Class Component와 Function Component

<aside>
⚠️ Functional(함수형) 컴포넌트라고 하는 말이 있는 데, 리액트에서는 Function Component가 맞는 말이라고 권고하고 있다.

</aside>

이 부분은 [다른 글](https://www.notion.so/React-hooks-df1bae39aacc4029a7c7ad319f17c927)에서 다뤘기 때문에 짧게 짚고 넘어가겠다. 리액트에는 클래스 컴포넌트와 함수 컴포넌트의 두 가지 컴포넌트 선언 방식이 있다. 클래스 컴포넌트보다는 함수 컴포넌트를 사용하는 것이 최신 개발 트렌드에 더 알맞고 간결하다는 판단으로 최근엔 함수 컴포넌트를 사용하는 추세이다.

## State

React에서의 state(상태값)은 무엇일까?

사실 이 것 때문에 위에 update에 관한 내용을 주구장창 다뤘다. `state`가 변하면 update가 일어나기 때문이다.

```jsx
// extreamly simplified implementation
function render(Component) {
  // draw component
}
class Component {
  constructor(props) {
    this.props = props;
  }
  setState(state) {
    this.state = state;

    render();
  }
}
OverReact = {
  render,
  Component,
};
```

`Class Component`를 뜯어보면 대략 이런 형태로 진행이 된다고 할 수 있다.

클래스 컴포넌트에서 setState가 일어나게 되면 render함수를 호출하게 되고, 이에 다시 랜더링이 되어 결과값이 Virtual DOM 에서 처리되게 되는 것이다.

`react-hook`이 나오기 전까지는 함수 컴포넌트에서 render 함수를 호출할 수 있는 방법이 없었고, 함수 컴포넌트가 단순히 presentation component로 props를 받아서 UI를 그리는 정도로 사용되었다.

`react-hook`의 useState를 사용하면 위의 setState와 비슷한 형태로 랜더링이 일어난다고 할 수 있다.

## Props

props는 속성값으로, 컴포넌트가 스스로 가지고 태어난 것이 아닌 상위 level에서 정해주는 속성들의 값이다. 속성값이 변경될 때도 역시 컴포넌트에서 re-render가 일어납니다. props는 React에서 `immutable` 한 값으로, 변경이 불가능하다.

함수 컴포넌트와 클래스 컴포넌트는 각각 동작 방식이 다른데, 아래에서 좀 더 자세히 살펴보기로 하자.

## 다시 Class Component와 Function Component

<aside>
⚠️ 해당 부분은 Overreacted를 약간 각색하여 사용됩니다.

</aside>

```jsx
function ProfilePage(props) {
  const showMessage = () => {
    alert("Followed " + props.user);
  };

  const handleClick = () => {
    setTimeout(showMessage, 3000);
  };

  return <button onClick={handleClick}>Follow</button>;
}
```

```jsx
class ProfilePage extends React.Component {
  showMessage = () => {
    alert("Followed " + this.props.user);
  };

  handleClick = () => {
    setTimeout(this.showMessage, 3000);
  };

  render() {
    return <button onClick={this.handleClick}>Follow</button>;
  }
}
```

위의 두 코드는 각각 함수, 클래스로 구현된 컴포넌트이다. 클릭을 누르면 props로 전달된 유저를 팔로우 했다는 메시지를 3초 뒤에 출력한다.

완전히 같은 코드로 보이지만, 두 코드는 미묘하게 다른 것을 [데모](https://codesandbox.io/s/pjqnl16lm7)로 확인할 수 있다

1. Follow버튼을 누르고**,**
2. 3초가 지나기 전에 선택된 프로필을 바꿔라**.**
3. 알림창의 글을 읽어보자**.**

여기서 클래스 컴포넌트는 명백한 버그를 불러일으킨다고 볼 수 있는데, 왜 그런 지 알아보겠습니다.

```jsx
showMessage = () => {
  alert("Followed " + this.props.user);
};
```

여기서 `props`는 앞에서 살짝 언급했듯이, **변경할 수 없는 값**이지만 `this`는 **변경이 가능**하다.

리액트가 시간이 지나면서 this를 변경하기 때문에 render에서 업데이트된 값을 읽어오는 것이다. 따라서 요청이 진행되고 있는 도중에 클래스 컴포넌트가 다시 랜더링 된다면 `this.props` 또한 바뀌게 된다. 그것이 user가 새로운 props로 바뀌게 되는 이유이다.

반대로 함수 컴포넌트에서는 함수가 받는 인자를 React는 변경할 수 없다.

만약에 부모 컴포넌트에서 다른 props를 이용해 ProfilePage를 또 다시 랜더링 하게 되면 리액트는 ProfilePage를 새로 호출한다. 그래도 이전에 클릭했던 버튼의 이벤트 핸들러는 이전 render에 종속돼있기 때문에 이전의 user값들을 사용하게 된다.

이것은 함수 컴포넌트와 클래스 컴포넌트의 아주 큰 차이를 얘기할 수 있다.

> _함수형 컴포넌트는 render 될 때의 값들을 유지한다._

## React hooks

자세한 내용은 [다른 글](https://www.notion.so/React-hooks-df1bae39aacc4029a7c7ad319f17c927)에서 참고하도록 하자. 아무튼 react의 hook이 등장하여 함수 컴포넌트를 더 잘 쓸 수 있게 되었고, 개발 방식이 변경되었다.

# 급 마무리

사실 이제 뭘 더 짚어야 될 지 난감해서 급 마무리를 해본당.

이 정도 개념만 머리에 잘 넣고 있다면 최소한 이런 간단한 원칙들이 모여서 리액트가 되고, 내 코드가 리액트 위에서 이렇게 돌아가기 때문에 특정한 안티패턴을 누가 가르쳐주는 일 없이도 몸소 예방할 수 있다.

그럼 Good Luck.

~~근데 시작에 근본있는 React 개발자가 되고싶다면 알아보자고 했는데 사실 js가 근본이 없는 언어라 근본이 생기기는 당분간 어려울 것으로 보인다~~

# Reference

[1] [https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/소개](https://developer.mozilla.org/ko/docs/Web/API/Document_Object_Model/%EC%86%8C%EA%B0%9C)

[2] [https://wit.nts-corp.com/2019/02/14/5522](https://wit.nts-corp.com/2019/02/14/5522)

[3] [https://www.samsungsds.com/kr/insights/jQuery.html](https://www.samsungsds.com/kr/insights/jQuery.html)

[4] [https://medium.com/@RianCommunity/react의-탄생배경과-특징-4190d47a28f](https://medium.com/@RianCommunity/react%EC%9D%98-%ED%83%84%EC%83%9D%EB%B0%B0%EA%B2%BD%EA%B3%BC-%ED%8A%B9%EC%A7%95-4190d47a28f)

[5] [https://medium.com/react-native-seoul/react-리액트를-처음부터-배워보자-02-react-createelement와-react-component-그리고-reactdom-render의-동작-원리-41bf8c6d3764](https://medium.com/react-native-seoul/react-%EB%A6%AC%EC%95%A1%ED%8A%B8%EB%A5%BC-%EC%B2%98%EC%9D%8C%EB%B6%80%ED%84%B0-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-02-react-createelement%EC%99%80-react-component-%EA%B7%B8%EB%A6%AC%EA%B3%A0-reactdom-render%EC%9D%98-%EB%8F%99%EC%9E%91-%EC%9B%90%EB%A6%AC-41bf8c6d3764)

[6] [https://medium.com/react-native-seoul/react-리액트를-처음부터-배워보자-03-react-의-reconciliation-과정-2e6fb59c0c2d](https://medium.com/react-native-seoul/react-%EB%A6%AC%EC%95%A1%ED%8A%B8%EB%A5%BC-%EC%B2%98%EC%9D%8C%EB%B6%80%ED%84%B0-%EB%B0%B0%EC%9B%8C%EB%B3%B4%EC%9E%90-03-react-%EC%9D%98-reconciliation-%EA%B3%BC%EC%A0%95-2e6fb59c0c2d)

[7] [https://www.awesomezero.com/development/reacthook/](https://www.awesomezero.com/development/reacthook/)

[8] [https://overreacted.io/ko/how-are-function-components-different-from-classes/](https://overreacted.io/ko/how-are-function-components-different-from-classes/)
