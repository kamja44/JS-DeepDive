1. [함수의 구분](#함수의-구분)<br>
   &nbsp;&nbsp;1-1. [ES6 에서의 함수 구분](#es6-에서는-함수를-사용-목적에-따라-세-종류로-구분한다)<br>
2. [메서드](#메서드)<br>
3. [화살표 함수](#화살표-함수)<br>
   &nbsp;&nbsp;3-1. [화살표 함수 정의](#화살표-함수-정의)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;3-1-1. [함수 정의](#함수-정의)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;3-1-2. [매개변수 선언](#매개변수-선언)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;3-1-3. [함수 몸체 정의](#함수-몸체-정의)<br>
   &nbsp;&nbsp;3-2. [화살표 함수와 일반 함수의 차이](#화살표-함수와-일반-함수의-차이)<br>
   &nbsp;&nbsp;3-3. [this](#this)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;3-3-1. [콜백 함수 내부의 this 문제를 해결하기 위한 방법(ES5)](#콜백-함수-내부의-this-문제를-해결하기-위한-방법)<br>
   &nbsp;&nbsp;&nbsp;&nbsp;3-3-2. [콜백 함수 내부의 this 문제를 해결하기 위한 방법(ES6)](#es6에서는-화살표-함수를-사용하여-콜백-함수-내부의-this-문제를-해결할-수-있다)<br>
   &nbsp;&nbsp;3-4. [super](#super)<br>
   &nbsp;&nbsp;3-5. [arguments](#arguments)<br>

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

### 화살표 함수와 일반 함수의 차이

1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor이다.

```js
const Foo = () => {};
// 화살표 함수는 생성자 함수로서 호출할 수 없다.
new Foo(); // TypeError
Foo.hasOwnProperty("prototype"); // false
```

- 화살표 함수는 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.

2. 중복된 매개변수 이름을 선언할 수 없다.

```js
const arrow (a, a) => a + a;
// SyntaxError
```

3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.

- 화살표 함수 내부에서 this, arguments, super, new.target을 참조하면 스코프체인을 통해 상위 스코프의 this, arguments, super, new.target을 참조한다.
- 만일, 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this, arguments, super, new.target 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this, arguments, super, new.target을 참조한다.

### this

- 화살표 함수의 this는 일반 함수의 this와 다르게 동작한다.
  - 이는 콜백 함수 내부의 this가 외부 함수의 this와 다르기 때문에 발생하는 문제를 해결하기 위해 의도적으로 설계된 것이다.
- this 바인딩은 함수의 호출 방식에 따라 동적으로 결정된다.
  - 즉, 함수를 정의할 때 this에 바인딩 할 객체가 정적으로 결저오디는 것이 아닌, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정된다.

주어진 배열의 각 요소에 접두어를 추가하는 예

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }
  add(arr) {
    // add 메서드는 인수로 전달된 arr 배열을 순회하며 배열의 모든 요소에 prefix를 추가한다. ①
    return arr.map(function (item) {
      return this.prefix + item; // ②
      // TypeError
    });
  }
}
const prefixer = new Prefixer("-webkit-");
console.log(prefixer.add(["transition", "user-select"]));
```

- 위 예제의 기대결과는 ["-webkit-transition","-webkit-user-select"]이다. 하지만 에러가 발생한다.

- 프로토타입 메서드 내부인 ①에서 this는 메서드를 호출한 객체(prefixer)를 가리킨다.
  - Array.prototype.map의 인수로 전달한 콜백 함수의 내부인 ②에서 this는 undefined를 가리킨다.
    - 이는 Array.prototype.map 메서드가 콜백 함수를 일반 함수로 호출하기 때문이다.
- 일반 함수로서 호출되는 모든 함수 내부의 this는 전역 객체를 가리킨다.
  - 클래스 내부의 모든 코드에는 strict mode가 적용된다.
    - 즉, Array.prototype.map 메서드의 콜백 함수에도 strict mode가 적용된다.
      - strict mode에서 일반 함수로서 호출된 모든 함수 내부의 this에는 전역 객체가 아닌 undefined가 바인딩되므로 일반 함수로서 호출되는 메서드의 콜백 함수 내부의 this에는 undefined가 바인딩된다.
- 즉, 콜백 함수의 this(②)와 외부 함수의 this(①)가 서로 다른 값을 가리키고 있기에 TypeError가 발생한다.

#### 콜백 함수 내부의 this 문제를 해결하기 위한 방법

1. add 메서드를 호출한 prefixer 객체를 가리키는 this를 회피시킨 후 콜백 함수 내부에서 사용한다.

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }
  add(arr) {
    const that = this; // this 회피
    return arr.map(function (item) {
      // this 대신 that 참조
      return that.prefix + " " + item;
    });
  }
}
```

2. Array.prototype.map의 두 번째 인수로 add 메서드를 호출한 prefixer 객체를 가리키는 this를 전달한다.
   - Array.prototype.map은 콜백 함수 내부의 this 문제를 해결하기 위해 두 번째 인수로 콜백 함수 내부에서 this로 사용할 객체를 전달할 수 있다.

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }
  add(arr) {
    return arr.map(function (item) {
      return this.prefix + " " + item;
    }, this); // this에 바인딩 된 값이 콜백 함수 내부의 this에 바인딩된다.
  }
}
```

3. Function.prototype.bind 메서드를 사용하여 add 메서드를 호출한 prefixer 객체를 가리키는 this를 바인딩한다.

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }
  add(arr) {
    return arr.map(function(item){
        return this.prefix + " " = item;
    }.bind(this)); // this에 바인딩된 값이 콜백 함수 내부의 this에 바인딩된다.
  }
}
```

### ES6에서는 화살표 함수를 사용하여 콜백 함수 내부의 this 문제를 해결할 수 있다.

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }
  add(arr) {
    return arr.map((item) => this.prefix + item);
  }
}
const prefixer = new Prefixer("-webkit-");
console.log(prefixer.add(["transition", "user-select"]));
```

- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다.
  - 이를 `lexical this`라 한다.
    - 이는 렉시컬 스코프와 같이 화살표 함수의 this가 함수가 정의된 위치에 의해 결정된다는 것을 의미한다.

화살표 함수를 제외한 모든 함수에는 this 바인딩이 반드시 존재한다. - ES6에서 화살표 함수가 도입되기 이전에는 일반적인 식별자처럼 스코프 체인을 통해 this를 탐색할 필요가 없었다. - 화살표 함수는 함수 자체의 this 바인딩이 존재하지 않기에 화살표 함수 내부에서 this를 참조하면 스코프 체인을 통해 상위 스코프에서 this를 탐색한다.
화살표 함수를 Function.prototype.bind를 사용하여 표현하면 다음과 같다.

```js
// 화살표 함수는 상위 스코프의 this를 참조한다.
() => this.x;
// 익명 함수에 상위 스코프의 this를 주입한다. 위의 화살표 함수와 동일하게 동작한다.
(function () {
  return this.x;
}).bind(this);
```

만일 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중 화살표 함수가 아닌 함수의 this를 참조한다.

```js
// 중첩 함수 foo의 상위 스코프는 즉시 실행 함수이다.
// 즉, 화살표 함수 foo의 this는 상위 스코프인 즉시 실행 함수의 this를 가리킨다.
(function () {
  const foo = () => console.log(this);
  foo();
}).call({ a: 1 }); // {a: 1}

// bar 함수는 화살표 함수를 반환한다.
// bar 함수가 반환한 화살표 함수의 상위 스코프는 화살표 함수 bar 이다.
// 하지만 화살표 함수는 함수 자체의 this 바인딩을 갖지 않으므로 bar 함수가 반환한 화살표 함수 내부에서 참조하는 this는 화살표 함수가 아닌 즉시 실행 함수의 this를 가리킨다.
(function () {
  const bar = () => () => console.log(this);
  bar()();
}).call({ a: 1 }); // {a: 1}
```

만일 화살표 함수가 전역 함수라면 화살표 함수의 this는 전역 객체를 가리킨다.

- 전역 함수의 상위 스코프는 전역이고 this는 전역 객체를 가리키기 때문이다.

```js
// 전역 함수 foo의 상위 스코프는 전역이므로 화살표 함수 foo의 this는 전역 객체를 가리킨다.
const foo = () => console.log(this);
foo(); // window
```

프로퍼티에 할당한 화살표 함수도 스코프 체인 상에서 가장 가까운 상위 함수 중 화살표 함수가 아닌 함수의 this를 참조한다.

```js
// increase 프로퍼티에 할당한 화살표 함수의 상위 스코프는 전역이다.
// 즉, increase ㅍ로퍼티에 할당한 화살표 함수의 this는 전역 객체를 가리킨다.
const counter = {
  num: 1,
  increase: () => ++this.num,
};
console.log(counter.increase()); // NaN
```

화살표 함수는 함수 자체의 this 바인딩을 갖지 않기에 Function.prototype.call, Function.prototype.apply, Function.prototype.bind 메서드를 사용해도 화살표 함수 내부의 this를 교체할 수 없다.

```js
window.x = 1;

const normal = function () {
  return this.x;
};
const arrow = () => this.x;

console.log(normal.call({ x: 10 })); // 10
console.log(arrow.call({ x: 10 })); // 1
```

이는 화살표 함수가 Function.prototype.call, Function.prototype.apply, Function.prototype.bind 메서드를 호출할 수 없다는 의미가 아닌, 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기에 this를 교체할 수 없고 언제나 상위 스코프의 this 바인딩을 참조한다.

```js
const add = (a, b) => a + b;
console.log(add.call(null, 1, 2)); // 3
console.log(add.apply(null, [1, 2])); // 3
console.log(add.bind(null, 1, 2)()); // 3
```

메서드를 화살표 함수로 정의하는 것은 피해야 한다.

- 여기서 말하는 메서드란 ES6 메서드가 아닌 일반적인 의미의 메서드를 말한다.

```js
// 나쁜 예
const person = {
  name: "Lee",
  sayHi: () => console.log(`Hi ${this.name}`),
};
// sayHi 프로퍼티에 할당된 화살표 함수 내부의 this는 상위 스코프인 전역 객체를 가리키므로 이 예제를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는 window.name과 같다.
person.sayHi(); // Hi

// 좋은 예
const person = {
  name: "Lee",
  sayHi() {
    console.log(`Hi ${this.name}`);
  },
};
person.sayHi(); // Hi Lee
```

프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우에도 동일한 문제가 발생한다.

```js
// 나쁜 예
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = () => console.log(`Hi ${this.name}`);

const person = new Person("Lee");
// 이 예를 브라우저에서 실행하면 this.name은 빈 문자열을 갖는 window.name과 같다.
person.sayHi(); // Hi

// 프로퍼티를 동적 추가할 때는 ES6 메서드 정의를 사용할 수 없으므로 일반 함수를 할당한다.

// 좋은 예
function Person(name) {
  this.name = name;
}
Person.prototype.sayHi = function () {
  console.log(`Hi ${this.name}`);
};
const person = new Person("Lee");
person.sayHi(); // Hi Lee
```

일반 함수가 아닌 ES6 메서드를 동적 추가하고 싶다면 객체 리터럴을 바인딩하고 프로토타입의 constructor 프로터피와 생성자 함수 간의 연결을 재설정한다.

```js
function Person(name) {
  this.name = name;
}
Person.prototype = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 재설정
  constructor: Person,
  sayHi() {
    console.log(`Hi ${this.name}`);
  },
};
const person = new Person("Lee");
person.sayHi(); // Hi Lee
```

클래스 필드 정의 제안을 사용하여 클래스 필드에 화살표 함수를 할당할 수도 있다.

```js
// 나쁜 예
class Person {
  // 클래스 필드 정의 제안
  name = "Lee";
  sayHi = () => console.log(`Hi ${this.name}`);
}
const person = new Person();
person.sayHi(); // Hi Lee
```

이때 sayHi 클래스 필드에 할당한 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this 바인딩을 참조한다.

- 즉, sayHi 클래스 필드에 할당한 화살표 함수의 상위스코프는 다음과 같다.
  - sayHi 클래스 필드는 인스턴스 프로퍼티이다.

```js
class Person {
  constructor() {
    this.name = "Lee";
    // 클래스가 생성한 인스턴스(this)의 sayHi 프로퍼티에 화살표 함수를 할당한다.
    // 따라서 sayHi 프로퍼티는 인스턴스 프로퍼티이다.
    this.sayHi = () => console.log(`Hi ${this.name}`);
  }
}
```

sayHi 클래스 필드에 할당한 화살표 함수의 상위 스코프는 constructor이다.

- 즉, sayHi 클래스 필드에 할당한 화살표 함수 내부에서 참조한 this는 constructor 내부의 this 바인딩과 같다.
  - constructor 내부의 this 바인딩은 클래스가 생성한 인스턴스를 가리키므로 sayHi 클래스 필드에 할당한 화살표 함수 내부의 this 또한 클래스가 생성한 인스턴스를 가리킨다.
    - 하지만, 클래스 필드에 할당한 화살표 함수는 프로토타입 메서드가 아니라 인스턴스 메서드가 된다. -즉, 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 권장된다.

```js
// 좋은 예
class Person {
  // 클래스 필드 정의
  name = "Lee";
  sayHi() {
    console.log(`Hi ${this.name}`);
  }
}
const person = new Person();
person.sayHi(); // Hi Lee
```

### super

- 화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다.
  - 즉, 화살표 함수 내부에서 super를 참조하면 상위스코프의 super를 참조한다.

```js
class Base {
  constructor(name) {
    this.name = name;
  }
  sayHi() {
    return `Hi! ${this.name}`;
  }
}
class Derived extends Base {
  // 화살표 함수의 super는 상위 스코프인 constructor의 super를 가리킨다.
  sayHi = () => `${super.sayHi()} how are you doing?`;
}
const derived = new Derived("Lee");
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

- super는 내부 슬롯 [[HomeObject]]를 갖는 ES6 메서드 내에서만 사용할 수 있는 키워드이다.
  - sayHi 클래스 필드에 할당한 화살표 함수는 ES6 메서드는 아니지만 함수 자체의 super 바인딩을 갖지 않으므로 super를 참조해도 에러가 발생하지 않고 상위 스코프인 constructor의 super 바인딩을 참조한다.

### arguments

- 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다.
  - 화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다.

```js
(function () {
  // 화살표 함수 foo의 arguments는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
  const foo = () => console.log(arguments);
  // [Arguments] [1, 2, callee: ƒ, Symbol(Symbol.iterator): ƒ]
  foo(3, 4);
})(1, 2);

// 화살표 함수 foo의 arguments는 상위 스코프인 전역의 arguments를 가리킨다.
// 하지만 전역에는 arguments 객체가 존재하지 않는다.
// arguments 객체는 함수 내부에서만 유효하다.
const foo = () => console.log(arguments);
foo(1, 2); //ReferenceError
```

- arguments 객체는 함수를 정의할 때 매개변수의 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다.
  - 하지만, 화살표 함수에서는 arguments 객체를 사용할 수 없다.
    - 상위 스코프의 arguments 객체는 참조할 수 있지만 화살표 함수 자신에게 전달된 인수 목록을 확인할 수 없고 상위 함수에게 전달된 인수 목록을 참조하므로 그다지 도움되지 않는다.
      - 즉, 화살표 함수로 가변 인자 함수를 구현해야 할 때는 Rest 파라미터를 사용해야 한다.
