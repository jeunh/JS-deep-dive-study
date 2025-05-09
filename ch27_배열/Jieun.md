## 27.1 배열이란?
- 배열은 여러 개의 값을 순차적으로 나열한 자료구조다.
- 배열이 가지고 있는 값을 요소라고 부르며, 원시값, 객체, 함수, 배열 등 자바스크립트에서 값으로 인정하는 모든 것은 배열의 요소가 될 수 있다.
- 자바스크립트에 배열이라는 타입은 존재하지 않는다. 배열은 객체 타입이다.
  ```
  const arr = ['apple', ' banana', ' orange'];
  typeof arr // object
  ```

## 27.2 자바스크립트 배열은 배열이 아니다
- 자료구조에서 말하는 일반적인 의미의 배열
  - 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료구조이다. 이러한 배열을 **밀집 배열**이라 한다.
  - 장점: 인덱스를 통해 단 한 번의 연산으로 임의의 요소에 접근이 가능하여 매우 효율적이고, 고속으로 동작한다.
  - 단점: 정렬되지 않은 배열에서 특정한 요소를 검색하는 경우 특정 요소를 발견할 때까지 처음부터 차례대로 검색해야 한다. 배열에 요소를 삽입하거나 삭제하는 경우 배열의 요소를 연속적으로 유지하기 위해 요소를 이동시켜야한다.
- 자바스크립트 배열
  - 배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수 있다. 배열의 요소가 연속적으로 이어져 있지 않는 배열을 **희소 배열**이라 한다.
  - 자바스크립트 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체다.
  - 장점: 특정 요소를 검색하거나 요소를 삽입 또는 삭제하는 경우에는 일반적인 배열보다 빠르다.
  - 단점: 인덱스로 배열 요소에 접근하는 경우에는 일반적인 배열보다 느리다.

## 27.3 length 프로퍼티와 희소 배열
- length 프로퍼티는 배열의 길이를 나타내는 0 이상의 정수를 값으로 갖는다.
- length 프로퍼티의 값은 배열에 요소를 추가하거나 삭제하면 자동 갱신된다.
-  length 프로퍼티 값은 요소의 개수, 즉 배열의 길이를 바탕으로 결정되지만 임의의 숫자 값을 명시적으로 할당할 수도 있다.
    ```
    const arr = [1, 2, 3, 4, 5];

    // 현재 length 프로퍼티 값인 5보다 작은 숫자 값 3을 length 프로퍼티에 할당 
    arr.length = 3;

    // 배열의 길이가 5에서 3으로 줄어든다. 
    console.log(arr); // [1, 2, 3]
    ```
    ```
    const arr = [1];

    // 현재 length 프로퍼티 값인 1보다 큰 숫자 값 3을 length 프로퍼티에 할당 
    arr.length = 3;

    // length 프로퍼티 값은 변경되지만 실제로 배열의 길이가 늘어나지는 않는다. 
    console.log(arr.length); // 3
    console.log(arr); // [1, empty × 2]
    ```
- 위 예제의 출력 결과에서 empty × 2는 실제로 추가된 배열의 요소가 아니다. 즉, arr[1]과 arr[2]에는 값이 존재하지 않는다.
- 이처럼 배열의 요소가 연속적으로 위치하지 않고 일부가 비어 있는 배열을 희소 배열이라 한다.
  ```
  // 희소 배열 
  const sparse = [, 2, , 4];

  // 희소 배열의 length 프로퍼티 값은 요소의 개수와 일치하지 않는다.
  console.log(sparse.length); // 4
  console.log(sparse); // [empty, 2, empty, 4]

  // 배열 sparse에는 인덱스가 0, 2인 요소가 존재하지 않는다.
  console.log(Object.getOwnPropertyDescriptors(sparse));
  /*
  {
    '1': { value: 2, writable: true, enumerable: true, configurable: true },
    '3': { value: 4, writable: true, enumerable: true, configurable: true }, 
    length: { value: 4, writable: true, enumerable: false, configurable: false }
  }
  */
  ```
- 일반적인 배열의 length는 배열 요소와 언제나 일치하지만, 희소 배열은 length와 배열 요소의 개수가 일치하지 않는다.
- 자바스크립트는 문법적으로 희소 배열을 허용하지만 희소 배열은 사용하지 않는 것이 좋다.

