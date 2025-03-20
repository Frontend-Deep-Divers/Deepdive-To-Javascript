## 16장 프로퍼티 어트리뷰트

### 16.1 `내부 슬롯`과 `내부 메서드`

내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드다.

ECMAScript 사양에서는 내부 슬롯과 내부 메서드를 이중 대괄호(`[[...]]`)로 감싸서 표기한다. 이는 개발자가 직접 접근할 수 있는 객체 프로퍼티가 아니라 자바스크립트 엔진 내부에서만 사용되는 요소다. 따라서 원칙적으로 직접 접근하거나 호출할 수 있는 방법을 제공하지 않는다.

단, 일부 내부 슬롯은 간접적으로 접근할 수 있는 수단을 제공하기도 한다. 예를 들어, 모든 객체는 `[[Prototype]]`이라는 내부 슬롯을 가지는데, 원칙적으로 직접 접근할 수 없지만 `__proto__`를 통해 간접적으로 접근할 수 있다.

> 내부슬롯 대표 예제가 [[Prototype]] 이다.
> 

```jsx
const obj = { key: 'value' };
console.log(obj.__proto__); // Object.prototype에 접근 가능

```

### 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

> 프로퍼티 생성할 때 무슨 일이 벌어지는가??? 프로퍼티 어트리뷰트는 프로퍼티의 상태를 나타냄. 여러 속성 포함함.
> 

자바스크립트 엔진은 `프로퍼티를 생성`할 때, 해당 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다. 프로퍼티의 상태란 다음과 같은 속성을 포함한다.

```jsx
- **값 (Value)**: 프로퍼티의 값
- **값의 갱신 가능 여부 (Writable)**: 값을 변경할 수 있는지 여부
- **열거 가능 여부 (Enumerable)**: `for...in` 루프나 `Object.keys`에서 조회 가능한지 여부
- **재정의 가능 여부 (Configurable)**: `delete`로 삭제하거나, 어트리뷰트를 수정할 수 있는지 여부
```

직접 접근할 수 없지만 `Object.getOwnPropertyDescriptor` 메서드를 사용하여 간접적으로 확인할 수 있다.

```jsx
const obj = { key: 'value' };
const descriptor = Object.getOwnPropertyDescriptor(obj, 'key');
console.log(descriptor);
// { value: 'value', writable: true, enumerable: true, configurable: true }

```

---

### 16.3 데이터 프로퍼티와 접근자 프로퍼티

### 데이터 프로퍼티 (Data Property)

데이터 프로퍼티는 키와 값으로 구성된 일반적인 프로퍼티다. 지금까지 살펴본 모든 프로퍼티는 데이터 프로퍼티에 해당한다.

```jsx
const obj = {
  key: 'value'
};
console.log(obj.key); // 'value'

```

### 접근자 프로퍼티 (Accessor Property)

접근자 프로퍼티는 자체적으로 값을 가지지 않고, 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수(Accessor Function)로 구성된 프로퍼티다. `getter`와 `setter`를 통해 값을 정의할 수 있다.

```jsx
const obj = {
  _value: 10,
  get value() {
    return this._value;
  },
  set value(val) {
    this._value = val;
  }
};

console.log(obj.value); // 10 (getter 호출)
obj.value = 20; // setter 호출
console.log(obj.value); // 20

```

접근자 프로퍼티는 `Object.getOwnPropertyDescriptor`를 사용하면 `get`과 `set` 메서드로 확인할 수 있다.

```jsx
const descriptor = Object.getOwnPropertyDescriptor(obj, 'value');
console.log(descriptor);
// { get: [Function: get value], set: [Function: set value], enumerable: true, configurable: true }

```

## 16.3.1 데이터 프로퍼티

데이터 프로퍼티(Data Property)는 키와 값으로 구성된 일반적인 프로퍼티로, 다음과 같은 **4가지 프로퍼티 어트리뷰트**를 가진다. 이 어트리뷰트는 자바스크립트 엔진이 프로퍼티를 생성할 때 자동으로 기본값으로 설정된다.

### 데이터 프로퍼티의 프로퍼티 어트리뷰트

1. **`[[Value]]`**: 프로퍼티의 값
2. **`[[Writable]]`**: 값을 변경할 수 있는지 여부 (기본값: `true`)
3. **`[[Enumerable]]`**: 열거 가능 여부 (`for...in`, `Object.keys` 사용 가능 여부, 기본값: `true`)
4. **`[[Configurable]]`**: 삭제 및 속성 변경 가능 여부 (기본값: `true`)

