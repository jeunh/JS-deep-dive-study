- 디스트럭처링 할당(구조 분해 할당)은 구조화된 배열과 같은 이터러블 또는 객체를 destructuring(비구조화, 구조 파괴)하여 1개 이상의 변수에 개별적으로 할당하는 것을 말한다. 
- 배열과 같은 이터러블 또는 객체 리터럴에서 필요한 값만 추출하여 변수에 할당할 때 유용하다.

## 36.1 배열 디스트럭처링 할당
- 배열 디스트럭처링 할당의 대상(할당문의 우변)은 이터러블이어야 하며, 할당 기준은 배열의 인덱스다.
  ```
  // ES5
  var arr = [1, 2, 3];

  var one = arr[0];
  var two = arr[1];
  var three = arr[2];

  console.log(one, two, three); // 1 2 3
  ```
  ```
  // ES6
  const arr = [1, 2, 3];
  const [one, two, three] = arr;
  console.log(one, two, three); // 1 2 3
  ```
- 변수의 개수와 이터러블의 요소 개수가 반드시 일치할 필요는 없다.
  ```
  const [a, b] = [1, 2];
  console.log(a, b); // 1 2
  
  const [c, d] = [1];
  console.log(c, d); // 1 undefined
  
  const [e, f] = [1, 2, 3];
  console.log(e, f); // 1 2
  
  const [g, , h] = [1, 2, 3];
  console.log(g, h); // 1 3
  ```
- 배열 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수도 있다.
  ```
  const [a, b, c=3] = [1, 2];
  console.log(a, b, c); // 1 2 3
  
  // 기본값보다 할당된 값이 우선
  const [e, f=10, g=3] = [1, 2];
  console.log(e, f, g); // 1 2 3
  ```
- 배열 디스트럭처링 할당을 위한 변수에 Rest 요소 ...을 사용할 수 있다. 반드시 마지막에 위치해야 한다.
  ```
  const [x, ...y] = [1, 2, 3];
  console.log(x, y); // 1 [2, 3]
  ```

## 36.2 객체 디스트럭처링 할당
- 객체 디스트럭처링 할당의 대상(할당문의 우변)은 객체이어야 하며, 할당 기준은 프로퍼티 키다. 
- 순서는 의미가 없으며 선언된 변수 이름과 프로퍼티 키가 일치하면 할당된다.
  ```
  const user = { firstName: 'Gildong', lastName: 'Hong' };
  const { lastName, firstName } = user;
  console.log(firstName, lastName); // Gildong Hong
  ```
- 객체의 프로퍼티 키와 다른 변수 이름으로 프로퍼티 값을 할당받으려면 다음과 같이 변수를 선언한다.
  ```
  const user = { firstName: 'Gildong', lastName: 'Hong' };
  const { lastName: ln, firstName:fn } = user;
  console.log(fn, ln); // Gildong Hong
  ```
- 객체 디스트럭처링 할당을 위한 변수에 기본값을 설정할 수 있다.
  ```
  const { firstName = 'Gildong', lastName } = { lastName: 'Hong' };
  console.log(firstName, lastName); // Gildong Hong
  ```
- 객체를 인수로 전달받는 함수의 매개변수에도 사용할 수 있다.
  ```
  function printTodo({ content, completed }) {
    console.log(`할일 ${content}은 ${completed ? '완료' : '비완료'} 상태입니다.`);
  }

  printTodo({ id: 1, content: 'HTML', completed: true }); // 할일 HTML은 완료 상태입니다.
  ```
- 객체 디스트럭처링 할당을 위한 변수에 Rest 파라미터 ...을 사용할 수 있다. 반드시 마지막에 위치해야 한다.
  ```
  const { x, ...rest } = { x: 1, y: 2, z: 3 };
  console.log(x, rest); // 1 {y: 2, z: 3}
  ```
