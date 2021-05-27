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

## 정적 메서드와 속성

static 키워드는 클래스를 위한 정적(static) 메서드를 정의합니다. 정적 메서드는 클래스의 인스턴스화 없이 호출되며, 클래스의 인스턴스에서는 호출할 수 없습니다. 정적 메서드는 어플리케이션(application)을 위한 유틸리티(utillity) 함수를 생성하는 데 주로 사용됩니다. 반면, 정적 속성은 캐시, 고정 환경설정 또는 인스턴스 간에 복제할 필요가 없는 기타 데이터에 유용합니다.

```
class Point {
    constructor(x, y) {
        this.x = x;
        this.y = y;
    }

    static displayName = "Point";
    static distance(a, b) {
        const dx = a.x - b.x;
        const dy = a.y - b.y;

        return Math.hypot(dx, dy);
    }
}

const p1 = new Point(5, 5);
const p2 = new Point(10, 10);
p1.displayName; // undefined
p1.distance; // undefined
p2.displayName; // undefined
p2.distance; // undefined

console.log(Point.displayName); // "Point"
console.log(Point.distance(p1, p2)); // 7.0710678118654755
```

## 프로토타입 및 정적 메서드를 사용한 `this` 바인딩

메서드를 변수에 할당 한 다음 호출하는 것과 같이, 정적 메서드나 프로토타입 메서드가 `this`값 없이 호출될 때, `this`값은 메서드 안에서 `undefined`가 됩니다. 이 동작은 `"use strict"` 명령어 없이도 같은 방식으로 동작하는데, `class` 문법 안에 있는 코드는 항상 strict mode로 실행되기 때문입니다.

```
class Animal {
    speak() {
        return this;
    }
    static eat() {
        return this;
    }
}

let obj = new Animal();
obj.speak(); // ths Animal object
let speak = obj.speak;
speak(); // undefined

Animal.eat() // class Animal
let eat = Animal.eat;
eat(); // undefined
```

위의 작성된 코드를 전통적 방식의 함수기반의 non-strict mode 구문으로 재작성하면 `this`메서드 호출은 기본적으로 전역 객체인 초기 `this` 값에 자동으로 바인딩 됩니다. Strict mode에서는 자동 바인딩이 바랫ㅇ하지 않습니다. `this`값은 전달된 대로 유지됩니다.

```
function Animal() { }

Animal.prototype.speak = function() {
    return this;
}

Animal.eat = function() {
    return this;
}

let obj = new Animal();
let speak = obj.speak;
speak(); // global object (in non-strict mode)

let eat = Animal.eat;
eat(); // global object (in non-strict mode)
```

## 인스턴스 속성

인스턴스 속성은 반드시 클래스 메서드 내에 정의되어야 합니다.

```
class Rectangle {
    constructor(height, width) {
        this.height = height;
        this.width = width;
    }
}
```

정적 (클래스사이드)속성과 프로토타입 데이터 속성은 반드시 클래스 선언부 바깥쪽에서 정의되어야 합니다.

> Rectangle.staticWidth = 20;
> Rectangle.prototype.prototypeWidth = 25;
