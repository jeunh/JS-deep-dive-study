## 19.10 instanceof 연산자
```
객체 instanceof 생성자함수
```
- 우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true, 그렇지 않으면 false로 평가된다.
  ```
  function Person(name) {
    this.name = name;
  }
  const me = new Person('Lee');

  console.log(me instanceof Person); // true
  console.log(me instanceof Object); // true
  ```
- instanceof 연산자는 프로토타입의 constructor 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아니라 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.
  ```
  function Person(name) {
    this.name = name;
  }
  const me = new Person('Lee');

  const parent = {};
  Object.setPropertyOf(me, parent); // 프로토타입의 교체

  // Person 생성자 함수와 parent 객체는 연결되어 있지 않다.
  console.log(Person.prototype === parent); // false
  console.log(parent.constructor === Person); // false

  //  Person.prototype이 me 객체의 프로토타입 체인 상에 존재하지 않음
  console.log(me instanceof Person); // false
  console.log(me instanceof Object); // true

  // parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩
  Person.prototype = parent;

  console.log(me instanceof Person); // true
  console.log(me instanceof Object); // true
  ```

## 19.11 직접 상속
### 19.11.1 Object.create에 의한 직접 상속
- Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다.
  ```
  /**
  * @param {Object} prototype - 생성할 객체의 프로토타입으로 지정할 객체
  * @params {Object} [propertiesObject] - 생성할 객체의 프로퍼티를 갖는 객체
  * @return {Object} 지정된 프로토타입 및 프로퍼티를 갖는 새로운 객체
  */
  Object.create(prototype[, propertiesObject])
  ```
  ```
  // 프로토타입이 null인 객체 생성
  // obj -> null
  let obj = Object.create(null);

  // obj = {}; 와 동일
  // obj -> Object.prototype -> null
  obj = Object.create(Object.prototype);

  // obj = { x: 1 }; 와 동일
  // obj -> Object.prototype -> null
  obj = Object.create(Object.prototype, {
    x: { value: 1, writeable: true, enumerable: true, configurable: true }
  });

  // 임의의 객체를 직접 상속
  // obj -> myProto -> Object.prototype -> null
  const myProto = { x: 10 };
  obj = Object.create(myProto);
  console.log(obj.x); // 10

  // obj = new Person('Lee') 와 동일
  // obj -> Person.prototype -> Object.prototype -> null
  function Person(name) {
    this.name = name;
  }
  obj = Object.create(Person.prototype);
  obj.name = 'Lee';
  console.log(obj.name); // Lee
  ```
- Object.create 메서드의 장점
  - new 연산자 없이도 객체를 생성할 수 있다.
  - 프로토타입을 지정하면서 객체를 생성할 수 있다.
  - 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.

### 19.11.2 객체 리터럴 내부에서 `__proto__`에 의한 직접 상속
- ES6에서는 객체 리터럴 내부에서 `__proto__` 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.
  ```
  const myProto = { x: 10 };

  // 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
  // obj -> myProto -> Object.prototype -> null
  const obj = {
    y: 20,
    __proto__: myProto
  };

  /* 위 코드는 아래와 동일하다.
  const obj = Object.create(myProto, {
    y: { value: 20, writable: true, enumerable: true, configurable: true }
  });
  ★/
  ```

## 19.12 정적 프로퍼티/메서드
- 정적 프로퍼티/메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드를 말한다.
  ```
  function Person(name) {
    this.name = name;
  }

  // 정적 프로퍼티
  Person.staticProp = 'static prop';

  // 정적 메서드
  Person.staticMethod = function () [
    console.log('staticMethod');
  ];

  Person.staticMethod(); // staticMethod

  const me = new Person('Lee');
  me.staticMethod(); // TypeError
  // 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
  ```

## 19.13 프로퍼티 존재 확인
### 19.13.1 in 연산자
- in 연산자는 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.
  ```
  key in object
  ```
  ```
  const person = {
    name: 'Lee',
    address: 'Seoul'
  };

  console.log('name' in person);    // true
  console.log('address' in person); // true
  console.log('age' in person);     // false

  // in 연산자는 person 객체가 속한 프로토타입 체인 상에 존재하는 모든 프로토타입에서 toString 프로퍼티를 검색한다.
  // toString은 Object.prototype의 메서드다.
  console.log('toString' in person); // true
  ```

### 19.13.2 Object.prototype.hasOwnProperty 메서드
- Object.property.hasOwnPerperty 메서드를 사용하여 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.
- 전달받은 프로퍼티 키가 객체 고유의 프로퍼티 키인 경우에만 true를 반환하고, 상속받은 프로토타입의 프로퍼티 키인 경우 false를 반환한다.
  ```
  console.log(person.hasOwnProperty('name')); // true
  console.log(person.hasOwnProperty('age'));  // false

  console.log(person.hasOwnProperty('toString')); // false
  ```

## 19.14 프로퍼티 열거
### 19.14.1 for ... in 문
```
for (변수선언문 in 객체) {...}
```
```
const person = {
  name: 'Lee',
  address: 'Seoul'
};

for (const key in person) {
  console.log(key + ': ' + person[key]);
}
// name: Lee
// address: Seoul
```
- for ... in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트 [[Enumerable]] 값이 true인 프로퍼티를 순회하며 열거한다.
  ```
  const person = {
    name: 'Lee',
    address: 'Seoul',
    __proto__: { age: 20 }
  };

  for (const key in person) {
    console.log(key + ': ' + person[key]);
  }
  // name: Lee
  // address: Seoul
  // age: 20

  // 상속받은 프로퍼티는 제외하고 객체 자신의 프로퍼티만 열거하고 싶다면
  for (const key in person) {
    if (!person.hasOwnProperty(key)) continue;
    console.log(key + ': ' + person[key]);
  }
  // name: Lee
  // address: Seoul
  ```
### 19.14.2 Object.keys/values/entries 메서드
- 상속받은 프로퍼티를 제외하고 객체 자신의 고유 프로퍼티만 열거하기 위해서는 for ... in 문을 사용하는 것보다 Object.keys/values/entries 메서드를 사용하는 것을 권장한다.
  ```
  const person = {
    name: 'Lee',
    address: 'Seoul',
    __proto__: { age: 20 }
  };

  console.log(Object.keys(person)); // ["name", "address"]
  console.log(Object.values(person)); // ["Lee", "Seoul"]
  console.log(Object.entries(person)); // [["name", "Lee"], ["address", "Seout"]]
  Object.entries(person).forEach(([key, value]) => console.log(key, value));
  // name Lee
  // address Seoul
  ```