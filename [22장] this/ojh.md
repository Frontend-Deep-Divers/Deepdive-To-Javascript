# 22.1 this 키워드

---

- **🌟 자기 참조 변수 this**
- 객체는 프로퍼티와 메서드가 존재한다. 메서드는 자신이 속한 객체의 상태, 프로퍼티를 참조하고 변경할 수 있어야 한다. 참조하려면 **자신이 속한 객체를 가르키는 식별자를 참조할 수 있어야 한다.**
- 객체 리터럴 방식으로 생성한 객체의 경우 메서드 내부에서 메서드 자신이 속한 객체를 가르키는 식별자를 재귀적으로 참조할 수 있다.

![image.png](attachment:3c389769-5fbe-4f61-9b9d-f50cc374e922:image.png)

- getDiameter 메서드 내에서 메서드 자신이 속한 객체를 가르키는 식별자 circle을 참조하고 있다. 이 참조 표현식이 평가되는 시점은 getDiameter 메서드가 호출되어 함수 몸체가 실행되는 시점이다.
- 객체는 circle 변수에 할당되기 직전에 평가되기 때문에, 호출하는 시점에는 이미 circle 식별자에 객체가 할당되어 식별자를 참조할 수 있다. → 하지만 자기 자신이 속한 객체를 재귀적으로 참조하는 방식은 일반적이지도 바람직하지도 않다.

![image.png](attachment:3a08a2ef-0704-43a1-a389-da4a4be54f4d:image.png)

- 생성자 함수 내부에서는 프로퍼티, 메서드를 추가하기 위해서는 자신이 생성할 인스턴스를 참조할 수 있어야 한다. 하지만 생성자 함수를 사용한 객체 생성 방식은 먼저 생성자 함수 정의 후, new 연산자와 함께 생성자 함수를 호출해야 한다. → 생성자 함수를 정의하는 시점에는 아직 인스턴스를 생성하기 이전이므로 생성자 함수가 생성할 인스턴스를 가르키는 식별자를 알 수 없다. → 따라서 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가르키는 특수한 식별자가 필요하다 → 이것이 this
- 🌟 this 는 자신이 속한 객체 또는 자신이 생성할 인스턴스를 가르키는 자기 참조 변수다. this 를 통해 자신이 속한 객체 또는 자신이 생성할 인스턴스의 프로퍼티나 메서드를 참조할 수 있다.
- this 는 자바스크립트 엔진에 의해 암묵적을 생성된다. 함수를 호출하면 arguments 객체와 this 가 암묵적으로 함수 내부에 전달된다.
- **🌟❗단, this 가 가르키는 값, 즉 this 바인딩은 함수 호출 방식에 의해 동적으로 결정된다.**
- this 바인딩: this 가 가르킬 객체를 바인딩하는 것.

![image.png](attachment:6b26fd5b-c5c8-402b-8da9-0bf5897890d7:image.png)

- 객체 리터털 메서드 내부에서의 this 는 메서드를 호출한 객체, 즉 circle 을 가르킨다.

![image.png](attachment:9bb4d00d-d5d4-4d46-a03a-4e92532d8ff2:image.png)

- 생성자 함수 내부의 this 는 생성자 함수가 생성할 인스턴스를 가리킨다. 이처럼 this 는 상황에 따라 가리키는 대상이 다르다.
- 자바나 C++ 같은 클래스 기반 언어에서는 this 는 언제나 클래스가 생성하는 인스턴스를 가리킨다. ❗ **하지만 자바스크립트의 this 는 함수가 호출되는 방식에 따라 this 에 바인딩이 동적으로 결정된다.**
- this 는 전역이든 어디서든 참조할 수 있다.
  ![image.png](attachment:8ebf5433-5e61-4476-a45b-9e8ef4438244:image.png)
  - 전역, 일반 함수 내부 : 전역 객체
  - 객체의 메서드 내부: 호출한 객체
  - 생성자 함수 내부: 생성자 함수가 생성할 인스턴스
- strict mode 가 적용된 일반 함수의 this 는 undefined 가 바인딩된다. 필요가 없기 때문.

# 22.2 함수 호출 방식과 this 바인딩

---

