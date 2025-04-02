# 25장 클래스

## 25.1 클래스는 프로토타입의 문법적 설탕인가?

ES6에서 도입된 클래스는 기존의 자바와 C#과 같은 클래스 기반 객체지향 프로그래밍에 익숙한 개발자를 위한 객체 생성 메커니즘이다.

자바스크립트에서는 클래스 또한 함수이며 기존 프로토타입 기반 패턴을 클래스 기반 패턴처럼 사용할 수 있게 해주는 문법적 설탕이라고 볼 수도 있다.

단, 클래스와 생성자 함수는 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다. 클래스는 생성자 함수보다 엄격하며 생성자 함수에서는 제공하지 않는 기능도 제공한다.

| 구분              | 클래스 (Class)                                                | 생성자 함수 (Constructor Function)    |
| ----------------- | ------------------------------------------------------------- | ------------------------------------- |
| 1. 호출 방식      | `new` 없이 호출하면 에러 발생                                 | `new` 없이도 일반 함수처럼 호출 가능  |
| 2. 상속           | `extends`, `super` 키워드 지원                                | `extends`, `super` 키워드 미지원      |
| 3. 호이스팅       | 호이스팅이 발생하지 않는 것처럼 동작                          | 함수 선언문으로 정의 시 호이스팅 발생 |
| 4. strict mode    | 암묵적으로 `strict mode` 적용됨 (해제 불가)                   | 기본적으로 적용되지 않음              |
| 5. 열거 가능 여부 | `constructor`, 프로토타입 메서드, 정적 메서드가 열거되지 않음 | 열거될 수 있음                        |

## 25.2 클래스 정의

```js
class Person {}
```

일반적이지는 않지만 표현식으로도 클래스를 정의할 수 있다. 익명으로도, 기명으로도 가능하다.

```js
// 익명 클래스 표현식
const Person = class {};

// 기명 클래스 표현식
const Person = class MyClass {};
```

클래스가 표현식으로 정의될 수 있다는 것은 클래스도 일급 객체라는 것을 의미한다.

클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다. 클래스 몸체에서 정의할 수 있는 메서드는 `constructor`(생성자), 프로토타입 메서드, 정적 메서드 세 가지가 있다.

```js
// 클래스 선언문
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name; // name 프로퍼티는 public하다.
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }

  // 정적 메서드
  static sayHello() {
    console.log("Hello!");
  }
}

// 인스턴스 생성
const me = new Person("Lee");

// 인스턴스의 프로퍼티 참조
console.log(me.name); // Lee
// 프로토타입 메서드 호출
me.sayHi(); // Hi! My name is Lee
// 정적 메서드 호출
Person.sayHello(); // Hello!
```

## 25.3 클래스 호이스팅

클래스는 함수로 평가된다.

때문에 클래스 선언문으로 선언한 클래스는 함수 선언문과 같이 소스코드 평가, 즉 런타임 이전에 먼저 평가되어 함수 객체를 생성한다. 이때 클래스가 평가되어 생성된 함수 객체는 생성자 함수로서 호출할 수 있는 함수, 즉 `constructor`다.

이떄 프로토타입도 같이 생성된다. 생성자 함수와 프로토타입은 단독으로 존재할 수 없고 언제나 쌍으로 존재하기 때문이다.

단, 클래스는 클래스 정의 이전에 참조하려고 하면 에러가 발생한다.

클래스 선언문도 호이스팅이 발생한다. 단 `let`과 `const` 키워드의 변수처럼 호이스팅된다. (TDZ에 빠질 수 있다.)

💡`var`, `let`, `const`, `function`, `function*`, `class` 키워드를 사용하여 선언된 모든 식별자는 호이스팅된다. 모든 선언문은 런타임 이전에 먼저 실행되기 때문이다.

## 25.4 인스턴스 생성

반드시 `new` 연산자와 함께 생성한다.

클래스 표현식으로 정의된 클래스는 클래스를 가리키는 식별자를 사용해야 한다. 클래스 이름을 사용해 인스턴스를 생성할 수 없다. (함수 표현식과 동일하게 동작한다.)

## 25.5 메서드

클래스 몸체에는 0개 이상의 메서드만 정의할 수 있다.

