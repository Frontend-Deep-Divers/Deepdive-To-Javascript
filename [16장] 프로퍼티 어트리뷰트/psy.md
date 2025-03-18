# 내부 슬롯과 내부 메서드

자바스크립트 엔진의 내부 구현을 설명하기 위해 ECMAScript 사양에서 사용하는 pseudo 프로퍼티와 메서드

이중 대괄호로 감싼 이름

개발자가 직접 접근할 수는 없음 - `[[Prototype]]` 제외

# 프로퍼티 어트리뷰트와 프로퍼티 디스크립터

자바스크립트 엔진은 프로퍼티를 생성할 때 프로퍼티의 상태를 나타내는 프로퍼티 어트리뷰트를 기본값으로 자동 정의함 → `[[Value]] [[Writable]] [[Enumerable]] [[Configurable]]` 

```jsx
const person = {
	name: 'Park',
};

console.log(Object.getOwnPropertyDescriptor(person, 'name'));
// 디스크립터 객체
// { value: 'Park', writable: true, enumerable: true, configurable: true }
```

# 데이터 프로퍼티와 접근자 프로퍼티

프로퍼티는 두 가지로 구분됨

### 데이터 프로퍼티

키와 값으로 구성되는 일반적인 프로퍼티

| 프로퍼티 어트리뷰트 | 설명 |
| --- | --- |
| [[Value]] | 프로퍼티 키를 통해 프로퍼티 값에 접근하면 반환되는 값<br />프로퍼티 키를 통해 프로퍼티 값을 변경하면 재할당됨<br />변경할 때 프로퍼티가 없으면 동적 생성하고 생성된 프로퍼티의 [[Value]]에 값을 저장함 |
| [[Writable]] | 프로퍼티 값의 변경 가능 여부 |
| [[Enumerable]] | 프로퍼티의 열거 가능 여부<br />`for … in` 문이나 `Object.keys` 메서드 등으로 열거할 수 있는지 여부 |
| [[Configurable]] | 프로퍼티의 재정의 가능 여부<br />`false`라면 해당 프로퍼티의 삭제, 프로퍼티 어트리뷰트 값의 변경이 금지됨<br />[[Value]]의 변경과 [[Writable]]을 false로 변경하는 것은 허용 |

### 접근자 프로퍼티

자체적으로 값을 가지지 않고 다른 데이터 프로퍼티의 값을 읽거나 저장할 때 사용되는 접근자 함수로 구성된 프로퍼티

| 프로퍼티 어트리뷰트 | 설명 |
| --- | --- |
| [[Get]] | 데이터 프로퍼티의 값을 읽을 때 호출되는 접근자 함수 |
| [[Set]] | 데이터 프로퍼티의 값을 저장할 때 호출되는 접근자 함수 |
| [[Enumerable]] |  |
| [[Configurable]] |  |

# 프로퍼티 정의

새로운 프로퍼티를 추가하면서 프로퍼티 어트리뷰트를 명시적으로 정의하거나, 기존 프로퍼티의 프로퍼티 어트리뷰트를 재정의하는 것

```jsx
const person = {};

Object.defineProperty(person, 'firstName', {
	value: 'Park',
	writable: true,
	enumerable: true,
	configurable: true,
});

Object.defineProperty(person, 'fullName', {
	get() {
		return `${this.firstName} ${this.lastName}`;
	},
	
	set(name) {
		[this.firstName, this.lastName] = name.split(' ');
	},
	
	...
});
```

# 객체 변경 방지

### 객체 확장 금지 `Object.preventExtensions()`

프로퍼티 추가 금지

### 객체 밀봉 `Object.seal()`

프로퍼티 값 읽기/쓰기 만 가능

프로퍼티 추가/삭제, 프로퍼티 어트리뷰트 재정의 금지

### 객체 동결 `Object.freeze()`

프로퍼티 값 읽기만 가능

중첩 객체까지 영향을 주지는 않음 → 재귀적으로 직접 호출
