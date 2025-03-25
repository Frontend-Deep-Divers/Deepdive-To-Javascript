# `this`

객체 리터럴 방식으로 생성한 객체는 자신을 재귀적으로 참조할 수 있음

```jsx
const circle = {
	radius: 5,
	getDiameter() {
		return 2 * circle.radius;
	}
}
```

바람직하지는 않음

생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 전이므로 생성할 인스턴스를 가리키는 식별자를 알 수 없음 → 이를 위해 자바스크립트는 `this`  식별자를 제공함

`this` 는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가리키는 자기 참조 변수이다

`this` 바인딩은 함수 호출 방식에 의해 동적으로 결정됨

`this` 는 어디에서든 참조가 가능하지만 자기 참조 변수 목적이므로 자기 참조 변수로 쓰일 때 의미가 있다 ( 일반 함수 호출 시에는 의미가 없음 )

# 함수 호출 방식과 `this` 바인딩

### 일반 함수 호출

전역 객체가 바인딩됨

strict mode 에서는 `undefined` 가 바인딩됨

중첩 함수든 콜백 함수든 일반 함수로 호출되면 전부 전역 객체(`undefined` ) 로 바인딩되는데 이는 헬퍼 함수로 동작하기 어렵게 만듦 → `this` 바인딩을 일치시키기 위한 방법

- `this` 를 변수에 할당한 후 중첩(콜백) 함수 내에서 사용
- `apply` `call` `bind` 사용
- 화살표 함수 사용

### 메서드 호출

메서드를 호출한 객체가 바인딩됨 ( 메서드를 소유한 객체가 아님 )

### 생성자 함수 호출

생성자 함수가 생성할 인스턴스가 바인딩됨

### `Function.prototype.apply/call/bind` 메서드에 의한 간접 호출

```jsx
// apply와 call은 함수를 호출함
Function.prototype.apply(thisArg[, argsArray])

Function.prototype.call(thisArg[, arg1[, arg2[, ...]]])

// 예시
function foo() {
	// arguments는 배열이 아니기 때문에 간접 호출
	const arr = Array.prototype.slice.call(arguments);
}
```

```jsx
// bind는 this 바인딩이 교체된 함수를 생성해 반환
Function.prototype.bind(thisArg);

// 예시
const obj = {
	foo(callback) {
		setTimeout(callback.bind(this), 1000);
	}
}
```
