# 20장 strict mode

## 20.1 strict mode란?

```js
function foo() {
  x = 10;
}
foo();

console.log(x);
```

위 예제 코드의 실행 결과는 놀랍게도 10이 나온다.

자바스크립트 엔진이 암묵적으로 전역 객체에 `x` 프로퍼티를 동적 생성하기 때문이다. 이때 이 `x` 프로퍼티를 마치 전역 변수처럼 사용 가능한데, 이러한 현상을 **암묵적 전역(implicitly global)**이라 한다.

💡때문에 반드시 `var`, `let`, `const` 키워드를 사용하여 변수를 선언한 뒤 사용해야 한다.

위와 같은 실수를 방지하기 위해 ES5부터 도입된 것이 strict mode다.

strict mode는 자바스크립트의 문법을 좀 더 엄격히 적용하여 오류를 발생시킬 가능성이 있거나, 엔진의 최적화 작업에 문제를 일으키는 코드에 대해 명시적인 에러를 발생시킨다.

ESLint 같은 도구를 활용해서 유사한 효과를 얻을 수 있다.

> ES6 모듈(ESM을 사용하는 파일)에서는 자동으로 strict mode가 적용된다.

## 20.2 strict mode의 적용

strict mode는 코드 최상단에 선언하면 스크립트 전체에 적용되며, 함수 내부에 추가하면 해당 함수와 중첩 함수에 적용된다.

반드시 코드의 선두에 위치시켜야만 그 아래 코드에 적용된다.

```js
"use strict";

function foo() {
  x = 10;
}
foo();

console.log(x); // ReferenceError: x is not defined
```

## 20.3 전역에 strict mode를 적용하는 것은 피하자

strict mode는 스크립트 단위로 적용된다.

strict mode 스크립트와 non-strict mode 스크립트를 혼용하는 것을 오류를 발생시킬 수 있다. 특히 외부 서드파티 라이브러리를 사용하는 경우, 라이브러리가 non-strict mode인 경우도 있기 때문에 전역에 strict mode를 적용하면 안 된다.

이런 경우 즉시 실행 함수로 스크립트 전체를 감싸 스코프를 구분하고 즉시 실행 함수의 선두에 strict mode를 적용한다.

```js
(function () {
  "use strict";
})();
```

## 20.4 함수 단위로 strict mode를 적용하는 것도 피하자

어떤 함수는 적용하고, 어떤 함수는 적용하지 않는 것은 바람직하지 않다. 그렇다고 모든 함수에 일일이 적용하는 것도 번거로울 것이다.

그리고 strict mode가 적용된 함수가 참조할 함수 외부 컨텍스트에 strict mode를 적용하지 않으면 이 또한 문제가 된다.

```js
(function () {
  var let = 10; // 에러가 발생하지 않는다.
  function foo() {
    "use strict";
    let = 20; // SyntaxError: Unexpected strict mode reserved word
  }
  foo();
})();
```

💡따라서 즉시 실행 함수로 감싼 스크립트 단위로 적용하는 것이 바람직하다.

## 20.5 strict mode가 발생시키는 에러

### 1. 암묵적 전역

선언하지 않은 변수를 참조하면 참조 에러를 발생시킨다.

### 2. 변수, 함수, 매개변수의 삭제

`delete` 연산자로 변수, 함수, 매개변수를 삭제하면 문법 에러를 발생시킨다.

### 3. 매개변수 이름의 중복

매개변수 이름을 중복해서 사용하면 문법 에러를 발생시킨다.

```js
(function () {
  "use strict";

  //SyntaxError: Duplicate parameter name not allowed in this context
  function foo(x, x) {
    return x + x;
  }
  console.log(foo(1, 2));
})();
```

### 4. with 문의 사용

`with` 문을 사용하면 문법 에러를 발생시킨다.

`with` 문은 전달된 객체를 스코프 체인에 추가한다. 동일한 객체의 프로퍼티를 반복해서 사용할 때 객체 이름을 생략할 수 있어서 코드가 간단해지지만 성능과 가독성이 나빠진다. `with` 문은 사용하지 않는 것이 좋다.

```js
(function () {
  "use strict";

  // SyntaxError: Strict mode code may not include a with statement
  with ({ x: 10 }) {
    console.log(x);
  }
})();
```

## 20.6 strict mode 적용에 의한 변화

### 1. 일반 함수의 this

strict mode에서 함수를 일반 함수로서 호출하면 `this`에 `undefined`가 바인딩된다. 일반 함수에서는 `this`를 사용할 필요가 없기 때문이다.

### 2. arguments 객체

strict mode에서 매개변수에 전달된 인수를 재할당하여 변경해도 `arguments` 객체에 반영되지 않는다.

```js
(function (a) {
  "use strict";
  // 매개변수에 전달된 인수를 재할당하여 변경
  a = 2;

  // 변경된 인수가 arguments 객체에 반영되지 않는다.
  console.log(arguments); // { 0: 1, length: 1 }
})(1);
```
