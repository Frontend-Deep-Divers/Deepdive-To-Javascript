# Object 생성자 함수

`new` 연산자와 함께 `Object`  생성자 함수를 호출하면 빈 객체를 생성하여 반환

생성자 함수: `new` 연산자와 함께 호출하여 인스턴스를 생성하는 함수

# 생성자 함수

### 객체 리터럴에 의한 객체 생성 방식의 문제점

객체 리터럴에 의한 객체 생성 방식은 단 하나의 객체만 생성함 → 동일한 프로퍼티를 갖는 객체를 여러 개 생성하는 경우 비효율적

### 생성자 함수에 의한 객체 생성 방식의 장점

인스턴스를 생성하기 위한 템플릿(클래스)처럼 활용

```jsx
function Circle(radius) {
	this.radius = radius;
	this.getDiameter = function() {
		return 2 * this.radius;
	};
}

const circle = new Circle(8);
```

`new` 연산자를 함께 호출하지 않으면 일반 함수로 동작

### 생성자 함수의 인스턴스 생성 과정

생성자 함수의 역할은 인스턴스를 생성하고 생성될 인스턴스를 초기화하는 것

1. 인스턴스를 생성하고 `this` 바인딩
    1. 암묵적으로 빈 객체가 생성됨
    2. 빈 객체에 `this` 가 바인딩됨
    3. 코드 평가 단계에서 일어남
2. 인스턴스 초기화
    1. 생성자 함수가 실행됨
3. 인스턴스 반환
    1. 완성된 인스턴스가 바인딩되어 있는 `this` 가 암묵적으로 반환됨
    2. 명시적으로 다른 객체를 반환하면 명시한 객체가 반환됨
    3. 명시적으로 다른 원시값을 반환하면 무시되고 `this` 가 반환됨

# 내부 메서드 [[Call]]과 [[Construct]]

함수 선언문 또는 함수 표현식으로 정의한 함수는 일반적인 함수 + 생성자 함수로 호출할 수 있음

함수 객체는 일반 객체의 내부 슬롯 뿐만 아니라 함수로서 동작하기 위한 내부 슬롯과 내부 메서드도 가지고 있음

- [[Call]] : 일반 함수로 호출되었을 때 실행되는 함수 ( callable )
- [[Construct]] : 생성자 함수로 호출되었을 때 실행되는 함수( constructor )

모든 함수가 constructor인 것은 아님

- constructor: 함수 선언문, 함수 표현식, 클래스
- non-constructor: 메서드(ES6 메서드 축약 표현), 화살표 함수

### `new.target`

생성자 함수가 `new` 연산자 없이 호출되는 것을 방지하기 위해 ES6에서는 `new.target` 을 지원함 ( IE X )

`new` 연산자와 함께 생성자 함수로서 호출되면 `new.tareget` 은 함수 자신을 가리키고, 일반 함수로서 호출되면 `undefined` 

```jsx
function Circle(radius) {
	if(!new.target) { 
		return new Circle(radius);	
	} 
	...
}

// 스코프 세이프 생성자 패턴
function Circle(radius) {
	if(!(this instanceof Circle)) {
		return new Circle(radius);
	}
}
```
