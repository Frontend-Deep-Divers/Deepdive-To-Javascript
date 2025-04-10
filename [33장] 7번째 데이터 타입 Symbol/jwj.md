# 33장 7번째 데이터 타입 Symbol

## 33.1 심벌이란?

심벌(Symbol)은 ES6에서 도입된 7번째 데이터 타입으로 불변한 원시 타입 값이다.

심벌 값은 다른 값과 중복되지 않는 유일무이한 값이다. 따라서 주로 이름의 충돌 위험이 없는 **유일한 프로퍼티 키를 만들기 위해 사용한다.**

## 33.2 심벌 값의 생성

### `Symbol` 함수

심벌 값은 `Symbol` 함수를 호출하여 생성한다. 리터럴 표기법으로 생성할 수는 없다.

심벌 값은 외부로 노출되지 않아 확인할 수 없다.

```js
// Symbol 함수를 호출하여 유일무이한 심벌 값을 생성한다.
const mySymbol = Symbol();
console.log(typeof mySymbol); // symbol

// 심벌 값은 외부로 노출되지 않아 확인할 수 없다.
console.log(mySymbol); // Symbol()
```

`Symbol` 함수의 인수로 문자열을 전달할 수 있고, 이 문자열은 심벌 값에 대한 설명으로 디버깅 용도로만 사용된다.

```js
// 심벌 값에 대한 설명이 같더라도 유일무이한 심벌 값을 생성한다.
const mySymbol1 = Symbol("mySymbol");
const mySymbol2 = Symbol("mySymbol");

console.log(mySymbol1 === mySymbol2); // false
```

객체처럼 접근하면 암묵적으로 래퍼 객체를 생성한다.

```js
const mySymbol = Symbol("mySymbol");

// 심벌도 레퍼 객체를 생성한다
console.log(mySymbol.description); // mySymbol
console.log(mySymbol.toString()); // Symbol(mySymbol)
```

암묵적으로 문자열이나 숫자 타입으로 변환되지 않지만, 불리언 타입으로는 변환된다.

```js
const mySymbol = Symbol();

const mySymbol = Symbol();

// 심벌 값은 암묵적으로 문자열이나 숫자 타입으로 변환되지 않는다.
console.log(mySymbol + ""); // TypeError: Cannot convert a Symbol value to a string
console.log(+mySymbol); // TypeError: Cannot convert a Symbol value to a number

// 불리언 타입으로는 암묵적으로 타입 변환된다.
console.log(!!mySymbol); // true

// if 문 등에서 존재 확인을 위해 사용할 수 있다.
if (mySymbol) console.log("mySymbol is not empty.");
```

### `Symbol.for/Symbol.keyFor` 메서드

`Symbol.for` 메서드는 인수로 전달받은 문자열을 키로 사용하여 키와 심벌 값의 쌍들이 저장되어 있는 전역 심벌 레지스트리에서 해당 키와 일치하는 심벌 값을 검색한다.

- 검색에 성공하면 새로운 심벌 값을 생성하지 않고 검색된 심벌 값을 반환
- 검색에 실패하면 새로운 심벌 값을 생성하며 `Symbol.for` 메서드의 인수로 전달된 키로 전역 심벌 레지스트리에 저장한 후, 생성된 심벌 값을 반환

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for("mySymbol");
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 있으면 해당 심벌 값을 반환
const s2 = Symbol.for("mySymbol");

console.log(s1 === s2); // true
```

`Symbol.for` 메서드로 심벌 값을 지정해서 전역 심벌 레지스트리를 통해 공유할 수 있다.

`Symbol.keyFor` 메서드를 사용하면 전역 심벌 레지스트리에 지정된 심벌 값의 키를 추출할 수 있다.

```js
// 전역 심벌 레지스트리에 mySymbol이라는 키로 저장된 심벌 값이 없으면 새로운 심벌 값을 생성
const s1 = Symbol.for("mySymbol");
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s1); // -> mySymbol

// Symbol 함수를 호출하여 생성한 심벌 값은 전역 심벌 레지스트리에 등록되어 관리되지 않는다.
const s2 = Symbol("foo");
// 전역 심벌 레지스트리에 저장된 심벌 값의 키를 추출
Symbol.keyFor(s2); // -> undefined
```

## 33.3 심벌과 상수

심벌로 다른 언어의 `enum`을 흉내낼 수 있다.

```js
// JavaScript enum
// Direction 객체는 불변 객체이며 프로퍼티는 유일무이한 값이다.
const Direction = Object.freeze({
  UP: Symbol("up"),
  DOWN: Symbol("down"),
  LEFT: Symbol("left"),
  RIGHT: Symbol("right"),
});

