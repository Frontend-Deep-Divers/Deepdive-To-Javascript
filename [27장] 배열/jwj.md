# 27장 배열

## 27.1 배열이란?

배열은 여러 개의 값을 순차적으로 나열한 자료구조다.

```js
const arr = [1, 2, 3];
```

자바스크립트의 모든 값은 배열의 요소가 될 수 있다. 값으로 인정하는 모든 것을 요소로 넣을 수 있다.

배열은 요소의 개수를, 즉 배열의 길이를 나타내는 **`length` 프로퍼티**를 갖는다.

배열 생성 방법은 4가지가 있다.

1. 배열 리터럴
2. `Array` 생성자 함수
3. `Array.of` 메서드
4. `Array.from` 메서드

배열의 생성자 함수는 `Array`, 프로토타입 객체는 `Array.prototype`이다. `Array.prototype`은 배열을 위한 빌트인 메서드를 제공한다.

일반 객체와 배열의 차이점은 값의 순서 유무, `length` 프로퍼티의 유무다.

## 27.2 자바스크립트 배열은 배열이 아니다.

흔히 자료구조에서 말하는 배열은 하나의 데이터 타입으로 구성되며 서로 연속적으로 인접해 있다. (= **밀집 배열**)

때문에 보통 배열은 인덱스를 통해 단 한 번의 연산으로 임의의 요소에 접근(임의 접근, 시간복잡도 O(1))할 수 있다.

하지만 정렬되지 않은 배열에서 요소를 찾으려면 선형 검색(O(n))해야 하며, 삽입하거나 삭제하는 경우 요소를 이동시켜야 한다.

하지만 자바스크립트의 배열은 데이터 타입도 제각각이며 연속적으로 이어져 있지 않을 수도 있다. (= **희소 배열**) 자바스크립트의 배열은 일반적인 배열을 흉내내는 객체다.

자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지며, `length` 프로퍼티를 갖는 특수 객체다. 배열의 요소는 프로퍼티 값이 된다.

```js
// "16.2. 프로퍼티 어트리뷰트와 프로퍼티 디스크립터 객체" 참고
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
/*
{
  '0': {value: 1, writable: true, enumerable: true, configurable: true}
  '1': {value: 2, writable: true, enumerable: true, configurable: true}
  '2': {value: 3, writable: true, enumerable: true, configurable: true}
  length: {value: 3, writable: true, enumerable: false, configurable: false}
}
*/
```

자바스크립트 배열은 해시 테이블로 구현되어 있어 인덱스로 요소에 접근하는 경우 일반적인 배열보다 느릴 수 밖에 없다. 하지만 요소를 삽입, 삭제하는 경우 일반적으로 더 빠를 수 있다.

구조적인 단점을 보완하기 위해 모던 자바스크립트 엔진은 배열을 일반적인 배열처럼 동작하도록 최적화하여 구현하고 있다. 이로써 일반 객체의 성능의 약 2배 정도 빠를 수 있게 되었다.

## 27.3 `length` 프로퍼티와 희소 배열

`length` 프로퍼티의 값은 0 ~ $2^{32}$-1(4,294,967,295) 미만의 양의 정수다. 이것이 자바스크립트 배열의 인덱스의 범위가 된다.

명시적으로 원래 `length` 프로퍼티의 값보다 작은 값을 할당하면 배열의 길이가 줄어든다.

⚠️만약 큰 값을 할당하면 값이 변경되지만 배열의 길이가 늘어나지는 않는다.

**희소 배열 특성 때문에 `length` 프로퍼티의 값은 배열의 요소의 개수와 일치하지 않을 수 있다.**

성능 상의 이슈가 있을 수 있기 때문에 희소 배열은 생성하지 말아야 하며, **배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선이다**.

## 27.4 배열 생성

### 배열 리터럴

```js
const arr = [1, 2, 3];
```

대괄호를 사용하여 배열을 선언할 수 있다.

