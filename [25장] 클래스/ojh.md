# 25.1 클래스는 프로토타입의 문법적 설탕인가?

---

- 자바스크립트는 프로토타입 기반 객체지향 언어입니다. 비록 다른 객체지향 언어와 차이점은 있지만
- ES5 까지 클래스 없이도 프로토타입을 통해 객체지향 언어의 상속을 구현할 수 있다.

![image.png](attachment:a6460d54-adbb-470e-9771-200c907d87e9:image.png)

- 하지만 클래스 기반 언어에 익숙한 프로그래머들에게는 장벽같은 존재 → 그래서 ES6 에 클래스가 도입됨. → 기존 클래스 기반 객체지향 프로그래밍 언어와 매우 흡사하다.
- **❗ 하지만 클래스도 결국 함수이면 클래스 기반 패턴처럼 사용할 수 있게 만들어준 문법적 설탕이다. → 단 둘 다 프로토타입 기반 인스턴스를 생성하지만, 정확히 동일하게 동작하지는 않는다. ← 클래스는 생성자 함수보다 엄격! 하며 생성자 함수에서 제공하지 않는 기능도 제공한다.**
- 유사하지만 차이가 있다.
  ![image.png](attachment:93d4606a-f57a-4db3-9092-4481765ad873:image.png)
- 생성자 함수와 클래스는 프로토타입 기반의 객체지향을 구현했다는 점에서 매우 유사하다. 하지만 더욱 격고하다. → 특히 extends , super 를 통한 상속
- 문법적 설탕보다는 **새로운 객체 생성 메커니즘**

# 25.2 클래스 정의

---

- class 키워드 → 일반적으로 파스칼 케이스 사용안해도 에러는 아님
- 일반적이지 않지만 표현식으로도 정의는 가능하다. 무형 클래스 가능. → 가능한 근거는 클래스도 일급 객체이기 때문이다.
  - 무명의 리터럴로 생성 가능. 런타임에 생성 가능
  - 변수나 자료구조에 저장 가능
  - 함수의 매개변수에 전달 가능
  - 함수의 반환값으로 사용 가능
- ❗**클래스 몸체에서 정의할 수 있는 메서드는 생성자, 프로토타입 메서드, 정적 메서드 3가지가 있다.**
- 클래스와 생성자 함수의 정의 방식 비교.

![image.png](attachment:4042acec-fba6-48df-8f95-b78e89d31f2f:image.png)

# 25.3 클래스 호이스팅

---

- 클래스는 함수로 평가된다.
- 생성자 함수로서 런타임 이전에 평가되어 함수 객체를 생성하고 프로토 타입도 생성한다.
- **❗단, 클래스 정의 이전에 참조할 수 없다.**
- 마치 호이스팅이 발생하지 않은 것처럼 보이지만 발생한다.
- let, const 키워드 처럼 호이스팅은 되지만 TDZ 일시적 사각지대에 빠지기 때문에 호이스팅 발생하지 않는 것처럼 동작한다.

![image.png](attachment:4e4e100d-692a-4153-8020-37fc404f326b:image.png)

- 모든 식별자는 호이스팅 된다. 선언문은 런타임 이전에 실행되기 때문

# 25.4 인스턴스 생성

- new 연산자와 함께 인스턴스를 생성한다.
- 반드시 함께 호출되어야 한다.

![image.png](attachment:31690141-361f-4d06-a85e-5f3cb0d080e1:image.png)

- 그렇지 않으면 에러 발생한다.
- 함수와 동일하게 클래스 표현식을 사용하면 클래스 이름이 아니라 식별자로 생성해야 한다.

![image.png](attachment:9307d093-066b-4312-9e17-e19bec2831d9:image.png)

# 25.5 메서드

---

- 메서드만 선언할 수 있다. 생성자, 프로토타입 메서드, 정적 메서드
- 클래스에 대한 새로운 제안 사항.
  - 원래는 반드시 contructor 내부에서 정의해야 하지만, 직접 정의할 수 있는 표준 사항이 제안되었다. → 이거 써본거 같음.

