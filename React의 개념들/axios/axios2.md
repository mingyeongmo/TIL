# Axios란?

---

Axios는 브라우저, Node.js를 위한 **Promise API를 활용하는 HTTP 비동기 통신 라이브러리이다.** (백엔드와 프론트엔드와 통신을 쉽게하기 위해 AJAX도 더불어 사용하기도 한다.)

## axios 특징

- 운영 환경에 따라 브라우저의 XMLHttpRequest 객체 또는 Node.js의 HTTP API 사용
- Promise(ES6) API 사용
- 요청과 응답 데이터의 변형
- HTTP 요청 취소 및 요청과 응답을 JSON 형태로 자동 변경

## Axios 사용법

- Axios 다운로드
- HTTP Methods
- Axios 사용해보기
  - GET
  - POST
  - PUT
  - DELETE

---

## Axios 다운로드

```
yarn add axios

npm i axios

// 자신이 진행준인 프로젝트 상위에 import 시켜주기
import axios from 'axios'
```

## HTTP Methods

**클라이언트가 웹서버에게 사용자 요청의 목적/종류를 알리는 수단**

이 Method중 Axios 통신하면서 가장 많이 사용되는 메소드를 정리해보았다.

### 1. GET

> GET: 입력한 url에 존재하는 자원에 요청을 한다.

**문법**

```javascript
axios.get(url, [, config]);
```

**GET은 서버에서 어떤 데이터를 가져와서 보여준다거나 하는 용도이다.**
주소에 있는 쿼리스트링을 활용해서 정보를 전달하는 것이지 **GET메서드는 값이나 상태등을 바꿀 수 없다.**

**예제 코드**

```javascript
// 가상으로 보여주는 코드와 response 형태이다. 참고만 하길 바란다.
import axios from "axios";

axios
  .get("https://localgost:3000/sewon/user")
  .then((Response) => {
    console.log(Response.data);
  })
  .catch((Error) => {
    console.log(Error);
  });
```

```
[
    { id: 1, pw: '1234', name: 'sewon' },
    { id: 2, pw: '1234', name: 'hongil' },
    { id: 3, pw: '1234', name: 'daeyeon' }
]
```

응답은 `json` 형태로 넘어온다.

### 2. POST

> POST: 새로운 리소스를 생성(create)할 때 사용한다.

**문법**

```javascript
axios.post(
  "url주소",
  {
    data객체,
  },
  [, config]
);
```

**POST 메서드의 두 번째 인자는 본문으로 보낼 데이터를 설정한 객체 리터럴을 전달한다.**

로그인, 회원가입 등 사용자가 생성한 파일을 서버에다가 업로드할때 사용한다.
**POST를 사용하면 주소창에 쿼리스트링이 남지 않기 때문에 GET보다 안전하다.**

**예제 코드**

```javascript
axios.post( 'url',
  {
   contact: 'Sewon',
   email: 'sewon@gmail.com'
   },
  {
   headers:{
    'Content-type': 'application/json',
    'Accept': 'application/json'
      }
    }
)
  .then((response) => { console.log(response.data); })
  .catch((response) => { console.log('Error!) });
```

### 3. Delete

> REST 기반 API 프로그램에서 데이터베이스에 저장되어 있는 내용을 삭제하는 목적으로 사용한다.

**문법**

```javascript
axios.delete(url, [, config]);
```

Delete메서드는 HTML Form 태그에서 기본적으로 지원하는 HTTP 메서드가 아니다.

Delete메서드는 서버에 있는 데이터베이스의 내용을 삭제하는 것을 주 목적으로 하기에 두 번째 인자를 아예 전달하지 않는다.

**예제 코드**

```javascript
axios.delete("/thisisExample/list/30").then(function(response){
    console.log(response);
}).catch(function(ex){
    throw new Error(ex)
}
```

### 4. PUT

> REST 기반 API 프로그램에서 데이터베이스에 저장되어 있는 내용을 갱신하는 목적으로 사용된다.

**문법**

```javascript
axios.put(url[, data[, config]])
```

PUT메서드는 HTML Form 태그에서 기본적으로 지원하는 HTTP 메서드가 아니다.
PUT메서드는 서버에 있는 데이터베이스의 내용을 변경하는 것을 주 목적으로 하고 있다.