### `Array` 생성자 함수

```js
const arr = new Array(3);
```

이떄 생성된 배열은 희소 배열이다.

전달된 인수가 없는 경우 빈 배열을 생성한다.

전달된 인수가 2개 이상이거나 숫자가 아닌 경우 인수를 요소로 갖는 배열을 생성한다.

`new`와 함께 호출하지 않아도 동일하게 동작한다.

### `Array.of`

```js
Array.of(1);
```

ES6에서 도입되었다. 전달된 인수를 요소로 갖는 배열을 생성한다.

전달된 인수가 1개이고 숫자이더라도 인수를 요소로 갖는 배열을 생성한다.

### `Array.from`

```js
Array.from({ length: 2, 0: "a", 1: "b" });
Array.from("Hello");
```

ES6에서 도입되었다. 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환한다.

두 번째 인수로 콜백 함수를 전달해 값을 만들면서 요소를 채울 수도 있다.

```js
Array.from({ length: 3 }); // [undefined, undefined, undefined]
Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```

## 27.5 배열 요소의 참조

대괄호 표기법을 통해 참조할 수 있다.

정수로 평가되는 표현식이라면 인덱스를 대신할 수 있다.

존재하지 않는 요소에 접근하면 `undefined`가 반환된다.

## 27.6 배열 요소의 추가와 갱신

동적으로 배열의 요소를 추가할 수 있다.

```js
const arr = [0];

// 배열 요소의 추가
arr[1] = 1;

console.log(arr); // [0, 1]
console.log(arr.length); // 2

arr[100] = 100;

console.log(arr); // [0, 1, empty × 98, 100]
console.log(arr.length); // 101
```

⚠️인덱스로 요소에 접근해 명시적으로 값을 할당하지 않은 요소는 생성되지 않는다.

이미 요소가 있는 인덱스로 값을 재할당하면 요소값이 갱신된다.

⚠️만약 정수 이외의 값을 인덱스처럼 사용하면 프로퍼티가 생성된다. 이때 `length` 프로퍼티 값에 영향을 주지 않는다.

```js
const arr = [];

// 배열 요소의 추가
arr[0] = 1;
arr["1"] = 2;

// 프로퍼티 추가
arr["foo"] = 3;
arr.bar = 4;
arr[1.1] = 5;
arr[-1] = 6;

console.log(arr); // [1, 2, foo: 3, bar: 4, '1.1': 5, '-1': 6]

// 프로퍼티는 length에 영향을 주지 않는다.
console.log(arr.length); // 2
```

## 27.7 배열 요소의 삭제

배열도 객체이므로 `delete`를 사용할 수 있지만, 희소 배열이 되며 `length` 프로퍼티 값도 변하지 않아 사용하지 않는 것이 좋다.

일반 배열처럼 삭제하기 위해서는 `Array.prototype.splice` 메서드를 사용해야 한다.

## 27.8 배열 메서드

배열 메서드는 결과물을 반환하는 패턴이 두 가지이므로 주의해야 한다.

1. 원본 배열(배열 메서드를 호출한 배열, 즉 배열 메서드의 구현체 내부에서 `this`가 가리키는 객체)을 직접 변경하는 메서드(mutator method) (\*으로 구분)
2. 원본 배열을 직접 변경하지 않고 새로운 배열을 생성하여 반환하는 메서드(accessor method)

ES5부터 도입된 배열 메서드는 대부분 원본 배열을 직접 변경하지 않고, 초창기 배열 메서드는 원본 배열을 변경하는 경우가 많다.

원본 배열을 변경하는 메서드는 부수 효과를 일으킬 수 있으므로 주의해야 한다. 따라서 가급적 accessor 메서드를 사용하는 편이 좋다.

### `Array.isArray`

전달된 인수가 배열이면 `true`, 아니면 `false`를 반환한다.

### `Array.prototpye.indexOf`