1. **constructor**

   - 인스턴스를 생성하고 초기화하기 위한 특수 메서드다.

   ![image.png](attachment:728558f7-317f-410a-8e1f-1863ff8c3515:image.png)

   - 한번 개발자 도구로 봐보자.

   ![image.png](attachment:e2ca52a1-f053-4bdb-a7bb-d19e47d9f2a8:image.png)

   - 클래스는 평가되어 함수 객체가 된다. 함수 고유의 프로퍼티를 모두 가지고 있다. 자신의 스코프 체인을 구성한다.
   - contructor 프로퍼티는 클래스 자신을 가르킨다. → 클래스가 인스턴스를 생성하는 생성자 함수라는 뜻

   ![image.png](attachment:9f3d9957-4c94-41b9-9c86-9eb80c8f891f:image.png)

   - 인스턴스 생성

   ![image.png](attachment:eed91b2a-c5b0-4bc8-b08b-b889cdddf09c:image.png)

   - ❓흥미로운 것은 클래스가 평가되어 생성된 함수 객체나 클래스가 생성한 인스턴스 어디에도 contructor 메서드가 보이지 않는다는 것이다. → 이는 클래스 몸체에 정의한 constructor 가 단순한 메서드가 아니라는 뜻.
   - constructor 는 메서드로 해석되는 것이 아니라 클래스가 평가되어 생성한 함수 객체 코드의 일부가 된다. 다시 말해 클래스 정의가 평가되면 constructor 기술된 동작을 하는 함수 객체가 생성된다.
   - ❗클래스의 constructor 메서드와 프로토타입의 constructor 프로퍼티는 이름이 같아 혼동하기 쉽지만 직접적인 연관은 없다. 프로토타입의 constructor 프로퍼티는 모든 프로토타입이 가지고, 생성자 함수를 가르킨다.
   - 생성자 함수와 유사하지만 몇 가지 차이가 있다.
   - ❗**클래스 내에서 최대 1개**

   ![image.png](attachment:3fa7ab76-f7aa-4e54-bbdd-a2d41e0907f2:image.png)

   - ❗**생략 가능**

   ![image.png](attachment:60648091-d218-4ae9-a6db-4cdc78f8b40f:image.png)

   - 고정값으로 인스턴스 초기화 하던, 인수로 인스턴스 초기화 하던 초기화 하려면 생략하면 안된다.
   - **생성자 함수와 동일하게 명시적으로 this 를 반환하면 인스턴스가 반환되지 못한다.**
     - 객체로 반환시, 명시적 반환
     - 원시값인 경우에는 무시하고 this 반환
   - ❗**constructor 내부에서 return 문은 반드시 생략해야 한다.**

1. **프로토타입 메서드**

   - 생성자 함수를 사용한 인스턴스 생성은 명시적으로 프로토 타입 메서드를 추가해줘야 한다.

   ![image.png](attachment:2391f19b-1401-419e-ab41-df0e6de8d658:image.png)

   - 클래스는 명시적으로 prototype 프로퍼티에 추가하지 않아도 기본적으로 프로토타입 메서드가 된다.

   ![image.png](attachment:a24076a9-4c66-42fc-96ee-7dc3010b52e2:image.png)

   - 생성자 함수와 마찬가지로 클래스가 생성한 인스턴스는 프로토타입 체인의 일원이 된다.

   ![image.png](attachment:4edbdaff-8dac-45a7-a29d-613266d2c117:image.png)

   ![image.png](attachment:7a5988f8-c445-4112-bce3-0fefcb0bdc2d:image.png)

   - 그렇기에 인스턴스는 프로토타입 메서드를 상속받아 사용할 수 있다.
   - 프로토타입 체인은 기존의 모든 객체 생성 방식뿐만 아니라 클래스에 의해 생성된 인스턴스에도 동일하게 적용된다.
   - 결국 클래스는 생성자 함수와 마찬가지로 프로토타입 기반의 객체 생성 방식이다.