> ES2020에 따르면 인스턴스 프로퍼티는 반드시 `constructor` 내부에서 정의해야 했으나, 클래스 몸체에 메서드뿐만이 아니라 프로퍼티를 직접 정의할 수 있는 새로운 표준 사양이 제안되어 있다.

### `constructor`

`constructor`는 인스턴스를 생성하고 초기화하기 위한 특수한 메서드다. `constructor`는 이름을 변경할 수 없다.

`constructor`는 인스턴스 내부에 보이지 않는데, 이는 `constructor`는 메서드로 해석되지 않고 클래스가 평가되어 생성한 함수 객체 코드의 일부가 되기 때문이다. 즉, 클래스 정의가 평가되면 `constructor`의 기술된 동작을 하는 함수 객체가 생성된다는 것이다.

> ⚠️클래스의 `constructor` 메서드와 프로토타입의 `constructor` 프로퍼티는 직접적인 관련이 없다. 프로토타입의 `constructor` 프로퍼티는 모든 프로토타입이 가지고 있는 프로퍼티로, 생성자 함수를 가리킨다.

`constructor`는 생성자 함수와 유사하지만 몇 가지 차이점이 있다.

1. `constructor`는 클래스 내 한 개 이상 존재할 수 없다.
2. `constructor`를 생략하면 빈 `constructor`가 암묵적으로 정의된다.

   인스턴스를 초기화하려면 `constructor`를 생략하면 안 된다.

3. `constructor`는 별도의 반환문을 갖지 않아야 한다.

   생성자 함수와 동일하게 암묵적으로 `this`, 즉 인스턴스를 반환하기 때문이다. 다른 객체를 명시적으로 반환하면 `return` 문에 명시한 객체가 반환된다. 하지만 원시값을 반환하면 원시값 반환은 무시되고 `this`가 반환된다.

### 프로토타입 메서드

클래스 몸체에 정의한 메서드는 기본적으로 프로토타입 메서드가 된다.

```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 프로토타입 메서드
  sayHi() {
    console.log(`Hi! My name is ${this.name}`);
  }
}

const me = new Person("Lee");
me.sayHi(); // Hi! My name is Lee
```

### 정적 메서드

`static` 키워드를 붙이면 정적 메서드가 된다.

```js
class Person {
  // 생성자
  constructor(name) {
    // 인스턴스 생성 및 초기화
    this.name = name;
  }

  // 정적 메서드
  static sayHi() {
    console.log("Hi!");
  }
}
```

💡정적 메서드는 인스턴스로 호출할 수 없다.

### 정적 메서드와 프로토타입 메서드의 차이

1. 자신이 속해 있는 프로토타입 체인이 다르다.
2. 정적 메서드는 클래스로 호출하지만 프로토타입 메서드는 인스턴스로 호출한다.
3. 정적 메서드는 인스턴스 프로퍼티를 참조할 수 없지만 프로토타입 메서드는 가능하다.

### 클래스에서 정의한 메서드의 특징

1. `function` 키워드를 생략한 메서드 축약 표현을 사용한다.
2. 객체 리터럴과는 다르게 클래스에 메서드를 정의할 때는 콤마가 필요 없다.
3. 암묵적으로 strict mode로 실행된다.
4. `for...in` 문이나 `Object.keys` 메서드 등으로 열거할 수 없다. (`[[Enumerable]]` 값이 `false`다.)
5. 내부 메서드 `[[Constructor]]`를 갖지 않는 non-constructor다. 따라서 `new` 연산자와 함께 호출할 수 없다.

## 25.6 클래스의 인스턴스 생성 과정

`new` 연산자와 함께 클래스를 호출하면 생성자 함수와 같이 클래스의 내부 메서드 `[[Construct]]`가 호출된다.

1. 인스턴스 생성과 `this` 바인딩

`constructor` 내부 코드가 실행되어 암묵적으로 (추후 인스턴스가 될)빈 객체를 생성한다. 이때 인스턴스의 프로토타입으로 클래스의 `prototype` 프로퍼티가 가리키는 객체가 설정된다. 그리고 빈 객체, 즉 인스턴스가 `this`에 바인딩된다. 이제 `constructor` 내부의 `this`는 클래스가 생성한 인스턴스를 가리킨다.

2. 인스턴스 초기화

