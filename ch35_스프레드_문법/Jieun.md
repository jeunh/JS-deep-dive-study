- ES6에서 도입된 스프레드 문법 ...은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.
- 스프레드 문법을 사용할 수 있는 대상은 Array, String, Map, Set, DOM 컬렉션(NodeList, HTMLCollection), arguments와 같이 for...of 문으로 순회할 수 있는 이터러블에 한정된다.
  ```
  console.log(...[1, 2, 3]); // 1 2 3
  console.log(...'Hello'); // H e l l o
  console.log(...new Map([['a', '1'], ['b', '2']])); // ['a', '1'] ['b', '2']
  console.log(...new Set([1, 2, 3])); // 1 2 3
  ```

## 35.1 함수 호출문의 인수 목록에서 사용하는 경우
  ```
  const arr = [1, 2, 3];
  Math.max(arr); // → NaN
  // Math.max 메서드에 숫자가 아닌 배열을 인수로 전달하면 최대값을 구할 수 없으므로 NaN을 반환한다.

  Math.max(1, 2, 3); // → 3

  // 스프레드 문법 사용
  Math.max(...arr); // → 3
  ```
- 스프레드 문법과 Rest 파라미터는 서로 반대의 개념이다. 형태가 동일하여 혼동할 수 있으므로 주의해야한다.
  ```
  // Rest 파라미터는 인수들의 목록을 배열로 전달받는다.
  function foo(...rest) {
    console.log(rest); // 1, 2, 3 → [1, 2, 3]
  }
  
  // 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
  // [1, 2, 3] → 1, 2, 3
  foo(...[1, 2, 3]);
  ```

## 35.2 배열 리터럴 내부에서 사용하는 경우
- 스프레드 문법을 배열 리터럴에서 사용하면 ES5에서 사용하던 기존의 방식보다 더욱 간결하고 가독성 좋게 표현할 수 있다.

### 35.2.1 concat
- 2개의 배열을 1개의 배열로 결합하고 싶은 경우
  ```
  // ES5
  var arr = [1, 2].concat([3, 4]);
  console.log(arr); // [1, 2, 3, 4]
  
  // ES6
  const arr = [...[1, 2], ...[3, 4]];
  console.log(arr); // [1, 2, 3, 4]
  ```

### 35.2.2 splice
- 어떤 배열의 중간에 다른 배열의 요소를 추가하거나 제거하려면 splice 메서드를 사용한다.
  ```
  // ES5
  var arr1 = [1, 4];
  var arr2 = [2, 3];
  arr1.splice(1, 0, arr2);
  console.log(arr1); // [1, [2, 3], 4]
  // splice 메서드 세 번째 인수로 배열을 전달하면 배열 자체가 추가된다.
  
  // ES6
  const arr1 = [1, 4];
  const arr2 = [2, 3];
  arr1.splice(1, 0, ...arr2);
  console.log(arr1); // [1, 2, 3, 4]
  ```

### 35.2.3 배열 복사
- 배열을 복사하려면 slice 메서드를 사용한다.
- slice 메서드와 스프레드 문법을 이용한 배열 복사 모두 얕은 복사하여 새로운 복사본을 생성한다.
  ```
  // ES5
  var origin = [1, 2];
  var copy = origin.slice();
  console.log(copy); // [1, 2]
  console.log(copy === origin); // false

  // ES6
  const origin = [1, 2];
  const copy = [...origin];
  console.log(copy); // [1, 2]
  console.log(copy === origin); // false
  ```

## 35.3 객체 리터럴 내부에서 사용하는 경우
- 스프레드 문법의 대상은 이터러블이어야 하지만 스프레드 프로퍼티 제안은 일반 객체를 대상으로도 스프레드 문법의 사용을 허용한다.
  ```
  // 객체 복사(얕은 복사)
  const obj = { x: 1, y: 2 };
  const copy = { ...obj };
  console.log(copy); // {x: 1, y: 2}
  console.log(obj === copy); // false
  
  // 객체 병합
  const marged = { ...{x: 1, y: 2}, ...{y:10, z: 3} };
  console.log(marged); // {x: 1, y: 10, z: 3}
  ```
