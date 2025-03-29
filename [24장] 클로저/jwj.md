# 24장 클로저

    클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.

자바스크립트에서 클로저가 발생하는 이유는 자바스크립트가 렉시컬 스코프를 따르는 언어이기 때문이다.

## 24.1 렉시컬 스코프

**렉시컬 스코프(정적 스코프)는 함수를 어디에 정의했는지에 따라 상위 스코프가 결정되는 것을 의미**한다.

실행 컨텍스트의 관점에서 보면, 렉시컬 환경의 외부 렉시컬 환경에 대한 참조에 저장할 참조값, 즉 상위 스코프에 대한 참조가 함수 정의가 평가되는 시점에 함수가 정의된 환경(위치)에 의해 결정되는 것을 말한다.

## 24.2 함수 객체의 내부 슬롯 [[Environment]]

자바스크립트의 **환경**은 코드 내부에서의 위치를 말한다.

함수는 자신의 내부 슬롯 `[[Environment]]`에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.

**함수가 평가되어 함수 객체를 생성할 때** 자신이 정의된 환경에 의해 결정된 상위 스코프 참조를 함수 객체 자신의 **내부 슬롯 `[[Environment]]`에 저장**한다.

이때 저장되는 건 지금 실행중인 실행 컨텍스트의 렉시컬 환경이다.

**함수 객체는 내부 슬롯을 통해 저장한 렉시컬 환경의 참조, 즉 상위 스코프를 자신이 존재하는 한 기억한다.**

## 24.3 클로저와 렉시컬 환경

```js
const x = 1;

// ①
function outer() {
  const x = 10;
  const inner = function () {
    console.log(x);
  }; // ②
  return inner;
}

// outer 함수를 호출하면 중첩 함수 inner를 반환한다.
// 그리고 outer 함수의 실행 컨텍스트는 실행 컨텍스트 스택에서 팝되어 제거된다.
const innerFunc = outer(); // ③
innerFunc(); // ④ 10
```

**외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있다. 이러한 중첩 함수를 클로저라고 부른다.**

`outer` 함수의 렉시컬 환경은 `inner` 함수의 `[[Environment]]`에 의해 참조되고 있기 때문에 가비지 컬렉션의 대상이 되지 않는다. 때문에 `outer` 함수의 생명 주기가 종료된 후에도 `inner` 함수는 `x`를 참조할 수 있다.

자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론상 모든 함수는 클로저다.

하지만 일반적으로 중첩 함수라고 해서 모두 클로저라고 하지는 않는다. 외부 함수의 식별자를 참조하거나 하는 경우가 아니면 클로저가 아니다.

또, 외부 함수의 식별자를 참조하지만 외부 함수에 의해 반환되지 않는 경우, 즉 외부 함수보다 내부 함수의 생명 주기가 더 짧은 경우, 클로저라고 하지 않는다.

클로저에 의해 참조되는 상위 스코프의 변수를 **자유 변수(free variable)**라고 부른다.

클로저는 **함수가 자유 변수에 대해 닫혀있다**라는 의미다. "자유 변수에 묶여있는 함수"라고도 할 수 있다.

## 24.4 클로저의 활용

**클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.**

상태가 의도치 않게 변경되지 않도록 **상태를 안전하게 은닉**하고 **특정 함수에게만 상태 변경을 허용**하기 위해 사용한다.

```js
const counter = (function () {
  // 카운트 상태 변수
  let num = 0;

  // 클로저인 메서드를 갖는 객체를 반환한다.
  // 객체 리터럴은 스코프를 만들지 않는다.
  // 따라서 아래 메서드들의 상위 스코프는 즉시 실행 함수의 렉시컬 환경이다.
  return {
    // num: 0, // 프로퍼티는 public하므로 은닉되지 않는다.
    increase() {
      return ++num;
    },
    decrease() {
      return num > 0 ? --num : 0;
    },
  };
})();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

클로저를 통해 다음과 같은 조건을 지킬 수 있게 되었다.

1. 카운트 상태(`num` 값)는 메서드가 호출되기 전까지 변경되지 않고 유지되어야 한다. (메서드를 제외한 무언가에 의한 의도치 않은 상태 변경을 막기 위함이다.)
2. 이를 위해 카운트 상태는 메서드만이 변경할 수 있어야 한다.

이를 생성자 함수로 표현하면 다음과 같이 작성할 수 있다.

```js
const Counter = (function () {
  // ① 카운트 상태 변수
  let num = 0;

  function Counter() {
    // this.num = 0; // ② 프로퍼티는 public하므로 은닉되지 않는다.
  }

  Counter.prototype.increase = function () {
    return ++num;
  };

  Counter.prototype.decrease = function () {
    return num > 0 ? --num : 0;
  };

  return Counter;
})();

const counter = new Counter();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2

console.log(counter.decrease()); // 1
console.log(counter.decrease()); // 0
```

외부 상태 변경, 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수 효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저가 적극 활용된다.

아래 코드는 함수형 프로그래밍에서 클로저를 활용하는 예제다.

```js
// 함수를 인수로 전달받고 함수를 반환하는 고차 함수
// 이 함수는 카운트 상태를 유지하기 위한 자유 변수 counter를 기억하는 클로저를 반환한다.
function makeCounter(aux) {
  // 카운트 상태를 유지하기 위한 자유 변수
  let counter = 0;

  // 클로저를 반환
  return function () {
    // 인수로 전달 받은 보조 함수에 상태 변경을 위임한다.
    counter = aux(counter);
    return counter;
  };
}

