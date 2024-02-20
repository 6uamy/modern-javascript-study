# 📒22장_ this
### 📑목차
- [`this` 키워드](#this-키워드)
- [함수 호출 방식과 `this` 바인딩](#함수-호출-방식과-this-바인딩)

### ⚡Quick Summary
- `this` 키워드
    > 자바스크립트 엔진에 의해 암묵적으로 생성되는 자기 참조 변수
- `this` 바인딩은 함수 호출 시점에 결정됨
    |함수 호출 방식|`this` 바인딩|
    |--|--|
    |일반 함수 호출|전역 객체(global object)|
    |메서드 호출|메서드를 호출한 객체|
    |생성자 함수 호출|생성자 함수가 (미래에) 생성할 인스턴스|
    |`Function.prototype.apply/call/bind` 메서드에 의한 간접 호출|메서드에 첫 번째 인수로 전달한 객체|

## 📌`this` 키워드
> 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 ***자기 참조 변수(self-referencing variable)***
- 자바스크립트 엔진에 의해 암묵적으로 생성됨
- 코드 어디서든 참조 가능
- 함수를 호출하면 `arguments` 객체와 `this`가 암묵적으로 함수 내부에 전달됨
- ⭐`this`가 가리키는 값(this binding)은 함수 호출 방식에 의해 동적으로 결정됨

## 📌함수 호출 방식과 `this` 바인딩
- 함수 호출 방식
    1. [일반 함수 호출](#일반-함수-호출)
    2. [메서드 호출](#메서드-호출)
    3. [생성자 함수 호출](#생성자-함수-호출)
    4. [`Function.prototype.apply/call/bind` 메서드에 의한 간접 호출](#functionprototypeapplycallbind-메서드에-의한-간접-호출)

### 일반 함수 호출
> 기본적으로 `this`에 전역 객체(global object)가 바인딩됨
- 객체를 생성하지 않는 일반 함수에서 `this`는 무의미
- strict mode가 적용된 일반 함수 내부의 `this`에는 `undefined`가 바인딩됨
- ⚠️메서드 내부의 중첩 함수나 콜백 함수의 `this`가 전역 객체를 바인딩하는 것은 문제이기 때문에 메서드의 `this` 바인딩과 일치시켜야 함
    - 메서드의 `this`를 변수에 할당하고 해당 변수를 사용
    - `Function.prototype.apply/call/bind` 메서드를 통해 `this`를 명시적으로 바인딩
    - **화살표 함수** 내부의 `this`가 **상위 스코프**의 `this`를 가리키는 것을 활용
### 메서드 호출
> 메서드 내부의 `this`에는 **메서드를 호출한 객체**, 즉 메서드를 호출할 때 메서드 이름 앞의 마침표(.) 연산자 앞에 기술한 객체가 바인딩됨
- `this`에 바인딩될 객체는 **호출 시점에 결정**됨!
    - 메서드를 소유한 객체가 아닌, 메서드를 **호출**한 객체에 바인딩됨
    - 메서드는 함수 객체, 즉 *참조값*을 가리키고 있을 뿐이므로 메서드를 가리키고 있는 객체와는 상관이 없고, 메서드를 **호출**한 객체에 바인딩됨
### 생성자 함수 호출
> 생성자 함수 내부의 `this`에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩됨
- 생성자 함수는 이름 그대로 객체(인스턴스)를 생성하는 함수
- `new` 연산자와 함께 호출하지 않으면 그냥 일반 함수로 동작
```jsx
// 생성자 함수
function Circle(radius) {
  // 생성자 함수 내부의 this는 생성자 함수가 생성할 인스턴스를 가리킴
  this.radius = radius;
  this.getDiameter = function () {
    return 2 * this.radius;
  };
}

// 반지름이 5인 Circle 객체를 생성
const circle1 = new Circle(5);
// 반지름이 10인 Circle 객체를 생성
const circle2 = new Circle(10);

console.log(circle1.getDiameter()); // 10
console.log(circle2.getDiameter()); // 20

const circle3 = Circle(15);
console.log(circle3); // undefined
// 일반 함수로 호출된 Circle 내부의 this는 전역 객체를 가리킴
console.log(radius); // 15
```
### `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출
- `Function.prototype`의 메서드이므로 모든 함수가 상속받아 사용 가능
- `Function.prototype.apply/call`
    ```jsx
    /**
     * 주어진 this 바인딩과 인수 리스트 배열을 사용하여 함수를 호출
     * @params thisArg - this로 사용할 객체
     * @parmas argsArray - 함수에게 전달할 인수 리스트의 배열 또는 유사 배열 객체
     * @returns 호출된 함수의 반환값 
     */
    Function.prototype.apply(thisArg[, argsArray])

    /**
     * 주어진 this 바인딩과 ,로 구분된 인수 리스트를 사용하여 함수를 호출
     * @params thisArg - this로 사용할 객체
     * @parmas arg1, arg2, … - 함수에게 전달할 인수 리스트
     * @returns 호출된 함수의 반환값 
     */
    Function.prototype.call(thisArg[, arg1[, arg2[, …]]]);
    ```
    - 즉, 호출할 함수에 인수를 전달하는 방식만 다를 뿐 동일하게 동작
        ```jsx
        function getThisBinding() {
            console.log(arguments);
            return this;
        }

        // this로 사용할 객체
        const thisArg = { a: 1 };

        console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
        // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
        // {a: 1}

        console.log(getThisBinding.call(thisArg, 1, 2, 3));
        // Arguments(3) [1, 2, 3, callee: ƒ, Symbol(Symbol.iterator): ƒ]
        // {a: 1}
        ```
    - ✔️보통 `arguments` 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우에 `apply`와 `call` 메서드를 이용
        ```jsx
        function convertArgsToArray() {
            console.log(arguments);

            // arguments 객체를 배열로 변환
            // Array.prototype.slice를 인수없이 호출하면 배열의 복사본을 생성
            const arr = Array.prototype.slice.call(arguments);
            // const arr = Array.prototype.slice.apply(arguments);
            console.log(arr);

            return arr;
        }

        convertArgsToArray(1, 2, 3); // [1, 2, 3]
        ```
- `Function.prototype.bind`
    - 함수를 호출하지는 않고 첫 번째 인수로 전달한 값으로 `this` 바인딩이 교체된 함수를 새롭게 생성해 반환
        ```jsx
        function getThisBinding() {
            return this;
        }

        // this로 사용할 객체
        const thisArg = { a: 1 };

        // bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 함
        console.log(getThisBinding.bind(thisArg)()); // {a: 1}
        ```