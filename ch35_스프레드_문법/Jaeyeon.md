- ES6에서 도입된 스프레드 문법(전개 문법) ...은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록을 만든다.
- 사용할 수 있는 대상은 for...of 문으로 순회할 수 있는 이터러블에 한정된다.

```
// ...[1, 2, 3]은 [1, 2, 3]을 개별 요소로 분리한다.(-> 1, 2, 3)
console.log(...[1, 2, 3]); // 1 2 3

// 문자열은 이터러블이다.
console.log(...'Hello'); // H e l l o

// Map과 Set은 이터러블이다.
console.log(...new Map([['a', '1'], ['b', '2'],])); // [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Set([1, 2, 3])); // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다.
console.log(...{ a: 1, b: 2 }); // TypeError
```

- 스프레드 문법 ...이 연산자가 아니기 때문에 변수에 할당할 수 없다.
```
// 스프레드 문법의 결과는 값이 아니다.
const list = ...[1, 2, 3]; // SyntaxError
```

- 다음과 같이 쉼표로 구분한 값의 목록을 사용하는 문맥에서만 사용할 수 있다.
  - 함수 호출문의 인수 목록
  - 배열 리터럴의 요소 목록
  - 객체 리터럴의 프로퍼티 목록

## 함수 호출문의 인수 목록에서 사용하는 경우
- 요소들의 집합인 배열을 펼쳐서 개별적인 값들의 목록으로 만든 후, 이를 함수의 인수 목록으로 전달해야하는 경우가 있다.
```
const arr = [1, 2, 3];

// 스프레드 문법을 사용하여 배열 arr을 1, 2, 3을 펼쳐서 Math.max에 전달한다.
// Math.max(...[1, 2, 3])은 Math.max(1, 2, 3)과 같다.
const max =  Math.max(...arr); // 3
```


### Rest 파라미터와 형태다 동일하여 혼동할수 있으니 주의

- Rest 파라미터
힘수에 전달된 인수들의 목록을 배열로 전달받기 위해 매개변수 이름 앞에 ... 을 붙이는 것 이다.

- 스프레드 문법
여러 개의 값이 하나로 뭉쳐 있는 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만드는 것이다.

- 따라서 Rest 파라미터와 스프레드 문법은 서로 반대개념이다.

```
// Rest 파라미터는 인수들의 목록을 배열로 전달 받는다.
function foo(...rest) {
    console.log(rest); // 1, 2, 3 -> [1, 2, 3]
}

// 스프레드 문법은 배열과 같은 이터러블을 펼쳐서 개별적인 값들의 목록을 만든다.
// [1, 2, 3] -> 1, 2, 3
foo(...[1, 2, 3];
```

## 배열 리터럴 내부에서 사용하는 경우
- ES5에서 사용하던 기본의 방식보다 더욱 간결하고 가독성 좋게 표현할 수 있다. ES5와 사용하던 방식과 비교하여 살펴보자.



### concat

```
// ES5
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]

// ES6
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4];
```

### splice
```
// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

/*
apply 메서드의 2번째 인수(배열)는 apply 메서드가 호출한 splice 메서드의 인수목록이다.
apply 메서드의 2번째 인수 [1, 0].concat(arr2)는 [1, 0, 2, 3]으로 평가된다.
따라서 splice 메서드에 apply 메서의 2번째 인수 [1, 0, 2, 3]이 해체되어 전달된다.
즉. arr1[1]부터 0개의 요소를 제거하고 그 자리(arr1[1])에 새로운 요소(2, 3)를 삽입한다.
*/
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [1, 2, 3, 4]

let arr1 = [1, 4]
let arr2 = [2, 3]

// ES6
arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4];
```

### 배열 복사

```
// ES5
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(origin === copy); // false

// ES6
let origin = [1, 2];
let copy = [...origin];

console.log(copy) // [1, 2];
console.log(origin === copy); // false
```


### 이터러블을 배열로 변환
```
// ES5
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments 를 배열로 변환
  var args = Array.prototype.slice.call(arguments);

  return args.reduce(function(pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3)); // 6

// ES6
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments 를 배열로 변환
  return [...arguments].reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6
```

- 위 예제보다 나은 방법은 Rest 파라미터를 사용하는 것이다.
```
const sum = (...args) => args.reduce((pre, cur) => pre + cur, 0);
console.log(sum(1, 2, 3));
```

- 단, 이터러블이 아닌 유사 배열 객체는 스프레드 문법의 대상이 될 수 없다.
```
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

const arr = [...arrayLike]; // TypeError
```

- 이터러블이 아닌 유사 배열 객체를 배열로 변경하려면 ES6에서 도입된 Array.from 메서드를 사용한다.
```
// Array.from 은 유사 배열 객체 또는 이터러블을 배열로 변환한다.
Array.from(arrayLike); // [1, 2, 3]
```

## 객체 리터럴 내부에서 사용하는 경우
```
// 객체 병합, 프로퍼티가 중복되는 경우 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = { ...{ x: 1, y: 2 }, y: 100 };
console.log(changed) // { x: 1, y: 100 }

// 프로퍼티 추가
const added = { ...{ x: 1, Y: 2 }, z: 0 };
console.log(added) // { x: 1, Y: 2, z: 0 }
```
