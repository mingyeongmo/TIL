# Side effect란?

---

### : 리액트의 함수형 컴포넌트의 밖에서 로컬의 상태를 변경하는 것을 뜻한다.

- 그렇다면, **왜 리액트에서는 Side effect 의 개념이 필요한 것일까?**

  -
  - 대표적으로 "비동기처리"가 있다.
  - 리액트에서 컴포넌트를 rendering 할 때, 서버에서 값을 받아오는 경우를 생각해보자.
  - 컴포넌트의 lifecycle 중에서, 서버와 통신하게 되면, 화면이 잠시 멈추거나 끊기는 상황이 나타날 수 있다.
  - 따라서, 서버에서 값을 받아오는 작업은 lifecycle에 영향을 주지 않는 방법으로 처리해야 한다.
  - 이때, **컴포넌트의 lifecycle과 무관하게**
  - 외부에서 비동기로 서버와 통신하고, 컴포넌트 state를 업데이트하는 Side effect 개념이 필요하게 된다.
  - 이러한 작업시, useEffect()가 매우 유용하게 사용될 수 있다.
  - 비동기처리, 병렬처리, setTimeout(),setInterval(), Event handling 등 시간이 많이 걸리거나, lifecycle과 무관한 작업을 수행하고 싶을 때 사용될 수 있다.

  ```
  function UserInfo({ userId }) {
      // 서버 통신은 컴포넌트 외부에서 실행(=Side Effect)
      // 1. useEffect()함수를 사용하여 외부에서 실행될 Side Effect 함수 등록
      // 2. Side Effect 함수 처리가 완료되면 컴포넌트 외부에서 컴포넌트 상태 업데이트
      useEffect(
          () => {
              getUserFromServer(userId)
                  .then(userData => setUser(userData));
          },
          [userId]
      );
      return (
          <div>
              {!user && <p>조회 중입니다...</p>}
              {user && (
                  <ul>
                      <li>{`아이디: $user.name}`</li>
                      <li>{`이메일: ${user.email}`}</li>
                  </ul>
              )}
          </div>
      );
  }
  ```

- useEffect 는 function component 내에서 Side effects가 가능하게 한다. (= Side effect 함수를 처리할 때 사용한다.)
- class component의 **componentDidMount, componentDidUpdate, componentWillUnmount** 와 비슷한 역할을 한다. (하나의 api로 통합된 것)
- effect를 해제하기 위해서는 해제하는 함수를 반환하면 된다. (optional)

- 따라서, `useEffect` HOOK을 사용하면 (구독을 추가하고 제거하는 로직과 같이) 서로 관련 있는 코드들을 한군데에 모아서 작성할 수 있다. (class component에서 mount, update, unmount로 나눠서 넣은 것과 달리)

- 단, 최상위(at the top level)에서만 HOOK을 호출해야 한다.
  - 이 규칙을 따르면 컴포넌트가 렌더링 될 때마다 항상 동일한 순서로 Hook이 호출되는 것이 보장된다.
  - useState와 useEffect가 여러번 호출되는 중에도 HOOK의 상태를 잘 유지할 수 잇게 한다.
- react function component 내에서만 HOOK을 호출해야 한다.
  - (예외) Custom Hook에서 Hook을 호출할 수 있다.
