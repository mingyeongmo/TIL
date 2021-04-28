# Callback 함수

---

## 콜백함수란?

콜백함수는 간단하게 다른 함수에 매개변수로 넘겨준 함수를 말한다.  
매개변수로 넘겨받은 함수는 일단 넘겨받고, 때가 되면 나중에 호출(call back)한다는 것이 콜백함수의 개념이다.

```
function checkGang(count, link, good){
    count < 3 ? link() : good();
}

function linkGang() {
    console.log('1일 3깡은 기본입니다. 아래 링크를 통해 깡을 시청해주세요');
    console.log('https://youtu.be/xqFvYsy4wE4');
}

function goodGang(){
    console.log('오늘 할당량은 모두 채우셨습니다! :)');
}

checkGang(2, linkGang, goodGang);
```

예를 들자면 이런 것이다.  
코드를 살펴보면 checkGang, linkGang, goodGang 총 3가지 함수를 선언하고 checkGang 함수를 호출할 때 매개변수로 count에 숫자값을, 그리고 link와 good에 각각 linkGang과 goodGang함수를 전달했다.

여기서 linkGang함수와 goodGang함수가 콜백함수 인 것이다.

checkGang함수가 **먼저 호출**되고, 매개변수로 들어온 count의 값에 따라 linkGang과 goodGang함수 둘 중 한 가지가 **나중에 호출**된다.

위 코드는 count가 2이기 때문에 linkGang이 실행된다.

## 콜백함수가 필요한 이유?

콜백함수의 이유에 대해 설명을 할 때 변수의 유효범위(scope)에 대한 이야기도 하고 동기/비동기(Synchronous/Asynchronous) 처리에 대한 이야기도 하면 좋을 것이다.

콜백함수는 때로는 그냥 가독성이나 코드 재사용성 면에서도 활용한다.

```
function add(a, b){
    return a + b;
}

function sayResult(value){
    console.log(value);
}

sayResult(add(3, 4));
```

위 코드도 콜백함수를 이용하면 아래와 같이 바꿀 수 있다.

```
function add(a, b, callback){
    callback(a+b);
}

function sayResult(value){
    console.log(value);
}

add(3, 4, sayResult);
```

결과는 같지만, 함수를 호출하는 시점이나 동작하는 순서가 조금식 달라진다.

이렇듯 콜백함수를 처음 접할 때는 어떤 특수한 필요에 의해 작성되어지는 개념 보다는 그저 코드를 작성할 때 다양하게 활용되어지는 하뭇의 표현방식중 하나라고 이해하는 것이 더 좋지 않을까 라는 생각을 한다.
