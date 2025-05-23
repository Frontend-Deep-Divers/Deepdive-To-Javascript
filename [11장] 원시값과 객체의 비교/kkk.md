# **11장 원시 값과 객체의 비교**

# MP

# 🩵 **객체 안의 프로퍼티가 원시값을 가질 때, 메모리 동작 방식**

```jsx
let obj = { value: 10 };  // 객체 생성 (참조 타입)
let copy = obj;  // 같은 객체를 참조 (참조값 복사)

copy.value = 20;  // 프로퍼티 변경 (새 메모리 할당 X)

console.log(obj.value);  // 20 (원본도 변경됨)
```

📌 **객체 안의 프로퍼티는 원시 값을 가질 수 있지만, 객체 자체는 참조 타입이므로 프로퍼티를 변경해도 새로운 객체가 생성되지 않는다.**

📌 **반면, 원시 값을 변수에 직접 할당하면 새로운 메모리 공간을 할당받고 복사된다.**

🚀 **즉, "원시값의 불변성"은 유지되지만, 객체 프로퍼티는 새로운 메모리 공간을 할당하지 않고 바로 변경 가능하도록 동작한다.**

# **🩵 자바스크립트에서 값이 원시 타입인지 객체 타입인지 판단하기 전에, 먼저 프로퍼티 여부를 확인하는가?**

**동일한 원시 값이라도 "어디에 저장되었느냐"에 따라 동작 방식이 달라진다.**

✅ **변수에 저장된 원시값 → 새로운 메모리 공간 생성**

✅ **객체 프로퍼티로 저장된 원시값 → 객체 내부에서 변경(새로운 객체 생성 X)**

### **📌 실행 과정 (변수 선언 → 할당 → 타입 확인)**

✔**자바스크립트는 변수 선언을 먼저 실행(호이스팅)한 후, 코드 실행 순서대로 변수에 값을 할당할 때 원시 타입인지 객체 타입인지 판별한다.**

✔**할당된 값이 원시 값이면 "값 자체"를 저장하고, 객체면 "참조 값"을 저장한다.**

✔**변수에 접근할 때도 먼저 "객체인지 확인하고" 이후 원시 값인지 판단하여 적절한 동작을 수행한다.**

### 정리

✔**"변수에 저장된 원시값은 값 자체를 저장하지만, 객체 프로퍼티로 저장된 원시값은 객체 내부에서 관리되므로 새로운 객체가 생성되지 않는다."**

✔**"자바스크립트는 변수 선언을 먼저 실행(호이스팅)한 후, 값이 할당될 때 비로소 원시 타입인지 객체 타입인지 판단한다."**

---

## **1. 자바스크립트의 데이터 타입**

자바스크립트는 **7가지 데이터 타입**을 제공하며, 이를 **원시 타입(Primitive Type)**과 **객체 타입(Object Type)**으로 구분할 수 있다.

