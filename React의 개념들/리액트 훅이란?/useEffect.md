# Using the Effect Hook

---

Effect Hook을 사용하면 함수 컴포넌트에서 side effect를 수행할 수 있습니다.

```
import React, { useState, useEffect } from 'react';

function Example() {
    const [count, setCount] = useState(0);

    // componentDidMount, componentDidUpdate와 같은 방식으로
    useEffect(() => {
        // 브라우저 API를 이용하여 문서 타이틀을 업데이트합니다.
        document.title = `You clicked ${count} times`;
    });

    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={() => setCount(count + 1)}>
                Click me
            </button>
        </div>
    );
}
```

위의 코드는 [이전 페이지의 카운터 예시](https://ko.reactjs.org/docs/hooks-state.html)를 바탕으로 하지만, 문서의 타이틀을 클릭 횟수가 포함된 문장으로 표현할 수 있도록 새로운 기능을 더했습니다.

데이터 가져오기, 구독(subscription) 설정하기, 수동으로 리액트 컴포넌트의 DOM을 수정하는 것까지 이 모든 것이 side effects입니다. 이런 기능들(operations)을 side effect(혹은 effect)라 부르는 것이 익숙하지 않을 수도 있지만, 아마도 이전에 만들었던 컴포넌트에서 위의 기능들을 구현해보았을 것입니다.

> **팁**
> 리액트의 class 생명주기 메서드에 친숙하다면, useEffect Hook을 componentDidMount 와 componentDidUpdate, componentWillUnmount 가 합쳐진 것으로 생각해도 좋습니다.

리액트 컴포넌트에는 일반적으로 두 종류의 side effects가 있습니다. 정리(clean-up)가 필요한 것과 그렇지 않은 것. 이 둘을 어떻게 구분해야 할지 자세하게 알아봅시다.

---

## 정리(Clean-up)을 이용하지 않는 Effects

**리액트가 DOM을 업데이트한 뒤 추가로 코드를 실행해야 하는 경우가 있습니다.** 네트워크 리퀘스트, DOM 수동 조작, 로깅 등은 정리(clean-up)가 필요 없는 경우들입니다. 이러한 예들은 실행 이후 신경 쓸 것이 없기 때문입니다. class와 hook이 이러한 side effects를 어떻게 다르게 구현하는지 비교해봅시다.

### class를 사용하는 예시

리액트의 class 컴포넌트에서 render 메서드 그 자체는 side effect를 발생시키지 않습니다. 이때는 아직 이른 시기로서 이러한 effect를 수행하는 것은 리액트가 DOM을 업데이트하고 난 이후입니다.

리액트 class에서 side effect를 componentDidMount 와 componentDidUpdate 에 두는 것이 바로 이 때문입니다. 예시로 돌아와서 리액트가 DOM을 바꾸고 난 뒤 문서 타이틀을 업데이트하는 리액트 counter 클래스 컴포넌트를 봅시다.

```
class Example extends React.Component {
    constructor(props) {
        super(props);
        this.state = {
            count: 0
        };
    }


componentDidMount() {
    document.title = `You clicked ${this.state.count} times`;
}

componentDidUpdate() {
    document.title = `You clicked ${this.state.count} times`;
}

render() {
    return (
        <div>
            <p>You clicked {this.state.count} times</p>
            <button onClick={() => this.setState({ count: this.state.count + 1})}>
                Click me
            </button>
        </div>
    );
}
}
```

위 코드에서 class 안의 두 개의 생명주기 메서드에 같은 코드가 중복되는 것에 주의합시다

이는 컴포넌트가 이제 막 마운트된 단계인지 아니면 업데이트되는 것인지에 상관없이 같은 side effect를 수행해야 하기 때문입니다. 개념적으로 렌더링 이후에는 항상 같은 코드가 수행되기를 바라는 것이죠. 하지만 리액트 클래스 컴포넌트는 그러한 메서드를 가지고 있지 않습니다. 함수를 별개의 메서드로 뽑아낸다고 해도 여전히 두 장소에서 함수를 불러내야 합니다.

이제 useEffect Hook에서 같은 기능을 어떻게 구현하는지 보겠습니다.

### Hook을 이용하는 예시

아래의 코드는 위에서 이미 보았던 것이지만 이번에는 좀 더 자세히 살펴보겠습니다.

```
import React, { useState, useEffect } from 'react';

function Example() {
    const [count, setCount] = useState(0);

    useEffect(() => {
        document.title = `You clicked ${count} times`;
    });

    return (
        <div>
            <p>You clicked {count} times</p>
            <button onClick={() => setCount(count + 1)}>
                Click me
            </button>
        </div>
    );
}
```

### useEffect가 하는 일은 무엇일까요?

useEffect Hook을 이용하여 우리는 리액트에게 컴포넌트가 렌더링 이후에 어떤 일을 수행해야하는 지를 말합니다. 리액트는 우리가 넘긴 함수를 기억했다가(이 함수를 'effect'라고 부릅니다) DOM 업데이트를 수행한 이후에 불러낼 것입니다. 위의 경우에는 effect를 통해 문서 타이틀을 지정하지만, 이 외에도 데이터를 가져오거나 다른 명령형(imperative) API를 불러내는 일도 할 수 있습니다.

### useEffect를 컴포넌트 안에서 불러내는 이유는 무엇일까요?

useEffect를 컴포넌트 내부에 둠으로써 effect를 통해 count state 변수(또는 그 어떤 prop에도)에 접근할 수 있게 됩니다. 함수 범위 안에 존재하기 때문에 특별한 API 없이도 값을 얻을 수 있는 것입니다. Hook은 자바스크립트의 클로저를 이용하여 리액트에 한정된 API를 고안하는 것보다 자바스크립트가 이미 가지고 있는 방법을 이용하여 문제를 해결합니다.

### useEffect는 렌더링 이후에 매번 수행되는 걸까요?

네, 기본적으로 첫번째 렌더링과 이후의 모든 업데이트에서 수행됩니다.(나중에 effect를 필요에 맞게 수정하는 방법에 대해 다룰것 입니다.) 마운팅과 업데이트라는 방식으로 생각하는 대신 effect를 렌더링 이후에 발생하는 것으로 생각하는 것이 더 쉬울 것입니다. 리액트는 effect가 수행되는 시점에 이미 DOM이 업데이트되었음을 보장합니다.

### 상세한 설명

effef에 대해 좀 더 알아보았으니 아래의 코드들을 더 쉽게 이해할 수 있을 것입니다.

```
function Example() {
    const [count, setCount] = useState(0);

    useEffect(() => {
        document.title = `You clicked ${count} times`;
    });
}
```

const state 변수를 선언한 뒤 리액트에게 effect를 사용함을 말하고 있습니다.
useEffect Hook에 함수를 전달하고 있는데 이 함수가 바로 effect입니다. 이 effect 내부에서 document.title이라는 브라우저 API를 이용하여 문서 타이틀을 지정합니다. 같은 함수 내부에 있기 때문에 최신의 count를 바로 얻을 수 있습니다. 컴포넌트를 렌더링할 때 리액트는 우리가 이용한 effect를 기억하였다가 DOM을 업데이트한 이후에 실행합니다. 이는 맨 첫 번째 렌더링은 물론 그 이후의 모든 렌더링에 똑같이 적용됩니다.

숙련된 자바스크립트 개발자라면 useEffect에 전달된 함수가 모든 렌더링에서 다르다는 것을 알아챘을지도 모릅니다. 이는 의도된 것으로서, count 값이 제대로 업데이트 되는지에 대한 걱정 없이 effect 내부에서 그 값을 읽을 수 있게 하는 부분이기도 합니다. 리렌더링하는 때마다 모두 이전과 다른 effect로 교체하여 전달합니다. 이 점이 렌더링의 결과의 한 부분이 되게 만드는 점인데, 각각의 effect는 특정한 렌더링에 속합니다.

> **팁**
> componentDidMout 혹은 componentDidUpdate와는 달리 useEffect에서 사용되는 effect는 브라우저가 화면을 업데이트하는 것을 차단하지 않습니다. 이를 통해 애플리케이션의 반응성을 향상해줍니다. 대부분의 effect는 동기적으로 실행될 필요가 없습니다. 흔하지는 않지만 (레이아웃의 측정과 같은) 동기적 실행이 필요한 경우에는 useEffect와 동일한 API를 사용하는 useLayoutEffect라는 별도의 Hook이 존재합니다.

---

## 정리(clean-up)를 이용하는 Effects

위에서 정리(clean-up)가 필요하지 않은 side effect를 보았지만, 정리가 필요한 effect도 있습니다. 외부 데이터에 **구독을 설정해야 하는 경우**를 생각해보겠습니다. 이런 경우에 메모리 누수가 발생하지 않도록 정리하는 것은 매우 중요합니다. class와 Hook을 사용하는 두 경우를 비교해보겠습니다.

### Class를 사용하는 예시

리액트 class에서는 흔히 componentDidMount에 구독을 설정한 뒤 componentWillUnmount에서 이를 정리합니다. 친구의 온라인 상태를 구독할 수 있는 ChatAPI 묘듈의 예를 들어보겠습니다. 다음은 class를 이용하여 상태를 구독하고 보여주는 코드입니다.

```
class FriendStatus extends React.Component {
  constructor(props) {
    super(props);
    this.state = { isOnline: null };
    this.handleStatusChange = this.handleStatusChange.bind(this);
  }

  componentDidMount() {
    ChatAPI.subscribeToFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
  componentWillUnmount() {
    ChatAPI.unsubscribeFromFriendStatus(
      this.props.friend.id,
      this.handleStatusChange
    );
  }
  handleStatusChange(status) {
    this.setState({
      isOnline: status.isOnline
    });
  }

  render() {
    if (this.state.isOnline === null) {
      return 'Loading...';
    }
    return this.state.isOnline ? 'Online' : 'Offline';
  }
}
```

componentDidMount와 componentWillUnmount가 어떻게 대칭을 이루고 있는지를 봅시다. 두 개의 메서드 내에 개념상 똑같은 effect에 대한 코드가 있음에도 불구하고 생명주기 메서드는 이를 분리하게 만듭니다.

### Hook을 이용하는 예시

이제 이 컴포넌트를 Hook을 이용하여 구현해봅시다.

정리의 실행을 위해 별개의 effect가 필요하다고 생각할 수도 있습니다. 하지만 구독의 추가와 제거를 위한 코드는 결합도가 높기 때문에 useEffect는 이를 함께 다루도록 고안되었습니다. effect가 함수를 반환하면 리액트는 그 함수를 정리가 필요한 때에 실행시킬 것입니다.

```
import React, { useState, useEffect } from 'react';

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
    return 'Loading...';
  }
  return isOnline ? 'Online' : 'Offline';
}
```

### effect에서 함수를 반환하는 이유는 무엇일까요?

이는 effect를 위한 추가적인 정리 메커니즘입니다. 모든 effect는 정리를 위한 함수를 반환할 수 있습니다. 이 점이 구독의 추가와 제거를 위한 로직을 가까이 묶어둘 수 있게 합니다. 구독의 추가와 제거가 모두 하나의 effect를 구성하는 것입니다.
