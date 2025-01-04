## 8.1 블록문
- 블록문은 0개 이상의 문을 중괄호로 묶은 것으로, 코드 블록 또는 블록이라고 부르기도 한다.
- 블록문은 단독으로 사용할 수 있으나, 제어문이나 함수를 정의할 때 사용하는 것이 일반적이다.

## 8.2 조건문
- 조건문은 주어진 조건식(불리언 값으로 평가될 수 있는 표현식)의 평가 결과에 따라 코드 블록의 실행을 결정한다.
    ### 8.2.1 if...else 문
    - if...else 문은 주어진 조건식의 평가결과, 즉 논리적 참 또는 거짓에 따라 실행할 코드 블록을 결정한다.
        ```
        if (조건식1) {
            // 조건식1이 참이면 이 코드 블록이 실행된다.
        } else if (조건식2) {
            // 조건식2가 참이면 이 코드 블록이 실행된다.
        } else {
            // 조건식1과 조건식2가 모두 거짓이면 이 코드 블록이 실행된다.
        }
        ```
        ```
        var num = 2;
        var kind;

        // if...else if 문
        if (num > 0) {
            kind = '양수';
        } else if (num < 0) {
            kind = '음수';
        } else {
            kind = '영';
        }
        
        // 코드 블록 내의 문이 하나뿐이면 중괄호 생략 가능
        if (num > 0)      kind = '양수';
        else if (num < 0) kind = '음수';
        else              kind = '영';

        // 삼항 조건 연산자
        kind = num = 0 ? (num > 0 ? '양수' : '음수') : '영';
        ```

    ### 8.2.2 switch 문
    - switch 문은 주어진 표현식을 평가하여 그 값과 일치하는 표현식을 갖는 case 문으로 실행 흐름을 옮긴다.
    - if...else 문은 논리적 참, 거짓으로 실행할 코드 블록을 결정하지만, switch문은 논리적 참, 거짓보다는 다양한 상황(case)에 따라 실행할 코드 블록을 결정할 때 사용한다.
        ```
        switch (표현식) {
            case 표현식1: 
                // switch 문의 표현식과 표현식1이 일치하면 실행될 문;
                break;
            case 표현식2:
                // switch 문의 표현식과 표현식2가 일치하면 실행될 문;
                break;
            default:
                // switch 문의 표현식과 일치하는 case 문이 없을 떄 실행될 문;
        }
        ```
        ```
        var score = 83;
        var grade;

        switch (score / 10) {
            case 10: 
            case 9:
                grade = 'A';
                break;
            case 8:
                grade = 'B';
                break;
            case 7: 
                grade = 'C';
                break;
            case 6:
                grade = 'D';
                break;
            default: 
                grade = 'F';
        }

        console.log(grade); // B
        ```
    - 만약 break를 사용하지 않는다면 switch 문을 탈출하지 않고 switch 문이 끝날 때까지 모든 case문과 default문을 실행하여 마지막 default에 있는 'F'가 재할당 되어 출력된다. 이것을 폴스루 라고 한다.
    - 여러개의 case 문을 하나의 조건으로 사용하고 싶은 경우 break문을 생략한 폴스루가 유용할 떄도 있다. 

## 8.3 반복문
- 반복문은 조건식의 평가 결과가 참인 경우 코드 블록을 실행한다. 그 후 조건식을 다시 평가하여 여전히 참인 경우 코드 블록을 다시 실행한다. 이는 조건식이 거짓일 때까지 반복된다.

    ### 8.3.1 for 문
    - for 문은 조건식이 거짓으로 평가될 때까지 코드 블록을 반복 실행한다.
        ```
        for (변수 선언문 또는 할당문; 조건식; 증감식) {
            // 조건식이 참인 경우 반복 실행될 문;
        }
        ```
        ```
        for (var i = 0; i < 2; i++) {
            console.log(i);
        }
        // 0 1

        // 선언문, 조건식, 증감식에 어떤 식도 선언하지 않으면 무한루프가 된다.
        for (;;) { ... }
        ```

    ### 8.3.2 while 문
    - while 문은 주어진 조건식의 평가 결과가 참이면 코드 블록을 계속해서 반복 실행한다.
        ```
        var count = 0;

        while (count < 3) {
            console.log(count);
            count++:
        }
        // 0 1 2

        count = 0;

        // 무한루프
        while (true) {
            console.log(count);
            count++;
            // count가 3이면 코드 블록 탈출
            if (count === 3) break;
        }
        // 0 1 2
        ```
    
    ### 8.3.3 do...while 문
    - do...while 문은 코드 블록을 먼저 실행하고 조건식을 평가한다. 따라서 코드 블록은 무조건 한 번 이상 실행된다.
        ```
        var count = 0;

        do {
            console.log(count); 
            count++;
        } while (count < 3);
        // 0 1 2
        ```

## 8.4 break 문
- break 문은 레이블 문, 반복문(for, for...in, for...of, while, do...while) 또는 switch 문의 코드 블록을 탈출할 때 사용한다. 
- 레이블 문, 반복문, switch 문의 코드블록 외에 break 문을 사용하면 SyntaxError(문법에러)가 발생한다.
- 레이블 문
    - 식별자가 붙은 문
    - 프로그램의 실행 순서를 제어하는 데 사용한다.
    - switch 문의 case 문과 default 문도 레이블 문이다.
    ```
    // foo라는 식별자가 붙은 레이블 블록문
    foo: {
        console.log(1);
        break foo; // foo 레이블 블록문을 탈출한다.
        console.log(2);
    }
    console.log('Done!');

    // 1 Done!
    ```

## 8.5 continue 문
- continue 문은 반복문의 코드 블록 실행을 현 지점에서 중단하고 반복문의 증감식으로 실행 흐름을 이동시킨다.
    ```
    var string = 'Hello World.';
    var search = 'l';
    var count = 0;

    // continue 문을 사용하면 if 문 밖에 코드를 작성할 수 있다.
    for (var i=0; i<string.length; i++) {
        if (string[i] !== search) continue;

        count++;
        // code
        // code
    }

    count = 0;
    // continue 문을 사용하지 않으면 if 문 내에 코드를 작성해야 한다.
    for (var i=0; i<string.length; i++) {
        if (string[i] === search) {
            count++;
            // code
            // code
        }
    }
    ```