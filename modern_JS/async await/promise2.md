# Promise[ES6]

---

### javascript 의 비동기성

사람은 한 번에 두 가지 일을 할 수 가 없습니다.  
흔히들 말하는 멀티태스킹도 실제로는 한 번에 두 가지 일을 동시에 하는 것이 아니라 빠르게 스위칭해서 동시에 일어나는 것 처럼 보일 뿐입니다.
그에 비해 컴퓨터는 동시에 두 가지 일을 할 수 있습니다. 두 가지 뿐만 아니라 그 이상도 가능하죠.
영상을 녹화하며 충전을 하고 오디오를 녹음하고 사용자로 부터 입력을 받고 시간이 바뀌는 것을 기다리는 것이 가능합니다.
자바스크립트도 이와 같습니다. 동시에 많은 일을 하는게 가능합니다.
한 쪽에서 다른 일을 하고 있어도 프로그램은 계속 실행이 됩니다.

예시를 통해 알아보도록 하겠습니다.
다음과 같이 fetch를 한 후 console.log 를 찍으면 fetch 에서 에러가 발생하기 때문에 끝나야하는데 실제 결과에서는 example 이 찍히고 에러가 발생합니다.
이러한 이유가 발생하는 것이 바로 자바스크립트의 비동기성입니다.
fetch 를 통해 먼저 요청을 보내놓고 그 쪽에서는 일을 진행하고 있지만 프로그램을 계속 실행이 되므로 console.log 를 출력하는 것이죠. 그리고 요청의 결과, 에러가 발생했기 때문에 그 다음에 에러가 나는 것이고요.

```
const example = fetch("http://example.com/"");

console.log("example");
// example
// Uncaught (in promise) TypeError: Failed to fetch
```

### Promise 란?

`Promise` 개체는 비동기 작업이 맞이할 미래의 완료 또는 실패와 그 결과 값을 나타냅니다.

- 매개변수
  우선 `Promise`는 매개변수로 `executor`를 받게됩니다. `executor`는 `resolve`와 `reject`인수를 전달할 실행함수입니다.
  `resolve`와 `reject`는 함수로서 호출되면 `promise`를 이행하거나 거부합니다. 이 둘을 이용하여 비동기 작업이 모두 끝나면 `resolve`를 호출해 이행하고 중간에 오류가 생기면 `reject`를 이용해 거부하게 됩니다.
- 상태
  `Promise`는 3가지 상태를 가질 수 있습니다.
  - pedding: 대기 상태로서 아직 `resolve` 할지 `reject` 할 지 결정되지 않은 초기의 상태입니다.
  - fullfilled : 이행 상태로서 연산이 성공적으로 완료된 상태입니다.
  - rejected : 거부 상태로서 연산이 실패한 상태입니다.
    `Promise`를 사용하게 되면 아직 모르는 값들과 함께 작업을 할 수 있게 됩니다.

`Promise`를 사용해봅시다.
`executor`로 arrow function 을 사용했고 이는 `resolve`와 `reject`, 두 함수를 매개변수로 받습니다.
그 후 결과를 확인하며 아직 어느 것도 결정되지 않았으므로 `pedding` 상태입니다.

```
const promise = new Promise((resolve, reject)=>{});

console.log(promise);
// Promise { <pending>}
```

`promise`는 3초 후에 `resolve`하게 되고 밖에서는 1초마다 `promise`를 실행해보았습니다.
3번째 까지는 `pending` 상태고 그 이후에는 `resolve`의 매개변수로 들어간 string이 출력이 되는 걸 확인할 수 있습니다.

```
const promise = new Promise((resolve, reject)=>{
    setTimeout(()=>{
        resolve("resolve");
    }, 3000);
});

console.log(promise);

setInterval(()=>{
    console.log(promise);
}, 1000);

/*
Promise { <pending> }
Promise { <pending> }
Promise { <pending> }
Promise { 'resolve' }
Promise { 'resolve' }
Promise { 'resolve' }
                            ...
*/
```

### Promise 의 then 과 catch

**then**

`promise`가 종료가 되면 `resolve`에 들어간 값을 받을 수 있습니다.

```
const promise = new Promise((resolve, reject)=>{
    setTimeout(()=>{
        resolve("resolve");
    }, 3000);
});

promise.then(value => console.log(value));
// 3초 후에 결과가 출력
// resolve
```

**catch**

하지만 `reject`된 경우에는 `then`으로 받을 경우, 에러가 발생합니다.

```
const promise = new Promise((resolve, reject)=>{
    setTimeout(()=>{
        reject("reject");
    }, 3000);
});

promise.then(value => console.log(value));
// UnhandledPromiseRejectionWarning: reject
```