1. **정적 메서드**

   - 정적 메서드란 인스턴스를 생성하지 않아도 호출할 수 있는 메서드이다.

   ![image.png](attachment:64e0f5cf-4548-4ab0-b823-535a4c41d61d:image.png)

   - class 에서는 static 키워드 붙이면 됨.

   ![image.png](attachment:7991092c-cc1c-44ef-84bd-8b590909b207:image.png)

   - 다음과 같은 프로토타입 체인

   ![image.png](attachment:93eff88a-011c-4350-a11c-0cf7fb13a793:image.png)

   - 클래스는 클래스 정의가 평가되는 시점에 함수 객체가 생성되므로, ❗ 정적 메서드는 클래스 정의 이후에 인스턴스를 생성하지 않아도 호출할 수 있다. ⇒ 클래스로 호출한다.
   - 당연히 인스턴스로는 호출 못한다. 프로토타입 체인상에 없지 않느냐.

   ![image.png](attachment:ff2a3184-b370-47b9-a257-d855d7735b09:image.png)

   ![image.png](attachment:162f9751-4cc9-41a3-aa1f-69b21f4f2c18:image.png)

1. **정적 메서드와 프로토타입 메서드의 차이**

   - 정적 메서드와 프로토 타입 메서드의 차이
     1. 자신이 속한 프로토타입 체인이 다르다.
     2. 정적 메서드는 클래스로, 프로토타입 메서드는 인스턴스로 호출
     3. 정적 메서드는 인스턴스 참조 불가능, 프로토타입 메서드는 참조 가능.

   ![image.png](attachment:ff91e845-71ba-4a52-90fb-93991b77779e:image.png)

   - 위 메서드는 인스턴스 참조 안한다. 인스턴스 참조해야 하면 아래처럼.

   ![image.png](attachment:2c30973c-035d-44c1-9a34-07ab5bac3d9a:image.png)

   - 정적 메서드는 클래스를 호출해야 하기에 this 는 클래스이고, 프로토타입은 this 가 인스턴스다. → 즉, 내부의 this 바인딩이 다르다.
   - 🌟 물론 메서드 내부에서 this 를 사용하지 않더라도 프로토타입 메서드로 정의할 수 있다. 하지만 반드시 인스턴스를 생성하고 호출해야 하므로 → this 사용 안하면 정적 메서드로 정의하자.
   - 표준 빌트인 객체의 정적 메서드는 유틸리티 함수다.

   ![image.png](attachment:666b7c4b-f5c9-4c25-b811-6efddc969a11:image.png)

   - **이처럼 클래스 또는 생성자 함수를 하나의 네임스페이스로 사용하여 정적 메서드를 모아 놓으면 이름 충돌 가능성을 줄여주고 관련 함수들을 구조화하는 효과가 있다.**
   - 정적 메서드는 애플리케이션 전역에서 사용할 유틸리티 함수를 전역 함수로 정의 않고 메서드로 구조화 할 때 유리한다.

   ![image.png](attachment:d76700c1-6968-4dea-91f3-b5fa1384e665:image.png)

1. **클래스에서 정의한 메서드 특징**
   1. function 키워드를 생략한 축약 표현을 사용한다.
   2. 객체 리터럴과 다르게 클래스 생성할 때는 콤마 필요 X
   3. 암묵저으로 strict mode 로 실행된다.
   4. for … in 등으로 열거할 수 없다. [[Enumerable]] 이 false 이다.
   5. 내부 메서드 [[Construct]] 를 가지지 않는 non-constructor 이다. new 와 함께 호출할 수 없다.

# 25.6 클래스의 인스턴스 생성 과정

---

- new 연산자와 함께 클래스를 호출하면 [[Construct]] 가 호출됨.
- 생성자 함수랑 유사한 과정을 거쳐 인스턴스 생성
  1. **인스턴스 생성과 this 바인딩**
  2. **인스턴스 초기화**
  3. **인스턴스 반환**

# 25.7 프로퍼티

---

