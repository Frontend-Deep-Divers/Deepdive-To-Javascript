# 16.1 내부 슬롯과 내부 어트리뷰트

---

- 프로퍼티 어트리뷰트를 이해하기 위해 내부 슬롯과 내부 메서드의 개념에 대해 알아보자.
- 💭 내부 슬롯과 내부 메서드는 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 ECMAScript 사양에서 사용하는 의사 프로퍼티와 의사 메서드이다. ECMAScript 사양에 등장하는 이중 대괄호 [[…]] 로 감싼 이름들이 내부 슬롯과 내부 메서드이다.
- ❗내부 슬롯과 내부 메서드는 ECMAScript 사양에 정의된 대로 구현되어 자바스크립트 엔진에서 실제로 동작하지만 개발자가 직접 접근할 수 있도록 외부에 공개된 객체의 프로퍼티는 아니다.
- 예를 들어, 모든 객체는 [[Prototype]] 이라는 내부 슬롯을 가진다. 직접 접근할 수는 없지만, **proto** 를 통해 간접적으로 접근할 수 있다.

# 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

---

- 자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.
- ❗프로퍼티의 상태 (프로퍼티 어트리부트)
  - 값 : [[Value]]
  - 값의 갱신 여부 : [[Writable]]
  - 열겨 가능 여부 : [[Enumerable]]
  - 정의 가능 여부 : [[Configurable]]
- ❗직접 접근할 수는 없기에 Object.getOwnPropertyDescriptor 메서드를 사용하여 간접적으로 확인할 수 있다. → 해당 메서드로 프로퍼티 디스크립터 객체를 반환한다. , 존재하지 않으면 undefined

```jsx
const person = {
  name: "lee",
};

console.log(Object.getOwnPropertyDescriptor(person, "name"));
// { value: 'lee', writable: true, enumerable: true, configurable: true }
```

- Object.getOwnPropertyDescriptors 는 모든 프로퍼티의 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체들을 반환한다.

```jsx
const person = {
  name: "lee",
};
person.age = 20;
console.log(Object.getOwnPropertyDescriptors(person));

{
  name: {
    value: 'lee',
    writable: true,
    enumerable: true,
    configurable: true
  },
  age: { value: 20, writable: true, enumerable: true, configurable: true }
}
```

# 16.3 데이터 프로퍼티와 접근자 프로퍼티

---

- ⚠️ 프로퍼이의 종류
  - 데이터 프로퍼티: 키와 값으로 구성된 일반적이 프로퍼티, 지금까지 살펴본 모든 프로퍼티
  - ❓접근자 프로퍼티: 자체적으로 값을 가지지 않고, 다른 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티이다.

1. **데이터 프로퍼티**

   - 데이터 프로퍼티 어트리뷰트 / 프로퍼티 디스크립터 객체의 프로퍼티
     - [[Value]] / value
       - 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값.
       - 프로퍼티 키를 통해 값을 변경하면 [[Value]]에 값을 재할당함. 프로퍼티가 없으면 동적으로 생성하면 생성된 프로퍼티에 [[Value]]에 저장
     - [[Writable]] / writable
       - 프로퍼티 값의 변경 가능 여부를 불리언 값으로 가짐.
       - false 인 경우 해당 프로퍼티의 [[Value]]의 값을 변경할 수 없는 읽기 전용 프로퍼티가 됨.
     - [[Enumerable]] / enumerable
       - 프로퍼티의 열거 가능 여부 불리언
       - false 인 경우 for … in 이나 Object.keys 메서드 등으로 열겨 불가능
     - [[Configurable]] / configurable
       - 프로퍼티의 재정의 가능 여부 불리언
       - false 인 경우 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지된다.
       - 단, [[Writable]] 이 true 인 경우 [[Value]] 의 변경과 [[Writable]]을 false로 변경은 허용된다.
   - 프로퍼티가 처음 생성할 때 value 제외한 나머지 3 값 모두 true 로 초기화 된다. value는 프로퍼티 값으로 초기화. 동적으로 추가해도 마찬가지

