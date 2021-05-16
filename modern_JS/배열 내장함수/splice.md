# splice

---

splice 는 배열에서 특정 항목을 제거할 때 사용합니다.

```
const numbers = [10, 20, 30, 40];
```

위 배열에서 30 을 지운다고 가정해봅시다. 그러면, 30이 몇번째 index 인지 알아낸 이후, 이를 splice 를 통해 지워줄 수 있습니다.

```
const numbers = [10, 20, 30, 40];
const index = numbers.indexOf(30);
numbers.splice(index, 1);
console.log(numbers);
```

결과는 다음과 같습니다.

```
[10, 20, 40]
```

splice 를 사용 할 때 첫번째 파라미터는 어떤 인덱스부터 지울지를 의미하고 두번째 파라미터는 그 인덱스부터 몇개를 지울지를 의미합니다.