1. **인스턴스 프로퍼티**

   - 인스턴스 프로퍼티는 contructor 내부에서 정의해야 한다.

   ![image.png](attachment:bf0478a6-50db-4d68-a746-fee6b1134372:image.png)

   - 접근 제한자 지원하지 않고 언제나 public 하다.

2. **접근자 프로퍼티**

   - 접근자 프로퍼티란 자체적으로 값을 가지지 않고 다른 프로퍼티의 값을 읽거나 저장할 때 사용하는 접근자 함수로 구성된 프로퍼티이다.

   ![image.png](attachment:906374c5-05c2-4d98-ad1f-0f584270e3c7:image.png)

   - 위 예제는 객체 리터럴

   ![image.png](attachment:615e6b13-b9f1-4bbe-967d-bd1873e0c71e:image.png)

   - getter 함수로 setter 함수로 구성되어 있음.
   - get, set 키워드를 사용해 정의한다.
   - ❗클래스의 메서드는 기본적으로 프로토타입의 메서드가 된다. 그렇기 때문에 접근자 프로퍼티 또한 인스턴스의 프로퍼티가 아니라 프로토타입의 프로퍼티가 된다.

   ![image.png](attachment:34eacdbe-3953-4fc3-97c0-cdf5bb8ca2bf:image.png)

3. **클래스 필드 정의 제안**

   - 클래스 필드란 객체자향 언어에서 클래스가 생성할 인스턴스의 프로퍼티를 가르키는 용어이다.
   - 자바스크립트는 클래스에 변수를 선언하고 초기화하려면 반드시 contructor 내부에서 this에 프로퍼티를 추가해야 한다.
   - 몸체에는 메서드만 선언할 수 있다. 그래서 클래스 몸체에 자바와 유사하게 클래스 필드를 선언하면 문법 에러가 발생한다.

   ![image.png](attachment:f33cc768-def0-4893-b311-23bf54e78d6f:image.png)

   - ❗**하지만 위 예제를 브라우저 (크롬 72 이상) 또는 node.js 12 이상에서 실행하면 문법에러가 발생하지 않고 정상 동작한다!**
   - 자바스크릅에서도 인스턴스 프로퍼티를 마치 클래스 기반 객체지향 언어의 필드처럼 정의할 수 있는 표준 사양인 Class field declarations 가 TC39 프로세스의 stage3 에 제안되어 있다.
   - **TC39 : ECMA-262 사양 관리를 담당하는 위원회가 바로 TC39 이다.**
   - **TC39 프로세스: 새로운 표준 사양을 추가하기 위해 공식적으로 명문화해 놓은 과정을 말한다.**
     ![image.png](attachment:4f440869-8493-4bfb-b3ef-80e273272e10:image.png)
   - 21년 당시에는 표준 사양으로 승급되지는 않았지만 → `stage 4`를 통과해서 **ECMAScript 2022 (ES13)**에 포함됐고, **대부분의 최신 브라우저와 JS 엔진에서 정식 지원**하고 있어. 따라서 아래와 같이 몸체 내부에 정의할 수 있다.

   ![image.png](attachment:ddaafd3b-d25a-48f5-828e-51ccb6e0a919:image.png)

   - 클래스 몸체에서 필드를 정의하는 경우에는 this에 클래스 필드를 바인딩해서는 안된다.

   ![image.png](attachment:da29eeaf-8f46-4c48-8a55-fffe6787ce4a:image.png)

   - 또한 클래스 필드를 참조하는 경우에는 꼭 this 를 참조해야 한다.

   ![image.png](attachment:99761b23-e455-4216-aab3-60edb694e52d:image.png)

   - 필드에 초기값을 할당하지 않으면 undefined 이다.

   ![image.png](attachment:3dcc6516-2dce-4905-aeb2-59d107774e44:image.png)

   - 외부의 초기값으로 클래스 필드를 초기화할 필요가 있다면 constructor 에서 클래스 필드를 초기화 해야 한다.

   ![image.png](attachment:1a3d8c4d-8770-4c53-ad35-7b0d1c7a33bf:image.png)

   - ❗ 어차피 클래스 필드를 초기화할 필요가 있다면, 굳이 constructor 외부에서 클래스 필드를 정의할 필요가 없다.
   - 함수는 일급 객체 이므로 클래스 필드에 할당할 수 있다. 따라서 클래스 필드를 통해 메서드를 정의할 수 있다.

   ![image.png](attachment:348ae088-291f-42f8-90e2-8b2c90347200:image.png)

   - 만약 이렇게 필드에 함수를 할당하는 경우 프로토타입 메서드가 아닌 인스턴스 메서드가 된다. 그렇기에 클래스 필드에 함수를 할당하는 것은 원장하지 않는다.

   ![image.png](attachment:998dadc1-972b-45d8-ad02-11816290fd7b:image.png)

   - 이 예시는 한 번 이야기 해 볼 법 한듯

