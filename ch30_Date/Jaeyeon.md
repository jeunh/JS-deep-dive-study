## Date

표준 빌트인 객체인 Date는 날짜와 시간(연, 월, 일, 시, 분, 초, 밀리초)을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다. UTC(협정 세계시, Coordinated Universal Time)는 국제 표준시를 의미하며, UTC는 GMT(그리니치 평균시)와 거의 동일하게 취급된다. UTC는 초 단위까지 동일하나 소수점 이하에서만 차이가 나며, 일반적인 환경에서는 UTC와 GMT를 혼용한다. KST(한국 표준시)는 UTC+9이다.

현재 날짜와 시간은 자바스크립트 코드가 실행되는 시스템의 시계에 의해 결정된다.

## 30.1 Date 생성자 함수

### 30.1.1 new Date()

Date 생성자 함수는 여러 방식으로 호출 가능하며, new 연산자와 함께 사용하면 Date 객체를 반환한다.

```js
new Date();
// 현재 날짜와 시간 반환
// Sun May 04 2025 13:49:19 GMT+0900 (한국 표준시) {}

Date();
// 문자열 반환
// 'Sun May 04 2025 13:50:02 GMT+0900 (한국 표준시)'
```

### 30.1.2 new Date(milliseconds)

인수로 전달한 만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환

```js
new Date(0); // 1970-01-01T00:00:00.000Z
new Date(86400000); // 1970-01-02T00:00:00.000Z
```

### 30.1.3 new Date(dateString)

날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date객체를 반환(Date.parse에 의해 해석 가능한 형식이어야 함)
```js
new Date('2020-03-26/10:00:00');
// Thu Mar 26 2020 10:00:00 GMT+0900 (한국 표준시)
```

### 30.1.4 new Date(year, month\[, day, hour, minute, second, millisecond])

Date 생성자 함수에 아래 내용을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date객체를 반환함. 연, 월은 반드시 지정해야 함. 지정하지 않으면 0이나 1로 초기화됨.

| 인수          | 내용                     |
| ----------- | ---------------------- |
| year        | 연도 (1900 이상 정수)        |
| month       | 월 (0부터 시작, 0 = 1월)     |
| day         | 일 (1부터 시작, 생략 시 기본값 1) |
| hour        | 시 (0\~23)              |
| minute      | 분 (0\~59)              |
| second      | 초 (0\~59)              |
| millisecond | 밀리초 (0\~999)           |

```js
// 월을 나타내는 2는 3월을 의미함.
new Date(2020, 2); // Sun Mar 01 2020 00:00:00 GMT+0900 (한국 표준시) {}
new Date(2020, 2, 26, 10, 0, 0); // Thu Mar 26 2020 10:00:00 GMT+0900 (한국 표준시)
// 가독성 좋게 아래처럼 나타내도 됨
new Date('2020/3/26 10:00:00'); // Thu Mar 26 2020 10:00:00 GMT+0900 (한국 표준시)
```

## 30.2 Date 메서드

### 30.2.1 Date.now

```js
const now = Date.now(); // 여기까지는 1970년 1월 1일 00:00:00 을 기점으로 현재까지 경과한 밀리초가 반환됨
new Date(now) // 여기서 현재 시간 알 수 있음
// Sun May 04 2025 13:59:31 GMT+0900 (한국 표준시)
```

### 30.2.2 Date.parse
UTC 를 기점으로 인수로 전달된 시간까지의 밀리초를 숫자로 변환함.
```js
Date.parse('Jan 2, 1970 00:00:00 UTC');     // 86400000
Date.parse('1970/01/02/09:00:00');          // 86400000
```

### 30.2.4 Date.prototype.getFullYear
연도 반환

```js
new Date('2020/07/24').getFullYear(); // 2020
```

### 30.2.5 Date.prototype.setFullYear
연도를 설정. 월과 일도 옵션으로 설정 가능

```js
const today = new Date();
today.setFullYear(2000);
today.setFullYear(1900, 0, 1);
```

### 30.2.6 Date.prototype.getMonth
월을 0부터 시작하는 숫자로 반환
```js
new Date('2020/07/24').getMonth(); // 6
```

### 30.2.7 Date.prototype.setMonth
월을 설정한다. 옵션으로 일도 설정할 수 있다.

```js
const today = new Date();
today.setMonth(0); // 1월

today.setMonth(11, 1); // 12월 1일 // Mon Dec 01 2025 14:10:27 GMT+0900 (한국 표준시)
```

