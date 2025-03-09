## 22.1 this 키워드
- this는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수다.
  ```
  // 객체 리터럴
  const circle = {
    radius: 5,
    getDiameter() {
      // this는 메서드를 호출한 객체(circle)를 가리킨다.
      return 2 * this.radius;
    }
  };

  console.log(circle.getDiameter()); // 10
  ```
  ```
  // 생성자 함수
  function Circle(radius) {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    this.radius = radius;
  }

  Circle.prototype.getDiameter = function() {
    // this는 생성자 함수가 생성할 인스턴스를 가리킨다.
    return 2 * this.radius;
  }

  const circle = new Circle(5);
  console.log(circle.getDiameter()); // 10
  ```

## 22.2 함수 호출 방식과 this 바인딩
- this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
- 함수 호출 방식
  1. 일반 함수 호출
  2. 메서드 호출
  3. 생성자 함수 호출
  4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출

### 22.2.1 일반 함수 호출
- 기본적으로 this에는 전역 객체가 바인딩된다.
  ```
  function foo() {
    console.log(this); // window
    function bar() {
      console.log(this); // window
    }
    bar();
  }
  foo();
  ```
- 어떠한 함수라도 일반 함수로 호출되면 this에 전역 객체가 바인딩된다.
  ```
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      console.log(this); // {value: 100, f00: f}
      console.log(this.value); // 100

      // 메서드 내에서 정의한 중첩 함수
      function bar() {
        console.log(this); // window
        console.log(this.value); // 1
      }
      bar();

      // 콜백 함수 내부의 this에는 전역 객체가 바인딩된다.
      setTimeout(function () {
        console.log(this); // window
        console.log(this.value); // 1
      }, 100);
    }
  }
  obj.foo();
  ```
- 메서드 내부의 중첩 함수나 콜백 함수의 this 바인딩을 메서드의 this 바인딩과 일치시키기 위한 방법
  ```
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      // this 바인딩(obj)를 변수 that에 할당
      const that = this;

      setTimeout(function () {
        console.log(that.value); // 100
      }, 100);
    }
  }
  obj.foo();
  ```
  ```
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      // 콜백 함수에 명시적으로 this를 바인딩
      setTimeout(function () {
        console.log(this.value); // 100
      }.bind(this), 100);
    }
  }
  obj.foo();
  ```
  ```
  var value = 1;

  const obj = {
    value: 100,
    foo() {
      // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다
      setTimeout(() => console.log(this.value), 100);
    }
  }
  obj.foo();
  ```

### 22.2.2 메서드 호출
- 메서드 내부의 this는 메서드를 소유한 객체가 아닌 메서드를 호출할 객체에 바인딩된다.
  ```
  const person = {
    name: 'Lee',
    getName() {
      return this.name;
    }
  };

  console.log(person.getName()); // Lee
  ```
- getName 프로퍼티가 가리키는 함수 객체, 즉 getName 메서드는 다른 객체의 프로퍼티에 할당하는 것으로 다른 객체의 메서드가 될 수도 있고 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.
  ```
  const anotherPerson = {
    name: 'Kim'
  };

  anotherPerson.getName = person.getName;
  console.log(anotherPerson.getName()); // Kim

  const getName = person.getName;
  console.log(getName()); // ''
  // window.name -> 빌트인 프로퍼티, 기본값은 ''
  ```

### 22.2.3 생성자 함수 호출
- 생성자 함수 내부의 this에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.
  ```
  function Circle(radius) {
    this.radius = radius;
    this.getDiameter = function() {
      return 2 * this.radius;
    };
  }

  const circle1 = new Circle(5);
  const circle2 = new Circle(10);

  console.log(circle1.getDiameter()); // 10
  console.log(circle2.getDiameter()); // 20
  ```

### 22.2.4 Function.prototype.apply/call/bind 메서드에 의한 간접 호출
- apply와 call 메서드는 함수를 호출하면서 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 this에 바인딩한다.
- bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
  ```
  function getThisBinding(name, age) {
    console.log(this);
    console.log(`name: ${name}, age: ${age}`);
  }

  const thisArg = { a: 1 };

  getThisBinding.apply(thisArg, ['홍길동', 30]);
  // {a: 1}
  // name: 홍길동, age: 30

  getThisBinding.call(thisArg, '홍길동', 30);
  // {a: 1}
  // name: 홍길동, age: 30

  getThisBinding.bind(thisArg, '홍길동', 30)();
  // {a: 1}
  // name: 홍길동, age: 30
  ```