1. **접근자 프로퍼티**

   - 🌟 자체적으로 값을 가지지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티이다.
   - 접근자 프로퍼티 어트리뷰트 / 프로퍼티 디스크립터 객체의 프로퍼티
     - [[Get]] / get
       - 접근자 프로퍼티를 통해 프로퍼티의 값을 읽을 때 호출되는 접근자 함수
       - 즉, 접근자 프로퍼티 키로 프로퍼티 값에 접근하면 프로퍼티 어트리뷰트 [[Get]] 의 값, 즉 getter 함수가 호출되고 그 결과가 프로퍼티 값으로 반환된다.
     - [[Set]] / set
       - 접근자 프로퍼티를 통해 프로퍼티 값을 저장할 때 호출되는 접근자 함수
       - 접근자 프로퍼티 키로 값을 저장하면 프로퍼티 어트리뷰트 [[Set]]의 값, 즉 setter 함수가 호출되고 그 결과가 프로퍼티 값으로 저장된다.
     - [[Enumerable]] / enumerable
       - 데이터 프로퍼티와 동일
     - [[Configurable]] / configurable
       - 데이터 프로퍼티와 동일
   - 접근자 함수는 getter/setter 함수 라고도 부른다. 둘 다 정의할 수도 하나만 정의할 수도 있다.

   ```jsx
   const person = {
     firstName: "Unmgo",
     lastName: "Lee",

     get fullName() {
       return `${this.firstName} ${this.lastName}`;
     },

     set fullName(name) {
       [this.firstName, this.lastName] = name.split(" ");
     },
   };

   console.log(person.firstName, person.lastName, person.fullName); // Unmgo Lee Unmgo Lee
   person.fullName = "JeongHwan Oh";
   console.log(person.fullName); // JeongHwan Oh

   console.log(Object.getOwnPropertyDescriptors(person));
   ```

   ```jsx
   {
     firstName: {
       value: 'JeongHwan',
       writable: true,
       enumerable: true,
       configurable: true
     },
     lastName: { value: 'Oh', writable: true, enumerable: true, configurable: true },
     fullName: {
       get: [Function: get fullName],
       set: [Function: set fullName],
       enumerable: true,
       configurable: true
     }
   }
   ```

   - get, set 이 붙은 메서드가 getter, setter 함수이다. 접근자 프로퍼티는 자체적으로 값을 가지지 않으며 다만 데이터 프로퍼티의 값을 읽거나 저장할 때 관여할 뿐이다.
   - 🌟 접근자 프로퍼티 fullName 프로퍼티 값에 접근할 때, 내부적 동작 과정
     1. 프로퍼티 키가 유효한지 확인 → fullName 은 문자열이므로 유효
     2. 프로토타입 체인에서 프로퍼티 검색 → person 에 fullName 프로퍼티 존재.
     3. 검색된 fullName 프로퍼티가 데이터 프로퍼티인지 접근자 프로퍼티인지 확인 → fullName 은 접근자 프로퍼티
     4. 접근자 프로퍼티 fullName 프로퍼티 어트리뷰트 [[Get]], 즉 getter 함수를 호출하여 결과 반환
   - 접근자 프로퍼티와 데이터 프로퍼티의 프로퍼티 디스크립터 객체의 프로퍼티가 다른 것으로 구별할 수 있다

# 16.4 프로퍼티 정의

---

- Object.defineProperty 메서드를 사용해 프로퍼티 어트리뷰트를 정의할 수 있다. 인수로는 객체의 이름, 프로퍼티 키 문자열, 프로퍼티 디스크립터 객체 전달.
- Object.defineProperty 메서드로 프로퍼티 정의할 때, 프로퍼티 디스크립터 객체의 프로퍼티를 일부 생략할 수 있는데, 이 때 아래와 같이 기본값이 정의된다.
  - value : undefined
  - get : undefined
  - set : undefined
  - writable : false
  - enemerable : false
  - configurable : false

```jsx
const person = {};

Object.defineProperty(person, "firstName", {
  value: "JeongHwan",
  writable: true,
  enumerable: true,
  configurable: true,
});

Object.defineProperty(person, "lastName", {
  value: "Oh",
});

console.log(Object.getOwnPropertyDescriptors(person));
console.log(Object.keys(person)); // lastName 은 반복 불가.
person.lastName = "Kim"; // 변경 불가
delete person.lastName; // 재정의 불가, 삭제 불가
console.log(Object.getOwnPropertyDescriptor(person, "lastName"));

Object.defineProperty(person, "fullName", { // 접근자 프로퍼티 정의
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  set(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
  enumerable: true,
  configurable: true,
});

console.log(person.fullName);

console.log(Object.getOwnPropertyDescriptors(person));

// 결과
{
  firstName: {
    value: 'JeongHwan',
    writable: true,
    enumerable: true,
    configurable: true
  },
  lastName: {
    value: 'Oh',
    writable: false,
    enumerable: false,
    configurable: false
  }
}
[ 'firstName' ]
{
  value: 'Oh',
  writable: false,
  enumerable: false,
  configurable: false
}
JeongHwan Oh
```

- Object.definePropertise 메서드로 한 번에 여러 개의 프로퍼티를 정의할 수도 있다.

```jsx
const person = {};

Object.defineProperties(person, {
  firstName: {
    value: "JeongHwan",
    writable: true,
    enumerable: true,
    configurable: true,
  },
  lastName: {
    value: "Oh",
  },
  fullName: {
    get() {
      return `${this.firstName} ${this.lastName}`;
    },
    set(name) {
      [this.firstName, this.lastName] = name.split(" ");
    },
    enumerable: true,
    configurable: true,
  },
});

console.log(Object.getOwnPropertyDescriptors(person));
console.log(Object.keys(person));
person.lastName = "Kim";
delete person.lastName;
console.log(Object.getOwnPropertyDescriptor(person, "lastName"));
console.log(person.fullName);
```

