# Next.js 제대로 알고 쓰자

---

Next.js는 React의 SSR을 쉽게 구현할 수 있게 도와주는 간단한 프레임워크입니다. React도 SSR을 고려하여 설계되었기 때문에 자체적으로도 구현이 가능하긴 하지만, 개발환경을 만들기 위해서는 생각보다 복잡합니다. 그래서 이러한 문제를 해결한 Next가 나왔고, 사용법도 꽤 간편합니다.

![Next.js](https://miro.medium.com/max/1400/1*TzAXPv-yG1yYn3x5EHKkiw.png)

과거의 웹사이트들은 대부분 SSR로 동작 되어 왔기 때문에, 페이지가 여러개로 구성된 Multi Page Form 방식을 사용했었습니다. 하지만 스마트폰이 등장하게 되면서, 예전 웹들은 모바일에 최적화가 되어있지 않았기 때문에 사용에 불편함이 커지게 되었습니다. 그래서 사용자들은 모바일 앱과 같은 형태의 웹페이지가 필요하게 되었는데요.

![](https://miro.medium.com/max/1400/1*FAAkplrC-HXq9tfk_dAqQQ.jpeg)
![](https://miro.medium.com/max/1400/1*5gCHJbU0qoYSKa-hvyyfVA.png)

이러한 문제를 해결하기 위해서 React, Angular, Vue 등 여러 라이브러리, 프레임워크가 생기게 되었고, CSR이 가능한 SPA가 등장하게 되었습니다. 이 SPA의 특징은 기존의 웹페이지와는 다르게 1개의 페이지에서 여러 동작이 이루어지는 방식입니다. 그래서 마치 모바일 앱과 비슷한 형태로 이루어져 있어서 Header영역은 고정되어 있고, Main 영역의 상태만 계속 변경하는 방식을 사용합니다.

## 1. SSR vs CSR

SSR와 CSR의 차이점

![SSRvsCSR](https://miro.medium.com/max/1400/1*mU0dHw7xyVDVQlopkdVJfA.png)

먼저 일반적인 React 렌더링 방식은 render() 함수가 먼저 실행되고, 그다음에 ComponentDidMount() 함수를 통해 데이터를 가져와서 다시 한 번 렌더링이 됩니다. 반면에 Next는 getInitialProps() 라는 함수를 사용하기 때문에 데이터를 먼저 가져와서 한 번에 렌더링을 해줍니다. 보시다시피 SSR은 한번에 렌더링이 되기 때문에 초기로딩 속도는 빠르지만, page 이동 시에는 CSR보다 느립니다. 왜냐하면, page를 요청할 때마다 중복되는 파일을 내려 받아야 하기 때문입니다. 하지만 CSR은 초기로딩 속도가 느리지만, 첫 페이지 로딩 때 모든 파일을 내려받기 때문에 피잊를 이동할 때마다 해당 페이지에서 필요한 데이터만 불러서 사용하면 됩니다.

## 2. Next.js 핵심기능

Next.js는 어떤 기능들을 제공해주는가?

#### 코드 스플리팅

일반적인 리액트 싱글페이지에서는 초기 렌더링 때 모든 컴포넌트를 내려 받습니다. 하지만 규모가 커지고, 용량이 커지면 로딩속도가 지연될 수 있는 문제점이 있습니다. Next는 이러한 문제점을 개선해서 필요에 따라 파일을 불러올 수 있게 여러 개의 파일을 분리하는 코드 스플리팅을 사용하였습니다. 폴더 구조를 보시면 pages 폴더 안에 각 page 즉, 라우트들이 들어가며, Components 폴더에는 React 컴포넌트들이 들어가게 되는데요. 예를 들어, 브라우저가 실행되고, 사용자가 접속을 하게 되면, 첫 페이지인 index page만 불러오게 되고, 그 이후에 다른 페이지로 넘어갔을 때는 해당 페이지만 불러오게 됩니다.

![](https://miro.medium.com/max/1400/1*rhe9iJeFYfm_mrDxrCuQ5A.png)

#### 간단한 클라이언트 사이드 라우팅 제공

Next.js는 정말 간편한 라우팅을 제공해줍니다.

![](https://miro.medium.com/max/898/1*MqxrBSIXPmNyQ39aNxb2TA.png)

사용방법은 Router와 Link를 모두 import 해서 사용할 수 있는데요. 먼저 Link에서는 href와 as props가 있는데 이 href는 해당 페이지로 이동해 주는 역할을 하고, as는 href의 URL을 조금 더 직관적으로 만들어주는 역할을 해줍니다. Router는 링크과 동일하게 해당 페이지로 이동해주는 역할을 하지만 개발자에게 조금 더 제어권을 넘겨줘서 쉽게 Redirect도 가능합니다.

#### 커스텀 API 서버 (as - 라우트 마스킹)

Next는 커스텀 서버를 통해서 라우트를 마스킹 할 수 있습니다.

![](https://miro.medium.com/max/1400/1*9L2NbzP0ei71-45Wr3fhxA.png)

만약에 Link 컴포넌트에서 as를 사용하게 되면 실제 페이지가 없는 곳에 요청하게 됩니다. 그래서 직접 커스텀 서버를 생성해서 as의 URL이 href를 바라볼 수 있게 처리를 따로 해줘야 새로 고침이나 뒤로 갔을 때도 렌더링이 가능해집니다.

#### getInitialProps()

Next의 핵심기능인 getInitialProps 함수를 통해 데이터를 가져올 수 있습니다. 밑에 코드를 보시면 React의 ComponentDidMount 는 렌더링이 두 번 되지만, Next에서는 데이터를 미리 갖고 오기 때문에 한 번에 렌더링이 가능합니다.

![](https://miro.medium.com/max/1034/1*22dvNdyMsP4rhlqtZODZQQ.png)![](https://miro.medium.com/max/1086/1*Lxgw5ujGVYcT-UyvNe1W5w.png)

Next.js를 사용하면서 느낀 점은 리액트 라우터와 비교했을 때 훨씬 코드스플리팅 구조가 잘되어있다 보니 사용하기가 매우 편했습니다. 또한, SSR의 개념을 정확히 이해하지 않고 프로젝트를 진행하다 보니까 시행착오를 많이 겪에 되었었던 것 같습니다. 하지만 이러한 시행착오 덕분에 SSR 방식과 CSR 방식의 차이점을 조금 더 명확히 이해하였고, 모두 적절히 사용하는게 제일 좋은 방법인 것 같습니다.

## 출처

[moonsj](https://medium.com/@msj9121/next-js-%EC%A0%9C%EB%8C%80%EB%A1%9C-%EC%95%8C%EA%B3%A0-%EC%93%B0%EC%9E%90-8727f76614c9)
