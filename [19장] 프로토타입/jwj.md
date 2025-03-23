# 19장 프로토타입

자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍을 지원하는 멀티 패러다임 프로그래밍 언어다.

## 19.1 객체지향 프로그래밍

- 객체지향 프로그래밍: 기존의 명령형 프로그래밍의 절차지향적 관점에서 벗어나 프로그램을 여러 개의 독립적인 단위, 즉 객체로 보는 프로그래밍 패러다임

**속성(attribute, property)**: 실세계의 특징이나 성질을 나타낸다.

**추상화**: 다양한 속성 중에 필요한 속성만 간추려 표현하는 행위를 나타낸다.

객체는 상태(프로퍼티)와 동작(메서드)을 하나의 논리적인 단위로 묶은 것이다.

## 19.2 상속과 프로토타입

상속: 어떤 객체의 프로퍼티나 메서드를 다른 객체가 상속받아 사용할 수 있는 것을 의미한다.

상속으로 불필요한 중복을 제거할 수 있다.

```js
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radius ** 2;
  };
}
...
console.log(circle1.getArea === circle2.getArea); // false
```

위 코드는 인스턴스를 생성할 때마다 `getArea` 함수를 계속해서 생성해낸다. 모든 인스턴스가 사용해야 하는 메서드가 동일하다면, 하나만 생성해서 메모리 낭비를 줄일 수 있을 것이다.

```js
Circle.prototype.getArea = function () {
  return Math.PI * this.radius ** 2;
};
...
console.log(circle1.getArea === circle2.getArea); // true
```

하나의 프로토타입 객체를 통해 동일한 하나의 메서드를 모든 인스턴스가 공유할 수 있다.

## 19.3 프로토타입 객체

프로토타입은 상속을 구현하기 위해 사용된다. **어떤 객체의 상위(부모) 객체 역할을 하는 객체**다.

모든 객체는 `[[Prototype]]` 내부 슬롯을 가지며, 이 내부 슬롯 값은 프로토타입의 참조다. 저장되는 값은 객체 생성 방식에 의해 결정된다. 즉, 객체 생성 시 프로토타입이 결정되고 저장된다.

예를 들어,

- 객체 리터럴로 생성하면 `Object.prototype`를 프로토타입으로 가진다.
- 생성자 함수에 의해 생성된 객체는 생성자 함수의 `prototype` 프로퍼티에 바인딩되어 있는 객체를 프로토타입으로 가진다.

객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입 객체(`[[Prototype]]` 내부 슬롯의 값)에 접근할 수 있다.

프로토타입은 자신의 `constructor` 프로퍼티를 통해 생성자 함수에 접근할 수 있다. <-> 생성자 함수는 자신의 `prototype` 프로퍼티를 통해 프로토타입에 접근할 수 있다.

### **proto** 접근자 프로퍼티

모든 객체는 `__proto__` 접근자 프로퍼티를 통해 자신의 프로토타입 객체(`[[Prototype]]` 내부 슬롯의 값)에 간접적으로 접근할 수 있다.

```js
const obj = {};
const parent = { x: 1 };

// getter 함수인 get __proto__가 호출되어 obj 객체의 프로토타입을 취득
obj.__proto__;
// setter함수인 set __proto__가 호출되어 obj 객체의 프로토타입을 교체
obj.__proto__ = parent;

console.log(obj.x); // 1
```

`__proto__` 접근자 프로퍼티는 **객체 자신이 소유하는 프로퍼티가 아니라** `Object.prototype`의 프로퍼티다.

💡`__proto__`를 통해 프로토타입에 접근하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다. 프로토타입 체인은 단방향 연결 리스트로 구현되어야 한다. 순환 참조하게 되면 프로토타입의 종점이 존재하지 않게 되기 때문에 무한 루프에 빠지게 된다. (사이클이 생기면 안됨!)

`__proto__`는 ES5까지 비표준이었으나 이미 사용하고 있는 브라우저가 있었기 때문에 브라우저간 호환성을 위해 ES6부터 표준화되었다.

⚠️`__proto__`를 직접 사용하는 건 권장되지 않는다. `__proto__`를 사용할 수 없는 객체가 있기 때문이다. (그 예시로 프로토타입 체인의 종점인 경우가 있다.)

대신 `Object.getPropertyOf` 메서드(ES5 이상)를 사용하는 것이 권장되고, 프로토타입을 교체하고 싶은 경우 `Object.setPropertyOf`(ES6 이상) 메서드를 사용하는 것이 권장된다.