## 27.4 배열 생성
### 27.4.1 배열 리터럴
- 배열에는 다양한 생성 방식이 있는데, 가장 일반적이고 간편한 배열 생성 방식은 배열 리터럴을 사용하는 것이다.
- 배열 리터럴은 0개 이상의 요소를 쉼표로 구분하여 대괄호([])로 묶는다. 객체 리터럴과 달리 프로퍼티 키가 없고 값만 존재한다.
  ```
  const arr = [1, 2, 3];
  ```
- 배열에 리터럴 요소를 생략하면 희소 배열이 생성된다.
  ```
  const arr = [1, , 3];    // 희소배열
  console.log(arr.length); // 3
  console.log(arr);        // [1, empty, 3]
  console.log(arr[1]);     // undefined
  ```

### 27.4.2 Array 생성자 함수
- Array 생성자 함수를 통해 배열을 생성할 수 있다. Array 생성자 함수는 전달된 인수의 개수에 따라 다르게 동작하므로 주의가 필요하다.
  - 전달된 인수가 1개이고 숫자인 경우 length 프로퍼티 값이 인수인 배열을 생성한다.
    ```
    const arr = new Array(10);

    console.log(arr); // [empty × 10]
    console.log(arr.length); // 10
    
    // 희소 배열
    // length는 10이지만 실제 배열의 요소는 존재하지 않는다.
    ```
  - 전달된 인수가 없는 경우 빈 배열을 생성한다. 배열 리터럴 []과 같다.
    ```
    new Array(); // → []
    ```
  - 전달된 인수가 2개 이상이거나 숫자가 아닌 경우 인수를 요소로 갖는 배열을 생성한다.
    ```
    new Array(1, 2, 3); // → [1, 2, 3]
    new Array({}); // → [{}]
    ```
  - Array 생성자 함수는 new 연산자와 함께 호출하지 않더라도 배열을 생성하는 생성자 함수로 동작한다. 이는 Array 생성자 함수 내부에서 new.target을 확인하기 때문이다.
    ```
    Array(1, 2, 3); // → [1, 2, 3]
    ```
  
  **new.target**
  - 함수 내부에서 new.target을 사용하면 new 연산자와 함께 생성자 함수로서 호출되었는지 확인할 수 있다.
  - new 연산자와 함께 생성자 함수로서 호출되면 함수 내부의 new.target은 함수 자신을 가리킨다.
  - new 연산자 없이 일반 함수로서 호출된 함수 내부의 new.target은 undefined다.
    ```
    function Circle(radius) {
      if (!new.target) {
        // new 연산자와 함께 생성자 함수를 재귀 호출하여 생성된 인스턴스를 반환한다.
        return new Circle(radius);
      }

      this.radius = radius;
      this.getDiameter = function () { 
        return 2 * this.radius;
      };
    }
    
    // new 연산자 없이 생성자 함수를 호출하여도 new.target을 통해 생성자 함수로서 호출된다.
    const circle = Circle(5);
    console.log(circle.getDiameter()); // 10
    ```

### 27.4.3 Array.of
- ES6에서 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성한다.
- Array.of는 Array 생성자 함수와 다르게 전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.
  ```
  Array.of(1); // → [1]
  Array.of(1, 2, 3); // → [1, 2, 3]
  Array.of('string'); // → ['string']
  ```

### 27.4.4 Array.from
- ES6에서 도입된 Array.from 메서드는 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.
  - 유사 배열 객체: 마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖는 객체
  - 이터러블 객체: 반복 가능한 객체, for ... of 문으로 순회할 수 있다.
    ```
    // 유사 배열 객체를 변환하여 배열 생성
    Array.from({ length: 2, 0: 'a', 1: 'b' }); // → ['a', 'b']
    
    // 이터러블을 변환하여 배열 생성 (문자열은 이터러블이다.)
    Array.from('Hello'); // → ['H', 'e', 'l', 'l', 'o']
    
    // Array.from에 length만 존재하는 유사 배열 객체를 전달하면 undefined 요소로 채운다.
    Array.from({ length: 3 }); // → [undefined, undefined, undefined]
    
    // Array.from은 두 번째 인수로 전달한 콜백 함수의 반환값으로 구성된 배열을 반환한다.
    Array.from({ length: 3 }, (_, i) => i); // → [0, 1, 2]
    ```

## 27.5 배열 요소의 참조
- 배열의 요소를 참조할 때에는 대괄호([]) 표기법을 사용하며, 대괄호 안에는 인덱스가 와야 한다.
  ```
  const arr = [1, 2];

  console.log(arr[0]); // 1
  console.log(arr[1]); // 2
  // 존재하지 않는 요소에 접근하면 undefined가 반환된다.
  console.log(arr[2]); // undefined
  
  // 희소 배열
  const arr2 = [1, , 3];

  // 희소 배열의 존재하지 않는 요소를 참조해도 undefined가 반환된다.
  console.log(arr2[1]); // undefined
  ```

