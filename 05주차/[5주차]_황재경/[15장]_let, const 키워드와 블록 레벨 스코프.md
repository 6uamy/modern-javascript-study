# 📒15장_ let, const 키워드와 블록 레벨 스코프
### 📑목차
- [`var` 키워드로 선언한 변수의 문제점](#var-키워드로-선언한-변수의-문제점)
- [`let`과 `const` 키워드](#let과-const-키워드)

### ⚡Quick Summary
- 변수 선언 단계 👉 [04장 > 변수 선언과 할당](../../02주차/[2주차]_황재경/[04장]_변수.md#변수-선언declaration과-할당assignment) 참고
    1. **선언 단계(Declaration phase)**
        > 변수 이름을 (실행 컨텍스트의 변수 객체에) 등록해서 자바스크립트 엔진에 **변수의 존재를 알림**
    2. **초기화 단계(Initialization phase)**
        > 선언 단계의 변수를 위한 값을 저장하기 위한 **메모리 공간을 확보**하고 **`undefined`로 초기화**
    3. **할당 단계(Assignment phase)**
        > 사용자가 변수에 다른 값 할당 (`undefined` → 특정한 값)
- `var` 변수 문제점
    - 변수 중복 선언 가능
    - 함수 레벨 스코프
    - 변수 호이스팅
        > **런타임 전에 "선언 단계"와 "초기화 단계"가 한번에**
        ```mermaid
        graph TD
            subgraph A[ ]
                A1[선언 단계]
                A2[초기화 단계]
            end

            subgraph B[런타임ㅤㅤㅤㅤㅤㅤ]
                B1[할당 단계]
            end

            A1 --> A2 --> B1
                
            style A fill:#98FB98
            style B fill:#87CEEB
        ```
- `let`과 `const` 변수 공통 특징
    - 변수 중복 선언 불가능
    - 블록 레벨 스코프
    - 변수 호이스팅
        > **런타임 전에 "선언 단계"만, "초기화 단계"는 런타임 시 실제 변수 선언문이 위치한 곳에서**
        ```mermaid
        graph TD
            subgraph A[ ]
                A1[선언 단계]
            end

            subgraph B[런타임ㅤㅤㅤㅤㅤㅤㅤ]
                B1[초기화 단계]
                B2[할당 단계]
            end

            A1 -->|⚠️Temporal Dead Zone, TDZ| B1 --> B2

            style A fill:#98FB98
            style B fill:#87CEEB
        ```
        - ***TDZ(Temporal Dead Zone)***
            > *스코프의 시작 지점 ~ 초기화 시작 지점*까지 변수를 참조할 수 없는 구간 → 이 구간에서 변수 참조 시 `ReferenceError` 발생
- `var` vs `let` vs `const`
    |keyword|var|let|const|
    |--|--|--|--|
    |global scope|⭕|✖️|✖️|
    |function scope|⭕|✖️|✖️|
    |block scope|✖️|⭕|⭕|
    |중복 선언|⭕|✖️|✖️|
    |재할당|⭕|⭕|✖️|
    
    ※ `const` 키워드는 선언과 동시에 초기화!
- ***Always `const`, Sometimes `let`, Never `var`***
## 📌`var` 키워드로 선언한 변수의 문제점
> `var` 키워드는 ES5까지 변수를 선언할 수 있었던 유일한 방법😯
- 변수 중복 선언 허용
    - 의도치 않게 먼저 선언된 변수 값이 변경될 수 있음
    ```jsx
    var x = 1;
    var y = 1;

    // var 키워드로 선언된 변수는 같은 스코프 내에서 중복 선언을 허용한다.
    // 초기화문이 있는 변수 선언문은 자바스크립트 엔진에 의해 var 키워드가 없는 것처럼 동작한다.
    var x = 100;
    // 초기화문이 없는 변수 선언문은 무시된다.
    var y;

    console.log(x); // 100
    console.log(y); // 1
    ```
- 함수 레벨 스코프
    - 코드 블록 내에서 선언해도 모두 전역 변수가 됨
    - 전역 변수를 남발할 가능성을 높여 의도치 않게 전역 변수가 중복 선언되는 경우 발생
- 변수 호이스팅
    - 변수 호이스팅에 의해 변수 선언문 이전에 참조 가능 (`undefined` 반환)
    - 프로그램의 흐름에 맞지 않음
    - 가독성을 떨어뜨리고 오류를 발생시킬 여지를 남김

## 📌`let`과 `const` 키워드
> `var` 키워드의 단점을 보완하기 위해 ES6에서 도입된 새로운 키워드
- 변수 중복 선언 금지
    - 중복 선언 시 `SyntaxError` 발생
    ```jsx
    let bar = 123;
    // 중복 선언 불가능
    let bar = 456;  // SyntaxError: Identifier 'bar' has already been declared
    ```
    ```jsx
    const foo;  // SyntaxError: Missing initializer in const declaration
    const foo = 1;  // ✔️const는 반드시 선언과 동시에 초기화
    // 중복 선언 불가능
    const foo = 2;  // SyntaxError: Identifier 'foo' has already been declared
    // ⭐const는 재할당도 불가능!
    foo = 3;    // TypeError: Assignment to constant variable.
    ```
- 블록 레벨 스코프
    - 모든 코드 블록(함수, if 문, for 문, while 문, try/catch 문 등)을 지역 스코프로 인정
    ```jsx
    // 전역 변수
    let foo = 1;
    const x = 0;

    {
        // 지역 변수
        let foo = 2;
        let bar = 3;
        const x = -1;
        const y = -2;
    }

    console.log(foo);   // 1
    console.log(bar);   // ReferenceError: bar is not defined

    console.log(x);     // 0
    console.log(y);     // ReferenceError: y is not defined
    ```
- 변수 호이스팅이 발생하지 않는 것처럼 동작
    - `var` 키워드는 런타임 전에 "선언 단계"와 "초기화 단계"가 한번에 진행됨
        ```jsx
        console.log(foo); // undefined
        var foo;
        console.log(foo); // undefined

        foo = 1; // 할당 단계 실행
        console.log(foo); // 1
        ```
    - `let`과 `const` 키워드는 "선언 단계"와 "초기화 단계"가 분리되어 진행
        - ⭐런타임 전에 "선언 단계"가 먼저 실행되고 "초기화 단계"는 변수 선언문에 도달했을 때 실행됨
        - "초기화 단계"가 실행되기 이전에 변수에 접근하려고 하면 `ReferenceError` 발생
        - ⭐스코프의 시작 지점부터 초기화 시작 지점까지 변수를 참조할 수 없는 구간을 ***일시적 사각지대(TDZ, Temporal Dead Zone)*** 라고 부름
        ```jsx
        console.log(foo); // ReferenceError: Cannot access 'foo' before initialization

        let foo; // 초기화 단계 실행
        console.log(foo); // undefined

        foo = 1; // 할당 단계 실행
        console.log(foo); // 1
        ```
        ```jsx
        let foo = 1; // 전역 변수

        {
            console.log(foo); // ReferenceError: Cannot access 'foo' before initialization
            let foo = 2; // 지역 변수
        }
        ```
        ```jsx
        console.log(bar); // ReferenceError: Cannot access 'bar' before initialization
        const bar = 1; // 초기화 단계 & 할당 단계 실행
        console.log(bar); // 1
        ```
        ```jsx
        const bar = 1; // 전역 변수

        {
            console.log(bar); // ReferenceError: Cannot access 'bar' before initialization
            const bar = 2; // 지역 변수
        }
        ```
- `let` 키워드로 선언된 전역 변수는 전역 객체의 프로퍼티가 아님
    - ✔️`var` 키워드로 선언된 전역 변수 또는 선언하지 않은 변수에 값을 할당한 *암묵적 전역*의 경우 전역 객체 `window`의 프로퍼티가 됨 (참조 시 `window` 생략 가능)
    ```jsx
    // 이 예제는 브라우저 환경에서 실행해야 한다.

    // 전역 변수
    var x = 1;
    // 암묵적 전역
    y = 2;
    // 전역 함수
    function foo() {}

    // var 키워드로 선언한 전역 변수는 전역 객체 window의 프로퍼티
    console.log(window.x); // 1
    // window의 프로퍼티는 전역 변수처럼 사용 가능
    console.log(x); // 1

    // 암묵적 전역은 window의 프로퍼티
    console.log(window.y); // 2
    console.log(y); // 2

    // 함수 선언문으로 정의한 전역 함수는 window의 프로퍼티
    console.log(window.foo); // ƒ foo() {}
    // window의 프로퍼티는 전역 변수처럼 사용 가능
    console.log(foo); // ƒ foo() {}

    let x = 1;
    const y = 2;

    // let, const 키워드로 선언한 전역 변수는 window의 프로퍼티 ❌
    console.log(window.x); // undefined
    console.log(x); // 1
    console.log(window.y); // undefined
    console.log(y); // 2
    ```

- `const` 키워드로 상수를 선언할 때에는 ***🅰️대문자 & 🐍snake case***로 이름 짓기
    ```jsx
    const TAX_RATE = 0.1;
    const PI = 3.141592;
    ```