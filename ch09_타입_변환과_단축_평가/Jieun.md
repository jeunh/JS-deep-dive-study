## 9.1 타입 변환이란?
- 명시적 타입 변환 / 타입 캐스팅 : 개발자가 의도적으로 값의 타입을 변환하는 것
    ```
    var x = 10;
   
    var str = x.toString();
    console.log(typeof str, str); // string 10
   
    // x 변수의 값이 변경된 것은 아니다.
    console.log(typeof x, x); // number 10
    ```
- 암묵적 타입 변환 / 타입 강제 변환 : 개발자의 의도와는 상관없이 표현식을 평가하는 도중에 자바스크립트 엔진에 의해 암묵적으로 타입이 자동 변환되는 것
    ```
    var x = 10;
   
    var str = x + '';
    console.log(typeof str, str); // string 10
   
    // x 변수의 값이 변경된 것은 아니다.
    console.log(typeof x, x); // number 10
    ```

## 9.2 암묵적 타입 변환
    
### 9.2.1 문자열 타입으로 변환
- 자바스크립트 엔진은 문자열 연결 연산자 표현식을 평가하기 위해 문자열 연결 연산자의 피연산자 중에서 문자열 타입이 아닌 피연산자를 문자열 타입으로 암묵적 타입 변환한다.
    ```
    1 + '2' // 12
    `1 + 1 = ${1 + 1}` // "1 + 1 = 2"
    ```

### 9.2.2 숫자 타입으로 변환
- 자바스크립트 엔진은 산술 연산자 표현식을 평가하기 위해 산술 연산자의 피연산자 중에서 숫자 타입이 아닌 피연산자를 숫자 타입으로 암묵적 타입 변환한다. 
    ```
    1 - '1' // 0
    1 * '10' // 10
    1 / 'one' // NaN
    '1' > 0 // true
    ```
### 9.2.3 불리언 타입으로 변환
- 자바스크립트 엔진은 불리언 타입이 아닌 값을 true/false 로 구분하여 암묵적 타입 변환한다.
- 아래는 false로 평가되는 값이며, 아래의 값 외의 모든 값은 모두 true로 평가된다.
    - false
    - undefined
    - null
    - 0, -0
    - NaN
    - '' (빈 문자열)

## 9.3 명시적 타입 변환
### 9.3.1 문자열 타입으로 변환
   1. String 생성자 함수를 new 연산자 없이 호출하는 방법
   2. Object.prototype.toString 메서드를 사용하는 방법
   3. 문자열 연결 연산자를 이용하는 방법
        ```
        // 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
        String(1);    // "1"
        String(true); // "true"
        
        // 2. Object.prototype.toString 메서드를 사용하는 방법
        (1).toString();    // "1"
        (true).toString(); // "true"
        
        // 3. 문자열 연결 연산자를 이용하는 방법
        1 + '';    // "1"
        true + ''; // "true"
        ```
   
### 9.3.2 숫자 타입으로 변환
   1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
   2. parseInt, parseFloat 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
   3. \+ 단항 산술 연산자를 이용하는 방법
   4. \* 산술 연산자를 이용하는 방법
        ```
        // 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
        Number('0');     // 0
        Number('10.53'); // 10.53
        Number(false);   // 0
        
        // 2. parseInt, parseFloat 함수를 사용하는 방법 (문자열만 숫자 타입으로 변환 가능)
        parseInt('0');       // 0
        parseFloat('10.53'); // 10.53
        
        // 3. + 단항 산술 연산자를 이용하는 방법
        +'-1';    //-1
        +'10.53'; //10.53
        +false;   == -1
        
        // 4. * 산술 연산자를 이용하는 방법
        '0' * 1;     // 0
        '10.53' * 1; // 10.53
        true * 1;    // 1
        ```
   
