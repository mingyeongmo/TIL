# Map 함수

---

map 함수는 callbackFunction을 실행한 결과를 가지고 새로운 배열을 만들 때 사용한다.

```
array.map(callbackFunction(currenValue, index, array), thisArg)
```

filter, forEach와 같은 구문이다.

callbackFunction, thisArg 두개의 매개변수가 있고 callbackFunction은 currentValue, index, array 3개의 매개변수를 갖는다.

- currentValue : 배열 내 현재 값
- index : 배열 내 현재 값의 인덱스
- array : 현재 배열
- thisArg : callbackFunction 내에서 this로 사용될 값

```
const days = ["Mon", "Tue", "Wed", "Thus", "Fri"];
```

이러한 변수가 있다고 하자.  
이때 **모든 값에 숫자를 추가 하고** 싶다면 map()함수를 이용하는 것이다. map() 함수는 모든 배열의 값에 Function을 실행하는 Method이다.

즉, days.map(...) 한다는 것은 days에 있는 모든 요일에 Function을 실행하고 Function에서 나온 값을 저장해서 새로운 배열로 만든다는 것이다.  
days.map()이 해당하는 Function에 주는건 Variable(변수)인데 이건 아무거나 가능하다. 예를 들어 day가 될 수도 있다.

```
const smilmingDays = days.map(day => console.log(day));
```

여기서 console.log(day)를 Return 하게 되는 것이다.  
이렇게 되면 day가 배열의 각각의 값을 가지게 된다.  
days.map(...)은 return한 값으로 이루어진 배열을 return한다.

그렇다면 위에서 처럼 console.log()를 리턴하는 대신 string을 return한다면 어떻게 될까?

```
const days = ["Mon", "Tue", "Wed", "Thus", "Fri"];
const smilmingDays = days.map(day => '😁${day}');

console.log(smilmingDays);
```

이렇게 하니까 웃는 이모티콘이 추가된 요일이 배열되고 있다. '😁${day}' 이부분 때문이다. 이 값을 return하고 있는 것이다. array Function은 return 을 함축적으로 가지고 있다. 물론 day라는 변수를 바꿀수도 있다.

다른곳에서 days.map(...)의 Function을 정의하고 싶으면 아래처럼 변수를 또 선언해 주면 된다. 그럼 아래 map()의 내용은 더 간결해 진다.

```
const days = ["Mon", "Tue", "Wed", "Thus", "Fri"];
const addsmile = day => '😃 ${day}';
const smilmingDays = days.map(addsmile);
console.log(smilmingDays);
```

결과는 위 방식과 동일하게 나온다.  
days.map()이 day라는 Argument를 가지고 addsmile Function을 call한다.  
map()함수는 하나의 Argument만 전달하지 않는다. 이외에 다른 많은 Argument도 전달한다. 그 중 하나가 index이다.

```
const days = ["Mon", "Tue", "Wed", "Thus", "Fri"];
const smilmingDays = days.map((day,index) => '#${index}  ${day}');
console.log(smilmingDays);
```

여기서 day는 위 array에 각각의 값일 것이고, 두번째 argument인 index는 현재 있는 숫자들일 것이다. inde의 결과는 Mon의 위치가 [0]인 것을 의미한다.

이런것들을 map()함수에서 할 수 있다. 다시한번 말하자면 map Function은 Function이 준 값으로 이루어진 배열을 return한다. 어떤 값도 return하지 않는다면 어떤 배열도 받지 못할 것이다.

### 여러가지 예시를 들어 array map 함수를 익혀보자.

**1. 간단하게 square root (제곱근)을 구해보자**

```
var numbers= [4, 9, 16, 25, 36];
var result = numbers.map(Math.sqrt);
console.log(result);
```

> result
> [2, 3, 4, 5, 6]

**2. 이번에는 기존 배열에 값의 x2를 한 배열을 생성해 보자.**

다음 세개는 결과가 같다.

```
var numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9 ];
var newNumbers = numbers.map(number => number *2);
console.log(newNumbers);
```

> result
> [2, 4, 6, 8, 10, 12, 14, 16, 18]

```
var numbers = [1, 2, 3, 4, 5, 6, 7, 8, 9 ];
var newNumbers = numbers.map(function(number){
    return number*2; // 이렇게 해도 결과가 위에 것과 똑같다.
});
console.log(newNumbers);
```

> result
> [2, 4, 6, 8, 10, 12, 14, 16, 18]

위에 결과들은 다 같다.  
numbers에 곱하기 2한 값이 newNumbers에 똑같이 나타난다.

> map 함수는 기존의 배열을 callbackFunction에 의해 새 배열을 만드는 함수이다.
> 그러니 기존의 배열이 변하지는 않는다.

**3. map 함수는 filter함수와 같이 Object(객체)타입도 컨트롤 할 수도 있다.**

```
var students = [
    {id:1, name:"james"},{id:2, name:"tim"},
    {id:3, name:"john"},
    {id:4, name:"brian"}
];
```

이러한 Object(객체)타입의 변수가 있다.  
여기서 이름만 추출하고 싶으면 Array map을 이요해서 쉽게 추출할 수 있다.

```
var students = [
    {id:1, name:"james"},
    {id:2, name:"tim"},
    {id:3, name:"john"},
    {id:4, name:"brian"}
];
var names = students.map(student => student.name);
console.log(names);
```

> result
> ['james','tim','john','brian']

result를 보면 이름만 추출된것을 볼 수 있다.  
여기서 student.name을 student.id로 바꾸면 id 값만 추출된다.

**3-2 또다른 Object(객체)를 보자.**

```
var testJson = [
    {name : "이건", salary : 50000000},
    {name : "홍길동", salary : 1000000},
    {name : "임신구", salary : 3000000},
    {name : "이승룡", salary : 2000000}
];

var newJson = testJsonmap(function(element, index){
    console.log(element);
    var returnObj = {}
    returnObj[element.name] = element.salary;
    return returnObj;
});
console.log("newObj");
console.log(newJson);
```

> result
>
> 1. console.log(element);
>    { name: '이건', salary: 50000000 }
>    { name: '홍길동', salary: 1000000 }
>    { name: '임신구', salary: 3000000 }
>    { name: '이승룡', salary: 2000000 }
> 2. console.log(newJson);
>    [{ '이건': 50000000 },{ '홍길동': 1000000 },{ '임신구': 3000000 },{ '이승룡': 2000000 }]

**4. 이번에는 Array를 reverse해보자.**

배열 값에 곱하기 2한 값들을 reverse한다.

```
var numbers = [1,2,3,4,5,6];
var numbersReverse = numbers.map(number => number *2).reverse();
console.log(numbersREverse);
```

> result
> [12, 10, 8, 6, 4, 2]

**5. Array안에 Array가 있는 경우를 보자.**
var numbers = [[1,2,3],[4,5,6],[7,8,9]]; // array안에 array가 있는 경우
var newNumbers = numbers.map(array => array.map(number => number \*2));
console.log(newNumbers);

> result
> [[2,4,6],[8,10,12],[14,16,18]]
