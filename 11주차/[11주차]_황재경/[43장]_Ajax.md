# 📒43장_ Ajax
### 📑목차
- [Ajax(Asynchronous JavaScript and XML)](#ajaxasynchronous-javascript-and-xml)
- [JSON(JavaScript Object Notation)](#jsonjavascript-object-notation)
- [XMLHttpRequest](#xmlhttprequest)

### ⚡Quick Summary
- *Ajax*
    > **비동기 방식**으로 데이터 송수신 & 동적으로 웹페이지를 갱신하는 프로그래밍 방식
    - 전통적인 웹페이지 리렌더링 패러다임 전환
    - 변경에 필요한 데이터만 수신 & 렌더링 ⇒ 부드러운 화면 전환
- *JSON*
    > 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷
    - 이름에 자바스크립트가 들어 있지만 독립형 데이터 포맷으로 대부분의 프로그래밍 언어에서 사용 가능
    - 객체/배열 ↔ JSON 문자열
        - `JSON.stringify` 메서드
            - 객체/배열 → JSON 문자열
            - 직렬화(serialize)
        - `JSON.parse` 메서드
            - JSON 문자열 → 객체/배열
            - 역직렬화(deserialize)
- *XMLHttpRequest*
    > 자바스크립트를 사용해 HTTP 요청을 전송할 때 사용하는 Web API 객체로 브라우저에서만 동작

## 📌Ajax(Asynchronous JavaScript and XML)
> 자바스크립트를 사용하여 브라우저가 서버에게 비동기 방식으로 데이터를 요청하고, 서버가 응답한 데이터를 수신하여 웹페이지를 동적으로 갱신하는 프로그래밍 방식
- 브라우저에서 제공하는 Web API인 `XMLHttpRequest` 객체를 기반으로 동작
    - `XMLHttpRequest`는 HTTP 비동기 통신을 위한 메서드와 프로퍼티를 제공
    - 1999년 마이크로소프트가 개발
- 이전 웹페이지는 완전한 HTML을 서버로부터 전송받아 웹페이지를 리렌더링
    - 이전 웹페이지와 차이가 없어서 변경할 필요가 없는 부분까지 포함된 완전한 HTML을 서버로부터 매번 다시 전송받기 때문에 불필요한 데이터 통신 발생
    - 변경할 필요가 없는 부분까지 처음부터 다시 렌더링하기 때문에 화면 전환이 일어나면 화면이 순간적으로 깜빡이는 현상 발생
    - 클라이언트와 서버의 통신이 동기 방식으로 동작하기 때문에 서버로부터 응답이 있을 때까지 다음 처리는 블로킹됨
    ![image](https://github.com/namu56/modern-javascript-study/assets/71831926/da03cae2-2310-47b2-8e6a-8c276f6c7f08)
- Ajax의 등장으로 이전의 전통적인 패러다임 전환
    - 변경에 필요한 데이터만 서버로부터 전송받기 때문에 불필요한 데이터 통신이 발생하지 않음
    - 변경할 필요가 없는 부분은 다시 렌더링하지 않기 때문에 화면이 깜빡이는 현상 발생 X (부드러운 화면 전환)
    - 클라이언트와 서버와의 통신이 비동기 방식으로 동작하기 때문에 서버에게 요청을 보낸 이후 블로킹 발생 X
    ![image](https://github.com/namu56/modern-javascript-study/assets/71831926/672d0407-e736-4255-b232-51b8431d8440)


## 📌JSON(JavaScript Object Notation)
> 클라이언트와 서버 간의 HTTP 통신을 위한 텍스트 데이터 포맷
- 자바스크립트에 종속되지 않는 독립형 데이터 포맷으로, 대부분의 프로그래밍 언어에서 사용 가능

### JSON 표기 방식
- 자바스크립트의 객체 리터럴과 유사하게 키와 값으로 구성된 순수한 텍스트
- ⭐JSON의 키는 반드시 큰따옴표(작은따옴표 사용 불가)로 묶어야 함! 문자열도 마찬가지!
    ```json
    {
        "name": "Hwang",
        "age": 20,
        "alive": true,
        "hobby": ["basketball", "tennis"]
    }
    ```

### JSON.stringify
- ✔️클라이언트가 서버로 객체를 전송하려면 객체를 문자열화하는 **직렬화**(serialize)를 해야 함!
- `JSON.stringify` 메서드는 **객체/배열**을 **JSON 포맷의 문자열**로 변환
    ```js
    JSON.stringify(value[, replacer[, space]])
    ```
    - `value`: JSON 문자열로 변환할 값
    - `replacer`
        - 문자열화 동작 방식을 변경하는 함수, 혹은 JSON 문자열에 포함될 값 객체의 속성들을 선택하기 위한 화이트리스트(whitelist)로 쓰이는 String과 Number 객체들의 배열
        - 이 값이 `null` 이거나 제공되지 않으면, 객체의 모든 속성들이 JSON 문자열 결과에 포함됨
    - `space`
        - 가독성을 목적으로 JSON 문자열 출력에 공백을 삽입하는데 사용되는 String 또는 Number 객체
        - Number 객체인 경우
            - 공백으로 사용되는 스페이스(space) 개수
            - 1보다 작으면 스페이스 사용 X, 10보다 크면 10으로 제한
        - String 객체인 경우
            - 문자열(길이가 10보다 길면 처음 10개 문자로 제한)이 공백으로 사용됨
        - 이 값이 `null` 이거나 제공되지 않으면, 공백 사용 X

### JSON.parse
- ✔️서버로부터 클라이언트에게 전송된 JSON 데이터는 문자열이므로 객체화하는 **역직렬화**(deserializing)를 해야 함!
- `JSON.stringify` 메서드는 JSON 포맷의 문자열을 객체/배열로 변환 (`JSON.stringify` 메서드의 역연산)
    ```js
    JSON.parse(text[, reviver])
    ```
    - `text`: JSON으로 변환할 문자열
    - `reviver`: 함수라면 변환 결과를 반환하기 전에 이 인수에 전달해 변형

## 📌XMLHttpRequest
> 자바스크립트를 사용해 HTTP 요청을 전송할 때 사용하는 Web API 객체
### 객체 생성
- 생성자 함수 호출
- ✔️XMLHttpRequest 객체는 브라우저에서 제공하는 Web API이므로 브라우저 환경에서만 정상적으로 실행됨
```js
const xhr = new XMLHttpRequest();
```

### [객체의 프로퍼티와 메서드](https://developer.mozilla.org/ko/docs/Web/API/XMLHttpRequest)
- 프로토타입 프로퍼티
    - `readyState`: HTTP 요청의 현재 상태를 나타내는 정수 (0 ~ 4)
    - `status`: HTTP 요청에 대한 HTTP 상태 코드
    - `statusText`: HTTP 요청에 대한 응답 메시지
    - `responseType`: HTTP 응답 타입
    - `response`: 응답 몸체(response body)
- 이벤트 핸들러 프로퍼티
    - `onreadystatechange`: readyState 프로퍼티 값이 변경된 경우
    - `onerror`: HTTP 요청에 에러가 발생한 경우
    - `onload`: HTTP 요청이 성공적으로 완료한 경우
- 메서드
    - `open`: HTTP 요청 초기화
    - `send`: HTTP 요청 전송
    - `abort`: 이미 전송된 HTTP 요청 중단
    - `setRequestHeader`: 특정 HTTP 요청 헤더의 값을 설정

### HTTP 요청 전송
- 클라이언트가 서버에게 요청의 종류와 목적(리소스에 대한 행위)을 알리는 **HTTP 요청 메서드**(GET, POST, PUT, PATCH, DELETE 등) 사용
```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('GET', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send();
```
- payload(request body)에 담아 보낼 데이터는 `JSON.stringify` 메서드로 직렬화한 후 전달
```js
// XMLHttpRequest 객체 생성
const xhr = new XMLHttpRequest();

// HTTP 요청 초기화
xhr.open('POST', '/users');

// HTTP 요청 헤더 설정
// 클라이언트가 서버로 전송할 데이터의 MIME 타입 지정: json
xhr.setRequestHeader('content-type', 'application/json');

// HTTP 요청 전송
xhr.send(JSON.stringify({ id: 1, content: 'HTML', completed: false }));
```

### HTTP 응답 처리
- 서버가 전송한 응답을 처리하려면 `XMLHttpRequest` 객체가 발생시키는 이벤트를 캐치해야 함
- `onreadystatechange` 이벤트 사용
    ```js
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
    xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

    // HTTP 요청 전송
    xhr.send();

    // readystatechange 이벤트는 HTTP 요청의 현재 상태를 나타내는 readyState 프로퍼티가
    // 변경될 때마다 발생한다.
    xhr.onreadystatechange = () => {
        // readyState 프로퍼티는 HTTP 요청의 현재 상태를 나타낸다.
        // readyState 프로퍼티 값이 4(XMLHttpRequest.DONE)가 아니면 서버 응답이 완료되지 상태다.
        // 만약 서버 응답이 아직 완료되지 않았다면 아무런 처리를 하지 않는다.
        if (xhr.readyState !== XMLHttpRequest.DONE) return;

        // status 프로퍼티는 응답 상태 코드를 나타낸다.
        // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
        // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
        // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
        if (xhr.status === 200) {
            console.log(JSON.parse(xhr.response));
            // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
        } else {
            console.error('Error', xhr.status, xhr.statusText);
        }
    };
    ```
- `onload` 이벤트 사용
    ```js
    // XMLHttpRequest 객체 생성
    const xhr = new XMLHttpRequest();

    // HTTP 요청 초기화
    // https://jsonplaceholder.typicode.com은 Fake REST API를 제공하는 서비스다.
    xhr.open('GET', 'https://jsonplaceholder.typicode.com/todos/1');

    // HTTP 요청 전송
    xhr.send();

    // load 이벤트는 HTTP 요청이 성공적으로 완료된 경우 발생한다.
    xhr.onload = () => {
        // status 프로퍼티는 응답 상태 코드를 나타낸다.
        // status 프로퍼티 값이 200이면 정상적으로 응답된 상태이고
        // status 프로퍼티 값이 200이 아니면 에러가 발생한 상태다.
        // 정상적으로 응답된 상태라면 response 프로퍼티에 서버의 응답 결과가 담겨 있다.
        if (xhr.status === 200) {
            console.log(JSON.parse(xhr.response));
            // {userId: 1, id: 1, title: "delectus aut autem", completed: false}
        } else {
            console.error('Error', xhr.status, xhr.statusText);
        }
    };
    ```