`constructor` 내부 코드가 실행되어 `this`에 바인딩되어 있는 인스턴스를 초기화한다. 즉, `this`에 바인딩된 인스턴스에 프로퍼티를 추가하고 `constructor`가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티 값을 초기화한다.

3. 인스턴스 반환

인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.

## 25.7 프로퍼티

### 인스턴스 프로퍼티

`constructor` 내부에 정의할 수 있다.

```js
constructor(name) {
  this.name = name; // 인스턴스 프로퍼티
}
```

생성된 빈 객체, 즉 인스턴스의 프로퍼티로 추가되어 인스턴스가 초기화된다.

자바스크립트의 클래스는 접근 제한자를 지원하지 않아 언제나 인스턴스 프로퍼티는 public하다.

> 💡 ES2022에서 `#`를 통해 private 필드/메서드를 정의할 수 있게 되었다.

### 접근자 프로퍼티

데이터 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티다.

생성자 함수와 비슷하게 사용한다.

```js
const person = {
  // 데이터 프로퍼티
  firstName: "Ungmo",
  lastName: "Lee",

  // fullName은 접근자 함수로 구성된 접근자 프로퍼티다.
  // getter 함수
  get fullName() {
    return `${this.firstName} ${this.lastName}`;
  },
  // setter 함수
  set fullName(name) {
    // 배열 디스트럭처링 할당: "36.1. 배열 디스트럭처링 할당" 참고
    [this.firstName, this.lastName] = name.split(" ");
  },
};

// 데이터 프로퍼티를 통한 프로퍼티 값의 참조.
console.log(`${person.firstName} ${person.lastName}`); // Ungmo Lee

// 접근자 프로퍼티를 통한 프로퍼티 값의 저장
// 접근자 프로퍼티 fullName에 값을 저장하면 setter 함수가 호출된다.
person.fullName = "Heegun Lee";
console.log(person); // {firstName: "Heegun", lastName: "Lee"}

// 접근자 프로퍼티를 통한 프로퍼티 값의 참조
// 접근자 프로퍼티 fullName에 접근하면 getter 함수가 호출된다.
console.log(person.fullName); // Heegun Lee

// fullName은 접근자 프로퍼티다.
// 접근자 프로퍼티는 get, set, enumerable, configurable 프로퍼티 어트리뷰트를 갖는다.
console.log(Object.getOwnPropertyDescriptor(person, "fullName"));
// {get: ƒ, set: ƒ, enumerable: true, configurable: true}
```

접근자 프로퍼티 역시 메서드처럼 프로토타입의 프로퍼티가 된다.

### 클래스 필드 정의 제안

클래스 필드는 클래스 기반 객체지향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가리키는 용어다.

본래 자바스크립트의 클래스 몸체에는 메서드만 선언할 수 있었다. 클래스 몸체에 자바와 같이 필드를 선언하면 문법 에러가 발생했었다.

하지만 Chrome 72 이상 혹은 Node.js 12버전 이상에서 실행하면 문법 에러가 발생하지 않으며 자바와 같이 사용 가능하다.

ES2020부터는 인스턴스 프로퍼티를 마치 클래스 기반 객체지향 언어처럼 정의할 수 있게 되었다.

```js
class Person {
  name = "Jeon";
}
```

`this`에 클래스 필드를 바인딩할 수는 없다. `this`는 클래스의 `constructor`와 메서드 내에서만 사용할 수 있다.

클래스 필드를 참조할 땐 `this` 키워드를 반드시 사용해야 한다.

초기값을 할당하지 않으면 `undefined`를 갖는다.

만약 인스턴스 생성 시 클래스 필드를 초기화할 필요가 있다면 `constructor` 밖에서 굳이 정의할 필요는 없다. 클래스 필드는 `constructor` 내부에서 참조하여 초기값을 할당해야 하는데, 이때 프로퍼티가 없다면 인스턴스에 자동으로 추가하기 때문이다.

자바스크립트의 함수는 일급 객체이므로 클래스 필드에 함수를 할당할 수 있고, 따라서 클래스 필드를 통해 메서드를 정의할 수 있다.

```js
class Person {
  // 클래스 필드에 문자열을 할당
  name = "Lee";

  // 클래스 필드에 함수를 할당
  getName = function () {
    return this.name;
  };
  // 화살표 함수로 정의할 수도 있다.
  // getName = () => this.name;
}

const me = new Person();
console.log(me); // Person {name: "Lee", getName: ƒ}
console.log(me.getName()); // Lee
```

