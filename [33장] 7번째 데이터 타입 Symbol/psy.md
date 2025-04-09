ES6에서 도입된 변경 불가능한 원시 타입의 값

다른 값과 중복되지 않는 유일무이한 값 → 충돌 위험이 없는 유일한 프로퍼티 키를 만들기 위해 사용

## 심벌 값의 생성

### Symbol 함수

```jsx
const sb = Symbol();
// consructor가 아님

const sb = Symbol('description');
// 인수로 전달하는 문자열은 디버깅 용도의 설명
```

래퍼 객체가 존재하고, 암묵적으로 불리언 타입으로만 변환됨

### Symbol.for/keyFor

인수로 전달된 문자열을 키로 사용하여 전역 심벌 레지스트리에서 키와 일치하는 심벌 값을 검색함

검색 결과가 없으면 새로운 심벌을 생성함

```jsx
const s1 = Symbol.for('asdf');
const s2 = Symbol.for('asdf');

s1 === s2 // true

Symbol.keyFor(s1); // 'asdf'
```

Symbol.for 를 사용해 생성한 심벌 값만 전역 심벌 레지스트리에 등록됨

## 심벌과 상수

```jsx
const Direction = {
	U: 1,
	R: 2,
	D: 3,
	L: 4,
} // 이런 상수는 값이 중복될 수 있음

const Direction = {
	U: Symbol('U'),
	R: Symbol('R'), ...
}
```

## 심벌과 프로퍼티 키

```jsx
const obj = {
	[Symbol.for('asdf')]: 1,
}
```

## 심벌과 프로퍼티 은닉

심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티는 for … in 문이나 Object.keys, Object.getOwnPropertyNames 메서드로 찾을 수 없음

Object.getOwnPropertySymbols 메서드를 사용하면 심벌 값을 프로퍼티 키로 사용하여 생성한 프로퍼티를 찾을 수 있음

## 심벌과 표준 빌트인 객체 확장

표준 빌트인 객체를 확장하는 것은 권장되지 않음 → 나중에 프로퍼티 이름이 겹칠 수 있기 때문

심벌 값을 프로퍼티 키로 사용하면 확장할 수 있음

## Well-known Symbol

자바스크립트가 기본 제공하는 빌트인 심벌 값

예를 들어, 이터러블은 Symbol.iterator를 키로 갖는 메서드를 가지며, Symbol.iterator 메서드를 호출하면 이터레이터를 반환하도록 ECMAScript에 규정되어 있음
