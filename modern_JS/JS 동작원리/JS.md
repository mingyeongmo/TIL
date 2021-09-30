# JavaScript 동작원리

---

일단은 아래 코드를 봐보자.

```javascript
console.log(1 + 1);
setTimeout(function () {
  console.log(2 + 2);
}, 1000); // 1초 쉬는코드
console.log(3 + 3);
```

javascript를 아직 입문 하는 사람들이라면 출력 결과를 이렇게 이해할 것이다.

```javascript
2;
4;
// 1초 쉬고
6;
```

그렇지만 실제 출력 결과는

```javascript
2;
6;
// 1초 쉬고
4;
```

이렇게 나온다.

이걸보고 javascript는 처리가 빠른것부터 해주는 느낌도 들고하니깐 병렬처리가 되는 언어다 하는 사람들이 있는데 그것은 틀린말이다.

출력결과가 왜 저렇게 나오는지 동작원리를 배우고 나면 알게 될것이다.

웹 브라우저는 자바스크립트를 실행해주는 애들이다.

이 내부에는 스택이라는 공간이 있는데(하나) 한번에 코드 한줄밖에 실행을 못한다. (single threaded language)

![힙과스택](https://ingg.dev/b5a94511e3d3f116f5f9a01fe9e5aed6/work1.svg)

스택 안에

```javascript
console.log(1 + 1);
setTimeout(function () {
  console.log(2 + 2);
}, 1000);
console.log(3 + 3);
```

이런 코드가 있다고 생각해보자.

setTimeout코드는 바로 실행되지 않으므로 일단 대기실로 재껴두고 2와 6이 찍힌다.

이 대기실로 가는 코드들은 Ajax 요청 코드, 이벤트리스너 그리고 방금 같은 setTimeout 등 이 있다.

아무튼 setTimeout 코드가 1초가 지나면 이 코드는 Queue로 들어가게 되는데 다시말해 대기가 끝난 코드들이 Queue로 들어가게된다는 소리다.

![Queue](https://ingg.dev/213a51586e94904c5075045d2c94273b/work22.svg)

queue에 있는 코드들은 Stack으로 하나씩 올라가게 되는데 이때 Stack은 공간이 텅 비워져있어야 한다.

그렇다면 setTimeout이 1초가 아닌 0초 대기를 시킨다면 2다음 4가 먼저찍힐까?

하지만 직접실행해보면 역시나 똑같이 2 -> 6 -> 4 이렇게 출력이 된다.

왜냐하면 setTimeout을 보면 그냥 대기실로 치워버리기 때문이다.

## 결론

1. Stack을 바쁘게 하지말자.
2. Queu 또한 바쁘게 하지말자.

자바스크립트는 동기적 vs 비동기적?

자바스크립트는 동기적으로 처리됩니다 한번에 한줄 순서대로. 그런데 가끔 비동기적인 처리도 가능합니다. (setTimeout, 이벤트리스너, ajax 함수등등은 재끼고 다음 코드를 실행하기때문. )

### 출처

---

[코딩애플 유튜브](https://www.youtube.com/watch?v=v67LloZ1ieI)