```js
const obj = {};
const parent = { x: 1 };

// obj 객체의 프로토타입을 취득
Object.getPrototypeOf(obj); // obj.__proto__;
// obj 객체의 프로토타입을 교체
Object.setPrototypeOf(obj, parent); // obj.__proto__ = parent;

console.log(obj.x); // 1
```

### 함수 객체의 prototype 프로퍼티

함수 객체만이 소유하는 `prototype` 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.

일반 객체는 `prototype` 프로퍼티를 가지지 않는다.

생성자 함수로 호출할 수 없는 함수, 즉 non-constructor인 화살표 함수와 ES6 메서드 축약 표현으로 정의한 메서드 역시 `prototype` 프로퍼티가 없으며 프로토타입도 생성하지 않는다.

`__proto__` 접근자 프로퍼티와 `prototype` 프로퍼티는 결국 동일한 프로토타입을 가리킨다. 하지만 사용하는 주체가 다르다.

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 결국 Person.prototype과 me.__proto__는 결국 동일한 프로토타입을 가리킨다.
console.log(Person.prototype === me.__proto__); // true
```

| 구분                        | 소유        | 값                | 사용 주체   | 사용 목적                                                                    |
| --------------------------- | ----------- | ----------------- | ----------- | ---------------------------------------------------------------------------- |
| `__proto__` 접근자 프로퍼티 | 모든 객체   | 프로토타입의 참조 | 모든 객체   | 객체가 자신의 프로토타입에 접근 또는 교체하기 위해 사용                      |
| `prototype` 프로퍼티        | constructor | 프로토타입의 참조 | 생성자 함수 | 생성자 함수가 자신이 생성할 객체(인스턴스)의 프로토타입을 할당하기 위해 사용 |

### 프로토타입의 constructor 프로퍼티와 생성자 함수

모든 프로토타입은 `constructor` 프로퍼티를 갖는다. 이 `constructor` 프로퍼티는 `prototype` 프로퍼티로 자신을 참조하고 있는 생성자 함수를 가리킨다.

```js
// me 객체의 생성자 함수는 Person이다.
console.log(me.constructor === Person); // true
```

`me` 객체에는 `constructor`가 없지만 `Person.prototype`에는 `constructor`가 있다. `me` 객체는 프로토타입인 `Person.prototype`의 `constructor` 프로퍼티를 상속받아 사용할 수 있다.

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토 타입

생성자 함수로 생성된 인스턴스는 프로토타입의 `constructor` 프로퍼티를 통해 생성자 함수로 연결된다.

```js
// obj 객체를 생성한 생성자 함수는 Object다.
const obj = new Object();
console.log(obj.constructor === Object); // true

