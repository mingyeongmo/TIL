# Promise

---

## Promise가 뭔가요?

#### "A promise is an object that may produce a single value some time in the future"

프로미스는 자바스크립트 비동기 처리에 사용되는 객체입니다. 여기서 자바스크립트의 비동기 처리란 **'특정 코드의 실행이 완료될 때까지 기다리지 않고 다음 코드를 먼저 수행하는 자바스크립트의 특성'**을 의미합니다.

## Promise가 왜 필요한가요?

프로미스는 주로 서버에서 받아온 데이터를 화면에 표시할 때 사용합니다. 일반적으로 웹 애플리케이션을 구현할 때 서버에서 데이터를 요청하고 받아오기 위해 아래와 같은 API를 사용합니다.

```
$.get('url 주소/products/1', function(response){
    // ...
});
```

위 API가 실행되면 서버에다가 '데이터 하나 보내주세요'라는 요청을 보내죠. 그런데 여기서 데이터를 받아오기도 전에 마치 데이터를 다 받아온 것 마냥 화면에 데이터를 표시하려고 하면 오류가 발생하거나 빈 화면이 뜹니다. 이와 같은 문제점을 해결하기 위한 방법 중 하나가 프로미스입니다.

## 프로미스 코드 - 기초

그럼 프로미스가 어떻게 동작하는지 이해하기 위해 예제 코드를 살펴보겠습니다. 먼저 아래 코드는 간단한 ajax 통신 코드입니다.

```
function getData(callbackFunc){
    $.get('url 주소/products/1', function(response){
        callbackFunc(response); // 서버에서 받은 데이터 response를 callbackFunc() 함수에 넘겨줌
    });
}

getData(function(tableData){
    console.log(tableData); // $.get()의 response 값이 tableData에 전달됨
});
```

위 코드는 제이쿼리의 **_ajax 통신 API_**를 이용하여 지정된 url에서 1번 상품 데이터를 받아오는 코드입니다. 비동기 처리를 위해 프로미스 대신에 콜백 함수를 사용했습니다.

위 코드에 프로미스를 적용하면 아래와 같은 코드가 됩니다.

```
function getData(callback){
    // new Promise() 추가
    return new Promise(function(resolve, reject){
        $.get('url 주소/products/1', function(response){
            // 데이터를 받으면 resolve() 호출
            resolve(response);
        });
    });
}
```

콜백 함수로 처리하던 구조에서 `new Promise()`, `resolve()`, `then()`와 같은 프로미스 API를 사용한 구조로 바뀌었습니다. 여기서 `new Promise()`는 좀 이해가 가겠는데 `resolve()`, `then()은 뭐 하는 애들일까요? 아래에서 함께 알아보겠습니다.

## 프로미스의 3가지 상태(states)

프로미스를 사용할 때 알아야 하는 가장 기본적인 개념이 바로 프로미스의 상태(states)입니다. 여기서 말하는 상태란 프로미스의 처리 과정을 의미합니다. `new Promise()로 프로미스를 생성하고 종료될 때까지 3가지 상태를 갖습니다.

- Pending(대기) : 비동기 처리 로직이 아직 완료되지 않은 상태
- Fulfilled(이행) : 비동기 처리가 완료되어 프로미스가 결과 값을 반환해준 상태
- Rejected(실패) : 비동기 처리가 실패하거나 오류가 발생한 상태

### Pending(대기)

먼저 아래와 같이 `new Promise()` 메서드를 호출하면 대기(Pending) 상태가 됩니다.

```
new Promise();
```

`new Promise()` 메서드를 호출할 때 콜백 함수를 선언할 수 있고, 콜백 함수의 인자는 `resolve`, `reject`입니다.

```
new Promise(function(resolve, reject){
    // ...
});
```

### Fulfilled(이행)

여기서 콜백 함수의 인자 `resolve`를 아래와 같이 실행하면 이행(Fulfilled) 상태가 됩니다.

```
new Promise(function(resolve, reject){
    resolve();
});
```

그리고 이행 상태가 되면 아래와 같이 `then()`을 이용하여 처리 결과 값을 받을 수 있습니다.

```
function getData(){
    return new Promise(function(resolve, reject){
        var data = 100;
        resolve(data);
    });
}

// resolve()의 결과 값 data를 resolvedData로 받음
getData().then(function(resolvedData){
    console.log(resolvedData); //100
});
```

