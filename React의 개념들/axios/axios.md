# axios란? (feat.Fetch API)

---

# Intro

리액트는 효율적인 UI 구현을 위한 라이브러리이다. `HTTP Client(HTTP 상에서 커뮤니케이션을 하는 자바 기반 컴포넌트)` 를 내장하고 있는 Angular와는 다르게 리액트는 따로 내장 클래스가 존재하지 않는다.

따라서 리액트에서 `AJAX`를 구현하려면 Javascript 내장객체인 **XMLRequest**를 사용하거나, 다른 `HTTP Client` 를 사용해야 한다.

그렇다면 어떤 HTTP Client 라이브러리를 사용하는게 좋을까? 리액트와 함께 쓰면 좋은 HTTP Client 라이브러리가 많지만, 여기에선 리액트에서 많이 쓰이는 것 중에 하나인 `Fetch API` 를 비교하며 **`axios`** 라이브러리를 알아볼 것이다.

> # 짚고 넘어가기
>
> ### AJAX (Asynchronous Javascript And XML)
>
> AJAX란, Javascript의 라이브러리중 하나이며 Asynchronous Javascript And Xml(비동기식 자바스크립트와 xml)의 약자이다. 브라우저가 가지고 있는 XMLHttpRequest 객체를 이용해서 전체 페이지를 새로 고치지 않고도 페이지의 일부만을 위한 데이터를 로드하는 기법 이며,
> **JavaScript를 사용한 비동기 통신, 클라이언트와 서버간에 XML 데이터를 주고받는 기술이다.**
> 정리하자면, **자바스크립트를 통해서 서버에 데이터를 요청하는 것이다.**
>
> ### 비동기 방식이란?
>
> **비동기 방식은 웹페이지를 리로드하지 않고 데이터를 불러오는 방식**이며, Ajax를 통해서 서버에 요청을 한 후 멈추어 있는 것이 아니라 그 프로그램은 계속 돌아간다는 의미를 내포하고 있다.
> ![동기비동기차이](https://media.vlpt.us/images/shin6403/post/5535e38e-e602-40f5-9b5d-bc29bd835ed1/image.png)
>
> ### 비동기 방식의 장점
>
> 페이지 리로드의 경우 전체 리소스를 다시 불러와야 하는데 이미지, 스크립트, 기타 코드등을 모두 재요청할 경우 불필요한 리소스 낭비가 발생하게 되지만 비동기식 방식을 이용할 경우 필요한 부분만 불러와 사용할 수 있으므로 매우 큰 장점이 있다.

## axios VS Fetch API

우리는 일반적으로 자바스크립트에서 API를 연동하기 위해서 보통 `Fetch API` 를 사용하곤 했다.
리액트도 자바스크립트 built-in 라이브러리중 하나인 `Fetch API` 라는 훌륭한 API 연동 모듈을 사용한다.
하지만 `Fetch API`는 자바스크립트의 built-in 라이브러리라는 특성 때문인지 많은 사람들이 리액트에서 `axios` 를 사용하는 것을 선호한다.

![axios 다운로드수 통계](https://media.vlpt.us/images/shin6403/post/ee3250e9-52ed-49b2-89ac-abcbf6df9103/%E1%84%83%E1%85%A1%E1%84%8B%E1%85%AE%E1%86%AB%E1%84%85%E1%85%A9%E1%84%83%E1%85%B3.png)

```
<React 보다 높은 axios의 npm 다운로드 통계>
```

그렇다면 많은 사람들이 왜 `Fetch API` 말고 `axios` 를 더 선호할까?

서로 각 코드를 비교해 보기로 하자.

### Fetch API

```
//fetch
const url ='http://localhost3000/test`
const option ={
   method:'POST',
   header:{
     'Accept':'application/json',
     'Content-Type':'application/json';charset=UTP-8'
  },
  body:JSON.stringify({
  	name:'sewon',
    	age:20
  })

  fetch(url,options)
  	.then(response => console.log(response))
```

### axios

```
// axios
const option ={
  url ='http://localhost3000/test`
   method:'POST',
   header:{
     'Accept':'application/json',
     'Content-Type':'application/json';charset=UTP-8'
  },
  data:{
  	name:'sewon',
    	age:20
  }

  axios(options)
  	.then(response => console.log(response))
```

동일한 기능을 수행하는 코드이며, 간단한 코드이다.

위 코드에서 차이점을 찾아보자.

- `Fetch()` 는 body 프로퍼틸르 사용하고, `axios`는 data 프로퍼티를 사용한다.
- `Fetch`의 url이 `Fetch()` 함수의 인자로 들어가고, `axios` 에서는 url이 `option` 객체로 들어간다.
- `Fetch` 에서 body 부분은 stringify()로 되어진다.

> 이처럼 axios는 HTTP 통신의 요구사항을 컴팩트한 패키지로써 사용하기 쉽게 설계 되었다.

위와 같은 내용들을 보며, 이제 우리가 왜 axios를 배워야 하는지 알게 되었을거라 생각했고, 이제 진짜 axios에 대해 알아보도록 하자.
