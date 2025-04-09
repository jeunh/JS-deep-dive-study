## 26.1 함수의 구분
- ES6 이전의 자바스크립트 함수는 사용 목적에 따라 명확히 구분되지 않고 일반적인 함수, 생성자 함수, 메서드로 호출할 수 있다.
- ES6 이전의 함수는 동일함 함수라도 다양한 형대로 호출할 수 있다.
  ```
  var foo = function() {
    return 1;
  }

  // 일반적인 함수로 호출
  foo(); 

  // 생성자 함수로 호출
  new foo(); 

  // 메서드로 호출
  var obj = { foo: foo };
  obj.foo();
  ```
- ES6 이전의 모든 함수는 callable 이면서 constructor다.
  - callable: 호출할 수 있는 함수 객체
  - constructor: 인스턴스를 생성할 수 있는 함수 객체
  - non-constructor: 인스턴스를 생성할 수 없는 객체
- ES6 이전의 모든 함수는 사용 목적에 따라 명확한 구분이 없으므로 호출 방식에 특별한 제약이 없고 생성자 함수로 호출되지 않아도 불필요한 프로토타입 객체를 생성한다.
- 이러한 문제를 해결하기 위해 ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분했다.

  |ES6 함수의 구분|constructor|prototype|super|arguments|
  |---|---|---|---|---|
  |일반함수(Normal)|O|O|X|O|
  |메서드(Method)|X|X|O|O|
  |화살표 함수(Arrow)|X|X|X|X|

## 26.2 메서드
- ES6 이전 일반적으로 메서드는 객체에 바인딩된 함수를 일컫는 의미로 사용되었지만, ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 의미한다.
  ```
  const obj = {
    x: 1,
    // foo는 메서드다
    foo() { return this.x; },
    // bar에 바인딩된 함수는 메서드가 아닌 일반 함수다
    bar: function() { return this.x; }
  };
  ```
- ES6 메서드는 인스턴스를 생성할 수 없는 non-constructor이므로 생성자 함수로서 호출할 수 없다.
- 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
  ```
  new obj.foo(); // TypeError: obj.foo is not a constructor
  new obj.bar(); // bar {}
  ```
- ES6 메서드는 자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 갖고 있으므로 super 키워드를 사용할 수 있다.
  ```
  const base = {
    name: 'Lee',
    sayHi() {
      return `Hi! ${this.name}`;
    }
  };

  const derived = {
    __proto__: base,
    // sayHi는 ES6 메서드다. ES6 메서드는 [[HomeObject]]를 갖는다.
    // sayHi의 [[HomeObject]]는 derived.prototype을 가리키고
    // super는 sayHi의 [[HomeObject]]의 프로토타입인 base.prototype을 가리킨다.
    sayHi() {
      return `${super.sayHi()}. how are you doing?`;
    }
  };

  console.log(derived.sayHi()); // Hi! Lee. how are you doing?
  ```
- ES6 메서드가 아닌 함수는 내부 슬롯 [[HomeObject]]를 갖지 않기 때문에 super 키워드를 사용할 수 없다.
- ES6 메서드는 본연의 기능(super)을 추가하고 의미적으로 맞지 않는 기능(constructor)은 제거했다. 따라서 메서드를 정의할 때 프로퍼티 값으로 익명 함수 표현식을 할당하는 ES6 이전의 방식은 사용하지 않는 것이 좋다.

## 26.3 화살표 함수
- 화살표 함수는 function 키워드 대신 화살표(=>)를 사용하여 기존의 함수 정의 방식보다 간략하게 함수를 정의할 수 있다.
- 화살표 함수는 콜백 함수 내부에서 this가 전역 객체를 가리키는 문제를 해결하기 위한 대안 으로 유용하다.

### 26.3.1 화살표 함수 정의
#### 함수 정의
- 화살표 함수는 함수 선언문으로 정의할 수 없고 함수 표현식으로 정의해야 한다. 
- 호출 방식은 기존 함수와 동일하다.
  ```
  const multiply = (x, y) => x * y;
  multiply(2, 3);
  ```

#### 매개변수 선언
- 매개변수가 여러 개인 경우 소괄호 () 안에 매개변수를 선언한다.
  ```
  const arrow = (x, y) => { ... };
  ```
