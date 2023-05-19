1. [Number 생성자 함수](#number-생성자-함수)<br>
   표준 빌트인 객체인 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드를 제공한다.

## Number 생성자 함수

Number 객체는 생성자 함수 객체다.

- new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.

Number 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 0을할당한 Number 래퍼 객체를 생성한다.

```js
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```

[[PrimitiveValue]]는 [[NumberData]]와 동일하다.

Number 생성자 함수의 인수로 숫자를 전달하면서 new 연산자와 함께 호출하면 [[NumberData]] 내부 슬롯에 인수로 전달받은 숫자를 할당한 Number 래퍼 객체를 생성한다.

```js
const numObj = new Number(10);
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
```

전달받은 인수를 숫자로 변환할 수 없다면 NaN을 [[NumberData]] 내부 슬롯에 할당한 Number 래퍼 객체를 생성한다.

- new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 바노한한다.
  - 이를 이용하여 명시적 타입 변환이 가능하다.

```js
let numObj = new Number("10");
console.log(numObj); // Number {[[PrimitiveValue]]: 10}
numObj = new Number("Hello");
console.log(numObj); // Number {[[PrimitiveValue]]: NaN}

// 문자열 => 숫자 변환
Number("0"); // 0
Number("-1"); // -1

// 불리언 => 숫자
Number(true); // 1
```
