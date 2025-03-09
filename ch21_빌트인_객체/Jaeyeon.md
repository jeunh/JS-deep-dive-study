# 빌트인 객체

## 자바스크립트 객체의 분류

- 표준 빌트인 객체, 호스트 객체, 사용자 정의 객체

## 표준 빌트인 객체

- Math, Reflect, JSON을 제외한 표준 빌트인 객체는 모두 인스턴스를 생성할 수 있는 생성자 함수 객체다.
- 생성자 함수 객체인 표준 빌트인 객체는 프로토타입 메서드와 정적 메서드를 모두 제공한다.
- 생성자 함수 객체가 아닌 표준 빌트인 객체는 정적 메서드만 제공한다.

  ```
  // String 생성자 함수에 의한 String 객체 생성
  const strObj = new String('Lee'); // String {"Lee"}
  console.log(typeof strObj); // object

  // Number 생성자 함수에 의한 Number 객체 생성
  const numObj = new Number(123); // Number {123}
  console.log(typeof numObj); // object

  // Boolean 생성자 함수에 의한 Boolean 객체 생성
  const boolObj = new Boolean(true); // Boolean {true}
  console.log(typeof boolObj); // object

  // Function 생성자 함수에 의한 Function 객체(함수) 생성
  const func = new Function('x', 'return x * x'); // f anonymous(x)
  console.log(typeof func); // function

  // Array 생성자 함수에 의한 Array 객체(배열) 생성
  const arr = new Array(1, 2, 3); // (3) [1, 2, 3]
  console.log(typeof arr); // object

  // RegExp 생성자 함수에 의한 RegExp 객체(정규 표현식) 생성
  const regExp = new RegExp(/ab+c/i); // /ab+c/i
  console.log(typeof regExp); // object

  // Date 생성자 함수에 의한 Date 객체 생성
  const date = new Date(); // Fri May 08 2020 10:43:25 GMT+0900 (대한민국 표준시)
  console.log(typeof date); // object

  ```

- 생성자 함수인 표준 빌트인 객체가 생성한 인스턴스의 프로토타입은 표준 빌트인 객체의 prototype 프로퍼티에 바인딩된 객체다. 예를 들어, 표준 빌트인 객체인 String을 생성자 함수로서 호출하여 생성한 String 인스턴스의 프로토타입은 String.prototype이다.

  ```
  const strObj = new String('Lee'); // String 객체 생성
  console.log(Object.getPrototypeOf(strObj) === String.prototype); // true

  ```

  1. new String('Lee') → String 생성자 함수를 사용해서 **객체(인스턴스)** 를 생성.
  2. 이렇게 만들어진 strObj의 원형(프로토타입)은 **String.prototype**이라는 객체.
  3. Object.getPrototypeOf(strObj)를 하면 strObj의 프로토타입을 확인할 수 있음.
  4. String.prototype과 비교하면 true가 나옴 → 즉, strObj는 String.prototype을 상속받음.

## 원시값과 래퍼 객체

- 다음 예제에서 원시값은 객체가 아니므로 프로퍼티나 메서드를 가질 수 없는데도 원시값인 문자열이 객체처럼 동작한다.

  ```
  const str = 'hello'

  // 원시 타입인 문자열이 프로퍼티와 메서드를 갖고 있는 객체처럼 동작한다.
  console.log(str.length); //5
  console.log(str.toUpperCase()); // HELLO
  ```

