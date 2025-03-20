## 17장 생성자 함수에 의한 객체 생성

자바스크립트는 **프로토타입 기반 객체 지향 언어**로서, 클래스 기반 언어와 다르게 여러 가지 방식으로 객체를 생성할 수 있다. 그중 하나인 **생성자 함수(Constructor Function)** 를 사용한 객체 생성 방식에 대해 살펴본다.

---

## 17.1 Object 생성자 함수

### **생성자 함수란?**

생성자 함수란 `new` 연산자와 함께 호출하여 객체(인스턴스)를 생성하는 함수다. 생성자 함수에 의해 생성된 객체를 **인스턴스(Instance)** 라고 한다.

### **빌트인 생성자 함수**

자바스크립트는 `Object` 생성자 함수뿐만 아니라, 다양한 **빌트인 생성자 함수**를 제공한다.

예) `String`, `Number`, `Boolean`, `Function`, `Array`, `Date`, `RegExp`, `Promise` 등

### **Object 생성자 함수 예제**

```jsx
const obj = new Object(); // 빈 객체 생성
obj.name = "Hugo";
console.log(obj); // { name: "Hugo" }

```

### **결론**

`Object` 생성자 함수를 반드시 사용해야 하는 것은 아니다. 객체를 생성할 때 **객체 리터럴 `{}` 방식이 더 간편**하기 때문에, 특별한 이유가 없다면 객체 리터럴을 사용하는 것이 더 효율적이다.

```jsx
const obj = {}; // 객체 리터럴 방식이 더 간단

```

---

## 17.2 생성자 함수

### **17.2.1 객체 리터럴 방식의 한계**

객체 리터럴 방식은 직관적이고 간편하지만, **하나의 객체만 생성**할 수 있다. 즉, 동일한 프로퍼티 구조를 가지는 여러 객체를 생성할 때 비효율적이다.

```jsx
const user1 = { name: "Alice", age: 25 };
const user2 = { name: "Bob", age: 30 }; // 같은 구조의 객체를 반복해서 생성해야 함

```

### **17.2.2 생성자 함수의 장점**

생성자 함수는 **객체(인스턴스)를 생성하는 함수**다. 하지만 클래스 기반 언어(Java 등)처럼 특정한 형식이 정해져 있는 것이 아니라 **일반 함수처럼 정의하고 `new` 연산자와 함께 호출하면 생성자 함수로 동작**한다.

```jsx
function Person(name, age) {
  this.name = name;
  this.age = age;
}

const user1 = new Person("Alice", 25);

console.log(user1); // Person { name: "Alice", age: 25 }
```

✅ **`new`를 사용하면 생성자 함수로 동작하며, 그렇지 않으면 일반 함수로 실행된다.**

```jsx
function Person(name, age) {
  this.name = name; // this -> global object (window or globalThis)
  this.age = age;
}

const user = Person("Charlie", 28);

console.log(user); // undefined
console.log(user.name); // TypeError: Cannot read properties of undefined
```

---

## 17.2.3 생성자 함수의 인스턴스 생성 과정

생성자 함수는 **인스턴스를 생성하고 초기화하는 역할**을 한다. 하지만 개발자가 명시적으로 객체를 생성하고 반환하는 코드를 작성하지 않아도, **자바스크립트 엔진이 암묵적으로 처리**한다.

### **1️⃣ 인스턴스 생성과 `this` 바인딩**

- `new` 연산자와 함께 생성자 함수가 호출되면 **암묵적으로 빈 객체(인스턴스)** 가 생성된다.
- 이 인스턴스가 생성자 함수 내부의 `this`에 바인딩된다.
- 즉, 생성자 함수 내부에서 `this`를 사용하면 새롭게 생성된 인스턴스를 가리키게 된다.

```jsx
function Example() {
  console.log(this); // 새로 생성된 인스턴스
}

new Example();

```

### **2️⃣ 인스턴스 초기화**

- 생성자 함수의 코드가 실행되면서 `this`에 바인딩된 인스턴스에 **프로퍼티와 메서드가 추가**된다.
- 전달된 인수를 활용하여 초기값을 설정할 수도 있다.

```jsx
function Person(name, age) {
  this.name = name;
  this.age = age;
  this.sayHello = function () {
    console.log(`Hello, my name is ${this.name}.`);
  };
}

const user = new Person("Alice", 25);
user.sayHello(); // "Hello, my name is Alice."

```

