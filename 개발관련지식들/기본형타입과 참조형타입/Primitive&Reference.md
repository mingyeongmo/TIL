# 기본형 타입과 참조형 타입

## 기본형 타입

---

### 데이터 타입의 종류

| 기본형 타입(Primitive Type) | 참조형 타입(Reference Type) |
| :-------------------------: | :-------------------------: |
|        숫자(Number)         |        객체(Object)         |
|       문자열(String)        |         배열(Array)         |
|       불리언(Boolean)       |       함수(Function)        |
|            null             |         날짜(Date)          |
|          undefined          |     정규표현식(RegExp)      |
|        심볼(Symbol)         |             Map             |
|                             |           WeakMap           |
|                             |             Set             |
|                             |           WeakSet           |

### 기본형 타입(Primitive Type)

기본형 타입의 종류에는 **숫자, 문자열, 불리언, null, undefined, symbol**이 있습니다.
일반적으로 기본형은 '할당이나 연산시 데이터가 복제'된다고 알려져있습니다.

#### 기본형 타입의 메모리 저장방식

메모리 할당 시 두 영역을 사용한다고 생각하면 쉽습니다.

- 식별자가 할당되는 변수 영역
- 데이터 값이 담기는 데이터 영역

변수 영역에는 식별자와 데이터 영역의 주솟값을 이루어져있습니다.
데이터 영역에는 데이터가 담겨있습니다.

#### 예시)1 변수 선언

```javascript
var name = "홍길동";
```

![변수선언](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ft0JCh%2FbtqPsCpqAkm%2FMrdKvsO7GfcqF4t5VSJQV1%2Fimg.png)

변수 영역(1002)에 식별자(변수명)를 name으로 할당합니다.
데이터 영역(5000)에 문자열 '홍길동'데이터를 할당합니다.
식별자 name의 변수 영역에 데이터 영역의 주솟값을 값에 연결해줍니다.

사용자가 식별자 name을 호출했을때,
해당 변수 영역 값에 연결된 주솟값(5000)에 담긴 데이터가 반환됩니다.

#### 예시)2 데이터 재할당

```javascript
var name = "홍길동";
name = "고길동";
```

![데이터 재할당](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FPrkBm%2FbtqPql9kmhc%2F5MY4RYm2aRsl9x3jSKIYkk%2Fimg.png)

새로운 데이터이므로 비어있는 데이터 영역(5001)에 문자열 '고길동'을 할당합니다.
식별자 name에 변수 영역에 해당하는 데이터 영역의 주솟값(@5000 -> @5001)으로 변경해줍니다.

#### 예시3) 변수에 변수를 대입

```javascript
var name = "홍길동";
var name2 = name;
```

![변수에 변수를 대입](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FKRGvu%2FbtqPELNnUD8%2Fa1res8nXKGSPNCESzGywzK%2Fimg.png)

비어있는 변수 영역(1003)에 식별자 name2를 할당하고 name2의 데이터 주소에 name의 주솟값(5000)을 가져와 할당합니다.
name2의 데이터가 변경되는 부분을 이어서보겠습니다.

```javascript
var name = '홍길동';
var name2 = name'
name2 = '고길동';
```

![변수에 변수를 대입2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbiD8Se%2FbtqPIIigVvs%2FW4w2VhvnIceVf0LMDjRPak%2Fimg.png)

새로운 문자열 '고길동'을 비어있는 데이터 영역(5001)에 할당하고 식별자 name2의 주솟값을 변경해줍니다.

데이터 영역에 문자열 '홍길동'을 넣었다가 문자열 '고길동'으로 변경했을때,
해당 영역의 데이터를 수정하지 않고 왜 새로운 메모리 영역을 사용하는가?
이것을 이해하기 위해서는 불변성을 이해해야합니다.

#### 불변성(Immutability)

단순히 정의만을 말하자면 '변하지 않는 성질'이라고 할 수 있습니다.
하지만 **불변성이 해당하는 부분이 어디인지를 확실히 이해해야합니다.**
**불변성은 변수와 상수의 개념으로 말하는 것이 아닙니다.**

**변수와 상수**는 변수 영역 메모리에 데이터 할당 후 재할당이 되는지에 대한 여부로 구분되는 것이며
**불변성**은 데이터 영역의 메모리에 대한 것 입니다.

예시로 간단히 보겠습니다.

```javascript
var name = "홍길동";
name = "고길동";
```

#### 변수

![불변성 예시 1.변수](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbtXhcI%2FbtqPFVgUmv0%2FMqsCRH6jrHwaxy5NyzjkZ1%2Fimg.png)

식별자 name의 변수 영역에 값이 문자열 '홍길동'에서 '고길동'으로 재할당이 되었으므로 해당 데이터 영역 주솟값으로 변경해줍니다.
(5000 -> 5001)
변수는 이처럼 데이터 영역의 주솟값 재할당이 가능합니다. (재할당)

#### 상수

![불변성 예시 2.상수](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Ft6mWk%2FbtqPysG3wIB%2FzNN0JocGHhGv6eLxkKKWK1%2Fimg.png)

상수는 데이터 주솟값 재할당이 안되므로 식별자 name의 변수 영역에 데이터 값 변경이 불가능합니다.

#### 불변성

![불변성 예시 3.불변성1](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2Fqa93H%2FbtqPAtFFUeZ%2FMjUYPslkNjNSLZHvZ54850%2Fimg.png)

불변성은 식별자 변수 영역에 해당하는 데이터 값의 재할당이 기준이 아니고
데이터 영역의 데이터가 변경이 가능한지에 대한 여부입니다.
즉 문자열 '홍길동'에서 '고길동'으로 변경되었을때, 데이터 영역의 데이터는 변경할 수 없습니다.

![불변성 예시 3.불변성2](https://img1.daumcdn.net/thumb/R1280x0/?scode=mtistory2&fname=https%3A%2F%2Fblog.kakaocdn.net%2Fdn%2FbWYhfw%2FbtqPAvkgmNm%2FIskc3qSy13NnEgci1YKle1%2Fimg.png)

불변성에 의해 비어있는 데이터 영역에 새로 문자열 '고길동'을 할당 한 후 식별자 name의 데이터 주솟값이 재할당됩니다.

위의 예시에서 알 수 있듯,
데이터 영역의 데이터는 한번 생성되었을 경우 수정이 안되며(불변성)
새로운 데이터일 경우 비어있는 데이터 영역에 새로 할당됩니다.
그리고 새로 할당된 데이터 영역의 주솟값을 변수 영역의 데이터 주솟값으로 재할당하는 것입니다.
즉, 데이터 영역의 변경이란 새로 만드는 동작에서만 이루어집니다.

**불변성은 왜 필요한거지?라는 의문이 또 한번 머리에 스쳐지나갈 수 있습니다.**

간단하게, 메모리 저장에 대한 이야기를 해보겠습니다.
메모리에 데이터를 저장하기 위해서는 메모리 공간을 선행으로 확보해야합니다.
불변성이 없다고 생각했을때,
처음 저장한 데이터의 크기보다 더 큰 데이터를 '재할당' 해야한다면 어떤일이 생길까요?

> 데이터 공간을 재확보해야하는 일이 생깁니다.
> 그리고 이 재확보작업을 하게되면 뒤에 저장된 메모리들의 공간이 뒤로 밀이는 현상이 생기고
> 이 현상으로 인해 각각의 주솟값들을 식별자에 다시 연결해야하는 작업이 발생할 수 있습니다.
> 위와 같은 이유로 불변성은 효율적으로 데이터를 저장하기 위해 생겼습니다.