💡클래스 필드에 함수를 정의하면 프로토타입 메서드가 아닌 인스턴스 메서드가 된다. 모든 클래스 필드는 인스턴스 프로퍼티가 되기 때문이다. 따라서 클래스 필드에 함수를 할당하는 것은 권장하지 않는다. (메서드 축약 표현을 사용해서 프로토타입 메서드를 선언할 수 있다.)

클래스 필드에 화살표 함수를 할당해 화살표 함수 내부의 `this`가 인스턴스를 가리키게 하는 경우도 있다.

```html
<!DOCTYPE html>
<html>
  <body>
    <button class="btn">0</button>
    <script>
      class App {
        constructor() {
          this.$button = document.querySelector(".btn");
          this.count = 0;

          // increase 메서드를 이벤트 핸들러로 등록
          // 이벤트 핸들러 increase 내부의 this는 DOM 요소(this.$button)를 가리킨다.
          // 하지만 increase는 화살표 함수로 정의되어 있으므로
          // increase 내부의 this는 인스턴스를 가리킨다.
          this.$button.onclick = this.increase;

          // 만약 increase가 화살표 함수가 아니라면 bind 메서드를 사용해야 한다.
          // $button.onclick = this.increase.bind(this);
        }

        // 인스턴스 메서드
        // 화살표 함수 내부의 this는 언제나 상위 컨텍스트의 this를 가리킨다.
        increase = () => (this.$button.textContent = ++this.count);
      }
      new App();
    </script>
  </body>
</html>
```

하지만, 인스턴스가 여러 개 생성되면 이 방법도 메모리 손해를 감수해야 한다.

인스턴스 프로퍼티 정의의 두 가지 방식의 차이점을 잘 이해하면 용도에 맞게 사용할 수 있다.

### `private` 필드 정의 제안

ES2020부터 `#`를 붙여 private한 필드/메서드를 생성할 수 있게 되었다.

```js
class Person {
  // private 필드 정의
  #name = "";

  constructor(name) {
    // private 필드 참조
    this.#name = name;
  }
}

const me = new Person("Lee");

// private 필드 #name은 클래스 외부에서 참조할 수 없다.
console.log(me.#name);
// SyntaxError: Private field '#name' must be declared in an enclosing class
```

클래스 외부에서 접근자 프로퍼티를 통해 `private` 필드에 간접적으로 접근할 수는 있다.

`private` 필드는 반드시 클래스 몸체에 정의해야 한다. `constructor` 내부에 직접 정의하면 에러가 발생한다.

### `static` 필드 정의 제안

`static` 키워드를 사용해서 정적 메서드를 정의할 수 있었다. 하지만 `static` 키워드를 사용한 정적 필드 선언은 ES2020에서 표준 사양으로 추가되었다.

```js
class MyMath {
  static PI = 22 / 7;
  static #num = 10;
  static increment() {
    return ++MyMath.#num;
  }
}
```

## 25.8 상속에 의한 클래스 확장

### 클래스 상속과 생성자 함수 상속

클래스 기반 객체지향 언어의 `extends`를 사용한 클래스 상속과 비슷하다.

클래스가 존재하기 전 생성자 함수를 확장하기 위해서는 다음과 같이 의사 클래스 상속(pesudo classical inheritance) 패턴을 사용했어야 했다.

```js
// 의사 클래스 상속(pseudo classical inheritance) 패턴
var Animal = (function () {
  function Animal(age, weight) {
    this.age = age;
    this.weight = weight;
  }

  Animal.prototype.eat = function () {
    return "eat";
  };

  Animal.prototype.move = function () {
    return "move";
  };

  return Animal;
})();

// Animal 생성자 함수를 상속하여 확장한 Bird 생성자 함수
var Bird = (function () {
  function Bird() {
    // Animal 생성자 함수에게 this와 인수를 전달하면서 호출
    Animal.apply(this, arguments);
  }

  // Bird.prototype을 Animal.prototype을 프로토타입으로 갖는 객체로 교체
  Bird.prototype = Object.create(Animal.prototype);
  // Bird.prototype.constructor을 Animal에서 Bird로 교체
  Bird.prototype.constructor = Bird;

  Bird.prototype.fly = function () {
    return "fly";
  };

  return Bird;
})();
```