### **3️⃣ 인스턴스 반환**

- 생성자 함수의 실행이 끝나면 **완성된 인스턴스가 `this`에 바인딩된 상태로 암묵적으로 반환**된다.
- 따라서 `return` 문을 사용하지 않아도 `new`를 통해 생성된 객체를 얻을 수 있다.

```jsx
const user = new Person("Alice", 25);
console.log(user); // Person { name: "Alice", age: 25, sayHello: ƒ }

```

✅ **만약 `return` 문에서 객체가 아닌 값을 반환하면, 무시되고 `this`(인스턴스)가 반환됨.**

```jsx
function Test() {
  this.value = 100;
  return 42; // 무시됨 (객체가 아닌 값 반환 시 `this`가 반환됨)
}

const instance = new Test();
console.log(instance); // Test { value: 100 }

```

✅ **하지만 객체를 반환하면 `this` 대신 해당 객체가 반환됨.**

```jsx
function Test() {
  this.value = 100;
  return { message: "Hello" }; // this가 아닌 새로운 객체 반환
}

const instance = new Test();
console.log(instance); // { message: "Hello" }

```

---

<aside>
🍊

## **정리**

✅ **객체 리터럴 방식은 직관적이지만, 동일한 구조의 객체를 여러 개 만들기에는 비효율적이다.**

✅ **생성자 함수는 `new`와 함께 호출하면 인스턴스를 생성하고, `this`를 통해 프로퍼티를 초기화할 수 있다.**

✅ **`new` 연산자 없이 호출하면 일반 함수처럼 동작하므로, 반드시 `new`와 함께 사용해야 한다.**

✅ **자바스크립트 엔진이 `new`를 사용한 생성자 함수 호출 시 3단계를 거쳐 인스턴스를 생성하고 반환한다.**

</aside>

---

## 17.2.4 내부 메서드 `[[Call]]` 과 `[[Construct]]`

자바스크립트에서 함수는 객체이지만, 일반 객체와는 다르게 **호출이 가능**하다. 일반 객체는 호출할 수 없지만, 함수 객체는 호출할 수 있기 때문에 **함수 객체에는 일반 객체가 가지지 않는 내부 슬롯과 내부 메서드가 추가로 포함**된다.

### **함수 객체의 내부 슬롯과 내부 메서드**

함수 객체는 일반 객체가 가지는 내부 슬롯과 내부 메서드 외에도 **함수로 동작하기 위한 내부 요소**를 가진다.

대표적으로:

- `[[Environment]]`, `[[FormalParameters]]` 등의 내부 슬롯
- `[[Call]]`, `[[Construct]]` 내부 메서드

### **`[[Call]]`과 `[[Construct]]`의 차이**

- **일반 함수 호출** → `[[Call]]` 내부 메서드가 실행됨.
- **`new` 연산자와 함께 호출** → `[[Construct]]` 내부 메서드가 실행됨.

```jsx
function example() {
  console.log("Hello");
}

example(); // [[Call]] 호출됨
new example(); // [[Construct]] 호출됨

```

### **함수 객체의 유형**

| 유형                  | 설명                                                                |
| --------------------- | ------------------------------------------------------------------- |
| **`Callable`**        | `[[Call]]`을 가지며, 호출할 수 있는 함수 객체                       |
| **`Constructor`**     | `[[Construct]]`를 가지며, `new`와 함께 생성자 함수로 호출할 수 있음 |
| **`Non-Constructor`** | `[[Construct]]`를 가지지 않으며, `new`와 함께 호출할 수 없음        |

결론적으로 **모든 함수 객체는 `[[Call]]`을 가지고 호출할 수 있지만, `[[Construct]]`를 가지는 함수만 생성자 함수로 호출 가능**하다.

```jsx
function ConstructorFunc() {}
const arrowFunc = () => {};

new ConstructorFunc(); // ✅ 가능
new arrowFunc(); // ❌ TypeError 발생 (arrowFunc은 Non-Constructor)

```

---

## 17.2.5 `Constructor` 와 `Non-Constructor`의 구분

자바스크립트 엔진은 **함수 정의 방식에 따라** 함수 객체를 **Constructor** 와 **Non-Constructor** 로 구분한다.

### **Constructor (생성자 함수가 될 수 있는 함수 유형)**

