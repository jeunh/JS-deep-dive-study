
## 27.9.2 Array.prototype.forEach

함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 조건문과 반복문을 제거하고 상태 변경을 억제하여 코드의 복잡성을 줄이는 패러다임. 전통적인 for문은 반복을 위한 변수 선언, 조건식, 증감식 등을 요구하며 로직의 가독성을 해치는 단점이 있음.

forEach 메서드는 반복문을 추상화한 고차 함수로, 내부적으로 반복문을 사용하여 배열을 순회하고 콜백 함수를 반복 호출함. 이 메서드는 함수형 프로그래밍 스타일을 실현하는 데 유용.

### 예제 27-95
```js
const numbers = [1, 2, 3];
const pows = [];

for (let i = 0; i < numbers.length; i++) {
  pows.push(numbers[i] ** 2);
}
console.log(pows); // [1, 4, 9]
```

### 예제 27-96
```js
const numbers = [1, 2, 3];
const pows = [];

numbers.forEach(item => pows.push(item ** 2));
console.log(pows); // [1, 4, 9]
```

forEach 메서드는 배열의 요소 수만큼 콜백 함수를 호출하며, 콜백에는 배열의 요소값, 인덱스, 배열 자체가 인수로 전달됨.

### 예제 27-97
```js
[1, 2, 3].forEach((item, index, arr) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
});

// 요소값: 1, 인덱스: 0, this: [1,2,3]
// 요소값: 2, 인덱스: 1, this: [1,2,3]
// 요소값: 3, 인덱스: 2, this: [1,2,3]
```

원본 배열은 변경되지 않지만, 콜백 함수 내부에서 변경할 수 있음.

### 예제 27-98
```js
const numbers = [1, 2, 3];
// 세 번째 매개변수 arr가 원본 배열 numbers라서 콜백 함수 안에서 arr 직접 변경할 수 있음 
numbers.forEach((item, index, arr) => {
  arr[index] = item ** 2;
});
console.log(numbers); // [1, 4, 9]
```

forEach의 반환값은 항상 undefined.

### 예제 27-99
```js
const result = [1, 2, 3].forEach(console.log);
console.log(result); // undefined
```

forEach의 두 번째 인수로 콜백 함수 내에서 사용할 this를 지정 가능.

### 예제 27-100 (문제 발생)
```js
class Numbers {
  numberArray = [];

  multiply(arr) {
    arr.forEach(function (item) {
      this.numberArray.push(item * item);
    });
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
// TypeError: Cannot read property 'numberArray' of undefined
```

strict mode로 인해 콜백 함수의 this는 undefined가 되며 오류 발생.(class 내부는 strict mode가 적용되므로)

### 예제 27-101 (this 지정)
```js
class Numbers {
  numberArray = [];

  multiply(arr) {
    arr.forEach(function (item) {
      this.numberArray.push(item * item);
    }, this);
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray); // [1, 4, 9]
```

thisArg를 통해 multiply 내부의 this를 콜백 함수에도 전달.

### 예제 27-102 (화살표 함수 사용)
```js
class Numbers {
  numberArray = [];

  multiply(arr) {
    arr.forEach(item => this.numberArray.push(item * item));
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray); // [1, 4, 9]
```

화살표 함수는 this를 상위 스코프에서 가져오기 때문에 더 간단한 해결책.
화살표 함수는 함수 자체의 this 바인딩을 갖지 않아서 화살표 내부에서 this를 참조하면 상위 스코프인 multiply 메서드 내부의 this를 그대로 참조.

