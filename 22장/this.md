# this 키워드

객체

- 상태를 나타내는 프로퍼티와 동작을 나타내는 메서드를 논리적인 단위로 묶은 복합적인 자료구조이다.

메서드가 자신이 속한 객체의 프로퍼티를 참조하려면 먼저 자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다.

객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```js
cosnt circle = {
    // 프로퍼티: 객체 고유의 상태 데이터
    radius: 5,
    // 메서드: 상태 데이터를 참조하고 조작하는 동작
    getDiameter(){
        // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
        return 2 * circle.radius;
    }
};
console.log(circle.getDiameter()); // 10
```

- getDiameter() 가 호출되어 함수 몸체가 실행되는 시점에 참조표현식이 평가된다.
- 객체 리터럴은 circle 변수에 할당되기 직전에 평가된다.
  - 즉, getDiameter 메서드가 호출되는 시점에는 이미 객체 리터럴의 평가가 완료되어 객체가 생성되었고, circle 식별자에 생성된 객체가 할당된 이후이다.
  - 따라서 메서드 내부에서 circle 식별자를 참조할 수 있다.
    - 하지만, 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 바람직하지 않다.

생성자 함수 방식으로 인스턴스를 생성하는 경우

```js
function Circle(radius){
    // 이 시점에는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
    ???.radius = radius;
}
Circle.prototype.getDiameter = function(){
    // 이 시점에서는 생성자 함수 자신이 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
    return 2 * ???.radius;
}
// 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수를 정의해야 한다.
const Circle = new Circle(5);
```

- 생성자 함수로 인스턴스를 생성하려면 먼저 생성자 함수가 존재하는지 확인해야 한다.
  - 생성자 함수를 정의하는 시점에서는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다.
    - 즉, 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 특수한 식별자가 필요하다.
      - `특수한 식별자가 this 이다.`

## this

- this 키워드는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
- this 키워드를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
- this 키워드는 JS엔진에 의해 암묵적으로 생성되므로 코드 어디서든 참조할 수 있다.
- 함수를 호출하면 arguments 객체와 this가 암묵적으로 함수 내부에 전달된다.
  - 함수 내부에서 arguments 객체를 지역 변수처럼 사용할 수 있는 것처럼 this도 지역 변수처럼 사용할 수 있다.
    - 즉, this가 가리키는 값(this 바인딩)은 함수 호출 방식에 의해 동적으로 결정된다.

## this 바인딩

- 바인딩이란 식별자와 값을 연결하는 과정을 의미한다.
  - ex) 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다.
  - 즉, this 바인딩은 this와 this가 가리킬 객체를 바인딩하는 것이다.

위에서 살펴본 객체 리터럴과 생성자 함수의 예제를 this를 사용하여 수정

```js
// 객체 리터럴
const Circle = {
  radius: 5,
  getDiameter() {
    // this는 메서드를 호출한 객체를 가리킨다.
    return 2 * this.radius;
  },
};
console.log(Circle.getDiameter()); // 10
```

객체 리터럴의 메서드 내부에서의 this는 메서드를 호출한 객체 즉, circle을 가리킨다.

```js
// 생성자 함수
function Circle(radius) {
  this.radius = radius;
}
Circle.prototype.getDiameter = function () {
  // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  return 2 * ths.radius;
};

// 인스턴스 생성
const circle = new Circle(5);
console.log(circle.getDiameter()); // 10
```

- 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  - 이처럼 this는 상황에 따라 가리키는 대상이 다르다.

`자바나 C++ 같은 클래스 기반 언어에서 this는 언제나 클래스가 생성하는 인스턴스를 가리킨다.`
`JS의 this는 함수가 호출되는 방식에 따라 this에 바인딩될값, 즉 this 바인딩이 동적으로 결정된다.`
strict mode 역시 this 바인딩에 영향을 준다.

this 키워드는 코드 어디에서든 참조가 가능하다.

```js
// 전역에서도 함수 내부에서도 this 키워드의 참조가 가능하다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.og(this); // window
  return number * number;
}
square(2);
const person = {
  name: "Lee",
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name : "Lee", getName: f}
    return this.name;
  },
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name : "Lee"}
}
const me = new Person("Lee");
```

this는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로, 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.

- 즉, strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다.

# 함수 호출 방식과 this 바인딩

`this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.`

## 렉시컬 스코프와 this 바인딩은 결정시기가 다르다

함수의 상위 스코프를 결정하는 방식인 렉시컬 스코프는 함수 정의가 평가되어 함수 객체가 생성되는 시점에 상위 스코프를 결정한다.
this 바인딩은 함수 호출 시점에 결정된다.

### 함수를 호출하는 방식

1. 일반 함수 호출
2. 메서드 호출
3. 생성자 함수 호출
4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

