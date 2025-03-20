# 18.1 일급 객체

---

- 다음 조건을 만족하는 객체를 일급 객체라고 한다.

  1. 무명의 리터럴로 생성할 수 있다. 즉, 런타임에 생성이 가능하다.
  2. 변수나 자료구조에 저장할 수 있다
  3. 함수의 매개변수로 전달할 수 있다
  4. 함수의 반환값으로 사용할 수 있다.

- 자바스크립트 함수는 다음 조건을 모두 만족하므로 일급객체이다.
- 함수는 객체이지만 일반 객체와 차이가 있다. 일반 객체는 호출할 수 없지만, 함수 객체는 호출할 수 있다. 또한 일반 객체에는 없는 함수 고유의 프로퍼티를 소유한다.

# 18.2 함수 객체의 프로퍼티

---

- 함수의 모든 프로퍼티의 프로퍼티 어트리뷰트를 Object.getOwnPropertyDescriptors 메서드로 확인해보면 다음과 같다.
- arguments, caller , length, name , prototype 프로퍼티는 일반 객체에는 없는 함수 객체 고유의 프로퍼티이다.
- ❗ **proto** 는 접근자 프로퍼티이며 Object.prototype 객체의 프로퍼티를 상속받은 것임! 모든 객체가 사용하기 때문에 함수 객체 고유의 것이 아니다.

1. **arguments 프로퍼티**

   - **arguments 객체는 함수 호출 시 전달된 인수들의 정보를 담고 있는 순회 가능한 유사 배열 객체이다.**

   ```jsx
   function sum(a, b) {
     console.log(arguments); // [Arguments] { '0': 1, '1': 2 }
     return a + b;
   }

   console.log(sum(1, 2)); // 3
   ```

   - 함수 내부에서 지역 변수처럼 사용됨.
   - ES3 부터 arguments 프로퍼티는 폐지되어 Function.arguments 같은 사용법은 권장 X
   - 초과된 인수는 버려지지 않고, 암묵적으로 arguments 객체의 프로퍼티로 보관된다.
   - 👾 **arguments 객체의 Symbol 프로퍼티**
     - Symbol.iterator 는 순회 가능한 자료구조인 이터러블로 만듬.
     ```jsx
     function sum(a, b) {
       const iter = arguments[Symbol.iterator]();
       console.log(iter.next());
       console.log(iter.next());
       console.log(iter.next());
       console.log(iter.next());

       return a + b;
     }

     console.log(sum(1, 2));

     // 결과

     { value: 1, done: false }
     { value: 2, done: false }
     { value: undefined, done: true }
     { value: undefined, done: true }
     3
     ```
   - 🌟 **arguments 객체는 매개변수 개수를 확정할 수 없는 가변 인자 함수를 구현할 때 유용하다!**

   ```jsx
   function sum() {
     let result = 0;

     for (let i = 0; i < arguments.length; i++) {
       result += arguments[i];
     }

     return result;
   }

   console.log(sum()); // 0
   console.log(sum(1, 2)); // 3
   console.log(sum(15, 123)); // 138
   console.log(sum(1, 2, 3, 4, 5, 6)); // 21
   ```

   - arguments 객체는 배열 형태로 인자 정보를 담고 있지만, 실제 배열이 아닌 유사 배열 객체이다. ⚠️ **유사 객체 배열** : length 프로퍼티를 가진 객체로 for 문으로 순회할 수 있는 객체
   - ES6 부터 arguments 는 유사 배열 객체 이면서 이터러블이다.
     - ❗**이터러블이 되면서 for … of 연산자와 … 스프레드 연산자를 사용할 수 있게 됨!!**
   - 유사 배열 객체는 배열이 아니기 때문에 배열 메서드를 사용하면 에러가 발생한다. Function.prototypr.call 혹은 Function.prototypr.apply 를 사용해 간접 호출해야 한다.
   - 하지만 번거로움이 있었기에 → ES6 문법에 rest 파라미터를 도입. 나머지 인수들 배열로 만드는 연산자!

   ```jsx
   function sum(...rest) {
     return rest.reduce((sum, num) => sum + num, 0);
   }

   console.log(sum());
   console.log(sum(1, 2));
   console.log(sum(15, 123));
   console.log(sum(1, 2, 3, 4, 5, 6));
   ```

   - 이를 도입하게 되었지만 ES6 만 사용할 수 있는 게 아니니까 arguments 에 대해 알아둘 필요가 있다.

2. **caller 프로퍼티**

   - 비표준 프로퍼티이다. 자기 자신을 호출한 함수를 가르킨다.

   ```jsx
   function foo(func) {
     return func();
   }

   function bar() {
     return bar.caller;
   }

   console.log(foo(bar));
   console.log(bar());
   ```

3. **length 프로퍼티**

   - 함수 정의할 때 선언한 매개변수의 개수이다.
   - ❗arguments 의 length 는 인자의 개수 이므로 같지 않을 수 있음에 주의해야 한다.

4. **name 프로퍼티**

   - 함수 이름을 나타내는 프로퍼티
   - ES6 에서 정식 표준이 되었다.
   - **ES5 에서는 익명 함수 표현식의 경우 name 이 빈 프로퍼티가 나옴**
   - **ES6 에서는 익명 함수가 함수 객체를 가르키는 식별자가 나옴.**
   - 초반에 얘기했지만 함수 이름과 식별자는 다르다. 식별자로 호출해야 한다.

5. ****proto** 프토퍼티**

   - 모든 객체는 [[Prototype]] 이라는 내부 슬롯을 가진다. 이는 상속을 구현하는 프로토타입 객체를 가르킨다.
   - **proto** 프로퍼티는 프로토타입 객체에 접근하기 위해 사용하는 접근자 프로퍼티이다. 내부 슬롯에는 직접 접근할 수는 없고 간접적인 접근 방법을 제공하는 경우에 한해 접근할 수 있는데, [[prototype]] 내부 슬롯에 **proto** 접근자 프로퍼티로 간접적으로 프로토타입 객체에 접근할 수 있다.

   ```jsx
   const obj = { a: 1 };
   console.log(obj.__proto__);
   console.log(Object.prototype);
   console.log(Object.prototype === obj.__proto__);

   // 결과
   [Object: null prototype] {}
   [Object: null prototype] {}
   ```

   - **hasOwnProperty 메서드 :** 상속받은 거 말고 객체 고유의 프로퍼티 인 경우만 true
   - 프로토타입에 대해서는 다음장에서 깊이 다룰 예정…

6. **prototype 프로퍼티**
   - 생성자 함수로 호출할 수 있는 함수 객체, 즉 constructor 만이 소유하는 프로퍼티이다.
   - prototype 프로퍼티는 함수가 객체를 생성하는 생성자 함수로 호출될 때 생성자 함수가 생성할 인스턴스의 프로토타입 객체를 가르킨다.