// add 함수 객체를 생성한 생성자 함수는 Function이다.
const add = new Function("a", "b", "return a + b");
console.log(add.constructor === Function); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// me 객체를 생성한 생성자 함수는 Person이다.
const me = new Person("Lee");
console.log(me.constructor === Person); // true
```

리터럴 표기법에 의한 객체 생성 방식과 같이 명시적으로 `new` 연산자와 함께 생성자 함수를 호출해 인스턴스를 생성하지 않는 객체 생성 방식도 있다.

리터럴 표기법으로 생성한 객체도 프로토타입이 존재한다. 하지만 프로토타입의 `constructor` 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수 없다.

그렇지만 **프로토타입과 생성자 함수는 단독으로 존재할 수 없고, 언제나 쌍으로 존재한다.**

객체 리터럴과 `Object` 생성자 함수로 생성한 객체는 **생성 과정에 있어서의 차이만 있을뿐** 객체로서 동일한 특성을 갖는다.

함수 리터럴과 `Function` 생성자 함수로 생성한 함수는 **생성 과정, 스코프, 클로저 등의 차이가 있지만** 결국 동일한 특성을 갖는다.

프로토타입의 `constructor` 프로퍼티를 통해 연결되어 있는 생성자 함수를, 리터럴 표기법으로 생성한 객체를 생성한 생성자 함수로 큰 무리는 없다.

## 19.5 프로토타입의 생성 시점

💡프로토타입은 생성자 함수가 생성되는 시점에 같이 생성된다. (언제나 쌍으로 존재하기 때문에)

### 사용자 정의 생성자 함수와 프로토타입 생성 시점

생성자 함수로써 호출할 수 있는 함수, 즉 `constructor`는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 생성된다.

사용자 정의 생성자 함수는 자신이 평가되어 함수 객체로 생성되는 시점에 프로토타입이 생성되며, 생성된 프로토타입의 프로토타입은 언제나 `Object.prototype`이다.

### 빌트인 생성자 함수와 프로토타입 생성 시점

마찬가지로 생성자 함수가 생성되는 시점, 즉 전역 객체가 생성되는 시점에 생성된다.

객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다. 이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 `[[Prototype]]` 내부 슬롯에 할당된다.
이로써 생성된 객체는 프로토타입을 상속받는다.

## 19.6 객체 생성 방식과 프로토타입의 결정

객체 생성 방식에는 다양한 방법이 있고, 각 방식은 세부 생성 방식의 차이는 있으나 모두 추상 연산 `OrdianryObjectCreate`에 의해 생성된다.

1. `OrdianryObjectCreate`는 필수적으로 자신이 생성할 객체의 프로토타입을 인수로 전달받음
2. 자신이 생성할 객체에 추가할 프로퍼티 목록을 옵션으로 전달
3. 빈 객체 생성 후, 객체에 추가할 프로퍼티 목록이 인수로 전달된 경우 프로퍼티를 객체에 추가
4. 인수로 전달 받은 프로토타입을 자신이 생성한 객체의 `[[Prototype]]` 내부 슬롯에 할당
5. 생성한 객체 반환

즉 인수에 의해 프로토타입이 결정되는데, 이 인수는 객체 생성 시점에 객체 생성 방식에 의해 결정된다.

### 객체 리터럴에 의해 생성된 객체

인수로 `Object.prototype`이 전달된다.

### Object 생성자 함수에 의해 생성된 객체

인수 없이 호출하면, 인수로 `Object.prototype`이 전달된다.

이 방식과 객체 리터럴과 차이점은 일단 빈 객체를 먼저 생성한 이후 프로퍼티를 추가한다는 점이다.

### 생성자 함수에 의해 생성된 객체

인수로 생성자 함수의 `prototype` 프로퍼티에 바인딩되어 있는 객체가 전달된다.

## 19.7 프로토타입 체인

프로토타입의 프로토타입은 언제나 `Object.prototype`이다.

자바스크립트는 객체의 프로퍼티에 접근할 때 해당 객체에 접근하려는 프로퍼티를 찾을 때까지 `[[Property]]` 내부 슬롯의 참조를 따라 상위 프로토타입을 순차적으로 검색한다. 이것이 프로토타입 체인이다.

`Object.prototype`은 프로토타입 체인의 최상위 객체이자 종점이다. 이것의 상위 프로토타입은 `null`이다.

⚠️검색할 수 없는 경우, `undefiend`를 반환하므로 주의하자.

반면에 **식별자는 스코프 체인에 의해 검색된다.**

## 19.8 오버라이딩과 프로퍼티 섀도잉

자바스크립트에서도 역시 인스턴스에서 메서드를 재정의해줄 수 있는데(오버라이딩), 상속 관계에 의해 상위 메서드가 가려지는 현상을 프로퍼티 섀도잉이라 한다.

> 자바스크립트는 오버로딩을 지원하지 않지만 `arguments` 객체를 사용하여 구현할 수 있다.

하위 객체를 통해 프로토타입의 메서드를 변경 또는 삭제할 수 없다. 다시 말해 `get`은 허용되나 `set`은 허용하지 않는다.

## 19.9 프로토타입의 교체

프로토타입은 임의의 다른 객체로 교체할 수 있다.

### 생성자 함수에 의한 프로토타입의 교체

```js
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  },
};

// ① me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee
```

이렇게 교체하면 `prototype`에 `constructor` 프로퍼티가 사라지게 된다. 이 프로퍼티는 자바스크립트가 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티기 때문이다.

````js
const Person = (function () {
  function Person(name) {
    this.name = name;
  }

  // 생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 교체
  Person.prototype = {
    // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
    constructor: Person,
    sayHello() {
      console.log(`Hi! My name is ${this.name}`);
    },
  };

  return Person;
})();

const me = new Person("Lee");

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false
```;
````

이렇게 되살려 줄 수 있다.

### 인스턴스에 의한 프로토타입의 교체

생성자 함수의 `prototype` 프로퍼티뿐만 아니라 인스턴스의 `__proto__` 접근자 프로퍼티를 통해 프로토타입을 교체할 수 있다.

이는 **미래에 생성할 인스턴스의 프로토타입을 교체한다는 것을 의미**한다.

