# 클래스의 인스턴스 생성 과정
1. 인스턴스 생성과 this 바인딩
- `new` 연산자와 함께 클래스를 호출하면 암묵적으로 인스턴스가 빈 객체 형태로 생성된다.
- 그리고 그 인스턴스는 `this`에 바인딩된다.

2. 인스턴스 초기화
- `constructor` 내부 코드가 실행되어 `this`에 바인딩되어 있는 인스턴스를 초기화한다.

3. 인스턴스 반환
- 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.

```javascript
class Person {
    constructor(name, age) {
        // 2. 인스턴스 초기화
        this.name = name;
        this.age = age;
    }
}

// 1. 인스턴스 생성과 this 바인딩
const person = new Person('Jaeyeon', 30);

// 3. 인스턴스 반환
console.log(person); // Person { name: 'Jaeyeon', age: 30 }
```

# 프로퍼티
## 인스턴스 프로퍼티
- 인스턴스 프로퍼티는 constructor 내부에서 정의한다. 
- constructor 내부에서 this에 추가한 프로퍼티는 언제나 클래스가 생성한 인스턴스의 프로퍼티가 된다.
- private한 프로퍼티를 정의할 수 있는 사양이 현재 제안 중에 있긴 하지만 ES6의 클래스는 언제나 public하다.

```javascript
class Car {
    constructor(brand) {
        // 인스턴스 프로퍼티 정의
        this.brand = brand;
    }
}

const myCar = new Car('모닝');
// 퍼블릭한 인스턴스들..
console.log(myCar); // Car { brand: '모닝' }
```

## 접근자 프로퍼티

- 접근자 프로퍼티는 값을 읽거나 저장할 때 사용하는 접근자 함수 프로퍼티다.
- 자체적으로 값을 저장하지 않고, 다른 데이터 프로퍼티의 값을 읽거나 수정할 때 사용된다.
- getter는 `get` 키워드를 사용하며, setter는 `set` 키워드를 사용한다.

    ```javascript
    class Person {
        constructor(firstName, lastName) {
            // 데이터 프로퍼티 정의
            this.firstName = firstName; // 이름
            this.lastName = lastName;   // 성
        }

        // getter: fullName을 읽을 때 호출됨
        get fullName() {
            // firstName과 lastName을 조합하여 반환
            return `${this.firstName} ${this.lastName}`;
        }

        // setter: fullName에 값을 할당할 때 호출됨
        set fullName(name) {
            // 전달받은 문자열을 분리하여 firstName과 lastName에 저장
            const [firstName, lastName] = name.split(' ');
            this.firstName = firstName;
            this.lastName = lastName;
        }
    }

    const person = new Person('John', 'Doe');

    // getter 호출: fullName을 읽음
    console.log(person.fullName); // John Doe

    // setter 호출: fullName에 새로운 값 할당
    person.fullName = 'Jane Smith';

    // getter 호출: 변경된 fullName을 읽음
    console.log(person.fullName); // Jane Smith

    // 데이터 프로퍼티 확인
    console.log(person); // Person { firstName: 'Jane', lastName: 'Smith' }
    ```

- 클래스의 메서드는 기본적으로 프로토타입 메서드가 된다. 따라서 클래스의 접근자 프로퍼티 또한 인스턴스 프로퍼티가 아닌 프로토타입의 프로퍼티가 된다.
    ```javascript
    // 프로토타입 확인
    console.log(Object.getPrototypeOf(person).hasOwnProperty('fullName')); // true

    // 인스턴스 프로퍼티 확인
    console.log(person.hasOwnProperty('fullName')); // false
    ```

## 클래스 필드 정의 제안
- 일단 자바의 클래스 필드는 아래와 같다. 마치 클래스 내부에서 변수처럼 사용된다.