- 🌟 **this 바인딩은 함수 호출 방식에 따라 동적으로 결정된다.**
- 렉시컬 스코프와 this 바인딩은 결정 시기가 다름
  - 렉시컬 스코프: 함수 객체가 생성되는 시점에 상위 스코프 결정
  - this 바인딩: 함수 호출 시점
- ⚠️ 동일한 함수도 다양한 방식으로 호출할 수 있다.
- 함수 호출 방식
  - 일반 함수 호출
  - 메서드 호출
  - 생성자 함수 호출
  - Function.prototype.apply/call/bind 메서드에 의한 간접 호출
  ![image.png](attachment:6dcf16eb-e336-45c5-94a4-6111e96ee6d6:image.png)
  ![image.png](attachment:51302380-d6ff-4e91-a8f1-abbcd66a7bfe:image.png)

1. **일반 함수 호출**

   - 기본적으로 this 에는 전역 객체가 바인딩된다.
   - use strict 을 적용하면 일반 함수 내부의 this 에는 undefined 가 바인딩 됨.
   - **❗메서드 내에서 정의한 중첩 함수도 일반 함수로 호출되면, 중첩 함수 내부의 this 에는 전역 객체가 바인딩된다. (주의하자)**

   ![image.png](attachment:339b2722-3dcc-4c5c-8e11-f4cb7b875edc:image.png)

   - 콜백 함수가 일반 함수로 호출되면 콜백 함수 내부의 this에도 전역 객체가 바인딩된다. 어떤 함수라도 일반 함수로 호출되면 this 에 전역 객체가 바인딩된다. → ❗ 이것도 주의하자!!

   ![image.png](attachment:38d87922-65e5-4ba6-87ef-22113c04575d:image.png)

   - setTimeout 함수는 두 번째 인수로 전달한 시간만큼 대기한 다음, 첫 번째 인수로 전달한 콜백 함수를 호출하는 타이머 함수다. 위 함수는 0.1 초 후 콜백함수 호출한다.
   - ✍️ 이처럼 일반 함수로 호출된 모든 함수 (중첩, 콜백 함수 포함) 내부의 this 에는 전역 개체가 바인딩 된다.
   - ❗그런데 외부 함수인 메서드와 내부 함수인 중첩 함수 또는 콜백 함수의 this 가 일치하지 않는 것은 헬퍼 함수로 동작하기 어렵게 만든다. → 🤔 이를 일치시키는 방법들을 알아보자!
   - **새로운 변수에 this 를 할당에 호출하는 것**

   ![image.png](attachment:4e245000-6847-445b-8b9e-f40017df54fb:image.png)

   - **bind, call, apply 메서드를 사용한다. (this 명시적 바인딩)**

   ![image.png](attachment:179398f1-4b37-4f5f-bb86-cb8715ff279b:image.png)

   - ❗화살표 함수를 사용해 this 바인딩 ← 화살표 함수는 상위 스코프의 this 를 가르킴.

   ![image.png](attachment:ea550a1e-16f9-433c-a620-a783085d8259:image.png)

