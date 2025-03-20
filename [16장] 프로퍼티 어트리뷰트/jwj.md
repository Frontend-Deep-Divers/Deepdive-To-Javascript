# 16장 프로퍼티 어트리뷰트

## 16.1 내부 슬롯과 내부 메서드

내부 슬롯과 내부 메서드은 자바스크립트 엔진의 구현 알고리즘을 설명하기 위해 **ES 사양에서 사용하는 의사 프로퍼티와 의사 메서드**다. ES 사양에서 등장하는 **이중 대괄호(`[[...]]`)**로 감싼 이름들이 내부 슬롯과 내부 메서드다.

이들은 ES 사양에 정의된 대로 구현되어 자바스크립트 엔진에서 실제 동작한다. 하지만 개발자가 직접 접근할 수 있도록 공개된 객체의 프로퍼티는 아니다. 단 일부 간접적으로 접근할 수 있는 수단을 제공하기는 한다.

모든 객체는 `[[Prototype]]`이라는 내부 슬롯을 갖는다. 이는 `__proto__`를 통해 간접적으로 접근할 수 있다.

```js
const obj = {};

obj.[[Prototype]] // 직접 접근 불가
obj.__proto__; // Object.prototype
```

## 16.2 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체

**자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의한다.**

프로퍼티의 상태는 값(value), 값의 갱신 가능 여부(writable), 열거 가능 여부(enumerable), 재정의 가능 여부(configurable)를 말한다.

프로퍼티 어트리뷰트는 내부 상태 값(meta-property)인 내부 슬롯 `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]`이다. 따라서 직접 접근할 수는 없지만, `Object.getOwnPropertyDescriptor` 메서드를 통해 간접적으로 확인할 수 있다.

```js
const person = {
  name: "Lee",
  age: "20",
};

// 프로퍼티 어트리뷰트 정보를 제공하는 프로퍼티 디스크립터 객체를 반환한다.
console.log(Object.getOwnPropertyDescriptor(person, "name"));
// {value: "Lee", writable: true, enumerable: true, configurable: true}

console.log(Object.getOwnPropertyDescriptors(person));
/*
{
  name: {value: "Lee", writable: true, enumerable: true, configurable: true}
  age: {value: "20", writable: true, enumerable: true, configurable: true}
}
*/
```

## 16.3 데이터 프로퍼티와 접근자 프로퍼티

- 데이터 프로퍼티: 키와 값으로 구성된 일반적인 프로퍼티다. 지금까지 살펴본 모든 프로퍼티에 해당한다.
- 접근자 프로퍼티: 자체적으로는 값을 갖고 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 호출되는 접근자 함수로 구성된 프로퍼티다.

데이터 프로퍼티는 `[[Value]]`, `[[Writable]]`, `[[Enumerable]]`, `[[Configurable]]`를 갖는다. 프로퍼티 어트리뷰트는 프로퍼티 생성 시 자동으로 정의된다.

- Value: 키의 값
- Writable: 값의 변경 가능 여부(`false` 시 read-only 프로퍼티가 된다)
- Enumerable: 열거 가능 여부(`false` 시 `Object.keys` 메서드 등 사용 불가)
- Configurable: 재정의 가능 여부(`false` 시 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값 변경이 금지된다. 단, Writable이 `true`인 경우 Value의 변경과 Writable을 `false`로 변경은 가능)

접근자 프로퍼티는 다음과 같은 프로퍼티 어트리뷰트를 갖는다.

- Get: 데이터 프로퍼티를 읽을 때 호출되는 접근자 함수다. (getter 함수)
- Set: 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수다. (setter 함수)
- Enumerable: 데이터 프로퍼티의 것과 같다.
- Configurable: 데이터 프로퍼티의 것과 같다.

