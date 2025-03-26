# 22장 `this`

## 22.1 `this` 키워드

객체지향 프로그래밍에서 동작을 나타내는 메서드는 자신이 속한 객체의 상태, 즉 프로퍼티를 참조하고 변경할 수 있어야 한다. 이때 **자신이 속한 객체를 가리키는 식별자를 참조할 수 있어야 한다**.

객체 리터럴로 생성한 객체는 메서드 내부에서 메서드 자신이 속한 객체를 가리키는 식별자를 재귀적으로 참조할 수 있다.

```js
const circle = {
  // 프로퍼티: 객체 고유의 상태 데이터
  radius: 5,
  // 메서드: 상태 데이터를 참조하고 조작하는 동작
  getDiameter() {
    // 이 메서드가 자신이 속한 객체의 프로퍼티나 다른 메서드를 참조하려면
    // 자신이 속한 객체인 circle을 참조할 수 있어야 한다.
    return 2 * circle.radius;
  },
};

console.log(circle.getDiameter()); // 10
```

객체 리터럴은 식별자에 할당되기 직전에 평가된다. 따라서 메서드가 호출될 시점에는 객체가 생성된 이후이기 때문에 메서드 내부에서 식별자를 참조할 수 있다.

그러나 자기 자신이 속한 객체를 재귀적으로 참조하는 건 일반적이지 않고 바람직하지 않다.

생성자 함수를 통해 인스턴스를 생성하고자 할 땐, 생성자 함수 정의 후 호출할 때까지 생성자 함수가 생성할 인스턴스를 가리키는 식별자를 알 수 없다. 자바스크립트는 `this` 식별자를 제공한다.

**`this`는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수(self-referencing variable)다. `this`를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.**

`this`는 자바스크립트 엔진에 의해 암묵적으로 생성되며, **코드 어디서든 참조할 수 있다**. 함수를 호출하면 `arguments` 객체와 `this`가 암묵적으로 함수 내부에 전달된다. `this` 역시 `arguments` 객체처럼 함수 내부에서 지역 변수처럼 사용할 수 있다. 단 `this`가 가리키는 값(`this` 바인딩)은 함수 호출 방식에 의해 동적으로 결정된다.

바인딩이란 식별자와 값을 연결하는 과정을 의미한다. 변수 선언은 변수 이름(식별자)과 확보된 메모리 공간의 주소를 바인딩하는 것이다. `this` 바인딩은 `this`(키워드지만 식별자의 역할을 한다)와 `this`가 가리킬 객체를 바인딩하는 것을 의미한다.

클래스 기반 언어에서 `this`는 언제나 클래스가 생성하는 인스턴스를 가리킨다.

💡하지만, **자바스크립트의 `this`는 함수가 호출되는 방식에 따라 `this`에 바인딩될 값, 즉 `this` 바인딩이 동적으로 결정된다.**

```js
// this는 어디서든지 참조 가능하다.
// 전역에서 this는 전역 객체 window를 가리킨다.
console.log(this); // window

function square(number) {
  // 일반 함수 내부에서 this는 전역 객체 window를 가리킨다.
  console.log(this); // window
  return number * number;
}
square(2);

const person = {
  name: "Lee",
  getName() {
    // 메서드 내부에서 this는 메서드를 호출한 객체를 가리킨다.
    console.log(this); // {name: "Lee", getName: ƒ}
    return this.name;
  },
};
console.log(person.getName()); // Lee

function Person(name) {
  this.name = name;
  // 생성자 함수 내부에서 this는 생성자 함수가 생성할 인스턴스를 가리킨다.
  console.log(this); // Person {name: "Lee"}
}

const me = new Person("Lee");
```

`this`는 객체의 프로퍼티나 메서드를 참조하기 위한 자기 참조 변수이므로 일반적으로 객체의 메서드 내부 또는 생성자 함수 내부에서만 의미가 있다.

## 22.2 함수 호출 방식과 `this` 바인딩

`this` 바인딩은 함수가 어떻게 호출되었는지에 따라 동적으로 결정된다.

