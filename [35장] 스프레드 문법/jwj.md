# 35장 스프레드 문법

ES6에서 도입되었다.

**스프레드 문법(`...`)은 하나로 뭉쳐 있는 여러 값들의 집합을 펼쳐서 개별적인 값들의 목록으로 만든다.**

스프레드 문법은 `Array`, `String`, `Map`, `Set`, DOM 컬렉션(`NodeList`, `HTMLCollection`), `arguments`처럼 `for...of` 문으로 순회 가능한 이터러블에만 사용할 수 있다.

```js
// ...[1, 2, 3]은 [1, 2, 3]을 개별 요소로 분리한다(→ 1, 2, 3)
console.log(...[1, 2, 3]); // 1 2 3

// 문자열은 이터러블이다.
console.log(..."Hello"); // H e l l o

// Map과 Set은 이터러블이다.
console.log(
  ...new Map([
    ["a", "1"],
    ["b", "2"],
  ])
); // [ 'a', '1' ] [ 'b', '2' ]
console.log(...new Set([1, 2, 3])); // 1 2 3

// 이터러블이 아닌 일반 객체는 스프레드 문법의 대상이 될 수 없다.
console.log(...{ a: 1, b: 2 });
// TypeError: Found non-callable @@iterator
```

스프레드 문법의 결과는 값이 아닌, 값들의 목록이다. 따라서 스프레드 문법의 결과는 변수에 할당할 수 없다.

💡**쉼표로 구분한 값의 목록을 사용하는 문맥에만 사용할 수 있다.**

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록

## 35.1 함수 호출문의 인수 목록에서 사용하는 경우

배열을 펼쳐서 함수의 인수 목록에 개별 값들의 목록의 형태로 전달해야 할 경우가 있다.

`Math.max` 메서드를 사용할 땐, 개별 값들의 목록을 인수로 전달받는다.

이떄 배열을 스프레드 문법으로 펼쳐서 전달할 수 있다.

```js
const arr = [1, 2, 3];

// 스프레드 문법을 사용하여 배열 arr을 1, 2, 3으로 펼쳐서 Math.max에 전달한다.
// Math.max(...[1, 2, 3])은 Math.max(1, 2, 3)과 같다.
const max = Math.max(...arr); // -> 3
```

이전에는 `Function.prototype.apply`를 사용해야만 했다.

```js
var arr = [1, 2, 3];

// apply 함수의 2번째 인수(배열)는 apply 함수가 호출하는 함수의 인수 목록이다.
// 따라서 배열이 펼쳐져서 인수로 전달되는 효과가 있다.
var max = Math.max.apply(null, arr); // -> 3
```

⚠️Rest 파라미터와 형태는 동일하지만 다른 문법이므로 주의하자. 둘은 서로 반대의 개념이다.

- Rest 파라미터: 함수에 전달된 인수들의 목록을 배열로 전달받기 위해 `...`을 붙임
- 스프레드 문법: 배열 같은 이터러블을 펼쳐서 개별 값의 목록을 만듦

## 35.2 배열 리터럴 내부에서 사용하는 경우

스프레드가 없던 ES5에서는 어떤 식으로 사용했는지 비교하여 살펴보자.

### `concat`

여러 배열을 하나로 결합하려면 `concat`를 사용했어야 했다.

```js
// ES5
var arr = [1, 2].concat([3, 4]);
console.log(arr); // [1, 2, 3, 4]
```

스프레드 문법을 사용하면 배열 리터럴만으로 2개의 배열을 하나로 결합할 수 있다.

```js
// ES6
const arr = [...[1, 2], ...[3, 4]];
console.log(arr); // [1, 2, 3, 4]
```

### `splice`

배열 중간에 다른 배열의 요소를 추가하거나 제거하려면 `splice`를 사용한다. 이때 **세 번째 인수로 배열을 전달하면 배열 자체가 추가된다**.

이 배열을 펼쳐서 전달하기 위해서는 다음과 같이 작성해줄 수 있었다.

```js
// ES5
var arr1 = [1, 4];
var arr2 = [2, 3];

/*
apply 메서드의 2번째 인수(배열)는 apply 메서드가 호출한 splice 메서드의 인수 목록이다.
apply 메서드의 2번째 인수 [1, 0].concat(arr2)는 [1, 0, 2, 3]으로 평가된다.
따라서 splice 메서드에 apply 메서드의 2번째 인수 [1, 0, 2, 3]이 해체되어 전달된다.
즉, arr1[1]부터 0개의 요소를 제거하고 그 자리(arr1[1])에 새로운 요소(2, 3)를 삽입한다.
*/
Array.prototype.splice.apply(arr1, [1, 0].concat(arr2));
console.log(arr1); // [1, 2, 3, 4]
```