- 매개변수가 한 개인 경우 소괄호 ()를 생략할 수 있다.
  ```
  const arrow = x => { ... };
  ```
- 매개변수가 없는 경우 소괄호 ()를 생략할 수 없다.
  ```
  const arrow = () => { ... };
  ```

#### 함수 몸체 정의
- 함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 아를 생략할 수 있다. 
- 이때 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵적으로 반환된다.
  ```
  const power = x => x ** 2;
  power(2); // 4

  // 위 표현은 다음과 같다
  const power = x => { return x ** 2; };
  ```
- 함수 몸체가 하나의 문으로 구성된다 해도 함수 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략할 수 없다.
  ```
  const arrow = () => const x = 1; // SyntaxError: Unexpected token 'const'
  ```
- 객체 리터럴을 반환하는 경우 객체 리터럴을 소괄호()로 감싸 주어야 한다.
  ```
  const create = (id, content) => ({ id, content });
  create(l, 'JavaScript'); // {id: 1, content: "JavaScript"}

  // 위 표현은 다음과 동일하다.
  const create = (id, content) => { return { id, content }; };
  ```
- 화살표 함수도 즉시 실행 함수로 사용할 수 있다.
  ```
  const person = (name => ({ 
    sayHi() { return `Hi? My name is ${name}.`; }
  }))('Lee');

  console.log(person.sayHi()); // Hi? My name is Lee.
  ```
- 화살표 함수도 일급 객체이므로 Array.prototype.map, Array.prototype.filter, Array.prototype.reduce 같은 고차 함수에 인수로 전달할 수 있다. 이 경우 일반적인 함수 표현식보다 표현이 간결하고 가독성이 좋다.
  ```
  // ES5
  [1, 2, 3].map(function (v) { 
    return v * 2;
  });

  // ES6
  [1, 2, 3].map(v => v * 2); 
  ```

### 26.3.2 화살표 함수와 일반 함수의 차이
1. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor 다. 인스턴스를 생성할 수 없으므로 prototype 프로퍼티가 없고 프로토타입도 생성하지 않는다.
2. 일반 함수는 중복된 매개변수 이름을 선언해도 에러가 발생하지 않지만, 화살표 함수는 중복된 매개변수 이름을 선언할 수 없다.
3. 화살표 함수는 함수 자체의 this, arguments, super, new.target 바인딩을 갖지 않는다.

### 26.3.3 this
- 화살표 함수가 일반 함수와 구별되는 가장 큰 특징은 바로 this다.
- 함수를 정의할 때 this에 바인딩할 객체가 정적으로 결정되는 것이 아니고, 함수를 호출할 때 함수가 어떻게 호출되었는지에 따라 this에 바인딩할 객체가 동적으로 결정된다.
  ```
  class Prefixer {
    constructor(prefix) { 
      this.prefix = prefix;
    }
    add(arr) {
      // ①
      return arr.map(function (item) {
        return this.prefix + item; // ②
        // — TypeError: Cannot read property 'prefix' of undefined 
      });
    }
  }
  const prefixer = new Prefixer('-webkit-');
  console.log(prefixer.add(['transition', 'user-select']));
  ```
- 위 예제를 실행했을 때 기대하는 결과는 ['-webkit-transition', '—webkit-user-select']다. 하지만 TypeError가 발생한다.
- 프로토타입 메서드 내부인 ①에서 this는 메서드를 호출한 객체(위 예제의 경우 prefixer 객체)를 가리킨다. 그런데 Array.prototype.map의 인수로 전달한 콜백 함수의 내부인 ②에서 this는 undefined를 가리킨다. 이는 Array.prototype.map 메서드가 콜백 함수를 일반 함수로서 호출하기 때문이다.
- 일반 함수로서 호출되는 모든 함수 내부의 this는 전역 객체를 가리키지만, 클래스 내부의 모든 코드에는 strict mode가 암묵적으로 적용된다. strict mode에서 일반 함수로서 호출된 모든 함수 내부의 this에는 전역 객체가 아니라 undefined가 바인딩된다.
- ES6에서는 화살표 함수를 사용하여 "콜백 함수 내부의 this 문제"를 해결할 수 있다.
  ```
  class Prefixer {
    constructor(prefix) { 
      this.prefix = prefix;
    }
    add(arr) {
      return arr.map(item => this.prefix + item);
    }
  }
  const prefixer = new Prefixer('-webkit-');
  console.log(prefixer.add(['transition', 'user-select']));
  ```
- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 화살표 함수 내부에서 this를 참조하면 상위 스코프의 this를 그대로 참조한다. 이를 lexical this라 한다.
- 이는 마치 렉시컬 스코프와 같이 화살표 함수의 this가 함수가 정의된 위치에 의해 결정된다는 것을 의미한다.
- 만약 화살표 함수와 화살표 함수가 중첩되어 있다면 상위 화살표 함수에도 this 바인딩이 없으므로 스코프체인 상에서 가장 가까운 상위 함수 중에서 화살표 함수가 아닌 함수의 this를 참조한다.
- 화살표 함수는 함수 자체의 this 바인딩을 갖지 않기 때문에 Function.prototype.call, Function.prototype.apply, Function.prototype.bind 메서드를 사용해도 화살표 함수 내부의 this를 교체할 수 없다.
  ```
  window.x = 1;

  const normal = function () { return this.x; };
  const arrow = () => this.x;

  console.log(normal.call({ x: 10 })); // 10
  console.log(arrow.call({ x: 10 })); // 1
  ```
- 메서드를 화살표 함수로 정의하는 것은 피해야 한다.
  ```
  const person = {
    name: 'Lee',
    sayHi: () => console.log(`Hi ${this.name}`)
  };

  /*  sayHi 프로퍼티에 할당한 화살표 함수 내부의 this는 메서드를 호출한 객체인 person을 가리키지 않고 상위 스코프인 전역의 this가 가리키는 전역 객체를 가리킨다. */
  person.sayHi(); // Hi
  ```
- 메서드를 정의할 때는 ES6 메서드 축약 표현으로 정의한 ES6 메서드를 사용하는 것이 좋다.
  ```
  const person = {
    name: 'Lee',
    sayHi() {
      console.log(`Hi ${this.name}`);
    }
  };

  person.sayHi(); // Hi Lee
  ```
- 프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 동일한 문제가 발생한다. 프로퍼티를 동적 추가할 때는 ES6 메서드 정의를 사용할 수 없으므로 일반 함수를 할당한다.
  ```
  function Person(name) { 
    this.name = name;
  }

  // Bad
  // Person.prototype.sayHi = () => console.log(`Hi ${this.name}`);
  // Good
  Person.prototype.sayHi = function () { console.log(`Hi ${this.name}`); };

  const person = new Person('Lee');
  person.sayHi(); 
  ```

### 26.3.4 super
- 화살표 함수는 함수 자체의 super 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 super를 참조하면 this와 마찬가지로 상위 스코프의 super를 참조한다.
  ```
  class Base {
    constructor(name) {
      this.name = name;
    }
    sayHi() {
      return `Hi! ${this.name}`;
    }
  }

  class Derived extends Base {
    sayHi = () => {
      console.log(this);  // this는 Derived 인스턴스를 가리킴
      return `${super.sayHi()} how are you doing?`;
    };
  }

  const derived = new Derived('Lee');
  console.log(derived.sayHi()); // Hi! Lee how are you doing?
  ```

### 26.3.5 arguments
- 화살표 함수는 함수 자체의 arguments 바인딩을 갖지 않는다. 따라서 화살표 함수 내부에서 arguments를 참조하면 this와 마찬가지로 상위 스코프의 arguments를 참조한다.
  ```
  (function () {
    // 화살표 함수 foo의 arguments는 상위 스코프인 즉시 실행 함수의 arguments를 가리킨다.
    const foo = () => console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
    foo(3, 4);
  }(1, 2));

  // 화살표 함수 foo의 arguments는 상위 스코프인 전역의 arguments를 가리킨다.
  // 하지만 전역에는 arguments 객체가 존재하지 않는다. arguments 객체는 함수 내부에서만 유효하다.
  const foo = () => console.log(arguments);
  foo(1, 2); // ReferenceError: arguments is not defined
  ```

## 26.4 Rest 파라미터