이때 `catch`를 사용하여 에러를 잡아줄 수 있습니다.

```
const promise = new Promise((resolve, reject)=>{
    setTimeout(()=>{
        reject("reject");
    }, 3000)
});

promise.then(result => console.log(result)).catch(error => console.log(error));
// 3초 후에 결과가 출력
// reject
```

`catch`가 `then` 뒤에 연결되어있기 때문에 then 이 먼저 실행 후 `catch`가 실행 된다고 생각할 수 있지만 그렇지 않습니다.
둘은 절대 같이 실행 될 수 없습니다. `resolve` 시에는 `then`,`reject`시에는 `catch`가 실행되는 것이죠.

### Chaining Promise

다시 promise 와 then 을 써보면 아래와 같습니다.
결과가 2가 나오는건 별로 어렵지 않습니다.

```
const promise = new Promise((resolve, reject)=>{
    resolve(2);
});

promise.then(result => console.log(result));
// 2
```

하지만 상황에 따라 `promise`를 여러 번 사용해야 하는 경우가 존재합니다. API 로 data 를 받아오기 위해서 `promise` 를 사용하고 받아온 데이터의 암호를 풀기 위해서 `promise` 를 한 번 더 사용할 수 있습니다.
이를 `Chaining Promise` 라고 합니다.
간단한 연산을 예시로 한 번 알아보도록 하겠습니다.

```
const promise = new Promise((resolve, reject)=>{
    resolve(2);
});

promise.then(result => {
    const output = result+1;
    console.log(output);
    return output;
})
.then(result => {
    const output = result+1;
    console.log(output);
    return output;
})
// 3
// 4
```

chain 이 여러개 연결 된 것 처럼 `then`은 원하는 만큼 많이 연결할 수 있습니다.

```
const promise = new Promise((resolve, reject)=>{
    resolve(2);
});

const plusOne = num => num + 1;

promise
.then(plusOne)
.then(plusOne)
.then(plusOne)
.then(plusOne)
.then(result => console.log(result));
// 6
```

그런데 중간의 chain 에서 에러가 발생하면 어떻게 될까요?
이를 해결하기 위해서 각 `then`마다 `catch`를 넣어줘야 할까요?

```
promise
.then(plusOne)
.then(plusOne)
.then(plusOne)
.then(()=> {
    throw Error("error")
})
.then(result => console.log(result));
// UnhandledPromiseRejectionWarning: Error: error
```

다행히도 `catch` 는 한 번의 사용으로 모든 `then` 에 대해서 해결할 수 있습니다.

```
const promise = new Promise((resolve, reject)=>{
    resolve(2);
});

const plusOne = num => num + 1;

promise
.then(plusOne)
.then(plusOne)
.then(plusOne)
.then(()=> {
    throw Error("error")
})
.then(result => console.log(result))
.catch(error => console.log(error));
// Error: error
```

### Promise all

`all()` 은 주어진 모든 `Promise` 를 실행한 후 진행되는 하나의 Promise 를 반환합니다.
예시를 통해 살펴보겠습니다.
3개의 `promise` 에 대해 이들을 array로 `all` 에 넘겨주면 `allPromise` 는 3개의 `promise`가 모두 끝날 때 까지 기다린 후에 이들의 결과값을 array로 반환하게 됩니다.

```
const p1 = new Promise(resolve => {
    setTimeout(resolve, 2000, "First");
})

const p2 = new Promise(resolve => {
    setTimeout(resolve, 1000, "Second");
})

const p3 = new Promise(resolve => {
    setTimeout(resolve, 3000, "Third");
})

const allPromise = Promise.all([p1,p2,p3]);
allPromise.then(values => console.log(values));
// [ 'First', 'Second', 'Third' ]
```

이 중 하나의 `promise` 에서 에러가 발생한다면 어떻게 될까요?
`allPromise` 는 모든 `promise` 에 대해서 결과값을 받아와야 하는데 그러지 못하므로 에러가 발생하게 됩니다.

```
const p1 = new Promise((resolve, reject) => {
    setTimeout(reject, 2000, "First reject");
})

const p2 = new Promise(resolve => {
    setTimeout(resolve, 1000, "Second");
})

const p3 = new Promise(resolve => {
    setTimeout(resolve, 3000, "Third");
})

const allPromise = Promise.all([p1,p2,p3]);
allPromise.then(values => console.log(values));
// UnhandledPromiseRejectionWarning: First reject
```

