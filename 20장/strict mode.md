1. [strict mode](#strict-mode)
2. [strict mode 적용](#strict-mode-적용)
3. [전역에 strict mode를 적용하는 것은 지양해야 한다.](#전역에-strict-mode를-적용하는-것은-지양해야-한다)<br>
   &nbsp;&nbsp; 3.1 [함수 단위로 strict mode를 적용하는 것도 지양해야 한다.](#함수-단위로-strict-mode를-적용하는-것도-지양해야-한다)
4. [strict mode가 발생시키는 대표적인 에러](#strict-mode가-발생시키는-대표적인-에러)
5. [strict mode 적용에 의한 변화](#strict-mode-적용에-의한-변화)

# strict mode

- ES5부터 추가되었다.
- strict mode는 JS 언어의 문법으 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 JS 엔진의 최적화 작업에 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시킨다.

```js
function foo() {
  x = 10;
}
foo();
console.log(x);
```

- foo 함수 내에서 선언하지 않은 x 변수에 값 10 할당
- foo 함수의 스코프에서 x 변수의 선언을 검색
  - foo 함수에는 변수 x의 선언이 없으므로 상위 스코프에서 x 변수의 선언을 검색한다.
    - 상위 스코프(전역 스코프)에도 존재하지 않기에 ReferenceError를 발생시킬 것 하지만 JS엔진은 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성한다.
      - 이때 전역 객체의 x 프로퍼티는 전역 변수처럼 사용할 수 있는데 이러한 현상을 `암묵적 전역`이라고 한다.
        - 암묵적 전역은 오류를 발생시키는 원인이 될 가능성이 크기 떄문에 var, let, const 키워드를 사용하여 변수를 선언한 다음 사용해야 한다.

# strict mode 적용

- 전역의 선두 or 함수 몸체의 선두에 `"use strict";`를 추가한다.

전역 선두에서 사용

```js
"use strict";
function kamja() {
  x = 10; // ReferenceError : x is not defined
}
kamja();
```

함수 몸체의 선두에서 사용

- 함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode가 적용된다.

```js
function kamja() {
  "use strict";
  x = 10; // ReferenceError: x is not defined
}
kamja();

// 코드의 선두에 "use strict";를 위치시키지 않으면 strict mode가 제대로 동작하지 않는다.
function kokuma() {
  y = 10; // 에러 발생 x
  ("use strict");
}
```

# 전역에 strict mode를 적용하는 것은 지양해야 한다.

- 전역에 적용한 strict mode는 스크립트 단위로 적용되기 때문

```html
<!DOCTYPE html>
<html>
  <body>
    <script>
      "use strict";
    </script>
    <script>
      x = 1; // 에러 발생 x
      console.log(x); // 1
    </script>
    <script>
      "use strict";
      y = 1; // ReferenceError : y is not defined
      console.log(y);
    </script>
  </body>
</html>
```

- 즉, 스크립트 단위로 적용된 strict mode는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.

외부 서드파티 라이브러리를 사용하는 경우 라이브러리가 non-strict mode인 경우도 있기에 전역에 strict mode를 적용하는 것은 바람직하지 않다.

- 이러한 경우 즉시 실행 함수로 스크립트 전체를 감싸서 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용한다.

```js
// 즉시 실행 함수의 선두에 strict mode 적용
(function () {
  "use strict";

  // CODE
});
```

# 함수 단위로 strict mode를 적용하는 것도 지양해야 한다.

- strict mode는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

# strict mode가 발생시키는 대표적인 에러

1. 암묵적 전역

- 선언하지 않은 변수를 참조하면 ReferenceError가 발생한다.

```js
(function () {
  "use strict";

  x = 1;
  console.log(x); // ReferenceError : x is not defined
})();
```

2. 변수, 함수, 매개변수의 삭제

- delete 연산자로 변수, 함수, 매개변수를삭제하면 SyntaxError가 발생한다.

```js
(function(){
  "use strict';

  let x = 1;
  delete x; // SyntaxError : Delete of an unqualified identifier in strict mode

  function kamja(a){
    delete kamja; // SyntaxError : Delete of an unqualified identifier in strict mode
  }
  delete kamja; // SyntaxError : Delete of an unqualified identifier in strict mode
}());
```

3. 매개변수 이름의 중복

- 중복된 매개변수 이름을 사용하면 SyntaxError가 발생한다.
- SyntaxError : Duplicate parameter name not allowed in this context

4. with 문의 사용

- with 문은 전달된 객체를 스코프 체인에 추가한다.
  - with 문은 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해 지는 효과가 있지만 성능과 가독성이 나빠지는 문제가 있다.
    - 즉, with 문의 사용을 지양해야 한다.

```js
(function () {
  "use strict";

  // SyntaxError : Strict mode code may not include a with statement
  with ({ x: 1 }) {
    console.log(x);
  }
})();
```

# strict mode 적용에 의한 변화

- 일반 함수의 this
  - strict mode에서 함수를 일반 함수로서 호출하면 this에 undefined가 바인딩된다.
    - 생성자 함수가 아닌 일반 함수 내부에서는 this를 사용할 필요가 없기 때문이다.
      - 이때 에러는 발생하지 않는다.

```js
(function () {
  "use strict";

  function kamja() {
    console.log(this); // undefined
  }
  kamja();
  function kokuma() {
    console.log(this); // kokuma
  }
  new kokuma();
})();
```

- arguments 객체
  - strict mode에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

```js
(function (a) {
  "use strict";
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;
  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // {0 : 1, length ; 1}
})(1);
```