// 보조 함수
function increase(n) {
  return ++n;
}

// 보조 함수
function decrease(n) {
  return --n;
}

// 함수로 함수를 생성한다.
// makeCounter 함수는 보조 함수를 인수로 전달받아 함수를 반환한다
const increaser = makeCounter(increase); // ①
console.log(increaser()); // 1
console.log(increaser()); // 2

// increaser 함수와는 별개의 독립된 렉시컬 환경을 갖기 때문에 카운터 상태가 연동하지 않는다.
const decreaser = makeCounter(decrease); // ②
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

아래 코드는 렉시컬 환경을 공유하는 클로저 예제다. 이를 위해서는 `makeCounter` 함수를 두 번 호출해서는 안 된다.

```js
const counter = (function () {
  let num = 0; // 카운터 상태 유지를 위한 자유 변수

  // 함수를 인수로 전달받는 클로저를 반환
  return function (aux) {
    // 인수로 전달받은 보조 함수에 상태 변경을 위임한다.
    num = aux(num);
    return num;
  };
})();

function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}

console.log(counter(increase));
```

## 24.5 캡슐화와 정보 은닉

캡슐화는 객체 상태를 나타내는 프로퍼티와 프로퍼티를 참조하고 조작할 수 있는 동작인 메서드를 하나로 묶는 것을 말한다.

캡슐화는 객체의 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라 한다.

정보 은닉으로 의도치 않은 접근을 막아 객체 상태가 변경되는 것을 방지하고 객체 간의 상호 의존성, 즉 결합도를 낮출 수 있다.

자바스크립트 객체의 프로퍼티나 메서드는 기본적으로 `public`하다.

아래 코드는 자바스크립트에서 정보 은닉을 구현하기 위한 예제다.

```js
const Person = (function () {
  let _age = 0; // private

  // 생성자 함수
  function Person(name, age) {
    this.name = name; // public
    _age = age;
  }

  // 프로토타입 메서드
  Person.prototype.sayHi = function () {
    console.log(`Hi! My name is ${this.name}. I am ${_age}.`);
  };

  // 생성자 함수를 반환
  return Person;
})();

const me = new Person("Lee", 20);
me.sayHi(); // Hi! My name is Lee. I am 20.
console.log(me.name); // Lee
console.log(me._age); // undefined

const you = new Person("Kim", 30);
you.sayHi(); // Hi! My name is Kim. I am 30.
console.log(you.name); // Kim
console.log(you._age); // undefined
```

⚠️위 코드는 `Person` 인스턴스를 여러 개 생성할 경우 `_age` 변수의 상태가 유지되지 않는다. `sayHi` 메서드가 단 한 번 생성되는 클로저이기 때문이다. `sayHi` 메서드는 즉시 실행 함수를 `[[Environment]]`에 저장하여 기억한다.

자바스크립트는 정보 은닉을 완전하게 지원하지 않는다. 인스턴스 메서드로 자유 변수를 통해 `private`를 흉내낼 수는 있지만 프로토타입 메서드를 사용하면 이마저도 불가능해진다.

ES6의 `Symbol` 또는 `WeakMap`을 사용하며 `private`한 프로퍼티를 흉내 내기도 했으나 근본적인 해결책이 되지 않는다.

## 24.6 자주 발생하는 실수

```js
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i;
  };
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
```

위 예제 코드는 의도와 다르게 3을 세 번 출력한다. `var` 키워드의 변수는 함수 레벨 스코프를 갖기 때문에 전역 변수가 되기 때문이다.

```js
var funcs = [];

for (var i = 0; i < 3; i++) {
  funcs[i] = (function (id) {
    return function () {
      return id;
    };
  })(i);
}

for (var j = 0; j < funcs.length; j++) {
  console.log(funcs[j]());
}
```

즉시 실행 함수는 전역 변수 `i`의 값을 인수로 전달받아 매개변수 `id`에 할당한 후 중첩 함수를 반환하고 종료된다. 즉시 실행 함수가 반환한 함수는 `funcs` 배열에 순차적으로 저장된다.

각각의 중첩 함수들은 자신의 상위 스코프를 기억하는 클로저이기 때문에, 매개변수 `id`는 즉시 실행 함수가 반환한 실행 함수가 반환한 중첩 함수에 묶여있는 자유 변수가 되어 그 값이 유지된다.

이 문제는 `let`을 사용하면 간편하게 해결할 수 있다.

```js
var funcs = [];

for (let i = 0; i < 3; i++) {
  funcs[i] = function () {
    return i;
  };
}

for (let i = 0; i < 3; i++) {
  console.log(funcs[i]());
}
```

반복문 내부에 함수 정의가 없는 반복문이 생성하는 렉시컬 환경들은 아무도 참조하지 않기 때문에 가비지 컬렉션의 대상이 된다.

또 다른 방법으로 함수형 프로그래밍 기법인 고차 함수를 사용하는 방법이 있다.

```js
// 요소가 3개인 배열을 생성하고 배열의 인덱스를 반환하는 함수를 요소로 추가한다.
const funcs = Array.from(new Array(3), (_, i) => () => i); // (3) [f, f, f]

funcs.forEach((f) => console.log(f())); // 0 1 2
```
