# HTTP는 무엇일까요?

---

## HTTP란?

**H**yper**T**ext**T**ransfer**P**rotocol의 약자로 하이퍼텍스트 문서를 교환하기 위하여 사용된 통신 규약입니다. 즉, 웹 서버와 클라이언트 간의 통신을 하기 위한 통신 규약입니다. HTTP는 1989년 팀 버너스-리에 의해 처음 설계되어 인터넷을 통한 월드 와이드 웹(World-Wid Web 일명 : www) 기반에서 전 세계적인 정보 공유를 이루는데 큰 역할을 했습니다.  
HTTP는 웹에서만 사용하는 프로토콜로 TCP/IP 기반으로 한 지점에서 다른 지점(서버와 클라이언트)으로 요청과 응답을 전송합니다.

## HTTP의 특징

- HTTP 메시지는 HTTP 서버와 HTTP 클라이언트에 의해서 해석이 됩니다.
- TCP/IP를 이용하는 응용 프로토콜입니다.
- HTTP는 연결 상태를 유지하지 않는 비연결성 프로토콜입니다. (이러한 단점을 해결하기 위해 [Cookie](https://ko.wikipedia.org/wiki/HTTP_%EC%BF%A0%ED%82%A4)와 [Seesion](https://mohwaproject.tistory.com/entry/HTTP-Session-%EC%9D%B4%EB%9E%80) 등장)
- HTTP는 연결을 유지하지 않는 프로토콜이기 때문에 요청/응답(request/response) 방식으로 동작합니다.

기본적으로 아래 그림과 같이 동작합니다.

![HTTP 요청과 응답](https://t1.daumcdn.net/cfile/tistory/99971F345A93FB6D39)

예를 들면, 클라이언트(client) 즉, 사용자가 브라우저를 통해서 어떠한 서비스를 url을 통하거나 다른 것을 통해서 요청(request)을 하면 서버에서는 해당 요청사항에 맞는 결과를 찾아서 사용자에게 응답(response)하는 형태로 동작합니다.

- 요청 : client -> server
- 응답 : server -> client

마지막으로, HTML 문서만이 HTTP 통신을 위한 유일한 정보 문서는 아닙니다.

## 출처

[토마의 개발노트](https://toma0912.tistory.com/69)

<div style="text-align: right">작성일 : 2021.02.08</div>
