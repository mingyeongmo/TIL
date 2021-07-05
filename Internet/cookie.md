# 쿠키

---

쿠키와 세션을 이해하기 위해서는 먼저 http의 특징에 대해 이해하면 도움이 됩니다.

![HTTP](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fcyq6JL%2FbtqBOq9wy80%2F2pS23aufFvinXLh5swaeeK%2Fimg.png)

**HTTP**(Hypertext Transfer Protocol)는 인터넷상에서 데이터를 주고 받기 위해 서버/클라이언트 모델을 따르는 통신규약을 말합니다. 이 HTTP 프로토콜에는 비연결성(Connectionless)과 비상태성(Stateless)이라는 특징이 있습니다.
이는 서버의 자원을 절약하기 위해 모든 사용자의 요청마다 연결과 해제의 과정을 거치기 때문에 연결 상태가 유지되지 않고, 연결 해제 후에 상태 정보가 저장되지 않는다는 것입니다.

하지만, 이로 인해 사용자를 식별할 수 없어서 같은 사용자가 요청을 여러번 하더라도 매번 새로운 사용자로 인식하는 단점이 있습니다.

하지만 우리가 사용하고 있는 웹사이트를 생각해보면 로그인을 한 번 하고나면 그 사이트에서는 다시 로그인할 필요 없이 여러 페이지의 기능들을 이용할 수 있고 심지어 브라우저를 종료했다가 나중에 다지 접속했을 때도 그 로그인 상태를 유지할 수도 있습니다.

이렇게 HTTP의 비연결성과 비상태성을 보완하여 서버가 클라이언트를 식별하게 해주는 것이 쿠키와 세션입니다.

## 📌 쿠키의 개념

쿠키는 웹 사이트에 접속할 때 **생성되는 정보를 담은 임시 파일**
쿠키는 서버가 사용자의 웹 브라우저에 저장하는 데이터를 말합니다.
쿠키의 데이터 형태는 Key 와 Value로 구성되고 String 형태로 이루어져 있습니다.

> 브라우저마다 저장되는 쿠키는 다릅니다. (크롬으로 남긴 쿠키는 인터넷 익스플로어에서 사용할 수 없습니다.)
> 서버에서는 브라우저가 다르면 다른 사용자로 인식합니다.

쿠키는 서버를 대신해서 이러한 정보들을 웹 브라우저에서 저장 (정확히는, 웹 브라우저를 이용하고 있는 컴퓨터에 저장)하고 사용자가 요청을 할 때 그 정보를 함께 보내서 서버가 사용자를 식별할 수 있게 해줍니다.

![쿠키](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbZ0rtI%2FbtqBOrN7Fwx%2FaIwD2Kd3vsOGBWqQfKDzY1%2Fimg.png)

## 📌 쿠키의 사용목적

쿠키는 주로 아래의 세 가지 목적을 위해 사용됩니다.

1. **세션 관리**(Session Management) 로그인, 사용자 닉네임, 접속 시간 장바구니 등의 서버가 알아야 할 정보들을 저장합니다.
2. **개인화**(Personalization) 사용자마다 다르게 그 사람에 적절한 페이지를 보여줄 수 있습니다.
3. **트래킹**(Tracking) 사용자의 행동과 패턴을 분석하고 기록합니다.

## 📌 쿠키가 사용되는 예시

쿠키가 있기 때문에 여러 페이지를 이동할 때마다 로그인을 하지 않고 사용자 정보를 유지할 수 있는 것입니다.
쿠키가 없다면 다음 페이지로 정보를 파라미터로 넘겨줘야 합니다.

- ID 저장, 로그인 상태 유지
- 일주일간 다시 보지 않기
- 최근 검색한 상품들을 광고에서 추천
- 쇼핑몰 장바구니 기능

## 📌 쿠키의 단점

1. 방문했던 웹 사이트에 대한 정보 및 개인정보가 기록되기 때문에 사생활을 침해할 소지가 있으며, 이를 해소하기 위해서 웹 브라우저 자체에 쿠키 거부 기능이 있습니다. 이러한 쿠키에 대한 거부가 웹 브라우저에 설정되어 있으면, 쿠키 본래의 목적인 웹 브라우저와의 연결을 지속시키는 기능을 수행할 수 없는 경우가 발생합니다.

2. 서버가 가지고 있는 것이 아니라 사용자에게 저장되기 때문에, 임의로 고치거나 지울 수 있고, 가로채기도 쉬워 보안이 취약합니다. 따라서 , 쿠키에는 민감하거나 중요한 정보를 담는 것은 위험합니다.

그래서 이런 단점을 보완해주는 것이 세션입니다.