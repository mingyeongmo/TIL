# Side Effect란?

---

React 컴포넌트가 화면에 렌더링된 이후에 비동기로 처리되어야 하는 부수적인 효과들을 흔히 Side Effect라고 일컽습니다. 대표적인 예로 어떤 데이터를 가져오기 위해서 외부 API를 호출하는 경우, 일단 화면에 렌더링할 수 있는 것은 먼저 렌더링하고 실제 데이터는 비동기로 가져오는 것이 권장됩니다. 요청 즉시 1차 렌더링을 함으로써 연동하는 API가 응답이 늦어지거나 응답이 없을 경우에도 영향을 최소화 시킬 수 있어서 사용자 경험 측면에서 유리하기 때문입니다.

## componentDidXxx() - 클래스 기반 Side Effect

React Hooks가 나오기 전에는 클래스 컴포넌트의 `componentDidMount()` 나 `componentDidUpdate()` 함수와 같은 Life Cycle Hook 함수를 사용해서 Side Effect 처리했었습니다.

예를 들어, 아래코드와 같이 외부 API 연동을 통해 사용자 목록을 조회하는 React컴포넌트를 생각해보겠습니다. 이 클래스 컴포넌트는 로딩 여부와 사용자 목록을 `this.state` 필드의 `loading` 과 `users` 속성에 각각 저장하고 있습니다.

이 컴포넌트가 최초 렌더링될 때는 `loading`의 초기 값이 `true` 이기 때문에 `Loading...` 이라는 메세지가 화면에 나타납니다. 하지만 곧 `componentDidMount()` 함수가 호출되면 API가 호출되어 그 응답 결과가 `users` 속성에 할당되고 `loading` 이 `false` 로 업데이트 됩니다. 따라서 화면에 있던 `Loading...` 메세지는 사라지고, 대신 사용자 이름들이 나타나게 됩니다.

```
import React, { Component } from "react";

class UserListClass extends Component {
  state = {
    loading: true,
    users: [],
  };

  componentDidMount() {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((users) => this.setState({ users, loading: false }));
  }

  render() {
    const { loading, users } = this.state;
    if (loading) return <div>Loading...</div>;
    return (
      <ul>
        {users.map((user) => (
          <li key={user.id}>{user.name}</li>
        ))}
      </ul>
    );
  }
}
```

많은 React 개발자 들은 이렇게 간단한 Side Effect 구현 조차도 클래스 기반 컴포넌트로 작성해야 한다는 점에 대해서 불평했었습니다. 클래스 기반 컴포넌트는 함수 기반 컴포넌트에 비해 복잡하고 따라서 오류가 발생하기 쉽고 유지보수가 힘들기 때문입니다.

## setEffect() - 함수 기반 Side Effect

React Hooks에서 제공하는 `useEffect()` 함수를 사용해서 위의 클래스 기반 컴포넌트를 함수 기반으로 재작성 해보았습니다. `setEffect()` 함수에 인자로 외부 API를 호출 후에 호출 결과를 상태 변수에 저장해주는 코드를 넘기면 됩니다.

```
setEffect(() => {
    /* side effect code*/
});
```

함수 컴포넌트이기 때문에 `this.state` 가 아닌 또 다른 React Hooks인 `setState()` 함수를 사용해서 상태 관리를 해주고 있습니다.

```
import React, { useState, useEffect } from "react";

function UserListFunction() {
  const [loading, setLoading] = useState(true);
  const [users, setUsers] = useState([]);

  useEffect(() => {
    fetch("https://jsonplaceholder.typicode.com/users")
      .then((response) => response.json())
      .then((users) => {
        setUsers(users);
        setLoading(false);
      });
  });

  if (loading) return <div>Loading...</div>;
  return (
    <ul>
      {users.map((user) => (
        <li key={user.id}>{user.name}</li>
      ))}
    </ul>
  );
}
```

## 마치면서

본 예제 코드는 간단해서 코드가 많이 단순해지는 것처럼 보이지 않지만, 실제 프로젝트에서 사용되는 복잡한 클래스 컴포넌트를 `useState()` 를 사용해서 함수 컴포넌트로 리팩토링을 해보면 그 진가를 확인하실 수 있으실 것입니다.
