# 함수의 구분
ES6 이전의 함수 특징은 아래와 같음

- 함수는 다음 세 가지 방식으로 호출 가능:
    - 일반 함수로 호출
    - 생성자 함수로 호출 (new 사용)
    - 메서드로 호출 (객체에 바인딩)

    ```js
    var foo = function () {
    return 1;
    };

    // 일반적인 함수로서 호출
    foo(); // → 1

    // 생성자 함수로서 호출
    new foo(); // → foo {}

    // 메서드로서 호출
    var obj = { foo: foo };
    obj.foo(); // → 1

    ```
- 위 예시 -하나의 함수가 이 모든 호출을 지원함.
- ES6 이전에는 모든 함수가 constructor로 간주돼서, new 키워드 없이 호출해도 내부적으로 prototype 객체를 생성. 함수가 불필요한 프로토타입 객체를 생성해서 성능에 악영향. 
- constructor로 간주된다는 말은 그 함수를 new 키워드로 호출할 수 있다는 말이고 new Foo() 처럼 호출해서  내부적으로 prototype 객체를 생성하고 인스턴스 객체를 만들 수 있다는 뜻. 
- ES6의 해결책: 함수의 목적에 따라 함수 구분함

    | 함수 유형          | constructor | prototype | super | arguments |
    |--------------------|-------------|-----------|-------|-----------|
    | 일반 함수 (Normal) | O           | O         | X     | O         |
    | 메서드 (Method)    | X           | X         | O     | O         |
    | 화살표 함수 (Arrow)| X           | X         | X     | X         |

- 위 테이블 컬럼 설명
    - constructor:	new로 호출할 수 있는가?
    - prototype:	prototype 속성이 있는가 (생성자처럼 동작할 준비가 됐는가)
    - super:	super 키워드를 사용할 수 있는가
    - arguments:	arguments 객체를 쓸 수 있는가



# 메서드

- ES6 메서드 특징
     ```js
    const obj = {
    foo() { return this.x; }, // ES6 메서드
    bar: function() { return this.x; } // 일반 함수
    };

    // foo만 ES6 메서드, bar는 일반 함수\
    ```

    - constructor가 아님 (→ new 못 씀, prototype 없음)
    ```js
    new obj.foo(); // TypeError
    obj.foo.hasOwnProperty('prototype'); // false
    obj.bar.hasOwnProperty('prototype'); // true <- 일반함수라 있음

    ```

    - super 사용 가능  
        - 내부 슬롯 [[HomeObject]]이 있는 덕분에 ES6 메서드는 자신이 어떤 객체에 바인딩됐는지 기억함
        - 상위 객체의 메서드를 super로 참조할 수 있음

        ```js
        const base = {
            name: 'Lee',
            sayHi() {
                return `Hi! ${this.name}`;
            }
        };

        const derived = {
            __proto__: base,
            sayHi() {
                return `${super.sayHi()}. how are you doing?`;
            }
        };

        console.log(derived.sayHi());
        // → "Hi! Lee. how are you doing?"

        ```
    - 정의 방법이 축약형 (foo() {})

- ES6 이전 방식인 `bar: function() {}` 같은 일반 함수 특징
    - constructor임 (→ new 가능)
    - super 사용 불가
    - 의미적으로 메서드라 부르긴 했지만, 실제 메서드가 아님

   
- ES6 이후엔 메서드는 항상 축약형으로 작성하자.


# 화살표 함수
## 화살표 함수 정의
### 함수 정의
화살표 함수는 함수 선언문으로는 정의할 수 없고, 함수 표현식으로만 정의 가능하다. 호출 방식은 기존 함수와 같음

```js
const multiply = (x, y) => x * y;
multiply(2, 3); // → 6
```

### 매개변수 선언
- 매개변수가 여러 개일 경우 () 안에 나열        
`const arrow = (x, y) => { ... };`
- 매개변수가 하나일 경우 () 생략 가능
`const arrow = x => { ... };`
- 매개변수가 없을 경우 () 생략 불가
`const arrow = () => { ... };`

