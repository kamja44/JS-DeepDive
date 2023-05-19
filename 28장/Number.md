1. [Number 생성자 함수](#number-생성자-함수)<br>
   표준 빌트인 객체인 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드를 제공한다.
2. [Number 프로퍼티]<br>
   &nbsp;&nbsp; 2-1. [Number.EPSILON](#numberepsilon)<br>
   &nbsp;&nbsp; 2-2. [Number.MAX_VALUE](#numbermax_value)<br>
   &nbsp;&nbsp; 2-3. [Number.MIN_VALUE](#numbermin_value)<br>
   &nbsp;&nbsp; 2-4. [Number.MAX_SAFE_INTEGER](#numbermax_safe_integer)<br>
   &nbsp;&nbsp; 2-5. [Number.MIN_SAFE_INTEGER](#numbermin_safe_integer)<br>
   &nbsp;&nbsp; 2-6. [Number.POSITIVE_INFINITY](#numberpositive_infinity)<br>
   &nbsp;&nbsp; 2-7. [Number.NEGATIVE_INFINITY](#numbernegative_infinity)<br>
   &nbsp;&nbsp; 2-8. [Number.NaN](#numbernan)<br>

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

## Number 프로퍼티

### Number.EPSILON

1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이이다.

- 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.

Number.EPSILON을 사용하여 부동소수점을 비교하는 함수

```js
function isEqual(a, b) {
  // a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정한다.
  return Math.abs(a - b) < Number.EPSILON;
}
isEqual(0.1 + 0.2, 0.3); // true
```

### Number.MAX_VALUE

javascript에서 표현할 수 있는 가장 큰 양수 값이다.

- MAX_VALUE보다 큰 숫자는 Infinity이다.

```js
Number.MAX_VALUE; // 1.7976931348623157e+308
Infinity > Number.MAX_VALUE; // true
```

### Number.MIN_VALUE

javascript에서 표현할 수 있는 가장 작은 양수 값이다.

- MIN_VALUE보다 작은 숫자는 0이다.

```js
Number.MIN_VALUE; // 5e-324
Number.MIN_VALUE > 0; // true
```

### Number.MAX_SAFE_INTEGER

javascript에서 안전하게 표현할 수 있는 가장 큰 정수값이다.

```js
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

### Number.MIN_SAFE_INTEGER

javascript에서 안전하게 표현할 수 있는 가장 작은 정수값이다.

```js
Number.MIN_SAFE_INTEGER; // -9007199254740991
```

### Number.POSITIVE_INFINITY

양의 무한대를 나타내는 Infinity와 동일하다.

### Number.NEGATIVE_INFINITY

음의 무한대를 나타내는 -Infinity와 동일하다.

### Number.NaN

숫자가 아님을 나타내는 숫자값이다.

- Number.NaN은 window.NaN과 같다.
