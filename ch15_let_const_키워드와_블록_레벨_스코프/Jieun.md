## 15.1 var 키워드로 선언한 변수의 문제점
### 15.1.1 변수 중복 선언 허용
- var 키워드로 선언한 변수는 중복 선언이 가능하다.
  ```
  var x = 1;
  var y = 1;

  var x = 100; // 중복 선언
  var y; // 초기화문이 없는 변수 선언문은 무시된다.

  console.log(x); // 100
  console.log(y); // 1
  ```
### 15.1.2 함수 레벨 스코프
- var 키워드로 선언한 변수는 오로지 함수의 코드 블록만을 지역 스코프로 인정한다.
- 따라서 함수 외부에서 var 키워드로 선언한 변수는 코드 블록 내에서 선언해도 모두 전역 변수가 된다.
  ```
  var x = 1;

  if(true) {
    var x = 10;
  }

  console.log(x); // 10
  ```
  ```
  var i = 10;

  for(var i=0; i<5; i++) {
    console.log(i); // 0 1 2 3 4
  }

  console.log(i); // 5
  ```

### 15.1.3 변수 호이스팅
- 변수 호이스팅에 의해 var 키워드로 선언한 변수는 변수 선언문 이전에 참조할 수 있다.
- 변수 선언문 이전에 변수를 참조하는 것은 가독성을 떨어뜨리고 오류를 발생시킬 수 있다.
  ```
  console.log(foo); // undefined
  foo = 123;
  console.log(foo); // 123
  var foo;
  ```

## let 키워드
### 15.2.1 변수 중복 선언 금지
- let 키워드는 이름이 같은 변수를 중복 선언하면 문법 에러(SyntaxError)가 발생한다.
  ```
  var foo = 123;
  var foo = 456; // 중복 선언 가능 (에러 X)

  let bar = 123;
  let bar = 456; // SyntaxError
  ```

### 15.2.2 블록 레벨 스코프
- let 키워드로 선언한 변수는 모든 코드 블록(함수, if문, for문, while문, try/catch문 등)을 지역 스코프로 인정하는 블록 레벨 스코프를 따른다.
  ```
  let foo = 1; // 전역 변수

  {
    let foo = 2; // 지역 변수
    let bar = 3; // 지역 변수
  }

  console.log(foo); // 1
  console.log(bar); // ReferenceError: bar is not defined
  ```

### 15.2.3 변수 호이스팅
- 자바스크립트는 모든 선언(var, let, const, function, class 등)을 호이스팅 한다.
- 단, ES6에서 도입된 let, const, class를 사용한 선언문은 호이스팅이 발생하지 않는 것처럼 동작한다.
  ```
  console.log(foo); // undefined

  var foo;
  console.log(foo); // undefined

  foo = 1;
  console.log(foo); // 1
  ```
  ```
  // 초기화 이전의 일시적 사각지대에서는 변수를 참조할 수 없다.
  console.log(foo); // ReferenceError: foo is not defined

  let foo; // 변수 선언문에서 초기화 단계 실행
  console.log(foo); // undefined

  foo = 1; // 할당문에서 할당 단계 실행
  console.log(foo); // 1
  ```

### 15.2.4 전역 객체와 let
- var 키워드로 선언한 전역 변수, 전역 함수, 선언하지 않는 변수에 값을 할당한 암묵적 전역은 전역 객체 window의 프로퍼티가 된다. 전역 객체의 프로퍼티를 참조할 때 window를 생략할 수 있다.
  ```
  var x = 1; // 전역 변수
  y = 2; // 암묵적 전역
  function foo() {} // 전역 함수

  console.log(window.x); // 1
  console.log(x); // 1

  console.log(window.y); // 2
  console.log(y); // 2

  console.log(window.foo); // f foo() {}
  console.log(foo); // f foo() {}
  ```
- let 키워드로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.
  ```
  let x = 1;

  console.log(window.x); // undefined
  console.log(x); // 1
  ```

## 15.3 const 키워드
### 15.3.1 선언과 초기화
- const 키워드로 선언한 변수는 반드시 선언과 동시에 초기화해야 한다.
  ```
  const foo = 1;
  const bar; // SyntaxError
  ```
- const 키워드로 선언한 변수는 let 키워드로 선언한 변수와 마찬가지로 블록 레벨 스코프를 가지며, 변수 호이스팅이 발생하지 않는 것처럼 동작한다.

### 15.3.2 재할당 금지
- const로 키워드로 선언한 변수는 재할당이 금지된다.

### 15.3.3 상수
- const 키워드로 선언한 변수에 원시 값을 할당한 경우 변수 값을 변경할 수 없다.
- 상수는 재할당이 금지된 변수를 말한다.
- 상수는 상태 유지, 가독성, 유지보수의 편의를 위해 적극적으로 사용해야 한다.
- 일반적으로 상수 이름인 대문자로 선언해 상수임을 명확히 나타내고, 여러 단어로 이루어진 경우 언더스코어(_)로 구분해서 스네이크 케이스로 표현한다.
  ```
  const TAX_RATE = 0.1;
  
  let preTaxPrice = 100;

  let afterTaxPrice = preTaxPrice + (preTaxPrice * TAX_RATE);

  console.log(afterTaxPrice);
  ```


### 15.3.4 const 키워드와 객체
- const 키워드로 선언된 변수에 원시 값을 할당한 경우 값을 변경할 수 없지만, 객체를 할당할 경우 값을 변경할 수 있다.
- const 키워드는 재할당을 금지할 뿐 "불변"을 의미하지는 않는다.
  ```
  const person = {
    name: 'Lee'
  };

  person.name = 'Kim';

  console.log(person); // {name: "Kim"}
  ```

## 15.4 var VS let VS const
- ES6를 사용한다면 var 키워드는 사용하지 않는다.
- 재할당이 필요한 경우에 한정해 let 키워드를 사용한다. 이떄 변수의 스코프는 최대한 좁게 만든다.
- 변경이 발생하지 않고 읽기 전용으로 사용하는(재할당이 필요 없는 상수) 원시 값과 객체에는 const 키워드를 사용한다. const 키워드는 재할당을 금지하므로 var, let 키워드보다 안전하다.