4. **private 필드 정의 제안**

   - 자바스크립트는 캡슐화를 완전하게 지원하지 않는다. → 즉, 언제나 public

   ![image.png](attachment:a512de17-a0e8-4265-8f44-a43555b99ff9:image.png)

   - 둘 다 그대로 노출…
   - 다행히 새로운 사항이 생겼다. 바로 private 필드의 선두에 #를 붙여준다. ← ❗참조할 때도 붙여줘야 함.

   ![image.png](attachment:e9143a60-4dff-46bc-ad06-8fa85de57b43:image.png)

   - ❗❗ 놀랍게도 타입스크립트는 접근 제한자를 모두 지원하며, public, private, protected 를 모두 지원한다.
   - public 은 어디서나 참조 가능하지만 private 필드는 클래스 내부에서만 참조 가능하다.
   - **❗🌟 외부에서 private 필드에 직접 접근할 수는 없지만, 접근자 프로퍼티를 통해 간접적으로 접근하는 방법은 유효하다.**

   ```tsx
   class Person {
     #name = "";

     constructor(name) {
       this.#name = name;
     }

     get name() {
       return this.#name.trim();
     }
   }

   const me = new Person(" Lee ");
   console.log(me.name); // Lee
   ```

   - 🌟 private 필드는 반!드!시! 몸체에 정의해야 한다. private 필드를 직접 contructor 에 정의하면 에러가 발생한다.

   ![image.png](attachment:ce41e8b0-274a-4dc5-ae6c-1a0566b9f769:image.png)

5. **static 필드 정의 제안**

   - static 키워드를 사용하여 정적 메서드를 정의할 수 있다. 하지만 static 키워드로 정적 필드는 정의할 수 없었는데, → static public 필드, static private 필드, static private 메서드를 정의할 수 있는 새로운 사항이 제안되었고 이미 구현됨.

   ```tsx
   class MyMath {
     static PI = 22 / 7;
     static #num = 10;

     static increament() {
       return ++MyMath.#num;
     }
   }

   console.log(MyMath.PI); // 3.14 ...
   console.log(MyMath.increament()); // 11
   ```

# 25.8 상속에 의한 클래스 확장

---

