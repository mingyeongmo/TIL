# Styled Components 사용법

---

## CSS in JS?

먼저 CSS in JS의 개념을 짚고 넘어가겠습니다. CSS in JS는 스타일 정의를 CSS 파일이 아닌 JavaScript로 작성된 컴포넌트에 바로 삽입하는 스타일 기법입니다.

기존에 웹사이트를 개발할 때는 HTML과 CSS, JavaScript는 각자 별도의 파일에 두는 것이 best practice로 여겨졌었습니다. 하지만 React나 Vue, Angular와 같은 모던 자바스크립트 라이브러리가 인기를 끌면서 웹개발의 패러다임이 바뀌고 있습니다. 최근에는 웹 애플리케이션을 여러 개의 재활용이 가능한 빌딩 블록으로 분리하여 개발하는 컴포넌트 기반 개발 방법이 주류가 되고 있습니다.

따라서, 웹페이지를 HTML, CSS, JavaScript 3개로 분리하는 것이 아니라, 여러 개의 컴포넌트로 분리하고, 각 컴포넌트에 HTML, CSS, JavaScript를 몽땅 때려 박는 패턴이 많이 사용되고 있습니다. React는 JSX를 사용해서 이미 JavaScript가 HTML을 포함하고 있는 형태를 취하고 있는데, 여기에 CSS-in-JS 라이브러리만 사용하면 CSS도 손쉽게 JavaScript에 삽입할 수 있습니다.

> 유행은 항상 돌고 돌기 때문에, 어떤 방법이 더 좋다라고는 가급적 얘기하지 않으려고 합니다. 두 가지 방법 다 장단점이 있고 프로젝트의 특성을 고려해서 선택해야 한다고 생각합니다.

Styled Components는 이렇게 트랜드가 되고 있는 CSS-in-JS 라이브러리 중에서도 단연 가장 널리 사용되고 있는 라이브러리입니다.

## 패키지 설치

Styled Components는 `styled-components`라는 NPM 패키지명을 가지고 있습니다. 따라서 React 프로젝트에 다음과 같이 `npm` 커맨드로 간단히 설치할 수 잇습니다.

```javascript
$ npm i styled-components
```

설치 후에 `package.json`에 `styled-components`가 추가된 것을 확인할 수 있습니다.

```javascript
"dependencies": {
    "react" : "^16.8.0",
    "react-dom": "^16.8.0",
    "styled-components": "^4.3.2"
},
```

## 기본 문법

먼저 위에서 설치한 `styled-components` 패키지에서 `styled` 함수를 임포트합니다.
`styled`는 Styled Components의 근간이 되는 가장 중요한 녀석인데요. HTML 엘리먼트나 React 컴포넌트에 원하는 스타일을 적용하기 위해서 사용됩니다.

기본 문법은 HTML 엘리먼트나 React 컴포넌트 중 어떤 것을 스타일링 하느냐에 따라 살짝 다릅니다.

HTML 엘리먼트를 스타일링 할 때는 모든 알려진 HTML 태그에 대해서 이미 속성이 정의되어 있기 때문에 해당 태그명의 속성에 접근합니다.

```javascript
import styled from "styled-components";

styled.button`
  // <button> HTML 엘리먼트에 대한 스타일 정의
`;
```

React 컴포넌트를 스타일링 할 때는 해당 컴폰넌트를 임포트 후 인자로 해당 컴포넌트를 넘기면 됩니다.

```javascript
import styled from "styled-components";
import Button from "./Button";

styled(Button)`
  // <Button> React 컴포넌트에 스타일 정의
`;
```

두가지 문법 모두 ES6의 Tagged Template Literals을 사용해서 스타일을 정의합니다. 그리고 `styled` 함수는 결국 해당 스타일이 적용된 HTML 엘리먼트나 React 컴포넌트를 리턴합니다.

예를 들어, 다음과 같이 Styled Components로 작성된 Javascript 코드는

```javascript
import styled from "styled-components";

styled.button`
  font-size: 1rem;
`;
```

아래 CSS 코드가 적용된 `<button>` HTML 엘리먼트를 만들어낸다고 생각하면 쉽습니다.

```css
button {
  font-size: 1rem;
}
```

이런 식으로 Styled Components를 이용해서 JavaScript 코드 안에 삽입된 CSS 코드는 글로벌 네임 스페이스를 사용하지 않습니다. 다시 말해, 각 JavaScript 파일마다 고유한 CSS 네임 스페이스를 부여해주기 때문에, 각 React 컴포넌트에 완전히 격리된 스타일을 적용할 수 있게 됩니다.

이것은 순수하게 CSS만을 사용했을 때는 누리기 어려웠던 대표적인 CSS in JS의 장점 중 하나 입니다.

> 외부 라이브러리 없이도 각 CSS 파일에 고유의 네임 스페이스를 부여해주는 CSS 모듈이라는 기술을 통해서 위 동작을 흉내낼 수 있습니다.
