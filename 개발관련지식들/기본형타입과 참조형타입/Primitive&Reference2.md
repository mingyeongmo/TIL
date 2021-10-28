# 기본형 타입과 참조형 타입

## 참조형 타입

---

### 참조형 타입(Reference Type)

참조형 타입의 종류는 **객체, 배열, 함수, 날짜, 정규표현식, Map, WeakMap, Set, WeakSet**이 있다.
일반적으로 참조형은 '참조된다'고 알려져있습니다.

#### 참조형 타입의 메모리 저장 방식

#### 예시1) 변수 선언

```javascript
var person = {
  name: "홍길동",
  age: 123,
};
```

![참조형타입 예시 1.변수선언](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FK5UOi%2FbtqPyrBpexM%2FXGsc2SAFN5SG2jY4v7KvB0%2Fimg.png)

기본형타입과 마찬가지로 변수 영역, 데이터 영역이 존재합니다.
식별자 person 변수 영역에 데이터 영역 주솟값을 연결해줍니다.
**하지만 참조형타입은 영역이 하나 더 존재합니다.**
**'객체의 변수(프로퍼티) 영역'** 입니다.

데이터 영역 주솟값에 데이터가 바로 할당되는 것이 아니라 객체의 프로퍼티 영역의 주솟 값(7000~7001)이 연결됩니다.
그리고 해당 객체의 프로퍼티 영역에 데이터 영역의 주솟값을 연결해줍니다.(name-5002 / age-5003)

위에서 데이터 영역은 불변하다고 말했던 것처럼 참조형 데이터도 데이터 영역은 불변하다고 할 수 있습니다.
하지만 기본형 타입은 불변성을 띄고 참조형 타입은 불변하지 않다(가변성)고들 말한다고 하는데,
그 이유를 짚어보겠습니다.

#### 예시2) 참조형타입은 가변성을 띄는가

```javascript
var person = {
  name: "홍길동",
  age: 123,
};

person.name = "고길동";
```

![참조형타입 예시 2.참조형타입은 가변성을 띄는가](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2F0lMeN%2FbtqPC0XybWp%2FIP4JQXRxghu34CwOQpVO2k%2Fimg.png)

식별자 person의 변수 영역의 값과 데이터 영역(5000)의 값이 변경되지 않았습니다.
하지만 객체의 프로퍼티 영역의 데이터 주솟값이 변경된 것이 확인 되는데,
이같은 부분을 보고 가변성을 띈다고 하는 것입니다.

간단히 말해...
person의 데이터 영역은 불변성을 띄지만,
person의 객체의 프로퍼티 영역은 가변성을 띈다고 할 수 있습니다.

#### 예시3) 참조형 타입이 참조인 이유

```javascript
var person = {
  name: "홍길동",
};

var person2 = person;
```

![참조형타입 예시 3.참조형 타입이 참조인 이유1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlYIgq%2FbtqPAuR9o5S%2FlMqtSZkBuO6DFf48Ysxd8K%2Fimg.png)

식별자 person의 값을 person2에 대입하게되는 경우에 위와 같이 같은 데이터 영역의 주솟값을 가지게 됩니다.
이 부분은 기본형 타입과 참조형 타입 둘 다 동일합니다.
두 타입의 차이는 데이터 영역, 객체의 프로퍼티 영역에서 확인할 수 있습니다.
아래와 같이 식별자 person2의 name 프로퍼티의 데이터를 수정하게 될 경우 생기는 변화에 대해 확인해보겠습니다.

```javascript
var person = {
  name: "홍길동",
};

var person2 = person;
person2.name = "고길동";
```

![참조형타입 예시 3.참조형 타입이 참조인 이유2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FYnotk%2FbtqPsEu0RdM%2FcG8CK8gloKwkVK7j439ZYK%2Fimg.png)

변수 영역의 주솟값이나 데이터 영역의 주솟값은 변경되지 않습니다.
name 프로퍼티만 변경된 것이므로 해당 객체의 프로퍼티 영역에서 데이터 주솟값을 변경합니다.
즉, 5002에서 5003으로 변경합니다.
이때 해당 객체의 프로퍼티 영역의 주솟값만 변경하므로 변수 영역, 데이터 영역을 참조하고 똑같이 참조하고 있던 person 역시 영향을 받습니다.

```javascript
var person = {
  name: "홍길동",
};

var person2 = person;
person2.name = "고길동";

console.log(person.name); // '고길동'
```

#### 예시4) 객체끼리 참조하지 못하게 하려면

```javascript
var person = {
    name : '홍길동',
    age : 123
};

var person2 = person;

person2 = {
    name : '홍길동'
    age : 300
};
```

![참조형타입 예시 4.객체끼리 참조하지 못하게 하려면](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbAszKS%2FbtqPDJnLYDm%2FbH53fXkHvc8SNJrMQ5nic0%2Fimg.png)

식별자 person의 객체를 person2에 대입하고나서 새로운 객체로 데이터 자체를 변경해버리면 불변성을 유지할 수 있습니다.
객체 자체를 재 선언해버리면 새로운 객체의 프로퍼티 영역이 생기게 되고 이때 연결된 기존 객체 프로퍼티 영역의 주솟값이 변경되어 버리므로 서로 참조하지 않습니다.
(객체의 프로퍼티영역이 각각 생김)

즉, 참조형 데이터가 가변성읠 띈다라고 말할 수 있는 부분은
참조형데이터 자체를 변경할 때가 아닌 내부 프로퍼티를 변경하는경우에만 성립됩니다.
(참조되는 객체의 불변성을 지키기위해 복사하여 사용하기도합니다.)

하지만 어떠한 데이터 타입이든 변수에 할당하기 위해서는 데이터를 직접 할당하는 것이 아닌 데이터 영역의 주솟값을 복사하기 때문에 자바스크립트의 모든 데이터 타입은 참조형 데이터일 수 밖에 없습니다.
기본형도 결국 주솟값을 참조하는 형태입니다.
다만 기본형은 주솟값을 데이터 영역에서 한번 만 복사해오고 참조형은 객체의 프로퍼티 영역 한단계를 더 거치는 차이가 있습니다.