원본 배열에서 인수로 전달된 요소를 검색하여 인덱스를 반환한다.

요소가 여러 개 있으면 첫 번쨰로 검색된 요소의 인덱스를, 존재하지 않으면 -1을 반환한다.

ES7에서는 `Array.prototype.includes` 메서드를 사용하면 좋다.

### `Array.prototype.push`\*

인수로 전달받은 모든 값을 원본 배열의 마지막 요소로 추가하고 변경된 `lenght` 프로퍼티 값을 반환한다.

성능 면에서 좋지 않아 아래 방법을 사용하는 것이 성능 상 빠르다.

```js
arr[arr.length] = 3;
```

ES6의 스프레드 문법을 사용하면 부수 효과를 없앨 수 있다.

```js
const newArr = arr[...arr, 3];
```

### `Array.prototype.pop`\*

원본 배열에서 마지막 요소를 제거하고 제거한 요소를 반환한다. 빈 배열이면 `undefined`를 반환한다.

`push`, `pop` 메서드로 스택을 구현할 수 있다.

```js
class Stack {
  #array; // private class member

  constructor(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.#array = array;
  }

  // 스택의 가장 마지막에 데이터를 밀어 넣는다.
  push(value) {
    return this.#array.push(value);
  }

  // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼낸다.
  pop() {
    return this.#array.pop();
  }

  // 스택의 복사본 배열을 반환한다.
  entries() {
    return [...this.#array];
  }
}

const stack = new Stack([1, 2]);
console.log(stack.entries()); // [1, 2]

stack.push(3);
console.log(stack.entries()); // [1, 2, 3]

stack.pop();
console.log(stack.entries()); // [1, 2]
```

### `Array.prototype.unshift`\*

인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 `length` 프로퍼티 값을 반환한다.

이 방법 역시 스프레드 문법으로 부수 효과를 없애는 편이 좋다.

### `Array.prototype.shift`\*

원본 배열에서 첫 번째 요소를 제거하고 제거한 요소를 반환한다. 빈 배열의 경우 `undefined`를 반환한다.

`shift`와 `push` 메서드를 사용하면 큐를 구현할 수 있다.

```js
class Queue {
  #array; // private class member

  constructor(array = []) {
    if (!Array.isArray(array)) {
      throw new TypeError(`${array} is not an array.`);
    }
    this.#array = array;
  }

  // 큐의 가장 마지막에 데이터를 밀어 넣는다.
  enqueue(value) {
    return this.#array.push(value);
  }

  // 큐의 가장 처음 데이터, 즉 가장 먼저 밀어 넣은 데이터를 꺼낸다.
  dequeue() {
    return this.#array.shift();
  }

  // 큐의 복사본 배열을 반환한다.
  entries() {
    return [...this.#array];
  }
}

const queue = new Queue([1, 2]);
console.log(queue.entries()); // [1, 2]

queue.enqueue(3);
console.log(queue.entries()); // [1, 2, 3]

queue.dequeue();
console.log(queue.entries()); // [2, 3]
```

### `Array.prototype.concat`

인수로 전달된 값들(배열 또는 원시값)을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환한다.

배열을 받으면 해체하여 새로운 배열의 요소로 추가한다.

`push`와 `unshift` 메서드를 대체할 수 있다.