<aside>
🍊

프로퍼티가 생성될 때:

- `[[Value]]`는 해당 프로퍼티 값으로 초기화된다.
- 나머지 `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]`은 기본적으로 `true`로 설정된다.
- 프로퍼티를 동적으로 추가할 때도 동일한 규칙이 적용된다.

</aside>

```jsx
const person = {
  firstName: 'Hugo'
};

// 프로퍼티 디스크립터 확인
const descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// { value: 'Hugo', writable: true, enumerable: true, configurable: true }

```

---

## 16.3.2 접근자 프로퍼티

접근자 프로퍼티(Accessor Property)는 자체적으로 값을 가지지 않고, **다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수**로 구성된 프로퍼티이다. 접근자 프로퍼티는 다음과 같은 **4가지 프로퍼티 어트리뷰트**를 가진다.

### 접근자 프로퍼티의 프로퍼티 어트리뷰트

1. **`[[Get]]`**: 값을 읽을 때 호출되는 함수 (getter)
2. **`[[Set]]`**: 값을 저장할 때 호출되는 함수 (setter)
3. **`[[Enumerable]]`**: 열거 가능 여부 (기본값: `true`)
4. **`[[Configurable]]`**: 삭제 및 속성 변경 가능 여부 (기본값: `true`)

### 예제: 데이터 프로퍼티와 접근자 프로퍼티

```jsx
const person = {
  firstName: 'Hugo',
  lastName: 'Smith',

  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  set fullName(name) {
    const parts = name.split(' ');
    this.firstName = parts[0];
    this.lastName = parts[1];
  }
};

console.log(person.fullName); // "Hugo Smith" (getter 호출)
person.fullName = 'John Doe'; // setter 호출
console.log(person.firstName); // "John"
console.log(person.lastName); // "Doe"

```

내부슬롯/메서드관점 동작과정

<aside>
🍊

1. 프로퍼티 키가 유효한지 확인
    1. 프로퍼티 키는 문자열 또는 심벌이여야 한다.
2. 프로토타입 체인에서 프로퍼티 검색
    1. 객체에 해당 프로퍼티 존재하는가?
3. 검색된 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인
4. 접근자 프로퍼티의 프로퍼티 어트리뷰트 [[Get]]의 값, getter 함수를 호출한 결과 반환.
</aside>

### 프로퍼티 디스크립터 확인

```jsx
// firstName (데이터 프로퍼티)
let descriptor = Object.getOwnPropertyDescriptor(person, 'firstName');
console.log(descriptor);
// { value: 'John', writable: true, enumerable: true, configurable: true }

// fullName (접근자 프로퍼티)
descriptor = Object.getOwnPropertyDescriptor(person, 'fullName');
console.log(descriptor);
// { get: [Function: get fullName], set: [Function: set fullName], enumerable: true, configurable: true }

```

### 데이터 프로퍼티 vs 접근자 프로퍼티 구별법

객체의 프로퍼티 어트리뷰트를 확인할 때, **`Object.getOwnPropertyDescriptor()`**를 사용하여 반환된 **프로퍼티 디스크립터 객체**를 보면 두 프로퍼티가 다른 방식으로 정의되어 있음을 알 수 있다.

- **데이터 프로퍼티**는 `[[Value]]` 어트리뷰트를 가진다.
- **접근자 프로퍼티**는 `[[Get]]`, `[[Set]]` 어트리뷰트를 가진다.

이러한 개념을 활용하면, 객체의 속성을 보다 정교하게 제어할 수 있다.

---

## 16.4 프로퍼티 정의

### **프로퍼티 정의란?**

프로퍼티 정의는 **새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 어트리뷰트를 재정의하는 것**을 의미한다.

예를 들어, 프로퍼티 값을 변경할 수 있도록 할 것인지, 열거 가능하게 할 것인지, 재정의 가능하게 할 것인지를 설정할 수 있다.

`Object.defineProperty` 메서드를 사용하면 프로퍼티의 어트리뷰트를 명시적으로 정의할 수 있다. 이 메서드는 **객체의 참조**, **데이터 프로퍼티의 키(문자열)**, 그리고 **프로퍼티 디스크립터 객체**를 인수로 받는다.

```jsx
const obj = {};

Object.defineProperty(obj, 'key', {
  value: 42,
  writable: false, // 값 변경 불가능
  enumerable: false, // 열거 불가능
  configurable: false // 삭제 및 재정의 불가능
});

console.log(obj.key); // 42
obj.key = 100; // 변경되지 않음 (writable: false)
console.log(obj.key); // 42

```

