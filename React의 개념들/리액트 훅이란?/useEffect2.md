# useEffect

---

## 1. useEffect란 무엇인가?

useEffect는 기본적으로 몇 가지 조건에 의해 작동하게 됩니다.

첫번째 : 페이지가 처음 렌더링 되고 난 후 useEffect는 **무조건** 한번 실행됩니다.

두번째 : useEffect에 배열로 지정한 useState의 값이 변경되면 실행되게 됩니다.

즉 useEffect는 렌더링, 혹은 변수의 값 혹은 오브젝트가 달라지게 되면, 그것을 인지하고 업데이트를 해주는 함수입니다.
useEffect는 콜백 함수를 부르게 되며, 렌더링 혹은 값, 오브젝트의 변경에 따라 어떠한 함수 혹은 여러 개의 함수들을 동작시킬 수 있습니다.

## 2. useEffect를 사용하는 여러 가지 방법

useEffect를 사용하는 방법은 총 3가지 정ㅈ도로 압축해볼 수 있습니다.

```
useEffect(()=> {});

// Or

useEffect(()=> {},[]);

// Or

const [name, setName] = useState();
useEffect(()=> {},[name]);
```

첫 번째 : useEffect의 가장 기본 형태이지만, 이러한 형태를 거의 사용하지는 않습니다.
Dependency가 없기 때문에 렌더링 할 때 한번 그리고 어떠한 작은 요소라도 변화한다면 시시때때로 useEffect가 발동되어 불필요한 실행이 너무 많아집니다.

두 번째 : useEffect를 렌더링 후 단 한 번만 실행하고 싶을 때 사용하는 방법입니다.
콜백 함수 뒤에 배열을 나타내는 대괄호가 붙어있습니다.
이곳에 Dependency를 지정합니다. 하지만 아무 변수나 값 없이 대괄호만 있다면, 이 useEffect는 렌더링 후 단 한 번만 실행되고 다시는 실행되지 않습니다.

세 번째 : useEffect를 렌더링 후 한번, 그리고 배열 안 변수의 값이 변할 때마다 실행하는 코드입니다.
이렇게 Dependency를 지정해주어 지정된 변수의 값이 변했을 때만, 실행되게 됩니다.

## 3. useState와 useEffect 사용법

자 이제는 간단한 예시를 통해서 useEffect가 어떻게 동작하는지 좀 더 자세히 들여다보겠습니다.
이렇게 준비된 코드를 리액트 안에 넣고 실행시키게 되면,

```
import React, { useEffect, useState } from 'react';

const Number = () => {
    const [number, setNumber] = useState(0); // useState로 number 변수를 0으로 초기화.
    const [name, setName] = useState('Josh'); // Josh로 초기화

    useEffect(() => {
        console.log('hello');
    });

    const counter = () => setNumber(number + 1); // 함수
    const nameChanger = () => setName('Mike'); // 함수

    return (
        <div>
            <button onClick={counter}>click</button>
            <button onClick={nameChanger}>change Name</button>
            <div>{number}</div>
            <div>{name}</div>
        </div>
    ); // JSX 사이에 변수를 사용가능하며 중괄호를 이용합니다.
};

export default Number;
```
