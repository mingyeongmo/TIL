# React 상태관리

---

## 상태관리

- **React** 와 같은 **SPA** 를 개발할 때, 많은 컴포넌트의 페이지가 생성된다면, 컴포넌트 사이에 데이터 및 메소드의 접근이 매우 복잡해지는 경우가 발생할 수 있습니다.
- 작성하는 입장에서는 물론이고 가독성 면에서도 상당히 떨어지기 때문에, 이를 **Global Store 영역**에서 관리하여 일일이 props를 넘겨주지 않고도 **컴포넌트 트리 전체**에 데이터를 제공할 수 있습니다.

## Context API

- React에서 트리의 모든 레벨에 값을 공유하는 자체 방법을 제공.
- 성능, 기능이 적음.
- 함수형 Hooks에서는 기능이 제한
- 구조는 단순, 간단한 전역 상태관리만 해도 되면 좋음.
- State 받아서 보관, 공유만 해줌, 상태객체를 만들어줘야 함.
- 정말 간단한 상태관리만 필요하다면 Context API (React 자체 제공)

1. **React.createContext** : Context 객체를 만들고, 컴포넌트를 렌더링할 때 React는 트리 상위에서 가장 가까이 있는 짝이 맞는 Provider로부터 현재값을 읽음.

2. **Context.Provider** : context를 구독하는 컴포넌트들에게 context의 변화를 알리는 역할.

3. **Context.Consumer** : context 변화를 구독하는 React 컴포넌트입니다. 함수 컴포넌트안에서 context를 읽기 위해서 쓸 수 있음.

## Mobx

- **React**를 위한 상태관리 라이브러리.

![Mobx](https://media.vlpt.us/images/asdfgasdfg/post/01be9c33-c68b-4616-8562-d11c52ddaecf/image.png)

- 디버깅 툴이 좋지않음.
- 코드가 짧음.
- 처음 구조를 잡지 않고 개발하면 코드가 지저분해짐.
- Decorater가 오히려 독이 될 수 있음.

1. **Observable State** (관찰 받고 있는 상태)
   : 원시적인 값, 객체, 배열 내부의 객체 및 객체의 키 등 바뀌면, MobX 에서는 정확히 어떤 부분이 바뀌었는지 알 수 있음.
2. **Computed Value** (연산된 값)
   : 기존의 상태값과 다른 연산된 값에 기반하여 만들어질 수 있는 값.
   어떤 값을 연산해야 할 때, 연산에 기반이 되는 값이 바뀔 때만 새로 연산하게 하고, 바뀌지 않았다면 그냥 기존의 값을 사용.
3. **Reactions** (반응)
   : 값이 바뀜에 따라 해야 할 일을 정하는 것을 의미.
4. **Actions** (행동)
   : 상태에 변화를 일으키는 것.

## Redux

- **Javascript App**을 위한 예측 가능한(predictable) 상태 컨테이너.

![Redux](https://media.vlpt.us/images/asdfgasdfg/post/a894ea3a-00d5-415d-a16c-e3ea4bc21b93/image.png)

- 하나의 root에 하나의 store만이 존재하고, 순수함수(pure functions)에 의존.
- 순수함수는 동일한 인자가 주어졌을 때, 항상 동일한 결과 반환. 외부상태 변경 X
- MVC 패턴의 문제점을 극복하고자 페이스북에서 고안한 Flux 아키텍처 기반 중앙관리를 위한 상태 관리 도구.
- 상태가 읽기 전용이므로, 이전 상태로 돌아가기 위해서는 그저 이전 상태를 현재 상태에 덮어쓰기만 하면 되는 장점
- 전역 상태를 생성 및 관리, 러닝 커브가 있음.
- 데이터 흐름이 어떻게 작동하는지 이해하기 쉬움.
- 비동기 Action 처리를 위해선 Redux-saga, Redux-Thunk 등 사용 강제
  - Reducer 안에 부작용이 생길 처리를 써선 안 됨.
  - 같은 입력에 대해 확률적으로 다른 결과가 나오는 처리
  - 지연처리
  - HTTP 리퀘스트 처리 등...

1. Actions : 저장소로 data를 보내는 방법.
   - view에 정의된 액션을 호출하면 Action Creators는 state를 변경함
   - Action의 Type은 일반적으로 문자열 상수로 정의되고, 정의된 Action Type은 Action Creators(액션 생성자)를 통해 사용됨.
2. Reducer : Action을 통해 어떠한 행동을 정의하고, 그 결과 어플리케이션의 상태가 어떻게 바뀌는지는 특정하게 되는 함수.
   - 이전 상태와 액션을 받아 다음 상태를 반환.
   - 초기상태는 Reducer의 디폴트 인수에서 정의.
   - 상태가 변할 때 전해진 state는 그 자체의 값으로 대체 되는 것이 아니라, 새로운 것이 합성되는 것처럼 쓰여짐.
3. Store : Action과 상태를 수정하는 reducer를 저장하는 단 하나의 객체
4. React의 Component자체는 Redux의 흐름에 타는 것이 불가능함.
   -> connect 함수를 이용.

### Redux는 Flux의 구현체 느낌.

![Redux와 Flux](https://media.vlpt.us/images/asdfgasdfg/post/7c50c46e-03ba-4cc0-b1e2-8eab54b7a9be/image.png)

- 리액트의 state 개념 및 읽기전용 등 유사성을 가짐.

## Mobx vs Redux 공통점

- time-travel debugging
- open-source libraries
- client-side state management

**나머지**

- `Redux` 순수함수, 함수형 프로그래밍 패러다임으로 쉽게 제어가능
- `Redux` 개발자 툴 제공, 추상화 정도가 낮아 mobx보다 디버깅이 예측가능함
- `Redux` 커뮤니티 기반이 더 넓음
- `Redux` JS 데이터 사용하여 저장, 모든 업데이트 수동으로
- `Redux` 는 모든 상태가 저장되는 하나의 큰 스토어.

- `Mobx` 학습이 쉬움, OOP 개념에 기반
- `Mobx` 상태를 덮어쓸 수 있음, `Redux`직접 덮어쓸 수 없고, 이전 상태가 새 상태로 바뀜
- `Mobx` 관측 가능한 상태 값 사용
- `Mobx` 는 논리적으로 분리된 스토어가 두 개 이상.

## 결론

- 간단한 상태관리 또는 중앙화 만이 목적이라면 `Context API`
- 한 스토어에 저장되는 데이터가 명확하고, 드물게 다른 스토어에 있는 데이터에 접근하는 경우 복잡한 상태관리가 요구되지 않는 작은 프로젝트라면 `Mobx`
- 그 외 나머지, `Redux`
