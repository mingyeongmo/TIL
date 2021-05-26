# Class

---

Class는 객체를 생성하기 위한 템플릿입니다. 클래스는 데이터와 이를 조작하는 코드를 하나로 추상화합니다. 자바스크립트에서 클래스는 프로토타입을 이용해서 만들어졌지만 ES5의 클래스 의미와는 다른 문법과 의미를 가집니다.

## Class 정의

Class는 사실 "특별한 함수"입니다. 함수를 함수 표현식과 함수 선언으로 정의할 수 있듯이 class 문법도 class 표현식 and class 선언 두 가지 방법을 제공합니다.

### Class 선언

Class를 정의하는 한 가지 방법은 **class 선언**을 이용하는 것입니다. class를 선언하기 위해서는 클래스의 이름(여기서 "Reactangle")과 함께 `class` 키워드를 사용해야 합니다.

```
class Reactangle {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
}
```

### Hoisting

**함수 선언**과 **클래스 선언**의 중요한 차이점은 함수 선언의 경우 호이스팅이 일어나지만, 클래스 선언은 그렇지 않다는 것입니다. 클래스를 사용하기 위해서는 클래스를 먼저 선언 해야 합니다. 그렇지 않으면, 다음 코드는 ReferenceError를 던질 것입니다.

> const p = new Rectangle(); // ReferenceError
> class Rectangle {}

## Class 표현식

**Class 표현식**은 class를 정의하는 또 다른 방법입니다. Class 표현식은 이름을 가질 수도 있고, 갖지 않을 수도 있습니다.
이름을 가진 class 표현식의 이름은 클래스 body의 local scope에 한해 유효합니다. (하지만, 클래스의 (인스턴스 이름이 아닌)name 속성을 통해 찾을 수 있습니다).

```
// unnamed
let Rectangle = class {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
};
console.log(Rectangle.name);
// 출력: "Rectangle"

// named
let Rectangle = class Rectangle2 {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
};
console.log(Rectangle.name);
// 출력: "Rectangle2"
```

> **참고**: 클래스 표현식에는 Class 선언 섹션에 설명된 것과 동일한 호이스팅 제한이 적용됩니다.

## Class body 와 메서드 정의

Class body는 중괄호 `{}`로 묶여 있는 안쪽 부분입니다. 이곳은 여러분이 메서드나 constructor와 같은 class 멤버를 정의할 곳입니다.

### Strict mode

클래스의 본문(body)은 strict mode에서 실행됩니다. 즉, 여기에 적힌 코드는 성능 향상을 위해 더 엄격한 문법이 적용됩니다. 그렇지 않으면, 조용히 오류가 발생할 수 있습니다. 특정 키워드는 미래의 ECMAScript 버전용으로 예약됩니다.

### Constructor (생성자)

constructor 메서드는 `class`로 생성된 객체를 생성하고 초기화하기 위한 특수한 메서드입니다. "constructor"라는 이름을 가진 특수한 메서드는 클래스 안에 한 개만 존재할 수 있습니다. 만약 클래스에 여러 개의 `constructor` 메서드가 존재하면 `SyntaxError`가 발생할 것입니다.

constructor는 부모 클래스의 constructor를 호출하기 위해 `super` 키워드를 사용할 수 있습니다.

## 프로토타입 메서드

```
class Rectangle {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
    // Getter
    get area() {
        return this.calcArea();
    }
    // 메서드
    calcArea() {
        return this.height * this.width;
    }
}

const square = new Rectangle(10, 10);

console.log(square.area); // 100
```
