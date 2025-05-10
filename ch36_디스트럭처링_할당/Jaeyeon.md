- 디스트럭처링 할당(구조 분해 할당)은 구조화된 배열과 같은 이터러블 또는 객체를 destructuring(비구조화, 구조 파괴)하여 1개 이상의 변수에 개별적으로 할당하는 것을 말한다.

## 배열 디스트럭처링 할당
- 배열의 각 요소를 배열로부터 추출하여 1개 이상의 변수에 할당한다.
- 이때 배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야하며, 할당 기준은 배열의 인덱스다. 즉 순서대로 할당된다.
```
const arr = [1, 2, 3];
const [one, two, three] = arr;
console.log(one, two, three); // 1 2 3
```

- 우변에 배열 리터럴 형태이어야 하며, 할당하지 않으면 에러가 발생한다.
```
const [x, y] = [1, 2];
const [x, y]; // SyntaxError
const [a, b] = {}; // TypeError
```

- 배열 디스트럭처링 할당의 기준은 배열의 인덱스다. 즉, 순서대로 할당된다.
```
const [a, b] = [1, 2];
console.log(a, b) // 1 2

const [c, d] = [1];
console.log(c, d); // 1 undefined

const [e, f] = [1, 2, 3];
console.log(e, f); // 1 2

const [g, , h] = [1, 2, 3];
console.log(g, h); // 1 3
```

- 배열 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.
```
// 기본값
const [a, b, c = 3] = [1, 2];
console.log(a, b, c); // 1 2 3

// 기본값보다 할당된 값이 우선한다.
const [e, f = 10, g = 3] = [1, 2];
console.log(e, f, g); //  1 2 3
```

- 배열 디스트럭처링 할당을 위한 변수에 Rest 파라미터와 유사하게 Rest 요소를 사용할 수 있다.
Rest 요소는 Rest 파라미터와 마찬가지로 반드시 마지막에 위치해야 한다.

```
// Rest 요소
const [x, ...y] = [1, 2, 3];
console.log(x, y); // 1 [2, 3]
```

## 객체 디스트럭처링 할당
- 객체의 각 프로퍼티를 객체로부터 추출하여 1개 이상의 변수에 할당한다.
- 이때 객체 디스트럭처림 할당의 대상(할당문의 우변)은 객체이어야하며, 할당 기준은 프로퍼티 키다.
- 즉, 순서는 의미 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다.

```
const user = {
	firstName: 'Ungmo',
	lastName: 'Lee',
};

// 변수 lastName, firstName을 선언하고 user 객체를 디스트럭처링하여 할당한다.
// 이때 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다. 순서는 의미가 없다.
const { lastName, firstName } = user;

console.log(firstName, lastName); // Ungmo Lee
```

- 객체 디스트럭처링 할당을 위해서는 할당 연산자 왼쪽에 프로퍼티 값을 할당받을 변수를 선언해야 한다. 이때 변수를 객체 리터럴 형태로 선언한다.
```
const { lastName, firstName } = { firstName: 'Ungmo', lastName: 'Lee' };
```
- 우변에 객체 또는 객체로 평가될 수 있는 표현식(문자열, 숫자, 배열 등)을 할당하지 않으면 에러가 발생한다.
```
const {lastName, firstName}; // SyntaxError
const {lastName, firstName} = null; // TypeError
```

- 위 예제에서 객체 리터럴 형태로 선언한 변구 lastName, firstName이다. 프로퍼티 축약 표현을 통해 선언한 것이다.
```
const { lastName, firstName } = user;
// 위와 아래는 동치다.
const { lastName: lastName, firstName: firstName } = user;
```

- 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 변수를 선언한다.
```
const user = {
  firstName: 'Ungmo',
  lastName: 'Lee',
};

// 프로퍼티 키를 기준으로 디스트럭처링 할당이 이루어진다.
const { lastName: ln, firstName: fn } = user;

console.log(fn, ln); // Ungmo Lee
```

- 객체 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.
```
const { firstName = 'Ungmo', lastName } = { lastName: 'Lee' };
console.log(firstName, lastName); // Ungmo Lee

const { lastName: ln, firstName: fn = 'Ungmo' } = { lastName: 'Lee' };
console.log(fn, ln); // Ungmo Lee
```

- 객체 디스트럭처링 할당은 객체에서 프로퍼티 키로 필요한 프로퍼티 값만 추출하여 변수에 할당하고 싶을 때 유용하다.
```
const str = 'Hello';
// String 래퍼 객체로부터 length 프로퍼티만 추출한다.
const { length } = str;
console.log(length); // 5

const todo = { id: 1, content: 'HTML', completed: true };
// todo 객체로부터 id 프로퍼티만 추출한다.
const { id } = todo;
console.log(id); // 1
```

- 객체 디스트럭처링 할당은 객체를 인수로 전달받은 함수의 매개변수에도 사용할 수 있다.
```
function printTodo({content, completed}) {
  console.log(`할일 ${content}은 ${completed ? '완료' : '비완료'} 상태입니다.`);
}

printTodo({ id: 1, content: 'HTML', completed: true }); // 할일 HTML은 완료 상태입니다.
```

- 배열의 요소가 객체인 경우 배열 디스트럭처링 할당과 객체 디스트럭처링 할당을 혼용할 수 있다.
```
const todos = [
  { id: 1, content: 'HTML', completed: true },
  { id: 2, content: 'CSS', completed: false },
  { id: 3, content: 'JS', completed: false },
];

// todos 배열의 두 번째 요소인 객체로부터 id 프로퍼티만 추출한다.
const [, { id }] = todos;
console.log(id); // 2
```

- 중첩 객체의 경우는 다음과 같이 사용한다.
```
const user = {
  name: 'Lee',
  address: {
    zipCode: '03068',
    city: 'Seoul',
  },
};

// address 프로퍼티 키로 객체를 추출하고 이 객체의 city 프로퍼티 키로 값을 추출한다.
const { address: { city } } = user;
console.log(city); // 'Seoul'
```

- 객체 디스트럭처링 할당을 위한 Rest 파라미터나 Rest 요소와 유사하게 Rest 프로퍼티 ... 을 사용할 수 있다.
- 반드시 마지막에 위치해야 한다.
```
// Rest 프로퍼티
const { x, ...rest } = { x: 1, y: 2, z: 3 };
console.log(x, rest); // 1 {y: 2, z: 3}
```