1. **메서드 호출**

   - 메서드 내부의 this 에는 메서드를 호출한 객체, 🌟 즉, 메서드를 호출할 때, 메서드 이름 앞의 마침표 ( . ) 연산자 앞에 기술한 객체가 바인딩된다.
   - **❗❗❗주의할 점은 메서드를 소유한 객체가 아닌 메서드를 호출한 객체에 바인딩되는 것이다.**

   ![image.png](attachment:2672b9e5-6ba2-4af1-8a7e-0dabce9a2e15:image.png)

   - 🌟 위 예제의 getName 메서드는 person 객체의 메서드로 정의되어 있다. **❗메서드는 프로퍼티에 바인딩된 함수이다. →** 즉, person 객체의 getName 프로퍼티가 가르키는 함수 객체는 person 객체에 포함된 것이 아니라 독립적으로 존재하는 별도의 객체다. getName 프로퍼티가 함수 객체를 가르키고 있을 뿐이다.

   ```jsx
   const person = {
     name: "Lee",
     getName() {
       return this.name;
     },
   };

   const anotherPerson = {
     name: "kim",
   };

   anotherPerson.getName = person.getName;

   // 호출 객체는 anotherPerson 객체임.
   console.log(anotherPerson.getName()); // Kim
   const getName = person.getName;
   // 일반 함수로 호출
   console.log(getName()); // undefined 브라우저에서는 ''
   ```

   - 🌟 따라서 getName 프로퍼티가 가르키는 함수 객체를 다른 객체의 프로퍼티에 할당하거나 일반 변수에도 할당하고 호출할 수 있다.

   ![image.png](attachment:95985135-1eb6-4ec0-bc0f-0fc7cb237a42:image.png)

   - 프로토타입 메서드 내부에서 사용된 this도 일반 메서드와 마찬가지로 해당 메서드를 호출한 객체에 바인딩된다. **⚠️ 아래 예제는 좀 주의 깊게 살펴보자**

   ```jsx
   function Person(name) {
     this.name = name;
   }

   Person.prototype.getName = function () {
     return this.name;
   };

   const me = new Person("Lee");
   // 메서드 호출 객체는 me
   console.log(me.getName()); // Lee

   Person.prototype.name = "Kim";
   // 메서드 호출 객체는 Person.prototype
   console.log(Person.prototype.getName()); // Kim
   ```

   ![image.png](attachment:8fe22d6b-a56c-43d1-86e1-75b8aecc1de0:image.png)

1. **생성자 함수 호출**

   - **생성자 함수 내부의 this 에는 생성자 함수가 생성할 인스턴스가 바인딩된다.**

   ![image.png](attachment:de0e0788-3956-4a83-8ea8-41fc722b2323:image.png)

   - new 연산자 없으면 당연히 일반 함수로 동작

1. **Function.prototype.apply/call/bind 메서드에 의한 간접 호출**

   - apply, call, bind 메서드는 Function.prototype 의 메서드다. 즉, 이들 메서드는 모든 함수가 상속받아 사용할 수 있다.

   ![image.png](attachment:aaed6d18-2101-499f-bd92-3350ca8a0fed:image.png)

   - apply, call 메서드는 this 를 사용할 객체와 인수 리스트를 인수로 전달받아 호출한다.
   - **🌟 apply, call 두 메서드는 모두 본질적인 기능은 함수를 호출하는 것이다.**
   - apply 와 call 의 차이점은 인수를 전달하는 방식
     - apply는 인수를 배열로 묶어 전달 [ a,b,c ]
     - call은 인수를 쉼표로 구분한 리스트 형식으로 전달 a,b,c
   - apply 와 call 의 대표적인 용도는 arguments 객체와 같은 유사 배열 객체에 배열 메서드를 사용하는 경우이다. → Array.prototype.slice 와 같은 경우에 call 이나 apply 로 메서드 사용 가능

   ```jsx
   function getThisBinding() {
     console.log(arguments);
     return this;
   }
   const thisArg = { a: 1 };

   console.log(getThisBinding.apply(thisArg, [1, 2, 3]));
   console.log(getThisBinding.call(thisArg, 1, 2, 3));

   // 결과
   [Arguments] { '0': 1, '1': 2, '2': 3 }
   { a: 1 }
   [Arguments] { '0': 1, '1': 2, '2': 3 }
   { a: 1 }
   ```

   - 🌟 **bind 함수는 this 바인딩이 교체된 새로운 함수를 만들어 반환한다.**

   ```jsx
   function getThisBinding() {
     console.log(arguments);
     return this;
   }
   const thisArg = { a: 1 };

   console.log(getThisBinding.bind(thisArg)(1, 2, 3));
   // 명시적으로 호출 해주어야 함.
   ```

   - bind 메서드는 메서드의 this 와 중첩 함수나 콜백 함수의 this 가 불일치하는 문제를 해결하기 위해 유용하게 사용된다.

   ```jsx
   const person = {
     name: "Lee",
     foo(callback) {
       setTimeout(callback.bind(this), 100); // Hello Lee
     },
   };

   person.foo(function () {
     console.log(`Hello ${this.name}`);
   });
   ```

   ![image.png](attachment:4617a653-fdda-41d2-9f00-534e58e2f5e5:image.png)