- 이는 자바스크립트 엔진이 일시적으로 원시값을 연관된 객체로 변환해주기 때문에 가능하다.
- 원시값을 객체처럼 사용하면 자바스크립트 엔진은 암묵적으로 연관된 객체를 생성하여 생성된 객체로 프로퍼티에 접근하거나 메서드를 호출하고 다시 원시값으로 되돌린다.
- 이처럼 문자열, 숫자, 불리언 값에 대해 객체처럼 접근하면 생성되는 임시 객체를 **래퍼 객체**라고 한다.
- 이 때 생성된 생성자는 [[StringData]], [[NumberData]]와 같은 내부 슬롯에 할당된다.
- 그리고 이는 String.prototype (Number.prototype)을 상속받아 해당 메서드를 사용할 수 있다.
- 래퍼 객체의 처리가 종료되면 [[StringData]], [[NumberData]] 내부 슬롯에 할당된 원시값을 되돌리고 래퍼 객체는 가비지 컬렉션의 대상이 된다.

  ```
  const num = 1.5;
  // 원시 타입인 숫자가 래퍼 객체인 Number 객체로 변환된다.
  console.log(num.toFixed()); // 2
  // 래퍼 객체로 프로퍼티에 접근하거나 메서드를 호출한 후, 다시 원시값으로 되돌린다.
  console.log(typeof num, num) // number 1.5
  ```

## 전역 객체

- 전역 객체는 코드가 실행되기 이전에 어떤 객체보다도 먼저 생성되는 특수한 객체이다.
- 브라우저 환경은 window, Node.js 에서는 global이 전역객체이다.
- var 키워드로 선언한 전역 변수, 선언하지 않은 변수에 값을 할당한 암묵적 전역, 전역 함수는 전역 객체의 프로퍼티가 된다.

  ```
  var foo = 1;
  console.log(window.foo); // 1

  bar = 2;
  console.log(window.bar); // 2
  ```

- let, const 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

### 빌트인 전역 프로퍼티

- 전역 객체의 프로퍼티를 의미한다.
- Infinity: 무한대를 나타내는 숫자값
- NaN: 숫자가 아닌 값(Not a Number)
- undefined: 값이 정의되지 않았을 때의 기본 값

  ```
  console.log(window.Infinity === Infinity); // true
  console.log(window.NaN); // NaN
  console.log(window.undefined); // undefined
  ```

### 빌트인 전역 함수

- 애플리케이션 전역에서 호출할 수 있는 빌트인 함수로서 전역 객체의 메서드다.
- eval(): 문자열을 코드로 실행 (보안상 사용 자제).
  ```
  console.log(parseFloat('10.5')); // 10.5
  console.log(isNaN('hello')); // true
  console.log(isFinite(100)); // true
  console.log(eval('2 + 2')); // 4
  // eval함수는 런타임에 전달받은 인자가 표현식이면 바로 평가하고 변수 표현식이면 undefined으로 있다가 나중에 실행됨. 그런데 열심히 읽었다가 필자가 eval함수의 사용은 금지하라고 해서 그냥 가볍게 넘어가기로 함!!
  ```
- isFinite(): 값이 유한한 숫자인지 검사.
- isNaN(): 값이 NaN인지 검사.
- parseFloat(): 문자열을 실수로 변환.
- parseInt(): 문자열을 정수로 변환, 두번째 인수로 진법을 지정할 수 있다.

  ```
  // parseInt는 두번째 인자를 진수로 해석하고 다시 10진수로 반환한다.
  console.log(parseInt('1010', 2));  // 10  (2진수 → 10진수)

  // num.toString()을 사용하면 해당 진법으로 변환이 가능하다.
  const x = 15;
  console.log(x.toString(2));   // "1111"  (10진수 → 2진수)
  ```

- encodeURI/decodeURI: URI 전체를 인코딩/디코딩. 쿼리스트링 구분자인 =, ?, & 는 인코딩하지 않는다.
  [alt text](IMG_0685.jpeg)
- encodeURIComponent / decodeURIComponent: 쿼리스트링에 쓰이는 =,?,&까지 인코딩/디코딩.

### 암묵적 전역

- 암묵적 전역 식별자는 변수 선언 없이 전역 객체의 프로퍼티로 추가되었을 뿐으로 변수가 아니며 변수 호이스팅이 발생하지 않는다.