1. **클래스 상속과 생성자 함수 상속**

   - 🌟 **상속에 의한 클래스 확장은 지금까지 살펴본 프로토타입 기반의 상속과는 다른 개념이다.**
   - 프로토타입 기반 상속은 프로토타입 체인을 통해 다른 객체의 자산을 상속받는 개념이지만, **상속에 의한 클래스 확장은 기존 클래스를 상속받아 새로운 클래스를 확장하여 정의**하는 것이다.

   ![image.png](attachment:24987f41-d991-4838-a5b8-a77dc1bc709b:image.png)

   - 클래스와 생성자 함수는 인스턴스를 생성할 수 있다는 점에서 유사하지만, 생성자 함수는 상속이 없다.

   ![image.png](attachment:7fb6e0a4-dde9-4a7e-a951-b9abe2012e04:image.png)

   ```tsx
   class Animal {
     constructor(age, weight) {
       this.age = age;
       this.weight = weight;
     }
     eat() {
       return "eat";
     }
     move() {
       return "move";
     }
   }

   class Bird extends Animal {
     fly() {
       return "fly";
     }
   }

   const bird = new Bird(1, 5);
   console.log(bird instanceof Animal);
   console.log(bird instanceof Bird);

   console.log(bird.eat());
   console.log(bird.move());
   console.log(bird.fly());
   ```

   - 상속에 의해 확장된 클래스 Bird 를 통해 생성된 인스턴스의 프로토타입은 다음과 같다.

   ![image.png](attachment:2a3c67ee-577e-47e7-8a69-dbd10c19d9bf:image.png)

   - extends 키워드가 제공된다. 클래스 확장은 간편하고 직관적이나, 생성자 함수는 다른 생성자 함수를 확장하는 문법이 제공되지 않는다.
   - 예전에는 의사 클래스 상속 패턴을 사용하여 상속에 의한 클래스 확장을 흉내 내기도 했다. 다만 클래스의 등장으로 패턴은 필요하지 않다. 참고만 하길 바란다.

   ![image.png](attachment:0346597d-bac9-4267-8ecd-c081769b534d:image.png)

1. **extends 키워드**

   - 상속을 통해 클래스를 확장하려면 extends 키워드를 사용하여 상속받을 클래스를 정의한다.

   ![image.png](attachment:665705c9-bf13-43d5-a098-d8a89becf61f:image.png)

   - 수퍼 - 서브, 부모 - 자식, 베이스 - 파생 클래스
   - extends 키워드의 역할은 수퍼 클래스와 서브 클래스 간의 상속 관계를 설정하는 것이다. 클래스도 프로토타입을 통해 상속 관계를 구현한다.

   ![image.png](attachment:8e68bdbe-7c70-4172-a1fc-0cfb8d3d7b16:image.png)

   - 수퍼클래스와 서브클래스는 인스턴스 프로토타입 체인 뿐 아니라 클래스 간의 프로토타입 체인도 생성한다. 이를 통해 프로토타입 메서드, 정적 메서드 모두 상속 가능하다.

1. **동적 상속**

   - 생성자 함수를 상속받아 클래스를 확장할 수 있다. 단 extends 앞에 반드시 class 가 와야 한다.

   ![image.png](attachment:a445a87d-e03e-47b7-8671-3d5c19701b56:image.png)

   - extends 키워드 다음에는 클래스뿐 아니라 [[Construct]] 내부 메서드를 갖는 함수 객체로 평가될 수 있는 모든 표현식을 사용할 수 있다. 이를 통해 동적으로 상속받을 대상을 결정할 수 있다.

   ![image.png](attachment:6847d593-c8cd-41b8-8620-619a64035249:image.png)

1. **서브클래스의 constructor**

   - 클래스에서 constructor 를 생략하면 암묵적으로 비어있는 constructor 가 정의된다.
   - 서브클래스에서 constructor 를 생략하면 클래스에 다음과 같은 constructor 가 암묵적으로 정의된다. args 는 new 연산자와 함께 클래스를 호출할 때 전달한 인수의 리스트다.

   ![image.png](attachment:38148fad-baec-40a7-a97e-f3052cc9d759:image.png)

   - super 는 수퍼 클래스의 constructor 를 호출하여 인스턴스를 생성한다.

   ![image.png](attachment:b663d7d4-b575-408d-af2b-ee02427923b1:image.png)

