# 라벨문

---

자바스크립트에서는 모든 문장에 라벨을 붙일 수 있으며 사용법은 다음과 같습니다.

```
라벨 이름 : 문장
```

**라벨 이름**에는 모든 식별자를 사용할 수 있습니다. 자바스크립트에서 라벨로 점프할 수 있는 문장은 break 문과 continue 문뿐입니다. break 문은 switch 문과 반복문 안에서만 사용할 수 있고 continue 문은 반복문 안에서만 사용할 수 있습니다. 따라서 실제로 라벨을 붙여서 사용할 수 있는 문장은 switch 문과 반복문뿐입니다.

다음 코드에서 break loop;를 실행하면 앞뒤 코드와 상관없이 loop라는 라벨이 붙은 while 문에서 빠져나옵니다.

```
loop: while( true ){
    ...
    if( confirm("종료하시겠습니까?") ) break loop;
    ...
}
```

라벨을 사용하는 구체적인 예제는 break 문과 continue 문을 설명할 떄 함께 설명하겠습니다.
