# 20.1 strict mode란?

---

```jsx
function foo() {
  x = 10;
}
foo();

console.log(x); // 놀랍게도 10이 나온다.
```

- foo 함수 내에서 선언하지 않은 x 변수에 값 10을 할당한다. → foo 함수 스코프에서 검색하지만 선언이 없으므로 검색에 실패하고 상위 스코프에서 x 변수 선언을 검색한다. 전역에도 없기 때문에 ReferenceError 가 발생할 거 같지만, 자바스크립트 엔진이 암묵적으로 전역 객체에 x 프로퍼티를 동적 생성하여 전역 객체의 x 프로퍼티를 전역 변수 처럼 사용할 수 있게 된다. → 이런 현상을 **암묵적 전역**이라 한다.
- ❗당연히 암묵적 전역을 오류를 발생시키는 원인이 될 가능성이 크기 때문에 var , let, const 키워드를 사용해야 한다.
- but 언제나 실수는 발생한다.
- ❗이를 지원하기 위해 ES5 부터 strict mode 가 추가되었다. strict mode 는 자바스크립트 언어의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 높거나 엔진 최적화 작업에 문제 일으킬 수 있는 코드에 대해 명시적으로 에러를 발생시킨다.
- ESLint 와 같은 도구도 유사한 효과를 낼 수 있다. **🌟 린트 도구는 strict mode 가 제한하는 오류는 물론 코딩 컨벤션을 설정 파일 형태로 정의하고 강제할 수 있기 때문에 더욱 강력한 효과를 얻을 수 있다.**
- 참고로 ES6 에서 도입된 클래스와 모듈은 기본적으로 strict mode 가 적용된다.

# 20.2 strict mode 의 적용

---

- strict mode를 적용하려면 전역의 선두 또는 함수 몸체의 선두에 ‘use strict’;을 추가한다. 전역의 선두에 추가하면 스크립트 전체에 strict mode 가 적용된다.

```jsx
"use strict";

function foo() {
  x = 10;
}
foo();

console.log(x); // ReferenceError: x is not defined
```

- 함수 몸체의 선두에 추가하면 해당 함수와 중첩 함수에 strict mode가 적용된다.

```jsx
function foo() {
  "use strict";
  x = 10;
}
foo();

console.log(x); // referenceError

function foo() {
  x = 10;
  ("use strict");
}
foo();

console.log(x); // 10 선두에 위치하지 않으면 제대로 동작하지 않는다.
```

# 20.3 전역에 strict mode를 적용하는 것은 피하자

---

- 전역에 적용한 strict mode 는 스크립트 단위로 적용된다.

![image.png](attachment:c8929e14-a53a-4b26-826c-bde21cb6c6a1:image.png)

![image.png](attachment:39687094-910e-4a25-9f34-edd1ddf205fc:image.png)

- 스크립트 단위로 적용된 strict mode 는 다른 스크립트에 영향을 주지 않고 해당 스크립트에 한정되어 적용된다.
- 하지만 strict mode 스크립트와 no-strict mode 스크립트를 혼용하는 것은 오류를 발생시킬 수 있다. ❗특히, 외부 서드파티 라이브러리를 사용하는 경우, 라이브러리가 non-strict mode 인 경우도 있기 때문에 전역에 적용하는 것은 바람직하지 않다.
- 이런 경우 즉시 실행 함수로 스크립트 전체를 감싸 스코프를 구분하고 즉시 실행 함수 선두에 strict mode 적용하기

![image.png](attachment:104b8fcc-3bb8-4752-9c87-d7dd21e3c5ca:image.png)

# 20.4 함수 단위로 strict mode를 적용하는 것도 피하자.

---

- 함수 단위로도 strict mode 를 적용할 수는 있다. 어떤 함수는 적용하고 어떤 함수는 적용하지 않는 것은 바람직하지 않으며, 모든 함수에 일일이 적용하는 것 또한 번거로운 일이다.
- ❗strict mode 가 적용된 함수가 참조할 함수 외부의 컨텍스트에 strict mode를 적용하지 않는다면 이 또한 문제가 발생할 수 있기 때문에 strict mode 는 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

```jsx
(function () {
  var let = 10; // 에러가 발생하지 않는다.

  function foo() {
    "use strict";

    let = 20; //SyntaxError
  }

  foo();
  console.log(let);
})();
```

# 20.5 strict mode 가 발생시키는 에러

---

1. **암묵적 전역**

   - 선언하지 않은 변수를 참조하면 referenceError가 발생한다.

   ```jsx
   (function () {
     "use strict";

     x = 1;
     console.log(x); // referenceErorr
   })();
   ```

1. **변수, 함수 매개변수의 삭제**

   - delete 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 가 발생한다.

   ```
   (function () {
     "use strict";

     var x = 1;
     delete x; // SyntaxError: Delete of an unqualified identifier in strict mode.

     function foo(a) {
       delete a; // SyntaxError: Delete of an unqualified identifier in strict mode.
     }
     delete foo; // SyntaxError: Delete of an unqualified identifier in strict mode.
   })();

   ```

1. **매개변수 이름의 중복**

   - 중복된 매개변수 이름을 사용하면 SyntaxError 발생

   ```jsx
   (function () {
     "use strict";

     // SyntaxError: Duplicate parameter name not allowed in this context
     function foo(x, x) {
       return x * x;
     }
     console.log(foo(1, 2));
   })();
   ```

1. **with 문의 사용**

   - with 문은 전달된 객체를 스코프 체인에 추가한다. 동일한 객체 프로퍼티를 반복해서 사용할 때 코드가 간단해지는 효과가 있지만, 가독성과 성능이 나빠진다고 한다. → 사용하지 않는 것이 좋다.

   ```jsx
   (function () {
     "use strict";

     with ({ x: 1 }) {
       // SyntaxError: Strict mode code may not include a with statement
       console.log(x);
     }
   })();
   ```

# 20.6 strict mode 적용에 의한 변화

---

1. **일반 함수의 this**

   - strict mode 에서 함수를 일반 함수로서 호출하면 this 에 undefined 가 바인딩된다. 생성자 함수가 아닌 일반 함수 내부에서는 this 를 사용할 필요가 없기 때문이다. 이때 에러는 발생하지 않는다.

   ```jsx
   (function () {
     "use strict";

     function foo() {
       console.log(this); // undefined
     }
     foo();

     function Foo() {
       console.log(this); // Foo
     }
     new Foo();
   })();
   ```

2. **arguments 객체**

   - strict mode 에서는 매개변수에 전달된 인수를 재할당하여 변경해도 arguments 객체에 반영되지 않는다.

   ```jsx
   (function (a) {
     "use strict";

     a = 2;
     console.log(arguments); // { '0': 1 }
   })(1);
   ```
