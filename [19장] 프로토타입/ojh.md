- 🔑 **자바스크립트는 명령형, 함수형, 프로토타입 기반 객체지향 프로그래밍 언어를 지원하는 멀티 패러다임 언어다.**
- ❗ ES6 에서 클래스가 도입되었다. 하지만 클래스 역시 함수이며, 기존 프로토타입 기반 패턴의 문법적 설탕이다. → 클래스와 생성자 함수 모두 프로토타입 기반의 인스턴스를 생성하지만 정확히 동일하게 동작하지는 않는다. 클래스는 생성자 함수보다 엄격하며 생성자 함수가 제공하지 않는 기능도 제공한다.

# 19.1 객체 지향 프로그래밍

---

- 명령형 프로그래밍의 절차지향적 관점에서 벗어나 여러 개의 독립적 단위, 즉 객체의 집합으로 표현하는 프로그래밍 패러다임.
- 객체 == 실체 → 실체의 특징이나 성질을 속성 (property)
- 다양한 속성 중 프로그램에 필요한 속성만 간추리는 것을 추상화라 한다.
- 객체지향 프로그래밍은 객체의 상태를 나타내는 데이터와 데이터를 조작할 수 있는 동작을 하나의 논리적인 단위로 묶어 생각한다. → 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조.
- **상태: 프로퍼티 , 동작: 메서드**
- 객체는 독릭적인 부품으로 볼 수 있지만, 다른 객체와 관계성을 가질 수도 있고, 상속 받거나, 데이터를 주고 받을 수 있다..

# 19.2 상속과 프로토타입

---

- **상속은 프로퍼티나 메서드를 다른 객체가 상속받아 그대로 사용할 수 있는 것을 말한다.**

```jsx
function Circle(radius) {
  this.radius = radius;
  this.getArea = function () {
    return Math.PI * this.radius * 2;
  };
}

const cir1 = new Circle(1);
const cir2 = new Circle(2);

console.log(cir1.getArea === cir2.getArea); // false
console.log(cir1.getArea(), cir2.getArea());
```

- ⭐ 생성자 함수는 문제가 존재한다. 위의 getArea 메서드는 모든 객체가 가지지만, Circle 생성자 함수로 새로운 객체를 생성할 때마다 getArea 메서드를 중복 생성하고 모두 중복 소유한다.
- 이는 모든 인스턴스가 동일한 메서드를 중복 소유하여 메모리를 불필요하게 낭비하며, 인스턴스를 생성할 때마다 메서드를 생성하므로 퍼포먼스에도 악영향을 준다.
- ⭐상속을 사용하여 불필요한 중복을 제거할 수 있다. 자바스크립트는 프로토타입 기반으로 상속을 구현한다.

```jsx
function Circle(radius) {
  this.radius = radius;
}

Circle.prototype.getArea = function () {
  return Math.PI * this.radius * 2;
};

const cir1 = new Circle(1);
const cir2 = new Circle(2);

console.log(cir1.getArea === cir2.getArea); // true 하나의 메서드를 공유한다!
console.log(cir1.getArea(), cir2.getArea());
```

- Circle 생성자 함수가 생성한 모든 인스턴스는 자신의 프로토타입, 상위 객체 역할을 하는 Circle.prototype 의 모든 프로퍼티와 메서드를 상속받는다.
- getArea 메서드는 단 하나만 생성되어 프로토타입인 Circle.prototype의 메서드로 할당되어 있다.
- 상속은 코드의 재사용이란 관점에서 매우 유용하다!

# 19.3 프로토타입 객체

---

- 프로토타입 객체는 객체 간 상속을 구현하기 위해 사용된다.
- 모든 객체는 [[Prototype]] 이라는 내부 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참조이다. (null 인 경우도 있다)
- [[Prototype]] 에 저장되는 프로토타입은 객체 생성 방식에 의해 결정된다.
- ⚠️모든 객체는 하나의 프로토타입을 가진다. 모든 프로토타입은 생성자 함수와 연결되어 있다.

![image.png](attachment:f34bd788-bd68-41c4-ad68-81693818a1d9:image.png)

- **⭐ [[Prototype]] 내부 슬롯에는 직접 접근할 수 없지만, 그림처럼 **proto** 접근자 프로퍼티를 통해 자신의 프로토타입, [[Prototype]] 내부 슬롯이 가르키는 프로토타입에 간접적으로 접근할 수 있다.**
- **⭐ 그리고 프로토타입은 자신의 constructor 프로퍼티를 통해 생성자 함수에 접근할 수 있고 , 생성자 함수는 자신의 prototype 프로퍼티를 통해 프로토타입에 접근할 수 있다.**

