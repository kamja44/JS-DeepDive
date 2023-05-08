1. [함수의 구분](#함수의-구분)<br>
   &nbsp;&nbsp;1-1. [ES6 에서의 함수 구분](#es6-에서는-함수를-사용-목적에-따라-세-종류로-구분한다)<br>
2. [메서드](#메서드)<br>
3. [화살표 함수](#화살표-함수)<br>
   &nbsp;&nbsp;3-1. [화살표 함수 정의](#화살표-함수-정의)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;3-1-1. [함수 정의](#함수-정의)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;3-1-2. [매개변수 선언](#매개변수-선언)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;3-1-3. [함수 몸체 정의](#함수-몸체-정의)<br>

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

## 화살표 함수

- function 키워드 대신 화살표(=>)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.
  - 표현만 간략한 것이 아닌 내부 동작도 간략하다.
- 화살표 함수는 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안으로 유용하다.

### 화살표 함수 정의

#### 함수 정의

- 화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 한다.
  - 호출 방식은 기존 함수와 동일하다.

```js
const multiply = (x, y) => x * y;
multiply(2, 3);
```

#### 매개변수 선언

- 매개변수가 여러개인 경우 소괄호 안에 매개변수를 선언한다.

```js
const arrow = (x,y) => {...};
```

- 매개변수가 한 개인 경우 소괄호 생략이 가능하다.

```js
const arrow = x => {...};
```

- 매개변수가 없는 경우 소괄호 생략이 불가능하다.

```js
const arrow = () => {...};
```

#### 함수 몸체 정의

- 함수 몸체가 하나의 문이라면 중괄호를 생략할 수 있다.
  - 함수 몸체 내부의 문이 값으로 펴가될 수 있는 표현식인 문이라면 암묵적으로 반환된다.

```js
const power = (x) => x ** 2;
power(2); // 4

// 위의 표현은 다음과 동일하다.
const power = (x) => {
  return x ** 2;
};
```

함수 몸체를 감싸는 중과롷를 생략한 경우 함수 몸체 내부의 문이 표현식이 아닌 문이라면 에러가 발생한다. - 표현식이 아닌 문은 반환할 수 없다.

```js
const arrow = () => const x = 1; // SyntaxError
// 위 표현은 다음과 같다.
const arrow = () => {return const x = 1;};
```

- 즉, 함수 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략할 수 없다.

객체 리터럴을 반환한다면 객체 리터럴을 소괄호로 감싸 주어야 한다.

```js
const create = (id, content) => ({ id, content });
create(1, "JavaScript"); // {id: 1, content: "JavaScript"}

// 위 표현은 다음과 동일하다.
const create = (id, content) => {
  return { id, content };
};

// 객체 리터럴을 소괄호로 감싸지 않으면 객체 리터럴의 중괄호 {}를 함수 몸체를 감싸는 중괄호로 잘못 해석한다.
// {id, content}를 함수 몸체 내의 쉼표 연산자문으로 해석한다.
const create = (id, content) => {
  id, content;
};
create(1, "JavaScript"); // undefined
```

함수 몸체가 여러 개의 문으로 구성된다면 중괄호를 생략할 수 없으며 반환값을 명시적으로 반환해야 한다.

```js
const sum = (a, b) => {
  const result = a + b;
  return result;
};
```

화살표 함수도 즉시 실행 함수로 사용할 수 있다.

```js
const person = ((name) => ({
  sayHi() {
    return `Hi? My name is ${name}.`;
  },
}))("Lee");
console.log(person.sayHi()); // Hi? My name is Lee
```

화살표 함수도 일급 객체이므로 고차 함수에 인수로 전달할 수 있다.

```js
// ES5
[1, 2, 3].map(function (v) {
  return v * 2;
});

// ES6
[1, 2, 3].map((v) => v * 2); // [2, 4, 6]
```
