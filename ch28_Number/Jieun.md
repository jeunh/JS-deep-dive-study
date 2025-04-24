## 28.1 Number 생성자 함수
- Number 객체는 생성자 함수 객체이므로 new 연산자와 함께 호출하여 Number 인스턴스를 생성할 수 있다.
  ```
  // Number 래퍼 객체 생성
  let numObj = new Number();
  console.log(numObj); // Number {[[PrimitiveValue]]: 0}

  numObj = new Number(10);
  console.log(numObj); // Number {[[PrimitiveValue]]: 10}

  numObj = new Number('10');
  console.log(numObj); // Number {[[PrimitiveValue]]: 10}

  numObj = new Number('Hello');
  console.log(numObj); // Number {[[PrimitiveValue]]: NaN}
  ```
- new 연산자를 사용하지 않고 Number 생성자 함수를 호출하면 Number 인스턴스가 아닌 숫자를 반환한다.
  ```
  Number('0');     // → 0
  Number('-1');    // → -1
  Number('10.53'); // → 10.53
  Number('true');  // → 1
  ```

## 28.2 Number 프로퍼티

### 28.2.1 Number.EPSILON
- Number.EPSILON은 부동소수점으로 인해 발생하는 오차를 해결하기 위해 사용한다.
- Number.EPSILON은 약 2.2204460492503130808472633361816E-16 이다.
  ```
  console.log(0.1 + 0.2); // 0.30000000000000004
  console.log(0.1 + 0.2 === 0.3); // false
  
  function isEqual(a, b){
    // a와 b를 뺀 값의 절대값이 Number.EPSILON보다 작으면 같은 수로 인정한다.
    return Math.abs(a - b) < Number.EPSILON;
  }
  isEqual(0.1 + 0.2, 0.3); // → true
  ```

### 28.2.2 Number.MAX_VALUE
- 자바스크립트에서 표현할 수 있는 가장 큰 양수 값
  ```
  console.log(Number.MAX_VALUE); // 1.7976931348623157e+308
  ```
  
### 28.2.3 Number.MIN_VALUE
- 자바스크립트에서 표현할 수 있는 가장 작은 양수 값
  ```
  console.log(Number.MIN_VALUE); // 5e-324
  ```
  
### 28.2.4 Number.MAX_SAFE_INTEGER
- 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수값
  ```
  console.log(Number.MAX_SAFE_INTEGER); // 9007199254740991
  ```

### 28.2.5 Number.MIN_SAFE_INTEGER
- 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수값
  ```
  console.log(Number.MIN_SAFE_INTEGER); // -9007199254740991
  ```
  
### 28.2.6 Number.POSITIVE_INFINITY
- 양의 무한대를 나타내는 숫자값 Infinity와 같다.
  ```
  console.log(Number.POSITIVE_INFINITY); // Infinity
  ```

### 28.2.7 Number.NEGATIVE_INFINITY
- 음의 무한대를 나타내는 숫자값 -Infinity와 같다.
```
  console.log(Number.NEGATIVE_INFINITY); // -Infinity
  ```

### 28.2.8 Number.NaN
- 숫자가 아님(Not-a-Number)을 나타내는 숫자값
- Number.NaN은 window.NaN과 같다.
  ```
  console.log(Number.NaN); // NaN
  ```

## 28.3 Number 메서드

### 28.3.1 Number.isFinite
- 인수로 전달된 숫자값이 정상적인 유한수. 즉 Infinity 또는 -Infinity가 아닌지 검사하여 그 결과를 불리언 값으로 반환한다.
  ```
  // 인수가 정상적인 유한수이면 true를 반환한다.
  Number.isFinite(0); // → true
  Number.isFinite(Number.MAX_VALUE); // → true
  Number.isFinite(Number.MIN_VALUE); // → true

  // 인수가 무한수이면 false를 반환한다.
  Number.isFinite(Infinity); // → false
  Number.isFinite(-Infinity); // → false
  ```
- Number.isFinite 메서드는 빌트인 전역 함수 isFinite와 차이가 있다.
  ```
  // Number.isFinite는 인수를 숫지로 암묵적 타입 변환하지 않는다.
  Number.isFinite(null); // → false
  Number.isFinite('1');  // → false

  // isFinite는 인수를 숫자로 암묵적 타입 변환한다. null은 0으로 암묵적 타입 변환된다. 
  isFinite(null); // → true
  isFinite('1');  // → true
  ```

### 28.3.2 Number.isInteger
- 인수로 전달된 숫자값이 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.
  ```
  Number.isInteger(123)  // → true
  Number.isInteger(-123) // → true

  Number.isInteger(0.5)      // → false
  Number.isInteger('123')    // → false
  Number.isInteger(false)    // → false
  Number.isInteger(Infinity) // → false
  ```

### 28.3.3 Number.isNaN
- 인수로 전달된 숫자값이 NaN인지 검사하여 그 결과를 불리언 값으로 반환한다.
  ```
  Number.isNaN(NaN); // → true
  ```
- Number.isNaN 메서드는 빌트인 전역 함수 isNaN과 차이가 있다.
  ```
  // Number.isNaN은 인수를 숫자로 암묵적 타입 변환하지 않는다.
  Number.isNaN(undefined); // → false
  
  // isNaN은 인수를 숫자로 암묵적 타입 변환한다. undefined는 NaN으로 암묵적 타입 변환된다. 
  isNaN(undefined); // → true
  ```

### 28.3.4 Number.isSafeInteger
- 인수로 전달된 숫자값이 안전한 정수인지 검사하여 그 결과를 불리언 값으로 반환한다.
- 안전한 정수값은 -(2^53 - 1) 부터 2^53 - 1 사이의 정수값이다.
  ```
  Number.isSafeInteger(0) // → true
  Number.isSafeInteger(1000000000000000) // → true

  Number.isSafeInteger(10000000000000001) // → false
  Number.isSafeInteger(0.5) // → false
  // 숫자로 암묵적 타입 변환하지 않는다.
  Number.isSafeInteger('123') // → false
  Number.isSafeInteger(true) // → false
  ```

### 28.3.5 Number.prototype.toExponential
- 숫자를 지수 표기법으로 변환하여 문자열로 반환한다.
- 인수로 소수점 이하로 표현할 자릿수를 전달할 수 있다.
  ```
  (77.1234).toExponential(); // → '7.71234e+1'
  (77.1234).toExponential(4); // → '7.7123e+1'
  (77.1234).toExponential(2); // → '7.71e+1'
  ```

### 28.3.6 Number.prototype.toFixed
- 숫자를 반올림하여 문자열로 반환한다.
- 반올림하는 소수점 이하 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달할 수 있다.
  ```
  (12345.6789).toFixed(); // → "12346"
  (12345.6789).toFixed(2); // → "12345.68"
  ```

### 28.3.7 Number.prototype.toPrecision
- 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환한다.
  ```
  (12345.6789).toPrecision(); // → "12345.6789"
  // 전체 1자릿수 유효, 나머지 반올림
  (12345.6789).toPrecision(1); // → "1e+4"
  // 전체 2자릿수 유효, 나머지 반올림
  (12345.6789).toPrecision(2); // → "12e+4"
  ```

### 28.3.8 Number.prototype.toString
- 숫자를 문자열로 변환하여 반환한다.
- 진법을 나타내는 2〜36 사이의 정수값을 인수로 전달할 수 있다. (기본값: 10진법)
  ```
  (10).toString(); // → "10"
  // 2진수 문자열 반환
  (16).toString(2); // → "10000"
  // 8진수 문자열 반환
  (16).toString(8); // → "20"
  ```