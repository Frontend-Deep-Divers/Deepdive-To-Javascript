# 배열이란?

여러 개의 값을 순차적으로 나열한 자료구조

배열의 길이를 나타내는 length 프로퍼티를 가짐

객체 타입

객체와 배열을 구분하는 가장 명확한 차이는 값의 순서와 length 프로퍼티

# 자바스크립트 배열은 배열이 아니다

자료구조에서 말하는 배열은 밀집 배열 Dense Array - 임의의 요소에 접근하는 시간 복잡도 O(1)

자바스크립트 배열은 요소의 메모리 공간이 각각 다를 수 있고, 연속적으로 이어져 있지 않을 수도 있는 희소 배열 Sparse Array

자바스크립트 배열은 일반 배열의 동작을 흉내 낸 특수한 객체

```jsx
console.log(Object.getOwnPropertyDescriptors([1, 2, 3]));
/*
{
  '0': { value: 1, writable: true, enumerable: true, configurable: true },
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '2': { value: 3, writable: true, enumerable: true, configurable: true },
  length: { value: 3, writable: true, enumerable: false, configurable: false }
}
*/
```

인덱스를 나타내는 문자열을 프로퍼티 키로 가지며, length 프로퍼티를 갖는 특수한 객체

따라서 임의의 요소에 접근하는 경우 일반 배열보다 느린 구조면서, 요소를 삽입 또는 삭제하는 경우에는 일반 배열보다 빠를 수 있음

모던 자바스크립트 엔진은 배열을 더 배열처럼 동작하도록 최적화하여 구현

# length 프로퍼티와 희소 배열

length 프로퍼티 값은 자동 갱신되며, 직접 할당할 수도 있음

length 프로퍼티 값을 직접 할당하면 배열의 길이가 줄어들거나 늘어남

```jsx
const arr = [1, 2, 3];

arr.length = 2;

console.log(Object.getOwnPropertyDescriptors(arr));

arr.length = 3;

console.log(Object.getOwnPropertyDescriptors(arr));

/*
{
  '0': { value: 1, writable: true, enumerable: true, configurable: true },
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  length: { value: 2, writable: true, enumerable: false, configurable: false }
}
*/

// length를 늘려도 요소를 위한 메모리 공간이 확보되진 않음!
/*
{
  '0': { value: 1, writable: true, enumerable: true, configurable: true },
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  length: { value: 3, writable: true, enumerable: false, configurable: false }
}
*/
```

희소 배열을 문법적으로 허용함

```jsx
const arr = [, 2, , 4];

console.log(Object.getOwnPropertyDescriptors(arr));
/*
{
  '1': { value: 2, writable: true, enumerable: true, configurable: true },
  '3': { value: 4, writable: true, enumerable: true, configurable: true },
  length: { value: 4, writable: true, enumerable: false, configurable: false }
}
*/
```

희소 배열을 사용하지 않는 것이 좋음 → 같은 타입의 요소를 연속적으로 위치시키는 것이 최선

# 배열 생성

### 배열 리터럴

```jsx
const arr = [1,2,3];
```

### Array 생성자 함수

- 인수가 1개이고 숫자인 경우

```jsx
const arr = new Array(10);
// [empty * 10]
```

- 인수가 없는 경우

```jsx
const arr = new Array();
// []
```

- 인수가 2개 이상이거나 숫자가 아닌 경우

```jsx
const arr = new Array(2, 3);
// [2, 3]

const arr = new Array('asdf');
// ['asdf']
```

일반 함수로 호출해도 똑같이 동작함

### Array.of

인수를 요소로 갖는 배열 생성

```jsx
Array.of(1, 2, 3);
// [1, 2, 3]
```

### Array.from

유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열 생성

```jsx
Array.from({length: 2, 0: 1, 1: 2});
// [1, 2]

Array.from('Hello');
// ['H', 'e', 'l', 'l', 'o']
```

두 번째 인수로 전달한 콜백 함수를 통해 값을 만들면서 요소를 채울 수 있음

```jsx
Array.from({ length: 3 }); // [undefined, undefined, undefined]

Array.from({ length: 3 }, (_, i) => i); // [0, 1, 2]
```

유사 배열 객체: 인덱스로 프로퍼티 값에 접근할 수 있고, length 프로퍼티를 갖는 객체

# 배열 요소의 추가와 갱신

배열에 요소를 동적으로 추가/수정할 수 있음

```jsx
arr[3] = 3;

// length는 자동 갱신됨
```