# 16.5 객체 변경 방지

---

- 객체는 변경 가능한 값이므로 재할당 없이 직접 변경할 수 있다. 즉, 프로퍼티를 추가하거나 삭제할 수 있고, 프로퍼티 값을 갱신할 수 있고, Object.definePropery 를 통해 정의 재정의 할 수 있다.
- 자바스크립트는 객체의 변경을 방지하는 메서드를 제공한다. 금지 강도가 다르다.

| 구분 | 메서드 | 프로퍼티
추가 | 프로퍼티
삭제 | 프로퍼티
값 읽기 | 프로퍼티
값 쓰기 | 프로퍼티
어트리뷰트 재정의 |
| --- | --- | --- | --- | --- | --- | --- |
| 객체 확장
금지 | preventExtensios | X | O | O | O | O |
| 객체 밀봉 | seal | X | X | O | O | X |
| 객체 동결 | freeze | X | X | O | X | X |

1. **객체 확장 금지**

   - **Object.preventExtensions() 로 프로퍼티의 추가를 금지한다**.
   - 동적 추가와 Object.defineProperty 를 통한 추가를 모두 금지한다.
   - 확장 가능 여부를 Object.isExtensible 메서드로 확인 가능

   ```jsx
   const person = { name: "Lee" };
   console.log(Object.isExtensible(person));
   Object.preventExtensions(person);
   console.log(Object.isExtensible(person));
   person.age = 20; // 확장 불기
   console.log(person);

   delete person.name; // 삭제는 가능
   console.log(person);

   Object.defineProperty(person, "age", { value: 30 }); // 불가, 에러 발생
   // TypeError: Cannot define property age, object is not extensible
   ```

1. **객체 밀봉**

   - **Object.seal() 을 통해 객체를 밀봉한다. 밀봉하면 읽기와 쓰기만 가능하다.**
   - Object.isSealed를 통해 밀봉 여부 확인 가능

   ```jsx
   const person = { name: "Lee" };
   console.log(Object.isSealed(person));
   Object.seal(person);
   console.log(Object.isSealed(person));

   console.log(Object.getOwnPropertyDescriptors(person)); // configurable 이 false 임

   person.age = 20; // 추가 금지
   console.log(person);

   delete person.name; // 삭제 금지
   console.log(person);

   person.name = "Kim"; // 쓰기 가능
   console.log(person);

   Object.defineProperty(person, "name", { configurable: true }); // 재정의 금지
   // TypeError: Cannot redefine property: name
   ```

   ```jsx
   false
   true
   {
     name: {
       value: 'Lee',
       writable: true,
       enumerable: true,
       configurable: false
     }
   }
   ```

1. **객체 동결**

   - **Object.freeze() 메서드를 통해 객체를 동결한다. 오직 읽기만 가능!!!**
   - isFrozen 으로 확인 가능

   ```jsx
   const person = { name: "Lee" };
   console.log(Object.isFrozen(person));
   Object.freeze(person);
   console.log(Object.isFrozen(person));

   console.log(Object.getOwnPropertyDescriptors(person)); // writable 과 configurable 이 false 임

   person.age = 20; // 추가 금지
   console.log(person);

   delete person.name; // 삭제 금지
   console.log(person);

   person.name = "Kim"; // 쓰기 금지
   console.log(person);

   Object.defineProperty(person, "name", { configurable: true }); // 재정의 금지
   ```

   ```jsx
   false
   true
   {
     name: {
       value: 'Lee',
       writable: false,
       enumerable: true,
       configurable: false
     }
   }
   { name: 'Lee' }
   { name: 'Lee' }
   { name: 'Lee' }
   C:\Users\82105\Desktop\오정환\algorithm\JavaScript\deepdive.js:17
   Object.defineProperty(person, "name", { configurable: true }); // 재정의 금지
          ^

   TypeError: Cannot redefine property: name
   ```

1. **불변 객체**

   - 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지된다. 중첩 객체까지는 영향을 주지 못한다. → Object.freeze 로 동결해도 중첩 객체까지 동결할 수 없다.
   - **🌟🌟🌟 중첩 객체까지 동결하여 변경이 불가능한 읽기 전용의 불변 객체를 구현하기 위해서는 객체를 값으로 같은 모든 프로퍼티에 대해 재귀적으로 Object.freeze 를 적용해줘야 한다. (면접 단골 문제)**

   ```jsx
   function deepFreeze(target) {
     if (target && typeof target === "object" && !Object.isFrozen(target)) {
       Object.freeze(target);

       Object.keys(target).forEach((key) => deepFreeze(target[key]));
     }

     return target;
   }

   const person = {
     name: "Lee",
     address: { city: "seoul" },
   };

   deepFreeze(person);

   console.log(Object.isFrozen(person)); // true
   console.log(Object.isFrozen(person.address)); // true
   person.address.city = "Busan"; // 불변 객체이므로 변경 불가
   console.log(person); // { name: 'Lee', address: { city: 'seoul' } }
   ```
