## 값
- 값은 식(표현식)이 평가되어 생성된 결과이다.
    ```
    // 10 + 20 이 평가되어 생성된 숫자 값 30이 변수에 할당된다.
    var sum = 10 + 20;
    ```

## 리터럴
- 리터럴은 사람이 이해할 수 있는 문자 또는 약속된 기호를 사용해 값을 생성하는 표기법이다.
- 자바스크립트 엔진은 코드가 실행되는 시점인 런타임에 리터럴을 평가해 값을 생성한다. 즉, 리터럴은 값을 생성하기 위해 미리 약속한 표기법이다.
- 리터럴의 종류에는 정수 리터럴(100), 부동소수점 리터럴(10.5), 문자열 리터럴('hello'), 불리언 리터럴(true), 객체 리터럴({name: 'Lee', address: 'Seoul'}), 함수 리터럴(function() {}) 등이 있다.

## 표현식
- 표현식은 값으로 평가될 수 있는 문이다. 표현식이 평가되면 새로운 값을 생성하거나 기존 값을 참조한다.
- 표현식은 리터럴, 식별자(변수, 함수 등의 이름), 연산자, 함수 호출 등의 조합으로 이뤄질 수 있다. 값으로 평가될 수 있는 문은 모두 표현식이다.
    ```
    10 // 리터럴 표현식
    sum // 식별자 표현식 (선언이 이미 존재한다고 가정)
    10 + 20 // 연산자 표현식
    ```

## 문
- 문: 프로그램을 구성하는 기본 단위이자 최소 실행 단위
- 프로그램: 문의 집합으로 이루어진 것
- 프로그래밍: 문을 작성하고 순서에 맞게 나열하는 것
- 문은 컴퓨터에 내리는 명령이다.
- 문은 선언문, 할당문, 조건문, 반복문 등으로 구분할 수 있다.

## 세미콜론과 세미콜론 자동 삽입 기능
- 세미콜론(;)은 문의 종료를 나타낸다.
- 문을 끝낼 때는 세미콜론을 붙여야 하지만, 중괄호로 묶은 코드 블록({...})은 문의 종료를 의미하는 자체 종결성을 갖기 때문에 세미콜론을 붙이지 않는다.
-  자바스크립트 엔진이 소스코드를 해석할 때 문의 끝이라고 예측되는 지점에 세미콜론을 자동으로 붙여주는 세미콜론 자동 삽입 기능(ASI)이 암묵적으로 수행되기 때문에 세미콜론은 생략이 가능하다.

## 표현식인 문과 표현식이 아닌 문
- 표현식인 문: 값으로 평가될 수 있는 문 (ex. 변수 선언문)
- 표현식이 아닌 문: 값으로 평가될 수 없는 문 (ex. 할당문)
- 표현식인 문과 표현식이 아닌 문을 구별하는 가장 간단하고 명료한 방법은 변수에 할당해 보는 것이다. 
    ```
    // 표현식이 아닌 문은 값처럼 사용할 수 없다
    var foo = var x; 
    ```
-  할당문은 표현식인 문이기 때문에 값처럼 사용할수 있다. 할당문은 할당한 값으로 평가된다. 즉, x = 100은 x 변수에 할당한 값 100으로 평가된다.
    ```
    // 표현식인 문은 값처럼 사용할 수 있다
    var foo = x = 100;
    console.1og(foo); // 100
    ```