```js
function Person(name) {
  this.name = name;
}

const me = new Person("Lee");

// 프로토타입으로 교체할 객체
const parent = {
  // constructor 프로퍼티와 생성자 함수 간의 연결을 설정
  constructor: Person,
  sayHello() {
    console.log(`Hi! My name is ${this.name}`);
  },
};

// 생성자 함수의 prototype 프로퍼티와 프로토타입 간의 연결을 설정
Person.prototype = parent;

// me 객체의 프로토타입을 parent 객체로 교체한다.
Object.setPrototypeOf(me, parent);
// 위 코드는 아래의 코드와 동일하게 동작한다.
// me.__proto__ = parent;

me.sayHello(); // Hi! My name is Lee

// constructor 프로퍼티가 생성자 함수를 가리킨다.
console.log(me.constructor === Person); // true
console.log(me.constructor === Object); // false

// 생성자 함수의 prototype 프로퍼티가 교체된 프로토타입을 가리킨다.
console.log(Person.prototype === Object.getPrototypeOf(me)); // true
```

⚠️생성자 함수에 의한 교체와 다른 점은 `Person` 생성자 함수의 `prototype` 프로퍼티가 교체된 프로토타입을 가리키지 않는다는 점이다.

프로토타입 교체를 통해 상속 관계를 동적으로 변경하는 일은 번거롭다. 따라서 **직접 교체하지 않는 편이 좋다**. "직접 상속"이나 클래스를 이용하면 간편하고 직관적이다.

## 19.10 instanceof 연산자

```js
객체 instanceof 생성자 함수
```

우변의 생성자 함수의 `prototype`에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 참, 그렇지 않으면 거짓으로 평가된다.

## 19.11 직접 상속

### Object.create에 의한 직접 상속

명시적으로 프로토타입을 지정해 새로운 객체를 생성한다.

첫 번째 인자로 **프로토타입 객체**, 두 번째 인자로 **생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이루어진 객체**를 전달한다.

```js
// 프로토타입이 null인 객체를 생성한다. 생성된 객체는 프로토타입 체인의 종점에 위치한다.
// obj → null
let obj = Object.create(null);
console.log(Object.getPrototypeOf(obj) === null); // true
// Object.prototype을 상속받지 못한다.
console.log(obj.toString()); // TypeError: obj.toString is not a function

// obj → Object.prototype → null
// obj = {};와 동일하다.
obj = Object.create(Object.prototype);
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

// obj → Object.prototype → null
// obj = { x: 1 };와 동일하다.
obj = Object.create(Object.prototype, {
  x: { value: 1, writable: true, enumerable: true, configurable: true },
});
// 위 코드는 다음과 동일하다.
// obj = Object.create(Object.prototype);
// obj.x = 1;
console.log(obj.x); // 1
console.log(Object.getPrototypeOf(obj) === Object.prototype); // true

const myProto = { x: 10 };
// 임의의 객체를 직접 상속받는다.
// obj → myProto → Object.prototype → null
obj = Object.create(myProto);
console.log(obj.x); // 10
console.log(Object.getPrototypeOf(obj) === myProto); // true

// 생성자 함수
function Person(name) {
  this.name = name;
}

// obj → Person.prototype → Object.prototype → null
// obj = new Person('Lee')와 동일하다.
obj = Object.create(Person.prototype);
obj.name = "Lee";
console.log(obj.name); // Lee
console.log(Object.getPrototypeOf(obj) === Person.prototype); // true`
```

장점은 다음과 같다.

1. `new` 연산자 없이 객체를 생성
2. 프로토타입을 지정하면서 객체를 생성
3. 객체 리터럴에 의해 생성된 객체도 상속 받을 수 있음

⚠️ESLint에서는 예제처럼 `Object.prototype`의 빌트인 메서드를 객체가 직접 호출하는 것을 권장하지 않는데, 이는 `Object.create` 메서드를 통해 프로토타입 체인의 종점에 위치하는 객체를 생성할 수 있기 때문이다. 이런 경우 빌트인 메서드를 이용할 수 없다.

💡`Object.prototype`의 빌트인 메서드는 간접적으로 호출하는 것이 좋다.

### 객체 리터럴 내부에서 **proto**에 의한 직접 상속

위와 같은 방식은 여러 장점도 있지만 프로퍼티를 정의하는 것이 번거롭다.

ES6부터 객체 리터럴 내부에 `__proto__` 접근자 프로퍼티를 사용해 직접 상속을 구현할 수 있다.

```js
const myProto = { x: 10 };