---

## 16.5 객체 변경 방지

자바스크립트에서 객체는 기본적으로 변경 가능하며, 프로퍼티의 추가, 삭제, 변경이 자유롭다. 하지만 **객체 변경을 방지하는 다양한 메서드**를 제공하여 특정 수준에서 객체의 변경을 막을 수 있다.

### **16.5.1 객체 확장 금지 (`Object.preventExtensions`)**

`Object.preventExtensions(obj)` 메서드를 사용하면 **객체 확장(프로퍼티 추가)을 금지**할 수 있다. 하지만 기존 프로퍼티의 수정과 삭제는 가능하다.

```jsx
const obj = { prop: 42 };

Object.preventExtensions(obj);

obj.newProp = 'new'; // 추가되지 않음
console.log(obj.newProp); // undefined

console.log(Object.isExtensible(obj)); // false

```

### **16.5.2 객체 밀봉 (`Object.seal`)**

`Object.seal(obj)` 메서드를 사용하면 **객체를 밀봉(seal)** 하여 **프로퍼티 추가 및 삭제, 프로퍼티 어트리뷰트 재정의를 금지**할 수 있다. 하지만 기존 프로퍼티의 값 변경은 가능하다.

```jsx
const obj = { prop: 42 };

Object.seal(obj);

obj.newProp = 'new'; // 추가 불가능
console.log(obj.newProp); // undefined

delete obj.prop; // 삭제 불가능
console.log(obj.prop); // 42

console.log(Object.isSealed(obj)); // true

```

### **16.5.3 객체 동결 (`Object.freeze`)**

`Object.freeze(obj)` 메서드를 사용하면 **객체를 동결(freeze)** 하여 **프로퍼티 추가, 삭제, 어트리뷰트 재정의뿐만 아니라, 값 변경까지 금지**할 수 있다.

```jsx
const obj = { prop: 42 };

Object.freeze(obj);

obj.prop = 100; // 변경 불가능
console.log(obj.prop); // 42

console.log(Object.isFrozen(obj)); // true

```

### **16.5.4 불변 객체 (Deep Freeze)**

지금까지 살펴본 `Object.preventExtensions`, `Object.seal`, `Object.freeze` 메서드들은 **얕은 변경 방지(shallow-only)** 를 적용하므로, **중첩 객체까지 변경을 방지하지는 못한다.** 즉, `Object.freeze`를 사용해도 내부 중첩 객체는 여전히 변경할 수 있다.

```jsx
const obj = {
  outer: {
    inner: 42
  }
};

Object.freeze(obj);

obj.outer.inner = 100; // 내부 객체는 변경 가능
console.log(obj.outer.inner); // 100

```

중첩 객체까지 변경을 방지하려면 **재귀적으로 `Object.freeze()`를 적용하는 함수**를 만들어야 한다.

```jsx
function deepFreeze(obj) {
  Object.freeze(obj);

  Object.keys(obj).forEach(key => {
    if (typeof obj[key] === 'object' && obj[key] !== null) {
      deepFreeze(obj[key]);
    }
  });
}

const obj = {
  outer: {
    inner: 42
  }
};

deepFreeze(obj);

obj.outer.inner = 100; // 변경 불가능
console.log(obj.outer.inner); // 42

```

---

## **🚀 정리**

| 메서드                       | 프로퍼티 추가 | 프로퍼티 삭제 | 속성 변경 | 값 변경  |
| ---------------------------- | ------------- | ------------- | --------- | -------- |
| `Object.preventExtensions()` | ❌ 불가능      | ✅ 가능        | ✅ 가능    | ✅ 가능   |
| `Object.seal()`              | ❌ 불가능      | ❌ 불가능      | ❌ 불가능  | ✅ 가능   |
| `Object.freeze()`            | ❌ 불가능      | ❌ 불가능      | ❌ 불가능  | ❌ 불가능 |

✅ `Object.preventExtensions()` → **새로운 프로퍼티 추가만 금지**

✅ `Object.seal()` → **프로퍼티 추가, 삭제 및 속성 변경 금지 (값 변경은 가능)**

✅ `Object.freeze()` → **프로퍼티 추가, 삭제, 속성 변경, 값 변경까지 모두 금지**

✅ **중첩 객체까지 동결하려면 `deepFreeze()`를 사용해야 함**