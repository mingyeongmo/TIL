# Recoil - 또 다른 React 상태 관리 라이브러리?

---

많은 React 상태 관리 라이브러리들이 있고, 가끔 새로운 라이브러리가 등장한다. 그러나 페이스북에서 직접 상태 관리 솔루션을 소개하는 것은 흔하지 않다. 이 라이브러리가 어떤 장점이 있고 새로운 점이 있는지, 그리고 앞으로 시간을 투자할 가치가 있는지 알아보자.

![Recoil.js](https://miro.medium.com/max/2000/1*0SkjAGdVWYe4ja5Qu4DeJg.jpeg)
(Recoil.js - 페이스북에서 만든 상태 관리 라이브러리)

페이스북 소프트웨어 엔지니어 Dave McCabe가 유튜브에서 열린 'React Europe 2020' 온라인 이벤트에서 새로운 상태 관리 라이브러리를 소개하였다. 2020년 5월 현재 Recoil은 아직 실험 단계이지만(페이스북의 일부 프로덕션에 사용되었을 것이라고 추측한다), McCabe와 그의 동료들이 어떤 이유로 이 오픈소스 라이브러리를 만들었는지는 흥미롭다.

그들은 복잡한 UI를 대상으로 전역 상태 관리를 위한 최적화 방법을 찾으려고 하였으나 성능 및 효율성이라는 장벽에 부딪혔다. 그리고 이 문제를 해결할 가장 좋은 방법은 직접 라이브러리를 만드는 것이라고 결정하였다.

## Redux나 Mobx가 잘못된 것일까?

기존의 상태 관리 라이브러리들은 어떠한 문제도 없다. 하지만 중요한 점은 상태 관리 라이브러리들이 React 라이브러리가 아니라는 점이다.
store는 "외부요인으로" 취급되는 것이기 때문에 React의 내부 스케줄러에 접근할 수 없다. 지금까지는 이것이 중요하지 않을 수도 있었다.
그러나 동시성 모드가 등장하며 이야기가 달라졌다. 아마도 페이스북 소프트웨어 개발자들은 동시성 모드를 사용하고 있을 것이고, 그들이 React와 동시성 모드를 손쉽게 사용할 수 있는 해결 방안이 필요하였을 것이다 (Recoil은 내부적으로 React의 상태를 사용하고 있으며, 동시성 모드에 대한 지원도 곧 추가될 것이다).

또한 일부 라이브러리(Redux..)는 강력한 기능을 제공하지만, 기본적인 store 구성을 위해 많은 보일러 플레이트와 장황한 코드를 작성해야한다. 또한 비동기 데이터 처리 또는 계산된 값 캐시와 같은 중요한 기능은 라이브러리의 기능이 아니며, 이를 해결하기 위해 또 다른 라이브러리를 사용해야한다. 그리고 만약 selector가 동적인 prop을 받는 경우 이값을 정확하게 memoization하는 것은 어려운 일이다.

## Context API를 사용하는 것은 어떨까?

React가 가진 상태 공유 솔루션인 Context API도 한계가 있다.

반복적이고 복잡한 업데이트에 사용할 경우 비효율적이다. 또 다른 페이스북 엔지니어인 Sebastian Markbage의 말을 인용하면:

> 개인적으로 새로운 Context는 locale/theme와 같은 낮은 빈도이 업데이트에 사용 가능하다고 생각한다. 또한 이전에 Context를 사용했던 방법으로 사용해도 좋다. (즉 정적인 값, 구독을 통해 업데이트를 전파하는 것) **하지만 Context는 Flux와 같은 상태 관리 시스템을 대체할 수 없다.**

React-Redux 팀도 이전 버전과 비교하였을 때 상당한 성능 문제가 있어 6버전에서 Context API로 재작성한 라이브러리의 일부를 롤백해야 했다 (현재 React-Redux는 store 참조를 전달할 때만 컨텍스트를 사용한다).

이미지의 목록을 보여주는 `list` 컴포넌트와 특정 이미지의 메타데이터를 보여주는 `info` 컴포넌트를 렌더링한다고 생각해보자. 이미지를 클릭하면 해당 메타데이터가 `info` 컴포넌트에 표시되어야 한다. 또한 이미지의 이름도 변경할 수 있어야한다.

![image_list](https://miro.medium.com/max/1400/1*jd9hhCED1hosJyjFTUev4A.png)

이름을 변경하면 선택한 이미지 컴포넌트와 메타데이터 컴포넌트만 다시 렌더링하는 것이 가장 좋은 결과일 것이다.

하지만 Context API로 이것을 구현하기는 어려울 것이다. 왜냐하면 Context API는 데이터 서브셋을 대상으로 변경을 감지하고 업데이트할 수 없기 때문이다.

> Provider 하위의 모든 consumer들은 Provider 속성이 변경될 때마다 다시 렌더링된다.

만약 Provider의 값이 배열이나 객체인 경우 구조가 조금이라도 변경된다면, 그 Context를 구독하고 있는 **하위의 모든 것**(컴포넌트가 그 값의 일부분만 사용하더라도)이 다시 렌더링될 것이다. Javier Calzado의 예시를 보면 이해가 갈 것이다. 이를 통해 모든 이미지를 하나의 Context에 저장할 수 없다는 것을 알 수 있다. 왜냐하면 이미지 하나의 이름을 변경하면 모든 것이 다시 렌더링될 것이기 때문이다(memoization을 통해 일부 문제를 해결할 수 있지만, 모든 것을 해결할 수 있는 방법은 아니며 한계가 있다).

각각의 이미지가 각가의 Context를 가지고 있다고 가정해보자. 정확한 이미지 수를 알고 있다면 아무 문제가 ㅇ벗다. 하지만 이미지를 동적으로 변경할 수 있고, 추가할 수 있다면? 새로운 이미지에 Context Provider를 추가하여 컴포넌트 트리를 다시 구성하고 **전체 서브 트리를 다시 마운트**해야한다. 더 좋지 않은 방법이다. 아래 GIF를 통해 이 문제를 보자.

![](https://miro.medium.com/max/800/1*9fEPTka4XmZFrtp2HQNtYg.gif)

(동적으로 provider를 추가하면 전체 하위 트리가 다시 마운트됨)
3번째 슬라이드에서 Context Provider를 추가한 후 하위의 모든 것이 다시 마운트된다.

성능에도 좋지 않을 뿐 아니라 Provider와 컴포넌트 트리 노드 사이에 강한 커플링이 생긴다.

## Recoil은 무엇이 다를까?

우선 첫번째, Recoil은 배우기 쉽다. API가 단순하고 이미 hook을 사용하고 있는 사람들에게 익숙할 것이다. Recoil을 시작하기 위해서는 어플리케이션을 `RecoilRoot` 로 감싸고, 데이터를 `atom`이라는 단위로 선언하여 `useState`를 Recoil의 `useRecoilState`로 대체해야 한다.

두번째, 컴포넌트가 사용하는 데이터 조각만 사용할 수 있고, 계산된 selector를 선언할 수 있으며, 비동기 데이터 흐름을 위한 내장 솔루션까지 제공한다.

동적 키로 atom을 만들고, selector에 인자를 보내는 등 모두 간단하게 할 수 있다.

그리고 앞에서 말한 바와 같이 곧 React 동시성 모드에 대한 지원도 될 것이다.

## Recoil의 기본부터 시작해보자

**Atom** - atom은 하나의 상태라고 볼 수 있다. 컴포넌트가 구독할 수 있는 React state라고 생각하면 된다. atom의 값을 변경하면 그것을 구독하고 있는 컴포넌트들이 모두 다시 렌더링된다. atom을 생성하기 위해 어플리케이션에서 고유한 키 값과 디폴트 값을 설정해야한다. 디폴트 값은 정적인 값, 함수 또는 심지어 비동기 함수(나중에 지원 예정)가 될 수 있다.

```
export const nameState = atom({
    key: 'nameState',
    default: 'Jane Doe'
});
```

**useRecoilState** - atom의 값을 구독하여 업데이트할 수 있는 hook. `useState` 와 동일한 방식으로 사용할 수 있다.

**useRecoilValue** - setter 함수 없이 atom의 값을 반환만 한다.

**useSetRecoilState** - setter 함수만 반환한다.

```
import {nameState} from './someplace'
// useRecoilState
const NameInput = () => {
  const [name, setName] = useRecoilState(nameState);
  const onChange = (event) => {
   setName(event.target.value);
  };
  return <>
   <input type="text" value={name} onChange={onChange} />
   <div>Name: {name}</div>
  </>;
}
// useRecoilValue
const SomeOtherComponentWithName = () => {
  const name = useRecoilValue(nameState);
  return <div>{name}</div>;
}
// useSetRecoilState
const SomeOtherComponentThatSetsName = () => {
  const setName = useSetRecoilState(nameState);
  return <button onClick={() => setName('Jon Doe')}>Set Name</button>;
}
```

**selector** - selector는 상태에서 파생된 데이터로, 다른 atom에 의존하는 동적인 데이터를 만들 수 있게 해준다. Recoil의 selector는 기존에 우리가 알던 selector의 개념과는 조금 다르다. Redux의 `reselect`와 MobX의 `@computed` 처럼 동작하는 "get" 함수를 가지고 있다.
하지만 하나 이상의 atom을 업데이트 할 수 있는 "set" 함수를 옵션으로 받을 수 있다. 이 부분은 나중에 다룰 테니, 일단 "selector" 부분만 살펴보자.

```
// 동물 목록 상태
const animalsState = atom({
    key: 'animalsState',
    default: [{
        name: 'Rexy',
        type: 'Dog
    }, {
        name: 'Oscar',
        type: 'Cat'
    }],
});
// 필터링 동물 상태
const animalFilterState = atom({
    key: 'animalFilterState',
    default: 'dog',
});
// 파생된 동물 필터링 목록
const filteredAnimalsState = selector({
    key: 'animalListState',
    get: ({get}) => {
        const filter = get(animalFilterState);
        const animals get(animalsState);

        return animals.filter(animal => animal.type === filter);
    }
});
// 필터링된 동물 목록을 사용하는 컴포넌트
const Animals = () => {
    const animals = useRecoilValue(filteredAnimalsState);
    return animals.map(animal => (<div>{ animal.name }, { animal.type   }</div>));
}
```

## 복잡한 것을 해보자

위에서 이야기했던 이미지 어플리케이션을 Recoil로 만들어보자

![](https://miro.medium.com/max/1400/1*jd9hhCED1hosJyjFTUev4A.png)

**어플리케이션의 요구 사항은 다음과 같다.**

1. 이미지를 동적으로 추가할 수 있어야 한다.
2. 이미지의 이름을 변경할 때는 선택한 이미지와 메타데이터 컴포넌트만 다시 렌더링해야 한다.
3. 이미지와 데이터는 비동기적으로 로딩되어야 한다.