💡렉시컬 스코프와 `this` 바인딩은 결정시기가 다르다. 렉시컬 스코프는 함수 객체가 생성되는 시점에 상위 스코프를 결정하지만, `this` 바인딩은 함수 호출 시점에 결정된다.

1. 일반 함수 호출: `this`는 전역 객체를 가리킨다.
2. 메서드 호출: `this`는 메서드 호출 객체를 가리킨다.
3. 생성자 함수 호출: 생성자 함수가 생성한 인스턴스를 가리킨다.
4. `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출: 전달한 인수에 의해 결정된다.

```js
// this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.
const foo = function () {
  console.dir(this);
};

// 동일한 함수도 다양한 방식으로 호출할 수 있다.

// 1. 일반 함수 호출
// foo 함수를 일반적인 방식으로 호출
// foo 함수 내부의 this는 전역 객체 window를 가리킨다.
foo(); // window

// 2. 메서드 호출
// foo 함수를 프로퍼티 값으로 할당하여 호출
// foo 함수 내부의 this는 메서드를 호출한 객체 obj를 가리킨다.
const obj = { foo };
obj.foo(); // obj

// 3. 생성자 함수 호출
// foo 함수를 new 연산자와 함께 생성자 함수로 호출
// foo 함수 내부의 this는 생성자 함수가 생성한 인스턴스를 가리킨다.
new foo(); // foo {}

// 4. Function.prototype.apply/call/bind 메서드에 의한 간접 호출
// foo 함수 내부의 this는 인수에 의해 결정된다.
const bar = { name: "bar" };

foo.call(bar); // bar
foo.apply(bar); // bar
foo.bind(bar)(); // bar
```

### 일반 함수 호출

**일반 함수로 호출된 모든 함수(중첩 함수, 콜백 함수 포함) 내부의 `this`는 전역 객체가 바인딩된다.**

이는 메서드 호출 대상이 아니라 전역 객체를 바라보고 있기에 부자연스럽다.

메서드 내부의 중첩 함수, 콜백 함수의 `this` 바인딩을 메서드의 `this` 바인딩과 일치시키기 위한 방법:

1. `this` 바인딩을 외부 함수 변수에 할당

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // this 바인딩(obj)을 변수 that에 할당한다.
    const that = this;

    // 콜백 함수 내부에서 this 대신 that을 참조한다.
    setTimeout(function () {
      console.log(that.value); // 100
    }, 100);
  },
};

obj.foo();
```

2. `apply/call/bind` 메서드를 활용

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 콜백 함수에 명시적으로 this를 바인딩한다.
    setTimeout(
      function () {
        console.log(this.value); // 100
      }.bind(this),
      100
    );
  },
};

obj.foo();
```

3. 화살표 함수를 활용

```js
var value = 1;

const obj = {
  value: 100,
  foo() {
    // 화살표 함수 내부의 this는 상위 스코프의 this를 가리킨다.
    setTimeout(() => console.log(this.value), 100); // 100
  },
};

obj.foo();
```

### 메서드 호출

**메서드를 호출할 때 메서드 이름 앞의 마침표 연산자 앞에 기술한 객체가 바인딩된다.**

이때 메서드는 객체에 포함된 것이 아니라, 프로퍼티가 독립적으로 존재하는 함수 객체를 바라보고 있을 뿐이다.

따라서 해당 프로퍼티를 다른 객체에 할당함으로써 다른 객체의 메서드가 될 수도 있고, 일반 변수에 할당하여 일반 함수로 호출될 수도 있다.

```js
const anotherPerson = {
  name: "Kim",
};
// getName 메서드를 anotherPerson 객체의 메서드로 할당
anotherPerson.getName = person.getName;

// getName 메서드를 호출한 객체는 anotherPerson이다.
console.log(anotherPerson.getName()); // Kim

// getName 메서드를 변수에 할당
const getName = person.getName;

