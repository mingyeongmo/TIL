# React Hook

---

## 등장배경

리액트 컴포넌트는 **클래스형 컴포넌트**(Class Component)와 **함수형 컴포넌트**(Functional Component)로 나뉩니다.
기존의 개발방식은 일반적으로 **함수형 컴포넌트**를 주로 사용하되 **state**이나 **Life Cycle method**를 사용해야 할 때에만 클래스형 컴포넌트를 사용하는 방식이었는데요.

이유는 **클래스형 컴포넌트**가 **함수형 컴포넌트**에 비해 가지는 단점 때문입니다.

1. 코드가 길고 복잡합니다.
   **constructor**, **this**, **binding** 등 지켜야 할 규칙이 많아서 코드가 복잡하고 길어집니다.
   JAVA처럼 OOP(Object-oriented programming)의 테크닉을 수려하게 적용하지는 않으면서도 근본은 클래스의 모습을 띄고 있죠.
   클래스 자체가 **Life Cycle method**로 인해 기본적으로 뚱뚱합니다.

2. Logic의 재사용이 어렵습니다.
   클래스형 컴포넌트에서는 High-Order Components(HOC)로 컴포넌트 자체를 재사용 할 수는 있지만 **부분적인 DOM 관련 처리나 API사용 및 state을 다루는 등의 logic**에 있어서는 경우에 따라 같은 로직을 2개 이상의 Life Cycle method에 중복해서 넣어야하는 등 재사용에 제약이 따릅니다.
   이에 반해 **hooks를 활용한 함수형 컴포넌트에서는 원하는 기능을 함수로 만든후(hook) 필요한 곳에 훅 집어 넣어주기만 하면 되기 때문에** 로직의 재사용이 가능해집니다.

## Hooks

함수형 컴포넌트에 비해 가지는 이러한 단점들에도 불과하고, 그동안 클래스형 컴포넌트를 사용했던 이유는 위에서 언급했듯이 state관리와 Life cycle method의 사용때문이었습니다.
북잡하고 뚱뚱하지만 클래스의 힘을 빌려야만 React가 원활하게 작동할 수 있었던 것이죠.

**그런데 hooks의 등장으로 인해 함수형 컴포넌트에서도 이러한 클래스형 컴포넌트의 작업들을 할 수 있게 되었습니다.**
기존의 클래스형 컴포넌트가 가지고 있던 복잡성, 재사용성의 단점들까지 해결하면서 말이죠.
특히나 OOP를 선호하지 않는 개발자에게 있어서 React를 함수 중심으로 사용할 수 있게 되었다는 것은 매우 환영할만한 일입니다.

예제
두가지 컴포넌트의 차이를 간단한 예제를 통해 살펴보겠습니다.
동일한 기능(카운터)을 클래스형 컴포넌트와 **useState**이라는 **hook**을 사용한 함수형 컴포넌트로 각각 구현해보겠습니다.

#### 클래스형 컴포넌트

먼저 기본적인 화면의 틀입니다.

```
import React from "react";

class App extends React.Component {
    render() {
        return (
            <div style={{ textAlign: "center" }}>
                <div style= {{ fontSize: "100px" }}>0</div>
            </div>
        );
    }
}

export default App;

```

![0](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fbt2ypq%2Fbtqzvo7dC9K%2FaLpFT7ttjHcDG0inHuyFW0%2Fimg.png)

여기에 숫자를 **state**으로 선언하고 클릭하면 숫자를 1씩 증가시키는 버튼과 1씩 감소시키는 버튼을 각각 만들어 보겠습니다.

```
import Recat from "react";

class App extends React.Component {
    state = {
        number: 0
    };
    render() {
        return (
            <div style={{ "textAlign": "center" }}>
                <div style={{ "fontSize": "100px" }}>{this.state.number}</div>
                <button onClick={this.handleClickInncrement}>더하기</button>
                <button onClick={this.handleClickDecrement}>빼기</button>
            </div>
        );
    }
}
handleClickIncrement = () => {
    this.setState(state => ({
        number: state.number + 1
    }));
};
handleClickDecrement = () => {
    this.setState(sstate => ({
            number: state.number -1
        }));
    };
}

export default App;
```

![버튼생성](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FekKEDE%2FbtqzujZ5qBI%2FdvLXOCQMRbGcw8hCkviUhk%2Fimg.png)

숫자를 다루기 위해 컴포넌트에 **state**을 선언하고
더하는 함수 **handleClickIncrement**에서도 **this.setState()** 메소드를,
빼는 함수 **handleClickDecrement**에서도 **this.setState()** 메소드를 사용했습니다.

#### 함수형 컴포넌트 with hooks

이번엔 같은 기능을 리액트 훅의 **useState**을 사용해서 구현해보겠습니다.

```
import React, { useState } from "react";

const App = () => {
    const [number, setNumber] = useState(0);
    return (
        <div style={{ textAlign: "center" }}>
            <div style={{ fontSize: "100px" }}>number</div>
            <button onClick={() => setNumber(number +1)}>더하기</button>
            <button onClick={() => setNumber(number -1)}>빼기</button>
        </div>
    );
};

export default App;
```

코드가 많이 줄어들었습니다.

useState은 클래스형 컴포넌트의 state의 선언과 관리를 짧고 직관적인 코드로 가능하게 해주는 hook입니다.
number라는 state과 setNumber라는 state 변경함수를 useState을 통해 선언하였습니다.
useState에 관한 자세한 설명은 아래 글을 참고해주세요.
[[react] 예제로 따라하는 리액트 훅(hook) - useState](https://velog.io/@dblee/%EA%B9%83%ED%97%88%EB%B8%8CMarkdown-%EC%82%AC%EC%9A%A9%EB%B2%95-%EB%A7%81%ED%81%AC-%EA%B1%B8%EA%B8%B0)

클래스형 컴포넌트에서 setState을 사용하여 satte을 변경할 때에는 shallow copy로 인해 항상 새로운 객체를 생성해서 전달해줘야하는 불편함이 있었는데,
useState을 사용하여 얻은 함수(setNumber)에서는 원하는 값을 인자로 전달하기만하면 됩니다.
state의 선언과 변경과정이 매우 간소화된것이 느껴지나요?

위의 예제에서는 나타나지 않았지만 useState과 같은 hook들을 필요한 곳에 사용함으로써 Logic의 재사용이 가능하게 됩니다.
물론 useState, useEffect, useReducer 등은 React에 내장되어 있는 hook입니다만, 당연히 우리가 원하는 기능을 담당하는 커스텀 hook을 만들어서 재사용할 수도 있습니다.
