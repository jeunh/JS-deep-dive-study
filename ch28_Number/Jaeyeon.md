## 28.1 Number 생성자 함수

자바스크립트의 표준 빌트인 객체 Number는 숫자 처리를 유용하게 지원하는 프로퍼티와 메서드를 제공함.

### Number 생성자 함수

- 표준 빌트인 객체인 Number는 생성자 함수임.
- `new` 연산자와 함께 호출할 경우 Number 래퍼 객체를 생성.
- 인수 없이 `new Number()` 호출 시 [[NumberData]] 내부 슬롯의 값은 0이 됨.

```js
const numObj = new Number();
console.log(numObj); // Number {[[PrimitiveValue]]: 0}
```

- 인수에 숫자를 전달하면 해당 숫자를 내부 슬롯에 할당함.
- 인수가 숫자가 아니면 숫자로 변환 후 할당함. 변환할 수 없으면 NaN이 됨.

```js
const numObj1 = new Number(10);
console.log(numObj1); // Number {[[PrimitiveValue]]: 10}

const numObj2 = new Number('10');
console.log(numObj2); // Number {[[PrimitiveValue]]: 10}

const numObj3 = new Number('Hello');
console.log(numObj3); // Number {[[PrimitiveValue]]: NaN}
```

- 문자열이나 불리언 타입을 Number로 변환하는 경우

```js
Number('0');    // 0
Number('-1');   // -1
Number('10.53'); // 10.53
Number(true);   // 1
Number(false);  // 0
```

---

## 28.2 Number 프로퍼티

### 28.2.1 Number.EPSILON

- 1과 1보다 큰 수 사이에서 표현할 수 있는 가장 작은 값.

```js
 // 0.1과 0.2는 2진수(컴퓨터 내부 숫자 표현 방식)로 정확하게 표현할 수 없는 소수임
// 부동소수점을 표현하는 표준인 IEEE 754는 2전봅으로 변환했을 때 무한소수가 되어 미세한 오차 발생
0.1 + 0.2;             // 0.30000000000000004
0.1 + 0.2 === 0.3;     // false
```

- 부동소수점 오차를 해결하기 위해 Number.EPSILON을 사용.
```
컴퓨터는 소수를 정확히 표현하지 못하고 근삿값으로 저장함.
이로 인해 0.1 + 0.2처럼 부동소수점 오차가 발생할 수 있음.
Number.EPSILON은 이런 미세한 오차를 비교할 때 기준값으로 사용함.
```

```
1에 정말 작은 숫자를 더했을 때
그 결과가 1과 구분될 수 있는 가장 작은 크기가 바로 Number.EPSILON 임.
```

```js
function isEqual(a, b) {   
    // 그냥 a === b로 하면 false가 뜨니까 a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정하기로 함
  return Math.abs(a - b) < Number.EPSILON;
}

isEqual(0.1 + 0.2, 0.3); // true
```


### 28.2.2 Number.MAX_VALUE

- 표현할 수 있는 가장 큰 숫자값.

```js
Number.MAX_VALUE; // 1.7976931348623157e+308
Infinity > Number.MAX_VALUE; // true
```

### 28.2.3 Number.MIN_VALUE

- 표현할 수 있는 가장 작은 양수값.

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

- 양의 무한대.

```js
Number.POSITIVE_INFINITY; // Infinity
```

### 28.2.7 Number.NEGATIVE_INFINITY

- 음의 무한대.

```js
Number.NEGATIVE_INFINITY; // -Infinity
```

### 28.2.8 Number.NaN

- Not-a-Number를 나타내는 값.

```js
Number.NaN; // NaN
```

---

## 28.3 Number 메서드

### 28.3.1 Number.isFinite

- 인수가 정상적인 유한수인지 검사함.
- Infinity나 -Infinity는 false를 반환함.

```js
Number.isFinite(0);           // true
Number.isFinite(Number.MAX_VALUE); // true
Number.isFinite(Number.MIN_VALUE); // true

Number.isFinite(Infinity);    // false
Number.isFinite(-Infinity);   // false
Number.isFinite(NaN);         // false
```

- 빌트인 전역 함수 `isFinite`와 차이점
  - 전역 `isFinite`는 입력값을 숫자로 변환한 후 검사함.

```js
Number.isFinite(null); // false (변환 없이 검사)
isFinite(null);        // true (0으로 변환 후 검사)
```

### 28.3.2 Number.isInteger

- 인수가 정수인지 검사해 불리언 값으로 반환함.
- 전달받은 인수를 변환하지 않고 검사함.
```js
Number.isInteger(0);       // true
Number.isInteger(123);     // true
Number.isInteger(-123);    // true

Number.isInteger(0.5);     // false
Number.isInteger('123');   // false
Number.isInteger(false);   // false
Number.isInteger(Infinity);// false
Number.isInteger(-Infinity);// false
```

### 28.3.3 Number.isNaN
- 빌트인 전역 함수 isNaN은 암묵적 타입 변환을 하는데 이것과 다름 주의
- 전달받은 인수의 숫자값이 NaN.인지 판별 후 불리언 값으로 반환함 
```js
Number.isNaN(NaN); // true
Number.isNaN(undefined); // false
isNaN(undefined);        // true
```