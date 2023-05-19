1. [Number 생성자 함수](#number-생성자-함수)<br>
   표준 빌트인 객체인 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드를 제공한다.
2. [Number 프로퍼티](#number-프로퍼티)<br>
   &nbsp;&nbsp; 2-1. [Number.EPSILON](#numberepsilon)<br>
   &nbsp;&nbsp; 2-2. [Number.MAX_VALUE](#numbermax_value)<br>
   &nbsp;&nbsp; 2-3. [Number.MIN_VALUE](#numbermin_value)<br>
   &nbsp;&nbsp; 2-4. [Number.MAX_SAFE_INTEGER](#numbermax_safe_integer)<br>
   &nbsp;&nbsp; 2-5. [Number.MIN_SAFE_INTEGER](#numbermin_safe_integer)<br>
   &nbsp;&nbsp; 2-6. [Number.POSITIVE_INFINITY](#numberpositive_infinity)<br>
   &nbsp;&nbsp; 2-7. [Number.NEGATIVE_INFINITY](#numbernegative_infinity)<br>
   &nbsp;&nbsp; 2-8. [Number.NaN](#numbernan)<br>
3. [Number 메서드](#number-메서드)<br>
   &nbsp;&nbsp; 3-1. [Number.isFinite](#numberisfinite)<br>
   &nbsp;&nbsp; 3-2. [Number.isInteger](#numberisinteger)<br>
   &nbsp;&nbsp; 3-3. [Number.isNaN](#numberisnan)<br>
   &nbsp;&nbsp; 3-4. [Number.isSafeInteger](#numberissafeinteger)<br>
   &nbsp;&nbsp; 3-5. [Number.prototype.toExponential](#numberprototypetoexponential)<br>
   &nbsp;&nbsp; 3-6. [Number.prototype.toFixed](#numberprototypetofixed)<br>
   &nbsp;&nbsp; 3-7. [Number.prototype.toPrecision](#numberprototypetoprecision)<br>

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

## Number 메서드

### Number.isFinite

인수로 전달된 숫자값이 정상적인 유한수인지 검사하여 그 결과값을 불리언 값으로 반환한다.

- 즉, Infinity 또는 -Infinity가 아닌지 검사한다.
- 인수가 NaN이면 언제나 false를 반환한다.

```js
// 인수가 정상적인 유한수이면 true를 반환한다.
Number.isFinite(0); // true
Number.isFinite(Number.MAX_VALUE); // true

// 인수가 무한수이면 false를 반환한다.
Number.isFinite(Infinity); // false

// 인수가 NaN이면 항상 false를 반환한다.
Number.isFinite(NaN); // false
```

빌트인 전역 함수 isFinite는 전달 받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만 Number.isFinite는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.

```js
// Number.isFinite는 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isFinite(null); // false
// isFinite는 인수를 숫자로 암묵적 타입 변환한다. null은 0으로 암묵적 타입 변환된다.
isFinite(null); // true
```

### Number.isInteger

인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

- 검사하기 전 인수를 숫자로 암묵적 타입 변환하지 않는다.

```js
// 인수가 정수이면 true를 반환한다.
Number.isInteger(0); // true
Number.isInteger(123); // true

// 0.5는 정수가 아니다.
Number.isInteger(0.5); // false
```

### Number.isNaN

인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.

- 빌트인 전역 함수 isNaN은 전달받은 인수를 숫자로 암묵적 타입 변환하여 검사를 수행하지만, Number.isNaN 메서드는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.

```js
// 인수가 NaN이면 true를 반환한다.
Number.isNaN(NaN); // TRUE

// Number.isNaN은 인수를 숫자로 암묵적 타입 변환하지 않는다.
Number.isNaN(undefined); // false

// isFinite는 인수를 숫자로 암묵적 타입 변환한다. undefined는 NaN으로 암묵적 타입 변환된다.
isNaN(undefined); // true
```

### Number.isSafeInteger

인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.

- 안전한 정수값은 -(253 - 1)과 253 - 1 사이의 정수값이다.
- 검사전 인수를 숫자로 암묵적 타입 변환하지 않는다.

```js
// 안전한 정수
Number.isSafeInteger(0); // true
Number.isSafeInteger(1000000000000000); // true
// 안전하지 않은 정수
Number.isSafeInteger(10000000000000001); // false
```

### Number.prototype.toExponential

숫자를 지수 표기법으로 변환하여 문자열로 반환한다.

- 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다.

```js
(77.1234).toExponential(); // "7.71234e+1"
(77.1234).toExponential(4); // "7.7123e+1"
(77.1234).toExponential(2); // "7.71e+1"

// 숫자 리터럴과 함께 Number 프로토타입 메서드를 사용하 경우 에러가 발생한다.
77.toExponential(); // SyntaxError
```

자바스크립트 엔진은 숫자 뒤의 .을 부동 소수점 숫자의 소수 구분 기호로 해석한다. 하지만, 위의 77.toExponential()에서 77은 Number 래퍼 객체이다. 즉, 77 뒤의 .을 소수 구분 기호로 해석하면 뒤에 이어지는 toExponential을 프로퍼티로 해석할 수 없으므로 문법 에러가 발생한다.

- 즉, 숫자 리터럴과 함께 메서드를 사용할 경우 혼란을 방지하기 위해 그룹 연산자를 사용할 것을 권장한다.

```js
(77.1234).toExponential(); // "7.71234e+1"
```

### Number.prototype.toFixed

숫자를 반올림하여 문자열로 반환한다.

- 반올림하는 소수점 이하 자릿수를 나타내는 0 ~ 20 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 0이 지정된다.

```js
// 인수를 생략하면 기본값 0이 지정된다.
(12345.6789).toFixed(); // 12346
// 소수점 이하 2자릿수까지 반올림
(12345.6789).toFixed(2); // 12345.68
```

### Number.prototype.toPrecision

인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.

- 인수로 전달받은 전체 자릿수로 표현할 수 없는 경우 지수 표기법으로 결과를 반환한다.
- 전체 자릿수를 나타내는 0 ~ 21 사이의 정수값을 인수로 전달할 수 있으며, 인수를 생략하면 기본값 0이 지정 된다.

```js
// 전체 자릿수 유효. 인수를 생략하면 기본값 0이 지정된다.
(12345.6789).toPrecision(); // "12345.6789"
// 전체 2자릿수 유효, 나머지 반올림
(12345.6789).toPrecision(2); // "1.2e+4"
```

### Number.prototype.toString

숫자를 문자열로 변환하여 변환한다.

- 진법을 나타내는 2 ~ 36 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 10진법이 지정된다.

```js
// 인수를 생략하면 10진수 문자열 반환.
(10).toString(); // "10"
// 2진수 문자열을 반환한다.
(16).toString(2); // "10000"
```
