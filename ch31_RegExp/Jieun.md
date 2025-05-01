## 31.1 정규 표현식이란?
- 정규 표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.
- 정규 표현식은 문자열을 대상으로 패턴 매칭 기능을 제공한다. 패턴 매칭 기능이란 특정 패턴과 일치하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.
- 정규표현식은 주석이나 공백을 허용하지 않고 여러 가지 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다.

## 31.2 정규 표현식의 생성
- 정규 표현식 객체(RegExp 객체)를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다. 일반적인 방법은 정규 표현식 리터럴을 사용하는 것이다.
- 정규 표현식 리터럴은 패턴과 플래그로 구성된다.
![정규 표현식 리터럴](image.png)
- RegExp 생성자 함수
  ```
  /**
   * pattern: 정규 표현식의 패턴
   * flags: 정규 표현식의 플래그(g, i, m, u, y)
   */
  new RegExp(pattern[, flags])
  ```

## 31.3 RegExp 메서드

### 31.3.1 RegExp.prototype.exec
- exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다. (매칭 결과가 없는 경우 null 반환)
- g 플래그(문자열 내의 모든 패턴을 검색)를 지정해도 첫 번째 매칭 결과만 반환하므로 주의!
  ```
  const target = 'Is this all there is?';
  const regExp = /is/;

  regExp.exec(target); 
  // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
  ```

### 31.3.2 RegExp.prototype.test
- test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검사하여 매칭 결과를 불리언 값으로 반환한다.
  ```
  const target = 'Is this all there is?';
  const regExp = /is/;

  regExp.test(target); // true
  ```

### 31.3.3 String.prototype.match
- String 표준 빌트인 객체가 제공하는 match 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.
  ```
  const target = 'Is this all there is?';
  const regExp = /is/;

  target.match(regExp);
  // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
  ```
- exec 메서드는 g 플래그를 지정해도 첫 번째 매칭 결과만 반환하지만, String.prototype.match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.
  ```
  const target = 'Is this all there is?';
  const regExp = /is/g;

  target.match(regExp); // ['is', 'is']
  ```

## 31.4 플래그
- 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다. 6개의 플래그 중 중요한 3개의 플래그는 아래와 같다.

  |플래그|의미|설명|
  |---|---|---|
  |i|Ignore case|대소문자를 구별하지 않고 패턴을 검색한다.|
  |g|Global|대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다.|
  |m|Multi line|문자열의 행이 바뀌더라도 패턴 검색을 계속 한다.|

- 플래그는 옵션이므로 선택적으로 사용할 수 있으며, 순서와 상관없이 하나 이상의 플래그를 동시에 설정할 수도 있다.
- 플래그를 사용하지 않은 경우 대소문자를 구별해서 패턴을 검색하며, 문자열에 패턴 검색 매칭 대상이 1개 이상 존재해도 첫 번째 매칭한 대상만 검색하고 종료한다.
  ```
  const target = 'Is this all there is?';
  
  // is 문자열을 대소문자를 구별하여 한 번만 검색
  target.match(/is/); 
  // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
  
  // is 문자열을 대소문자를 구별하지 않고 한 번만 검색
  target.match(/is/i);
  // ['Is', index: 0, input: 'Is this all there is?', groups: undefined]
  
  // is 문자열을 대소문자를 구별하여 전역 검색
  target.match(/is/g);
  // ['is', 'is']
  
  // is 문자열을 대소문자를 구별하지 않고 전역 검색
  target.match(/is/ig);
  // ['Is', 'is', 'is']
  ```

## 31.5 패턴
- 정규 표현식의 패턴은 문자열의 일정한 규칙을 표현하기 위해 사용한다.
- 패턴은 /로 열고 닫으며 문자열의 따옴표는 생략한다. 따옴표를 포함하면 따옴표까지도 패턴에 포함되어 검색된다.
- 어떤 문자열 내에 패턴과 일치하는 문자열이 존재할 때 '정규 표현식과 매치한다'고 표현한다.

### 31.5.1 문자열 검색
- 정규 표현식 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 또는 문자열을 검색한다.
  ```
  const target = 'Is this all there is?';
  const regExp = /is/;

  regExp.test(target); // true
  target.match(regExp); // ['is', index: 5, input: 'Is this all there is?', groups: undefined]
  ```
  ```
  const target = 'Is this all there is?';
  const regExp = /is/i; // 플래그 i - 대소문자 구별 X

  target.match(regExp); // ['Is', index: 0, input: 'Is this all there is?', groups: undefined]
  ```
  ```
  const target = 'Is this all there is?';
  const regExp = /is/ig; // 플래그 g - 패턴과 일치하는 모든 문자열 전역 검색

  target.match(regExp); // ['Is', 'is', 'is']
  ```

### 31.5.2 임의의 문자열 검색
- .은 임의의 문자 한 개를 의미한다. 문자의 내용은 무엇이든 상관없다.
  ```
  const target = 'Is this all there is?';
  const regExp = /.../g;

  target.match(regExp); // ['Is ', 'thi', 's a', 'll ', 'the', 're ', 'is?']
  ```

### 31.5.3 반복 검색
- {m,n}은 앞선 패턴(다음 예제의 경우 A)이 최소 m번, 최대 n번 반복되는 문자열을 의미한다.
- 콤마 뒤에 공백이 있으면 정상 동작하지 않으므로 주의!
  ```
  const target = 'A AA B BB Aa Bb AAA';

  // 'A'가 최소 1번, 최대 2번 반복되는 문자열을 전역 검색
  const regExp = /A{1,2}/g;
  
  target.match(regExp); // ['A', 'AA', 'A', 'AA', 'A']
  ```