### 예제 27-103 (polyfill)
```js
// 브라우저가 forEach를 지원하지 않으면 (= undefined일 경우) 직접 Array.prototype.forEach를 정의해서 추가함
if (!Array.prototype.forEach) {
  Array.prototype.forEach = function (callback, thisArg) {
    // 첫 번째 인수로 받은 callback이 함수가 아니면 강제 오류 발생(just 방어코드)
    if (typeof callback !== 'function') {
      throw new TypeError(callback + ' is not a function');
    }
    // this는 forEach를 호출한 배열 (arr.forEach(...)에서 arr)
    // 혹시 this가 배열이 아니라 배열 유사 객체일 수도 있어서 강제로 객체로 변환
    const O = Object(this);
    // >>> 0은 부호 없는 32비트 정수로 변환하는 트릭. 안전하게 정수 길이 확보.
    const len = O.length >>> 0;
    let k = 0;

    // this로 사용할 두 번째 인수를 전달받지 못하면 전역 객체를 this로 사용하겠다는 뜻임
    // if (!thisArg) {
    //   thisArg = window;
    // } 이 뜻임.

    thisArg = thisArg || window;

    for (var i = 0; i < len; i++) {
     // 명시적으로 this를 지정할 수 있는 기능이 필요해서 반복문을 돌림.
     // forEach가 내부적으로 이렇게 진행되고 있다는 뜻임. 
     // callback이라는 함수를 thisArg를 this로 바인딩해서 호출해라(첫번째 인자에서 생기는 일임)
     // 0[i], i, this는 그대로 전달됨. 맨 마지막 this는 원본 배열에 해당하는 this임. 
     // Array.prototype.forEach(function(item, index, array) {
    // 이 array가 바로 이 'this' 임(원본 배열)
    //});
      callback.call(thisArg, O[i], i, this);
    }
  };
}
```

forEach는 내부적으로 반복문을 사용하지만 로직은 메서드 내부에 은닉됨. 로직의 흐름을 단순화하고 가독성 향상.

break, continue 문 사용 불가. 중간에 순회를 종료할 수 없음.

### 예제 27-104
```js
[1, 2, 3].forEach(item => {
  console.log(item);
  if (item > 1) break; // SyntaxError
});

[1, 2, 3].forEach(item => {
  console.log(item);
  if (item > 1) continue; // SyntaxError
});
```

forEach는 break, continue 문법을 사용할 수 없음. 모든 요소 순회 필요.

희소 배열의 경우 존재하지 않는 요소는 순회하지 않음.

### 예제 27-105
```js
const arr = [1, , 3];

for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 1, undefined, 3
}

arr.forEach(v => console.log(v)); // 1, 3
```

for 문은 빈 슬롯도 순회하며 undefined 출력, forEach는 존재하는 값만 순회.

정리하면, forEach는 반복문을 추상화하여 가독성을 높이고 코드의 명확성을 제공함. 
성능이 아주 중요한 상황이 아니라면 for문보다 권장되는 방식.


## 27.9.3 Array.prototype.map

map 메서드는 자신을 호출한 배열의 모든 요소를 순회하며, 인수로 전달된 콜백 함수를 반복 호출하고 콜백 함수의 반환값들로 구성된 **새로운 배열**을 반환. 이때 원본 배열은 변경되지 않음.

forEach와의 공통점은 배열 요소를 순회하며 콜백을 반복 호출한다는 점. 하지만 forEach는 반환값이 없고, map은 반환값으로 새로운 배열을 생성함.

map 메서드가 생성하는 배열의 length는 원본 배열과 항상 동일하며, 1대1 매핑을 보장함.

콜백 함수는 배열의 요소값, 인덱스, 배열 자체(this)를 인수로 전달받음.

### 예제 27-106
```js
const numbers = [1, 4, 9];
const roots = numbers.map(item => Math.sqrt(item));
console.log(roots);    // [1, 2, 3]
console.log(numbers);  // [1, 4, 9]
```

원본 배열은 그대로이며, 콜백 함수의 반환값들로 구성된 새로운 배열이 생성됨.

### 예제 27-107
```js
[1, 2, 3].map((item, index, arr) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
  return item;  
});

// 요소값: 1, 인덱스: 0, this: [1,2,3]
// 요소값: 2, 인덱스: 1, this: [1,2,3]
// 요소값: 3, 인덱스: 2, this: [1,2,3]
```

map 메서드는 콜백 함수에 요소값, 인덱스, 배열을 순서대로 전달.

### 예제 27-108 (thisArg 사용)
```js
class Prefixer {
  constructor(prefix) {
    this.prefix = prefix;
  }

  add(arr) {
    return arr.map(function (item) {
      return this.prefix + item;
    }, this);
  }
}

const prefixer = new Prefixer('-webkit-');
console.log(prefixer.add(['transition', 'user-select']));
// ['-webkit-transition', '-webkit-user-select']
```