1. **super 키워드**

   - super 키워드는 함수처럼 호출할 수도 this 와 같이 식별자처럼 참조할 수 있는 특수한 키워드이다.
     - **super 를 호출하면 수퍼클래스의 constructor 를 호출한다.**
     - **super 를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.**

   **[super 호출]**

   - super를 호출하면 수퍼클래스의 constructor 를 호출한다.
   - 서브클래스의 constructor 를 생략하면 서브 클래스를 호출하면서 전달한 인수가 모두 암묵적으로 정의된 super 호출을 통해 수퍼클래스의 constructor 에 전달된다.

   ![image.png](attachment:434d26c2-f434-4cad-b75b-91b6f0f7a0b7:image.png)

   - 만약 수퍼클래스에서 추가한 프로퍼티와 서브클래스에서 추가한 프로퍼티를 갖는 인스턴스를 생성한다면 서브클래스의 constructor 를 생략할 수 없다.

   ![image.png](attachment:4160550b-ee35-410d-bf55-f0d396c09068:image.png)

   - ⚠️ super 연산자 호출할 때 주의할 사항.
     1. **서브클래스에서 constructor 를 생략하지 않는 경우, 반드시 super 를 호출해야 한다.**

        ![image.png](attachment:07d46672-5e99-46a3-87a7-20597a31832f:image.png)

     2. **서브클래스의 constructor 에서 super 를 호출하기 전까지는 this 를 참조할 수 없다.**

        ![image.png](attachment:1ded2b3e-0a6f-4f1e-9331-b2f0daca47ea:image.png)

     3. **super는 반드시 서브클래스의 constructor에서만 호출한다. 서브클래스가 아닌 클래스나 함수에서 super 를 호출하면 에러가 발생한다.**

        ![image.png](attachment:62f21388-07c5-4911-8f8d-34ad78aa9f3c:image.png)

   **[super 참조]**

   - **메서드 내에서 super 를 참조하면 수퍼클래스의 메서드를 호출할 수 있다.**
     1. **서브클래스의 프로토타입 메서드 내에서 super.sayHi는 수퍼클래스의 프로토타입 메서드 sayHi 를 가르킨다.**

        - 여기서 말이 되게 장황하게 나오지만 → 🌟 **결론적으로 super 가 동작하기 위해서는 자신의 프로토타입을 찾을 수 있어야 하는데, 이를 위해 메서드의 내부 슬롯 [[HomeObject]] 가 자신을 바인딩하는 객체를 가르킨다.**

        ![image.png](attachment:994d40d4-a7d8-422d-9825-d0011d2bca8c:image.png)

        - **⚠️ 주의할 점은 ES6 메서드 축약 표현으로 정의된 함수만이 [[HomeObject]] 를 가진다는 것이다.**

        ![image.png](attachment:22688244-3fe6-4130-8cdd-b7f4befb5e92:image.png)

        - super 참조는 클래스의 전유물은 아니고, 객체 리터럴에서도 super 를 참조할 수 있다. ❗ 다만, ES6 메서드 축약 표현으로 정의된 함수만 가능

        ![image.png](attachment:6a1026d8-5012-49f1-9f4a-ccf6e8de2d3e:image.png)

     2. **서브클래스의 정적 메서드 내에서 super.sayHi 는 수퍼 클래스의 정적 메서드 sayHi 를 가르킨다.**

        - 당연한 이야기, 프로토타입 체인을 생각하면 됨.

        ![image.png](attachment:dfa83a1f-fd0f-459d-8334-cc0957063f3e:image.png)

