# 문법적 설탕

자바스크립트는 클래스가 필요 없는 객체지향 언어로 생성자 함수와 프로토타입을 통해 상속을 구현할 수 있음

ES6에서 클래스가 도입되었지만, 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있도록 하는 문법적 설탕으로 볼 수 있음

하지만 몇 가지 차이가 있음

1. 클래스를 new 연산자 없이 호출하면 에러 발생
2. extends, super 키워드 지원
3. 호이스팅이 발생하지 않는 것처럼 동작
4. 클래스 내의 모든 코드는 암묵적으로 strict mode가 적용됨
5. 클래스의 constructor, 프로토타입 메서드, 정적 메서드는 모두 [[Enumerable]] 이 `false`

따라서 새로운 객체 생성 메커니즘으로 볼 수 있음

# 클래스 정의

```jsx
class Person {}

// 클래스를 표현식으로 정의할 수 있음 -> 클래스도 일급 객체
const Person = class {}
const Person = class MyClass {}
```

클래스에는 메서드만 정의할 수 있음

```jsx
class Person {
	constructor() {}
	
	sayHi() {}
	
	static sayHello() {}
}
```

# 클래스 호이스팅

클래스는 함수로 평가됨

클래스 선언문으로 정의한 클래스는 코드 평가 단계에서 평가되어 함수 객체를 생성함(`constructor`) → 프로토타입도 생성됨

하지만 클래스 정의 이전에 참조할 수는 없음  → TDZ 형성

# 인스턴스 생성

```jsx
const me = new Person(); // 반드시 new 연산자와 함께 호출
```

```jsx
const Person = class MyClass {}

// ReferenceError
// 클래스 표현식에서 사용한 클래스 이름은 외부 코드에서 접근 불가
const me = new MyClass();
```

# 메서드

### `constructor`

인스턴스를 생성하고 초기화하기 위한 특수한 메서드

클래스가 정의되면 `constructor` 의 기술된 동작을 하는 함수 객체가 생성됨

생성자 함수와 차이가 있음

1. 클래스 내에 최대 한 개만 존재할 수 있음
2. 생략하면 빈 `constructor` 가 암묵적으로 정의됨

### 프로토타입 메서드

클래스 몸체에서 정의한 메서드는 인스턴스 프로토타입에 존재하는 프로토타입 메서드가 됨

### 정적 메서드

`static` 키워드를 붙여서 정적 메서드를 생성함

### 클래스에서 정의한 메서드의 특징

1. `function` 키워드를 생략한 메서드 축약 표현을 사용함
2. 클래스에 메서드를 정의할 때는 콤마가 필요 없다
3. 암묵적 strict mode 적용
4. 메서드를 열거할 수 없음
5. non-constructor

# 클래스의 인스턴스 생성 과정

1. 인스턴스 생성과 `this` 바인딩
2. 인스턴스 초기화
3. 인스턴스 반환

# 프로퍼티

### 인스턴스 프로퍼티

```jsx
class Person {
	constructor(name) {
		this.name = name; // constructor 내부에서 정의
	}
}
```

접근 제한자를 지원하지 않음

### 접근자 프로퍼티

```jsx
class Person {
	...
	
	get fullName() {
		return `${this.firstName} ${this.lastName}`;
	}
}
```

접근자 프로퍼티는 프로토타입 메서드가 됨

### 클래스 필드 정의 제안

인스턴스 프로퍼티를 마치 클래스 기반 객체지향 언어의 클래스 필드처럼 정의할 수 있는 새로운 표준 사양이 제안되어 있음 ( 2022.06 정식 표준으로 승급 )

```jsx
class Person {
	name = 'Lee';
}
```

클래스 필드를 통해 메서드를 정의할 수 있음 → 인스턴스 메서드 정의

### private 필드 정의 제안 (ES2019 표준)

```jsx
class Person {
	#name = 'asdf';
}

const me = new Person();

me.#name // SyntaxError
```

private 필드는 반드시 클래스 몸체에 클래스 필드로 정의해야 함

### static 필드 정의 제안

```jsx
class Person {
	static num = 5;
}

Person.num // 5
```

# 상속에 의한 클래스 확장

### 클래스 상속과 생성자 함수 상속

프로토타입 기반 상속과 다른 개념

기존 클래스를 상속받아 새로운 클래스를 확장하여 정의하는 것

### `extends`

수퍼클래스와 서브클래스 간의 상속 관계 설정

클래스 간의 프로토타입 체인 생성 → 정적 메서드도 상속 가능

### 동적 상속

`extends` 키워드 다음에는 클래스뿐만 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식 사용 가능

### 서브클래스의 constructor

서브클래스에서 constructor를 생략하면 다음과 같은 constructor가 암묵적으로 정의됨

```jsx
constructor(...args) { super(...args); }
```

`super` 는 수퍼클래스의 constructor를 호출하여 인스턴스를 생성함

### `super`

호출하면 수퍼클래스의 constructor를 호출

참조하면 수퍼클래스의 메서드를 호출할 수 있음

주의사항

1. 서브클래스에서 constructor를 정의하는 경우 반드시 super를 호출해야 함
2. super를 호출하기 전에는 this를 참조할 수 없음
3. super는 반드시 constructor에서만 호출

super는 참조하고 있는 메서드가 바인딩되어 있는 객체의 프로토타입 [[HomeObject]] 을 가리킴

[[HomeObject]]는 메서드 축약 표현으로 정의된 함수만이 가짐

즉, [[HomeObject]]를 가지는 함수만이 super 참조가 가능

### 상속 클래스의 인스턴스 생성 과정

1. 서브클래스의 super 호출
    1. 수퍼클래스와 서브클래스는 [[ConstructorKind]] 값으로 구분됨 → 동작도 구분됨
    2. 서브클래스는 직접 인스턴스를 생성하지 않고 수퍼클래스에게 생성을 위임함
2. 수퍼클래스의 인스턴스 생성과 this 바인딩
    1. 빈 객체 생성 → this 바인딩
    2. 수퍼클래스의 `constructor` 에서 `new.target` 은 서브클래스를 가리킴
    3. 즉, 생성될 인스턴스의 프로토타입은 서브클래스의 `prototype` 프로퍼티가 가리키는 객체
3. 수퍼클래스의 인스턴스 초기화
4. 서브클래스 `constructor` 로의 복귀와 this 바인딩
    1. `super` 가 반환한 인스턴스가 this에 바인딩됨 → `super` 호출 전에 this를 참조할 수 없는 이유
5. 서브클래스 인스턴스 초기화
6. 인스턴스 반환

### 표준 빌트인 생성자 함수 확장

```jsx
class MyArray extends Array {
	unique() { return ... }
}
```

`Array.prototype` 의 메서드 중에서 새로운 배열을 반환하는 메서드가 `MyArray` 클래스의 인스턴스를 반환함

```jsx
class MyArray extends Array {
	// 정적 접근자 프로퍼티를 추가하면 Array 인스턴스를 반환함
	static get [Symbol.species]() { return Array; }
}
```
