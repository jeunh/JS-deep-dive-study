# 31 RegExp

## 31.1 정규 표현식이란?

정규 표현식(regular expression)은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어(formal language)다. 정규 표현식은 자바스크립트의 고유 문법이 아니며, 대부분의 프로그래밍 언어의 표준 라이브러리에 포함되어 있다. 자바스크립트는 `RegExp`의 정규 표현식 문법을 ES3부터 도입하였다.

정규 표현식은 문자열을 대상으로 **패턴 매칭 기능**을 제공한다. 패턴 매칭 기능이란 특정 패턴을 포함하는 문자열을 검색하거나 추출 또는 치환할 수 있는 기능을 말한다.

예를 들어, 회원가입 화면에서 사용자로부터 입력받은 휴대폰 전화번호가 유효한 휴대폰 전화번호인지 체크하는 경우를 생각해보자. 휴대폰 전화번호는 `숫자 3개` + `-` + `숫자 4개` + `-` + `숫자 4개`로 일관된 패턴을 가진다. 이처럼 특정한 패턴을 가지는 문자열은 정규 표현식으로 정의하고 사용자로부터 입력받은 문자열이 이 패턴을 만족하는지 테스트함으로써 유효성 검사를 할 수 있다.

```js
// 사용자로부터 입력받은 휴대폰 전화번호
const tel = '010-1234-5678';

// 정규 표현식 리터럴로 휴대폰 전화번호의 형식을 정의한다.
const regExp = /^\d{3}-\d{4}-\d{4}$/;

// tel이 휴대폰 전화번호 패턴과 일치하는지 테스트(검증)한다.
regExp.test(tel); // → true
```

## 31.2 정규 표현식의 생성

정규 표현식 객체(RegExp 객체)를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다. 일반적인 방법은 정규 표현식 리터럴을 사용하는 것이다. 정규 표현식 리터럴은 아래와 같이 표현한다.

```js
/정규표현식/플래그
```

정규 표현식 리터럴은 패턴과 플래그로 구성된다. 정규 표현식 리터럴을 사용하여 간단한 정규 표현식 객체를 생성해 보자.

```js
const target = 'Is this all there is?';

// 패턴: is → 대상문자열 구별하지 않고 검색한다.
const regExp = /is/i;

// target 문자열에 대해 정규 표현식 regExp의 패턴을 검색하여 매칭 결과를 반환한다.
regExp.test(target); // → true
```

### new RegExp(pattern\[, flags])
생성자 함수 사용 예시

```js
const target = 'Is this all there is?';
const regExp = new RegExp(/is/i); // ES6
regExp.test(target); // → true
```

## 31.3 RegExp 메서드

### 31.3.1 RegExp.prototype.exec

`exec` 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환하고, 매칭되지 않으면 null을 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;

regExp.exec(target);
// → ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

### 31.3.2 RegExp.prototype.test

`test` 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;

regExp.test(target); // → true
```

### 31.3.3 String.prototype.match

`match` 메서드는 대상 문자열과 인수로 전달받은 정규 표현식과의 매칭 결과를 배열로 반환한다.

```js
const target = 'Is this all there is?';
const regExp = /is/;

