## 24.1 렉시컬 스코프
- 렉시컬 스코프 (정적 스코프)
  - 자바스크립트 엔진은 함수를 어디서 호출했는지가 아니라 함수를 어디에 정의했는지에 따라 상위 스코프를 결정한다.
  - 렉시컬 환경의 "외부 렉시컬 환경에 대한 참조(Outer Lexical Environment Reference)"에 저장할 참조값, 즉 상위 스코프에 대한 참조는 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정된다.
  ```
  const x = 1;

  function foo() {
    const x = 10;
    bar();
  }

  function bar() {
    console.log(x);
  }

  foo(); // 1
  bar(); // 1
  ```

## 24.2 함수 객체의 내부 슬롯 [[Environment]]
- 함수가 정의된 환경(위치)과 호출되는 환경(위치)은 다를 수 있다.
- 함수 정의가 위치하는 스코프(상위 스코프)를 기억하기 위해 함수는 자신의 내부 슬롯 [[Environment]]에 자신이 정의된 환경(상위 스코프의 참조)를 저장한다.
- 자신의 내부 슬롯 [[Environment]]에 저장된 상위 스코프의 참조는 현재 실행 중인 실행 컨텍스트의 렉시컬 환경을 가리킨다.
- 함수 렉시컬 환경의 구성 요소인 외부 렉시컬 환경에 대한 참조 (Outer Environment Reference)에는 함수 객체의 내부 슬롯 [[Environment]]에 저장된 렉시컬 환경의 참조가 할당된다.

## 24.3 클로저와 렉시컬 환경
```
const x = 1;

function outer() {
  const x = 10;
  const inner = function() { console.log(x); };
  return inner;
}

const innerFunc = outer();
innerFunc(); // 10
```
- outer 함수의 실행이 종료되면 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되기 때문에 outer 함수의 지역변수 x에 접근할 수 있는 방법은 없어보이지만, 위 코드의 실행 결과는 outer 함수의 지역 변수 x의 값인 10이다.
- 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 **클로저**라고 부른다.
- 중첩 함수 inner가 평가될 때 자신의 [[Environment]] 내부 슬롯에 현재 실행 중인 실행 컨텍스트의 렉시컬 환경, 즉 outer 함수의 렉시컬 환경을 상위 스코프로서 저장한다.
- outer 함수의 실행이 종료되면 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 제거되지만 outer 함수의 렉시컬 환경까지 소멸하는 것은 아니다.
- 자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로는 모든 함수는 클로저다. 하지만 일반적으로 모든 함수를 클로저 라고 하지는 않는다.
  ```
  function foo() {
    const x = 1;
    const y = 2;

    function bar() {
      const z = 3;
      debugger;
      console.log(z);
    }
    return bar;
  }
  const bar = foo();
  bar();
  ```
  - bar 함수는 상위 스코프의 식별자를 참조하지 않으므로 클로저라고 하지 않는다.
  ```
  function foo() {
    const x = 1;

    function bar() {
        debugger;
        console.log(x);
    }
    bar();
  }
  foo();
  ```
  - bar 함수는 상위 스코프의 식별자를 참조하여 클로저였지만 곧바로 소멸하기 때문에 클로저라고 하지 않는다.
  ```
  function foo() {
    const x = 1;
    const y = 2;
    
    function bar() {
      debugger;
      console.log(x);
    }
    return bar;
  }
  const bar = foo();
  bar();
  ```
  - 중첩 함수 bar는 외부 함수보다 더 오래 유지되며 상위 스코프의 식별자를 참조하기 때문에 클로저다.
  - 클로저에 의해 참조되는 상위 스코프의 변수(foo 함수의 x 변수)를 자유 변수라고 부른다.

## 24.4 클로저의 활용
- 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.
  ```
  const increase = (function () {
    let num = 0;

    // 클로저
    return function() {
      return ++num;
    };
  }());

  console.log(increase()); // 1
  console.log(increase()); // 2
  ```
  ```
  const counter = (function() {
    let num = 0;

    // 클로저인 메서드를 갖는 객체를 반환한다.
    return {
      increase() {
        return ++num;
      },
      decrease() {
        return num > 0 ? --num : 0;
      }
    };
  }());

  console.log(counter.increase()); // 1
  console.log(counter.increase()); // 2

  console.log(counter.decrease()); // 1
  console.Log(counter.decrease()); // 0
  ```
  ```
  // 생성자 함수로 표현
  const Counter = (function() {
    let num = 0;

    function Counter() {}

    Counter.prototype.increase = function() {
      return ++num;
    };
    Counter.prototype.decrease = function() {
      return num > 0 ? --num : 0;
    };

    return Counter;
  }());

  const counter = new Counter();

  console.log(counter.increase()); // 1
  console.log(counter.increase()); // 2

  console.log(counter.decrease()); // 1
  console.Log(counter.decrease()); // 0
  ```
