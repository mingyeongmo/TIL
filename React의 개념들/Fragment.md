# Fragment

---

리액트에서는 하나의 컴포넌트가 여러 개의 엘리먼트들을 반환한다. 리액트를 사용하기 위한 문법인 JSX를 쓸 때, return 문 안에는 반드시 하나의 최상위 태그가 있어야 한다. 이는 리액트가 하나의 컴포넌트만을 리턴할 수 있기 때문이다.

## Fragment 사용 이유

Fragment 를 사용해야 하는 이유를 찾아보면 가장 많이 나오는 예시! 테이블 예시를 보자.

```
import React from "react";
function App() {
    return (
        <div className="App">
            <table>
                <tr>
                    <Table />
                </tr>
            </table>
        </div>
    );
}

function Table() {
    return (
        <div className="Table">
            <td>Hello</td>
            <td>World</td>
        </div>
    );
}

export default App;
```

table 컴포넌트의 랜더링을 위해서 table 이라는 클래스명을 가진 div 로 묶어서 return 을 한다. 그리고 메인에서 table 컴포넌트를 임포트하면 최종적으로 보여지는 html 은 다음과 같다.

```
<table>
    <tr>
        <div className="Table>
            <td>Hello</td>
            <td>World</td>
        </div>
    </tr>
</table>
```

이렇게 의미없는 div 가 추가된다. 이를 피하기 위해서 fragment 를 사용한다.

```
import { Fragment } from "react";
function Table() {
    return (
        <Fragment>
            <td>Hello</td>
            <td>World</td>
        </Fragment>
    );
}
```

fragment 를 import 해준 후 table 을 div 가 아닌 fragment 로 묶어준다. 그러면 html 구조는 다음과 같다.

```
<table>
    <tr>
        <td>Hello</td>
        <td>World</td>
    </tr>
</table>
```

## 단축문법

`<Fragment> </Fragment>` 대신 `<> </>` 이렇게 써도 된다.

## Fragments 사용 예시

map 을 사용해서 컴포넌트를 여러 개 만들 때, key attribute 를 fragment 에 넣어줄 수 있다.

```
function Glossary(props) {
    return (
        <dl>
            {props.items.map(item => (
                // React는 'key'가 없으면 key warning을 발생합니다.
                <React.Fragment key={item.id}>
                    <dt>{item.term}</dt>
                    <dd>{item.description}</dd>
                </React.Fragment>
            ))}
        </dl>
    );
}
```