### 함수 몸체 정의
- 함수 몸체가 한 줄이고 표현식인 경우 중괄호 생략 가능하며, 암묵적으로 반환됨
```js
const power = x => x ** 2;
// 아래와 동일
const power = x => { return x ** 2; };
```
- 중괄호를 사용하는 경우에는 return을 명시해야 한다
```js
const arrow = () => {
    const x = 1;
    return x;
};
```

- 중괄호 없이 문을 쓰면 문법 오류가 발생한다.

```js
const arrow = () => const x = 1; // SyntaxError
```

- 객체 리터럴을 반환하려면 전체를 소괄호 ()로 감싸야 한다. 그렇지 않으면 중괄호가 함수 몸체로 해석된다.

```js
const create = (id, content) => ({ id, content });
// → { id: 1, content: 'JavaScript' }

const create = (id, content) => { id, content };
// → undefined
```


- 화살표 함수는 IIFE(즉시 실행 함수 표현식)로도 사용 가능하다.

```js
const person = (name => ({
sayHi() {
    return `Hi? My name is ${name}.`;
}
}))('Lee');

person.sayHi(); // → "Hi? My name is Lee."
```


## 화살표 함수와 일반 함수의 차이
01. 화살표 함수는 인스턴스를 생성할 수 없는 non-constructor이다
화살표 함수는 new 키워드로 호출할 수 없다.
따라서 prototype 프로퍼티도 없고, 프로토타입 객체도 생성되지 않는다.
```js
const Foo = () => {};
new Foo(); // TypeError: Foo is not a constructor
```
```js
const Foo = () => {};
Foo.hasOwnProperty('prototype'); // false
```

02. 중복된 매개변수 이름을 선언할 수 없다
일반 함수는 중복된 매개변수 이름을 허용한다. 

```js
function normal(a, a) {
  return a + a;
}
console.log(normal(1, 2)); // 4
```
- 단, strict mode에서는 에러 발생.
```js
// (일반 함수 + strict mode)
'use strict';
function normal(a, a) {
  return a + a; // SyntaxError
}
```
- 화살표 함수는 strict mode 여부와 관계없이 항상 중복 매개변수 선언이 문법 에러임
```js
const arrow = (a, a) => a + a; // SyntaxError
```

03. 화살표 함수는 this, arguments, super, new.target을 바인딩하지 않는다
- 화살표 함수는 일반 함수와 다르게, 자신만의 this, arguments, super, new.target을 갖지 않음
- 대신 외부(상위 스코프)의 값을 그대로 참조한다 (→ 렉시컬 바인딩)
- 관련 예시는 이후 챕터에서 다룰 예정

## this

화살표 함수와 일반 함수의 가장 큰 차이는 this의 동작 방식에 있음.

### 일반 함수의 this 바인딩

일반 함수에서 this는 함수의 **호출 방식**에 따라 **동적으로 바인딩**됨. 함수를 정의할 때 this가 고정되지 않고, 호출 시점에 어떻게 호출되었는지가 기준이 됨.

콜백 함수로 일반 함수를 사용하면, this가 외부 스코프의 this와 달라지는 문제가 발생함. 이는 특히 고차 함수(Array.prototype.map 등)에서 자주 나타나는 패턴.

