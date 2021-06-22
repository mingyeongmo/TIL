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