```js
const arr1 = [3, 4];

// unshift 메서드는 원본 배열을 직접 변경한다.
// 따라서 원본 배열을 변수에 저장해 두지 않으면 변경된 배열을 사용할 수 없다.
arr1.unshift(1, 2);
// unshift 메서드를 사용할 경우 원본 배열을 반드시 변수에 저장해 두어야 결과를 확인할 수 있다.
console.log(arr1); // [1, 2, 3, 4]

// push 메서드는 원본 배열을 직접 변경한다.
// 따라서 원본 배열을 변수에 저장해 두지 않으면 변경된 배열을 사용할 수 없다.
arr1.push(5, 6);
// push 메서드를 사용할 경우 원본 배열을 반드시 변수에 저장해 두어야 결과를 확인할 수 있다.
console.log(arr1); // [1, 2, 3, 4, 5, 6]

// unshift와 push 메서드는 concat 메서드로 대체할 수 있다.
const arr2 = [3, 4];

// concat 메서드는 원본 배열을 변경하지 않고 새로운 배열을 반환한다.
// arr1.unshift(1, 2)를 다음과 같이 대체할 수 있다.
let result = [1, 2].concat(arr2);
console.log(result); // [1, 2, 3, 4]

// arr1.push(5, 6)를 다음과 같이 대체할 수 있다.
result = result.concat(5, 6);
console.log(result); // [1, 2, 3, 4, 5, 6]
```

스프레드 문법으로 대체할 수 있어 스프레드 문법을 일관성 있게 사용하는 것이 낫다.

### `Array.prototype.splice`\*

원본 배열의 중간에 요소를 추가, 삭제할 수 있다. 3개의 매개변수를 받는다.

- `start`: 제거를 시작할 인덱스다. -1이면 배열의 끝 인덱스를 나타낸다. -n의 경우 끝에서 n번째 요소를 가리킨다.
- `deleteCount`: `start`부터 제거할 요소의 개수다. 0이면 제거하지 않는다. (옵션)
- `items`: 제거한 위치에 삽입할 요소들의 목록이다. 생략할 경우 제거만 한다. (옵션)

```js
const arr = [1, 2, 3, 4];

// 원본 배열의 인덱스 1부터 2개의 요소를 제거하고 그 자리에 새로운 요소 20, 30을 삽입한다.
const result = arr.splice(1, 2, 20, 30);

// 제거한 요소가 배열로 반환된다.
console.log(result); // [2, 3]
// splice 메서드는 원본 배열을 직접 변경한다.
console.log(arr); // [1, 20, 30, 4]
```

💡배열에서 특정 요소 제거하려면?

```js
const arr = [1, 2, 3, 1, 2];

// 배열 array에서 item 요소를 제거한다. item 요소가 여러 개 존재하면 첫 번째 요소만 제거한다.
function remove(array, item) {
  // 제거할 item 요소의 인덱스를 취득한다.
  const index = array.indexOf(item);

  // 제거할 item 요소가 있다면 제거한다.
  if (index !== -1) array.splice(index, 1);

  return array;
}

console.log(remove(arr, 2)); // [1, 3, 1, 2]
console.log(remove(arr, 10)); // [1, 3, 1, 2]
```

💡`filter` 메서드를 사용해서 특정 요소를 제거할 수도 있다. 중복된 요소를 모두 제거한다.

```js
function removeAll(array, item) {
  return array.filter((v) => v !== item);
}
```

### `Array.prototype.slice`

인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다.

- `start`: 복사를 시작할 인덱스다.
- `end`: 복사를 종료할 인덱스다. 이 인덱스의 요소는 복사되지 않는다. 생략하면 `start`부터 끝까지 복사된다.

`start`가 음수인 경우 배열의 끝에서 부터 복사한다.

모든 인수를 생략하면 원본 배열의 복사본을 반환한다. (**얕은 복사**)

`slice`를 통해 `arguments`, `HTMLCollections`, `NodeList` 같은 유사 배열 객체를 배열로 변환할 수 있다. 하지만 `Array.from` 메서드를 사용하는 것이 더 편하다.

### `Array.prototype.join`

원본 배열의 모든 요소를 문자열로 변환한 후 인수로 전달받은 문자열, 즉 구분자로 연결한 문자열을 반환한다. 구분자는 생략할 수 있고 디폴트는 콤마(`,`)다.

```js
const arr = [1, 2, 3, 4];
arr.join(" "); // 1 2 3 4
```

### `Array.prototype.reverse`\*

원본 배열의 순서를 반대로 뒤집는다.

### `Array.prototype.fill`\*

