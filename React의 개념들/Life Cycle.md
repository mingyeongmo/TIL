# 리액트 Life Cycle

---

## 0. LifeCycle Api

### 1) LifeCycle 정의

- React는 컴포넌트 단위로 UI를 화면에 보이게하고, UI를 바꾸고, 현재 UI를 화면에 없앤다.

- 각각의 컴포넌트들은 생성 -> 업데이트 -> 제거 단계를 겪는 생명주기(LifeCycle)를 가지고 있다.

### LifeCycle API

- React는 컴포넌트를 DOM 위에 생성하여 Rendering(화면에 보이기) 한다.

- 여기서 컴포넌트가 DOM 위에 생성 시간 상황에 따라 실행되는 메소드들이 LifeCycle API 이다.

- 컴포넌트가 DOM 위에 생성 할 때 나타나는 상황

> 컴포넌트가 DOM 위에 생성 될 때 [컴포넌트가 처음 화면에 렌더링]
> 컴포넌트가 DOM 위에 사라지기 전 [컴포넌트가 화면에 지워짐]
> 데이터가 변경되어 상태를 업데이트를 한 후 [데이터가 변경될 때]

- React 컴포넌트 rendering 할 때 나오는 메소드 순서

> Constructor
> ComponentWillMount
> Render
> ComponentDidMount

- React 상태 값 변경이 될 때 나오는 메소드

> componentDidUpdate

- 컴포넌트 제거 시 나오는 메소드

> componentWillUnmount

- Lifecycle API 메소드는 클래스형 컴포넌트에서만 사용이 가능하다.
- 함수형 컴포넌트에서 useEffect Hook이 대신 그 역할을 한다. (useEffect 참고..)

### 3) LifeCycle API 흐름 정리

![LifeCycleAPI흐름정리](https://media.vlpt.us/images/sdc337dc/post/5145d421-a716-43e6-b510-44edffa46716/image.png)

## 1. 컴포넌트 화면 렌더링 관련 LifeCycle

#### 1) constructor

- 생성자 메소드
- 컴포넌트가 처음 컴파일되고 만들어질 때 실행
- 여기에 기본 state를 지정할 수 있다.
- 이후 화면이 변경하거나 다시 나타나도 실행되지 않음.

![](https://media.vlpt.us/images/sdc337dc/post/6a9ab995-c54a-4483-986b-91bcd56f69f3/image.png)

#### 2) componentWillMount

- 컴포넌트가 DOM 위에 만들어지기 전에 실행됩니다.

![](https://media.vlpt.us/images/sdc337dc/post/5ef882ae-54ea-47f7-9758-f61bcd1492a5/image.png)

#### 3) render

- 컴포넌트 렌더링을 담당합니다.

![](https://media.vlpt.us/images/sdc337dc/post/7019f487-da79-4bcd-92eb-76b8e22111ec/image.png)

#### 4) componentDidMount

- 컴포넌트가 만들어지고 첫 render를 한 후 실행되는 메소드
- 여기서 비동기처리, 기타 자바스크립트 프레임워크를 연동한다.

![](https://media.vlpt.us/images/sdc337dc/post/c97d3780-02e1-4ea4-af33-3d882df80237/image.png)

#### 5) 컴포넌트 화면 렌더링 최종 정리

![](https://media.vlpt.us/images/sdc337dc/post/0b97594a-b373-433a-a4c8-2191c75ba870/image.png)

![](https://media.vlpt.us/images/sdc337dc/post/621fe510-6e0b-4db7-adcb-dce97e53bbce/image.png)

## 2. 상태 변경 관련 LifeCycle

#### 1) componentDidUpdate

- 컴포넌트에서 render()를 호출하고 난 다음에 발생. 최초의 render에서는 호출되지 않는다.
- component가 업데이트 되었을 때 작동한다.

## 3. 컴포넌트 화면 사라지는 경우

#### 1) componentWillUnmount

- 컴포넌트가 DOM 에서 사라진 후 실행되는 메소드입니다.
- 필수적인 제거, 유효하지 않은 타이머, 네트워크 요청 취소, 구독 해제 같은 일들을 할 때 호출

![](https://media.vlpt.us/images/sdc337dc/post/8b8c016a-d8bb-4506-a3e6-220c6bc07ee4/image.png)
