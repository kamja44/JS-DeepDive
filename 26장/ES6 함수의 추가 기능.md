1. [함수의 구분](#함수의-구분)<br>
   &nbsp;&nbsp;1-1. [ES6 에서의 함수 구분](#es6-에서는-함수를-사용-목적에-따라-세-종류로-구분한다)<br>
2. [메서드](#메서드)<br>

## 함수의 구분

ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않는다.

- ES6 이전의 모든 함수는 일반 함수로서 호출할 수 있는 것은 물론 생성자 함수로서 호출할 수 있다.
  - 즉, ES6 이전의 모든 함수는 callable이면서 constructor이다.

```js
var foo = function () {
  return 1;
};

// 일반적인 함수로서 호출
foo(); // 1

// 생성자 함수로서 호출
new foo(); // foo {}

// 메서드로서 호출
var obj = { foo: foo };
obj.foo(); // 1

var kamja = function () {};

// ES6 이전의 모든 함수는 callable이면서 constructor이다.
kamja(); // undefined
new kamja(); // kamja{}
```

- 호출할 수 있는 함수 객체를 callable 이라고 한다.
- 인스턴스를 생성할 수 있는 함수 객체를 constructor 이라고 한다.
- 인스턴스를 생성할 수 없는 함수 객체를 non-constructor이라고 한다.

### ES6 에서는 함수를 사용 목적에 따라 세 종류로 구분한다.

| ES6 함수의 구분 | constructor | prototype | super | arguments |
| :-------------: | :---------: | :-------: | :---: | :-------: |
|    일반 함수    |      O      |     O     |   X   |     O     |
|     메서드      |      X      |     X     |   O   |     O     |
|   화살표 함수   |      X      |     X     |   X   |     X     |

- 일반 함수는 함수 선언문이나 함수 표현식으로 정의한 함수를 말한다.
  - ES6 이전의 함수와 차이가 없다.
- 일반 함수는 constructor이지만 ES6의 메서드와 화살표 함수는 non-constructor이다.

## 메서드

- ES6에서 메서드는 `메서드 축약 표현으로 정의된 함수만을 의미한다.`

```js
const obj = {
  x: 1,
  // foo는 메서드이다.
  foo() {
    return this.x;
  },
  // bar에 바인딩된 함수는 메서드가 아닌 일반 함수이다.
  bar: function () {
    return this.x;
  },
};
console.log(obj.foo()); // 1
console.log(obj.bar()); // 1

// ES6 메서드는 인스턴스를 생성할 수 없는 non-constructor이다.
// 즉, 생성자 함수로 호출할 수 없다.
new obj.foo(); // TypeError
new obj.bar(); // bar{}

// ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
obj.foo.hasOwnProperty("prototype"); // false
obj.bar.hasOwnProperty("prototype"); // true
```

- ES6 메서드는 인스턴스를 생성할 수 없는 non-constructor이다.
  - 즉, 생성자 함수로 호출할 수 없다.
- ES6 메서드는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없으며, 프로토타입도 생성하지 않는다.
- 표준 빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor이다.
- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖는다.
  - super 참조는 내부 슬롯 [[HomeObject]]를 사용하여 슈퍼클래스의 메서드를 참조한다.
    - 즉, [[HomeObject]]를 갖는 ES6 메서드는 super 키워드를 사용할 수 있다.

```js
const base = {
  name: "Lee",
  sayHi() {
    return `Hi ${this.name}`;
  },
};
const derived = {
  __proto__: base,
  // sayHi는 ES6 메서드이기에 [[HomeObject]]를 갖는다.
  // sayHi의 [[HomeObject]]는 derived.prototype을 가리키고
  // super는 sayHi의 [[HomeObject]]의 프로토타입인 base.prototype을 가리킨다.
  sayHi() {
    return `${super.sayHi()} How are you doing?`;
  },
};
console.log(derived.sayHi()); // Hi Lee How are you doing?
// ES6 메서드가 아니 함수는 super 키워드를 사용할 수 없다.
// ES6 메서드가 아닌 하뭇는 내부 슬롯 [[HomeObject]]를 갖지 않기 때문이다.

const derived2 = {
  __proto__: base,
  // sayHi는 ES6 메서드가 아니기에 super 키워드를 사용할 수 없다.
  sayHi: function () {
    // SyntaxError
    return `${super.sayHi()}. how are you doing?`;
  },
};
```

- ES6 메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거했다.