- {n}은 앞선 패턴이 n번 반복되는 문자열을 의미한다. {n}은 {n,n}과 같다.
  ```
  // 'A'가 2번 반복되는 문자열을 전역 검색
  const regExp = /A{2}/g;
  
  target.match(regExp); // ['AA', 'AA']
  ```
- {n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.
  ```
  // 'A'가 최소 2번 이상 반복되는 문자열을 전역 검색
  const regExp = /A{2,}/g;
  
  target.match(regExp); // ['AA', 'AAA']
  ```
- +는 앞선 패턴이 최소 한 번 이상 반복되는 문자열을 의미한다. +는 {1,}과 같다.
  ```
  // 'A'가 최소 한 번 이상 반복되는 문자열('A', 'AA', 'AAA', ...)을 전역 검색
  const regExp = /A+/g;
  
  target.match(regExp); // ['A', 'AA', 'A', 'AAA']
  ```
- ?는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다. ?는 {0,1}과 같다.
  ```
  const target = 'color colour';

  // 'colo' 다음 'u'가 최대 한 번(0번 포함) 이상 반복되고 'r'이 이어지는
  // 문자열 'color', 'colour'를 전역 검색
  const regExp = /colou?r/g;

  target.match(regExp); // ['color', 'colour']
  ```

### 31.5.4 OR 검색
- |는 or의 의미를 갖는다.
  ```
  const target = 'A AA B BB Aa Bb';

  // 'A' 또는 'B'를 전역 검색
  let regExp = /A|B/g;
  target.match(regExp); // ['A', 'A', 'A', 'B', 'B', 'B', 'A', 'B']
  
  // 분해되지 않은 단어 레벨로 검색하기 위해서는 +를 함께 사용
  // 'A' 또는 'B'가 한 번 이상 반복되는 문자열을 전역 검색
  regExp = /A+|B+/g;
  target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']
  ```
- [] 내의 문자는 or로 동작한다. 그 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복한다.
  ```
  const target = 'A AA B BB Aa Bb';
  
  // const regExp = /A+|B+/g 와 같다
  const regExp = /[AB]+/g;
  target.match(regExp); // ['A', 'AA', 'B', 'BB', 'A', 'B']
  ```
- 범위를 지정하려면 [] 내에 -를 사용한다.
  ```
  const target = 'A AA BB ZZ Aa Bb';

  // 'A' ~ 'Z'가 한 번 이상 반복되는 문자열 전역 검색 (대문자 알파벳 검색)
  const regExp = /[A-Z]+/g;
  target.match(regExp); // ['A', 'AA', 'BB', 'ZZ', 'A', 'B']
  ```
  ```
  const target = 'AA BB Aa Bb 12';
  
  // 대소문자 구별하지 않고 알파벳 검색
  const regExp = /[A-Za-z]+/g;
  
  target.match(regExp); // ['AA', 'BB', 'Aa', 'Bb']
  ```
  ```
  const target = 'AA BB 12,345';

  // '0' ~ '9'가 한 번 이상 반복되는 문자열을 전역 검색 (숫자 검색)
  const regExp = /[0-9]+/g;
  target.match(regExp); // ['12', '345']
  ```
  ```
  const target = 'AA BB 12,345';

  // '0' ~ '9' 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
  const regExp = /[0-9,]+/g;
  target.match(regExp); // ['12,345']
  ```
- \d는 숫자를 의미한다. \d는 [0-9]와 같다.
- \D는 숫자가 아닌 문자를 의미한다.
  ```
  const target = 'AA BB 12,345';

  // const regExp = /[0-9,]+/g; 와 같다.
  let regExp = /[\d,]+/g;
  target.match(regExp); // ['12,345']
  
  // '0' ~ '9'가 아닌 문자(숫자가 아닌 문자) 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
  regExp = /[\D,]+/g;
  target.match(regExp); // ['AA BB ', ',']
  ```
- \w는 알파벳, 숫자, 언더스코어를 의미한다. \w는 [A-Za-z0-9_]와 같다.
- \W는 알파벳, 숫자, 언더스코어가 아닌 문자를 의미한다.
  ```
  const target = 'Aa Bb 12,345 _$%&';

  // 알파벳, 숫자, 언더스코어, ','가 한 번 이상 반복되는 문자열을 전역 검색
  let regExp = /[\w,]+/g;
  target.match(regExp); // ['Aa', 'Bb', '12,345', '_']
  
  // 알파벳, 숫자, 언더스코어가 아닌 문자 또는 ','가 한 번 이상 반복되는 문자열을 전역 검색
  const regExp = /[\W,]+/g;
  target.match(regExp); // [' ', ' ', ',', ' ', '$%&']
  ```

### 31.5.5 NOT 검색
- [] 내의 ^은 not의 의미를 갖는다.
  - [0-9] = \d
  - [^0-9] = \D
  - [A-Za-z0-9_] = \w
  - [^A-Za-z0-9_] = \W
  ```
  const target = 'AA BB 12 Aa Bb';

  // 숫자를 제외한 문자열을 전역 검색
  const regExp = /[^0-9]+/g;
  target.match(regExp); // ['AA BB ', ' Aa Bb']
  ```

### 31.5.6 시작 위치로 검색
- [] 밖의 ^은 문자열의 시작을 의미한다.
  ```
  const target = 'https://www.naver.com';

  // 'https'로 시작하는지 검사
  const regExp = /^https/;
  regExp.test(target); // true
  ```

### 31.5.7 마지막 위치로 검색
- $는 문자열의 마지막을 의미한다.
  ```
  const target = 'https://www.naver.com';

  // 'com'으로 끝나는지 검사
  const regExp = /com$/;
  regExp.test(target); // true
  ```

## 31.6 자주 사용하는 정규표현식