#### 예제 26-28

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map(function (item) {
      return this.prefix + item;
    });
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
```

- map의 콜백 함수는 일반 함수로 호출됨
- 클래스 내부는 암묵적으로 strict mode이므로 콜백 내부의 this는 undefined
- 결과적으로 this.prefix에서 에러 발생

### this 문제 해결 방법 (ES6 이전 방식)

#### 예제 26-29: 외부 this를 변수에 저장

```js
add(arr) {
  const that = this;
  return arr.map(function (item) {
    return that.prefix + ' ' + item;
  });
}
```

- 콜백 내부에서 직접 this를 쓰는 대신, 외부에서 const that = this로 참조값을 저장
- 클로저로 인해 내부 함수가 that을 참조 가능

#### 예제 26-30: map의 두 번째 인수로 this 전달

```js
add(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }, this);
}
```

- map은 콜백 외에 두 번째 인수로 this를 전달받을 수 있음
- 콜백 내부에서 해당 this를 그대로 사용 가능

#### 예제 26-31: bind로 this 고정

```js
add(arr) {
  return arr.map(function (item) {
    return this.prefix + ' ' + item;
  }.bind(this));
}
```

- Function.prototype.bind로 this를 강제로 고정

### 화살표 함수로 해결 (ES6 방식)

#### 예제 26-32

```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map(item => this.prefix + item);
  }
}
```

- 화살표 함수는 자체적인 this 바인딩이 없음
- this는 외부 스코프의 값을 그대로 참조함
- 이를 **lexical this**라고 부름: 함수가 정의된 위치의 this를 기억

결과:

```js
['-webkit-transition', '-webkit-user-select']
```

화살표 함수는 콜백 함수 내부의 this 문제를 해결하기 위해 설계됨. this는 호출 방식과 무관하게 외부 스코프에서 가져오기 때문에 예측 가능성이 높음.

---

## 추가 질문 및 보완 설명

### 예제 26-29에서 const that = this;가 동작하는 이유

- that은 단순한 변수이며, 함수가 실행되는 시점의 this 값을 저장한 것
- add 함수가 실행될 때 this는 prefixer 인스턴스
- 콜백 함수 내부에서는 클로저를 통해 that 변수에 접근 가능
- 따라서 that.prefix는 prefixer.prefix를 정상적으로 참조함

정리:

| 요소          | 설명                                               |
|---------------|----------------------------------------------------|
| this          | 콜백 내부에서 직접 쓰면 호출 방식 따라 결정됨     |
| that = this   | 외부에서 this 값을 복사하여 고정하는 방식          |
| 클로저        | 내부 함수가 외부 변수(that)를 자유롭게 참조 가능   |


### map의 콜백 내부에서 this가 undefined인 이유

```js
arr.map(function (item) {
  return this.prefix + item;
});
```

- 이 콜백은 일반 함수이며, map 내부에서 별도의 객체 없이 호출됨
- 클래스 내부는 strict mode가 기본 적용됨
- strict mode에서 일반 함수는 this가 undefined로 바인딩됨
- 결국 this.prefix는 undefined.prefix가 되어 TypeError 발생

많은 개발자가 this가 외부 prefixer 인스턴스를 가리킬 것으로 착각하지만, 실제로는 add 함수와 콜백 함수는 호출 주체가 다르기 때문에 this는 완전히 다르게 동작함

### that은 왜 constructor를 참조할 수 있는가

- that은 this 값을 복사한 변수
- const that = this; 실행 시점에 this는 prefixer 인스턴스를 가리킴
- 콜백 함수가 실행될 때, 클로저를 통해 that을 참조
- 콜백 내부에서 this는 undefined지만, that은 prefixer를 안정적으로 참조함

비유:
- this: 호출마다 새로 결정되는 동적인 참조
- that: 정적인 참조. 한 번 저장되면 값이 변하지 않음

정리:

- 화살표 함수는 lexical this를 활용하여 구조를 단순화함
- 일반 함수는 호출 방식에 따라 this가 바뀌므로 주의가 필요함
- this 바인딩이 중요한 상황에서는 화살표 함수 사용 또는 명시적 바인딩을 고려해야 함


## super

화살표 함수는 함수 자체의 super 바인딩을 갖지 않음. 
따라서 화살표 함수 내부에서 super를 참조하면 상위 스코프에서 super를 탐색.

### 예제 26-47

```js
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  // 화살표 함수의 super는 constructor의 super를 가리킨다.
  sayHi = () => `${super.sayHi()} how are you doing?`;
}

const derived = new Derived('Lee');
console.log(derived.sayHi()); // Hi! Lee how are you doing?
```

sayHi는 화살표 함수이므로 함수 자체의 super 바인딩이 없음. 따라서 상위 스코프인 constructor의 super 바인딩을 참조함.

## arguments

화살표 함수는 함수 자체의 arguments 객체를 갖지 않음. 
화살표 함수 내부에서 arguments를 참조하면 상위 스코프에서 탐색하게 됨.

### 예제 26-48

```js
(function () {
  const foo = () => console.log(arguments);
  foo(1, 2); // [Arguments] { '0': 1, '1': 2 }
})(1, 2);

