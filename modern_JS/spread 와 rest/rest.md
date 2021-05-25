# rest

---

rest는 생김새는 spread 랑 비슷한데, 역할이 매우 다릅니다.

rest는 객체, 배열, 그리고 함수의 파라미터에서 사용이 가능합니다.

### 객체에서의 rest

우선 객체에서의 예시를 알아볼까요?

```
const purpleCuteSlime = {
    name: '슬라임',
    attribute: 'cute,
    color: 'purple'
};

const { color, ...rest } = purpleCuteSlime;
console.log(color);
console.log(rest);
```

![rest1](https://i.imgur.com/XYg74q3.png)

rest 안에 color 값을 제외한 값이 들어있습니다.

rest 는 객체와 배열에서 사용 할 때는 이렇게 비구조화 할당 문법과 함께 사용됩니다. 주로 사용 할때는 위와 같이 rest 라는 키워드를 사용하게 되는데요, 추출한 값의 이름이 꼭 rest 일 필요는 없습니다.

```
const purpleCuteSlime = {
    name: '슬라임',
    attribute: 'cute',
    color: 'purple'
};

const { color, ...cuteSlime } = purpleCuteSlime;
console.log(color);
console.log(cuteSlime);
```

이렇게 해도 무방합니다.

이어서, attribute 까지 없앤 새로운 객체를 만들고 싶다면 이렇게 해주면 되겠죠.

```
const purpleCuteSlime = {
    name: '슬라임',
    attribute: 'cute',
    color: 'purple'
};

const { color, ...cuteSlime } = purpleCuteSlime;
console.log(color);
console.log(cuteSlime);

const { attribute, ...slime } = cuteSlime;
console.log(attribute);
console.log(slime);
```

![rest2](https://i.imgur.com/ejvAHKJ.png)

### 배열에서의 rest

다음, 배열에서의 사용 예시를 알아봅시다.

```
const numbers = [0, 1, 2, 3, 4, 5, 6];

const [one, ...rest] = numbers;

console.log(one);
console.log(rest);
```

![rest3](https://i.imgur.com/tEpTlMQ.png)

배열 비구조화 할당을 통하여 원하는 값을 밖으로 꺼내고, 나머지 값을 rest 안에 넣었습니다.

반면 이렇게 할 수는 없답니다.

```
const numbers = [0, 1, 2, 3, 4, 5, 6];

const [...rest, last] = numbers;
```

![rest4](https://i.imgur.com/E9dyzir.png)

### 함수 파라미터에서의 rest

rest 를 함수파라미터에서 사용 할 수도 있습니다. 예를 들어서 우리가 파라미터로 넣어준 모든 값들을 합해주는 함수를 만들어주고 싶다고 가정해봅시다.

```
function sum(a, b, c, d, e, f, g) {
    let sum = 0;
    if (a) sum += a;
    if (b) sum += b;
    if (c) sum += c;
    if (d) sum += d;
    if (e) sum += e;
    if (f) sum += f;
    if (g) sum += g;
    return sum;
}

const result = sum(1, 2, 3, 4, 5, 6);
console.log(result);
```

위에서의 sum 함수는 7개의 파라미터를 받아오는데, 아래에서 사용 할때에는 6개만 넣어줬습니다. 그러면, g 값이 undefined 가 되기 때문에 sum 에 더하는 과정에서 += undefined 를 하게 되면 결과는 NaN 이 되버립니다. 그렇기 때문에 함수에서 하나하나 유효한 값인지 확인을 해줬지요.

함수의 파라미터가 몇개가 될 지 모르는 상황에서 rest 파라미터를 사용하면 매우 유용합니다. 코드를 다음과 같이 수정해 보세요.

```
function sum(...rest) {
     return rest;
}

const result = sum(1, 2, 3, 4, 5, 6);
console.log(result);
```

![rest5](https://i.imgur.com/Pvm0tha.png)

result 가 가르키고 있는 것은 함수에서 받아온 파라미터들로 이루어진 배열입니다. 우리가 이제 파라미터들이 들어가있는 배열을 받았으니, 그냥 모두 더해주면 되겠죠?

```
function sum(...rest) {
    return rest.reduce((acc, current) => acc + current, 0);
}

const result = sum(1, 2, 3, 4, 5, 6);
console.log(result); // 21
```

### 함수 인자와 spread

이번에는, 다시 아까 배웠던 spread 로 돌아와서 한가지를 더 가르쳐드리겠습니다. 바로 함수의 인자와 spread 인데요,
만약 프로그래밍을 처음 배우신다면 파라미터와 인자가 좀 헷갈릴 수 있습니다. 이에 대해서 간단하게 설명 드려볼게요.

```
const myFunction(a) { // 여기서 a 는 파라미터
    console.log(a); // 여기서 a 는 인자
}

myFunction('hello world'); // 여기서 'hello world' 는 인자
```

함수에서 값을 읽을때, 그 값들은 파라미터라고 부릅니다. 그리고 함수에서 값을 넣어줄 때, 그 값들은 인자라고 부릅니다.

인자가 무엇인지 이해를 하셨다면 이제 함수인자와 spread 문법을 사용하는 것에 대하여 알아볼게요.

우리가 방금 함수파라미터와 rest 를 사용한 것과 비슷한데, 반대의 역할입니다. 예를 들어서, 우리가 배열 안에 있는 원소들을 모두 파라미터로 넣어주고 싶다고 가정해보겠습니다.

```
function sum(...rest) {
    return rest.reduce((acc, current) => acc + current, 0);
}

const numbers = [1, 2, 3, 4, 5, 6];
const result = sum(
    numbers[0],
    numbers[1],
    numbers[2],
    numbers[3],
    numbers[4],
    numbers[5]
);
console.log(result);
```

굉장히 불편하죠? 만약에 sum 함수를 사용 할 때 인자 부분에서 spread 를 사용하면 다음과 같이 표현이 가능합니다.

```
function sum(...rest) {
    return rest.reduce((acc, current) => acc + current, 0);
}

const numbers = [1, 2, 3, 4, 5, 6];
const result = sum(...numbers);
console.log(result);
```

어떤가요? 정말 편하죠?

이렇게, spread 와 rest 를 잘 사용하면 앞으로 보기 깔끔한 코드를 작성하는 것에 큰 도움을 줄 것입니다.

### 퀴즈

함수에 n 개의 숫자들이 파라미터로 주어졌을 때, 그 중 가장 큰 값을 알아내세요.

```
function max() {
    return 0;
}

const result = max(1, 2, 3, 4, 10, 5, 6, 7);
console.log(result);
```

### 정답

```
function max(...numbers) {
    return numbers.reduce(
        // acc 이 current 보다 크면 결과값을 current 로 하고
        // 그렇지 않으면 acc 가 결과값
        (acc, current) => (current > acc ? current : acc),
        numbers[0]
    );
}

const result = max(1, 2, 3, 4, 10, 5, 6, 7);
console.log(result);

// 테스트 코드에서 불러오기 위하여 사용하는 코드
export default max;
```
