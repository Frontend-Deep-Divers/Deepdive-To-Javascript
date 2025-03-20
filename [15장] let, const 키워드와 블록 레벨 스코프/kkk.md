## 15장 let, const 키워드와 블록 레벨 스코프

### 15.1 var 키워드의 문제점

### 변수 중복 선언 허용

`var`는 같은 이름의 변수를 여러 번 선언할 수 있어, 이전 값이 덮어써진다.
이로 인해 변경 추적이 어려워지고, 사이드 이펙트가 발생할 가능성이 높아진다.

```jsx
var x = 10;
var x = 20; // 문제 없이 덮어써짐 ❌
console.log(x); // 20
```

### 함수 레벨 스코프

`var`는 **오로지 함수의 코드 블록만을 지역 스코프로 인정**한다.
즉, `{}` 내부에서 선언해도 함수 바깥이면 전역 변수가 된다.

```jsx
if (true) {
  var y = 100;
}
console.log(y); // 100 (if 블록 내부에서 선언했지만 전역 변수가 됨) ❌
```

### 변수 호이스팅

`var`는 선언과 동시에 **초기화(= undefined 할당)**가 이루어진다.
즉, 선언은 코드의 최상단으로 끌어올려지고, 초기값 `undefined`로 설정된다.

```jsx
console.log(z); // undefined (선언만 끌어올려짐)
var z = 30;
console.log(z); // 30
```

---

### 15.2 let 키워드

### 변수 중복 선언 금지

`let`은 **같은 변수명을 중복 선언할 수 없다**.
이는 불필요한 변수 재선언을 방지하고, 코드의 예측 가능성을 높여준다.

```jsx
let a = 10;
let a = 20; // SyntaxError ❌

```

### 블록 레벨 스코프

`let`은 모든 코드 블록 `{}`을 지역 스코프로 인정한다.
즉, 코드 블록 내부에서 선언한 변수는 해당 블록을 벗어나면 접근할 수 없다.

```jsx
if (true) {
  let b = 50;
}
console.log(b); // ReferenceError ❌
```

### 변수 호이스팅과 TDZ

`let`은 선언과 초기화가 분리되어 있다.
자바스크립트 엔진은 선언 단계에서 **실행 컨텍스트의 렉시컬 환경에 변수 식별자를 등록**한다.
그러나 초기화가 이루어지지 않으므로, **TDZ(Temporal Dead Zone, 일시적 사각지대)**에 놓인다.
TDZ 구간에서는 변수를 참조할 수 없어 `ReferenceError`가 발생한다.

```jsx
console.log(c); // ReferenceError ❌ (TDZ에 있음)
let c = 100;
console.log(c); // 100
```

### 전역 객체와 let

`var`로 선언한 전역 변수는 **전역 객체(window)의 프로퍼티가 된다**.
하지만 `let`으로 선언한 전역 변수는 전역 객체의 프로퍼티가 아니다.

```jsx
var globalVar = "I am var";
let globalLet = "I am let";
console.log(window.globalVar); // "I am var" ✅
console.log(window.globalLet); // undefined ❌
```

---

### 15.3 const 키워드

### 선언과 초기화

`const`는 선언과 동시에 **반드시 초기화**해야 한다.
초기값을 설정하지 않으면 `SyntaxError`가 발생한다.

```jsx
const d; // SyntaxError ❌
const d = 200; // ✅
```

### 재할당 금지

`const` 변수는 재할당이 불가능하다.
이는 상수 값을 보호하고, 불필요한 변경을 방지하기 위한 목적이다.

```jsx
const e = 10;
e = 20; // TypeError ❌
```

### 상수 표기 관례

일반적으로 상수는 **대문자로 선언**하며, 여러 단어로 이루어진 경우 **스네이크 케이스(SNAKE_CASE)**를 사용한다.

```jsx
const MAX_LIMIT = 100;
const API_KEY = "123456-abcdef";
```

### 객체에서의 const

`const`는 **재할당을 금지할 뿐, 객체의 불변성을 보장하지 않는다**.
즉, 객체 내부 속성은 변경할 수 있다.

```jsx
const user = { name: "Alice" };
user.name = "Bob"; // 가능 ✅
user = { name: "Charlie" }; // TypeError ❌ (재할당 불가)
```

👉 객체의 참조(주소)는 유지되지만, 내부 속성 값은 변경 가능하다.

---

### 15.4 var, let, const 비교

변수 선언에는 기본적으로 **`const`를 사용**하고,
재할당이 필요한 경우에만 `let`을 사용한다.
`var`는 사용하지 않는 것이 좋다 (호이스팅 문제, 전역 객체 프로퍼티화 등).

| 키워드  | 스코프    | 중복 선언 | 호이스팅                  | 재할당 | 전역 객체 프로퍼티 |
| ------- | --------- | --------- | ------------------------- | ------ | ------------------ |
| `var`   | 함수 레벨 | 가능      | 선언 + `undefined` 초기화 | 가능   | O                  |
| `let`   | 블록 레벨 | 불가능    | TDZ 존재                  | 가능   | X                  |
| `const` | 블록 레벨 | 불가능    | TDZ 존재                  | 불가능 | X                  |

👉 **최대한 `const`를 사용하고, 필요한 경우에만 `let`을 쓰는 것이 가장 좋은 방식!** 🚀