1. **상속 클래스의 인스턴스 생성 과정**

   - 상속 관계의 두 클래스가 어떻게 협력하여 인스턴스를 생성하는지 보자.

   ![image.png](attachment:6f215683-25ab-437f-b4f0-55080b8785c6:image.png)

   - 아래는 프로토타입 체이닝

   ![image.png](attachment:93b620b6-3ee0-48f0-9b38-2f9059decd4d:image.png)

   - 아래의 과정을 거쳐 인스턴스가 생성된다.

   **1️⃣ 서브클래스의 super 호출**

   - 🌟 **자바스크립트 엔진은 수퍼 클래스와 서브클래스를 구분하기 위해 base 와 derived 를 값으로 갖는 내부 슬롯 [[ConstructorKind]] 를 가진다.**
   - 상속 안받으면 base , 받으면 derived
   - 다른 클래스를 상속받지 않는 클래스는 new 연산자와 함께 호출되었을 때 암묵적으로 빈 객체, 즉 인스턴스를 생성하고 이를 this 에 바인딩한다.
   - **❗하지만 서브클래스는 자신이 직접 인스턴스를 생성하지 않고, 수퍼클래스에게 인스턴스 생성을 위임한다. 이것이 바로 서브클래스의 constructor 에서 반드시 super 를 호출해야 하는 이유다.**
   - 서브클래스가 new 연산자와 함께 호출되면 constructor 내부의 super 키워드가 함수처럼 호출된다. 그렇게 수퍼클래스의 constructor 가 호출된다.

   **2️⃣ 수퍼클래스의 인스턴스 생성과 this 바인딩**

   - 수퍼클래스의 constructor 내부 코드가 실행되기 이전에 암묵적으로 빈 객체를 생성한다. 이 빈 객체가 클래스가 생성할 인스턴스로 this 에 바인딩된다.

   ![image.png](attachment:650d9f4b-1265-4d4a-aaf5-63775a586c34:image.png)

   - 이때 인스턴스는 수퍼클래스가 생성했지만, new 연산자와 함께 호출된 클래스는 서브클래스이기 때문에 new.target 은 서브클래스를 가르킨다. 따라서 🌟 **인스턴스는 서브클래스가 생성한 것으로 처리된다. → 그래서 서브클래스의 prototype 프로퍼티가 가르키는 객체가 프로토타입이 되는 것이다.**

   ![image.png](attachment:0391d3bd-e782-4343-88d2-39350eb0df38:image.png)

   **3️⃣ 수퍼클래스의 인스턴스 초기화**

   - this 에 바인딩되어 있는 인스턴스를 초기화한다. 즉, 프로퍼티를 추가하고 constructor 에 인수로 전달받은 초기값으로 초기화함.

   ![image.png](attachment:6857ed6d-0a3e-4750-8a91-ff050a500688:image.png)

   **4️⃣ 서브클래스 constructor로의 복귀와 this 바인딩**

   - super 의 호출이 끝나고 종료되어 제어 흐름이 서브클래스 constructor 로 돌아온다. **이때 반환한 인스턴스가 this 에 바인딩되어, 서브클래스는 별도의 인스턴스를 생성하지 않고, 그대로 this 에 바인딩하여 사용한다.**

   ![image.png](attachment:06d9cb24-ec0d-4fb4-8e8a-c3105214a0c1:image.png)

   - 🌟 **그래서 super 를 호출하지 않으면 인스턴스 생성도 this 바인딩도 안된다. super 호출 전에 this 를 참조할 수 없는 이유이다.**

   **5️⃣ 서브클래스의 인스턴스 초기화**

   - super 호출 이후, 서브클래스의 constructor 에 기술되어 있는 인스턴스 초기화가 실행된다.

   **6️⃣ 인스턴스 반환**

   - 클래스의 모든 처리가 끝나면 완성된 인스턴스가 바인딩된 this 가 암묵적으로 반환된다.

1. **표준 빌트인 생성자 함수 확장**

   - 표준 빌트인 객체도 확장할 수 있다. extends 로

   ![image.png](attachment:f7b55ccd-9de8-4e16-8f78-a2c5c1a98e3b:image.png)

   - Array 생성자 함수를 상속받아 확장한 MyArray 클래스가 생성한 인스턴스는 Array.prototype과 MyArray.prototype 의 메서드를 사용할 수 있다.
   - ❗ **이때 주의해야 할 점은 map,filter 와 같은 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환한다는 점이다.**
   - 만약 새로운 배열을 반환하는 메서드가 MyArray 클래스의 인스턴스를 반환하지 않고 Array 의 인스턴스를 반환하면 MyArray 클래스의 메서드와 메서드 체이닝이 불가능하다.
   - 만약 Array 가 생성한 인스턴스를 반환하게 만들고 싶다면 Symbol.species 를 사용하여 접근자 프로퍼티를 추가한다.

   ![image.png](attachment:a4890e8e-c73f-4cb2-a4bb-9b0aeff7b77d:image.png)

   - 이걸 쓸 일이 있을까 싶긴 하네…