# 배열 요소의 삭제

```jsx
const arr = [1, 2, 3];

delete arr[1];
// [1, empty, 2]
// 희소 배열이 만들어지기 때문에 delete를 사용하면 안됨
// 대신 Array.prototype.splice를 활용
```

# 배열 메서드

### Array.isArray

인수가 배열이면 true

### Array.prototype.indexOf

인수로 전달된 요소를 검색하여 첫 번째로 검색된 인덱스 반환, 존재하지 않으면 -1

ES7 이후 → Array.prototype.includes 사용할 수 있음

### 🔍 Array.prototype.push

인수로 전달된 모든 값을 원본 배열의 마지막 요소로 추가하고 length 반환

성능이 좋지 않아서 length 프로퍼티를 사용하여 배열의 마지막 요소를 직접 추가하는 것이 좋음

원본 배열을 변경하기 때문에 스프레드 문법을 사용할 것

### 🔍 Array.prototype.pop

마지막 요소를 제거하고 반환

### 🔍 Array.prototype.unshift

인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 length 반환

원본 배열을 변경하기 때문에 스프레드 문법을 사용할 것

### 🔍 Array.prototype.shift

첫 요소를 제거하고 반환

### Array.prototype.concat

인수로 전달된 값들을 원본 배열의 마지막 요소로 추가한 새로운 배열을 반환

인수로 배열을 받으면 배열을 해체하여 요소로 추가

스프레드 문법을 사용하는 것이 좋음

### 🔍 Array.prototype.splice

원본 배열의 중간에 요소를 추가하거나 중간에 있는 요소를 제거하는 경우 사용

3개의 매개변수가 있음

- start: 원본 배열의 요소를 제거하기 시작할 인덱스, 음수인 경우 배열의 끝에서의 인덱스
- deleteCount: 제거할 요소의 개수, 0이면 제거하지 않음, 생략하면 start 부터 모든 요소를 삭제
- items: 삽입할 요소들의 목록, 생략하면 제거만 함

### Array.prototype.slice

인수로 전달된 범위의 요소들을 복사하여 배열로 반환

인수를 모두 생략하면 원본 배열의 복사본을 생성하여 반환

얕은 복사를 진행함

### Array.prototype.join

원본 배열의 모든 요소를 문자열로 변환한 후 인수로 전달받은 문자열을 구분자로 연결한 문자열을 반환

매개변수 생략 시 콤마가 구분자가 됨

### 🔍 Array.prototype.reverse

원본 배열의 순서를 반대로 뒤집음

### 🔍 Array.prototype.fill

```jsx
arr.fill(채울 값, 채우기 시작할 인덱스, 채우기를 멈출 인덱스)
```

### Array.prototype.includes

```jsx
arr.includes(찾을 요소, 찾기 시작할 인덱스)
```

### Array.prototype.flat

```jsx
arr.flat(평탄화할 깊이) // Infinity를 전달하면 중첩 배열 모두 평탄화
```

# 배열 고차 함수

함수를 인수로 전달하거나 함수를 반환하는 함수

순수 함수를 통해 부수효과를 최대한 억제하여 안정성을 높임

### 🔍 Array.prototype.sort

기본적으로 오름차순 (문자열 변환 후 유니코드 코드 포인터 순서 기준)

비교 함수의 반환값이 0보다 작으면 비교 함수의 첫 번째 인수를 우선, 0이면 엔진에 따라 다르게 처리

### Array.prototype.forEach

for문을 대체할 수 있는 고차 함수

```jsx
arr.forEach((item, index, arr) => {}, thisBinding)
```

희소 배열의 경우 존재하지 않는 요소는 제외 (map, filter, reduce도 마찬가지)

### Array.prototype.map

콜백 함수의 반환값들로 구성된 새로운 배열 반환

map 메서드가 생성하는 새로운 배열의 length는 map 메서드를 호출한 배열의 length와 정확히 일치

### Array.prototype.filter

콜백 함수의 반환값이 true인 요소로 구성된 새로운 배열 반환

### Array.prototype.reduce

콜백 함수를 호출하여 하나의 결과값을 만들어 반환

### Array.prototype.some

콜백 함수의 실행값이 하나라도 true면 true

### Array.prototype.every

콜백 함수의 실행값이 하나라도 false면 false

### Array.prototype.find/findIndex

콜백 함수의 반환값이 true인 첫 요소를 반환

### Array.prototype.flatMap

map 이후 생성된 배열을 1단계 평탄화