1. ****proto** 접근자 프로퍼티**

   - 모든 객체는 **proto** 접근자 프로퍼티를 통해 자신의 프로토타입, 즉, [[Prototype]] 내부 슬롯에 간접적으로 접근할 수 있다.

   ![image.png](attachment:18c27a85-6da8-4789-ab08-0e736cacfe99:image.png)

   - 빨간 박스가 person 객체의 프로토타입인 Object.prototype 이다.
   - **⚠️ **proto**는 접근자 프로퍼티이다.**
   - 16 장에서 다루었든 내부 슬롯은 프로퍼티가 아니기 때문에 원칙적으로 내부 슬롯과 내부 메서드에 직접적으로 접근하거나 호출할 수는 없다. → 단, 일부 내부 슬롯과 메서드에 한하여 간접적으로 접근할 수단을 제공해주는 데, 이 중 하나가 **proto** 접근자 프로퍼티이다.
   - 접근자 프로퍼티는 이전 장에서 다루엇든 값 어트리뷰트를 가지고 있지 않고 다른 데이터 프로퍼티를 읽거나 저장할 때 사용하는 접근자 함수, get, set 프로퍼티 어트리뷰트로 구성된 프로퍼티이다.

   ```jsx
   console.log(Object.getOwnPropertyDescriptor(Object.prototype, "__proto__"));

   {
     get: [Function: get __proto__],
     set: [Function: set __proto__],
     enumerable: false,
     configurable: true
   }
   ```

   ![image.png](attachment:f4822735-d72d-4047-a4bf-cb059503894a:image.png)

   - object.prototype의 접근자 프로퍼티인 **proto** 는 get , set 을 통해 내부 슬롯의 값을 취득하거나 할당한다. → **proto** 접근자 프로퍼티를 통해 프로토타입에 접근하면 내부적으로 getter 함수가 호출되고, **proto** 접근자 프로퍼티를 통해 새로운 프로토타입을 할당하면 setter 함수가 호출된다.

   ![image.png](attachment:acf7b613-159a-433b-b5cf-c7fd4764f81b:image.png)

   ![image.png](attachment:7ddac8b0-b153-492a-98c9-ffef45a26999:image.png)

   - **⚠️ **proto** 접근자 프로퍼티는 상속을 통해 사용된다.**
   - **proto** 접근자 프로퍼티는 객체가 직접 소유하는 프로퍼티가 아니라 Object.prototype의 프로퍼티이다. 모든 객체는 상속을 통해 Object.prototype.**proto** 접근자 프로퍼티를 사용할 수 있다.

   ```jsx
   const person = { name: "Lee" };
   console.log(person.hasOwnProperty("__proto__")); // false
   console.log(Object.prototype.hasOwnProperty("__proto__")); // true
   console.log({}.__proto__ === Object.prototype); // true
   ```

   - **⭐ 모든 객체는 프로토타입 계층 구조인 프로토타입 체인에 묶여 있다. 자바스크립트 엔진은 객체의 프로퍼티에 접근하려고 할 때, 해당 객체에 없다면 **proto** 접근자 프로퍼티가 가르키는 참조를 따라 자신의 부모 역할을 하는 프로토타입 프로퍼티를 순차적으로 검색하고, 종점은 Object.prototype 이다.**
   - **⚠️ **proto** 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유**
   - ❗[[Prototype]] 내부 슬롯의 값, 즉 프로토타입에 접근하기 위해 접근자 프로퍼티를 사용하는 이유는 상호 참조에 의해 프로토타입 체인이 생성되는 것을 방지하기 위해서다!

   ```jsx
   const parent = {};
   const child = {};

   child.__proto__ = parent;
   parent.__proto__ = child; // typeError 발생 TypeError: Cyclic __proto__ value
   ```

   - 위의 코드가 에러 없이 정상 처리되면 서로가 자신의 프로토타입이 되는 비정상 적인 체인이 만들어지기 때문에 **proto** 접근자 프로퍼티는 에러를 발생시킨다.
   - ❗프로토타입 체인은 단방향 링크드 리스트로 구현되어야 한다. → 아무런 체크없이 무조건적으로 프로토타입을 교체할 수 없도록 **proto** 접근자 프로퍼티에 구현되어 있다.
   - **⚠️ **proto** 접근자 프로퍼티를 코드 내에서 직접 사용하는 것은 권장되지 않는다.**
   - **proto** 는 ES5 까지 비표준이였음. → ES6 부터 표준이 되었고, 지금은 대부분의 브라우저가 지원한다.
   - ❗ 하지만 코드 내에서 직접 사용하는 것은 권장하지 않음. → 모든 객체가 **proto** 접근자 프로퍼티를 사용할 수 있는게 아니기 때문, 나중에는 직접 상속을 통해 다음과 같이 Object.prototype 을 상속받지 않는 객체를 생성할 수 있기 때문에 사용할 수 없는 경우가 있다.
   - ⭐ 먼저 `Object.create`는 자바스크립트에서 **새로운 객체를 만들되, 그 객체의 프로토타입을 직접 지정할 수 있게 해주는 함수**야.

   ```jsx
   const obj1 = {}; // 내부적으로 Object.create(Object.prototype)
   ```

   ```jsx
   // 프로토타입의 종점이기 때문에 상속받을 수 없다.
   const obj = Object.create(null);
   console.log(obj.__proto__); // undefiend
   console.log(Object.getPrototypeOf(obj)); // null
   ```

   - ❗ 그렇기에 참조를 취득하고 싶은 경우에는 Object.getPropertyOf 메서드를, 교체하고 싶을 때는 Object.setPropertyOf 메서드를 사용하는 것을 권장한다.

   ```jsx
   const obj = {};
   const parent = { x: 1 };
   console.log(Object.getPrototypeOf(obj));
   Object.setPrototypeOf(obj, parent);
   console.log(Object.getPrototypeOf(obj));
   ```

   - Object.getPropertyOf 는 ES5 도입, IE9 이상 지원
   - Object.setPropertyOf 는 ES6 도입, IE11 이상 지원