```java
public class Person {
 // 1. 클래스 필드 정의
 // 클래스 필드는 클래스 몸체에 this 없이 선언해야 한다.
 private String firstName = "";
 private String lastName = "";

 // 생성자
 Person(String firstName, String lastName){
    // 3. this는 언제나 클래스가 생성할 인스턴스를 가리킨다.
    this.firstName = firstName;
    this.lastName = lastName;
 }

 public String getFullName(){
    // 2. 클래스 필드 참조
    // this 없이도 클래스 필드를 참조할 수 있다.
    return firstName + " " + lastName;
 }
}
```
- 자바스크립트의 클래스에서 인스턴스 프로퍼티를 선언하고 초기화하려면 constructor 내부에서 this에 프로퍼티를 추가해야 한다.
- 자바의 클래스에서는 1과 같이 클래스 필드를 마치 변수처럼 클래스 몸체에 this 없이 선언한다.
- 자바스크립트의 클래스에서 인스턴스 프로퍼티를 참조하려면 반드시 this를 사용하여 참조해야 한다.
- 자바의 클래스에서는 2와 같이 this 없이도 클래스를 참조할 수 있다.
- 위 예제의 3과 같이 this는 주로 클래스 필드가 생성자 또는 메서드의 매개 변수 이름과 동일 할 때( this.firstName = firstName;) 클래스 필드임을 명확히 하기 위해 사용된다.
- 자바스크립트의 클래스 몸체에는 메서드만 선언할 수 있다. 따라서 클래스 몸체에 자바와 유사하게 클래스 필드를 선언하면 문법 에러가 발생한다.

### 자바 코드 예시를 자바스크립트로 변환한 예제
```javascript

    class Person {
        // 1. 클래스 필드 정의
        // 클래스 필드는 클래스 몸체에 선언해야 한다.
        firstName = '';
        lastName = '';

        // 생성자
        constructor(firstName, lastName) {
            // 3. this는 언제나 클래스가 생성할 인스턴스를 가리킨다.
            this.firstName = firstName;
            this.lastName = lastName;
        }

        // 메서드 정의
        getFullName() {
            // 2. 클래스 필드 참조
            // this를 사용하여 클래스 필드를 참조한다.
            return `${this.firstName} ${this.lastName}`;
        }
    }

    // 클래스 사용 예시
    const person = new Person('John', 'Doe');
    console.log(person.getFullName()); // John Doe
```

- 자바스크립트에서는 현재 인스턴스 프로퍼티를 마치 클래스 기반 객체지향 언어의 클래스 필드처럼 정의할 수 있는 새로운 표준 사양인 Class field declarations가 TC39 프로세스에 제안되어있다.(TC39는 자바스크립트의 표준 사양인 ECMAScript를 개발하고 유지 관리하는 기술 위원회)

## private 필드 정의 제안
- 클래스 필드 정의 제안을 사용하더라도 클래스 필드는 기본적으로 public하기 때문에 외부에 그대로 노출된다.
- 그러나 현재 TC39 프로세스에는 private 필드를 정의할 수 있는 새로운 표준 사양이 제안되어 있다.
- 이 사양에 따르면, private 필드는 `#` 기호를 접두사로 사용하여 정의하며, 클래스 외부에서는 접근할 수 없다. 이를 통해 캡슐화를 보다 효과적으로 구현할 수 있다.

    ```javascript
    class Person {
        // private 필드 정의
        #firstName = '';
        #lastName = '';

        constructor(firstName, lastName) {
            this.#firstName = firstName;
            this.#lastName = lastName;
        }

        getFullName() {
            return `${this.#firstName} ${this.#lastName}`;
        }
    }

    const person = new Person('John', 'Doe');
    console.log(person.getFullName()); // John Doe

    // private 필드에 직접 접근 시도 (오류 발생)
    // console.log(person.#firstName); // SyntaxError: Private field '#firstName' must be declared in an enclosing class
    ```

- private 필드는 클래스 내부에서만 접근 가능하며, 외부에서 접근하려고 하면 에러가 발생한다. 이를 통해 데이터 은닉성을 강화할 수 있다.

## static 필드 정의 제안

- 클래스에는 static 키워드를 사용하여 정적 메서드를 정의할 수는 있었지만 static 키워드를 사용하여 정적 필드를 정의할 수는 없었다.
- 하지만 지금은 표준 사양으로 제안되어 구성되어있다.
    ```javascript
    class MyMath {
        // static public 필드 정의
        static PI = 22/7;
        // static private 필드 정의
        static #num = 10;
        // static 메서드
        static increment(){
            return ++MyMath.#num;
        }
    }

    console.log(MyMath.PI); //3.142857142857143
    console.log(MyMath.increment()); // 11
    ```