const myDirection = Direction.UP;

if (myDirection === Direction.UP) {
  console.log("You are going UP.");
}
```

## 33.4 심벌과 프로퍼티 키

심벌 값으로 프로퍼티 키를 동적 생성하여 프로퍼티를 만들 수 있다.

심벌 값에 대괄호를 사용해야 하며, 접근할 때도 대괄호를 사용해야 한다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol.for("mySymbol")]: 1,
};

obj[Symbol.for("mySymbol")]; // -> 1
```

이로써 다른 프로퍼티 키와 절대 충돌하지 않는 키를 생성해줄 수 있다.

## 33.5 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 생성하여 사용하면 `for..in` 문이나 `Object.keys`, `Object.getOwnPropertyNames` 메서드로 찾을 수 없다.

외부에 노출할 필요가 없는 프로퍼티를 생성할 수 있다.

ES6에서 도입된 `Object.getOwnPropertySymbols` 메서드를 사용하면 찾을 수 있다.

```js
const obj = {
  // 심벌 값으로 프로퍼티 키를 생성
  [Symbol("mySymbol")]: 1,
};

// getOwnPropertySymbols 메서드는 인수로 전달한 객체의 심벌 프로퍼티 키를 배열로 반환한다.
console.log(Object.getOwnPropertySymbols(obj)); // [Symbol(mySymbol)]

// getOwnPropertySymbols 메서드로 심벌 값도 찾을 수 있다.
const symbolKey1 = Object.getOwnPropertySymbols(obj)[0];
console.log(obj[symbolKey1]); // 1
```

## 33.6 심벌과 표준 빌트인 객체 확장

일반적으로 표준 빌트인 객체에 사용자 정의 메서드를 직접 추가해서 사용하는 것은 권장되지 않는다. 추후 같은 이름의 메서드가 표준 사양으로 추가되는 경우가 있기 때문이다.

하지만 심벌 값으로 프로퍼티 키를 생성하여 표준 빌트인 객체를 확장하면 그럴 위험이 없다.

```js
// 심벌 값으로 프로퍼티 키를 생성하면 충돌할 위험이 없다.
Array.prototype[Symbol.for("sum")] = function () {
  return this.reduce((acc, cur) => acc + cur, 0);
};

[1, 2][Symbol.for("sum")](); // 3
```

## 33.7 Well-known Symbol

자바스크립트가 기본 제공하는 빌트인 심벌 값이 있다. 이 값은 `Symbol` 함수의 프로퍼티에 할당되어 있다.

이 값들은 ES 사양에서는 `Well-known Symbol`이라 부른다. `Well-known Symbol`은 자바스크립트 엔진의 내부 알고리즘에 사용된다.

`for..of` 문으로 순회 가능한 빌트인 이터러블은 Well-known Symbol인 `Symbol.iterator`를 키로 갖는 메서드를 가지며, `Symbol.iterator` 메서드를 호출하면 이터레이터를 반환하도록 ES 사양에 규정되어 있다. 빌트인 이터러블은 해당 규정(이터레이션 프로토콜)을 준수한다.

빌트인 이터러블이 아닌 일반 객체를 이터러블처럼 동작하도록 구현하고 싶다면 이터레이션 프로토콜을 따르면 된다.

```js
// 1 ~ 5 범위의 정수로 이루어진 이터러블
const iterable = {
  // Symbol.iterator 메서드를 구현하여 이터러블 프로토콜을 준수
  [Symbol.iterator]() {
    let cur = 1;
    const max = 5;
    // Symbol.iterator 메서드는 next 메서드를 소유한 이터레이터를 반환
    return {
      next() {
        return { value: cur++, done: cur > max + 1 };
      },
    };
  },
};

for (const num of iterable) {
  console.log(num); // 1 2 3 4 5
}
```

`Symbol.iterator` 메서드는 기존 프로퍼티 키 또는 미래에 추가될 프로퍼티 키와 절대로 중복되지 않을 것이다.

**이처럼 심벌은 중복되지 않는 상수 값을 생성하고, 기존에 작성된 코드에 영향을 주지 않고 새로운 프로퍼티를 추가하기 위해, 즉 하위 호환성을 보장하기 위해 도입되었다.**
