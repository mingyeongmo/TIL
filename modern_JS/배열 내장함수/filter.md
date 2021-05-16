# filter

---

`filter` 함수는 배열에서 특정 조건을 만족하는 값들만 따로 추출하여 새로운 배열을 만듭니다.

```
const todos = [
    {
        id: 1,
        text: '자바스크립트 입문',
        done: true
    },
    {
        id: 2,
        text: '함수 배우기',
        done: true
    },
    {
        id: 3,
        text: '객체와 배열 배우기',
        done: true
    },
    {
        id: 4,
        text: '배열 내장함수 배우기',
        done: false
    }
];

const taskNotDone = todos.filter(todo => todo.done === false);
console.log(taskNotDone);
```

결과는 다음과 같습니다.

```
[
    {
        id: 4,
        text: '배열 내장 함수 배우기',
        done: false
    }
];
```

filter 함수에 넣는 파라미터는 조건을 검사하는 함수를 넣어주며, 이 함수의 파라미터로 각 원소의 값을 받아오게 됩니다.

```
const taskNotDone = todos.filter(todo => !todo.done);
```

filter 에 넣어준 함수에서 true 를 반환하면 새로운 배열에 추출을 해주는데요, 만약 todo.done 값이 false 라면, !false 가 되고 이 값은 true 이기 때문에, 이전의 todo.done === false 와 똑같이 작동하게 됩니다.