const foo = () => console.log(arguments);
foo(1, 2); // ReferenceError: arguments is not defined
```

첫 번째 함수는 즉시 실행 함수의 arguments를 참조하며, 두 번째는 전역에서 실행되기 때문에 arguments가 존재하지 않아 에러 발생.

# Rest 파라미터

## 기본 문법

Rest 파라미터는 ... 기호를 사용하여 정의하며, 전달된 나머지 인수들을 배열로 수집함.

### 예제 26-49

```js
function foo(...rest) {
  console.log(rest); // [1, 2, 3, 4, 5]
}

foo(1, 2, 3, 4, 5);
```

### 예제 26-50

```js
function foo(param, ...rest) {
  console.log(param); // 1
  console.log(rest);  // [2, 3, 4, 5]
}

function bar(param1, param2, ...rest) {
  console.log(param1); // 1
  console.log(param2); // 2
  console.log(rest);   // [3, 4, 5]
}

foo(1, 2, 3, 4, 5);
bar(1, 2, 3, 4, 5);
```

Rest 파라미터는 반드시 마지막에 위치해야 하며, 하나만 선언 가능.

### 예제 26-51

```js
function foo(...rest, param1, param2) {}
// SyntaxError: Rest parameter must be last formal parameter
```

### 예제 26-52

```js
function foo(...rest1, ...rest2) {}
// SyntaxError: Rest parameter must be last formal parameter
```

함수 객체의 length는 rest 파라미터 앞에 정의된 매개변수의 개수만 반영.

### 예제 26-53

```js
function foo(...rest) {}
console.log(foo.length); // 0

function bar(x, ...rest) {}
console.log(bar.length); // 1

function baz(x, y, ...rest) {}
console.log(baz.length); // 2
```

## Rest 파라미터와 arguments 객체

ES5에서는 가변 인자 함수 구현 시 arguments 객체를 사용.
arguments는 유사 배열 객체이기 때문에 배열 메서드를 바로 사용할 수 없음.

### 예제 26-54

```js
function sum() {
  console.log(arguments);
}

sum(1, 2); // { '0': 1, '1': 2 }
```

### 예제 26-55

```js
function sum() {
  var array = Array.prototype.slice.call(arguments);

  return array.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
```

ES6에서는 rest 파라미터를 사용해 배열 형태로 직접 받을 수 있음.

### 예제 26-56

```js
function sum(...args) {
  return args.reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3, 4, 5)); // 15
```

일반 함수와 ES6 메서드는 arguments와 rest parameter 모두 사용 가능. 화살표 함수는 arguments 객체를 갖지 않으므로 rest parameter만 사용 가능.

# 매개변수 기본값

자바스크립트에서는 인수가 부족해도 에러가 발생하지 않음. 전달되지 않은 인수는 undefined로 평가됨.

### 예제 26-57

```js
function sum(x, y) {
  return x + y;
}

console.log(sum(1)); // NaN
```

기본값을 수동으로 설정하는 전통적인 방식.

### 예제 26-58

```js
function sum(x, y) {
  x = x || 0;
  y = y || 0;
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1));    // 1
```

ES6에서는 함수 선언 시 기본값을 설정할 수 있음.

### 예제 26-59

```js
function sum(x = 0, y = 0) {
  return x + y;
}

console.log(sum(1, 2)); // 3
console.log(sum(1));    // 1
```

기본값은 인수가 전달되지 않거나 undefined일 때만 적용됨.

### 예제 26-60

```js
function logName(name = 'Lee') {
  console.log(name);
}

logName();           // Lee
logName(undefined);  // Lee
logName(null);       // null
```

rest 파라미터에는 기본값을 설정할 수 없음.

### 예제 26-61

```js
function foo(...rest = []) {}
// SyntaxError: Rest parameter may not have a default initializer
```

기본값을 지정해도 length나 arguments에는 영향 없음.

### 예제 26-62

```js
function sum(x, y = 0) {
  console.log(arguments);
}

console.log(sum.length); // 1

sum(1);       // Arguments { '0': 1 }
sum(1, 2);    // Arguments { '0': 1, '1': 2 }
```

함수 객체의 length는 기본값이 지정되지 않은 매개변수 개수만 반영.