// getName 메서드를 일반 함수로 호출
console.log(getName()); // ''
// 일반 함수로 호출된 getName 함수 내부의 this.name은 브라우저 환경에서 window.name과 같다.
// 브라우저 환경에서 window.name은 브라우저 창의 이름을 나타내는 빌트인 프로퍼티이며 기본값은 ''이다.
// Node.js 환경에서 this.name은 undefined다.
```

**메서드 내부의 `this`는 객체와는 관계가 없으며 메서드를 호출한 객체에 무조건 바인딩된다.**

### 생성자 함수 호출

생성자 함수 내부의 `this`에는 생성자 함수가 (미래에) 생성할 인스턴스가 바인딩된다.

### `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출

`Function.prototype`의 메서드이므로 이 메서드들은 모든 함수가 상속받아 사용할 수 있다.

`apply`, `call` 메서드는 `this`로 사용할 객체와 인수 리스트를 인수로 전달받아 함수를 호출한다.

- `apply`: 인수 리스트를 배열 또는 유사 배열 객체로 전달
- `call`: 인수 리스트를 개별 인자로 전달

```js
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

console.log(getThisBinding()); // window

// getThisBinding 함수를 호출하면서 인수로 전달한 객체를 getThisBinding 함수의 this에 바인딩한다.
console.log(getThisBinding.apply(thisArg)); // {a: 1}
console.log(getThisBinding.call(thisArg)); // {a: 1}
```

`apply`와 `call`은 기본적으로 함수를 호출하기 위한 메서드들이다. 호출 시 첫 번째 인수로 전달한 특정 객체를 호출한 함수의 `this`에 바인딩한다.

대표적인 용도는 `arguments` 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우다. `arguments` 객체는 배열이 아니므로 `Array.prototype.slice` 같은 배열의 메서드를 사용할 수 없으나, `apply`, `call` 메서드를 이용하면 가능하다.

```js
function convertArgsToArray() {
  console.log(arguments);

  // arguments 객체를 배열로 변환
  // Array.prototype.slice를 인수없이 호출하면 배열의 복사본을 생성한다.
  const arr = Array.prototype.slice.call(arguments);
  // const arr = Array.prototype.slice.apply(arguments);
  console.log(arr);

  return arr;
}

convertArgsToArray(1, 2, 3); // [1, 2, 3]
```

`bind` 메서드는 함수를 호출하지는 않지만 첫 번째 인수로 전달한 값으로 `this` 바인딩이 교체된 함수를 새로 생성해 반환한다.

```js
function getThisBinding() {
  return this;
}

// this로 사용할 객체
const thisArg = { a: 1 };

// bind 메서드는 첫 번째 인수로 전달한 thisArg로 this 바인딩이 교체된
// getThisBinding 함수를 새롭게 생성해 반환한다.
console.log(getThisBinding.bind(thisArg)); // getThisBinding
// bind 메서드는 함수를 호출하지는 않으므로 명시적으로 호출해야 한다.
console.log(getThisBinding.bind(thisArg)()); // {a: 1}
```

```js
const person = {
  name: "Lee",
  foo(callback) {
    // setTimeout(callback, 100);
    // bind 메서드로 callback 함수 내부의 this 바인딩을 전달
    setTimeout(callback.bind(this), 100);
  },
};

person.foo(function () {
  console.log(`Hi! my name is ${this.name}.`); // Hi! my name is Lee.
});
```

위 예제 코드는 `foo`가 호출될 때, `this`는 전역 객체에 바인딩되기 때문에, `bind`를 사용해서 콜백 함수 내부의 `this`를 외부 함수 내부의 `this`와 일치시키고 있다.

| 함수 호출 방식                                             | this 바인딩                                                            |
| ---------------------------------------------------------- | ---------------------------------------------------------------------- |
| 일반 함수 호출                                             | 전역 객체                                                              |
| 메서드 호출                                                | 메서드를 호출한 객체                                                   |
| 생성자 함수 호출                                           | 생성자 함수 (미래에 생성될 인스턴스)                                   |
| Function.prototype.apply/call/bind 메서드에 의한 간접 호출 | Function.prototype.apply/call/bind 메서드에 첫 번째 인수로 전달한 객체 |
