# map

---

arr.map은 유용성과 사용 빈도가 아주 높은 메서드 중 하나입니다.

`map`은 배열 요소 전체를 대상으로 함수를 호출하고, 함수 호출 결과를 배열로 반환해줍니다.
문법 :

> let result = arr.map(function(item, index, array) {
> // 요소 대신 새로운 값을 반환합니다.
> });

아래 예시에선 각 요소(문자열)의 길이를 출력해줍니다.

> let lengths = ["Bilbo", "Gandalf", "Nazgul"].map(item => item.length)
> alert(lengths); // 5,7,6