// 객체 리터럴에 의해 객체를 생성하면서 프로토타입을 지정하여 직접 상속받을 수 있다.
const obj = {
  y: 20,
  // 객체를 직접 상속받는다.
  // obj → myProto → Object.prototype → null
  __proto__: myProto,
};
/* 위 코드는 아래와 동일하다.
const obj = Object.create(myProto, {
  y: { value: 20, writable: true, enumerable: true, configurable: true }
});
*/

console.log(obj.x, obj.y); // 10 20
console.log(Object.getPrototypeOf(obj) === myProto); // true
```

### 19.12 정적 프로퍼티/메서드

정적 프로퍼티/메서드는 **생성자 함수에서 직접 참조/호출할 수 있는 프로퍼티/메서드**를 말한다.

정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.

```js
// 생성자 함수
function Person(name) {
  this.name = name;
}

// 프로토타입 메서드
Person.prototype.sayHello = function () {
  console.log(`Hi! My name is ${this.name}`);
};

// 정적 프로퍼티
Person.staticProp = "static prop";

// 정적 메서드
Person.staticMethod = function () {
  console.log("staticMethod");
};

const me = new Person("Lee");

// 생성자 함수에 추가한 정적 프로퍼티/메서드는 생성자 함수로 참조/호출한다.
Person.staticMethod(); // staticMethod

// 정적 프로퍼티/메서드는 생성자 함수가 생성한 인스턴스로 참조/호출할 수 없다.
// 인스턴스로 참조/호출할 수 있는 프로퍼티/메서드는 프로토타입 체인 상에 존재해야 한다.
me.staticMethod(); // TypeError: me.staticMethod is not a function
```

`this`를 참조하지 않는 생성자 함수의 메서드는 정적 메서드로 동작한다.

## 19.13 프로퍼티 존재 확인

### in 연산자

```js
키 in 객체;
```

ES6에서는 `Reflect.has` 메서드를 사용할 수 있다.

```js
Reflect.has(person, "name");
```

### Object.prototype.hasOwnProperty 메서드

```js
person.hasOwnProperty("name");
```

상속받은 프로토타입의 프로퍼티의 경우 `false`를 반환한다.

## 19.14 프로퍼티 열거

### for...in 문

```js
for (변수선언문 in 객체) { ... }
```

```js
const person = {
  name: "Lee",
  address: "Seoul",
};

// for...in 문의 변수 key에 person 객체의 프로퍼티 키가 할당된다.
for (const key in person) {
  console.log(key + ": " + person[key]);
}
// name: Lee
// address: Seoul
```

프로토타입의 프로퍼티까지 열거하지만, 열거될 수 없는 `toString`(`[[Enumerable]]`이 `false`인 경우) 같은 프로퍼티는 열거되지 않는다.

심벌도 열거되지 않는다.

자신의 프로퍼티만 열거하려면 `Object.prototype.hasOwnProperty` 메서드를 사용해 객체 자신의 프로퍼티인지 확인해야만 한다.

⚠️`for...in` 문은 프로퍼티의 순서를 보장하지 않기 때문에 주의해야 한다. 하지만 대부분 모던 브라우저는 순서를 보장하고 숫자(사실은 문자열)인 프로퍼티 키에 대해서는 정렬을 실시한다.

배열에는 `for 문`, `for...of` 문, `Array.prototype.forEach` 메서드를 사용하기를 권장한다.

```js
const arr = [1, 2, 3];
arr.x = 10; // 배열도 객체이므로 프로퍼티를 가질 수 있다.

for (const i in arr) {
  // 프로퍼티 x도 출력된다.
  console.log(arr[i]); // 1 2 3 10
}

// arr.length는 3이다.
for (let i = 0; i < arr.length; i++) {
  console.log(arr[i]); // 1 2 3
}

// forEach 메서드는 요소가 아닌 프로퍼티는 제외한다.
arr.forEach((v) => console.log(v)); // 1 2 3

// for...of는 변수 선언문에서 선언한 변수에 키가 아닌 값을 할당한다.
for (const value of arr) {
  console.log(value); // 1 2 3
}
```

### Object.keys/values/entries 메서드

객체 자신의 고유 프로퍼티를 열거하기 위해서는 `Object.keys/values/entries` 메서드를 사용해야 한다.

```js
const person = {
  name: "Lee",
  address: "Seoul",
  __proto__: { age: 20 },
};

console.log(Object.keys(person)); // ["name", "address"]
console.log(Object.values(person)); // ["Lee", "Seoul"]
console.log(Object.entries(person)); // [["name", "Lee"], ["address", "Seoul"]]

Object.entries(person).forEach(([key, value]) => console.log(key, value));
/*
name Lee
address Seoul
*/
```
