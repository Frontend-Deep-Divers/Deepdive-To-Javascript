- 객체 리터럴에 의한 객체 생성 방식이 가장 일반적이고 간단한 객체 생성 방식이다. 객체는 객체 리터럴 외에도 다양한 방법으로 생성할 수 있다.
- 이번 장에서는 생성자 함수를 사용하여 생성하는 방식을 살펴본다.
- 그리고 객체 리터럴을 통해 생성한 방식과 생성자 함수를 사용하여 생성하는 방식의 장단점을 살펴보자

# 17.1 Object 생성자 함수

---

- new 연산자 Object 생성자 함수를 호출하면 빈 객체를 생성한다. 이후 프로퍼티와 메서드를 추가하여 객체를 완성할 수 있다.
- 생성자 함수는 new 연산자와 함께 호출하여 객체를 생성하는 함수이다. 인스턴스 생성
- Object 생성자 함수 외에도 String, Number, Boolean, Function, Array, Date, RegExp(정규식), Promise 등의 빌트인 생성자 함수를 제공한다.

```jsx
const person = new Object();

person.name = "Lee";
person.sayHello = function () {
  console.log("안녕");
};

console.log(person);
person.sayHello();

const str = new String("Lee");
console.log(typeof str);
console.log(str);

const num = new Number(123);
console.log(typeof num);
console.log(num);

const bool = new Boolean(true);
console.log(typeof bool);
console.log(bool);

const func = new Function("x", "return x * x");
console.log(typeof func);
console.log(func);

const arr = new Array(1, 2, 3);
console.log(typeof arr);
console.log(arr);

const regExp = new RegExp(/ab+c/i);
console.log(typeof regExp);
console.log(regExp);

const date = new Date();
console.log(typeof date);
console.log(date);

// 결과

{ name: 'Lee', sayHello: [Function (anonymous)] }
안녕
object
[String: 'Lee']
object
[Number: 123]
object
[Boolean: true]
function
[Function: anonymous]
object
[ 1, 2, 3 ]
object
/ab+c/i
object
2025-03-20T10:49:37.333Z
```

- Object 생성자 함수로 객체를 생성하는 이유가 뭘까?

# 17.2 생성자 함수

---

1. **객체 리터럴에 의한 생성 방식의 문제점**

   - 직관적이고 간편하나, 단 하나의 객체만을 생성한다. 동일한 프로퍼티를 갖는 객체를 만들기 위해서는 매번 프로퍼티를 기술해야 하기에 비효율적이다.
   - 객체는 프로퍼티를 통해 객체 고유의 상태를 나타내고, 메서드를 통해 데이터인 프로퍼티를 참조하고 조작하는 행동을 표현한다. → ❗그렇기에 일반적으로 객체마다 프로퍼티 값은 다르지만 메서드 내용이 동일한 경우가 많다.
   - 구조가 동일한데 매번 기술하는 것은 비효율적이며, 많아지는 경우 큰 문제가 된다.

1. **생성자 함수에 의한 객체 생성 방식의 장점**

   - 객체를 생성하기 위한 탬플릿처럼 생성자 함수를 사용하여 프로퍼티 구조가 동일한 객체 여러 개를 간편하게 생성할 수 있다.

   ```jsx
   function Circle(r) {
     this.r = r;
     this.getDiameter = function () {
       return r * 2;
     };
   }

   const circle1 = new Circle(5);
   const circle2 = new Circle(10);

   console.log(circle1.getDiameter());
   console.log(circle2.getDiameter());
   ```

   ### 🔎 this 자기 참조 변수

   this 가 가르키는 값은 this 바인딩 호출 방식에 따라 동적으로 결정된다.

   - 일반 함수 호출: 전역 객체
   - 메서드로서의 호출 : 호출한 객체 (. 앞의 객체)
   - 생성자 함수로서 호출 : 생성자 함수가 생성할 인스턴스
   - 생성자 함수는 이름 그대로 객체를 생성하는 함수로, 일반 함수랑 동일한 방식으로 정의하고 new 연산자와 함께 호출하면 생성자 함수, ❗ 만약 new 연산자와 함께 호출하지 않으면 일반 함수로 동작한다.

   ```jsx
   const circle3 = Circle(15);
   console.log(circle3);
   console.log(r); //  this 가 전역 객체가 되어 r 이 15가 됨.
   ```

1. **생성자 함수의 인스턴스 생성 과정** 🌟

   - **생성자 함수의 역할은 인스턴스를 생성하는 것과 생성된 인스터스를 초기화 하는 것이다.**
   - ❗ 생성자 함수의 내부 코드를 살펴보면 인스턴스를 생성하고 반환하는 코드가 보이지 않는다!
   - 자바스크립트 엔진은 암묵적인 처리를 통해 인스턴스를 생성하고 반환한다. ( new 연산자와 함께 호출되는 경우 )

   1. 인스턴스 생성과 바인딩

      - 암묵적으로 빈 객체가 생성되고, 인스턴스는 this 에 바인딩 된다. this가 인스턴스를 가르키는 이유이다. **`바인딩`** 이란 식별자와 연결하는 과정이다. ex ) 변수 선언은 확보된 메모리 공간 주소를 변수 이름과 바인딩 하는 것

      ```jsx
      function Circle(r) {
        console.log(this); // Circle {}
        this.r = r;
        this.getDiameter = function () {
          return r * 2;
        };
      }

      const circle1 = new Circle(5);
      ```

   2. 인스턴스 초기화

      - this 에 바인딩되어 있는 인스턴스를 초기화한다. → 프로퍼티와 메서드를 추가한다. (개발자가 직접 하는 부분)

   3. 인스턴스 반환

      - 생성자 함수에서 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this 를 암묵적으로 반환한다.

      ```jsx
      function Circle(r) {
        this.r = r;
        this.getDiameter = function () {
          return r * 2;
        };
      }

      const circle1 = new Circle(5);
      console.log(circle1); // Circle { r: 5, getDiameter: [Function (anonymous)] }
      ```

      - 만약 다른 객체를 명시적으로 반환하면 명시한 객체가 반환된다.

      ```jsx
      function Circle(r) {
        this.r = r;
        this.getDiameter = function () {
          return r * 2;
        };

        return {};
      }

      const circle1 = new Circle(5);
      console.log(circle1); // {}
      ```

      - 명시적으로 원시 값을 반환하면 무시되고 this 가 반환된다.

      ```jsx
      function Circle(r) {
        this.r = r;
        this.getDiameter = function () {
          return r * 2;
        };

        return 1;
      }

      const circle1 = new Circle(5);
      console.log(circle1); // Circle { r: 5, getDiameter: [Function (anonymous)] }
      ```

