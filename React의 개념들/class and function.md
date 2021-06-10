# 클래스형과 함수형 차이

---

![클래스형컴포넌트](https://media.vlpt.us/images/sdc337dc/post/249993f6-b71d-46e2-aa54-ebbcfd5fbe41/image.png)

## 1. 클래스형 컴포넌트

> - react 컴포넌트를 선언하는 두가지 방식 중 하나. [클래스 컴포넌트 & 함수 컴포넌트]
> - 현재 자주 사용하지 않지만, 사용하는 기업들이 있다. 그 프로젝트의 유지보수를 위해서는 클래스형 컴포넌트에 대한 개념은 알고 있어야한다.

## 2. 클래스형 컴포넌트와 함수형 컴포넌트 차이

#### 1) 선언 방식

**함수형 컴포넌트**

```
import React from 'react';

function App() {
    const name = 'react';
    return <div className = "react">{name}
    </div>
}

export default App;
```

**클래스형 컴포넌트**

```
import React, { Component } from 'react';

class App extends Component {
    render() {
        const name = 'react';
        return <div className="react">
        {name}
        </div>
    }
}

export default App;
```

클래스 컴포넌트 핵심

- class 키워드 필요

> ![class키워드필요](https://media.vlpt.us/images/sdc337dc/post/ff440a4d-3e7d-4eb3-8095-3a7b88456be0/image.png)

- Component 로 상속을 받아야한다.

> ![Component상속](https://media.vlpt.us/images/sdc337dc/post/f8e93cd2-6148-4c22-a8ee-7bfb1adc3a09/image.png)

- render() 메소드가 반드시 있어야한다.

#### 2) 일반적 차이

**클래스형 :**

- state, lifeCycle 관련 기능사용이 가능하다.
- 메모리 자원을 함수형 컴포넌트보다 조금 더 사용한다.
- 임의 메서드를 정의할 수 있다.

**함수형 :**

- state, lifeCycle 관련 기능사용 불가능 [Hook을 통해 해결됨]
- 메모리 자원을 함수형 컴포넌트보다 덜 사용한다.
- 컴포넌트 선언이 편하다.

함수형이 클래스보다 후에 나왔기 때문에 더 편한 것은 사실이다. 그러나 과거 클래스 컴포넌트를 사용한 프로젝트가 있다. 유지보수를 위해서 알아둘 필요가 있다.

#### 3) 기능 : state 사용차이

**state**

- 컴포넌트 내부에서 바뀔 수 있는 값

**클래스 형**

- constructor 안에서 this.state 초기 값 설정 가능

> ![초기값설정](https://media.vlpt.us/images/sdc337dc/post/3add5950-c02b-4fbc-a27e-806f2c1dfb63/image.png)

- constructor 없이 바로 state 초기값을 설정할 수 있다.

> ![초기값설정2](https://media.vlpt.us/images/sdc337dc/post/1f1ebd5e-aace-45f7-bd65-74f1abc57473/image.png)

- 클래스형 컴포넌트의 state는 **객체 형식**

> ![초기값설정3](https://media.vlpt.us/images/sdc337dc/post/07b7cc0b-0c2d-4df9-9ae7-4c38eae15c8a/image.png)

- this.setState 함수로 state의 값을 변경할 수 있다.

> ![초기값설정4](https://media.vlpt.us/images/sdc337dc/post/0efb1cdf-3e99-44b7-be63-9172cc243889/image.png)

**함수 형**

- 함수형 컴포넌트에서는 useState 함수로 state를 사용한다.
- useState 함수를 호출하면 배열이 반한되는데 첫 번째 원소는 현재 상태
- 두 번째 원소는 상태를 바꾸어 주는 함수

> ![useState](https://media.vlpt.us/images/sdc337dc/post/29321ecc-a578-460b-9423-6910d7ff069a/image.png)

#### 4) props 사용차이

**props**

- 컴포넌트의 속성을 설정 할때 사용하는 요소
- 읽기 전용
- 컴포넌트 자체 props를 수정해서는 안된다.
- 모든 React 컴포넌트는 자신의 props를 다룰 때 반드시 순수 함수처럼 동작해야 한다.
- 수정 되는 것은 state

#### 클래스형 컴포넌트의 props

- this.props를 통해 값을 불러올 수 있다.

> ![this.props](https://media.vlpt.us/images/sdc337dc/post/dd0fc0d4-438a-4d2e-9943-d34fde6fd01d/image.png)

- 부모 객체의 키 값, 자식 props 활용

#### 함수형 컴포넌트의 props

- props를 불러올 필요 없이 바로 호출 할 수 있다.

> ![this.props2](https://media.vlpt.us/images/sdc337dc/post/f9e6559e-ba34-4dd5-bcb7-c0d219e8486d/image.png)
