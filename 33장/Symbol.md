1. [Symbol](#symbol)<br>
2. [심벌 값의 생성](#심벌-값의-생성)<br>
   &nbsp;&nbsp;2-1. [Symbol 함수](#symbol-함수)<br>
   &nbsp;&nbsp;2-1. [Symbol.for / Symbol.keyFor 메서드](#symbolfor--symbolkeyfor-메서드)<br>
3. [심벌과 상수](#심벌과-상수)<br>
4. [enum](#enum)<br>
5. [심벌과 프로퍼티 키](#심벌과-프로퍼티-키)<br>
6. [심벌과 프로퍼티 은닉](#심벌과-프로퍼티-은닉)<br>
7. [심벌과 표준 빌트인 객체 확장](#심벌과-표준-빌트인-객체-확장)<br>
8. [Well-known Symbol](#well-known-symbol)<br>

## Symbol

- ES6에서 도입된 변경 불가능한 원시 타입의 값이다.
- 다른 값과 중복되지 않는 유일무이한 값이다.
  - 주로 이름의 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용한다.

## 심벌 값의 생성

### Symbol 함수

- 심벌 값은 Symbol 함수를 호출하여 생성해야 한다.
  - 생성된 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
  - `다른 값과 절대 중복되지 않는 유일무이한 값이다.`
- 심벌은 new 연산자와 함께 호출하지 않는다.

```js
// Symbol 생성
const mySymbol = Symbol();
console.og(typeof mySymbol); // 'symbol'
// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()

new Symbol(); // Smbol is not a constructor
```

- Symbol 함수에 문자열을 인수로 전달할 수 있다.
  - 전달한 문자열은 심벌 값 생성에 어떠한 영향도 주지 않는다.
    - 즉, 전달한 문자열이 같더라도 생성된 심벌 값은 서로 다르다.

```js
const Symbol1 = Symbol("mySymbol");
const Symbol2 = Symbol("mySymbol");

console.log(Symbol1 === Symbol2); // false
```

- 심벌 값도 객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.

```js
const mySymbol = Symbol("mySymbol");

console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

- 심벌 값은 암묵적으로 문자열이나 숫자로 변환되지 않는다.
  - 단, 불리언 타입으로 타입 변환이 가능하다.

```js
const mySymbol = Symbol();

console.log(mySymbol + ""); //  Cannot convert a Symbol value to a string
console.log(+mySymbol); //  Cannot convert a Symbol value to a string

console.log(!!mySymbol); // true

if (mySymbol) {
  console.log("mySymbol is not empty");
}
```

### Symbol.for / Symbol.keyFor 메서드

- 인수로 전달받은 문자열을 키로 사용하여 키와 심벌값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.
  - 검색에 성공하면 검색된 심벌 값을 반환한다.
  - 검색에 실패하면 새로운 심벌 값을 생성하여 Symbol.for 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환한다.

```js
// mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성한다.
const s1 = Symbol.for("mySymbol");
// mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환한다.
const s2 = Symbol.for("mySymbol");
console.log(s1 === s2); // true
```

- Symbol.for 메서드를 사용하면 애플리케이션 전역에서 중복되지 않는 상수인 심벌 값을 단 하나만 생성하여 전역 심벌 레지스트리를 통해 공유할 수 있다.
- Symbol.keyFor 메서드를 사용하면 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출할 수 있다.

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값 생성
const s1 = Symbol.for("mySymbol");
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol("kamja");
Symbol.keyFor(s2); // undefined
```

## 심벌과 상수

위, 아래, 왼쪽, 오른쪽을 나타내는 상수를 정의하는 예제

```js
const Direction = {
  UP: 1,
  DOWN: 2,
  LEFT: 3,
  RIGHT: 4,
};
// 변수에 상수 할당
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.og("UP");
}
```

- 이와 같이 값에는 특별한 의미가 없고 상수 이름 자체에 의미가 있는 경우 심벌 값을 사용할 수 있다.
  - 변경, 중복될 가능성이 있는 무의미한 상수 대신 중복 가능성이 없는 심벌 사용

```js
const Direction = {
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
};
const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("UP");
}
```

## enum

- 명명된 숫자 상수의 집합으로 열거형이라고 부른다.
- 자바스크립트는 enum을 지원하지 않는다.
  - 자바스크립트에서 enum을 사용하기 위해선 객체의 변경을 방지하기 위해 `객체를 동결하는 Object.freeze 메서드와 심벌 값을 사용`한다.

```js
// JS enum
// Direction 객체는 불변 객체이며 프로퍼티 값은 유일무이한 값이다.
const Direction = Object.freeze({
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
});

const myDirection = Direction.UP;
if (myDirection === Direction.UP) {
  console.log("UP");
}
```

## 심벌과 프로퍼티 키

- 심벌 값을 프로퍼티 키로 사용하려면 프로퍼티 키로 사용할 심벌 값에 대괄호를 사용해야 한다.
  - 프로퍼티에 접근할 때도 마찬가지로 대괄호를 사용해야 한다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키 생성
  [Symbol.for("mySymbol")]: 1,
};
obj[Symbol.for("mySymbol")]; // 1
```

- `심벌 값은 유일무이한 값이므로 심벌 값으로 프로퍼티 키를 만들면 다른 프로퍼티 키와 절대 충돌하지 않는다.`

## 심벌과 프로퍼티 은닉

- 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for ...in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없다.
  - 즉, 외부에 노출할 필요가 없는 프로퍼티를 은닉할 수 있다.
  - Object.getOwnPropertyymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키 생성
  [Symbol("mySymbol")]: 1,
};
for (const key in obj) {
  console.log(key); // undefined (출력안됨)
}
console.log(Object.keys(obj)); // []
console.log(Object.getOwnPropertyNames(obj)); // []

// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환한다.
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```

## 심벌과 표준 빌트인 객체 확장

- 표준 빌트인 객체는 읽기 전용으로 사용하는 것을 권장한다.
  - 표준 빌트인 객체에 자용자 정의 메서드를 직접 추가하여 확장하는 것은 권장하지 않는다.
    - 개발자가 직접 추가한 메서드가 미래에 추가될 `메서드의 이름이 중복될 수 있기 때문`

```js
// 표준 빌트인 객체를 확장하는 것은 권장하지 않는다.
Array.prototype.sum = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};
[1, 2].sum(); // 3
```

- 이러한 경우 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장할 수 있다.

```js
// 심벌 값으로 프로퍼티 키를 동적 생성하면 다른 프로퍼티 키와 절대 충돌하지 않아 안전하다.
Array.prototype[Symbol.for("sum")] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};
[1, 2][Symbol.for("sum")](); // 3
```

## Well-known Symbol

- 자바스크립트가 기본 제공하는 빌트인 심벌 값을 Well-known Symbol이라고 부른다.
  - for...of 문으로 순회 가능한 빌트인 이터러블은 Well-knwon Symbol인 Symbol.iterator를 키로 갖는 메서드를 가지며, Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript 사양에 규정되어 있다.
  - 만일, 빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따라야 한다.
    - 즉, Well-known Symbol인 Symbol.iterator를 키로 갖는 메서드를 객체에 추가하고 이터레이터를 반환하도록 구현하면 그 객체는 이터러블이 된다.

```js
// 1 ~ 5 범위의 정수로 이뤄진 이터러블
const iterable = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환한다.
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      },
    };
  },
};
for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```

- 이터레이션 프로토콜을 준수하기 위해 일반 객체에 추가해야 하는 메서드의 키 Symbol.iterator는 기존 프로퍼티 키 또는 미래에 추가될 프로퍼티 키와 절대로 중복되지 않는다.

`심벌은 중복되지 않는 상수 값을 생성하는 것은 물론 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해 즉, 하위 호환성을 보장하기 위해 도입되었다.`