### `extends` 키워드

```js
class Bird extends Animal {}
```

### 동적 상속

`extends` 키워드로 클래스뿐만 아니라 생성자 함수도 상속받을 수 있다. 단, `extends` 키워드 앞에 클래스가 와야 한다.

`extends` 키워드 다음에는 클래스뿐만 아니라 `[[Construct]]` 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를 통해 동적으로 상속받을 대상을 결정할 수 있다.

```js
function Base1() {}

class Base2 {}

let condition = true;

// 조건에 따라 동적으로 상속 대상을 결정하는 서브클래스
class Derived extends (condition ? Base1 : Base2) {}

const derived = new Derived();
console.log(derived); // Derived {}

console.log(derived instanceof Base1); // true
console.log(derived instanceof Base2); // false
```

### 서브클래스의 constructor

서브클래스에서 `constructor` 메서드를 생략하면 클래스에 다음과 같은 `constructor`가 암묵적으로 정의된다. `args`는 `new` 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트다.

```js
constructor(...args) { super(...args) }
```

**`super()`는 수퍼클래스의 `constructor`를 호출하여 인스턴스를 생성한다.**

### `super` 키워드

`super` 키워드는 함수처럼 호출할 수도 있고 `this`와 같이 식별자처럼 참조할 수 있는 특수한 키워드다.

- `super`를 호출하면 수퍼클래스의 `constructor`를 호출한다.
- `super`를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

**`super` 호출**

수퍼클래스에서 추가한 프로퍼티와 서브클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 `constructor`를 생략할 수 없다.

```js
class Base {
  constructor(a, b) {
    this.a = a;
    this.b = b;
  }
}

class Derived extends Base {
  constructor(a, b, c) {
    super(a, b);
    this.c = c;
  }
}

const derived = new Derived(1, 2, 3);
console.log(derived); // Derived {a: 1, b: 2, c: 3}
```

`super`를 호출할 때 주의해야 할 사항이 있다.

1. 서브클래스에서 `constructor`를 생략하지 않는 경우, 서브클래스의 `constructor`에서는 반드시 `super`를 호출해야 한다.

```js
class Base {}

class Derived extends Base {
  constructor() {
    // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
    console.log("constructor call");
  }
}

const derived = new Derived();
```

2. 서브클래스의 `constructor`에서 `super`를 호출하기 전에는 `this`를 참조할 수 없다.

```js
class Base {}

class Derived extends Base {
  constructor() {
    // ReferenceError: Must call super constructor in derived class before accessing 'this' or returning from derived constructor
    this.a = 1;
    super();
  }
}

const derived = new Derived(1);
```

3. `super`는 반드시 서브클래스의 `constructor`에서만 호출한다. 그렇지 않은 경우 에러가 발생한다.

**`super` 참조**

메서드 내에서 `super`를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.

1. 서브클래스의 프로토타입 메서드 내에서 `super.sayHi`는 수퍼클래스의 프로토타입 메서드 `sayHi`를 가리킨다.

`super.sayHi()`는 아래 코드와 동일하게 동작한다.

```js
class Base {
  constructor(name) {
    this.name = name;
  }

  sayHi() {
    return `Hi! ${this.name}`;
  }
}

class Derived extends Base {
  sayHi() {
    // __super는 Base.prototype을 가리킨다.
    const __super = Object.getPrototypeOf(Derived.prototype);
    return `${__super.sayHi.call(this)} how are you doing?`;
  }
}
```

`super`는 자신을 참조하고 있는 메서드(서브클래스의 `sayHi`)가 바인딩되어 있는 객체(서브클래스의 프토토타입)의 프로토타입(수퍼클래스의 프로토타입)을 가리킨다. 따라서 `super.sayHi`는 수퍼클래스의 프로토타입의 `sayHi`를 가리킨다.

단, `super.sayHi`, 즉 수퍼클래스의 프로토타입의 메서드를 호출할 때 `call` 메서드를 사용해 `this`를 전달해야 한다. `this`를 사용하지 않으면 `Base.prototype.sayHi` 메서드 내부는 `Base.prototype`을 가리킨다.