스프레드 문법을 사용하면 다음과 같이 작성할 수 있다.

```js
// ES6
const arr1 = [1, 4];
const arr2 = [2, 3];

arr1.splice(1, 0, ...arr2);
console.log(arr1); // [1, 2, 3, 4]
```

### 배열 복사

ES5에서 배열을 복사하려면 `slice` 메서드를 사용할 수 있다.

```js
// ES5
var origin = [1, 2];
var copy = origin.slice();

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

스프레드 문법을 사용하면 다음과 같이 작성할 수 있다.

```js
// ES6
const origin = [1, 2];
const copy = [...origin];

console.log(copy); // [1, 2]
console.log(copy === origin); // false
```

### 이터러블을 배열로 반환

ES5에서 이터러블을 배열로 반환하려면 `apply` 또는 `call` 메서드를 사용해서 `slice` 메서드를 호출해야 한다.

```js
// ES5
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
  var args = Array.prototype.slice.call(arguments);

  return args.reduce(function (pre, cur) {
    return pre + cur;
  }, 0);
}

console.log(sum(1, 2, 3)); // 6
```

```js
// 이터러블이 아닌 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

const arr = Array.prototype.slice.call(arrayLike); // -> [1, 2, 3]
console.log(Array.isArray(arr)); // true
```

스프레드 문법으로 간편하게 이터러블을 배열로 변환할 수 있다.

```js
// ES6
function sum() {
  // 이터러블이면서 유사 배열 객체인 arguments를 배열로 변환
  return [...arguments].reduce((pre, cur) => pre + cur, 0);
}

console.log(sum(1, 2, 3)); // 6
```

단, 유사 배열 객체는 스프레드 문법의 대상이 될 수 없다. 유사 배열 객체를 배열로 변환하려면 `Array.from` 메서드를 사용한다.

```js
// 이터러블이 아닌 유사 배열 객체
const arrayLike = {
  0: 1,
  1: 2,
  2: 3,
  length: 3,
};

const arr = [...arrayLike];
// TypeError: object is not iterable (cannot read property Symbol(Symbol.iterator))

const arr = Array.from(arrayLike); // -> [1, 2, 3]
```

## 35.3 객체 리터럴 내부에서 사용하는 경우

ES9에서 도입된 스프레드 프로퍼티를 사용하면 객체 리터럴의 프로퍼티 목록에서도 스프레드 문법을 사용할 수 있다.

스프레드 문법의 대상은 이터러블이어야 하지만, 스프레드 프로퍼티는 일반 객체를 대상으로도 스프레드 문법을 사용할 수 있게 한다.

```js
// 객체 복사(얕은 복사)
const obj = { x: 1, y: 2 };
const copy = { ...obj };
console.log(copy); // { x: 1, y: 2 }
console.log(obj === copy); // false

// 객체 병합
const merged = { x: 1, y: 2, ...{ a: 3, b: 4 } };
console.log(merged); // { x: 1, y: 2, a: 3, b: 4 }
```

스프레드 프로퍼티 이전에는 ES6에서 도입된 `Object.assign` 메서드를 사용해서 여러 개의 객체를 병합하거나 특정 프로퍼티를 변경 또는 추가했다.

```js
// 객체 병합. 프로퍼티가 중복되는 경우, 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = Object.assign({}, { x: 1, y: 2 }, { y: 10, z: 3 });
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = Object.assign({}, { x: 1, y: 2 }, { y: 100 });
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = Object.assign({}, { x: 1, y: 2 }, { z: 0 });
console.log(added); // { x: 1, y: 2, z: 0 }
```

ES9에서는 스프레드 프로퍼티를 사용할 수 있다.

```js
// 객체 병합. 프로퍼티가 중복되는 경우, 뒤에 위치한 프로퍼티가 우선권을 갖는다.
const merged = { ...{ x: 1, y: 2 }, ...{ y: 10, z: 3 } };
console.log(merged); // { x: 1, y: 10, z: 3 }

// 특정 프로퍼티 변경
const changed = { ...{ x: 1, y: 2 }, y: 100 };
// changed = { ...{ x: 1, y: 2 }, ...{ y: 100 } }
console.log(changed); // { x: 1, y: 100 }

// 프로퍼티 추가
const added = { ...{ x: 1, y: 2 }, z: 0 };
// added = { ...{ x: 1, y: 2 }, ...{ z: 0 } }
console.log(added); // { x: 1, y: 2, z: 0 }
```
