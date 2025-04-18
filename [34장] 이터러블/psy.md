## 이터레이션 프로토콜

ES6에서 도입

순회 가능한 데이터 컬렉션을 만들기 위해 미리 약속한 규칙

for … of, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용 가능

이터러블 프로토콜과 이터레이터 프로토콜로 구성됨

### 이터러블

이터러블 프로토콜을 준수한 객체 → Symbol.iterator를 프로퍼티 키로 사용한 메서드를 구현하거나, 프로토타입 체인을 통해 상속받은 객체

```jsx
const isIterable = v => v !== null && typeof v[Symbol.iterator] === 'function';
```

### 이터레이터

이터레이터 프로토콜을 준수한 객체 → next 메서드를 소유하며, 호출하면 value와 done을 프로퍼티로 갖는 이터레이터 리절트 객체를 반환함

이터러블의 Symbol.iterartor 메서드를 호출하면 반환됨

```jsx
const array = [1, 2, 3];

const iterator = array[Symbol.iterator]();

console.log(iterator.next()); // { value: 1, done: false }
```

## for … of 문

이터러블을 순회하며 이터러블의 요소를 변수에 할당

## 이터러블과 유사 배열 객체

유사 배열 객체는 Symbol.iterator 메서드가 없기 때문에 이터러블이 아님 → 추가해야 함

## 이터레이션 프로토콜의 필요성

이터러블은 데이터 공급자의 역할을 한다고 할 수 있는데, 만약 데이터 공급자가 각자의 순회 방식을 갖는다면 데이터 소비자는 다양한 순회 방식을 모두 지원해야 함 → 이터레이션 프로토콜을 통해 통일 가능

데이터 소비자와 데이터 공급자를 연결하는 인터페이스의 역할