## 27.6 배열 요소의 추가와 갱신
- 객체에 프로퍼티를 동적으로 추가할 수 있는 것처럼 배열에도 요소를 동적으로 추가할 수 있다.
  ```
  const arr = [0];

  // 배열 요소의 추가
  arr[1] = 1;

  console.log(arr); // [0, 1]
  console.log(arr.length); // 2

  // 요소 값의 갱신
  arr[1] = 10;

  console.log(arr); // [0, 10]
  
  // 희소 배열
  arr[100] = 100;
  console.log(arr); // [0, 10, empty × 98, 100]
  console.log(arr.length); // 101
  ```
- 정수 이외의 값을 인덱스처럼 사용하면 요소가 생성되는 것이 아니라 프로퍼티가 생성된다. 이때 추가된 프로퍼티는 length 프로퍼티 값에 영향을 주지 않는다.
  ```
  const arr = [];

  // 배열 요소의 추가
  arr[0] = 1;
  arr['1'] = 2;

  // 프로퍼티 추가
  arr['foo'] = 3;
  arr.bar = 4;
  arr[1.1] = 5;
  arr[-1] = 6;

  console.log(arr); // [1, 2, foo: 3, bar: 4, 1.1: 5, -1: 6]

  console.log(arr.length); // 2
  ```

## 27.7 배열 요소의 삭제
- 배열은 사실 객체이기 때문에 배열의 특정 요소를 삭제하기 위해 delete 연산자를 사용할 수 있다.
  ```
  const arr = [1, 2, 3];

  // 배열 요소의 삭제
  delete arr[1];
  console.log(arr); // [1, empty, 3]

  // length 프로퍼티에 영향을 주지 않는다. 즉, 희소 배열이 된다.
  console.log(arr.length); // 3
  ```
- 희소 배열을 만드는 delete 연산자 보다 희소 배열을 만들지 않으면서 배열의 특정 요소를 완전히 삭제하는 Array.prototype.splice 메서드를 사용하는 것이 좋다.
  ```
  const arr = [1, 2, 3];

  // Array.prototype.splice(삭제를 시작할 인덱스, 삭제할 요소 수)
  // arr[1]부터 1개의 요소를 제거
  arr.splice(1, 1);
  console.log(arr); // [1, 3]

  // length 프로퍼티가 자동 갱신된다.
  console.log(arr.length); // 2
  ```

## 27.8 배열 메서드
- 자바스크립트는 배열을 다룰 때 유용한 다양한 빌트인 메서드를 제공한다.
- 배열 메서드는 결과물을 반환하는 패턴이 두 가지이므로 주의가 필요하다.
  - 원본 배열을 직접 변경하는 메서드
  - 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드
  ```
  const arr = [1];

  // push 메서드는 원본 배열(arr)을 직접 변경한다.
  arr.push(2);
  console.log(arr); // [1, 2]

  // concat 메서드는 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환한다.
  const result = arr.concat(3);
  console.log(arr); // [1, 2]
  console.log(result); // [1, 2, 3]
  ```

### 27.8.1 Array.isArray
- Array.isArray는 Array 생성자 함수의 정적 메서드다.
- Array.isArray 메서드는 전달된 인수가 배열이면 true, 배열이 아니면 false를 반환한다.
  ```
  // true
  Array.isArray([]);
  Array.isArray([1, 2]);
  Array.isArray(new Array());

  // false
  Array.isArray();
  Array.isArray({});
  Array.isArray(null);
  Array.isArray(1);
  Array.isArray('Array');
  Array.isArray(true);
  Array.isArray({ 0: 1, length: 1 });
  ```

### 27.8.2 Array.prototype.indexOf
- 원본 배열에 인수로 전달한 요소와 중복되는 요소가 여러 개 있다면 첫 번째로 검색된 요소의 인덱스를 반환한다.
- 원본 배열에 인수로 전달한 요소가 존재하지 않으면 -1을 반환한다.
  ```
  const arr = [1, 2, 2, 3];

  arr.indexOf(2); // → 1
  arr.indexOf(4); // → -1
  // 두 번째 인수는 검색을 시작할 인덱스다. 두 번째 인수를 생략하면 처음부터 검색된다.
  arr.indexOf(2, 2); // → 2
  ```
- indexOf 메서드는 배열에 특정 요소가 존재하는지 확인할 때 유용하다.
  ```
  const foods = ['apple', 'banana', 'orange'];

  if (foods.indexOf('orange') === -1) {
    foods.push('orange');
  }
  ```
