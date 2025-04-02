# 함수의 구분

ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않음 → 모두 callable, constructor

객체에 바인딩된 함수 또는 콜백 함수가 prototype을 가지기 때문에 성능 상에 문제가 있음

ES6에서는 함수를 목적에 따라 세 종류로 구분

|  | constructor | prototype | super | arguments |
| --- | --- | --- | --- | --- |
| 일반 함수 | O | O | X | O |
| 메서드 | X | X | O | O |
| 화살표 함수 | X | X | X | X |

# 메서드

메서드 축약 표현으로 정의된 함수

자신을 바인딩한 객체를 가리키는 내부 슬롯 [[HomeObject]]를 가짐 → super 참조에 활용됨

# 화살표 함수

함수 표현식으로만 정의 가능

```jsx
const add = (a, b) => a + b;
```

### 일반 함수와의 차이

1. non-constructor
2. 중복된 매개변수 이름을 선언할 수 없음 ( strict mode )
3. this, arguments, super, new.target 바인딩을 가지지 않음 → 스코프 체인 상에서 가장 가까운 상위 함수의 식별자를 참조
    1. `Function.prototype.call/apply/bind` 를 사용해도 this 바인딩을 교체할 수 없음
    2. 메서드로 정의하면 안됨

# Rest 파라미터

```jsx
function foo(...rest) {
	console.log(rest); // [1, 2, 3, 4, 5]
}

// 반드시 마지막 매개변수에 위치해야함
function bar(a, b, ...rest) {
	console.log(rest); // [3, 4, 5]
}
```

함수 객체의 `length` 프로퍼티에 영향을 주지 않음

배열이기 때문에 `arguements` 처럼 배열로 변환하는 과정이 필요하지 않음

# 매개변수 기본값

```jsx
function sum(x = 0, y = 0) {
	return x + y;
}
```
