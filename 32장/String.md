1. [String 생성자 함수](#string-생성자-함수)<br>
2. [Length 프로퍼티](#length-프로퍼티)<br>
3. [String 메서드](#string-메서드)<br>
   &nbsp;&nbsp;3-1. [String.prototype.indexOf](#stringprototypeindexof)<br>
   &nbsp;&nbsp;3-2. [String.prototype.search](#stringprototypesearch)<br>
   &nbsp;&nbsp;3-3. [String.prototype.includes](#stringprototypeincludes)<br>
   &nbsp;&nbsp;3-4. [String.prototype.startsWith](#stringprototypestartswith)<br>
   &nbsp;&nbsp;3-5. [String.prototype.endsWith](#stringprototypeendswith)<br>
   &nbsp;&nbsp;3-6. [String.prototype.charAt](#stringprototypecharat)<br>
   &nbsp;&nbsp;3-7. [String.prototype.substring](#stringprototypesubstring)<br>
   &nbsp;&nbsp;3-8. [String.prototype.slice](#stringprototypeslice)<br>
   &nbsp;&nbsp;3-9. [String.prototype.toUpperCase](#stringprototypetouppercase)<br>
   &nbsp;&nbsp;3-10. [String.prototype.toLowerCase](#stringprototypetolowercase)<br>
   &nbsp;&nbsp;3-11. [String.prototype.trim](#stringprototypetrim)<br>
   &nbsp;&nbsp;3-12. [String.prototype.repeat](#stringprototyperepeat)<br>
   &nbsp;&nbsp;3-13. [String.prototype.replace](#stringprototypereplace)<br>
   &nbsp;&nbsp;3-14. [String.prototype.split](#stringprototypesplit)<br>

## String 생성자 함수

- String 객체는 생성자 함수 객체이다.
  - new 연산자와 함께 호출하여 String 인스턴스를 생성할 수 있다.
- String 생성자 함수의 인수로 문자열을 전달하면서 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 인수로 전달받은 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
const strObj = new String("kamja");
console.log(strObj); // String {'kamja'}0:"k"1:"a"2:"m"3:"j"4: "a"length: 5[[Prototype]]: String[[PrimitiveValue]]: "kamja"
```

- String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블이다.
  - 즉, 배열과 유사하게 인덱스를 사용하여 각 문자에 접근할 수 있다.
  - 단, 문자열은 원시값이므로 변경할 수 없다.

```js
const strObj = new String("kamja");
console.log(strObj[0]); // k

// 문자열은 원시값이므로 변경할 수 없다.(에러발생 x)
strObj[0] = "S";
console.log(strObj); // kamja
```

- String 생성자 함수의 인수로 문자열이 아닌 값을 전달하면 인수를 강제로 형변환 후, [[StringData]] 내부 슬롯에 변환된 문자열을 할당한 String 래퍼 객체를 생성한다.

```js
let str = new String(123);
console.log(str); // String {'123'}0: "1"1: "2"2: "3"length: 3[[Prototype]]: String[[PrimitiveValue]]: "123"
```

- new 연산자를 사용하지 않고 String 생성자 함수를 호출하면 String 인스턴스가 아닌 문자열을 반환한다.
  - 이를 이용하여 명시적 타입 변환을 수행할 수 있다.

```js
String(1); // "1"
Strng(NaN); // "NaN"
String(true); // "true"
```

## length 프로퍼티

- 문자열의 문자 개수를 반환한다.

```js
"Hello".length; // 5
```

## String 메서드

- String 객체의 메서드는 언제나 새로운 문자열을 반환한다.
- 문자열을 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공된다.

```js
const str = new String("kamja");

console.log(Object.getOwnPropertyDescriptors(str));
// 0: {value: 'k', writable: false, enumerable: true, configurable: false}
// 1: {value: 'a', writable: false, enumerable: true, configurable: false}
// 2: {value: 'm', writable: false, enumerable: true, configurable: false}
// 3: {value: 'j', writable: false, enumerable: true, configurable: false}
// 4: {value: 'a', writable: false, enumerable: true, configurable: false}
// length: {value: 5, writable: false, enumerable: false, configurable: false}
```

- `String 객체의 메서드는 언제나 새로운 문자열을 생성하여 반환한다.`

### String.prototype.indexOf

- 대상 문자열에서 인수로 전달받은 문자열을 검색하여 첫 번째 인덱스를 반환한다.
  - 검색 실패시 -1을 반환한다.
- indexOf 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```js
const str = "Hello World";

// l을 검색하여 첫 번째 인덱스를 반환한다.
str.indexOf("l"); // 2
// 검색에 실패하면 -1을 반환한다.
str.indexOf("x"); // -1

// 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.
str.indexOf("l", 3); // 3
```

### String.prototype.search

- 대상 문자열에서 인수로 전달받은 `정규 표현식과 매치하는 문자열을 검색`하여 일치하는 문자열의 인덱스를 반환한다.
  - 검색 실패시 -1을 반환한다.

```js
const str = "Hello world";

str.search(/o/); // 4
str.search(/x/); // -1
```

### String.prototype.includes

- 대상 문자열에 인수로 전달받은 문자열이 포함되어 있는지 확인하여 true, false를 반환한다.
  - includes 메서드의 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```js
const str = "Hello world";

str.includes("Hello"); // true
str.includes("x"); // false

str.includes("l", 3); // true
str.includes("H", 3); // false
```

### String.prototype.startsWith

- 대상 문자열이 인수로 전달받은 문자열로 시작하는지 확인하여 true, false로 반환한다.
  - 2번째 인수로 검색을 시작할 인덱스를 전달할 수 있다.

```js
const str = "Hello world";

str.startsWith("He"); // true
str.startsWith("x"); // false
str.startsWith(" ", 5); // true
```

### String.prototype.endsWith

- 대상 문자열이 인수로 전달받은 문자열로 끝나는지 확인하여 true, false로 반환한다.
  - 2번째 인수로 검색할 문자열의 길이를 전달할 수 있다.

```js
const str = "Hello world";

str.endWith("ld"); // true
str.endWith("x"); // false
str.endWith("lo", 5); // true
```

### String.prototype.charAt

- 대상 문자열에서 인수로 전달받은 인덱스에 위치한 문자를 검색하여 반환한다.
  - 인덱스가 문자열의 범위를 벗어난 정수인 경우 빈 문자열을 반환한다.

```js
const str = "Hello";
for (let i = 0; i < str.length; i++) {
  console.log(str.charAt(i)); // H e l l o
}

console.log(str.charAt(6)); // ""
```

### String.prototype.substring

- 대상 문자열에서 첫 번째 인수로 전달받은 인덱스에 위치하는 문자부터 두 번째 인수로 전달받은 인덱스에 위치하는 문자의 바로 이전 문자까지의 부분 분자열을 반환한다.
  - 두 번째 인수를 생략하면 첫 번째 인수의 인덱스부터 마지막 문자까지 부분 문자열을 반환한다.
    - 두 번째 인수는 첫 번째 인수보다 커야 정상이다.
      - 하지만, 다음과 같이 인수를 전달하여도 정상 동작한다.
        - 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
        - 인수 < 0 또는 NaN인 경우 0으로 취급된다.
        - 인수 > 문자열의 길이인 경우 인수는 문자열의 길이로 취급된다.

```js
const str = "Hello World";
// 인덱스 1부터 4 이전까지의 부분 문자열 반환
str.substring(1, 4); // ell

str.substring(1); // ello World

// 첫 번째 인수 > 두 번째 인수인 경우 두 인수는 교환된다.
str.substring(4, 1); // ell

// 인수 < 0 또는 NaN인 경우 0으로 취급된다.
str.substring(-2); // Hello World
// 인수 > 문자열의 길이인 경우 인수는 문자열의 길이로 취급된다.
str.substring(1, 10); // ello World
str.substring(20); // ""
```

### String.prototype.slice

- substring 메서드와 동일하게 동작한다.
  - 단, slice 메서드에 음수를 전달할 수 있다.
  - 음수 전달 시 대상 문자열의 가장 뒤에서부터 시작하여 문자열을 잘라내어 반환한다.

```js
const str = "Hello World";

str.slice(0, 5); // hello
str.slice(2); // llo World
str.slice(-5); // World
```

### String.prototype.toUpperCase

- 대상 문자열을 모두 대문자로 변경한 문자열을 반환한다.

```js
const str = "Hello world";
str.toUpperCase(); // HELLO WORLD
```

### String.prototype.toLowerCase

- 대상 문자열을 모두 소문자로 변경한 문자열을 반환한다.

```js
const str = "Hello world";
str.toLowerCase(); // hello world
```

### String.prototype.trim

- 대상 문자열 앞뒤에 공백 문자가 있을 경우 이를 제거한 문자열을 반환한다.
  - trimStart를 사용하면 앞의 공백을 제거한다.
  - trimEnd를 사용하면 뒤의 공백을 제거한다.

```js
const str = "          kamja          ";
str.trim(); // kamja
```

### String.prototype.repeat

- 대상 문자열을 인수로 전달받은 만큼 반복하여 연결한 새로운 문자열을 반환한다.
- 인수로 전달받은 정수가 0이면 빈 문자열을 반환하고, 음수이면 RangeError를 발생시킨다.
  - 인수를 생략하면 기본값 0이 설정된다.

```js
const str = "abc";
str.repeat(0); // ""
str.repeat(1); // "abc"
str.repeat(2); // "abcabc"
```

### String.prototype.replace

- 첫 번째 인수로 전달받은 문자열 또는 정규표현식을 검색하여 두 번째 인수로 전달한 문자열로 치환한 문자열을 반환한다.
  - 검색된 문자열이 여럿 존재한다면, 첫 번째로 검색된 문자열만 치환된다.

```js
const str = "Hello world";
str.replace("world", "Kamja"); // Hello Kamja
```

- 특수한 교체 패턴

```js
const str = "Hello world";

str.replace("world", "<strong>$&</strong>"); // 'Hello <strong>world</strong>'
```

- 카멜 케이스 => 스네이크 케이스 변환

```js
function camelToSnake(camelCase) {
  return camelCase.replace(/.[A-Z]/g, (match) => {
    console.log(match);
    return match[0] + "_" + match[1].toLowerCase();
  });
}
const camelCase = "helloWorld";
camelToSnake(camelCase); // 'hello_world'
```

- 스네이크 케이스 => 카멜 케이스 변환

```js
function snakeToCamel(snakeCase) {
  return snakeCase.replace(/_[a-z]/g, (match) => {
    console.log(match);
    return match[1].toUpperCase();
  });
}
const snakeCase = "hello_world";
snakeToCamel(snakeCase); // helloWorld
```

### String.prototype.split

- 대상 문자열에서 첫 번째 인수로 전달한 문자열 또는 정규 표현식을 검색하여, 문자열을 구분한 후 분리된 각 문자열로 이뤄진 배열을 반환한다.
  - 인수로 빈 문자열을 전달하면 각 문자를 모두 분리하고, 인수를 생략하면 대상 문자열 전체를 단일 요소로 하는 배열을 반환한다.
  - 두 번째 인수로 배열의길이를 지정할 수 있다.

```js
const str = "How are you doing?";
// 공백으로 구분하여 배열 반환
str.split(" "); // ['How', 'are', 'you', 'doing?']
str.split(""); // ['H', 'o', 'w', ' ', 'a', 'r', 'e', ' ', 'y', 'o', 'u', ' ', 'd', 'o', 'i', 'n', 'g', '?']

str.split(" ", 3); // ['How', 'are', 'you']
```

- Array.prototype.reverse, Array.prototype.join 메서드와 함께 사용하면 문자열을 역순으로 뒤집을 수 있다.

```js
// 인수로 전달받은 문자열을 역순으로 뒤집는다.
function reverseString(str) {
  return str.split("").reverse().join("");
}

reverseString("Hello world!"); // '!dlrow olleH'
```