이 경우 `catch` 는 각 `promise` 에 해줄 필요는 없고, `allPromise` 에 대해서만 해주면 됩니다.

```
const p1 = new Promise((resolve, reject) => {
    setTimeout(reject, 2000, "First reject");
})

const p2 = new Promise(resolve => {
    setTimeout(resolve, 1000, "Second");
})

const p3 = new Promise(resolve => {
    setTimeout(resolve, 3000, "Third");
})

const allPromise = Promise.all([p1,p2,p3]);
allPromise
.then(values => console.log(values))
.catch(error => console.log(error));
// First reject
```

### Promise race

`race()` 는 주어진 `Promise` 중 가장 먼저 완료된 것의 결과값을 이행하거나 거부합니다.
위에서 살펴본 `all` 과 사용하는 방법은 같습니다.
`p1` 은 `reject` 하고 있지만 결과는 Second 가 나옵니다.
`p2` 는 1초만에 끝나기 때문에 그 결과값을 받아 바로 출력해버리는 것이죠.

```
const p1 = new Promise((resolve, reject) => {
    setTimeout(reject, 2000, "First reject");
})

const p2 = new Promise(resolve => {
    setTimeout(resolve, 1000, "Second");
})

const p3 = new Promise(resolve => {
    setTimeout(resolve, 3000, "Third");
})

const allPromise = Promise.race([p1,p2,p3]);
allPromise
.then(values => console.log(values))
.catch(error => console.log(error));
// Second
```

### Promise finally

`finally()` 는 `Promise` 가 `resolve` 되던 `reject` 되던 상관없이 지정된 함수를 실행합니다.
이를 통해 `promise`의 결과에 상관없이 동작을 해야할 때 유용하게 구현할 수 있습니다.
해당 예시가 중간에 에러가 발생하고 이를 `catch` 로 잡아주었어도 `finally` 는 실행되어 end 를 출력합니다.

```
const p1 = new Promise((resolve, reject) => {
    setTimeout(resolve, 2000, "First");
});

p1
.then(result => console.log(result))
.finally(() => console.log("end"));
// First
// end
```

### Promise 활용

포스팅의 도입부에서 `fetch`를 봤었습니다.
이도 해당 url 에 대해 `promise` 객체를 반환하기 때문에 `promise` 처럼 사용할 수 있습니다.

그러면 영화목록을 제공해주는 API 를 통해 데이터를 가져와보도록 하겠습니다.
`fetch` 를 사용하여 API endpoint 에 대해 response 를 확인해보면 아래와 같습니다.

```
fetch("https://yts.lt/api/v2/list_movies.json")
    .then(response => console.log(response))
    .catch(error => console.log(error));

// Response {type: "cors", url: "https://yts.lt/api/v2/list_movies.json", redirected: false, status: 200, ok: true, ...}
```

이는 0과 1로 이루어진 Stream 이기 때문에 읽을 수 있는 정보로 바꾸기 위해서 JSON 으로 바꿔줘야 합니다.
하지만 예상 외의 문제가 발생합니다.
이는 response 가 `promise`를 반환하는 함수를 가지고 있기 때문에 문제가 발생하는 것입니다.

```
fetch("https://yts.lt/api/v2/list_movies.json")
    .then(response => {
        console.log(response.json());
    })
    .catch(error => console.log(error));
// Promise {<pending>}
```

따라서 이를 위해 한 번 더 `then` 을 거치면 원하는 영화목록을 얻을 수 있습니다.

```
fetch("https://yts.lt/api/v2/list_movies.json")
  .then(response => response.json())
  .then(json => console.log(json))
  .catch(error => console.log(error));

/*
{status: "ok", status_message: "Query was successful", data: {…}, @meta: {…}}
status: "ok"
status_message: "Query was successful"
data:
movie_count: 14457
limit: 20
page_number: 1
movies: Array(20)
0: {id: 14848, url: "https://yts.lt/movie/wolf-guy-1975", imdb_code: "tt0202114", title: "Wolf Guy", title_english: "Wolf Guy", …}
1: {id: 14847, url: "https://yts.lt/movie/the-sublet-2015", imdb_code: "tt4285960", title: "The Sublet", title_english: "The Sublet", …}
생략
19: {id: 14829, url: "https://yts.lt/movie/the-story-of-temple-drake-1933", imdb_code: "tt0024617", title: "The Story of Temple Drake", title_english: "The Story of Temple Drake", …}
length: 20
__proto__: Array(0)
__proto__: Object
@meta: {server_time: 1577980301, server_timezone: "CET", api_version: 2, execution_time: "0.01 ms"}
__proto__: Object
*/
```
