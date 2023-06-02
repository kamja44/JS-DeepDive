1. [Math 프로퍼티](#math-프로퍼티)<br>
   &nbsp;&nbsp;1-1. [Math.PI](#mathpi)<br>
2. [Math 메서드](#math-메서드)<br>
   &nbsp;&nbsp;2-1. [Math.abs](#mathabs)<br>
   &nbsp;&nbsp;2-2. [Math.round](#mathround)<br>
   &nbsp;&nbsp;2-3. [Math.ceil](#mathceil)<br>
   &nbsp;&nbsp;2-4. [Math.floor](#mathfloor)<br>
   &nbsp;&nbsp;2-5. [Math.sqrt](#mathsqrt)<br>
   &nbsp;&nbsp;2-6. [Math.random](#mathrandom)<br>
   &nbsp;&nbsp;2-7. [Math.pow](#mathpow)<br>
   &nbsp;&nbsp;2-8. [Math.max](#mathmax)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;2-8-1. [배열에서 최대값 구하기](#배열에서-최대값-구하기)<br>
   &nbsp;&nbsp;2-9. [Math.min](#mathmin)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;2-9-1. [배열에서 최소값 구하기](#배열에서-최소값-구하기)<br>

# Math 프로퍼티

## Math.PI

- 원주율 값을 반환한다.
  - 3.141592653589793

```js
Math.PI; // 3.141592653589793
```

# Math 메서드

## Math.abs

- 전달받은 인수의 `절대값`을 반환한다.
  - 반드시 0 또는 양수여야 한다.

```js
Math.abs(-1); // 1
Math.abs("-1"); // 1
```

## Math.round

- 인수로 전달된 숫자의 소수점 이하를 `반올림`한 정수를 반환한다.

```js
Math.round(1.4); // 1
Math.round(1.6); // 2

Math.round(-2.4); // -2
Math.round(-2.6); // -3
```

## Math.ceil

- 인수로 전달된 숫자의 소수점 이하를 `올림`한 정수를 반환한다.

```js
Math.ceil(1.4); // 2
Math.ceil(1); // 1
```

## Math.floor

- 인수로 전달된 숫자의 소수점 이하를 `내림`한 정수를 반환한다.
  - 인수가 음수인 경우 소수점 이하를 떼어 버린 다음 -1을더한 정수를 반환한다.

```js
Math.floor(1.9); // 1
Math.floor(9.1); // 9
Math.floor(-1.9); // -2
Math.floor(-9.1); // -10
```

## Math.sqrt

- 인수로 전달된 숫자의 `제곱근`을 반환한다.

```js
Math.sqrt(9); // 3
Math.sqrt(-9); // NaN
Math.sqrt(1); // 1
Math.sqrt(0); // 0
Math.sqrt(); // NaN
```

## Math.random

- `임의의 난수(랜덤한 숫자)를 반환`한다.
  - 난수는 0에서 1미만의 실수가 반환된다.
    - 즉, 0은 포함되지만 1은 포함되지 않는다.

```js
Math.random();

// 1에서 10 사이의 랜덤 정수 취득
const random = Math.floor(Math.random() * 10 + 1);
```

## Math.pow

- `첫 번째 인수를 밑으로  두 번째 인수를 지수로 거듭제곱한 결과를 반환`한다.
- ES7에서 도입된 지수 연산자를 사용하면 Math.pow 함수를 대체할 수 있다.

```js
Math.pow(2, 7); // 128
Math.pow(2, -1); // 0.5

// ES7 지수 연산자
2 ** (2 ** 2); // 16
Math.pow(Math.pow(2, 2), 2); // 16
```

## Math.max

- 전달받은 인수 중 `가장 큰 수를 반환`한다.
  - 인수가 전달되지 않으면 -Infinity를 반환한다.

```js
Math.max(1); // 1
Math.max(1, 2); // 2
Math.max(); // Infinity
```

### 배열에서 최대값 구하기

- 배열을 인수로 전달받아 배열의 요소 중 최대값을 구하려면 Function.prototype.apply 메서드 or 스프레드 문법을 사용하면 된다.

```js
Math.max.apply(null, [1, 2, 3, 4, 5]); // 5

Math.amx(...[1, 2, 3, 4, 5]); // 5
```

## Math.min

- 전달받은 인수 중 `가장 작은 수를 반환`한다.
  - 인수가 전달되지 않으면 Infinity를 반환한다.

```js
Math.min(1, 2, 3); // 1
Math.min(); // Infinity
```

### 배열에서 최소값 구하기

- 배열을 인수로 전달받아 배열의 요수 중 최소값을 구하려면 Function.prototype.apply 메서드 또는 스프레드 문법을 사용하면 된다.

```js
Math.min.apply(null, [1, 2, 3, 4, 5]); // 1

Math.min(...[1, 2, 3, 4, 5]); // 1
```
