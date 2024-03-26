# 📒46장_ 제너레이터와 async/await
### 📑목차
- [async/await](#asyncawait)
- [에러 처리](#에러-처리)

### ⚡Quick Summary
- async/await
    > Promise를 사용하고 있는 비동기 함수를 동기 함수처럼 사용할 수 있게 해주는 도구
- async 함수는 항상 Promise 반환
- `await` 키워드는 async 함수 내부에서, 프로미스 앞에서, 사용
- 독립적인 비동기 처리는 정적 메서드 `Promise.all` 사용
- async 함수에서 `catch` 문으로 에러 처리를 하지 않으면 `reject(error)` 프로미스 반환

## 📌async/await
- 프로미스 기반으로 동작
- 후속 처리 메서드를 사용할 필요 없이 마치 동기 처리처럼 프로미스를 사용할 수 있음

### async 함수
- ⚠️`await` 키워드는 반드시 async 함수 내부에서 사용
- async 함수는 `async` 키워드로 정의하며 **언제나 프로미스를 반환**
- async 함수가 명시적으로 프로미스를 반환하지 않더라도 async 함수는 암묵적으로 반환값을 resolve하는 프로미스를 반환
    - 클래스의 constructor 메서드는 인스턴스를 반환해야 하지만 async 함수는 언제나 프로미스를 반환해야 하기 때문에 클래스의 constructor 메서드에는 async 사용 불가능

### await 키워드
- 프로미스가 settled 상태(비동기 처리가 수행된 상태)가 될 때까지 대기하다가 settled 상태가 되면 프로미스가 resolve한 처리 결과를 반환
- ✔️`await` 키워드는 반드시 프로미스 앞에서 사용
- ⭐**서로 연관이 없이 개별적으로 수행되는 비동기 처리의 경우**에는 정적 메서드 `Promise.all`로 병렬 처리
    ```js
    async function foo() {
    const res = await Promise.all([
        new Promise(resolve => setTimeout(() => resolve(1), 3000)),
        new Promise(resolve => setTimeout(() => resolve(2), 2000)),
        new Promise(resolve => setTimeout(() => resolve(3), 1000))
    ]);

    console.log(res); // [1, 2, 3]
    }

    foo(); // 약 3초 소요된다.
    ```
- ⭐**순서가 보장되어야 하는 경우**에는 `await` 키워드를 써서 순차적으로 처리
    ```js
    async function bar(n) {
    const a = await new Promise(resolve => setTimeout(() => resolve(n), 3000));
    // 두 번째 비동기 처리를 수행하려면 첫 번째 비동기 처리 결과가 필요하다.
    const b = await new Promise(resolve => setTimeout(() => resolve(a + 1), 2000));
    // 세 번째 비동기 처리를 수행하려면 두 번째 비동기 처리 결과가 필요하다.
    const c = await new Promise(resolve => setTimeout(() => resolve(b + 1), 1000));

    console.log([a, b, c]); // [1, 2, 3]
    }

    bar(1); // 약 6초 소요된다.
    ```

## 📌에러 처리
- `try … catch` 문 사용 가능
- ✔️`try` 코드 블록 내의 모든 문에서 발생한 일번적인 에러까지 모두 캐치 가능!
- ⭐async 함수 내에서 catch 문을 사용해서 에러 처리를 하지 않으면 async 함수는 발생한 에러를 reject하는 프로미스를 반환
    ```js
    const fetch = require('node-fetch');

    const foo = async () => {
    const wrongUrl = 'https://wrong.url';

    const response = await fetch(wrongUrl);
    const data = await response.json();
    return data;
    };

    foo()
    .then(console.log)
    .catch(console.error); // TypeError: Failed to fetch
    ```