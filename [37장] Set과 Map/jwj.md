# 37장 Set과 Map

## 37.1 `Set`

`Set` 객체는 중복되지 않는 유일한 값들의 집합이다. 배열과 유사하지만, `Set`은 다음과 같은 특성이 있다.

- 중복 값을 허용하지 않는다.
- 요소 순서의 의미가 없다.
- 인덱스로 요소에 접근할 수 없다.

수학의 집합을 구현하기 위한 자료구조다.

### `Set` 객체의 생성

`Set` 생성자 함수로 생성할 수 있다. 인수를 전달하지 않으면 빈 `Set` 객체가 생성된다.

**`Set` 생성자 함수는 이터러블을 인수로 전달받아 `Set` 객체를 생성한다**. 이때 이터러블의 중복된 값은 `Set` 객체에 요소로 저장되지 않는다.

`Set` 객체는 중복을 제거할 때 유용하다.

```js
// 배열의 중복 요소 제거
const uniq = (array) => array.filter((v, i, self) => self.indexOf(v) === i);
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]

// Set을 사용한 배열의 중복 요소 제거
const uniq = (array) => [...new Set(array)];
console.log(uniq([2, 1, 2, 3, 4, 3, 4])); // [2, 1, 3, 4]
```

### 요소 개수 확인

`Set.prototype.size` 프로퍼티를 사용한다. `size` 프로퍼티는 getter 함수만 존재하는 접근자 프로퍼티다.

```js
const { size } = new Set([1, 2, 3, 3]);
console.log(size); // 3
```

### 요소 추가

`Set.prototype.add` 메서드를 사용한다.

`add` 메서드는 새로운 요소가 추가된 `Set` 객체를 반환하므로 체이닝 호출이 가능하다.

이때 중복된 요소는 추가되지 않고 에러가 발생하지 않으며 무시된다.

```js
const set = new Set();

set.add(1).add(2).add(2);
console.log(set); // Set(2) {1, 2}
```

일치 비교 연산자 `===`는 `NaN`과 `NaN`을 다르다고 평가한다. 하지만 `Set` 객체는 이를 같다고 평가하여 중복 추가를 허용하지 않는다. `+0`과 `-0`도 일치 비교 연산자와 마찬가지로 같다고 평가한다.

`Set` 객체는 객체나 배열처럼 자바스크립트의 모든 값을 요소로 저장할 수 있다.

```js
const set = new Set();

set.add(1).add("a").add(true).add(undefined).add(null).add({}).add([]);

console.log(set); // Set(7) {1, "a", true, undefined, null, {}, []}
```

### 요소 존재 여부 확인

`Set.prototype.has` 메서드를 사용한다. 특정 요소의 존재 여부를 나타내는 불리언 값을 반환한다.

### 요소 삭제

`Set.prototype.delete` 메서드를 사용한다. 특정 요소의 삭제 성공 여부를 나타내는 불리언 값을 반환한다.인수로 요소값을 전달해야 한다.

### 요소 일괄 삭제

`Set.prototype.clear` 메서드를 사용한다. 언제나 `undefined`를 반환한다.

### 요소 순회

`Set.prototype.forEach` 메서드를 사용한다. `Array.prototype.forEach` 메서드와 유사하게 콜백 함수와 `forEach` 메서드의 콜백 함수 내부에서 `this`로 사용될 객체(옵션)을 인수로 전달한다.

- 첫 번째 인수: 순회 중인 요소값
- 두 번째 인수: 순회 중인 요소값
- 세 번째 인수: 현재 순회 중인 `Set` 객체 자체

첫 번쨰와 두 번째 인수는 같은 값이다. 이렇게 동작하는 이유는 `Array.prototype.forEach` 메서드와 인터페이스를 통일하기 위함이다.

`Set` 객체는 이터러블이다. 따라서 `for...of` 문으로 순회할 수 있다. 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.

> `Set` 객체의 순회 순서는 요소가 추가된 순서를 따른다. 이는 ES 사양에 규정되어 있지 않지만 다른 이터러블의 순회와 호환성을 유지하기 위함이다.

### 집합 연산

교집합, 합집합, 차집합 등을 구현할 수 있다.

```js
Set.prototype.intersection = function (set) {
  return new Set([...this].filter((v) => set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 교집합
console.log(setA.intersection(setB)); // Set(2) {2, 4}
// setB와 setA의 교집합
console.log(setB.intersection(setA)); // Set(2) {2, 4}
```

```js
Set.prototype.union = function (set) {
  return new Set([...this, ...set]);
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA와 setB의 합집합
console.log(setA.union(setB)); // Set(4) {1, 2, 3, 4}
// setB와 setA의 합집합
console.log(setB.union(setA)); // Set(4) {2, 4, 1, 3}
```

```js
Set.prototype.difference = function (set) {
  return new Set([...this].filter((v) => !set.has(v)));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA에 대한 setB의 차집합
console.log(setA.difference(setB)); // Set(2) {1, 3}
// setB에 대한 setA의 차집합
console.log(setB.difference(setA)); // Set(0) {}
```