- indexOf 메서드 대신 ES7에서 도입된 Array.prototype.includes 메서드를 사용하면 가독성이 더 좋다.
  ```
  if(!foods.includes('orange')) {
    foods.push('orange');
  }
  ```

### 27.8.3 Array.prototype.push
- push 메서드는 인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다.
- push 메서드는 원본 배열을 직접 변경한다.
  ```
  const arr = [1, 2];

  let result = arr.push(3, 4);
  console.log(result); // 4

  console.log(arr); // [1, 2, 3, 4]
  ```
- push 메서드는 성능 면에서 좋지 않다. 마지막 요소로 추가할 요소가 하나뿐이라면 아래의 방법이 push 메서드보다 빠르다.
  ```
  const arr = [1, 2];

  arr[arr.length] = 3;
  console.log(arr); // [1, 2, 3]
  ```
- ES6의 스프레드 문법을 사용하면 원본 배열을 직접 변경하는 부수 효과가 없다.
  ```
  const arr = [1, 2];

  const newArr = [...arr, 3];
  console.log(newArr); // [1, 2, 3]
  ```

### 27.8.4 Array.prototype.pop
- pop 메서드는 원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다.
- 원본 배열이 빈 배열이면 undefined를 반환한다.
- pop 메서드는 원본 배열을 직접 변경한다.
  ```
  const arr = [1, 2];

  let result = arr.pop();
  console.log(result); // 2
  console.log(arr); // [1]
  ```
- 스택: 후입 선출(LIFO - Last In First Out) 방식의 자료구조
  - 푸시: 스택에 데이터를 밀어 넣는 것
  - 팝: 스택에서 데이터를 꺼내는 것
  
  ![스택](image.png)

### 27.8.5 Array.prototype.unshift
- unshift 메서드는 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 프로퍼티 값을 반환한다.
- unshift 메서드는 원본 배열을 직접 변경한다.
  ```
  const arr = [1, 2];

  let result = arr.unshift(3, 4);
  console.log(result); // 4
  console.log(arr);    // [3, 4, 1, 2]
  ```
- unshift 메서드는 원본 배열을 직접 변경하는 부수 효과가 있기 때문에 ES6의 스프레드 문법을 사용하는 것이 좋다.
  ```
  const arr = [1, 2];

  const newArr = [3, ...arr];
  console.log(newArr); // [3, 1, 2]
  console.log(arr);    // [1, 2]
  ```

### 27.8.6 Array.prototype.shift
- shift 메서드는 원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다.
- 원본 배열이 빈 배열이면 undefined를 반환한다.
- shift 메서드는 원본 배열을 직접 변경한다.
  ```
  const arr = [1, 2];

  let result = arr.shift();
  console.log(result); // 1
  console.log(arr);    // [2]
  ```
- 큐: 선입선출(FIFO - First In First Out) 방식의 자료구조
![큐](image-1.png)

### 27.8.7 Array.prototype.concat
- concat 메서드는 인수로 전달된 값들(배열 또는 원시값)을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다.
- 인수로 전달한 값이 배열인 경우 배열을 해체하여 새로운 배열의 요소로 추가한다.
- 원본 배열은 변경되지 않는다.
  ```
  const arr1 = [1, 2];
  const arr2 = [3, 4];

  let result = arr1.concat(arr2);
  console.log(result); // [1, 2, 3, 4]

  result = arr1.concat(3);
  console.log(result); // [1, 2, 3]

  result = arr.concat(arr2, 5);
  console.log(result); // [1, 2, 3, 4, 5]

  console.log(arr1); // [1, 2]
  ```
- push와 unshift 메서드는 concat 메서드와 유사하게 동작하지만 다음과 같이 미묘한 차이가 있다.
  - push와 unshift: 원본 배열을 직접 변경, 원본 배열을 반드시 변수에 저장해 두어야 한다.
    concat: 원본 배열을 변경하지 않고 새로운 배열을 반환, 반환값을 반드시 변수에 할당받아야 한다.
  - 인수로 전달받은 값이 배열인 경우 push와 unshift 메서드는 배열을 그대로 원본 배열의 마지막/첫 번째 요소로 추가하지만 concat 메서드는 인수로 전달받은 배열을 해체하여 새로운 배열의 마지막 요소로 추가한다.
    ```
    const arr = [3, 4];

    arr.unshift([1, 2]);
    arr.push([5, 6]);
    console.log(arr); // [[1, 2], 3, 4, [5, 6]]

    let result = [1, 2].concat([3, 4]);
    result = result.concat([5, 6]);
    console.log(result); // [1, 2, 3, 4, 5, 6]
    ```
