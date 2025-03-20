## 18.1 일급 객체

### **📌 일급 객체란?**

다음과 같은 조건을 만족하는 객체를 **일급 객체(First-Class Object)** 라고 한다.

<aside>
🍊

1. 무명 리터럴로 생성할 수 있다. (즉, 런타임에 생성 가능)
2. 변수나 자료구조(객체, 배열 등)에 저장할 수 있다.
3. 함수의 매개변수로 전달할 수 있다.
4. 함수의 반환값으로 사용할 수 있다.
</aside>

### **📌 자바스크립트 함수는 일급 객체인가?**

✅ **자바스크립트 함수는 일급 객체다.** 즉, 함수를 객체처럼 다룰 수 있으며, 변수에 할당하거나 다른 함수의 인수 또는 반환값으로 사용할 수 있다.

```jsx
const sayHello = function () {
  return "Hello!";
};

function executeFunction(func) {
  console.log(func());
}

executeFunction(sayHello); // "Hello!"
```

✅ **함수형 프로그래밍이 가능해지는 이유**
일급 객체로서의 함수는 **고차 함수(Higher-Order Function)** 를 가능하게 하며, 이를 통해 함수형 프로그래밍을 구현할 수 있다.

✅ **함수는 일반 객체와 다르다**

- 일반 객체는 호출할 수 없지만, 함수 객체는 호출할 수 있다.
- 함수 객체는 일반 객체에는 없는 **고유의 프로퍼티**를 가진다.

---

## 18.2 함수 객체의 프로퍼티

### **📌 함수 객체의 고유 프로퍼티**

함수 객체는 일반 객체에는 없는 몇 가지 고유 프로퍼티를 가진다. 이를 `Object.getOwnPropertyDescriptors`를 사용하여 확인할 수 있다.

```jsx
function example() {}
console.log(Object.getOwnPropertyDescriptors(example));

```

✅ 함수 객체의 주요 프로퍼티:

- `arguments`
- `caller`
- `length`
- `name`
- `prototype`

> **단, `__proto__`는 접근자 프로퍼티로, 함수 객체 고유의 프로퍼티가 아니라 `Object.prototype`에서 상속받은 것이다.**
> 

---

### **18.2.1 `arguments` 프로퍼티**

✅ `arguments` 프로퍼티는 **함수 호출 시 전달된 인수 정보를 담고 있는 유사 배열 객체(arguments 객체)** 를 가리킨다.

```jsx
function multiply(a, b) {
  console.log(arguments);
}

multiply(1, 2, 3, 4); // { '0': 1, '1': 2, '2': 3, '3': 4 }

```

✅ 주요 특징:

- `arguments.length` → 전달된 인수 개수를 반환
- `arguments.callee` → 현재 실행 중인 함수 자신을 가리킴 (ES6 이후 사용 비권장)
- 배열처럼 보이지만 **실제 배열이 아닌 유사 배열 객체**
- ES6에서 `rest parameter (...)`가 등장하면서 `arguments`의 필요성이 줄어듦

```jsx
function sum(...args) {
  console.log(args); // [1, 2, 3, 4]
}

sum(1, 2, 3, 4);

```

---

### **18.2.6 `prototype` 프로퍼티**

✅ **`prototype` 프로퍼티는 생성자 함수만이 가지는 프로퍼티**이며, 생성자 함수가 생성할 인스턴스의 **프로토타입 객체를 가리킨다.**

```jsx
function Person(name) {
  this.name = name;
}

console.log(Person.prototype); // Person의 프로토타입 객체

```

✅ **일반 객체와 생성자 함수의 차이**

```jsx
console.log((function () {}).hasOwnProperty('prototype')); // true (함수 객체는 prototype 보유)
console.log(({}).hasOwnProperty('prototype')); // false (일반 객체는 prototype 없음)

```

✅ **프로토타입 체인과 연관됨**

```jsx
const user = new Person("Alice");
console.log(user.__proto__ === Person.prototype); // true

```

**즉, `prototype` 프로퍼티는 생성된 인스턴스의 `__proto__`와 연결된다.**

---

<aside>
🍊

## **정리**

✅ **자바스크립트의 함수는 일급 객체로, 변수에 할당하거나 다른 함수의 인수 및 반환값으로 사용할 수 있다.**

✅ **함수 객체는 일반 객체와 다르게 `arguments`, `caller`, `prototype` 등의 고유 프로퍼티를 가진다.**

✅ **`arguments` 객체는 함수 호출 시 전달된 인수 정보를 담고 있는 유사 배열 객체이며, ES6의 `rest parameter`가 이를 대체할 수 있다.**

✅ **`prototype` 프로퍼티는 생성자 함수만 가지며, 생성된 인스턴스의 `__proto__`와 연결되어 프로토타입 체인을 형성한다.**

</aside>