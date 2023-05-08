1. [함수의 구분](#함수의-구분)<br>
   &nbsp;&nbsp;1-1. [ES6 에서의 함수 구분](#es6-에서는-함수를-사용-목적에-따라-세-종류로-구분한다)<br>

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
