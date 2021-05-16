# shift 와 pop

---

`shift` 와 `pop`은 비슷하지만, 다릅니다.
`shift` 는 첫번째 원소를 배열에서 추출해줍니다. (추출하는 과정에서 배열에서 해당 원소는 사라집니다.)

```
const numbers = [10, 20, 30, 40];
const value = numbers.shift();
console.log(value);
console.log(numbers);
```

결과는 다음과 같습니다.

```
10
[20, 30, 40]
```

이번엔 `pop` 을 해볼까요?

```
const numbers = [10, 20, 30, 40];
const value = numbers.pop();
console.log(value);
console.log(numbers);
```

결과는 다음과 같습니다.

```
40
[10, 20, 30]
```

`pop` 은 `push` 의 반대로 생각하시면 됩니다. `push` 는 배열의 맨 마지막에 새 항목을 추가하고, `pop` 은 맨 마지막 항목을 추출합니다.
