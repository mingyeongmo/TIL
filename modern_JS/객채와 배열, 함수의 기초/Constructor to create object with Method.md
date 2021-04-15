# 메서드를 가진 객체를 생성하는 생성자

---

생성자에서 **this.프로퍼티 이름**에 함수의 참조를 대입하면 메서드를 정의할 수 있습니다. 이때 메서드 함수 안에 있는 this는 생성될 인스턴스를 가리킵니다.

```
function Circle(center, radius){
    this.center = center;
    this.radius = radius;
    this.area = function(){
        return Math.PI * this.radius * this.radius;
    };
}
var p = {x:0, y:0};
var c = new Circle(p, 2.0);
console.log("넓이 = " + c.area()); // -> 넓이 = 12.566370614359172
```

메서드 함수 안에서 this를 사용하면 그 값이 인스턴스의 프로퍼티임을 명시 할 수 있습니다.