1. **내부 메서드 [[Call]] 과 [[Construct]]**

   - 함수 선언문, 함수 표현식으로 정의한 함수는 일반적인 함수로서 호출할 수 있고 new 연산자와 함께 생성자 함수로서도 호출할 수도 있다
   - 또한 함수는 객체이므로 일급 객체와 동일하게 동작할 수 있다. ❗ 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드를 모두 가지고 있기 때문이다.
   - 함수는 객체이지만 일반 객체와는 다르다. ❗일반 객체는 호출할 수 없지만, 함수는 호출할 수 있다. → 즉, 함수 객체는 일반 객체가 가지고 있는 내부 슬롯과 내부 메서드는 물론, 함수로서 동작하기 위한 [[Call]], [[Construct]] 같은 내부 메서드를 추가로 가지고 있다.
   - 함수를 호출하면 Call 이 new 연산자와 함께 호출되면 Construct 가 호출된다.

   ```jsx
   function foo() {}
   foo(); // [[Call]] 호출
   new foo(); // [[Constructor]] 호출
   ```

   - 모든 함수 객체는 Call 을 가지고 있고 , Contruct 을 가지고 있는 함수 객체는 constructor 그렇지 않은 함수 객체를 non-contructor 이라고 한다.
   - 즉, 모든 함수 객체는 호출할 수 있지만, 모든 함수 객체를 생성자 함수로서 호출할 수 있는 건 아니다.

1. **constructor 와 non-constructor 구분**

   - **constructor : 함수 선언문, 함수 표현식, 클래스**
   - **non-contructor : 메서드 (단, ES6 축약형 메서드만), 화살표 함수**
   - non-constructor 는 new 연산자와 함께 호출되어도 Constructor 가 없기 때문에 에러가 발생한다.
   - ⚠️ 주의할 것은 생성자 함수로서 호출될 것을 기대하고 정의하지 않은 일반 함수에 new 연산자를 붙여 호출하면 생성자 함수처럼 동작할 수 있다.

1. **new 연산자**

   - **❗일반 함수와 생성자 함수에 형식적 차이는 없다.**
   - new 연산자와 함께 호출하면 생성자 함수로서 동작한다. Call 이 아니고 Construct 가 동작
   - 그래서 보통 생성자 함수는 첫 문자를 대문자 (파스칼 케이스) 로 명명하여 일반 함수와 구분을 한다.

1. **new.target**

   - 생성자 함수가 new 없이 호출되는 것을 방지하기 위해 ES6 에 도입된 방법이다.
   - 함수 내부에서 new.target 을 사용하면 함수 자신을 가르켜 만약 new 연산자 없이 호출되면 undefined 를 반환한다.

   ```jsx
   function Circle(r) {
     if (!new.target) {
       return new Circle(r);
     }
     this.r = r;
     this.getDiameter = function () {
       return r * 2;
     };
   }

   const circle1 = Circle(5); // new 가 없어도 정상적으로 생성된다.
   console.log(circle1); // Circle { r: 5, getDiameter: [Function (anonymous)] }
   ```

   - **스코프 세이프 생성자 패턴 - 만약 new.target 을 사용할 수 없을 때 (IE 라던가)**
     ```jsx
     function Circle(r) {
       if (!(this instanceof Circle)) {
         // this 가 무엇인지 확인
         return new Circle(r);
       }
       this.r = r;
       this.getDiameter = function () {
         return r * 2;
       };
     }

     const circle1 = Circle(5);
     console.log(circle1);
     ```
   - 🌟❗ 대부분의 빌트인 함수는 new 연산자와 함께 호출되었는지를 확인한 후 적절한 값을 반환한다.
   - Object 와 Function 함수는 new 연산자 없이 호출해도 new 연산자와 함께 호출했을 때와 동일하다.
   - 🌟 하지만 String,Number, Boolean 생성자 함수는 new 연산자가 있을 때는 각 객체를 생성하지만 없을 때는 문자열, 숫자, 불리언 값을 반환한다. → 🌟 이를 통해 타입을 변환하기도 한다.

   ```jsx
   let obj = new Object();
   console.log(obj);
   obj = Object();
   console.log(obj);

   let func = new Function();
   console.log(func);
   func = Function();
   console.log(func);

   const str = String("하하");
   console.log(str, typeof str);

   const num = Number(123);
   console.log(num, typeof num);

   const bool = Boolean(false);
   console.log(bool, typeof bool);

   // 결과

   {}
   {}
   [Function: anonymous]
   [Function: anonymous]
   하하 string
   123 number
   false boolean
   ```