`super` 참조는 `super`를 참조하는 메서드가 바인딩되어 있는 객체의 프로토타입을 찾아가야 하는데, 이를 위해서 메서드는 내부 슬롯 **`[[HomeObject]]`를 가지며**, 자신을 바인딩하고 있는 객체를 가리킨다.

```js
/*
[[HomeObject]]는 메서드 자신을 바인딩하고 있는 객체를 가리킨다.
[[HomeObject]]를 통해 메서드 자신을 바인딩하고 있는 객체의 프로토타입을 찾을 수 있다.
예를 들어, Derived 클래스의 sayHi 메서드는 Derived.prototype에 바인딩되어 있다.
따라서 Derived 클래스의 sayHi 메서드의 [[HomeObject]]는 Derived.prototype이고
이를 통해 Derived 클래스의 sayHi 메서드 내부의 super 참조가 Base.prototype으로 결정된다.
따라서 super.sayHi는 Base.prototype.sayHi를 가리키게 된다.
*/
super = Object.getPrototypeOf([[HomeObject]])
```

**⚠️ES6의 메서드 축약 표현으로 정의된 함수만이 `[[HomeObject]]`를 갖는다.** 이 내부 슬롯이 있는 함수만이 `super` 참조가 가능하다.

`super` 참조는 객체 리터럴에서도 사용할 수 있다. 단 메서드 축약 표현으로 정의해야 한다.

2. 서브클래스의 정적 메서드 내에서 `super.sayHi`는 수퍼클래스의 정적 메서드 `sayHi`를 가리킨다.

```js
class Base {
  static sayHi() {
    return "Hi!";
  }
}

class Derived extends Base {
  static sayHi() {
    // super.sayHi는 수퍼클래스의 정적 메서드를 가리킨다.
    return `${super.sayHi()} how are you doing?`;
  }
}

console.log(Derived.sayHi()); // Hi! how are you doing?
```

### 상속 클래스의 인스턴스 생성 과정

1. 서브클래스의 `super` 호출

자바스크립트 엔진은 클래스를 평가할 때 수퍼클래스, 서브클래스를 구분하기 위해 `base` 또는 `derived`를 값으로 갖는 내부 슬롯 `[[ConstructorKind]]`를 갖는다. 상속받지 않는 클래스(또는 생성자 함수)는 `base`를, 상속받는 클래스는 `derived`를 갖게 된다. 이를 통해 `new` 연산자와 함께 호출되었을 때의 동작이 구분된다.

서브클래스는 수퍼클래스에게 인스턴스 생성을 위임한다. 때문에 서브클래스의 `constructor`에서 반드시 `super`를 호출해야 한다.

`super`가 호출되면 수퍼클래스가 평가되어 함수 객체의 코드가 실행되기 시작한다.

2. 수퍼클래스의 인스턴스 생성과 `this` 바인딩

인스턴스는 `this`에 바인딩된다. 따라서 수퍼클래스의 `constructor` 내부의 `this`는 생성된 인스턴스를 가리킨다.

```js
class Rectangle {
  constructor(width, height) {
    // 암묵적으로 빈 객체, 즉 인스턴스가 생성되고 this에 바인딩된다.
    console.log(this); // ColorRectangle {}
    // new 연산자와 함께 호출된 함수, 즉 new.target은 ColorRectangle이다.
    console.log(new.target); // ColorRectangle
  ...
```

인스턴스는 수퍼클래스가 생성한다. 하지만 `new` 연산자와 함께 호출된 함수를 가리키는 `new.target`은 서브클래스를 가리킨다. 따라서 인스턴스는 **`new.target`이 가리키는 서브클래스가 생성한 것으로 처리된다**.

따라서 생성된 인스턴스의 프로토타입은 `new.target`, 즉 서브클래스의 `prototype` 프로퍼티가 가리키는 객체가 된다.

```js
  ...
    // 생성된 인스턴스의 프로토타입으로 ColorRectangle.prototype이 설정된다.
    console.log(Object.getPrototypeOf(this) === ColorRectangle.prototype); // true
    console.log(this instanceof ColorRectangle); // true
    console.log(this instanceof Rectangle); // true
  ...
```

3. 수퍼클래스의 인스턴스 초기화

수퍼클래스의 `constructor`가 실행되어 `this`에 바인딩된 인스턴스를 초기화한다.

