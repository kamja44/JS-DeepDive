1. [정규 표현식](#정규-표현식)<br>
   &nbsp;&nbsp;1-1. [휴대폰 번호 정규 표현식 테스트](#휴대폰-번호-정규-표현식-테스트)<br>
2. [정규 표현식의 생성](#정규-표현식의-생성)<br>
3. [RegEXP 메서드](#regexp-메서드)<br>
   &nbsp;&nbsp;3-1. [RegExp.prototype.exec](#regexpprototypeexec)<br>
   &nbsp;&nbsp;3-2. [RegExp.prototype.test](#regexpprototypetest)<br>
   &nbsp;&nbsp;3-3. [String.prototype.match](#stringprototypematch)<br>
4. [플래그](#플래그)<br>
5. [패턴](#패턴)<br>
   &nbsp;&nbsp;5-1. [문자열 검색](#문자열-검색)<br>
   &nbsp;&nbsp;5-2. [임의의 문자열 검색](#임의의-문자열-검색)<br>
   &nbsp;&nbsp;5-3. [반복 검색](#반복-검색)<br>
   &nbsp;&nbsp;5-4. [OR 검색](#or-검색)<br>
   &nbsp;&nbsp;5-5. [NOT 검색](#not-검색)<br>
   &nbsp;&nbsp;5-6. [시작 위치로 검색](#시작-위치로-검색)<br>
   &nbsp;&nbsp;5-7. [마지막 위치로 검색](#마지막-위치로-검색)<br>
6. [자주 사용하는 정규표현식](#자주-사용하는-정규표현식)<br>
   &nbsp;&nbsp;6-1. [특정 단어로 시작하는지 검사](#특정-단어로-시작하는지-검사)<br>
   &nbsp;&nbsp;6-2. [특정 단어로 끝나는지 검사](#특정-단어로-끝나는지-검사)<br>
   &nbsp;&nbsp;6-3. [숫자로만 이뤄진 문자열인지 검사](#숫자로만-이뤄진-문자열인지-검사)<br>
   &nbsp;&nbsp;6-4. [하나 이상의 공백으로 시작하는지 검사](#하나-이상의-공백으로-시작하는지-검사)<br>
   &nbsp;&nbsp;6-5. [아이디로 사용 가능하지 검사](#아이디로-사용-가능한지-검사)<br>
   &nbsp;&nbsp;6-6. [메일 주소 형식에 맞는지 검사](#메일-주소-형식에-맞는지-검사)<br>
   &nbsp;&nbsp;6-7. [핸드폰 번호 형식에 맞는지 검사](#핸드폰-번호-형식에-맞는지-검사)<br>
   &nbsp;&nbsp;6-8. [특수 문자 포함 여부 검사](#특수-문자-포함-여부-검사)<br>

## 정규 표현식

- 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어이다.
- 문자열을 대상으로 패턴 매칭 기능을 제공한다.
  - 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.
- 정규 표현식을 사용하면 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크할 수 있다.
  - 단, 가독성이 좋지 않다.

### 휴대폰 번호 정규 표현식 테스트

```js
// 휴대폰 전화번호
const tel = "010-1234-567감";
// 정규 표현식 리터럴로 휴대폰 전화번호 패턴 정의
const regExp = /^\d{3}-\d{4}=\d{4}$/;
// tel이 휴대폰 전화번호 패턴에 매칭하는지 테스트
regExp.test(tel); // false
```

## 정규 표현식의 생성

- 정규 표현식 객체를 생성하기 위해선 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다.
  - 일반적으로 정규 표현식 리터럴을 사용한다.
- 정규 표현식 리터럴은 패턴과 플래그로 구성된다.

```js
// 정규 표현식 리터럴
const target = "Is this all there is?";

// 패턴 : is
// 플래그: i -> 대소문자를 구별하지 않고 검색한다.
const regexp = /is/i;

// test 메서드는 target 문자열에 대하여 정규 표현식 regexp의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
regexp.test(target); // true

// RegExp 생성자 함수
const regexp2 = new RegExp(/is/i); // ES6
// const regexp2 = new RegExp(/is/,'i');
// const regexp2 = new RegExp('is','i');
regexp2.test(target); // true
```

## RegExp 메서드

### RegExp.prototype.exec

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.
  - 매칭 결과가 없는 경우 null을 반환한다.

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.exec(target); // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
```

- exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환한다.

### RegExp.prototype.test

- 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 변환한다.

```js
const target = "Is this all there is?";
const regExp = /is/;

regExp.test(target); // true
```

### String.prototype.match

- 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.
  - 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정하면 모든 매칭 결과를 배열로 반환한다.

```js
const target = "Is this all there is?";
const regExp = /is/;
const regExpG = /is/g;
target.match(regExp); // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
target.match(regExpG); // ['is', 'is']
```

## 플래그

- 정규 표현식의 검색 방식을 설정하기 위해 사용한다.
- 플래그는 선택적으로 사용할 수 있으며, 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수 있다.

| 플래그 |    의미     |                              설명                               |
| :----: | :---------: | :-------------------------------------------------------------: |
|   i    | ignore case |            대소문자를 구별하지 않고 패턴을 검색한다.            |
|   g    |   Global    | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
|   m    | Multi line  |         문자열의 행이 바뀌더라도 패턴 검색을 계속한다.          |

```js
const target = "Is this all there is?";

// is 문자열을 대소문자를 구별하여 한 번만 검색
target.match(/is/); // ['is', index: 5, input: 'Is this all there is?', groups: undefined]

// is 문자열을 대소문자 구분 하지 않고 한 번만 검색
target.match(/is/i); // ['Is', index: 0, input: 'Is this all there is?', groups: undefined]

// is 문자열을 대소문자 구별하여 전역 검색
target.match(/is/g); // ['is', 'is']

// is 문자열을 대소문자 구별하지 않고 전역 검색
target.match(/is/gi); // ['Is', 'is', 'is']
```

## 패턴

- 정규 표현식은 `패턴과 플래그`로 구성된다.
  - 정규 표현식의 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용한다.
  - 정규 표현식의 플래그는 검색 방식을 설정하기 위해 사용한다.
- 패턴은 /로 열고 닫으며 문자열의 따옴표는 생략한다.

### 문자열 검색

- 정규 표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색한다.
- 검색 대상 문자열과 플래그를 생략한 정규 표현식은 대소문자를 구별하여 정규 표현식과 매치한 첫 번째 결과만 반환한다.
  - 대소문자를 구분하지 않으려면 플래그 i를 사용한다.
  - 패턴과 매치하는 모든 문자열을 전역 검색하려면 플래그 g를 사용한다.

### 임의의 문자열 검색

- .은 문자 한개를 의미한다.

임의의 3자리 문자열을 대소문자를 구별하지 않고 전역에서 검색한다.

```js
const target = "Is this all there is?";

const regExp = /.../gi;

target.match(regExp); // ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
```

### 반복 검색

- {m,n}은 앞선 패턴이 최소 m번, 최대 n번 반복된느 문자열을 의미한다.
  - 콤마 뒤에 공백이 있으면 정상 동작하지 않는다.
- {n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.
- +는 패턴이 최소 한번 이상 반복되는 문자열을 의미한다.
  - 즉, +는 {1, }과 같다.
- ?는 앞선 패턴이 최대 한 번 이상 반복되는 문자열을 의미한다.
  - 즉, ?는 {0,1}과 같다.

```js
const target = "A AA B BB Aa Bb AAA";

// A가 최소 1번 최대 2번 반복되는 문자열을 전역 검색한다.
const regExp = /A{1,2}/g;
target.match(regExp); // ['A', 'AA', 'A', 'AA', 'A']

const regExp2 = /A{2,}/g;
target.match(regExp2); // ['AA', 'AAA']

const regExp3 = /A+/g;
target.match(regExp3); // ['A', 'AA', 'A', 'AAA']

const target2 = "color colour";

// "colo" 다음 "u"가 최대 한 번 (0번 포함)이상 반복되고 "r"이 이어지는 문자열 "color colour"을 전역 검색한다.
const regExp4 = /colou?r/g;
target.match(regExp); // ['color', 'colour']
```

### OR 검색

- |은 or의 의미를 갖는다.
  - [] 안의 문자는 or로 동작한다.
    - 그 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복한다.
  - [] 안에 -를 사용하면 범위를 지정할 수 있다.

```js
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'를 전역 검색한다.
const regExp = /A|B/g;
target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
```

- 분해되지 않는 단어 레벨로 검색하기 위해선 +를 함께 사용한다.

```js
const target = "A AA B BB Aa Bb";

// 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp = /A+|B+/g;
target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']

const regExp2 = /[AB]+/g;
target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']

const target2 = "A AA BB ZZ Aa Bb";
// A ~ Z가 한 번 이상 반복되는 문자열을 전역 검색한다.
const regExp3 = /[A-Z]+/g;
target2.match(regExp3); // ['A', 'AA', 'BB', 'ZZ', 'A', 'B']

// 대소문자를 구분하지 않고 알파벳을 검색하는 방법
const target3 = "AA BB Aa Bb 12";
// A ~ Z 또는 a ~ z가 한 번 이상 반복되는 문자열 전역 검색
const regExp4 = /[A-Za-z]+/g;
target3.match(regExp4); // ['AA', 'BB', 'Aa', 'Bb']

// 숫자 검색
const target5 = "AA BB 12,345";
// 0 ~ 9 또는 ,가 한 번 이상 반복되는 문자열 전역 검색
const regExp5 = /[0-9, ]+/g;
target5.match(regExp5); // ['12,345']
```

- \d는 숫자를 의미한다.
  - 즉, \d는 [0-9]와 동일하다.
- \D는 문자를 의미한다.

```js
const target = "AA BB 12,345";

// 0 ~ 9 또는 ,가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\d,]+/g;
target.match(regExp); // ['12,345']

// 0 ~ 9가 아닌 문자 또는 ,가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\D, ]+/g;
target.match(regExp); // ['AA BB ', ',']
```

- \w는 알파벳, 숫자, 언더스코어를 의미한다.
  - 즉, \w는 [A-Za-z0-9_]와 같다.
- \W는 문자를 의미한다.

```js
const target = "Aa Bb 12,345 _$%&";
// 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색한다.
let regExp = /[\w,]+/g;
target.match(regExp); // ['Aa', 'Bb', '12,345', '_']

// 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ,가 한 번 이상 반복되는 문자열을 전역 검색한다.
regExp = /[\W,]+/g;
target.match(regExp); // [' ', ' ', ',', ' ', '$%&']
```

### NOT 검색

- [] 안의 ^는 not의 의미를 갖는다.
  - [^0-9]는 숫자를 제외한 문자를 의미한다.

```js
const target = "AA BB 12 Aa Bb";
// 숫자를 제외한 문자열을 전역 검색한다.
const regExp = /[^0-9]+/g;
target.match(regExp); // ['AA BB ', ' Aa Bb']
```

### 시작 위치로 검색

[] 밖의 ^은 문자열의 시작을 의미한다.

```js
const target = "https://google.com";
// https로 시작하는지 확인한다.
const regExp = /^https/;
regExp.test(target); // true
```

### 마지막 위치로 검색

- $는 문자열의 마지막을 의미한다.

```js
const target = "https://google.com";
// com으로 끝나는지 검사한다.
const regExp = /com$/;
regExp.test(target); // true
```

## 자주 사용하는 정규표현식

### 특정 단어로 시작하는지 검사

- 검색 대상 문자열이 http:// 또는 https://로 시작하는지 검사한다.
  - [] 바깥의 ^은 문자열의 시작을 의미한다.
  - ?은 앞선 패턴이 최대 한 번(0번 포함)이상 반복되는지를 의미한다.

```js
const url = "https://google.com";

/^https?:\/\//.test(url); // true

// 위의 정규 표현식은 아래와 동일하다.
/^(http|https):\/\//.test(url); // true
```

### 특정 단어로 끝나는지 검사

- 검색 대상 문자열이 html로 끝나는지 검사한다.
  - $는 문자열의 마지막을 의미한다.

```js
const fileName = "index.html";

// html로 끝나는지 검사
/html$/.test(fileName); // true
```

### 숫자로만 이뤄진 문자열인지 검사

- 처음과 끝이 숫자이고 최소 한 번 이상 반복되는 문자열과 매치한다.
  - [] 바깥의 ^은 문자열의 시작을 의미한다.
  - $는 문자열의 마지막을 의미한다.
  - \d는 숫자를 의미한다.
  - +는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다.

```js
const target = "12345";
// 숫자로만 이뤄진 문자열인지 검사한다.
/^\d+$/.test(target); // true
```

### 하나 이상의 공백으로 시작하는지 검사

- 검색 대상 문자열이 하나 이상의 공백으로 시작하는지 검사한다.
  - \s는 여러 가지 공백 문자를 의미한다.
    - 즉, \s는 \t, \r, \n, \v, \f 와 같은 의미이다.

```js
const target = " Hi!";

// 하나 이상의 공백으로 시작하는지 검사한다.
/^[\s]+/.test(target); // true
```

### 아이디로 사용 가능한지 검사

- 검색 대상 문자열이 알파벳 대소문자 또는숫자로 시작하고 끝나며 4 ~ 10 자리인지 검사한다.

```js
const id = "abc123";

// 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4 ~ 10자리인지 검사한다.
/^[A-Za-z0-9]{4, 10}$/.test(id); // true
```

### 메일 주소 형식에 맞는지 검사

```js
const email = "kamja@gmail.com";

/^[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*@[0-9a-zA-Z]([-_\.]?[0-9a-zA-Z])*\.[a-zA-Z]{2,3}$/.test(
  email
); // true
```

RFC 5322에 맞는 정교한 패턴

```js
(?:[a-z0-9!#$%&'*+/=?^_`{|}~-]+(?:\.[a-z0-9!#$%&'*+/=?^_`{|}~-]+)*|"(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21\x23-\x5b\x5d-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])*")@(?:(?:[a-z0-9](?:[a-z0-9-]*[a-z0-9])?\.)+[a-z0-9](?:[a-z0-9-]*[a-z0-9])?|\[(?:(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?)\.){3}(?:25[0-5]|2[0-4][0-9]|[01]?[0-9][0-9]?|[a-z0-9-]*[a-z0-9]:(?:[\x01-\x08\x0b\x0c\x0e-\x1f\x21-\x5a\x53-\x7f]|\\[\x01-\x09\x0b\x0c\x0e-\x7f])+)\]).test(email);
```

### 핸드폰 번호 형식에 맞는지 검사

```js
const cellphone = "010-1234-5678";

/^\d{3}-\d{3,4}-\d{4}$/.test(cellphone); // true
```

### 특수 문자 포함 여부 검사

```js
const target = "abc#123";

/[^A-Za-z0-9]/gi.test(target); // true
```

위의 코드는 아래의 코드와 같다.

- 아래의 코드는 특수문자를 선택적으로 검사할 수 있다.

```js
const target = "abc#123";

/[\{\}\[\]\/?.,;:|\)*~`!^\-_+<>@\#$%&\\\=\(\'\"]/gi.test(target); // true
```

- 특수문자를제거할 때는 String.prototype.replace 메서드를 사용한다.

```js
const target = "abc#123";
// 특수 문자를 제거한다.
target.replace(/[^A-Za-z0-9]/gi, ""); // abc123
```
