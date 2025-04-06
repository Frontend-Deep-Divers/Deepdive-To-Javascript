# 생성자 함수

```jsx
const numObj = new Number();
// Number 래퍼 객체
// Number { [[PrimitiveValue]]: 0 }
// ES6 부터는 [[PrimitiveValue]]가 [[NumberData]] 를 가리킴

const numObj = new Number('sf');
// number로 형변환
// 변환이 안되면 NaN

const num = Number(0);
// 0

```

# 프로퍼티

### Number.EPSILON

1과 1보다 큰 숫자 중 가장 작은 숫자와의 차이

```jsx
function isEqaul(a, b) {
	return Math.abs(a - b) < Number.EPSILON;
}
```

### Number.MAX_VALUE/MIN_VALUE

자바스크립트에서 표현할 수 있는 가장 큰/작은 양수 값

### Number.MAX_SAFE_INTEGER/MIN_SAFE_INTEGER

자바스크립트에서 안전하게 표현할 수 있는 가장 큰/작은 정수 값

### Number.POSITIVE_INFINITY/NEGATIVE_INFINITY/NaN

# 메서드

### Number.isFinite

Infinity, -Infinity를 검사

window.isFinite는 전달받은 인수를 암묵적으로 숫자로 변환하지만, Number.isFinite는 그렇지 않음

### Number.isInteger/isNaN

암묵적으로 숫자로 변환하지 않음

### Number.isSafeInteger

-(2^53 - 1) ~ 2^53 - 1 사이의 값인지 검사

### Number.prototype.toExponential

숫자를 지수 표기법으로 변환하여 문자열로 반환

### Number.prototype.toFixed

숫자를 반올림하여 문자열로 반환, 자릿수를 나타내는 0~20 사이의 정수값을 인수로 전달

### Number.prototype.toPrecision

인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 전달

### Number.prototype.toString

진법을 인수로 전달할 수 있음