```js
// this가 subset의 상위 집합인지 확인한다.
Set.prototype.isSuperset = function (subset) {
  const supersetArr = [...this];
  return [...subset].every((v) => supersetArr.includes(v));
};

const setA = new Set([1, 2, 3, 4]);
const setB = new Set([2, 4]);

// setA가 setB의 상위 집합인지 확인한다.
console.log(setA.isSuperset(setB)); // true
// setB가 setA의 상위 집합인지 확인한다.
console.log(setB.isSuperset(setA)); // false
```

## 37.2 `Map`

`Map` 객체는 키와 값 쌍으로 이루어진 컬렉션이다. 객체와 유사하지만 다음과 같은 차이점이 있다.

| 구분                   | 객체                      | Map 객체              |
| ---------------------- | ------------------------- | --------------------- |
| 키로 사용할 수 있는 값 | 문자열 또는 심벌 값       | 객체를 포함한 모든 값 |
| 이터러블               | ❌                        | ⭕                    |
| 요소 개수 확인         | `Object.keys(obj).length` | `map.size`            |

### `Map` 객체의 생성

`Map` 생성자 함수로 생성할 수 있다. 인수를 전달하지 않으면 빈 `Map` 객체가 생성된다.

**`Set` 생성자 함수는 이터러블을 인수로 전달받아 `Set` 객체를 생성한다**. 이때 이터러블은 키와 값의 쌍으로 이루어진 요소로 구성되어야 한다.

```js
const map1 = new Map([
  ["key1", "value1"],
  ["key2", "value2"],
]);
console.log(map1); // Map(2) {"key1" => "value1", "key2" => "value2"}

const map2 = new Map([1, 2]); // TypeError: Iterator value 1 is not an entry object
```

중복된 키를 갖는 요소가 존재하면 값이 덮어써진다. `Map` 객체는 중복된 키를 갖는 요소를 허용하지 않는다.

### 요소 개수 확인

`Map.prototype.size` 프로퍼티를 사용한다.

### 요소 추가

`Map.prototype.set` 메서드를 사용한다.

```js
const map = new Map();
console.log(map); // Map(0) {}

map.set("key1", "value1");
console.log(map); // Map(1) {"key1" => "value1"}
```

`set` 메서드는 새로운 요소가 추가된 `Map` 객체를 반환하므로 체이닝 호출이 가능하다.

일치 비교 연산자 `===`는 `NaN`과 `NaN`을 다르다고 평가한다. 하지만 `Map` 객체는 이를 같다고 평가하여 중복 추가를 허용하지 않는다. `+0`과 `-0`도 일치 비교 연산자와 마찬가지로 같다고 평가한다.

객체는 문자열 또는 심벌 값만 키로 사용할 수 있지만, **`Map`은 키 타입에 제한이 없다.**

### 요소 취득

`Map.prototype.get` 메서드를 사용한다. 인수로 키를 전달하면 해당 키의 값을 반한한다. 해당 키가 존재하지 않으면 `undefined`를 반환한다.

### 요소 존재 여부 확인

`Map.prototype.has` 메서드를 사용한다.

### 요소 삭제

`Map.prototype.delete` 메서드를 사용한다.

### 요소 일괄 삭제

`Map.prototype.clear` 메서드를 사용한다.

### 요소 순회

`Map.prototype.forEach` 메서드를 사용한다. `Array.prototype.forEach` 메서드와 유사하게 콜백 함수와 `forEach` 메서드의 콜백 함수 내부에서 `this`로 사용될 객체(옵션)을 인수로 전달한다.

- 첫 번째 인수: 현재 순회 중인 요소값
- 두 번째 인수: 현재 순회 중인 요소키
- 세 번째 인수: 현재 순회 중인 `Map` 객체 자체

첫 번쨰와 두 번째 인수는 같은 값이다. 이렇게 동작하는 이유는 `Array.prototype.forEach` 메서드와 인터페이스를 통일하기 위함이다.

`Map` 객체는 이터러블이다. 따라서 `for...of` 문으로 순회할 수 있다. 스프레드 문법과 배열 디스트럭처링의 대상이 될 수 있다.

`Map` 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다.

| Map 메서드              | 설명                                                                                           |
| ----------------------- | ---------------------------------------------------------------------------------------------- |
| `Map.prototype.keys`    | Map 객체에서 요소키를 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.          |
| `Map.prototype.values`  | Map 객체에서 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다.          |
| `Map.prototype.entries` | Map 객체에서 요소키와 요소값을 값으로 갖는 이터러블이면서 동시에 이터레이터인 객체를 반환한다. |

```js
const lee = { name: "Lee" };
const kim = { name: "Kim" };

const map = new Map([
  [lee, "developer"],
  [kim, "designer"],
]);

// Map.prototype.keys는 Map 객체에서 요소키를 값으로 갖는 이터레이터를 반환한다.
for (const key of map.keys()) {
  console.log(key); // {name: "Lee"} {name: "Kim"}
}

// Map.prototype.values는 Map 객체에서 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const value of map.values()) {
  console.log(value); // developer designer
}

// Map.prototype.entries는 Map 객체에서 요소키와 요소값을 값으로 갖는 이터레이터를 반환한다.
for (const entry of map.entries()) {
  console.log(entry); // [{name: "Lee"}, "developer"]  [{name: "Kim"}, "designer"]
}
```

> `Map` 객체의 순회 순서는 요소가 추가된 순서를 따른다. 이는 ES 사양에 규정되어 있지 않지만 다른 이터러블의 순회와 호환성을 유지하기 위함이다.