map의 두 번째 인수로 this를 전달하여 콜백 내부에서 사용할 수 있도록 함.

### 예제 27-109 (화살표 함수 사용)
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

화살표 함수는 this를 바인딩하지 않기 때문에 상위 스코프의 this를 그대로 사용함. 
이 경우 add 메서드 내부의 this가 prefixer 인스턴스를 가리키므로, 별도의 thisArg 없이도 동작함.


## 27.9.4 Array.prototype.filter

filter 메서드는 자신을 호출한 배열의 모든 요소를 순회하며, 콜백 함수의 반환값이 true인 요소로만 구성된 **새로운 배열**을 반환함. 원본 배열은 변경되지 않음.

forEach, map과의 차이점은 반환되는 배열이 특정 조건을 만족하는 요소만 필터링된다는 점.

콜백 함수에는 요소값, 인덱스, 배열(this)가 인수로 전달됨. 두 번째 인수로 thisArg 지정 가능.

### 예제 27-110

```js
const numbers = [1, 2, 3, 4, 5];
const odds = numbers.filter(item => item % 2);
console.log(odds); // [1, 3, 5]
```

### 예제 27-111

```js
[1, 2, 3].filter((item, index, arr) => {
  console.log(`요소값: ${item}, 인덱스: ${index}, this: ${JSON.stringify(arr)}`);
  return item % 2;
});
// 콘솔 출력:
// 요소값: 1, 인덱스: 0, this: [1,2,3]
// 요소값: 2, 인덱스: 1, this: [1,2,3]
// 요소값: 3, 인덱스: 2, this: [1,2,3]
// 결과: [1, 3]
```



filter는 중복된 요소도 모두 제거 가능. 단 하나만 제거하려면 indexOf로 해당 요소의 인덱스를 찾은 후 splice로 해당 인덱스의 요소를 제거해야 함.

### 예제 27-112

```js
class Users {
  constructor() {
    this.users = [
      { id: 1, name: 'Lee' },
      { id: 2, name: 'Kim' }
    ];
  }

  findById(id) {
    return this.users.filter(user => user.id === id);
  }

  remove(id) {
    this.users = this.users.filter(user => user.id !== id);
  }
}

const users = new Users();
let user = users.findById(1);
console.log(user); // [{ id: 1, name: 'Lee' }]
users.remove(1);
user = users.findById(1);
console.log(user); // []
```



---

## 27.9.5 Array.prototype.reduce

reduce 메서드는 배열의 모든 요소를 순회하며 콜백 함수의 반환값을 누적하여 **하나의 결과값**을 반환함. 원본 배열은 변경되지 않음.

reduce 메서드는 두 개의 인수를 받음:

1. 콜백 함수: 배열의 각 요소를 순회하며 실행됨. 콜백 함수는 다음 네 개의 인수를 전달받음

   - 누적값(accumulator): 이전 콜백의 반환값 또는 초기값
   - 현재값(currentValue): 현재 순회 중인 요소
   - 인덱스(index): 현재 요소의 인덱스
   - 배열(array): reduce를 호출한 원본 배열

2. 초기값(initialValue): 첫 번째 순회 시 누적값으로 사용됨. 생략 가능하지만, 이 경우 배열의 첫 번째 요소가 누적값이 되며 순회는 두 번째 요소부터 시작됨. 초기값을 생략하면 빈 배열에서 reduce를 호출할 경우 TypeError가 발생할 수 있으므로, 예외를 방지하기 위해 항상 명시하는 것이 바람직함.

#### **- 누적 합계**

### 예제 27-113

```js
const sum = [1, 2, 3, 4].reduce(
  // 첫 번째 인수: 콜백 함수 (accumulator, currentValue, index, array) => accumulator + currentValue
  (accumulator, currentValue, index, array) => accumulator + currentValue,
  // 두 번째 인수: 초기값 0
  0
);
console.log(sum); // 10
```