- **함수 선언문**
- **함수 표현식**
- **클래스 (클래스도 함수다)**

```jsx
function Person(name) {
  this.name = name;
}
const person1 = new Person("Alice"); // ✅ 가능

```

### **Non-Constructor (생성자로 사용할 수 없는 함수 유형)**

- **메서드 (ES6 메서드 축약 표현)**
- **화살표 함수**

```jsx
const obj = {
  method() { console.log("Hello"); }
};

const arrowFunc = () => {};

new obj.method(); // ❌ TypeError 발생
new arrowFunc(); // ❌ TypeError 발생

```

### **메서드와 일반 함수의 구분**

ECMAScript 사양에서는 **메서드(Method)를 일반적인 의미보다 좁은 범위로 정의**한다. 즉, **ES6의 메서드 축약 표현만을 메서드로 인정**한다.

```jsx
const obj = {
  method() { console.log("Hello"); } // ES6 메서드 축약 표현 → Non-Constructor
};

const obj2 = {
  method: function() { console.log("Hello"); } // 일반 함수 표현 → Constructor
};

new obj.method(); // ❌ TypeError (메서드 축약 표현은 Non-Constructor)
new obj2.method(); // ✅ 가능 (일반 함수 표현은 Constructor)

```

---

## **🚨 Non-Constructor인 함수는 `new`로 호출할 수 없다**

`[[Construct]]`를 가지지 않는 함수(NC 함수)는 `new`와 함께 호출할 수 없으며, **`TypeError`가 발생**한다.

```jsx
const arrowFunc = () => {};
new arrowFunc(); // ❌ TypeError

```

하지만, 일반 함수(`Callable`이면서 `Constructor`)는 **설계에 따라 `new` 없이도 동작할 수 있다.**

```jsx
function Example(name) {
  this.name = name;
}

const instance = Example("Alice"); // new 없이 호출
console.log(instance); // undefined
console.log(globalThis.name); // "Alice" (this가 globalThis를 참조)

```

따라서 `new` 없이 호출될 가능성이 있는 경우, 다음과 같이 `new`를 강제하는 패턴을 사용할 수도 있다.

```jsx
function Example(name) {
  if (!(this instanceof Example)) {
    return new Example(name);
  }
  this.name = name;
}

const user1 = Example("Alice");
const user2 = new Example("Bob");
console.log(user1.name); // "Alice"
console.log(user2.name); // "Bob"

```

---

<aside>
🍊

## **정리**

✅ **모든 함수 객체는 `[[Call]]`을 가지므로 호출할 수 있다.**

✅ **`[[Construct]]`를 가지는 함수만 `new`와 함께 생성자로 호출할 수 있다.**

✅ **함수 선언문, 함수 표현식, 클래스는 Constructor이지만, 화살표 함수와 메서드 축약 표현은 Non-Constructor이다.**

✅ **`Non-Constructor` 함수에 `new`를 붙이면 `TypeError`가 발생한다.**

✅ **생성자 함수로 사용할 함수를 정의할 때는 `new` 여부를 체크하는 방식으로 안전하게 구현할 수 있다.**

</aside>

---

## 17.2.6 `new` 연산자

### **📌 일반 함수와 생성자 함수의 차이**

자바스크립트에서 **일반 함수와 생성자 함수에는 특별한 형식적 차이가 없다**. 하지만 `new` 연산자를 사용하여 함수를 호출하면 해당 함수는 **생성자 함수**로 동작한다.

즉, 함수 객체의 내부 메서드 **`[[Call]]`** 이 아닌 **`[[Construct]]`** 가 호출된다.
단, `new` 연산자와 함께 호출하는 함수는 **`Non-Constructor`가 아닌 `Constructor`여야 한다.**

```jsx
function Person(name) {
  this.name = name;
}

const user = new Person("Alice"); // [[Construct]] 호출
console.log(user.name); // "Alice"

const user2 = Person("Bob"); // [[Call]] 호출
console.log(globalThis.name); // "Bob" (this가 global 객체를 가리킴)

```

### **📌 생성자 함수의 네이밍 규칙**

생성자 함수는 일반적으로 **첫 문자를 대문자로** 작성하는 **파스칼 케이스(PascalCase)** 를 따르는 것이 관례이다.

---

## 17.2.7 `new.target`

### **📌 `new.target` 이란?**

