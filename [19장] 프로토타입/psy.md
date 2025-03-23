# 객체지향 프로그래밍

객체의 집합으로 프로그램을 표현하는 프로그래밍 패러다임

객체는 상태와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조

객체는 다른 객체와 관계성을 가질 수 있고 다른 객체의 상태나 동작을 상속받아 사용할 수 있음

# 상속과 프로토타입

자바스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거함

```jsx
function Circle(radius) {
	this.radius = radius;
}

Circle.prototype.getArea = function () {
	return Math.PI * this.radius ** 2;
}
```

<img width="862" alt="스크린샷 2023-02-03 오후 3 27 17" src="https://github.com/user-attachments/assets/fc62fcad-186c-4613-b289-b0c9dab7a852" />


`Circle` 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 즉 `Circle.prototype` 의 모든 프로퍼티와 메서드를 상속받는다

# 프로토타입 객체

모든 객체는 [[Prototype]]이라는 내부 슬롯을 가지며, 이 값은 프로토타입을 참조한다

### `__proto__` 접근자 프로퍼티

`__proto__` 접근자 프로퍼티를 통해 프로토타입 객체에 접근할 수 있다

`__proto__` 접근자 프로퍼티는 `Object.prototype` 의 프로퍼티로, 상속을 통해 해당 프로퍼티에 접근할 수 있다

접근자 프로퍼티로 만들어진 이유는 상호 참조에 의한 프로토타입 체인이 생성되는 것을 방지하기 위함 → 프로토타입 체인은 단방향 링크드 리스트로 구현되어야 함

직접 사용하지 말고 `getPrototypeOf()` `setPrototypeOf()` 를 사용할 것을 권장함

### 함수 객체의 `prototype` 프로퍼티

생성자 함수가 생성할 인스턴스의 프로토타입을 가리킴 → non-constructor에는 존재하지 않음

### 프로토타입의 `constructor` 프로퍼티

모든 프로토타입은 `constructor` 프로퍼티를 가짐 → 자신을 프로토타입으로 참조하고 있는 생성자 함수를 가리킴

생성자 함수 객체가 생성될 때 연결됨

# 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

new 연산자와 함께 생성자 함수를 호출하여 인스턴스를 생성하지 않는 객체 생성 방식도 존재함

프로토타입은 존재하나 `constructor` 프로퍼티가 가리키는 생성자 함수와 객체를 생성한 생성자 함수가 같다고 단정할 수 없음

객체 리터럴에 의해 생성된 객체는 `Object` 생성자 함수가 생성한 객체는 아니지만 `Object.prototype` 을 프로토타입으로 갖게됨

함수 리터럴에 의해 생성된 함수 객체는 `Function` 생성자 함수가 생성한 객체는 아니지만 `Function.prototype` 을 프로토타입으로 가짐

- 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 항상 쌍으로 존재

# 프로토타입의 생성 시점

프로토타입은 생성자 함수가 생성되는 시점에 같이 생성됨

### 사용자 정의 생성자 함수의 프로토타입 생성 시점

constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 생성

### 빌트인 생성자 함수와 프로토타입 생성 시점

모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성되고, 프로토타입도 동시에 생성됨

# 객체 생성 방식과 프로토타입의 결정

객체는 추상 연산 `OrdinaryObjectCreate` 에 의해 생성됨

`OrdinaryObjectCreate` 는 생성할 객체의 프로토타입을 인수로 전달받아 [[Prototype]] 내부 슬롯에 할당함

### 객체 리터럴에 의해 생성된 객체

`OrdinaryObjectCreate` 에 `Object.prototype` 이 전달됨

### `Object` 생성자 함수에 의해 생성된 객체

`OrdinaryObjectCreate` 에 `Object.prototype` 이 전달됨

### 생성자 함수에 의해 생성된 객체

`OrdinaryObjectCreate` 에 생성자 함수의 `prototype` 에 바인딩되어 있는 객체가 전달됨

# 프로토타입 체인

자바스크립트 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없으면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다 → 프로토타입 체인

모든 프로토타입의 최상위 프로토타입은 `Object.prototype` 이다

↔ 프로퍼티가 아닌 식별자는 스코프 체인에서 검색, 즉