### 9.3.3 불리언 타입으로 변환
   1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
   2. ! 부정 논리 연산자를 두 번 사용하는 방법
   ```
   // 1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
   Boolean('');        // false
   Boolean('false');   // true
   Boolean(0);         // false
   Boolean(null);      // false
   Boolean(undefined); // false
   Boolean({});        // true
   
   // 2. ! 부정 논리 연산자를 두 번 사용하는 방법
   !!'';      // false
   !!'false'; // true
   !!1;       // true
   !!null;    // false
   !![];      // true
   ```

## 9.4 단축 평가

### 9.4.1 논리 연산자를 사용한 단축 평가
   - 논리합(||) 또는 논리곱(&&) 연산자 표현식의 평가 결과는 불리언 값이 아닐 수도 있다.
   - 논리합(||) 또는 논리곱(&&) 연산자 표현식은 언제나 2개의 피연산자 중 어느 한쪽으로 평가된다.
   - 단축 평가: 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것 (타입 변환하지 않고 그대로 반환)

        |단축 평가 표현식|평가 결과|
        |---|---|
        |true \|\| anything|true|
        |false \|\| anyrhing|anything|
        |true && anything|anything|
        |false && anything|false|
   
        ```
        // 논리합(||) 연산자
        'Cat' || 'Dog' // "Cat"
        false || 'Dog' // "Dog"
        'Cat' || false // "Cat"
        
        // 논리곱(&&) 연산자
        'Cat' && 'Dog' // "Dog"
        false && 'Dog' // false
        'Cat' && false // false
        ```
   
        #### 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때
        ```
        var elem = null;
        var value = elem.value; // TypeError: Cannot read property 'value' of null
        ```
        ```
        var elem = null;
        var value = elem && elem.value; // null
        // 단축 평가를 사용하면 에러를 발생시키지 않는다.
        ```
   
      #### 함수 매개변수에 기본 값을 설정할 때
      함수를 호출할 때 인수를 전달하지 않으면 매개변수에는 undefined가 할당된다. 이 때 단축 평가를 사용해 매개변수의 기본값을 설정하면 undefined으로 발생할 수 있는 에러를 방지할 수 있다.
      ```
      // 단축 평가를 사용한 매개변수의 기본값 설정
      function getStringLength(str) {
         str = str || '';
         return str.length;
      }
      
      getStringLength(); // 0
      getStringLength('hi'); // 2
      
      // ES6의 매개변수의 기본값 설정
      function getStringLength(str = '') {
         return str.length;
      }
      
      getStringLength(); // 0
      getStringLength('hi'); // 2
      ```
   
   ### 9.4.2 옵셔널 체이닝 연산자
   - ES11(ECMAScript2020)에서 도입된 옵셔널 체이닝 연산자 ?.는 좌항의 피연산자가 null 또는 undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.
        ```
        var elem = null;
        var value = elem?.value;
        console.log(value); // undefined
        ```
        ```
        var elem = null;
        // 옵셔널 체이닝 연산자 ?.가 도입되기 이전에는 논리 연산자 &&를 사용
        var value = elem && elem.value;
        console.log(value); // null
        ```
   
   ### 9.4.3 null 병합 연산자
   - ES11(ECMAScript2020)에서 도입된 null 병합 연산자 ??는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
   - null 병합 연산자 ??는 변수에 기본값을 설정할 때 유용하다.
        ```
        var foo = null ?? 'default string';
        console.log(foo); // "default string"
        ```

   - null 병합 연산자 ??가 도입되기 이전에는 논리연산자 ||를 사용한 단축 평가를 통해 변수에 기본값을 설정했다.
   - 좌항의 피연산자가 false로 평가되는 값(false, undefined, null, 0, -0, NaN, '')이면 우항의 피연산자를 반환하나, 0이나 ''도 기본값으로서 유효하다면 예기치 않은 동작이 발생할 수 있다.
        ```
        var foo = '' || 'default string';
        console.log(foo); // "default string"
        ```

   - null 병합 연산자 ??는 좌항의 피연산자가 false로 평가되는 값이라도 null 또는 undefined가 아니면 좌항의 피연산자를 그대로 반환한다.
        ```
        var foo = '' ?? 'default string';
        console.log(foo); // ""
        ```