1. **함수 객체의 prototype 프로퍼티**

   - ⭐ **이전 장에서 말했듯, 함수 객체만이 소유하는 prototype 프토퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가르킨다.**
   - prototype 프로퍼티는 생성자 함수가 생성할 객체의 프로토타입을 가리킨다. 즉, non-contructor 인 화살표 함수와 ES6 메서드 축약 표현 메서드는 소유하지도 정의하지도 않는다.

   ![image.png](attachment:415e4aba-5d7e-45a4-9fd7-89ee8c3fecbc:image.png)

   - ❗생성자 함수로 호출하기 위해 정의하지 않은 일반 함수도 prototype 프로퍼티를 소유하지만, 객체를 생성하지 않는 일반 함수의 프로퍼티는 아무런 의미가 없다.
   - **🌟 모든 객체가 가지고 있는 (Object.prototype 으로부터 상속받은) **proto** 접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가르킨다.** 하지만 주체가 다르다.

   ![image.png](attachment:d730d2db-a600-4734-be35-5cae69d4dbb6:image.png)

   ![image.png](attachment:64fe006f-1cd5-4db1-adda-c13245ded1ae:image.png)

1. **프로토타입의 constructor 프로퍼티와 생성자 함수**

   - 모든 프로토타입은 constructor 프로퍼티를 갖는다. 이 프로퍼티는 자신을 참조하고 있는 생성자 함수를 가르킨다.

   ```jsx
   function Person(name) {
     this.name = name;
   }

   const me = new Person("Lee");
   console.log(Person.prototype === me.__proto__); // true 위의 그림 상황
   console.log(me.constructor === Person); // true
   ```

   ![image.png](attachment:dfd6a1be-dff0-4d76-b76b-4cc13206fc31:image.png)

   - me 객체의 프로토타입인 Person.prototype 의 constructor 프로퍼티를 통해 생성자 함수와 연결된다.

# 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

---

- 위와 같이 생성자 함수에 의해 생서된 인스턴스는 프로토타입의 constructor 프로퍼티에 의해 생성자 함수와 연결된다.
- 하지만 리터럴 표기법에 의한 객체 생성과 같이 명시적으로 new 를 사용하지 않고 객체를 생성하는 방식도 있다. EX ) 객체 리터럴, 함수 리터럴, 배열 리터럴, 정규 표현식 리터럴
- ❗리터럴 표기법에 의해 생성된 객체도 물론 프로토타입이 존재한다. 하지만 리터럴 표기법에 의해 생성된 객체의 경우 프로토타입의 constructor 프로퍼티가 가르키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없다.

```jsx
const obj = {};
console.log(obj.constructor === Object); // true
```

- obj 는 생성자 함수로 생성한 객체가 아닌 리터럴로 생성한 객체이다. 하지만 Object 생성자 함수와 constructor 프로퍼티로 연결되어 있다. → 그렇다면 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수로 생성되는 것은 아닐까?

![image.png](attachment:0dc4625f-7340-47a4-bb81-112ed49bea03:image.png)

![image.png](attachment:04b8335a-7399-47dc-a4a8-6acfc719c3c0:image.png)