ES6에서 추가되었다. 인수로 전달받은 값을 배열의 처음부터 끝까지 요소로 채운다.

```js
const arr = [1, 2, 3, 4];
arr.fill(0, 1, 3);
console.log(arr); // [1, 0, 0, 4]
```

💡`fill` 메서드를 사용하여 배열을 생성하면서 특정 값으로 요소를 초기화할 수 있다.

💡`Array.from` 메서드는 요소값을 만들면서 채울 수 있다.

### `Array.prototype.includes`

ES7에서 추가되었다. 배열 내 특정 요소 포함 여부를 확인하여 `true` 혹은 `false`를 반환한다.

두 번째 인자로 검색을 시작할 인덱스를 설정할 수 있다. 음수를 전달하면 배열의 끝부터 검색한다.

💡`indexOf` 메서드와 다르게 `NaN` 포함 여부를 확인할 수 있다.

### `Array.prototype.flat`

ES10에서 추가되었다. 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다.

```js
[1, [2, 3, 4, 5]].flat(); // [1, 2, 3, 4, 5]
```

인수로 평탄화할 깊이를 설정할 수 있다. 생략할 경우 1이 된다. `Infinity`를 전달하면 중첩 배열 모두를 평탄화한다.

## 27.9 배열 고차 함수

### `Array.prototype.sort`\*

배열의 요소를 오름차순 정렬한다.

💡내림차순 정렬은 한번 `sort` 한 후, `reverse` 메서드를 사용하면 된다.

⚠️기본 정렬 순서는 유니코드 코드 포인트의 순서를 따르기 때문에 숫자 요소 정렬은 주의가 필요하다.

따라서 숫자 정렬 시, 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다.

```js
arr.sort((a, b) => a - b);
```

💡객체를 요소로 갖는 배열을 정렬하려면?

```js
const todos = [
  { id: 4, content: "JavaScript" },
  { id: 1, content: "HTML" },
  { id: 2, content: "CSS" },
];

// 비교 함수. 매개변수 key는 프로퍼티 키다.
function compare(key) {
  // 프로퍼티 값이 문자열인 경우 - 산술 연산으로 비교하면 NaN이 나오므로 비교 연산을 사용한다.
  // 비교 함수는 양수/음수/0을 반환하면 되므로 - 산술 연산 대신 비교 연산을 사용할 수 있다.
  return (a, b) => (a[key] > b[key] ? 1 : a[key] < b[key] ? -1 : 0);
}

// id를 기준으로 오름차순 정렬
todos.sort(compare("id"));
console.log(todos);
/*
[
  { id: 1, content: 'HTML' },
  { id: 2, content: 'CSS' },
  { id: 4, content: 'JavaScript' }
]
*/

// content를 기준으로 오름차순 정렬
todos.sort(compare("content"));
console.log(todos);
/*
[
  { id: 2, content: 'CSS' },
  { id: 1, content: 'HTML' },
  { id: 4, content: 'JavaScript' }
]
*/
```

> `sort` 메서드는 quicksort를 사용했었으나 동일한 값의 요소가 중복되어 있을 때 초기 순서와 변경될 수 있는 불안정한 정렬 알고리즘이기에 ES10에서 timsort를 사용하도록 바뀌었다.

### `Array.prototype.forEach`

`for` 문을 함수형 프로그래밍의 의도에 맞게 설계한 함수다. (조건문, 반복문을 제거하고 변수를 억제함)

반복문을 추상화하여 내부에서 반복문을 통해 자신을 호출한 배열을 순회하면서 수행해야 할 처리를 콜백 함수로 전달받아 반복 호출한다.

배열의 요소값과 인덱스, `forEach` 메서드를 호출한 배열 자체, 즉 `this`를 순차적으로 전달받을 수 있다.

원본 배열을 변경하지 않지만 콜백 함수에 원본 배열을 전달하면 변경된다.

