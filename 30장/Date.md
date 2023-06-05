1. [Date 생성자 함수](#date-생성자-함수)<br>
   &nbsp;&nbsp;1-1. [new Date()](#new-date)<br>
   &nbsp;&nbsp;1-2. [new Date(milliseconds)](#new-datemilliseconds)<br>
   &nbsp;&nbsp;1-3. [new Date(dateString)](#new-datedatestring)<br>
   &nbsp;&nbsp;1-4. [new Date(year, month, ...)](#new-dateyear-month)<br>
2. [Date 메서드](#date-메서드)<br>
   &nbsp;&nbsp;2-1. [Date.now](#datenow)<br>
   &nbsp;&nbsp;2-2. [Date.parse](#dateparse)<br>
   &nbsp;&nbsp;2-3. [Date.UTC](#dateutc)<br>
   &nbsp;&nbsp;2-4. [Date.prototype.getFullYear](#dateprototypegetfullyear)<br>
   &nbsp;&nbsp;2-5. [Date.prototype.setFullYear](#dateprototypesetfullyear)<br>
   &nbsp;&nbsp;2-6. [Date.prototype.getMonth](#dateprototypegetmonth)<br>
   &nbsp;&nbsp;2-7. [Date.prototype.setMonth](#dateprototypesetmonth)<br>
   &nbsp;&nbsp;2-8. [Date.prototype.getTimezoneOffset](#dateprototypegettimezoneoffset)<br>
   &nbsp;&nbsp;2-9. [Date.prototype.toDateString](#dateprototypetodatestring)<br>
   &nbsp;&nbsp;2-10. [Date.prototype.toTimeString](#dateprototypetotimestring)<br>
   &nbsp;&nbsp;2-11. [Date.prototype.toISOString](#dateprototypetoisostring)<br>
   &nbsp;&nbsp;2-12. [Date.prototype.toLocaleString](#dateprototypetolocalestring)<br>
   &nbsp;&nbsp;2-13. [Date.prototype.toLocaleTimeString](#dateprototypetolocaletimestring)<br>
3. [Date를 활용한 시계 예제](#date를-활용한-시계-예제)<br>

# Date 생성자 함수

- Date 생성자 함수로 생성한 Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 갖는다.
  - 이 값은 1970년 1월 1일 00:00:00(UTC)을 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다.

## new Date()

- Date 생성자 함수를 인수 없이 new 연산자와 함께 호출하면 현재 날짜와 시간을 갖는 Date 객체를 반환한다.
- Date 생성자 함수를 new 연산자 없이 호출하면 Date 객체를 반환하지 않고 날짜와 시간 정보를 나타내는 문자열을 반환한다.

## new Date(milliseconds)

- Date 생성자 함수에 숫자 타입의 밀리초를 인수로 전달하면 1970년 1월 1일 00:00:0(UTC)을 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.

## new Date(dateString)

- Date 생성자 함수에 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

```js
new Date("JUN 5, 2023 09:00:00"); // Mon Jun 05 2023 09:00:00 GMT+0900 (한국 표준시)
```

## new Date(year, month, ...)

- Date 생성자 함수에 연, 월, 일, 시, 분, 초, 밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.

  - 이때 연, 월은 반드시 지정해야 한다. - 연, 월을 지정하지 않을 경우 1970년 1월 1일 00:00:00(UTC)을 나타내는 Date 객체를 반환한다.

|    인수     |                           내용                           |
| :---------: | :------------------------------------------------------: |
|    year     | 1900년 이후의 정수, 0부터 99는 1900부터 1999로 처리된다. |
|    month    |         월을 나타내는 0 ~ 11까지의 정수(0 = 1월)         |
|     day     |             일을 나타내는 1 ~ 31까지의 정수              |
|    hour     |             시를 나타내는 0 ~ 23까지의 정수              |
|   minute    |              분을 나타내는 0~ 59까지의 정수              |
|   second    |             초를 나타내는 0 ~ 59까지의 정수              |
| millisecond |           밀리초를 나타내는 0 ~ 999까지의 정수           |

# Date 메서드

## Date.now

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 현재 시간까지 경과한 밀리초를 숫자로 반환한다.

## Date.parse

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간 까지의 밀리초를 숫자로 반환한다.

## Date.UTC

- 1970년 1월 1일 00:00:00(UTC)을 기점으로 인수로 전달된 지정 시간 까지의 밀리초를 숫자로 반환한다.

## Date.prototype.getFullYear

- Date 객체의 연도를 나타내는 정수를 반환한다.

```js
new Date("2023/06/05").getFullYear(); // 2023
```

## Date.prototype.setFullYear

- Date 객체에 연도를 나타내는 정수를 설정한다.

```js
const today = new Date();

today.setFullYear(2023);
today.getFullYear(); // 2023
```

## Date.prototype.getMonth

- Date 객체의 월을 나타내는 0 ~ 11의 정수를 반환한다.
  - 1월은 0이다.
- getDate, getDay, getHours, getMinutes, getSeconds, getMilliseconds, getTime도 동일하다.

## Date.prototype.setMonth

- Date 객체에 월을 나타내는 0 ~ 11의 정수를 설정한다.
- setDate, setDay, setHours, setMinutes, setSeconds, setMilliseconds, setTime도 동일하다.

```js
const today = new Date();

today.setMonth(6);
today.getMonth(); // 7
```

## Date.prototype.getTimezoneOffset

- UTC와 Date 객체에 지정된 locale 시간과의 차이를 분 단위로 반환한다.

## Date.prototype.toDateString

- 문자열로 Date 객체의 날짜를 반환한다.

```js
const today = new Date("2023/6/05/09:10");

today.toString(); // 'Mon Jun 05 2023 09:10:00 GMT+0900 (한국 표준시)'
today.toDateString(); // 'Mon Jun 05 2023'
```

## Date.prototype.toTimeString

- Date 객체의 시간을 표현한 문자열을 반환한다.

```js
const today = new Date("2023/6/05/09:10");

today.toString(); // 'Mon Jun 05 2023 09:10:00 GMT+0900 (한국 표준시)'
today.toTimeString(); // '09:10:00 GMT+0900 (한국 표준시)'
```

## Date.prototype.toISOString

- ISO 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

```js
const today = new Date("2023/6/05/09:10");

today.toString(); // 'Mon Jun 05 2023 09:10:00 GMT+0900 (한국 표준시)'
today.toISOString(); // '2023-06-05T00:10:00.000Z'
```

## Date.prototype.toLocaleString

- 인수로 전달한 locale을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.

```js
const today = new Date("2023/6/05/09:10");

today.toString(); // 'Mon Jun 05 2023 09:10:00 GMT+0900 (한국 표준시)'
today.toLocaleString(); // '2023. 6. 5. 오전 9:10:00'
today.toLocaleString("ko-KR"); // '2023. 6. 5. 오전 9:10:00'
```

## Date.prototype.toLocaleTimeString

- 인수로 전달한 locale을 기준으로 Date 객체의 시간을 표현한 문자열을 반환한다.

```js
const today = new Date("2023/6/05/09:10");

today.toString(); // 'Mon Jun 05 2023 09:10:00 GMT+0900 (한국 표준시)'
today.toLocaleTimeString(); // '오전 9:10:00'
today.toLocaleTimeString("ko-KR"); // '오전 9:10:00'
```

# Date를 활용한 시계 예제

- 현재 날짜와 시간을 초 단위로 반복 출력한다.

```js
(function printNow() {
  const today = new Date();

  const dayNames = [
    "(일요일)",
    "(월요일)",
    "(화요일)",
    "(수요일)",
    "(목요일)",
    "(금요일)",
    "(토요일)",
  ];

  const day = dayNames[today.getDay()];
  const year = today.getFullYear();
  const month = today.getMonth() + 1;
  const date = today.getDate();
  let hour = today.getHours();
  let minute = today.getMinutes();
  let second = today.getSeconds();
  const ampm = hour >= 12 ? "PM" : "AM";

  // 12시간제로변경
  hour %= 12;
  hour = hour || 12; // hour가 0이면 12를 재할당

  // 10 미만인 분과 초를 2자리로 변경
  minute = minute < 10 ? "0" + minute : minute;
  second = second < 10 ? "0" + second : second;

  const now = `${year}년 ${month}월 ${date}일 ${day} ${hour} : ${minute} : ${second} ${ampm}`;
  console.log(now);

  // 1초마다 printNow 함수를 재귀 호출한다.
  setTimeout(printNow, 1000);
})();
```
