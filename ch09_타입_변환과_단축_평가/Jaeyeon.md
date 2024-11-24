## 타입 변환이란?

- 자바스크립트의 모든 값은 타입이 있다.
- 개발자의 의도에 따라 다른 타입으로 변환시키는 것을 `명시적 타입 변환` 혹은 `타입 캐스팅`이라 하며 개발자의 의도와 상관없이 암묵적으로 타입이 변환되는 것을 `암묵적 타입 변환` 또는 `타입 강제 변환`이라 한다.
- 타입 변환은 기존 원시 값을 변경시키지 않는다(원시 값은 변경 불가능한 값이기 때문) 고로 기존 변수에 타입 변환된 값을 재할당 하지 않는다.

## 암묵적 타입 변환

### 문자열 타입으로 변환

- `+` 연산자는 피연산자 중 하나 이상이 문자열이면 문자열 연결 연산자로 동작한다.

  ```
  // 숫자 타입
  0 + '' // "0"
  NaN + '' // "NaN"

  // 불리언 타입
  true + '' // "true"

  // null
  null + '' // "null"

  //심벌은 타입 에러가 남
  (Symbol()) + '' // TypeError

  // 객체 타입
  ({}) + '' // "[Object Object]"
  [] + '' // ""
  [10,20] + '' // "10,20"
  ```

### 숫자 타입으로 변환

- 산술 연산자나 비교연산자는 숫자 타입이 아닌 피연산자를 숫자 연산자로 암묵적 타입변환한다.
- `+` 단항 연산자는 피연산자가 숫자 타입의 값이 아니면 숫자 타입의 값으로 암묵적 타입 변환을 수행한다.

  ```
  // 문자열 타입
  +'' // 0
  +'0' // 0
  +'string' // NaN

  // 불리언 타입
  +true // 1
  +false //0

  //null 타입
  +null //0

  //undefined 타입
  +undefined // NaN -> null과 달리 NaN이다.

  // 심벌 타입
  +Symbol() // TypeError

  // 객체 타입
  +{} // NaN
  +[] //0
  +[10,20] //NaN -> 빈 배열이 아닌 배열은 NaN이다.
  +(function(){}) // NaN
  ```

### 불리언 타입으로 변환

- 자바스크립트 엔진은 불리언 타입이 아닌 값을 Truthy 혹은 Falsy 값으로 구분한다. 즉 제어문의 조건식과 같이 불리언 값으로 평가되어야 할 문맥에서 Truthy값은 true로, Falsy 값은 false로 암묵적 타입 변환된다.

## 명시적 타입 변환

- 개발자의 의도에 따라 명시적으로 타입을 변경한다.

### 문자열 타입으로 변환

- 문자열 타입이 아닌 값을 문자열 타입으로 변환하는 방법은 다음과 같다.

  1. String 생성자 함수를 new 연산자 없이 호출하는 방법
  2. Object.prototype.toString 메서드를 사용하는 방법
  3. 문자열 연결 연산자를 이용하는 방법

  ```
  // 1. String 생성자 함수를 new 연산자 없이 호출하는 방법
  String(1) // "1"
  String(false) // "false"

  // 2. Object.prototype.toString 메서드를 사용하는 방법
  (1).toString(); // "1"
  (NaN).toString(); // "NaN"
  (Infinity).toString(); // "Infinity"

  // 3. 문자열 연결 연산자를 이용하는 방법
  -> 이건 암묵적 타입변환에서도 나온 예시 같은데, 결국 암묵적/명시적 타입 변환의 기준은 개발자의 의도가 들어갔냐 아닌가가 관건인건가 라고 생각했다.
  1+'' // "1"
  NaN+'' // "NaN"
  true+'' // "true"
  ```

### 숫자 타입으로 변환

- 숫자 타입이 아닌 값을 숫자 타입으로 변환하는 방법은 다음과 같다.

  1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
  2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 숫자 타입으로 변환 가능)
  3. `+` 단항 산술 연산자를 이용하는 방법
  4. `*` 산술 연산자를 이용하는 방법

  ```
  // 1. Number 생성자 함수를 new 연산자 없이 호출하는 방법
  // 문자열 타입 -> 숫자 타입
  Number('0') // 0
  Number('10.53') // 10.53

  // 불리언 타입 -> 숫자 타입
  Number(true) // 1
  Number(false) // 0

  // 2. parseInt, parseFloat 함수를 사용하는 방법(문자열만 변환 가능)
  // 문자열 타입 -> 숫자 타입
  parseInt('0') // 0
  parseInt('-1') // -1
  parseFloat('10.53') // 10.53

  // 3. + 단항 산술 연산자를 이용하는 방법
  // 문자열 타입 -> 숫자 타입
  // +'0' //0
  // '-1' // -1

  // 불리언 타입 -> 숫자 타입
  +true // 1
  +false // 0

  // 4. * 산술 연산자를 이용하는 방법
  // 문자열 타입 -> 숫자 타입
  '0' * 1 // 0
  '10.53' * 1 // 10.53

  // 불리언 타입 -> 숫자 타입
  true * 1 // 1
  false * 1 // 0
  ```

### 불리언 타입으로 변환