reduce의 초기값은 0이며, 콜백은 배열 요소 수만큼 반복 호출됨. 호출 시 누적값(acc)과 현재값(cur)이 인수로 전달됨. 호출 흐름은 다음과 같음:

- 1회차: acc = 0, cur = 1 → 반환값: 1
- 2회차: acc = 1, cur = 2 → 반환값: 3
- 3회차: acc = 3, cur = 3 → 반환값: 6
- 4회차: acc = 6, cur = 4 → 반환값: 10

최종 반환값: 10



#### **- 평균 구하기**

### 예제 27-114

```js
const values = [1, 2, 3, 4, 5, 6];
const average = values.reduce((acc, cur, i, { length }) =>
  i === length - 1 ? (acc + cur) / length : acc + cur, 0);
// 마지막 순회가 아니면 누적값을 반환하고 마지막 순회면 누적값으로 평균을 구해 반환
console.log(average); // 3.5
```



#### **- 최댓값 구하기**

### 예제 27-115

```js
const values = [1, 2, 3, 4, 5];
const max = values.reduce((acc, cur) => (acc < cur ? cur : acc), 0);
console.log(max); // 5
```

####

Math.max를 사용하자 (reduce보다 더 직관적인 방법)

### 예제 27-116

```js
const values = [1, 2, 3, 4, 5];
const max = Math.max(...values);
console.log(max); // 5
```



#### **- 요소의 중복 횟수 구하기**

### 예제 27-117

```js
const count = fruits.reduce((acc, cur) => {
  // acc: 누적 객체. 초기값은 빈 객체 {}
  // cur: 현재 순회 중인 요소 (과일 이름)
  acc[cur] = acc[cur] || 0; // 아직 등장하지 않은 과일이면 0으로 초기화
  acc[cur]++;               // 해당 과일의 개수 1 증가
  return acc;               // 누적 객체 반환
}, {}); // 초기값: 빈 객체

```



#### **- 중첩 배열 평탄화**

### 예제 27-118

```js
const values = [1, [2, 3], [4, [5, 6]]];
const flatten = values.reduce((acc, cur) => acc.concat(cur), []);
console.log(flatten); // [1, 2, 3, 4, [5, 6]]
```

#### flat 사용 (reduce보다 더 직관적인 방법)

### 예제 27-119

```js
[1, [2, 3, [4, 5]]].flat();   // [1, 2, 3, [4, 5]]
[1, [2, 3, [4, 5]]].flat(2);  // [1, 2, 3, 4, 5]
```

flat 메서드의 인수는 중첩 배열을 얼마나 깊게 평탄화할지를 지정하는 값으로, 기본값은 1임. 예제에서 flat(2)는 최대 2단계까지 중첩된 배열을 평탄화함.



#### **- 중복 요소 제거 (reduce)**

### 예제 27-120

```js
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];
const result = values.reduce((unique, val, i, _values) =>
  _values.indexOf(val) === i ? 
 [...unique, val]:unique,[]
 );
 // _values.indexOf(val) === i인 경우는 해당 요소가 배열에서 처음 등장한 위치라는 뜻
  // 중복된 값은 이후에 다시 등장하면 indexOf(val) !== i가 되어 필터링됨
console.log(result); // [1, 2, 3, 5, 4]

```



#### 중복 요소 제거 (filter)

### 예제 27-121

```js
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];

const result = values.filter((val, i, _values) => {
  
  return _values.indexOf(val) === i;
});

console.log(result); // [1, 2, 3, 5, 4]
```

#### 중복 요소 제거 (Set)

### 예제 27-122

```js
const values = [1, 2, 1, 3, 5, 4, 5, 3, 4, 4];
const result = [...new Set(values)];
console.log(result); // [1, 2, 3, 5, 4]
```

#### 초기값 생략한 reduce

### 예제 27-123

```js
const sum = [1, 2, 3, 4].reduce((acc, cur) => acc + cur);
console.log(sum); // 10
```

#### 빈 배열에 초기값 생략 시 에러

### 예제 27-124

```js
const sum = [].reduce((acc, cur) => acc + cur);
// TypeError: Reduce of empty array with no initial value
```