- concat 메서드는 ES6 스프레드 문법으로 대체할 수 있다.
  ```
  let result = [...[1, 2], ...[3, 4]];
  console.log(result); // [1, 2, 3, 4]
  ```
- **push/unshift 메서드와 concat 메서드를 사용하는 대신 ES6의 스프레드 문법을 일관성 있게 사용하는 것을 권장한다.**

### 27.8.8 Array.prototype.splice
- 원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 splice 메서드를 사용한다. 
- splice 메서드는 3개의 매개변수가 있으며 원본 배열을 직접 변경한다.
  - start: 원본 배열의 요소를 제거하기 시작할 인덱스다. start만 지정하면 원본 배열의 start부터 모든 요소를 제거한다 start가 음수인 경우 배열의 끝에서의 인덱스를 나타낸다. 만약 start가 -1이면 마지막 요소를 가리키고 -n이면 마지막에서 n번째 요소를 가리킨다.
  - deleteCount: 원본 배열의 요소를 제거하기 시작할 인덱스인 start부터 제거할 요소의 개수다. deleteCount가 0인 경우 아무런 요소도 제거되지 않는다(옵션).
  - items: 제거한 위치에 삽입할 요소들의 목록이다. 생략할 경우 원본 배열에서 요소들을 제거하기만 한다(옵션).
- 제거한 요소가 배열로 반환된다.
  ```
  const arr = [1, 2, 3, 4];

  // 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입한다.
  const result = arr.splice(1, 2, 20, 30);

  console.log(result); // [2, 3]
  console.log(arr); // [1, 20, 30, 4]
  ```
  ```
   const arr = [1, 2, 3, 4];

   // 원본 배열의 인덱스 1부터 0개의 요소를 제거하고 그 자리에 새로운 요소 100을 삽입한다.
   const result = arr.splice(1, 0, 100);
 
   console.log(arr); // [l, 100, 2, 3, 4]
   console.log(result); // []
  ```
  ```
  const arr = [1, 2, 3, 4];

  // 원본 배열의 인덱스 1부터 2개의 요소를 제거한다.
  const result = arr.splice(1, 2);
 
  console.log(arr); // [1, 4]
  console.log(result); // [2, 3]
  ```
  ```
  const arr = [1, 2, 3, 4];

  // 원본 배열의 인덱스 1부터 모든 요소를 제거한다.
  const result = arr.splice(1);
  
  console.log(arr); // [1]
  console.log(result); // [2, 3, 4]
  ```
  
### 27.8.9 Array.prototype.slice
- slice 메서드는 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다.
- 2개의 매개변수가 있으며, 원본 배열은 변경되지 않는다.
  - start: 복사를 시작할 인덱스다. 음수인 경우 배열의 끝에서의 인덱스를 나타낸다. 예를 들어, slice(-2)는 배열의 마지막 두 개의 요소를 복사하여 배열로 반환한다.
  - end: 복사를 종료할 인덱스다. 이 인덱스에 해당하는 요소는 복사되지 않는다. end는 생략 가능하며 생략 시 기본값은 length 프로퍼티 값이다.
  ```
  const arr = [1, 2, 3];

  arr.slice(0, 1); // → [1]
  arr.slice(1, 2); // → [2]
  
  arr.slice(1); // → [2, 3]
  
  arr.slice(-1) // → [3]
  arr.slice(-2); // → [2, 3]

  // 인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환한다(얕은 복사)
  const copy = arr.slice();
  console.log(copy); // [1, 2, 3]
  console.log(copy === arr); // false
  
  console.log(arr); // [1, 2, 3]
  ```
  
- 얕은 복사는 객체의 첫 번째 레벨만 복사하고, 중첩된 객체는 참조를 공유한다.
  ```
  // 원본 배열
  let arr1 = [1, 2, 3, { name: 'John' }, [4, 5]];

  // slice()를 이용한 얕은 복사
  let arr2 = arr1.slice();

  // 배열 내부의 객체나 배열은 여전히 원본과 같은 참조를 가짐
  arr2[3].name = 'Doe';   // 객체 내부의 속성 변경
  arr2[4][0] = 100;       // 배열 내부 값 변경

  console.log(arr1);  // [1, 2, 3, { name: 'Doe' }, [100, 5]]
  console.log(arr2);  // [1, 2, 3, { name: 'Doe' }, [100, 5]]

  ```