> 프로미스의 '이행' 상태를 좀 다르게 표현해보면 '완료'입니다.

### Rejected(실패)

`new Promise()`로 프로미스 객체를 생성하면 콜백 함수 인자로 `resolve`와 `reject`를 사용할 수 있다고 했습니다. 여기서 `reject`를 아래와 같이 호출하면 실패(Rejected) 상태가 됩니다.

```
new Promise(function(resolve, reject){
    reject();
});
```

그리고, 실패 상태가 되면 실패한 이유(실패 처리의 결과 값)를 `catch()`로 받을 수 있습니다.

```
function getData(){
return new Promise(function(resolve, reject){
reject(new Error("Request is failed"));
});
}

// reject()의 결과 값 Error를 err에 받음
getData().then().catch(function(err){
console.log(err); // Error: Request is failed
});
```

![프로미스 처리 흐름](https://joshua1988.github.io/images/posts/web/javascript/promise.svg)

## 프로미스 코드 - 쉬운 예제

그럼 위에서 배운 내용들을 종합하여 간단한 프로미스 코들르 만들어보겠습니다. 이해하기 쉽게 앞에서 살펴본 ajax 통신 예제 코드에 프로미스를 적용해보겠습니다.

```
function getData(){
    return new Promise(function(resolve, reject){
        $.get('url 주소/products/1', function(response){
            if (response){
                resolve(response);
            }
            reject(new Error("Request is failed"));
        });
    });
}

// 위 $.get() 호출 결과에 따라 'response' 또는 'Error' 출력
getData().then(function(data){
    console.log(data); // response 값 출력
}).catch(function(err){
    console.error(err); // Error 출력
});
```

위 코드는 서버에서 제대로 응답을 받아오면 `resolve()` 메서드를 호출하고, 응답이 없으면 `reject()` 메서드를 호출하는 예제입니다. 호출된 메서드에 따라 `then()`이나 `catch()`로 분기하여 응답 결과 또는 오류를 출력합니다.

## 여러 개의 프로미스 연결하기 (Promise Chaining)

프로미스의 또 다른 특징은 여러 개의 프로미스를 연결하여 사용할 수 있다는 점입니다. 앞 예제에서 `then()`메서드를 호출하고 나면 새로운 프로미슥 개체가 반환됩니다. 따라서, 아래와 같이 코딩이 가능합니다.

```
function getData(){
    return new Promise({
        // ...
    });
}

// then() 으로 여러 개의 프로미스를 연결한 형식
getData()
    .then(function(data){
        // ...
    })
    .then(function(){
        // ...
    })
    .then(function(){
        // ...
    });
```

그러면 위의 형식을 참고하여 실제로 돌려볼 수 있는 에제를 살펴보겠습니다. 비동기 처리 예제에서 가장 흔하게 사용되는 setTimeout() API를 사용하였습니다.

```
new Promise(function(resolve, reject){
  setTimeout(function() {
    resolve(1);
  }, 2000);
})
.then(function(result) {
  console.log(result); // 1
  return result + 10;
})
.then(function(result) {
  console.log(result); // 11
  return result + 20;
})
.then(function(result) {
  console.log(result); // 31
});
```

위 코드는 프로미스 객체를 하나 생성하고 `setTimeout()`을 이용해 2초 후에 `resolve()`를 호출하는 예제입니다.

`resolve()`가 호출되면 프로미스가 대기 상태에서 이행 상태로 넘어가기 때문에 첫 번째 `.then()`의 로직으로 넘어갑니다. 첫번째 `.then()`에서는 이행된 결과 값 1을 받아서 10을 더한 후 그다음 `.then()`으로 넘겨줍니다.  
두 번째 `.then()`에서도 마찬가지로 바로 이전 프로미스의 결과 값 11을 받아서 20을 더하고 다음 `.then()`으로 넘겨줍니다. 마지막 `.then()`에서 최종 결과 값 31을 출력합니다.

## 실무에서 있을 법한 프로미스 연결 사례

실제 웹 서비스에서 있을 법한 사용자 로그인 인증 로직에 프로미스를 여러 개 연결해보겠습니다.

```
getData(userInfo)
    .then(parseValue)
    .then(auth)
    .then(display);
```

위 코드는 페이지에 입력된 사용자 정보를 받아와 파싱, 인증 등의 작업을 거치는 코드를 나타내었습니다. 여기서 `userInfo`는 사용자 정보가 담긴 객체를 읨하ㅗㄱ, `parseValue`,`auth`,`display`는 각각 프로미스를 반환해주는 함수라고 가정했습니다. 아래와 같이 말이죠.

```
var userInfo = {
  id: 'test@abc.com',
  pw: '****'
};

function parseValue() {
  return new Promise({
    // ...
  });
}
function auth() {
  return new Promise({
    // ...
  });
}
function display() {
  return new Promise({
    // ...
  });
}
```

이처럼 여러 개의 프로미스를 `.then()`으로 연결하여 처리할 수 있습니다.

## 프로미스의 에러 처리 방법

앞에서 살펴본 프로미스 예제는 코드가 항상 정상적으로 동작한다고 가정하고 구현한 예제입니다. 실제 서비스를 구현하다 보면 네트워크 연결 서버 문제 등으로 인해 오류가 발생할 수 있습니다. 따라서, 프로미스의 에러 처리 방법에 대해서도 알고 있어야 합니다.

에러 처리 방법에는 다음과 같이 2가지 방법이 있습니다.

1.`then()`의 두 번째 인자로 에러를 처리하는 방법

```
getData().then(
    handleSuccess,
    handleError
);
```

2.`catch()`를 이용하는 방법

```
getData().then().catch();
```

위 2가지 방법 모두 프로미스의 `reject()`메서드가 호출되어 실패 상태가 된 경우에 실행됩니다. 간단하게 말해서 프로미스의 로직이 정상적으로 돌아가지 않는 경우 호출되는 거죠. 아래와 같이 말입니다.

```
function getData() {
  return new Promise(function(resolve, reject) {
    reject('failed');
  });
}

// 1. then()의 두 번째 인자로 에러를 처리하는 코드
getData().then(function() {
  // ...
}, function(err) {
  console.log(err);
});

// 2. catch()로 에러를 처리하는 코드
getData().then().catch(function(err) {
  console.log(err);
});
```

## 프로미스 에러 처리는 가급적 catch()를 사용

앞에서 프로미스 에러 처리 방법 2가지를 살펴봤습니다. 개개인의 코딩 스타일에 따라서 `then()`의 두 번째 인자로 처리할 수도 있고 `catch()`로 처리할 수도 있겠지만 가급적 `catch()`로 에러를 처리하는 게 더 효율적입니다.

그 이유는 아래의 코드를 보시면 알 수 있습니다.

```
// then()의 두 번째 인자로는 감지하지 못하는 오류
function getData() {
  return new Promise(function(resolve, reject) {
    resolve('hi');
  });
}

getData().then(function(result) {
  console.log(result);
  throw new Error("Error in then()"); // Uncaught (in promise) Error: Error in then()
}, function(err) {
  console.log('then error : ', err);
});
```

`getData()` 함수의 프로미스에서 `resolve()` 메서드를 호출하여 정상적으로 로직을 처리했지만, `then()`의 첫번째 콜백 함수 내부에서 오류가 나는 경우 제대로 잡아내지 못합니다. 따라서 코드를 실행하면 아래와 같은 오류가 납니다.

![에러를 잡지 못했습니다.](https://joshua1988.github.io/images/posts/web/javascript/then-not-handling-error.png)

하지만 똑같은 오류를 `catch()`로 처리하면 다른 결과가 나옵니다.

```
// catch()로 오류를 감지하는 코드
function getData() {
  return new Promise(function(resolve, reject) {
    resolve('hi');
  });
}

getData().then(function(result) {
  console.log(result); // hi
  throw new Error("Error in then()");
}).catch(function(err) {
  console.log('then error : ', err); // then error :  Error: Error in then()
});
```

위 코드의 처리 결과는 다음과 같습니다.

![발생한 에러를 성공적으로 콘솔에 출력한 모습](https://joshua1988.github.io/images/posts/web/javascript/catch-handling-error.png)

**따라서, 더 많은 예외 처리 상황을 위해 프로미스의 끝에 가급적 catch()를 붙이시기 바랍니다.**

## 마무리

현대 웹 앱의 특성상 앞으로도 프로미스는 더 많이 사용될 것 같습니다. 숙련된 웹 개발자가 되기 위해 위 개념과 사용법을 꼭 숙지하시기 바랍니다 :)
