# useState

---

## state 변수 선언하기

클래스를 사용할 때, constructor 안에서 this.state 를 { count: 0 } 로 설정함으로써 count를 0으로 초기화했습니다.

```
class Example extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0
        };
    }
}
```

함수 컴포넌트는 this를 가질 수 없기 때문에 this.state를 할당하거나 읽을 수 없습니다. 대신, useState Hook을 직접 컴포넌트에 호출하니다.

```
import React, { useState } from 'react';

function Example() {
    // 새로운 state 변수를 선언하고, 이것을 count라 부르겠습니다.
    const [count, setCount] = useState(0);
}
```

### useState를 호출하는 것은 무엇을 하는 걸까요?

"state 변수"를 선언할 수 있습니다. 위에 선언한 변수는 count라고 부르지만 banana처럼 아무 이름으로 지어도 됩니다.
useState는 클래스 컴포넌트의 this.state가 제공하는 기능과 똑같습니다. 일반적으로 일반 변수는 함수가 끝날 때 사라지지만, state 변수는 React에 의해 사라지지 않습니다.

### useState의 인자로 무엇을 넘겨주어야 할까요?

useState() Hook의 인자로 넘겨주는 값은 state의 초기 값입니다. 함수 컴포넌트의 state는 클래스와 달리 객체일 필요는 없고, 숫자 타입과 문자 타입을 가질 수 있습니다. 위의 예시는 사용자가 버튼을 얼마나 많이 클릭했는지 알기를 원하므로 0을 해당 state의 초기 값으로 선언했습니다. (2개의 다른 변수를 저장하기를 원한다면 useState()를 두 번 호출해야 합니다.)

### useState는 무엇을 반환할까요?

state 변수, 해당 변수를 갱신할 수 있는 함수 이 두 가지 쌍을 반환합니다. 이것이 바로 const [count, setCount] = useState()라고 쓰는 이유입니다. 클래스 컴포넌트의 this.state.count와 this.setState와 유사합니다. 만약 이러한 문법에 익숙하지 않다면 현재 페이지의 끝에서 살펴볼게요.

이제 useState를 이용하여 많은 것을 만들 수 있습니다.

```
import React, { useState } from 'react';

function Example() {
    // 새로운 state 변수를 선언하고, 이것을 count라 부르겠습니다.
    const [count, setCount] = useState(0);
    }
```

count라고 부르는 state 변수를 선언하고 0으로 초기화합니다. React는 해당 변수를 리렌더링할 때 기억하고, 가장 최근에 갱신된 값을 제공합니다. count 변수의 값을 갱신하려면 setCount를 호출하면 됩니다.

> **주의**
> 왜 createState 가 아닌, useState 로 이름을 지었을까요?
>
> 컴포넌트가 렌더링할 때 오직 한 번만 생성되기 때문에 "Create"라는 이름은 꽤 정확하지 않을 수 있습니다. 컴포넌트가 다음 렌더링을 하는 동안 useState는 현재 state를 줍니다. Hook 이름이 항상 use로 시작하는 이유도 있습니다. Hook의 규칙에서 나중에 살펴보도록 할게요.

## state 가져오기

클래스 컴포넌트는 count를 보여주기 위해 this.state.count를 사용합니다.

`<p>You clicked {this.state.count} times</p>`

반면 함수 컴포넌트는 count를 직접 사용할 수 있습니다.

`<p>You clicked {count} times</p>`

## state 갱신하기

클래스 컴포넌트는 count를 갱신하기 위해 this.setState()를 호출합니다.

```
<button onClick={() => this.setState({ count: this.state.count + 1})}>
    Click me
</button>
```

반면 함수 컴포넌트는 setCount와 count 변수를 가지고 있으므로 this를 호출하지 않아도 됩니다.

```
<button onClick={() => setCount(count + 1)}>
    Click me
</button>
```

## 요약

아래 코드를 한 줄 한 줄 살펴보고, 얼마나 이해했는지 체크해봅시다.

