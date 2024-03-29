# continue 문

---

continue 문은 이미 반복문에서 설명한 적이 있습니다. continue 문의 사용법은 다음과 같습니다.

`continue;`

continue 문에도 점프할 라벨을 지정할 수 있는데 사용법은 다음과 같습니다.

`continue 라벨 이름;`

continue 문은 라벨 지정 여부와 관계 없이 반복문 안에서만 사용할 수 있다는 점이 break 문과 다른 점입니다.

continue 문을 실행하면 반복문 실행을 멈추고 반복을 새로 시작합니다. 바로 이때의 동작이 반복문에 따라 달라집니다. 표를 보세요.
|||
|--|--|
|while 문|반복문의 처음으로 되돌아가서 조건식을 다시 평가한다. 그 결과가 true면 반복문을 처음부터 실행한다.|
|do/while 문|중간을 건너뛰고 반복문의 마지막 조건식을 평가한다. 그 결과가 true면 반복문을 처음부터 실행한다.|
|for 문|반복식을 실행한 후에 조건식을 평가한다. 그 결과가 true면 반복문을 이어서 실행한다.|
|for/in 문|반복문의 처음으로 되돌아간다. 지정한 변수에 할당되어 있는 프로퍼티의 다음 프로퍼티를 대상으로 작업을 시작한다.

예를 들어 보겠습니다. 다음 코드는 배열 a 안에서 값이 0 이상인 요소의 값을 모두 더합니다.

```
var a = [2, 5, -1, 7, -3, 6, 9];
for(var i = 0, sum = 0; i < a.length; i++){
    if( a[i] < 0 ) continue;
    sum += a[i];
}
console.log(sum); // -> 29
```

다음은 라벨을 지정한 continue 문을 사용하는 예입니다. 배열 a는 배열을 요소로 갖는 배열입니다. 모든 요소의 값이 10 이하인 배열을 찾고, 각 배열 요소의 평균값을 구한 다음 최대 평균값을 구합니다.
