1. [var 키워드](#var-키워드로-선언한-변수의-문제점)<br>
2. [let 키워드](#let-키워드)<br>
   &nbsp;&nbsp;221 [일시적 사각지대(TDZ)](<#일시적-사각지대(TDZ)>)<br>
   &nbsp;&nbsp;222 [전역 객체와 let](#전역-객체와-let)<br>

# var 키워드로 선언한 변수의 문제점

3. [Const 키워드](# Const-키워드)

1. 변수 중복 선언 허용

```Javascript
var x = 1;
var y = 1;
// var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
var x = 100;
// 초기화문이 없는 변수 선언문은 무시된다.
var y;

console.log(x); // 100
console.log(y); // 1
```

- 변수를 중복 선언하면서 값을 할당했다면 의도치 않게 먼저 선언된 변수의 값이 변경되는 부작용이 있다.

2. 함수 레벨 스코프

- var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.
- 즉, 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.

```Javascript
var x = 1;
if(1){
    var x = 10;
}
console.log(x); // 10

var i = 10;
for(var i=0; i<5; i++){
    console.log(i); // 0 1 2 3 4
}
console.log(i); // 5
```

3. 변수 호이스팅

- var 키워드로 변수를 선언하면 변수 호이싕에 의해 변수 선언문이 스코프의 선두로 끌어 올려진 것처럼 동작한다.
  - 즉, var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다. 단, 할당문 이전에 변수를 참조하면 항상 undefined를 반환한다.

```Javascript
// 이미 kamja 변수 선언됨(변수 호이스팅)
console.log(kamja); // undefiend
kamja = 123;
console.log(kamja); // 123

// 변수 선언은 런타임 이전에 JS 엔진에 의해 암묵적으로 실행된다.
var kamja;
```

- 변수 선언문 이전에 변수를 참조하는 것은 에러를 발생시키지 않지만, 프로그램의 흐름에 맞지 않고 가독성을 떨어뜨리며 오류를 발생시킬 수 있다.

# let 키워드

- var키워드의 단점을 보완하기 위해 ES6에서 등장한 변수 선언 키워드

1. 변수 중복 선언 금지

```Javascript
let kamja = 123;
// let과 const 키워드는 같은 스코프 내에서 중복 선언을 허용하지 않는다.
let kamja = 456; // SyntaxError: Identifier 'kamja' has already been declared
```

2. 블록 레벨 스코프

```Javascript
let kamja = 1; // 전역 변수
{
    let kamja = 2; // 지역 변수
    let kokuma = 3; // 지역 변수
}
console.log(kamja); // 1
console.log(kokuma); // ReferenceError: kokuma is not defined
```

- let 키워드로 선언된 변수는 블록 레벨 스코프를 따른다.
  - 함수도 코드 블록이므로 스코프를 만든다. 이때 함수 내의 코드 블록은 함수 레벨 스코프에 중첩된다.

3. 변수 호이스팅

- let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

```Javascript
console.log(kamja); // ReferenceError: kamja is not defined
let kamja = "hi";
```

- <u>var키워드는 JS엔진에 의하여 암묵적으로 `선언 단계`와 `초기화 단계`가 한번에 진행된다.</u>
  - <u>선언 단계에서 스코프에 변수 식별자를 등록하여 JS엔지에 변수의 존재를 알리며 그 즉시 초기화 단계에서 undefiend로 변수를 초기화한다.</u>
- <u>let 키워드로 선언한 변수는 `선언 단계`와 `초기화 단계`가 분리되어 진행된다.</u>
  - 런타임 이전에 JS엔진에 의해 선언 단계가 먼저 실행되지만 초기화 단계는 변수 선언문에 도달했을 때 실행된다.
    - 즉, 초기화 단계 이전에 변수에 접근하려고 하면 ReferenceError가 발생한다.

## 일시적 사각지대(TDZ)

- let 키워드로 선언한 변수는 스코프의 시작 지점부터 초기화 단계 시작 지점(변수 선언문)까지 변수를 참조할 수 없다.
- 스코프의 시작 지점부터 변수를 참조할 수 없는 구간을 일시적 사각지대라고 한다.
- 즉, let 키워드로 선언한 변수는 변수 호이스팅이 발생하지 않는 것처럼 동작한다. <- 변수 호이스팅이 동작한다.
  - ```Javascript
    let kamja = 1; // 전역 변수
    {
        console.log(kamja); // ReferenceError : Can't access 'kamja' before initialization
        let kamja = 2;
    }
    ```

```
    - let 키워드가 변수 호이스팅이 발생하지 않는다면, 전역 변수 kamja의 값을 출력해야 한다.
        - 변수 호이스팅이 여전히 발생하기에 참조 에러(ReferenceError)가 발생한다.

`JS는 모든 선언(var, const, let, function, class 등)을 호이스팅한다. 단, ES6에서 도입된 let, const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다.`
```

## 전역 객체와 let

- `var 키워드로 선언한 전역 변수와 전역 함수, 선언하지 않은 변수에 값을 할당한 암묵적 전역은 전역객체 window의 프로퍼티가 된다.`
  - 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다.

```Javascript
// 이 예제는 브라우저에서 실행해야한다.
var x = 1; // 전역 변수
y = 2; // 암묵적 전역
function kamja(){} // 전역 함수

// 전역 객체 window의 프로퍼티
console.log(window.x); // 1
console.log(x) // 1 <- 전역 객체 window의 프로퍼티는 전역 변수처럼 사용할 수 있다.
console.log(window.y); // 2
console.log(y); // 2
console.log(window.kamja); // f kamja(){}
console.log(kamja); // f kamja(){}

let x = 1;
console.log(window.x); // undefined
console.log(x); // 1
```

- let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
  - 즉, window.kamja와 같이 접근할 수 없다.
    - let 전역 변수는 보이지 않는 개념적인 블록(전역 렉시컬 환경의 선언적 환경 레코드) 내에 존재하게 된다.

# Const 키워드

- 상수를 선언하기 위해 사용한다.
- const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.
- const 키워드로 선언한 변수는 재할당이 금지된다.
- `const 키워드로 선언된 변수에 원시 값을 할당한 경우 원시 값은 변경할 수 없는 값이고 const 키워드에 의해 재할당이 금지되므로 할당된 값을 변경할 수 있는 방법은 없다.`
  - const 키워드로 선언된 변수에 객체를 할당한 경우 객체의 값을 변경할 수 있다.
    - 변경 가능한 값인 객체는 재할당 없이도 직접 변경이 가능하기 때문
    ```Javascript
    const person = {
        name: "kamja"
    };
    person.name="kokuma";
    console.log(person); // {name:"kokuma"}
    ```
    - `const 키워드는 재할당을 금지할 뿐 "불변"을 의미하진 않는다.`
      - 즉, 새로운 값을 재할당하는 것은 불가능하지만, 프로퍼티 동적 생성, 삭제, 프로퍼티 값의 변경을 통해 객체를 변경하는 것은 가능하다.
        - 객체가 변경되더라도 변수에 할당된 참조값은 변경되지 않는다.