```js
const person = {
  // 데이터 프로퍼티
  firstName: "Ungmo",
  lastName: "Lee",

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    // 배열 디스트럭처링 할당
    [this.firstName, this.lastName] = name.split(" ");
  },
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(person.firstName + " " + person.lastName); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = "Heegun Lee";
console.log(person); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName); // Heegun Lee

// firstName은 데이터 프로퍼티다.
let descriptor = Object.getOwnPropertyDescriptor(person, "firstName");
console.log(descriptor);
// {value: "Heegun", writable: true, enumerable: true, configurable: true}

// fullName은 접근자 프로퍼티다.
descriptor = Object.getOwnPropertyDescriptor(person, "fullName");
console.log(descriptor);
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}
```

## 16.4 프로퍼티 정의

프로퍼티 정의는 새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것을 말한다.

객체의 프로퍼티의 동작 방식을 구체적으로 설정해줄 때 사용한다.

`Object.defineProperty` 메서드를 이용하면 프로퍼티의 어트리뷰트를 정의할 수 있다. 인자로 객체, 프로퍼티 키인 문자열, 프로퍼티 디스크립터 객체를 전달하면 된다.

`Object.defineProperties` 메서드로 여러 개의 프로퍼티를 한 번에 정의할 수도 있다.

```js
const person = {};

// 데이터 프로퍼티 정의
Object.defineProperty(person, "firstName", {
  value: "Ungmo",
  writable: true,
  enumerable: true,
  configurable: true,
});

// 프로퍼티를 생략하면 undefined 혹은 false로 설정된다.
Object.defineProperty(person, "lastName", {
  value: "Lee",
});

// 접근자 프로퍼티 정의
Object.defineProperty(person, "fullName", {
  // getter 함수
  get() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set(name) {
    [this.firstName, this.lastName] = name.split(" ");
  },
  enumerable: true,
  configurable: true,
});
```

## 16.5 객체 변경 방지

객체의 변경을 방지하는 메서드들도 존재한다.

| 구분           | 메서드                   | 프로퍼티<br>추가 | 프로퍼티<br>삭제 | 프로퍼티<br>값 읽기 | 프로퍼티<br>값 쓰기 | 프로퍼티 어트리뷰트<br>재정의 |
| -------------- | ------------------------ | :--------------: | :--------------: | :-----------------: | :-----------------: | :---------------------------: |
| 객체 확장 금지 | Object.preventExtensions |        ✕         |        ○         |          ○          |          ○          |               ○               |
| 객체 밀봉      | Object.seal              |        ✕         |        ✕         |          ○          |          ○          |               ✕               |
| 객체 동결      | Object.freeze            |        ✕         |        ✕         |          ○          |          ✕          |               ✕               |

이 변경 방지 메서드들은 얕은 변경 방지로 직속 프로퍼티만 변경이 방지되고 중첩 객체까지는 영향을 주지 못한다.

객체를 깊은 변경 방지하여 읽기 전용의 불변 객체로 만들기 위해서는 객체를 값으로 갖는 모든 프로퍼티에 대해 재귀적으로 `Object.freeze`를 호출해야 한다.

```js
function deepFreeze(target) {
  // 객체가 아니거나 동결된 객체는 무시하고 객체이고 동결되지 않은 객체만 동결한다.
  if (target && typeof target === "object" && !Object.isFrozen(target)) {
    Object.freeze(target);
    /*
      모든 프로퍼티를 순회하며 재귀적으로 동결한다.
      Object.keys 메서드는 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.
      ("19.15.2. Object.keys/values/entries 메서드" 참고)
      forEach 메서드는 배열을 순회하며 배열의 각 요소에 대하여 콜백 함수를 실행한다.
      ("27.9.2. Array.prototype.forEach" 참고)
    */
    Object.keys(target).forEach((key) => deepFreeze(target[key]));
  }
  return target;
}

const person = {
  name: "Lee",
  address: { city: "Seoul" },
};

// 깊은 객체 동결
deepFreeze(person);

console.log(Object.isFrozen(person)); // true
// 중첩 객체까지 동결한다.
console.log(Object.isFrozen(person.address)); // true

person.address.city = "Busan";
console.log(person); // {name: "Lee", address: {city: "Seoul"}}
```
