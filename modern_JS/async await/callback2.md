# Callback_2

---

아래는 예제이다.

```
function returnName(callback){
    callback("jason");
    console.log("만나서 반갑습니다.");
}

function sayHello(name){
    console.log("안녕하세요 "+name+"씨");
}

returnName(sayHello)
```

간단하게 동작을 살펴보면 returnName을 보면 파라미터에 callback이라는 함수가 선언되어있다.  
returnName함수의 파라미터로 받게되는 함수(여기선 sayHello)는 "jason" 이라는 string을 인자로 받게된다.  
returnName 함수는 sayHello라는 함수를 파라미터로 가지고 호출 되는데, returnName 함수는 sayHello 함수를 인자로 받아야 하기 때문에 sayHello 함수가 **먼저** 실행된 뒤 returnName 함수가 실행되게 된다.

따라서 결과는 아래와 같다.

```
안녕하세요 jason씨
만나서 반갑습니다
```

자 콜백함수에 대한 정의는 이정도로 설명이 되었다.
그러면 JS에서 콜백함수는 왜 중요하게 여겨질까?  
그 이유를 알기위해서는 먼저 동기식 처리 모델과 비동기식 처리 모델에 대하여 알아야한다.

![Task and API](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FlLyIP%2FbtqDbmklcoG%2FKvKsRbyIykg8Z2fcVuBF00%2Fimg.png)

어떠한 Task가 있고 이 Task는 API로 부터 응답을 받아서 동작한다고 해보자  
어떠한 스레드에서 Task를 실행할 때 Task가 API를 호출한 뒤 API로부터 응답이 끝나면 Task는 응답 받은 값으로 해야 할 연산을 마친 뒤 종료하게 될 것이다.

![동기식 처리 모델](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FoBPGK%2FbtqC8IoMMHs%2FE6HeJHhHy8twRqkJD6LPc1%2Fimg.png)

대충 이러한 순서로 일이 진행될 것이다. 이처럼 어떠한 작업이 순차적으로 실행되며 요청에 대한 응답이 올 때 까지 모든 테스크들이 블락 되는 프로세스를 동기식 처리 모델이라고 한다.

![비동기식 처리 모델](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FrVVdY%2FbtqC7rnxurI%2FLZLChLurhStlsc5HxXyuk0%2Fimg.png)

반면에 비동기식 처리 모델은 어떠한 작업이 종료되지 않아도 무한정 대기하지 않고 다른 작업을 실행하다가 API가 응답하면 이벤트 핸들러에 의하여 다시 작업을 재개하게 된다.

자바스크립트는 싱글스레드로 동작하기 때문에 어떤 요청에 대한 응답을 마냥 기다린다면 다른 모든 작업이 밀릴 수 있기 때문에 비동기식으로 처리를 해주어야 한다.

하지만 자바스크립트에서 그냥 소스코드를 작성한다면 아래의 그림처럼 완벽한 비동기 처리가 이루어지기 힘들다. 왜냐면 API로부터 응답을 받기 전에 기존 Task를 종료해버리기 때문이다.

![비동기식 처리 모델의 문제](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPkein%2FbtqC9zkFCCv%2FbaXax6opA5BjhXJxSQgdPK%2Fimg.png)

따라서 이러한 비동기 처리를 문제없이 수행하기 위하여 바로 콜백함수를 이용하게 된다.

우선 다음 예제를 봐보자.

![예제](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FczBeGX%2FbtqC9xUCtMb%2FGMLY57GWZJKWBKNNH6tZJ0%2Fimg.png)

setTimeout 함수에 대하여 간단히 설명하자면 두번째 인자로 설정한 시간[1000ms]이 지난 후 첫번째 인자로 설정한 익명함수를 실행하게 된다.  
우리가 처음에 설계한대로 완벽한 비동기가 이루어지는 경우를 생각해보자.

- 1이 출력된다.
- 1초를 기다린다.
- 2가 출력된다.
- 3이 출력된다.

하지만 앞서 소개한 비동기식 처리 모델의 문제점때문에 다음과 같은 결과를 얻게 된다.

- 1이 출력된다.
- 1초를 기다리는동안 3이 출력된다.
- 2가 출력된다.
  ![예제 실행 결과](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FvKJ3t%2FbtqC7rHRMXD%2FjUkod29Zr5SwJkK3FzEgmK%2Fimg.png)

이제 위의 예제를 콜백함수를 이용하여 비동기 처리를 해주자.  
글의 상단의 처음 콜백함수 예제를 다시 살펴보면 **먼저**라는 글씨에 하이라이팅을 한걸 확인할 수 있다. 이 먼저는 예약의 의미이다.
콜백함수를 이용하면 A라는 작업을 진해앟다가 대기가 발생할 때 대기가 끝나면 다시 A를 진행하도록 예약을 하여 원하는 동작을 수행시킬 수 있다.

![콜백함수를 이용한 예제](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fb0Rw6F%2FbtqC7qPM6bf%2F8RSymonjGgmocUDQKtjEK1%2Fimg.png)

다음과 같이 작성해준다면

- 1이 출력된다.
- InOrderPrint 함수가 익명함수를 파라미터로 실행된다.
- InOrderPrint 함수에 정의 된대로 1000ms 기다린 후 2를 출력한다.
- 이 후 callback() 실행(익명 함수에 작성 된 내용)하여 3을 출력

과 같이 정상적으로 동작한다.

다만 콜백함수를 남용하게 된다면 이른바 콜백 지옥이라는 지저분한 코드를 보게 될 수 있다.
이를 해결하기 위한 방법에는 Promise나 Async가 있으며 콜백함수를 코딩 패턴으로 분리해주는 방법도 있으니 찾아보면 좋을듯 하다.
