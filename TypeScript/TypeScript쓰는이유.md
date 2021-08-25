# TypeScript를 무서워하지 않아도 되는 이유

---

![TypeScript](https://t1.daumcdn.net/cfile/tistory/2455034257594CC82D)

TypeScript는 컴파일하면 JavaScript가 되는(compile-to-JavaScript) 언어이며, 컴파일 시점에 타입 체크를 하고, 전통적인 객체기반 프로그래밍 패턴을 도입하는 것 이외에도 강력한 기능들을 JavaScript에 추가한다.

그러나 적지 않은 수의 사람들이 TypeScript를 꺼리고 있다. 그도 그럴 것이, TypeScript 는 결국 컴파일 하면 JavaScript 가 되기 때문이다. 우리는 당연히 중간 과정을 필요로 하지는 않는다.

하지만, TypeScript 사용 여부를 떠나서 TypeScript 를 사용하면서 얻게 되는 강력한 이점이 있고, TypeScript에 대해 의심할 필요는 없을 것이다.

## 왜 순수한 JavaScript 를 쓰지 않는가?

컴파일해서 JavaScript 가 되는 언어들에 대한 이야기들이 점점 많아지고 있다. CoffeeScript, TypeScript, ClojureScript, GorillaScript 등등 많은 언어가 있지만 결국은 "왜"의 문제가 나온다. 결국 JavaScript 를 얻기 위해 왜 새 언어의 복잡성이나 새로운 툴체인을 접하는 수고를 할 필요가 있을까? Vanilla JS 를 쓰면 안되나?

많은 이유들로 인해 사람들이 변환 언어를 만들지만 그 중 가장 중요한 이유는 이것들이다.

- 기능의 추가
- 혁신의 자유로움

### 변환 언어와 추가되는 기능들

변환 언어의 한 가지 중요한 장점은 **기능이 추가된다**는 것이다. 이것이 TypeScript 의 가장 중요한 메리트이며 TypeScript는 인터페이스와 추상 클래스, 대수(algebraic) 데이터 타입 등의 기능을 JavaScript 에 더하며, 다른 라이브러리들을 추가적으로 사용하지 않아도 된다.

변환 언어는 순수한 JavaScript 에 새로운 기능을 도입하며 그것들에 대해 자세히 알지 않더라도 인터페이스를 이용하여 사용 할 수 있게 한다. 우리는 심지어 일반적인 JavaScript 를 쓸 때도 이미 이런 과정을 하고 있는데, JavaScript 의 최신 기능을 사용하기 위해 ES6 를 ES5 로 변환하는 트랜스파일러를 사용하는 것과 같은 방식이다. 일부 사람들이 ES6 가 완전히 지원되는 브라우저를 사용하겠지만 대체로 우리와 같이 변환 과정이 필요할 것이다.

### 변환언어는 혁신의 자유로움을 준다.

CoffeeScript 는 JavaScript 기반에 가장 큰 혁신을 가져온 언어일 것이다. CoffeeScript는 명백히 "문법적"이다.

```
span style="font-size: 12pt;"># Ruby-style comments w/ hash, no semicolons, only arrow functions
Person = (name) =>
  # @ indicates 'this'
  @name = name

  @speak = () =>
    console.log @name

  return @

Me = Person('Peleke')
Me.speak()
</span>
```

이 코드는 JavaScript 로 보이지는 않지만 다음과 같은 새로운 문법이 들어있다.

- `@` 는 `this` 를 대신한다. 그러므로 `this.user` 는 `@user` 이다.
- 함수는 `=>` 나 `->` 로 선언되고 function 키워드는 사용하지 않는다.

그리고 CoffeeScript는 `==` 를 허용하지 않고 `===` 를 강제한다. 예외가 되는 케이스는 강제 형변환이나 ` 사이에 JavaScript를 넣어서 사용할 때다.

TypeScript는 완전히 다른 목표를 갖고 있고 CoffeeScript 보다 좀 더 보수적이다. 새로운 문법이나 디자인 패턴을 제안하기 보다는 JavaScript 자체를 시작점으로, 기본에 작은 것을 더하는 것으로 시작했다. 예외사항은 거의 없으며 심지어 vanilla ES6 는 TypeScript 의 문법과 완벽하게 호환된다. 다른 JavaScript로 컴파일 되는 언어들에 비해 좀 더 접근하기 쉬운 이유이다.

변환 언어는 새 버전의 JavaScript 를 이전 버전의 JavaScript 로 변환하는 것보다 좀 더 급진적인 아이디어 이지만, 원리는 비슷하다. 그리고 변환 언어의 가치는 명백하다. 변환 언어는 새로운 방식을 실험, 확장, 탐험하는 데에 원동력이 된다.

## TypeScript 의 장점

TypeScript 는 몇 가지 새로운 기능을 도입한다. 가장 주목할만한 것은 **클래스**와 **인터페이스**, **컴파일타임 타입 체크**이며, 이외에도 많은 기능이 숨어있다.
이들 중 몇가지에 대해서는 이야기해 보겠지만, 잘 알려지지 않은 장점 몇가지는 이런 것들이 있다.

- 익숙함
- 커뮤니티의 지원과 툴체인의 발전
- JavaScript 표준과 상호 호환