- 프로토타입 체인: 상속과 프로퍼티 검색을 위한 메커니즘
- 스코프 체인: 식별자 검색을 위한 메커니즘

# 오버라이딩과 프로퍼티 섀도잉

같은 이름의 인스턴스 프로퍼티는 프로토타입 프로퍼티을 가리고(프로퍼티 섀도잉) 호출된다(오버라이딩)

하위 객체를 통해 프로토타입의 프로퍼티를 변경/삭제하는 것은 불가능 → 직접 프로토타입을 통해 접근해야함

# 프로토타입 교체

프로토타입은 임의의 다른 객체로 변경될 수 있어서 객체 간의 상속 관계를 동적으로 변경할 수 있음

### 생성자 함수에 의한 교체

```jsx
function Circle(r) { ... }

Circle.prototype = {
	sayHello() { ... }
}

const c = new Circle();
c.sayHello();
```

`Cricle` 의 프로토타입의 `constructor` 에 접근할 때, 교체한 프로토타입에 `constructor` 프로퍼티가 없다면 프로토타입 체인을 통해 `Object` 생성자 함수가 참조된다 → 명시적으로 `constructor` 프로퍼티를 추가해줘야함

미래에 생성할 모든 인스턴스의 프로토타입 교체

### 인스턴스에 의한 교체

이미 생성된 객체의 프로토타입 교체

인스턴스를 생성했던 생성자 함수가 교체된 프로토타입을 가리키지 않게됨

프로토타입 교체를 통해 직접 객체 간의 상속 관계를 변경하는 것은 생성자 함수와 프로토타입 간의 연결을 생각해야하므로 번거롭다 → 직접 교체보다는 직접 상속 또는 ES6의 클래스를 사용하는 것이 좋음

# `instanceof` 연산자

```jsx
<객체> instanceof <생성자 함수>
```

우변의 생성자 함수의 `prototype` 에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 `true` 

# 직접 상속

### `Object.create`

명시적으로 프로토타입을 지정하여 객체를 생성

```jsx
let obj = Object.create(null);
obj.toString(); // obj는 Object.prototype을 상속받지 못해서 에러 발생

obj = Object.create(Object.prototpye);
obj.toString(); // Object.prototype을 상속받았기 때문에 가능

const myProto = { x: 10 };
// obj -> myProto -> Object.prototype
obj = Object.create(myProto);
obj.x; // 10
```

`Object.prototype` 의 빌트인 메서드를 객체가 직접 호출하는 것은 해당 객체가 프로토타입 체인의 종점에 위치해 있을 수도 있기 때문에 권장되지 않고, `call` 과 같은 메서드를 통해 간접적으로 호출하는 것이 좋음

### ES6 객체 리터럴 내부에서 `__proto__` 에 의한 직접 상속

```jsx
const myProto = { x: 10 };

const obj = {
	y: 20,
	
	__proto__: myProto,
};
// obj -> myProto -> Object.prototype
```

# 정적 프로퍼티/메서드

생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드

생성자 함수 객체가 소유한 프로퍼티/메서드 → 인스턴스로 참조/호출할 수는 없음 ( 프로토타입 체인에 속하지 않음 )

프로토타입 프로퍼티 표기법

- Object.prototype.is()
- Object#is()

# 프로퍼티 존재 확인

### `in` 연산자

객체 내에 프로퍼티가 존재하는 지 여부를 확인

```jsx
key: string in object
```

프로토타입까지 모두 확인하므로 주의

ES6, `Reflect.has` 메서드도 동일하게 동작

### `Object.prototype.hasOwnProperty`

프로퍼티가 해당 객체의 고유 프로퍼티인 경우에만 `true` 를 반환

# 프로퍼티 열거

### `for ... in`

객체의 모든 프로퍼티를 순회하며 열거 enumeration ( 프로퍼티 키가 할당됨 )

프로퍼티 애트리뷰트 [[Enumerable]] 값에 따라 열거 대상이 결정됨

프로토타입 체인상의 모든 프로퍼티를 열거함

프로퍼티 키가 Symbol인 프로퍼티는 열거하지 않음

배열에는 권장되지 않음 → `for ... of` `Array.prototype.forEach` 권장

### `Object.keys/values/entries`

객체의 고유 프로퍼티만 열거
