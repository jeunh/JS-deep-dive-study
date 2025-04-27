## 28.1 Number 생성자 함수

표준 빌트인 객체 Number는 원시 타입인 숫자를 다룰 때 유용한 프로퍼티와 메서드를 제공.

### Number 생성자 함수

- 표준 빌트인 객체인 Number 객체는 생성자 함수 객체.
- `new` 연산자와 함께 호출할 경우 Number 래퍼 객체를 생성.
- 인수 없이 `new Number()` 호출 시 [[NumberData]] 내부 슬롯 값은 0이 됨.

```js
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```

- 인수로 숫자를 전달하면 해당 값을 할당함.
- 숫자가 아닌 값을 전달하면 강제 변환 후 할당. 변환 불가능할 경우 NaN 할당.

```js
const numObj1 = new Number(10);
console.log(numObj1); // Number {[[PrimitiveValue]]: 10}

const numObj2 = new Number('10');
console.log(numObj2); // Number {[[PrimitiveValue]]: 10}

const numObj3 = new Number('Hello');
console.log(numObj3); // Number {[[PrimitiveValue]]: NaN}
```

- 명시적 타입 변환 예시

```js
Number('0');     // 0
Number('-1');    // -1
Number('10.53'); // 10.53
Number(true);    // 1
Number(false);   // 0
```

## 28.2 Number 프로퍼티

### 28.2.1 Number.EPSILON

- 1과 1보다 큰 수 사이에서 표현할 수 있는 가장 작은 값.
- 부동소수점 연산의 오차를 해결할 때 사용.

```js
0.1 + 0.2;             // 0.30000000000000004
0.1 + 0.2 === 0.3;     // false
```

```js
function isEqual(a, b) {
  // a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정
  return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // true
```

- 부동소수점은 2진수로 실수를 표현하는 방식으로, 무한히 정확히 표현할 수 없는 경우가 많음.
- 1과 1보다 큰 수 사이에서 표현 가능한 가장 작은 간격을 Number.EPSILON으로 정의함.
- 이 값을 기준으로 수의 근접성을 비교할 수 있음.

### 28.2.2 Number.MAX_VALUE

- 자바스크립트에서 표현할 수 있는 가장 큰 숫자 값.

```js
Number.MAX_VALUE; // 1.7976931348623157e+308
Infinity > Number.MAX_VALUE; // true
```

### 28.2.3 Number.MIN_VALUE

- 표현할 수 있는 가장 작은 양수 값.

```js
Number.MIN_VALUE; // 5e-324
Number.MIN_VALUE > 0; // true
```

### 28.2.4 Number.MAX_SAFE_INTEGER

- 안전하게 표현할 수 있는 가장 큰 정수값.

```js
Number.MAX_SAFE_INTEGER; // 9007199254740991
```

### 28.2.5 Number.MIN_SAFE_INTEGER

- 안전하게 표현할 수 있는 가장 작은 정수값.

```js
Number.MIN_SAFE_INTEGER; // -9007199254740991
```

### 28.2.6 Number.POSITIVE_INFINITY

- 양의 무한대를 나타내는 값.

```js
Number.POSITIVE_INFINITY; // Infinity
```

### 28.2.7 Number.NEGATIVE_INFINITY

- 음의 무한대를 나타내는 값.

```js
Number.NEGATIVE_INFINITY; // -Infinity
```

### 28.2.8 Number.NaN

- 숫자가 아님(Not-a-Number)을 나타내는 값.

```js
Number.NaN; // NaN
```

## 28.3 Number 메서드

### 28.3.1 Number.isFinite

- 인수가 정상적인 유한수인지 검사하여 불리언 값 반환.

```js
Number.isFinite(0);           // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true
Number.isFinite(Infinity);    // false
Number.isFinite(-Infinity);   // false
Number.isFinite(NaN);         // false
```

- 빌트인 전역 함수 `isFinite`는 전달된 값을 숫자로 변환 후 검사하는 반면, `Number.isFinite`는 변환하지 않음.

```js
Number.isFinite(null); // false
isFinite(null);        // true
```

### 28.3.2 Number.isInteger

- 인수가 정수인지 검사하여 불리언 값 반환.
- 인수를 변환하지 않고 검사.

### 28.3.3 Number.isNaN

- 빌트인 전역 함수 isNaN은 암묵적 타입 변환을 하는데 이것과 다름 주의
- 전달받은 인수의 숫자값이 NaN인지 판별 후 불리언 값으로 반환함

```js
Number.isNaN(NaN); // true
Number.isNaN(undefined); // false
isNaN(undefined);        // true
```

### 28.3.4 Number.isSafeInteger

- 인수가 안전한 정수인지 검사하여 불리언 값 반환.
- 안전한 정수는 -(2^53 - 1)과 2^53 - 1 사이의 값.

```js
Number.isSafeInteger(0);                // true
Number.isSafeInteger(1000000000000000);  // true
Number.isSafeInteger(0.5);               // false
Number.isSafeInteger('123');             // false
Number.isSafeInteger(Infinity);          // false
```

### 28.3.4 Number.prototype.toExponential

- 숫자를 지수 표기법으로 문자열로 변환.
- 지수 표기법은 매우 크거나 작은 수를 e(Exponent) 앞에 10의 승을 곱하는 형식으로 나타냄.

```js
(77.1234).toExponential();   // "7.71234e+1"
(77.1234).toExponential(4);  // "7.7123e+1"
(77.1234).toExponential(2);  // "7.71e+1"
```

- 숫자 리터럴 뒤에 `.`이 바로 나오면 프로퍼티 접근으로 해석되므로 괄호로 감싸야 함.

```js
(77).toExponential(); // "7.7e+1"
```

- 인수가 안전한 정수인지 검사하여 불리언 값 반환.
- 안전한 정수는 -(2^53 - 1)과 2^53 - 1 사이의 값.

```js
Number.isSafeInteger(0);                // true
Number.isSafeInteger(1000000000000000);  // true
Number.isSafeInteger(0.5);               // false
Number.isSafeInteger('123');             // false
Number.isSafeInteger(Infinity);          // false
```

### 28.3.4 Number.prototype.toExponential

- 숫자를 지수 표기법으로 문자열로 변환.

```js
(77.1234).toExponential();   // "7.71234e+1"
(77.1234).toExponential(4);  // "7.7123e+1"
(77.1234).toExponential(2);  // "7.71e+1"
```

- 주의: 숫자 리터럴 뒤에 `.`이 바로 나오면 프로퍼티 접근으로 해석되므로 괄호로 감싸야 함.

```js
(77).toExponential(); // "7.7e+1"
```

### 28.3.5 Number.prototype.toFixed

- 숫자를 고정 소수점 표기법으로 문자열로 변환.

```js
(12345.6789).toFixed();    // "12346"
(12345.6789).toFixed(1);   // "12345.7"
(12345.6789).toFixed(2);   // "12345.68"
(12345.6789).toFixed(3);   // "12345.679"
```

### 28.3.6 Number.prototype.toPrecision

- 인수로 전달받은 전체 자릿수까지 유효하게 표현하여 문자열로 변환.

```js
(12345.6789).toPrecision();    // "12345.6789"
(12345.6789).toPrecision(1);   // "1e+4"
(12345.6789).toPrecision(2);   // "1.2e+4"
(12345.6789).toPrecision(6);   // "12345.7"
```

### 28.3.7 Number.prototype.toString

- 숫자를 문자열로 변환해 반환. 진법을 지정할 수 있음.

```js
(10).toString();    // "10"
(10).toString(2);   // "1010"
(16).toString(8);   // "20"
(16).toString(16);  // "10"
```

