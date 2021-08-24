# React스러운 상태관리 라이브러리 :: Recoil

![Recoil](https://media.vlpt.us/images/juno7803/post/1a7aa01e-6b1d-43c5-98d3-c386ad5ddf1b/recoil.png)

## 등장 배경

비교적 간단한 프로젝트엔 무겁게 느껴질 수 있는 Redux와 MobX, 그리고 이들은 근본적으로 React를 위해 출시된 것이 아니었습니다.
React의 hooks와 어울리면서 React에 필요한 것들만 React스럽게 제작한 상태관리 라이브러리 `Recoil` 을 소개합니다!

## Installation

```
npm install recoil 또는
yarn add recoil
```

npm 또는 yarn을 이용하여 `recoil` 패키지를 설치합니다.

## recoilRoot

```
import React from 'react';
import {
    RecoilRoot,
    atom,
    selector,
    useRecoilState,
    useRecoilValue,
} from 'recoil';

function App() {
    return (
        <RecoilRoot>
                <CharacterCounter />
        </RecoilRoot>
    );
}
```

`recoil state`를 사용할 컴포넌트라면, `RecoilRoot` 태그로 감싸주어야 합니다~

모든 곳에서 사용할 것이기 때문에 `root` 컴포넌트에 적용시켜 주겠습니다.

## Atom

```
const textState = atom({
    key: 'textState', // unique ID(다른 atom/selectors 와 구별하기 위함)
    default: '', // default value (=initial value)
})
```

`atom` 은 기존의 상태관리 라이브러리에서 쓰이는 `store` 와 유사한 개념으로, **상태의 단위** 입니다.

`atom`이 업데이트 되면, 해당 `atom`을 **구독**하고 있던 모든 컴포넌트들의 state가 새로운 값으로 리렌더링 됩니다.
unique 한 id인 `key`로 구분되는 각 `atom`은, 여러 컴포넌트에서 `atom`을 구독하고 있다면 그 컴포넌트들도 똑같은 상태를 공유합니다.

## useRecoilState

```
function CharacterCounter (){
    return (
        <div>
                <TextInput />
                <CharacterCount />
        </div>
    );
}

function TextInput() {
    const [text,setText] = useRecoilState(textState);
    const onChange = (event) => {
        setText(event.target.value);
    };

    return (
        <div>
                <input type="text" value={text} onChange={onChange} />
                <br />
                Echo: {text}
        </div>
    );
}
```

`useRecoilState` hook은 atom의 state를 `get` 그리고 `set` 할 수 있습니다

```
const [text, setText] = useRecoilState(textState);
```

다음과 같이 사용하면, `text` 는 textState의 value를 가지게 되고(`get`)

setText는 text의 값을 변경할 수 있습니다!(`set`)

## useRecoilValue 와 useSetRecoilValue

```
function TextInput() {
    // const [text,setText] = useRecoilState(textState);
    const text = useRecoilValue(textState);
    const setText = useSetRecoilValue(textState);

    const onChange = (event) => {
        setText(event.target.value);
    };
    ﹒﹒﹒
}
```

위의 코드에서 다음과 같이 주석 처리한 부분 중 `get`과 `set` 만을 사용할 수도 있습니다.

각각 `useRecoilValue` 와 `useSetRecoilValue` hook을 사용합니다!

## Selector

```
const charCountState = selector({
    key: 'charCountState', // unique ID (with respect to other atoms/selectors)
    get: ({get}) => {
        const text = get(textState);

        return text.length;
    },
});
```

공식문서의 표현을 빌리자면 `selector` 는 **derived state**, 즉 파생된 `state`를 나타낼 수 있습니다.

원래의 `state`를 그냥 가져오는 것이 아닌, `get` 프로퍼티를 통해 `state`를 가공하여 반환할 수 있습니다!

위의 코드는 `atom`의 `textState`를 가공하여 length를 받아옵니다.

```
function CharacterCount() {
    const count = useRecoilValue(charCountState);
    return(
        <>
                <div> Character Count : {count} </div>
        </>
    );
}
```

---