- **원시 타입 (Primitive Type)**
    - 숫자 (**`number`**) / 문자열 (**`string`**) / 불리언 (**`boolean`**) / **`null` / `undefined` /** 심벌 (**`symbol`**) / 빅인트 (**`bigint`**):(**ES11 (ECMAScript 2020)에 추가**)
- **객체 타입 (Object Type)**
    - 객체 (**`Object`**) / 배열 (**`Array`**) / 함수 (**`Function`**) / 날짜 (**`Date`**) 등등…

### **원시 타입 vs 객체 타입**

| **구분**             | **원시 타입**                      | **객체 타입**                        |
| -------------------- | ---------------------------------- | ------------------------------------ |
| **변경 가능 여부**   | ❌ 변경 불가능 (Immutable)          | ✅ 변경 가능 (Mutable)                |
| **메모리 저장 방식** | 값 자체 저장                       | 참조 값 저장                         |
| **변수에 할당 시**   | 값이 복사됨 (Pass by Value)        | 참조 값이 복사됨 (Pass by Reference) |
| **재할당 방식**      | 새로운 메모리 공간을 확보하여 저장 | 동일한 객체를 공유                   |

---

## **2. 원시값 (Primitive Values)**

### **2.1 원시 값은 변경 불가능 (Immutable)**

원시값은 생성되면 **절대 변경할 수 없으며(Read-Only),**

새로운 값을 재할당하면 **새로운 메모리 공간을 확보하여 값을 저장한다.**

즉, **변경되는 것은 변수이지, 값 자체가 아니다!**

```jsx
let a = 10;
let b = a;  // 값이 복사됨 (새로운 메모리 공간 확보)
b = 20;

console.log(a); // 10  (a는 변경되지 않음)
console.log(b); // 20  (b만 변경됨)
```

✅ **변경 불가능한 원시 값의 특징:**

- 기존 값이 바뀌는 것이 아니라, **새로운 값이 저장된 메모리 주소를 참조하도록 변경**됨.
- 이를 **"불변성(immutability)"**이라고 한다.

---

### **2.2 문자열(String)의 불변성**

문자열도 **원시 값이므로 불변(Immutable)** 하다.

문자열을 변경하면 기존 값이 변경되는 것이 아니라, **`새로운 문자열이 생성된다.`**

```jsx
let str = "Hello";
str = str + " World";  // 새로운 문자열이 생성됨

console.log(str); // "Hello World"
```

✅ **기존 문자열을 직접 수정하는 것이 아니라, 새 문자열을 생성하여 변수에 재할당됨.**

---

### **2.3 값에 의한 전달 (Pass by Value)**

변수에 원시 값을 할당하면 **값이 복사**되어 전달된다.

즉, **서로 다른 메모리 공간에 독립적으로 저장됨.**

```jsx
let x = 10;
let y = x;  // x의 값이 복사됨 (서로 다른 메모리 공간에 저장)
y = 20;

console.log(x); // 10 (변경되지 않음)
console.log(y); // 20 (변경됨)
```

✅ 원시 값은 **변경할 수 없으며, 변수에 새로운 값을 할당할 때마다 새로운 메모리 공간을 확보**한다.

---

## **3. 객체 (Objects)**

원시 값과 달리, 객체는 "참조 타입(Reference Type)"으로 동작한다.

### **3.1 객체의 메모리 저장 방식**

- 객체를 변수에 할당하면, 객체의 메모리 주소(참조 값)가 저장된다.
- 따라서 **변수를 통해 객체를 변경하면, 해당 객체를 참조하는 모든 변수가 영향을 받는다.**
- **객체를 직접 변경할 수 있으며(Mutable), 원시 값처럼 새로운 객체를 매번 생성하지 않는다.**

```jsx
let obj1 = { name: "홍길동" };
let obj2 = obj1;  // obj1의 참조 값을 복사 (같은 객체를 가리킴)

obj2.name = "김철수";  // obj2를 수정하면 obj1도 영향을 받음
console.log(obj1.name); // "김철수"
console.log(obj2.name); // "김철수
```

✅ **객체는 "참조에 의한 전달(Pass by Reference)" 방식으로 동작한다.**

---

### **3.2 객체의 변경 가능성 (Mutable)**

객체는 **동일한 메모리 주소를 공유**하므로,

객체를 변경할 때 **새로운 객체가 생성되지 않고, 기존 객체가 직접 수정**된다.

```jsx
const person = { name: "홍길동" };
person.age = 30;  // 기존 객체에 새로운 프로퍼티 추가

console.log(person);  // { name: "홍길동", age: 30 
```

✅ **객체를 변경하면 기존 객체가 그대로 수정됨(메모리 주소는 변하지 않음).**

---

### **3.3 객체 변경의 부작용**

객체를 여러 변수가 공유하면, **하나의 변수를 통해 변경한 내용이 다른 변수에도 영향을 미칠 수 있다.**

```jsx
let user1 = { name: "Alice" };
let user2 = user1;  // 같은 객체를 참조

user2.name = "Bob";  // user2를 변경하면 user1도 영향을 받음
console.log(user1.name); // "Bob"
```

✅ **객체의 공유로 인해 데이터가 예기치 않게 변경될 수 있는 문제점이 있다.**

---

## **4. 얕은 복사(Shallow Copy)와 깊은 복사(Deep Copy)**

객체의 **참조 값이 복사되는 문제를 해결하기 위해, 객체를 복사하는 방법이 필요**하다.

이를 “얕은 복사(Shallow Copy)”와 "깊은 복사(Deep Copy)"로 구분할 수 있다.

### **4.1 얕은 복사 (Shallow Copy)**

- **1단계 프로퍼티만 복사**되며, **중첩 객체(내부 객체)는 여전히 원본 객체를 참조**한다.
- 즉, **내부 객체를 변경하면 원본 객체에도 영향을 미친다.**

```jsx
const original = { name: "홍길동", details: { age: 30 } };
const copy = { ...original };  // 얕은 복사 (spread 연산자)

copy.details.age = 40;  // 내부 객체 수정
console.log(original.details.age); // 40 (원본도 영향을 받음)
```

✅ **얕은 복사로 인해 내부 객체는 여전히 원본 객체를 `공유`하고 있음**

### **얕은 복사 방법**

```jsx
const copy1 = Object.assign({}, original);
const copy2 = { ...original };  // Spread 연산
```

---

### **4.2 깊은 복사 (Deep Copy)**

- **중첩된 객체까지 완전히 새로운 객체를 생성**하여 복사한다.
- 원본 객체와 완전히 독립적인 객체를 만들기 때문에, **어떤 변경도 서로 영향을 주지 않는다.**

```jsx
const original = { name: "홍길동", details: { age: 30 } };

// 깊은 복사 (JSON 방법)
const deepCopy = JSON.parse(JSON.stringify(original));

deepCopy.details.age = 40;  // 내부 객체 변경
console.log(original.details.age); // 30 (원본은 유지됨)
```

✅ **깊은 복사를 하면 원본과 독립적인 객체가 생성되므로, 변경해도 원본에 영향을 주지 않음.**

### **깊은 복사 방법**

```jsx
// 방법 1: JSON 방식 (단, 함수/심벌은 복사 불가능)
const deepCopy = JSON.parse(JSON.stringify(original));

// 방법 2: Lodash 라이브러리 사용
const _ = require('lodash');
const deepCopy2 = _.cloneDeep(original);
```

---

## **📌 결론**

- 원시 값(Primitive Type)은 **불변(Immutable)하며, 값 자체가 복사**된다.
- 객체(Object Type)는 **변경 가능(Mutable)하며, 참조 값이 복사**된다.
- 얕은 복사(Shallow Copy)는 1단계 프로퍼티만 복사하며, **내부 객체는 `원본과 공유`**된다.
- 깊은 복사(Deep Copy)는 **중첩된 객체까지 완전히 새로운 객체를 생성**하여, `원본과 완전히 독립적`이다. 🚀

아늬 이장 개어렵네..
    