```js
const numbers = [1, 2, 3];

// forEach 메서드는 원본 배열을 변경하지 않지만 콜백 함수를 통해 원본 배열을 변경할 수는 있다.
// 콜백 함수의 세 번째 매개변수 arr은 원본 배열 numbers를 가리킨다.
// 따라서 콜백 함수의 세 번째 매개변수 arr을 직접 변경하면 원본 배열 numbers가 변경된다.
numbers.forEach((item, index, arr) => {
  arr[index] = item ** 2;
});
console.log(numbers); // [1, 4, 9]
```

반환값은 언제나 `undefined`다.

두 번째 인수로 콜백 함수 내부에서 `this`로 사용할 객체를 전달할 수 있다.

```js
class Numbers {
  numberArray = [];

  multiply(arr) {
    arr.forEach(function (item) {
      this.numberArray.push(item * item);
    }, this); // forEach 메서드의 콜백 함수 내부에서 this로 사용할 객체를 전달
  }
}

const numbers = new Numbers();
numbers.multiply([1, 2, 3]);
console.log(numbers.numberArray); // [1, 4, 9]
```

> 💡화살표 함수를 사용하면, `this`를 전달해줄 필요가 없다.

`for` 문과 달리 제어 `break`, `continue` 문을 사용할 수 없다. (모든 요소 순회, 순회를 중간에 중단 불가)

희소 배열의 경우 존재하지 않는 요소는 순회하지 않는다.

`for` 문에 비해 성능은 좋지 않지만 가독성은 좋다. 때문에 대단히 많은 요소를 순회하는게 아니라면 `forEach` 문을 쓰는 것도 좋다.

### `Array.prototype.map`

자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 **콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.**

`forEach`, `map` 모두 모든 요소를 순회한다. 하지만 `forEach`는 `undefined`만 반환하고 `map`은 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다.

새로운 배열의 `length` 프로퍼티 값은 호출한 배열의 것과 반드시 일치한다. 즉, 두 배열은 1:1 매핑된다.

두 번째 인수로 콜백 함수 내부에서 `this`로 사용할 객체를 전달할 수 있다.

### `Array.prototype.filter`

자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 **콜백 함수의 반환값이 `true`인 요소로만 구성된 새로운 배열을 반환한다.**

새로운 배열의 `length` 프로퍼티 값은 호출한 배열의 것에 비해 작거나 같다.

두 번째 인수로 콜백 함수 내부에서 `this`로 사용할 객체를 전달할 수 있다.

### `Array.prototype.reduce`

자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 그리고 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 **하나의 결과값을 만들어 반환**한다.

첫 번째 인수로 콜백 함수, 두 번째 인수로 초기값을 전달받는다.

콜백 함수는 4개의 인수를 받는데,

1. 초기값 또는 콜백 함수의 이전 반환값
2. `reduce` 메서드를 호출한 배열의 요소값
3. `reduce` 메서드를 호출한 배열의 인덱스
4. `reduce` 메서드를 호출한 배열 자체, 즉 `this`가 전달된다.

```js
// [1, 2, 3, 4]의 모든 요소의 누적을 구한다.
const sum = [1, 2, 3, 4].reduce(
  (accumulator, currentValue, index, array) => accumulator + currentValue,
  0
);

console.log(sum); // 10
```

💡`reduce` 메서드로 평균 구하기, 최대값 구하기, 요소의 중복 횟수 구하기, 중첩 배열 평탄화, 중복 요소 제거가 가능하다.

💡중복 요소 제거는 `Set`으로 하는 것이 좋다.

**⚠️`reduce` 메서드의 두 번째 인자를 생략할 수도 있다. 하지만 언제나 초기값을 전달하는 것이 안전하다.**