- 함수형 프로그래밍에서 클로저 활용
  ```
  // 함수를 인수로 전달받고 함수를 반환하는 고차 함수
  function makeCounter(predicate) {
    let counter = 0;

    // 클로저를 반환
    return function () {
      counter = predicate(counter);
      return counter;
    };
  }

  // 보조 함수
  function increase(n) {
    return ++n;
  }
  function decrease(n) {
    return --n;
  }

  const increaser = makeCounter(increase);
  console.log(increaser()); // 1
  console.log(increaser()); // 2

  // increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동되지 않는다.
  const decreaser = makeCounter(decrease);
  console.log(decreaser()); // -1
  console.log(decreaser()); // -2
  ```
  - 독립된 카운터가 아니라 연동하여 증감이 가능한 카운터를 만들려면 렉시컬 환경을 공유하는 클로저를 만들어야 한다. 
  ```
  const counter = (function() {
    let counter = 0;

    return function(predicate) {
      counter = predicate(counter);
      return counter;
    }
  }());

  // 보조 함수
  function increase(n) {
    return ++n;
  }
  function decrease(n) {
    return --n;
  }

  console.log(counter(increase)); // 1
  console.log(counter(increase)); // 2

  console.log(counter(decrease)); // 1
  console.log(counter(decrease)); // 0
  ```

## 24.5 캡슐화와 정보 은닉
- 캡슐화는 객체의 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.
- 캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라 한다.
  ```
  const Person = (function () {
    let _age = 0; // private

    // 생성자 함수
    function Person(name, age) {
      this.name = name; // public
      _age = age;
    }

    // 프로토타입 메서드
    Person.prototype.sayHi = function() {
      console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
    };

    return Person;
  }());

  const me = new Person('Lee', 20);
  me.sayHi(); // Hi! My name is Lee. I am 20.
  console.log(me.name); // Lee
  console.log(me._age); // undefined

  const you = new Person('Kim', 30);
  you.sayHi(); // Hi! My name is Kim. I am 30.
  console.log(you.name); // Kim
  console.log(you._age); // undefined

  // _age 변수 값이 변경된다.
  me.sayHi(); // Hi! My name is Lee. I am 30.
  ```
  - Person 생성자 함수와 sayHi 메서드는 이미 종료되어 소멸한 즉시 실행 함수의 지역 변수 _age를 참조할 수 있는 클로저다.
  - Person.prototype.sayHi 메서드는 단 한 번 생성되는 클로저이기 때문에 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 _age 변수의 상태가 유지되지 않는다.

## 24.6 자주 발생하는 실수
```
var funcs = [];

for(var i=0; i<3; i++) {
  funcs[i] = function() { return i };
}

for(var j=0; j<funcs.length; j++) {
  console.log(funcs[j]());
}
```
- 위 코드를 실행하면 0, 1, 2를 반환할 것으로 예상했지만 결과는 그렇지 않다.
- for 문의 변수 선언문에서 var 키워드로 선언한 i 변수는 블록 레벨 스코프가 아닌 함수 레벨 스코프를 갖기 때문에 전역 변수다. 따라서 3이 세 번 출력된다.
```
// 클로저를 사용해 바르게 동작하는 코드
var funcs = [];

for(var i=0; i<3; i++) {
  funcs[i] = (function (id) {
    return function() {
      return id;
    };
  }(i));
}

for(var j=0; j<funcs.length; j++) {
  console.log(funcs[j]()); // 0 1 2
}
```
```
// ES6 let 키워드 사용
const funcs = [];

for(let i=0; i<3; i++) {
  funcs[i] = function() { return i; };
}

for(let i=0l i<funcs.length; i++) {
  console.log(funcs[i]()); // 0 1 2
}
```