#### 초기값 지정 시 정상 동작

### 예제 27-125

```js
const sum = [].reduce((acc, cur) => acc + cur, 0);
console.log(sum); // 0
```



#### 객체의 특정 프로퍼티 값 합산할 경우에도 반드시 초기값 전달해야 함

### 예제 27-126

```js
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 }
];

const priceSum = products.reduce((acc, cur) => acc.price + cur.price);
console.log(priceSum); // NaN
```
## 27.9.6 Array.prototype.some

some 메서드는 배열의 요소 중 콜백 함수가 true를 반환하는 요소가 하나라도 있으면 true를 반환함. 모두 false일 경우 false 반환. 빈 배열에서는 항상 false를 반환함.

콜백 함수에는 요소값, 인덱스, 배열(this)가 인수로 전달되며, 두 번째 인수로 thisArg를 전달할 수 있음.

### 예제 27-128

```js
[5, 10, 15].some(item => item > 10); // true
[5, 10, 15].some(item => item < 0);  // false
['apple', 'banana', 'mango'].some(item => item === 'banana'); // true
[].some(item => item > 3); // false
```

## 27.9.7 Array.prototype.every

every 메서드는 배열의 모든 요소가 콜백 함수의 조건을 만족해야 true를 반환함. 하나라도 false면 false 반환. 빈 배열일 경우 항상 true를 반환함.

### 예제 27-129

```js
[5, 10, 15].every(item => item > 3);  // true
[5, 10, 15].every(item => item > 10); // false
[].every(item => item > 3);           // true
```

## 27.9.8 Array.prototype.find

find 메서드는 조건을 만족하는 첫 번째 요소를 반환함. 반환값이 true인 첫 번째 요소 하나만 반환하며, 없으면 undefined를 반환함. 반환값은 배열이 아니라 요소 자체임.

### 예제 27-130

```js
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 3, name: 'Choi' },
  { id: 4, name: 'Park' }
];

users.find(user => user.id === 2); // { id: 2, name: 'Kim' }
```

```js
[1, 2, 3].filter(item => item === 2); // [2]
[1, 2, 3].find(item => item === 2);   // 2
```

## 27.9.9 Array.prototype.findIndex

findIndex 메서드는 조건을 만족하는 첫 번째 요소의 인덱스를 반환함. 없으면 -1 반환.

### 예제 27-132

```js
const users = [
  { id: 1, name: 'Lee' },
  { id: 2, name: 'Kim' },
  { id: 3, name: 'Choi' },
  { id: 4, name: 'Park' }
];

users.findIndex(user => user.id === 2); // 1
users.findIndex(user => user.name === 'Park'); // 3

function predicate(key, value) {
  return item => item[key] === value;
}

users.findIndex(predicate('id', 2));       // 1
users.findIndex(predicate('name', 'Park')); // 3
```

## 27.9.10 Array.prototype.flatMap

flatMap은 map을 먼저 수행한 후 flat(1)로 1단계 평탄화를 수행하는 고차 함수임. 깊이 지정 불가.

### 예제 27-133

```js
const arr = ['hello', 'world'];
arr.map(x => x.split('')).flat(); 
// ['h','e','l','l','o','w','o','r','l','d']
```

배열의 평탄화 깊이를 지정해야 할 경우 map 한 뒤에 flat 메서드 각각 호출해야 한다.

### 예제 27-134

```js
const arr = ['hello', 'world'];
arr.flatMap((str, index) => [index, [str, str.length]]);
// [[0, ['hello', 5]], [1, ['world', 5]]]

arr.map((str, index) => [index, [str, str.length]]).flat(2);
// [0, 'hello', 5, 1, 'world', 5]
```




#### 객체 속성 합산 (정상 동작)

### 예제 27-127

```js
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 }
];

const priceSum = products.reduce((acc, cur) => acc + cur.price, 0);
console.log(priceSum); // 600
```

정리하면, reduce는 고차 함수 중 가장 범용적이며 모든 배열 처리 로직을 구현할 수 있음.
하지만 안전성과 예외 방지를 위해 초기값은 항상 명시하는 것이 좋음.