- 불리언 타입이 아닌 값을 불리언 타입으로 변환하는 방법은 다음과 같다.

  1.  Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
  2.  ! 부정 논리 연산자를 두 번 사용하는 방법

  ```
  1. Boolean 생성자 함수를 new 연산자 없이 호출하는 방법
  Boolean('x') // true
  Boolean('') // false
  Boolean('false') // true
  Boolean(0) // false
  Boolean(1) // true
  Boolean(null) // false
  Boolean({}) // true
  Boolean([]) // true

  2. ! 부정 논리 연산자를 두 번 사용하는 방법
  !!'x' // true
  !!'' // false
  !!0 // false
  !!1 // true
  // NaN // false
  !!Infinity // true
  !!null // false
  !!undefined // false
  !!{} //true
  !![] // true
  ```

## 단축평가

### 논리 연산자를 사용한 단축 평가

- `논리곱(&&)` 연산자는 두 개의 피연산자가 모두 true로 평가될 때 true를 반환한다. 논리곱 연산자는 좌항에서 우항으로 평가된다.
- 이레 예시에서는 논리 연산의 결과를 결정하는 두 번째 피연산자인 문자열 'Dog'를 그대로 반환한다.

  ```
  'Cat' && 'Dog' // 'Dog'
  ```

- `논리합(||)` 연산자는 두 개의 피연산자 중 하나만 true로 평가되어도 true를 반환한다. 논리합 연산자도 좌항에서 우항으로 평가가 진행된다.
- 아래 예시에서는 첫번째 피연산자 'Cat'이 truethy 이므로 첫 번째 피연산자인 문자열 'Cat'을 그대로 반환한다.

  ```
  'Cat' && 'Dog' // 'Cat'

  ```

  ```

  ```

- 위와 같이 표현식을 평가하는 도중에 평가 결과가 확정된 경우 나머지 평가 과정을 생략하는 것을 `단축 평가`라 한다.

  1. true || anything

  - 첫 번째 값이 true라서 두 번째 값은 평가하지 않고 결과는 true

  2. false || anything

  - 첫 번째 값이 false라 두 번째 값을 평가하고 그 값(anything)이 결과

  3. true && anything

  - 첫 번째 값이 true라 두 번째 값을 평가하고, 그 값(anything)이 결과

  4. false && anything

  - 첫 번째 값이 false라 두 번째 값은 평가하지 않고 결과는 false

- 단축평가를 사용하면 if문을 대체할 수 있다.

  ```
  var done = true;
  var message = '';

  message = done && '완료';
  console.log(message); // 완료

  done = false;

  message = done || '미완료';
  console.log(messages); // 미완료
  ```

- 단축평가는 다음과 같은 상황에서 유용하게 사용된다.

  - 객체를 가리키기를 기대하는 변수가 null 또는 undefined가 아닌지 확인하고 프로퍼티를 참조할 때

  ```
  var elem = null;
  var value = elem.value; // TypeError
  var value = elem && elem.value; // null
  ```

  - 함수 매개변수에 기본 값을 설정할 때

  ```
  function getStringLength(str){
      str = str || '';
      return str.length;
  }

  getStringLength(); // 0
  getStringLength('hi') // 2
  ```

### 옵셔널 체이닝 연산자

- 옵셔널 체이닝 연산자 `?.`는 좌항의 피연산자가 null, undefined인 경우 undefined를 반환하고, 그렇지 않으면 우항의 프로퍼티 참조를 이어간다.

  ```
  var elem = null;
  var value = elem?.value;
  console.log(value); // undefined
  ```

- 논리형 연산자 `&&` 는 좌항 피연산자가 `false, undefined, null, 0, -0, NaN, ''` 등의 Falsy 값이면 좌항 피연산자를 그대로 반환한다. 옵셔널 체이닝 연산자는 null과 undefined가 아니면 우항의 프로퍼티 참조를 이어나가서 이런 경우 유리하게 사용할 수 있다.

  ```
  var str = '';

  var length = str && str.length;
  console.log(length); // '' (문자열의 길이를 참조하지 못한다)

  var length = str?.length;
  console.log(length); // 0 (null이나 undefined가 아니라면 문자열 길이를 참조한다)
  ```

### null 병합 연산자

- null 병합 연산자 `??`는 좌항의 피연산자가 null 또는 undefined인 경우 우항의 피연산자를 반환하고, 그렇지 않으면 좌항의 피연산자를 반환한다.
- 변수에 기본값을 설정할 때 유용하다.
  ```
  var foo = null ?? 'default string';
  console.log(foo); // "default string"
  ```
- 논리 연산자 || 를 사용할 경우 좌항이 Falsy 값인 0이나 ''가 기본값으로 유효하다면 예기치 않은 동작이 발생할 수 있는데, `??` 연산자를 사용하면 좌항의 피연산자가 null이나 undefined가 아니면 좌항의 피연산자를 그대로 반환하여 유용하게 사용할 수 있다.

  ```
  var Foo = '' || 'default string';
  console.log(foo) // "default string'

  var foo = '' ?? 'default string'
  console.log(foo) // ''
  ```

- ?? 와 옵셔널 체이닝이 헷갈려서 위 예시를 옵셔널 체이닝으로 바꿔보았다.

  ```
  const obj1 = { value: '' };
  const obj2 = null;

  // 옵셔널 체이닝 사용 예시
  const result1 = obj1?.value || 'default string';
  console.log(result1); // "default string" (obj1.value가 ''이므로 Falsy)

  const result2 = obj1?.value ?? 'default string';
  console.log(result2); // "" (obj1.value가 Falsy이지만 null/undefined가 아니므로 그대로 반환)

  const result3 = obj2?.value ?? 'default string';
  console.log(result3); // "default string" (obj2가 null이므로 우항 반환)
  ```