target.match(regExp);
// → ["is", index: 5, input: "Is this all there is?", groups: undefined]
```

exec 메서드는 문자열 내의 모든 패턴을 검색하는 g플래그를 지정해도 첫 번째 매칭 결과만 반환했다. 하지만 String.prototype.match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.
```js
const target = 'Is this all there is?';
const regExp = /is/g;
target.match(regExp) // ["is", "is"]
```
## 31.4 플래그

패턴과 함께 정규 표현식을 구성하는 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용된다. 플래그는 총 6개가 있으며, 자주 사용하는 3가지는 아래와 같다.

| 플래그 | 의미          | 설명                                   |
| --- | ----------- | ------------------------------------ |
| i   | Ignore case | 대소문자를 구별하지 않고 패턴을 검색한다.              |
| g   | Global      | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색한다. |
| m   | Multi line  | 문자열의 행이 바뀌더라도 패턴 검색을 계속한다.           |


### 31.5 패턴

정규 표현식은 "일정한 규칙(패턴)을 가진 문자열의 집합"을 표현하기 위해 사용하는 형식 언어다. 정규 표현식은 패턴과 플래그로 구성된다. 정규 표현식의 패턴은 문자역의 일정한 규칙을 표현하기 위해 사용하며, 정규 표현식의 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.

패턴은 `/`로 열고 닫으며 문자열의 따옴표를 생략한다. 따옴표를 포함하면 따옴표까지도 패턴에 포함되어 검색된다. 또한 패턴은 특별한 의미를 가지는 메타문자(meta character) 또는 기호로 표현할 수 있다. 어떤 문자열에 패턴과 일치하는 문자열이 존재할 때 "정규 표현식과 매칭(match)한다"고 표현한다. 패턴을 표현하는 몇 가지 방법을 살펴보자.

#### 31.5.1 문자열 검색

```js
const target = 'Is this all there is?';
const regExp = /is/;
regExp.test(target); // → true
```

i는 대소문자 구별 없이 검색
```js
const regExp = /is/i;
target.match(regExp); // → ['Is', index: 0, input: 'Is this all there is?', groups: undefined]
```

플래그 g는 문자열 전역 검색
```js
const regExp = /is/ig;
target.match(regExp); // → ["Is", "is", "is"]
```

#### 31.5.2 임의의 문자열 검색

.은 임의의 문자 한 개를 의미함. 아래는 점 세개라서 3자리 문자열과 매치함.
```js
const regExp = /.../g;
target.match(regExp); // → ["Is ", "thi", "s a", "ll ", "the", "re ", "is?"]
```

#### 31.5.3 반복 검색
{m,n}은 앞선 패턴(다음 예제의 경우 A)가 최소 m번, 최대 n번 반복되는 문자열을 의미.
```js
const target = 'A AA B BB Aa Bb AAA';
const regExp = /A{1,2}/g;
target.match(regExp); // → ["A", "AA", "A", "AA"]
```

{n}은 앞선 패턴이 n번 반복되는 문자열을 의미함.

```js
const regExp = /A{2}/g;
target.match(regExp); // → ["AA", "AA"]
```

{n,}은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미함
```js
const regExp = /A{1,}/g;
target.match(regExp); // → ["A", "AA", "A", "AAA"]
```

+는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미. 즉, +는 {1,}과 같음. 
```js
const regExp = /A+/g;
target.match(regExp); // → ["A", "AA", "A", "AAA"]
```

?는 앞선 패턴이 최대 한 번(0번 포함)이상 반복되는 문자열을 의미함. 즉 ?는 {0,1}과 같다. 
```js
const target = 'color colour';
const regExp = /colou?r/g;
target.match(regExp); // → ["color", "colour"]
```

#### 31.5.4 OR 검색
| 는 or의 의미를 갖는다. 다음 예제는 A 또는 B를 의미한다.
```js
const target = 'A AA B BB Aa Bb';
const regExp = /A|B/g;
target.match(regExp); // → ["A", "A", "B", "B", "A", "B"]
```

분해되지 않은 단어 레벨로 검색하기 위해서는 +를 사용한다. 
```js
// A 또는 B가 한 번 이상 반복되는 문자열을 전역 검색함
const regExp = /A+|B+/g;
target.match(regExp); // → ["A", "AA", "B", "BB", "A", "B"]
```

위 예제를 간단히 표현하면 아래와 같음. [] 내의 문자는 or로 동작함. 그 뒤에 +를 사용하면 앞선 패턴을 한 번 이상 반복
```js
const regExp = /[AB]+/g;
target.match(regExp); // → ["A", "AA", "B", "BB", "A", "B"]
```

범위를 지정하려면 [] 내에 - 를 사용. 아래 예제는 대문자 알파벳을 검색
```js
const regExp = /[A-Z]+/g;
target.match(regExp); // → ["A", "AA", "BB", "ZZ", "A", "B"]
```

대소문자를 구별하지 않고 알파벳을 검색하는 방법은 다음과 같음
```js
const target = 'AA BB Aa Bb 12';
const regExp = /[A-Za-z]+/g;
target.match(regExp); // → ["AA", "BB", "Aa", "Bb"]
```

숫자를 검색하려면 아래처럼 하면 됨
```js
const target = 'AA BB 12,345';
const regExp = /[0-9]+/g;
target.match(regExp); // → ["12", "345"]
```

위 예제의 경우 쉼표 때문에 매칭 결과가 분리되므로 쉼표를 패턴에 포함시켜야 함(아래처럼)

```js
const regExp = /[0-9,]+/g;
target.match(regExp); // → ["12,345"]
```

위 예제를 간단히 표현하면 아래와 같음. \d 는 숫자를 의미. \D는 문자를 의미.

```js
// 0~9 또는 , 가 한 번 이상 반복되는 문자열을 전역 검색함
const regExp = /[\d,]+/g;
target.match(regExp); // → ["12,345"]
```

```js
// 문자 또는 , 가 한 번 이상 반복되는 문자열을 전역 검색함
const regExp = /[\D,]+/g;
target.match(regExp); // → ["AA BB "]
```

\w 는 알파벳, 숫자, 언더스코어임. \W 는 알파벳, 숫자, 언더스코어를 제외한 문자임.
```js

const target = 'Aa Bb 12,345 _$%&';

// 알파벳, 숫자, 언더스코어가 한번 이상 검색되는 문자열 전역 검색
let regExp = /[\w,]+/g;
target.match(regExp); // → ["Aa", "Bb", "12,345", "_"]
// 그 이외거 검색
regExp = /[\W,]+/g;
target.match(regExp); // → [" ", " ", " %&"]
```

#### 31.5.5 NOT 검색
[...] 내의 ^ 는 not의 의미를 가짐. 
```js
const target = 'AA BB 12,345';
const regExp = /[^0-9]+/g;
target.match(regExp); // → ["AA BB ", ","]
```

#### 31.5.6 시작 위치로 검색
[...] 밖에 있는 ^는 문자열의 시작을 의미함
```js
const target = 'https://poiemaweb.com';
const regExp = /^https/;
regExp.test(target); // → true
```

#### 31.5.7 마지막 위치로 검색
$는 문자열의 마지막을 의미함
```js
// com으로 끝나는지 검색함
const target = 'https://poiemaweb.com';
const regExp = /com$/;
regExp.test(target); // → true
```

