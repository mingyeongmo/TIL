# 고정 스타일링

위에서 배운 Styled Components 문법을 이용해서 간단하게 React로 작성된 버튼 컴포넌트를 스타일링 해보겠습니다. 우선 `<button>` HTML 엘리먼트에 원하는 스타일을 적용한 후 `StyledButton` 변수에 저장합니다.

이렇게 `styled` 함수가 리턴하는 것은 위에서 설명드린 것 처럼 React컴포넌트이기 때문에 JSX를 통해 자유롭게 사용할 수 있습니다.

```javascript
import React from "react";
import styled from "styled-components";

const StyledButton = styled.button`
  padding: 0.375rem 0.75rem;
  border-radius: 0.25rem;
  font-size: 1rem;
  line-height: 1.5;
  border: 1px solid lightgray;
  color: gray;
  background: white;
`;

function Button({ children }) {
  return <StyledButton>{children}</StyledButton>;
}
```

자, 이제 스타일이 적용된 이 버튼 컴포넌트를 다른 React 컴포넌트에서 다음과 같이 사용할 수 있습니다.

```javascript
import Button from "./Button";
<Button>Default Button</Button>;
```

브라우저에서 소스 보기를 해보면 다음과 같이 `<button>` HTML 엘리먼트에 Styled Components가 자동으로 생성해준 클래스 이름이 적용되었음을 알 수 있습니다.

```html
<button class="sc-kgAjt beQCgz">Default Button</button>
```

한편, 내부 스타일시트를 확인해보면 클래스 선택자(class selector)로 적용된 스타일이 위에서 Styled Components로 삽입한 스타일과 동일함을 알 수 있습니다.

```css
.beQCgz {
  padding: 0.375rem 0.75rem;
  border-radius: 0.25rem;
  font-size: 1rem;
  line-height: 1.5;
  border: 1px solid lightgray;
  color: gray;
  background: white;
}
```