`this`에 바인딩된 인스턴스에 프로퍼티를 추가하고 `constructor`가 인수로 전달받은 초기값으로 프로퍼티를 초기화한다.

```js
    // 인스턴스 초기화
    this.width = width;
    this.height = height;

    console.log(this); // ColorRectangle {width: 2, height: 4}
  }
...
```

4. 서브클래스 `constructro`로의 복귀와 `this` 바인딩

`super` 호출이 종료되고 서브클래스 `constructor`로 제어 흐름이 돌아온다. 이때 `super`가 반환한 인스턴스가 `this`에 바인딩된다. 서브클래스는 별도의 인스턴스를 생성하지 않고 수퍼클래스로부터 받은 인스턴스를 그대로 `this`에 바인딩하여 사용한다.

```js
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);

    // super가 반환한 인스턴스가 this에 바인딩된다.
    console.log(this); // ColorRectangle {width: 2, height: 4}
...
```

5. 서브클래스의 인스턴스 초기화

`super` 호출 이후, 서브클래스의 `constructor`에 기술된 인스턴스 초기화가 실행된다. 즉, `this`에 바인딩되어 있는 인스턴스에 프로퍼티를 추가하고 `constructor`가 인수로 전달받은 초기값으로 인스턴스의 프로퍼티를 초기화한다.

6. 인스턴스 반환

모든 처리가 끝나면 완성된 인스턴스가 바인딩된 `this`가 암묵적으로 반환된다.

```js
class ColorRectangle extends Rectangle {
  constructor(width, height, color) {
    super(width, height);

    // super가 반환한 인스턴스가 this에 바인딩된다.
    console.log(this); // ColorRectangle {width: 2, height: 4}

    // 인스턴스 초기화
    this.color = color;

    // 완성된 인스턴스가 바인딩된 this가 암묵적으로 반환된다.
    console.log(this); // ColorRectangle {width: 2, height: 4, color: "red"}
  }
...
```

### 표준 빌트인 생성자 함수 확장

표준 빌트인 생성자도 `[[Construct]]` 내부 메서드를 갖는 생성자 함수이므로 `extends` 키워드를 사용하여 확장할 수 있다.

```js
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
  // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);
console.log(myArray); // MyArray(4) [1, 1, 2, 3]

// MyArray.prototype.uniq 호출
console.log(myArray.uniq()); // MyArray(3) [1, 2, 3]
// MyArray.prototype.average 호출
console.log(myArray.average()); // 1.75
```

`MyArray` 클래스의 인스턴스는 `Array.prototype`과 `MyArray.prototype`의 모든 메서드를 사용할 수 있다.

`map, filter`와 같이 새로운 배열을 반환하는 메서드가 `MyArray` 클래스의 인스턴스를 반환해야만 메서드 체이닝을 사용할 수 있다.

```js
// 메서드 체이닝
// [1, 1, 2, 3] => [ 1, 1, 3 ] => [ 1, 3 ] => 2
console.log(
  myArray
    .filter((v) => v % 2)
    .uniq()
    .average()
); // 2
```

만약 `MyArray` 클래스의 `uniq` 메서드가 `Array`를 반환하게 하려면 `Symbol.species`를 사용하여 정적 접근자 프로퍼티를 추가한다.

```js
// Array 생성자 함수를 상속받아 확장한 MyArray
class MyArray extends Array {
  // 모든 메서드가 Array 타입의 인스턴스를 반환하도록 한다.
  static get [Symbol.species]() {
    return Array;
  }

  // 중복된 배열 요소를 제거하고 반환한다: [1, 1, 2, 3] => [1, 2, 3]
  uniq() {
    return this.filter((v, i, self) => self.indexOf(v) === i);
  }

  // 모든 배열 요소의 평균을 구한다: [1, 2, 3] => 2
  average() {
    return this.reduce((pre, cur) => pre + cur, 0) / this.length;
  }
}

const myArray = new MyArray(1, 1, 2, 3);

console.log(myArray.uniq() instanceof MyArray); // false
console.log(myArray.uniq() instanceof Array); // true

// 메서드 체이닝
// uniq 메서드는 Array 인스턴스를 반환하므로 average 메서드를 호출할 수 없다.
console.log(myArray.uniq().average());
// TypeError: myArray.uniq(...).average is not a function
```