```js
const products = [
  { id: 1, price: 100 },
  { id: 2, price: 200 },
  { id: 3, price: 300 },
];

// 1번째 순회 시 acc는 { id: 1, price: 100 }, cur은 { id: 2, price: 200 }이고
// 2번째 순회 시 acc는 300, cur은 { id: 3, price: 300 }이다.
// 2번째 순회 시 acc에 함수에 객체가 아닌 숫자값이 전달된다. 이때 acc.price는 undefined다.
const priceSum = products.reduce((acc, cur) => acc.price + cur.price);

console.log(priceSum); // NaN
```

이처럼 객체의 특정 프로퍼티 값을 합산하는 경우 반드시 초기값을 전달해줘야 한다.

### `Array.prototype.some`

자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 콜백 함수의 반환값이 단 한 번이라도 참이면 `true`, 모두 거짓이면 `false`를 반환한다.

두 번째 인수로 콜백 함수 내부에서 `this`로 사용할 객체를 전달할 수 있다.

### `Array.prototype.every`

자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 반복 호출한다. 콜백 함수의 반환값이 모두 참이면 `true`, 하나라도 거짓이면 `false`를 반환한다.

⚠️빈 배열의 경우 언제나 `true`를 반환한다.

두 번째 인수로 콜백 함수 내부에서 `this`로 사용할 객체를 전달할 수 있다.

### `Array.prototype.find`

ES6에서 추가되었다. 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 호출하여 반환값이 `true`인 첫 번째 요소를 반환한다. 만약 존재하지 않으면 `undefined`를 반환한다.

`filter` 메서드와 다르게 `find` 메서드는 배열이 아닌 요소값을 반환한다.

두 번째 인수로 콜백 함수 내부에서 `this`로 사용할 객체를 전달할 수 있다.

### `Array.prototype.findIndex`

ES6에서 추가되었다. 자신을 호출한 배열의 모든 요소를 순회하면서 인수로 전달받은 콜백 함수를 호출하여 반환값이 `true`인 첫 번째 요소의 인덱스를 반환한다. 만약 존재하지 않으면 -1을 반환한다.

```js
const users = [
  { id: 1, name: "Lee" },
  { id: 2, name: "Kim" },
  { id: 2, name: "Choi" },
  { id: 3, name: "Park" },
];

// id가 2인 요소의 인덱스를 구한다.
users.findIndex((user) => user.id === 2); // -> 1

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex((user) => user.name === "Park"); // -> 3

// 위와 같이 프로퍼티 키와 프로퍼티 값으로 요소의 인덱스를 구하는 경우
// 다음과 같이 콜백 함수를 추상화할 수 있다.
function predicate(key, value) {
  // key와 value를 기억하는 클로저를 반환
  return (item) => item[key] === value;
}

// id가 2인 요소의 인덱스를 구한다.
users.findIndex(predicate("id", 2)); // -> 1

// name이 'Park'인 요소의 인덱스를 구한다.
users.findIndex(predicate("name", "Park")); // -> 3
```

두 번째 인수로 콜백 함수 내부에서 `this`로 사용할 객체를 전달할 수 있다.

### `Array.prototype.flatMap`

ES10에서 추가되었다. `map` 메서드를 통해 생성된 새로운 배열을 평탄화한다. 즉, `map` 메서드와 `flat` 메서드를 순차적으로 실행하는 효과를 가진다.

`flat` 메서드처럼 평탄화 깊이를 지정할 수는 없다.

```js
const arr = ["hello", "world"];

// flatMap은 1단계만 평탄화한다.
arr.flatMap((str, index) => [index, [str, str.length]]);
// -> [[0, ['hello', 5]], [1, ['world', 5]]] => [0, ['hello', 5], 1, ['world', 5]]

// 평탄화 깊이를 지정해야 하면 flatMap 메서드를 사용하지 말고 map 메서드와 flat 메서드를 각각 호출한다.
arr.map((str, index) => [index, [str, str.length]]).flat(2);
// -> [[0, ['hello', 5]], [1, ['world', 5]]] => [0, 'hello', 5, 1, 'world', 5]
```