- 객체 리터럴이 평가될 때는 다음과 같이 추상 연산 OrdinaryObjectCreate 를 호출하여 빈 객체를 생성하고 프로퍼티를 추가하도록 정의되어 있다. → 이처럼 Object 생성자 함수 호출과 객체 리터럴의 평가는 OrdinaryObjectCreate 를 호출하여 빈 객체를 생성하는 점에서 동일하나 [new.target](http://new.target) 의 확인이나 프로퍼티를 추가하는 등 세부 내용은 다르다.**❗ 그렇기에 객체 리터럴에 의해 생성된 객체는 Object 생성자 함수가 생성한 객체가 아니다!**
- 함수는 차이가 더욱 명확하다. Function 생성자 함수를 호출하여 생성한 생성한 함수는 렉시컬 스코프를 만들지 않고 전역 함수 인 것처럼 스코프를 생성하며 클로져도 만들지 않는다. → **❗ 즉, 함수 선언문과 함수 표현식으로 함수 생성한 것은 Function 생성자 함수가 아니다.** → 하지만 역시 constructor 프로퍼티를 통해 확인해보면 생성자 함수는 Function 생성자 함수다.
- 🌟 리터럴 표기법에 의해 생성된 객체도 상속을 위해 프로토타입이 필요하다. 따라서 리터럴에 의해 생성된 객체도 생성자 함수를 갖는다. → 🌟 **즉, 하고 싶은 말은 프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다. → 리터럴 표기법에 의해 생성된 객체는 생성자 함수에 의해 생성된 객체는 아니지만 본질적인 면에서 큰 차이가 없다.**
- **🌟 따라서 프로토타입의 constructor 프로퍼티를 통해 연결되어 있는 생성자 함수를 리터럴 표기법으로 생성한 생성자 함수로 생각해도 큰 무리는 없다.**

![image.png](attachment:10ce2a7a-ad75-4281-8595-2f32b74099c1:image.png)

# 19.5 프로토타입의 생성 시점

---

- 결국 모든 객체는 생성자 함수와 연결되어 있다! ← 생성 방법이 어떻든
- **🌟 프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.** ← 언제나 쌍으로 존재하기 때문
- 생성자 함수는 `사용자 정의 생성자 함수`와 `빌트인 생성자 함수`로 구분할 수 있다.

1. **사용자 정의 생성자 함수와 프로토타입 생성 시점**

   - 생성자 함수로서 호출할 수 있는 함수, 즉, constructor는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.

   ```jsx
   console.log(Object.getOwnPropertyDescriptors(Person.prototype));

   {
     constructor: {
       value: [Function: Person],
       writable: true,
       enumerable: false,
       configurable: true
     }
   }

   function Person(name) {
     this.name = name;
   }

   const Person2 = (name) => {
     this.name = name;
   };

   console.log(Person2.prototype); // undefined
   ```

   - non-constructor 는 생성되지 않는다.
   - 함수 선언문은 런타임 이전에 자바스크립트 엔진에 의해 먼저 실행되어 함수 객체가 된다. 이와 동시에 프로토타입도 생성된다.

   ![image.png](attachment:1b42126e-e710-4585-b5f4-c8fa23ed59bf:image.png)

   - 생성된 프로토타입은 오직 constructor 만 가지는 객체다. 이렇게 생성된 프로토타입의 프로토 타입은 Object.prototype 이다.

   ![image.png](attachment:881d8f97-198a-49ec-8ca0-2d5d5d4407e7:image.png)

   - 이처럼 빌트인 생성자 함수가 아닌 사용자 정의 생성자 함수는 자신이 평가되어 객체를 생성하는 시점에 프로토타입도 생성된다.

1. **빌트인 생성자 함수와 프로토타입 생성 시점**

   - Object, Array, String, Number, Function, RegExp, Date ,Promise 등
   - 빌트인 생성자 함수도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다.
   - 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성된다. 생성된 프로토타입은 빌트인 생성자 함수의 prototype 프로퍼티에 바인딩된다.

   ![image.png](attachment:09333cba-075f-426d-a1f0-a49dc564cf4a:image.png)

   - ❗ 전역 객체는 특수한 객체, 브라우저 (클라이언트 사이드) 에서는 window, 서버 사이드는 global → 전역 객체는 표준 빌트인 객체와 호스트 객체 (클라이언트 Web api 또는 Node.js 의 호스트 api) var 키워드 전역 변수 등을 프로퍼티로 가짐. → ⚠️ Math, Reflect, JSON 을 제외한 표준 빌트인 객체는 모두 생성자 함수다.
   - window.Object === Object 는 true
   - **🌟 이처럼 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화되어 존재한다. 이후 객체를 생성하면 프로토타입은 생성된 객체의 [[Prototype]] 내부 슬롯에 할당된다. → 이로서 객체는 프로토타입을 상속받는다.**

# 19.6 객체 생성 방식과 프로토타입의 결정

---

- 객체 생성 방식
  - 객체 리터럴
  - Object 생성자 함수
  - 생성자 함수
  - Object.create 메서드
  - 클래스
- ❗다양한 방식으로 생성된 모든 객체는 각 방식마다 세부적인 생성 방식의 차이는 있으나 추상 연산 ordinaryObjectCreate에 의해 생성된다는 공통점이 있다.
- ⚠️ 프로토타입은 추상 연산 OrdinaryObjectCreate 에 전달되는 인수에 의해 결정된다. 이 인수는 객체가 생성되는 시점에 객체 생성 방식에 의해 결정된다.

1. **객체 리터럴에 의해 생성된 객체의 프로토타입**

   - 자바스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때, 추상 연산 OrdinaryObjectCreate 를 호출한다. 이 때 전달되는 프로토 타입은 Object.prototype이다.

   ```jsx
   const obj = { x: 1 };
   ```

   - 위 객체 리터럴이 평가되면 추상연산 OrdinaryObjectCreate 에 의해, Object 생성자 함수와 Object.prototype과 생성된 객체 사이이 연결이 만들어진다.

   ![image.png](attachment:5da65b81-f839-40dd-8eb4-8b54d29c0e76:image.png)

   - 💬 **위처럼 obj 객체는 Object.prototype 을 가지고 상속받게 되며, 상속받아 프로퍼티와 메서드를 사용할 수 있게 된다.**

1. **Object 생성자 함수에 의해 생성된 객체의 프로토타입**

   - Object 생성자 함수를 인수 없이 호출하면 빈 객체가 생성된다. 이 때 추상 연산 OrdinaryObjectCreate 에 전달되는 프로토타입은 Object.prototype이다.

   ```jsx
   const obj = new Object();
   obj.x = 1;
   ```

   - 위 코드가 실행되면 추상 연산에 의해 다음과 같이 연결이 만들어짐. 객체 리터럴에 의해 생성된 객체와 동일한 구조

   ![image.png](attachment:732769e3-7647-423b-b2cd-5dd7dec98027:image.png)

   - 역시 상속을 받기 때문에 사용할 수 있다.
   - ❗객체 리터럴과 Object 생성자 함수에 의한 객체 생성 방식 차이는 프로퍼티 추가하는 방식에 있다. 객체 리터럴 방식은 리터럴 내부에 Object 생성자 함수는 빈 객체를 생성한 이후 추가해야 한다.

1. **생성자 함수에 의해 생성된 객체의 프로토타입**

   - new 연산자와 생성자 함수 함께 호출하는 방식 역시 추상 연산 OrdinaryObjectCreate 가 호출된다. → 이 때 **추상 연산에 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체이다.**

   ```jsx
   function Person(name){
   	this.name = name;
   }
   const me = new Person("Lee);
   ```

   - 추상 연산에 의해 다음과 같이 객체와 생성자 함수의 prototype 에 바인딩되어있는 객체 사이의 연결이 만들어진다.

   ![image.png](attachment:a784aa0a-b8d9-452f-b4e2-0a7fda13495b:image.png)

   - 사용자 정의 생성자 함수는 Person 과 더불어 생성된 프로토타입 Person.prototype 의 프로퍼티는 constructor 뿐임.

   ```jsx
   function Person(name) {
     this.name = name;
   }

   Person.prototype.sayHello = function () {
     console.log(this.name);
   };

   const me = new Person("Lee");
   const you = new Person("Kim");

   me.sayHello(); // Lee
   you.sayHello(); // Kim
   ```

   - Person 생성자 함수를 통해 생성된 모든 객체는 프로토타입에 추가된 sayHello 메서드를 상속받아 자신의 메서드처럼 사용 가능

   ![image.png](attachment:a8373d05-8a51-4866-ab0d-0c062f2293a6:image.png)

# 19.7 프로토타입 체인

---

```jsx
function Person(name) {
  this.name = name;
}

Person.prototype.sayHello = function () {
  console.log(this.name);
};

const me = new Person("Lee");

console.log(me.hasOwnProperty("name")); // true
```

- me 객체는 Object.prototype 의 메서드인 hasOwnProperty를 호출할 수 있다. 이는 me 객체가 Person.prototype 뿐만 아니라 Object.prototype도 상속 받았다는 것을 의미한다.

```jsx
Object.getPrototypeOf(Person.prototype) === Object.prototype;
```

- Person.prototype 의 프로토타입은 Object.prototype. 프로토타입의 프로토타입은 언제나 Object.prototype이다.

![image.png](attachment:2ae40eb7-5f89-450c-bad5-8dceb88f2ef6:image.png)

- **🌟 자바스크립트는 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 접근하려는 프로퍼티가 없다면 [[Prototype]] 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색한다. 이를 프로토타입 체인이라하며, 이는 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.**

```jsx
me.hasOwnProperty("name");
```

- 먼저 me 객체에서 검색 → 없으므로 Person.prototype 이동 후 검색 → 없으므로 Object.prototype 으로 이동 존재하므로 Object.prototype.hasOwnProperty 메서드 호출 이때, 메서드의 this에는 me 객체가 바인딩됨.

```jsx
Object.prototype.hasOwnProperty.call(me, "name");
```

- ❗ call 은 this 를 설정해주는 함수로 뒤에서 다룰 것.
- **🌟 프로토타입 체인의 종점**은 언제나 Object.prototype 임. → Object.prototype 의 [[Prototype]] 의 내부 슬롯의 값은 null 이다.
- 종점에서도 프로퍼티가 없는 경우 undefined 임.
- ❗ **스코프 체인과 프로퍼티 체인은 서로 연관없이 별도로 동작하는 것이 아니라 서로 협력하여 식별자와 프로퍼티를 검색하는 것에 사용된다.**

# 19.8 오버라이딩과 프로퍼티 섀도잉

---

![image.png](attachment:f05c93d7-a7ea-4b65-996c-b2ade72c5376:image.png)

- 생성자 함수로 객체를 생성한 후, 인스턴스에 메서드를 추가했다. 그림은 아래와 같다.

![image.png](attachment:ebece8d5-f5f2-406f-8bca-6bf22fc9f22a:image.png)

- 프로토타입이 소유한 프로퍼티를 프로토타입 프로퍼티, 인스턴스가 소유한 프로퍼티를 인스턴스 프로퍼티라고 부른다.
- 지금 인스턴스 메서드 sayHello 가 프로토타입 메서드 sayHello 를 **오버라이딩**했고, 프로토타입 메서드 sayHello 는 가려진다. → 이를 **프로퍼티 섀도잉**이라고 함.
- 🌟 오버로딩은 함수명은 동일하지만 인수가 다른 것인데, 자바스크립트는 오버로딩은 지원하지 않지만, arguments 객체를 사용해 구현할 수 있다.
- delete me.sayHello 로는 인스턴스의 메서드만 삭제할 수 있음 → **당연히 하위 객체를 통해 상위 프로토타입의 프로퍼티를 변경, 삭제 하는 것을 불가능하다. → 즉, get 은 가능 , set 은 불가능**
- delete Person.prototype.sayHello 와 같이 직접 접근해서 삭제해야 한다.

# 19.9 프로토타입의 교체

---

- 프로토타입은 임의의 다른 객체로 변경할 수 있다. → 즉 부모 객체인 프로토타입을 동적으로 변경할 수 있다는 의미이다. 상속 관계를 동적으로 변경할 수 있다.
- 생성자 함수나 인스턴스에 의해 교체할 수 있다.

1. **생성자 함수에 의한 프로토타입 교체**

   ![image.png](attachment:61e097f0-28ea-4de3-ba51-965e56264993:image.png)

   - 위 예제에서 1 에 객체 리터털을 Person.Prototype 에 할당했다. 이는 아래와 같다.

   ![image.png](attachment:7cfaf379-0ca5-46f7-b7dd-4ef7e3cb0162:image.png)

   - 프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없다. 그렇기 때문에 me 객체의 생성자 함수를 검색하면 Object 가 나오게 된다. → 이처럼 프로토타입을 교체하면 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴된다. 아래는 연결을 되살리는 코드이다.
   - 객체 리터럴에 constructor 프로퍼티를 추가하여 되살릴 수 있다.

   ![image.png](attachment:3ac04c79-3c2d-41fb-84a9-306357068089:image.png)

1. **인스턴스의 의한 프로토타입의 교체**

   - 인스턴스의 **proto** 접근자 프로퍼티를 통해 교체할 수 있다. → 이미 생성된 객체의 프로토타입을 교체하는 것임!

   ![image.png](attachment:b88011a3-034c-40f2-b687-4d81b934071b:image.png)

   - 1에서 me 객체의 프로토타입을 parent 객체로 교체했다.

   ![image.png](attachment:1273f52c-82d7-4566-8d70-39871db7c7d5:image.png)

   - 역시 교체한 객체에는 constructor 프로퍼티가 없으므로 함수 간 연결이 파괴된다.
   - **🌟❗인스턴스에 의한 교체와 생성자 함수에 의한 교체가 별다른 차이가 없어 보이지만 미묘한 차이가 있다. → 인스턴스에 의한 교체는 생성자 함수와 교체된 프로토타입 간의 연결 조차 남아있지 않다.**

   ![image.png](attachment:3203ce79-7ee0-4e3c-8fe2-bf57c2b482f8:image.png)

   - ❗프로토타입으로 교체한 객체 리터럴에 constructor 프로퍼티를 추가하고 생성자 함수의 prototype 프로퍼티를 재설정하여 파과된 생성자 함수와 프로토타입간의 연결을 되살려 보자.

   ![image.png](attachment:1caedf32-8d8f-4562-9653-2b83fe54b83d:image.png)

   - 쌍방으로 연결을 해주어야 함..
   - 이처럼 프로토타입 교체를 통해 객체 간 상속을 동적으로 변경하는 것은 꽤나 번거로워, 프로토타입은 직접 교체하지 않는 편이 좋다.
   - 상속 관계를 인위적으로 설정하려면 직접 상속 또는 ES6 의 클래스를 사용하면 직관적으로 구현할 수 있다.

# 19.10 instanceof 연산자

---

- 좌변이 식별자, 우변이 생성자 함수를 가르키는 식별자이다. 우변의 피연산자 함수가 아닌 경우 typeError 가 발생한다.
- **🌟 프로토타입 체인 상에 존재하면 true 이고 아니면 false이다**
- ❗프로토타입 교체로 인해 생성자 함수와 프로토타입 간의 연결이 파괴된다면 Person 생성자 함수로 생성된 인스턴스여도 me instanceof Person 은 false 로 평가된다. → 이는 Person.protoype 이 me 객체의 프로토타입 체인 상에 존재하지 않기 때문이다. → 따라서 프로토타입으로 교체한 parent 객체를 Person 생성자 함수의 prototype 프로퍼티에 바인딩하면 me instanceof Person 은 true 로 평가될 것이다.
- **🌟❗ 이처럼 instanceof 연산자는 프로토타입의 constructor 가 가르키는 생성자 함수를 찾는 것이 아니라 생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.**

![image.png](attachment:45ce80e3-0a5a-45e2-ba3e-a988cae87674:image.png)

- me instanceof Person 의 경우 me 객체의 프로토타입 체인 상에 Person.prototype 에 바인딩된 객체가 존재하는지 확인한다. → ❗ 따라서 생성자 함수에 의해 프로토타입이 교체되어 constructor 프로퍼티와 생성자 함수 간의 연결이 파괴되어도 생성자 함수와 prototype 프로퍼티와 프로토타입 간의 연결은 파괴되지 않으므로 instanceof 는 아무런 영향을 받지 X

![image.png](attachment:379f6f86-3661-4bc9-a708-c9bc72c15538:image.png)

- 코드를 확인하면 constructor 와 생성자 함수 간의 연결을 파괴되었지만, 둘 다 true 가 나오는 것을 확인할 수 있다.

# 19.11 직접 상속

---

1. Object.create 에 의한 직접 상속

   - Object.create 는 명시적으로 프로토타입을 지정하여 새로운 객체를 만든다. 역시나 추상 연산 ordinaryObjectCreate 를 호출한다.
   - 첫 번째 매개변수는 프로토타입으로 지정할 객체이고, 두 번째 매개변수는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달한다. Object.defineProperties 메서드의 두 번째 인수와 동일하다. 생략 가능

   ![image.png](attachment:04c6a2fd-2c25-422f-a292-ac6a1fb8813d:image.png)

   ![image.png](attachment:0a0513d2-f2da-4885-bc27-a2d4b8f1c6c7:image.png)

   - 이처럼 Object.creat 메서드는 첫 번째 매개변수에 전달한 객체의 프로토타입 체인에 속하는 객체를 생성한다.
   - Object.create 의 장정
     - new 연산자 없이 객체 생성 가능
     - 프로토타입을 지정하면서 생성 가능
     - 객체 리터럴에 의해 생성된 객체도 상속 가능
   - Object.prototype 의 빌트인 메서드인 Object.prototype.hasOwnProperty 등은 모든 객체의 프로토타입 체인의 종점, 즉 Object.prototype의 메서드이므로 모든 객체가 상속받아 호출할 수 있다.
   - ❗그런데 ESLint 에서는 해당 빌트인 메서드를 객체가 직접 호출하는 것은 권장하지 않는데, 그 이유는 Object.create 를 통해 생성한 객체는 프로토타입 체인의 종점에 위치할 수도 있기 때문이다. ← 종점은 빌트인 메서드 사용 불가

   ![image.png](attachment:4083323e-68b2-4f40-bc3b-f987c9bf008b:image.png)

   - 그렇기 때문에 아래와 같이 사용하는 것을 권장한다.

   ![image.png](attachment:3260bdf3-53c2-4090-bbf3-eb9b12d7032d:image.png)

1. 객체 리터럴 내부에서 **proto** 에 의한 직접 상속

   - Object.create 메서드에 의한 직접 상속은 여러 장점이 있지만, 2번째 인자로 프로퍼티 정의가 번거롭다.
   - ES6 에서부터는 객체 리터럴 내부에서 **proto** 접근자 프로퍼티를 사용하여 직접 상속을 구현할 수 있다.

   ![image.png](attachment:34f4df7b-7b3c-4d50-9362-b2cd19120157:image.png)

# 19.12 정적 프로퍼티/메서드

---

- 정적 프로퍼티/ 메서드는 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티 메서드를 말한다. → **⚠️ 약간 자바의 static 메서드 같은 것인듯**

![image.png](attachment:4e44d567-9c58-4de4-9258-bc4136763497:image.png)

- Person 생성자 함수는 객체이므로 자신의 프로퍼티/메서드를 소유할 수 있다. Person 생성자 함수 객체가 소유한 것을 정적 프로퍼티/메서드라고 한다. ❗**인스턴스는 참조/호출할 수 없다.**

![image.png](attachment:df2d99ab-0fbe-44c2-b8aa-237d0f8d6396:image.png)

- **🌟 생성자 함수가 생성한 인스턴스는 프로토타입 체인에 속한 객체의 프로퍼티/메서드에는 접근할 수 있지만 정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 것이 아니므로 인스턴스는 접근할 수 없다.**
- 앞서 살펴본 Object.create 는 Object 생성자 함수의 정적 메서드이고 Object.prototype.hasOwnProperty 메서드는 Object.prototype 의 메서드이다. 따라서 Object.create 메서드는 인스턴스, 즉 Object 생성자 함수가 생성한 객체로 호출할 수 없다. 하지만 hasOwnProperty 메서드는 프로토타입 체인의 종점에 있어서 모든 객체가 호출할 수 있다.

```jsx
const obj = Object.create({ name: "Oh" });
console.log(obj.hasOwnProperty("name")); // false 호출 가능
```

- **❗만약 메서드 내에서 this 를 사용하지 않는다면 정적 메서드로 변경할 수 있다.**

![image.png](attachment:e059fc51-1a8e-4044-acc4-8a4f778ea903:image.png)

- MDN 문서를 참고하면 프로토타입 프로퍼티/메서드와 정적 프로퍼티/메서드를 구분하여 소개하고 있다. 표기법만으로 구분이 가능하다.
- 참고로 프로토타입 프로퍼티/메서드를 표기할 때 prototype 을 # 으로 표기하는 경우도 있으니 알아두자.

# 19.13 프로퍼티 존재 확인

---

1. **in 연산자**

   - 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.
   - key in object 형태로 씀.
   - in 연산자는 확인 객체 뿐 아니라 상속 받는 모든 프로토타입의 프로퍼티를 확인하므로 주의가 필요하다.
   - ES6 에서 도입된 Reflect.has 메서드를 대신 사용할 수도 있다. 동일하게 동작한다.

1. Object.prototype.hasOwnProperty 메서드
   - 객체의 특정 프로퍼티가 존재하는 지 확인
   - 이름에서 알 수 있듯이 인수로 전달받은 프로퍼티 키가 객체 고유의 프로퍼티인 경우에만 true 이며 이외에는 false 이다.

# 19.14 프로퍼티 열거

---

1. **for … in 문**

   - 객체의 모든 프로퍼티를 순회하며 열거하려면 for … in 문을 사용한다.

   ```jsx
   const person = {
     name: "Oh",
     address: "Seoul",
   };

   for (const key in person) {
     console.log(key, person[key]);
   }

   name Oh
   address Seoul
   ```

   - 프로퍼티 키가 할당됨.
   - for … in 문은 순회 대상 객체의 프로퍼티 뿐 아니라 상속받은 프로토타입의 프로퍼티까지 열거한다. 하지만 위 예제에서는 toString 과 같은 Object.prototype의 프로퍼티가 열거되지 않는다. → toString 은 열거할 수 없는 프로퍼티이기 때문이다. 즉, Object.prototype.toString 프로퍼티의 프로퍼티 어트리뷰트 [[Enumerable]] 값이 false 이기 때문이다.
   - 🌟 정확한 표현은 다음과 같다. **for … in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 열거가능 여부가 true 인 프로퍼티를 순회하며 열거한다.**

   ```jsx
   const person = {
     name: "Oh",
     address: "Seoul",
     __proto__: {
       age: 20,
     },
   };

   for (const key in person) {
     console.log(key, person[key]);
   }

   name Oh
   address Seoul
   age 20
   ```

   - 심벌인 프로퍼티는 열거하지 않는다.

   ```jsx
   const person = {
     name: "Oh",
     [Symbol()]: "Seoul",
     __proto__: {
       age: 20,
     },
   };

   for (const key in person) {
     console.log(key, person[key]);
   }

   name Oh
   age 20
   ```

   - 상속받은 프로퍼티는 제외하고 열거하려면 Object.prototype.hasOwnProperty 메서드로 객체 자신의 프로퍼티인지 확인해야 한다.

   ```jsx
   const person = {
     name: "Oh",
     address: "Seoul",
     __proto__: {
       age: 20,
     },
   };

   for (const key in person) {
     if (person.hasOwnProperty(key)) console.log(key, person[key]);
   }

   name Oh
   address Seoul
   ```

   - ❗🌟 for … in 문은 프로퍼티를 열거할 때 순서를 보장하지 않으므로 주의해야 할 필요가 있다. → 근데 사실 대부분의 모던 브라우저는 순서를 보장하고, 숫자인 프로퍼티 키에 대해서는 정렬을 실시한다. → 순서를 보장하지 않는다는 특징 때문에 순서 보장이 필요할 때는 Map 을 사용하는 것이 좋음. 애초에 동적으로 변하는 애는 map 이 좋음.

   ![image.png](attachment:9e52b8e1-f235-4edd-82b0-364d1597b036:image.png)

   - 배열에는 for … in 문 대신 일반적인 for 문 for … of 문 forEach 문 사용 권장.
   - 사실 배열도 객체이므로 상속 받은 프로퍼티가 포함될 수 있다.

   ```jsx
   const arr = [1, 2, 3];
   arr["3"] = 4; // 이건 추가됨.
   arr["a"] = 5;

   arr.forEach((v) => console.log(v)); // 1 2 3 4
   ```

   ![image.png](attachment:bbccab2e-c294-46d0-b73f-1c1c1a0f50e0:image.png)

1. **Object.keys / values / entries 메서드**

   - 🌟객체 자신의 고유 프로퍼티만 열거하기 위해서는 for … in 문 보다 **Object.keys / values / entries** 메서드를 사용하는 것이 좋다. → 자신의 열거 가능한 프로퍼티만 열거함.
   - Object.keys: 자신의 열거 가능한 프로퍼티 키를 배열로 반환
   - Object.values: 자신의 열거 가능한 프로퍼티 값을 배열로 반환
   - Object.entries: 자신의 열거 가능한 프로퍼티 키와 쌍의 배열을 배열로 반환

   ```jsx
   const person = {
     name: "Lee",
     address: "Seoul",
     __proto__: { age: 20 },
   };

   console.log(Object.keys(person));
   console.log(Object.values(person));
   console.log(Object.entries(person));

   ["name", "address"][("Lee", "Seoul")][
     (["name", "Lee"], ["address", "Seoul"])
   ];
   ```