## 일반 함수 호출

`기본적으로 this에는 전역 객체가 바인딩된다.`

```js
function foo() {
  console.log("foo's this: ", this); // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

전역 함수와 중첩 함수를 일반 함수로 호출하면 함수 내부의 this에는 전역 객체가 바인딩된다.

strict mode가 적용된 일반 함수 내부의 this에는 undefined가 바인딩된다.

```js
function foo() {
  "use strict";
  console.log("foo's this: ", this); // window
  function bar() {
    console.log("bar's this: ", this); // window
  }
  bar();
}
foo();
```

메서드 내에서 정의한 중첩 함수도 일반함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.

```js
var value = 1;
// var 키워드로 선언한 전역 변수 value는 전역 객체의 프로퍼티이다.

// const 키워드로 선언한 전역 변수 value는 전역 객체의프로퍼티가 아니다.
const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value: 100, foo: f}
    console.log("foo's this.value: ", this.value); // 100
    // 메서드 내에서 정의한 중첩 함수
    function bar() {
      console.log("bar's this: ", this); // window
      console.log("bar's this.value: ", this.value); // 1
    }
    // 메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면 중첩 함수 내부의 this에는 전역 객체가 바인딩된다.
    bar();
  },
};
obj.foo();
```

콜백 함수가 일반 함수로 호출된다면 콜백 함수 내부의 this에도 전역 객체가 바인딩된다.

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    console.log("foo's this: ", this); // {value : 100, foo : f}
    // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
    setTimeout(function () {
      console.log("callback's this ", this); // window
      console.log("callback's this.value: ", this.value); // 1
    }, 100);
  },
};
obj.foo();
```

### setTimeout 함수

두 번쨰 인수로 전달한 시간만큼 대기한 다음, 첫 번째 인수로 전달한 콜백 함수를 호출하는 타이머 함수이다.

`일반 함수로 호출된 모든 함수 내부의 this에는 전역 객체가 바인딩된다.`

- 즉, 외부 함수인 메서드와 중첩 함수 또는 콜백 함수의 this가 일치하지 않는 문제가 발생한다.
  - 즉, 중첩 함수 or 콜백 함수가 헬퍼 함수로 동작하기 어렵게 만든다.

메서드 내부의 중첩함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법은 다음과 같다.

```js
var value = 1;
const obj = {
  valeu: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다.
    const that = this;

    // 콜백 함수 내부에서 this 대신 that을 참조한다.
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);
  },
};
obj.foo();
```

이 방법 외에도 Function.prototype.apply/call/bind 메서드를 이용하여 this를 명시적으로 바인딩할 수 있다.

arrow function을 이용하여 this 바인딩을 일치시킬 수 있다.

```js
var value = 1;
const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); // 100
  },
};
obj.foo();
```

화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.

# 메서드 호출

메서드 내부의 this에는 메서드를 호출한 객체, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체가 바인딩된다.
주의할 것은 메소드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩된다는 것이다.

```js
const person = {
  name: "Lee",
  getName() {
    // 메서드 내부의 this는 메서드를 호출한 객체에 바인딩된다.
    return this.name;
  },
};
// 메서드 getName을 호출한 객체는 person이다.
console.log(person.getName()); // Lee

const anotherPerson = {
  name: "kim",
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;
console.log(anotherPerson.getName()); // kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName메서드를 일반 함수로 호출
console.log(getName()); // ""
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
```

위 예의 getName 메서드는 person 객체의 메서드로 정의되어 있다.
메서드는 프로퍼티에 바인딩된 함수이다.
즉, person 객체의 getName 프로퍼티가 가리키는 함수 객체는 person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체이다.
getName 프로퍼티가 함수 객체를 기리키고 있을 뿐이다.

`즉, 메서드 내부의 this는 자신을 호출한 객체를 가리킨다.`

프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩 된다.

```js
function Person(name) {
  this.name = name;
}
Person.prototype.getName = function () {
  return this.name;
};
const me = new Person("Lee");

// getName 메서드를 호출한 객체는 me다.
console.log(me.getName()); // Lee

Person.prototype.name = "Kim";

// getName 메서드를 호출한 객체는 Person.prototype이다.
console.log(Person.prototype.getName()); // Kim
```

# 생성자 함수 호출

생성자 함수 내부의 this에서는 생성자 함수가 미래에 생성할 인스턴스가 바인딩된다.

```js
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킨다
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}
// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20
```

생성자 함수는 객체(인스턴스)를 생성하는 함수이다.
일반 함수와 동일한 방법으로 생성자 함수를 정의하고 new 연산자와 함께 호출하면 해당 함수는 생성자 함수로 동작한다.
new 연산자와 함께 생성자 함수를 호출하지 않으면 생성자 함수가 아니라 일반 함수로 동작한다.
