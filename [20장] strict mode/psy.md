잠재적인 오류를 발생시키기 어려운 개발 환경을 만들어야 함 → 자바스크립트 언어의 문법을 더 엄격히 적용하여 문제를 일으킬 수 있는 코드에 대해 명시적인 에러를 발생시키는 strict mode

린트 도구로 대체 가능

ES6 클래스와 모듈은 기본적으로 strict mode 적용

# strict mode 적용

```jsx
'use strict';

function foo() {
	x = 10; // Reference Error
}

foo();
```

```jsx
function foo() {
	'use strict';
	
	x = 10; // Reference Error
}

foo();
```

전역의 선두 또는 함수 몸체의 선두에 추가해야 함

전역에 적용하는 것은 권장하지 않음 → non-strict mode 라이브러리가 있을 수 있기 때문

함수 단위로 적용하는 것도 권장하지 않음

스크립트 단위로 즉시 실행 함수로 감싸서 적용

# strict mode가 발생시키는 에러

### 암묵적 전역

선언하지 않은 변수를 참조하면 Reference Error 발생

### 변수, 함수, 매개변수의 삭제

`delete` 연산자로 변수, 함수, 매개변수를 삭제하면 SyntaxError 발생

### 매개변수 이름의 중복

중복된 매개변수 이름을 사용하면 SyntaxError 발생

### with 문의 사용

```jsx
// 스코프에 객체를 추가하는 문법
with({x: 1}) {
	console.log(x);
}
```

strict mode 에서 사용하면 SyntaxError 발생

# strict mode 에 의한 변화

### 일반 함수의 `this`

일반 함수의 `this` 에 `undefined` 가 바인딩 됨

### `arguments` 객체

매개변수에 전달된 인수를 재할당해도 `arguments` 객체에 반영되지 않음