`new.target`은 **생성자 함수가 `new` 연산자 없이 호출되는 것을 방지**하기 위해 사용된다.

- `new.target`은 `this`와 유사하게 **모든 생성자 함수 내부에서 암묵적으로 사용 가능한 지역 변수**이며, **메타 프로퍼티**라고 한다.
- `new`와 함께 생성자 함수로 호출되면 **함수 자체를 가리킨다.**
- `new` 없이 일반 함수로 호출되면 **`undefined`를 반환한다.**

```jsx
function Person(name) {
  if (!new.target) {
    return new Person(name); // new 없이 호출되면 자동으로 new를 붙여 재호출
  }
  this.name = name;
}

const user1 = Person("Alice"); // new 없이 호출 → 자동으로 new Person("Alice") 실행
const user2 = new Person("Bob");

console.log(user1.name); // "Alice"
console.log(user2.name); // "Bob"

```

🔹 **주의:** `new.target`은 최신 문법이므로 **IE에서는 지원되지 않는다.**

---

## **📌 스코프 세이프 생성자 패턴 (Scope-Safe Constructor)**

`new.target`을 사용할 수 없는 환경(예: IE)에서는 **스코프 세이프 생성자 패턴**을 사용하여 `new` 없이 호출되었을 때 자동으로 `new`를 붙이도록 처리할 수 있다.

```jsx
function Example(name) {
  if (!(this instanceof Example)) {
    return new Example(name);
  }
  this.name = name;
}

const user1 = Example("Alice");
const user2 = new Example("Bob");

console.log(user1.name); // "Alice"
console.log(user2.name); // "Bob"

```

✅ `this instanceof Example`를 사용하여 `new` 없이 호출되었는지 확인 후, **자동으로 `new`를 붙여 다시 호출**함.

---

## **📌 `new` 연산자 없이 호출될 때의 동작**

빌트인 생성자 함수(`Object`, `String`, `Number`, `Boolean`, `Function`, `Array`, `Date`, `RegExp`, `Promise` 등)는 **`new` 연산자와 함께 호출되었는지 확인한 후, 적절한 값을 반환**한다.

| 생성자 함수  | `new` 사용 | 반환 값                    |
| ------------ | ---------- | -------------------------- |
| `Object()`   | O          | 객체 반환                  |
| `Object()`   | X          | 객체 반환 (차이 없음)      |
| `Function()` | O          | 함수 객체 반환             |
| `Function()` | X          | 함수 객체 반환 (차이 없음) |
| `String()`   | O          | `String` 객체 반환         |
| `String()`   | X          | 문자열 반환                |
| `Number()`   | O          | `Number` 객체 반환         |
| `Number()`   | X          | 숫자 반환                  |
| `Boolean()`  | O          | `Boolean` 객체 반환        |
| `Boolean()`  | X          | 불리언 값 반환             |

```jsx
console.log(new String("Hello")); // [String: "Hello"] 객체
console.log(String("Hello")); // "Hello" (단순 문자열 반환)

console.log(new Number(123)); // [Number: 123] 객체
console.log(Number(123)); // 123 (단순 숫자 반환)

console.log(new Boolean(false)); // [Boolean: false] 객체
console.log(Boolean(false)); // false (단순 불리언 값 반환)

```

✅ `String`, `Number`, `Boolean` 생성자 함수는 `new`와 함께 호출하면 **객체 타입**을 반환하지만, `new` 없이 호출하면 **기본 타입 값**을 반환한다.

---

<aside>
🍊

## **정리**

✅ `new` 연산자를 사용하면 **함수의 `[[Call]]`이 아니라 `[[Construct]]`가 호출되어 생성자 함수로 동작**한다.

✅ 일반적으로 생성자 함수는 **파스칼 케이스(PascalCase)로 작성하여 구별**하는 것이 좋다.

✅ `new.target`을 사용하면 **`new` 없이 호출되는 것을 방지하고 자동으로 `new`를 붙여 다시 호출**할 수 있다.

✅ `new.target`을 지원하지 않는 환경에서는 **`instanceof`를 활용한 스코프 세이프 생성자 패턴을 사용할 수 있다.**

✅ 대부분의 빌트인 생성자 함수(`Object`, `Function` 등)는 `new` 없이 호출해도 동일한 동작을 하지만, `String`, `Number`, `Boolean` 등은 `new` 여부에 따라 반환 타입이 달라진다.

</aside>