```
1: import React, { useState } from 'react';
2:
3: function Example() {
4:    const [ count, setCount] = useState(0);
5:
6: return (
7:      <div>
8:          <p>You Clicked {count} times</p>
9:          <button onClick={() => setCount(count + 1)}>
10:             Click me
11:         </button>
12:     </div>
13:    );
14:}
```

- **첫 번째 줄**: useState Hook을 React에서 가져옵니다.

- **네 번째 줄**: useState Hook을 이용하면 state 변수와 해당 state를 갱신할 수 있는 함수가 만들어집니다. 또한, useState의 인자의 값으로 0을 넘겨주면 count 값을 0으로 초기화할 수 있습니다.

- **아홉 번째 줄**: 사용자가 버튼 클릭을 하면 setCount 함수를 호출하여 state 변수를 갱신합니다. React는 새로운 count 변수를 Example 컴포넌트에 넘기며 해당 컴포넌트를 리렌더링합니다.

많은 것들이 있기 때문에 처음에는 다소 어려울 수 있습니다. 설명이 이해가 잘 안 된다면, 위의 코드를 천천히 다시 읽어보세요. 클래스 컴포넌트에서 사용하던 state 동작 방식을 잊고, 새로운 눈으로 위의 코드를 보면 분명히 이해가 갈 것입니다.

### 팁: 대괄호가 의미하는 것은 무엇일까요?

대괄호를 이용하여 state 변수를 선언하는 것을 보셨을 겁니다.

```
const [count, setCount] = useState(0);
```

대괄호 왼쪽의 state 변수는 사용하고 싶은 이름으로 선언할 수 있습니다.

```
const [fruit, setFruit] = useState('banana');
```

위 자바스크립트 문법은 "배열 구조 분해"라고 하고, fruit과 setFruit, 총 2개의 값을 만들고 있습니다. 즉, useState를 사용하면 fruit이라는 첫 번째 값과 setFruit라는 두 번째 값을 반환합니다. 아래의 코드와 같은 효과를 낼 수 있습니다.

```
var fruitStateVariable = useState('banana'); // 두 개의 아이템이 있는 쌍을 반환
var fruit = fruitStateVariable[0]; // 첫 번째 아이템
var setFruit = fruitStateVariable[1]; // 두 번째 아이템
```

useState를 이용하여 변수를 선언하면 2개의 아이템 쌍이 들어있는 배열로 만들어집니다. 첫 번째 아이템은 현재 변수를 의미하고, 두 번째 아이템은 해당 변수를 갱신해주는 함수입니다. 배열 구조 분해라는 특별한 방법으로 변수를 선언해주었기 때문에 [0] 이나 [1] 로 배열에 접근하는 것은 좋지 않을 수 있습니다.

> **주의**
> this를 React에 알리지 않았는데, 어떻게 React가 특정 컴포넌트에서 useState를 사용한 것을 아는 지 궁금해할 수 있습니다. 이 질문과 다른 궁금 사항들은 나중에 살펴보겠습니다.

### 팁: 여러 개의 state 변수를 사용하기

[something, setSomething] 의 쌍처럼 state 변수를 선언하는 것은 유용합니다.
왜냐하면 여러 개의 변수를 선언할 때 각각 다른 이름을 줄 수 있기 때문입니다.

```
function ExampleWithManyStates() {
    // 여러 개의 state를 선언할 수 있습니다!
    const [age, setAge] = useState(42);
    const [fruit, setFruit] = useState('banana');
    const [todos. setTodos] = useState([{ text: 'Learn Hooks' }]);
}
```

위의 코드는 age, fruit, todos 라는 지역 변수를 가지며 개별적으로 갱신할 수 있습니다.

```
function handleOrangeClick() {
    // this.setState({ fruit: 'orange' })와 같은 효과를 냅니다.
    setFruit('orange');
}
```

여러 개의 state 변수를 **사용하지 않아도 됩니다.** state 변수는 객체와 배열을 잘 가지고 있을 수 있으므로 서로 연관있는 데이터를 묶을 수 있습니다. 하지만 클래스 컴포넌트의 this.setState와 달리 state를 갱신하는 것은 병합하는 것이 아니라 대체하는 것입니다.