### 30.2.8 Date.prototype.getDate
날짜(일)를 반환

```js
new Date('2020/07/24').getDate(); // 24
```

### 30.2.9 Date.prototype.setDate
날짜 설정

```js
const today = new Date();
today.setDate(1); // Thu May 01 2025 14:10:03 GMT+0900 (한국 표준시)
```

### 30.2.10 Date.prototype.getDay
객체의 요일을 나타내는 정수 반환

```js
new Date('2020/07/24').getDay(); // 5
```

| 요일  | 반환값 |
| --- | --- |
| 일요일 | 0   |
| 월요일 | 1   |
| 화요일 | 2   |
| 수요일 | 3   |
| 목요일 | 4   |
| 금요일 | 5   |
| 토요일 | 6   |

### 30.2.11 Date.prototype.getHours
'시' 반환

```js
new Date('2020/07/24/12:00').getHours(); // 12
```

### 30.2.12 Date.prototype.setHours
'시' 설정

```js
const today = new Date();
today.setHours(7);
```

### 30.2.13 Date.prototype.getMinutes
'분' 반환

```js
new Date('2020/07/24/12:30').getMinutes(); // 30
```

### 30.2.14 Date.prototype.setMinutes
'분'설정. 옵션으로 초, 밀리초도 설정할 수 있다.
```js
const today = new Date();
today.setMinutes(50);
today.setMinutes(5, 10, 999); // HH:05:10:999
```

### 30.2.15 Date.prototype.getSeconds
초(0~59)를 반환
```js
new Date('2020/07/24/12:30:10').getSeconds(); // 10
```

### 30.2.16 Date.prototype.setSeconds
초를 설정한다. 옵션으로 밀리초도 설정할 수 있다.
```js
const today = new Date();
today.setSeconds(30);
today.setSeconds(10, 0); // HH:MM:10:000
```

### 30.2.17 Date.prototype.getMilliseconds
밀리초(0~999)를 반환한다.
```js
new Date('2020/07/24/12:30:10:150').getMilliseconds(); // 150
```

### 30.2.19 Date.prototype.getTime
1970년 1월 1일 00:00:00 UTC부터 현재까지 경과한 밀리초를 반환한다.
```js
new Date('2020/07/24/12:30').getTime(); // 1595561400000
```

### 30.2.19 Date.prototype.setTime
1970년 1월 1일 00:00:00 UTC부터 현재까지 경과한 밀리초를 설정한다.
```js
const today = new Date(); 
today.setTime(86400000) // 하루
console.log(today)// Fri Jan 02 1970 09:00:00 GMT+0900 (한국 표준시)
```

### 30.2.22 Date.prototype.toDateString
사람이 읽기 쉬운 형식으로 날짜 문자열을 반환한다.
```js
const today = new Date('2020/7/24/12:30');
today.toDateString(); // 'Fri Jul 24 2020'
```

### 30.2.23 Date.prototype.toTimeString
사람이 읽기 쉬운 형식으로 시간 문자열을 반환한다.

```js
const today = new Date('2020/7/24/12:30');
today.toTimeString(); // '12:30:00 GMT+0900 (한국 표준시)'
```

### 30.2.24 Date.prototype.toISOString
ISO 8601 형식의 날짜 문자열을 반환한다.

```js
const today = new Date('2020/7/24/12:30');
today.toISOString(); // '2020-07-24T03:30:00.000Z'
today.toISOString().slice(0, 10); // '2020-07-24'
today.toISOString().slice(0, 10).replace(/-/g, ''); // '20200724'
```

### 30.2.25 Date.prototype.toLocaleString
로캘 기준의 날짜 및 시간 문자열을 반환한다.
```js
const today = new Date('2020/7/24/12:30');
today.toLocaleString(); // '2020. 7. 24. 오후 12:30:00'
today.toLocaleString('ko-KR'); // '2020. 7. 24. 오후 12:30:00'
today.toLocaleString('en-US'); // '7/24/2020, 12:30:00 PM'
today.toLocaleString('ja-JP'); // '2020/7/24 12:30:00'
```

### 30.2.26 Date.prototype.toLocaleTimeString

```js
const today = new Date('2020/7/24/12:30');
today.toLocaleTimeString(); // '오후 12:30:00'
today.toLocaleTimeString('ko-KR'); // '오후 12:30:00'
today.toLocaleTimeString('en-US'); // '12:30:00 PM'
today.toLocaleTimeString('ja-JP'); // '12:30:00'
```
