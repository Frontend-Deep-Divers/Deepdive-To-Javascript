# 26.1 함수의 구분

---

- ES6 이전까지 함수는 별다른 구분 없이 다양한 목적으로 사용되었다.
- 일반적인 함수, 생성자 함수, 메서드로서도 호출될 수 있었다.
- 이는 편리해보이긴 하나, 실수를 유발시킬 수 있으며 성능 면에서도 손해다.

![image.png](attachment:9368ef7b-f205-49c0-b279-6d81ff444921:image.png)

- ES6 이전의 함수는 사용 목적에 따라 명확히 구분되지 않는다. **ES6 이전의 모든 함수는 일반 함수로도 생성자 함수로도 호출될 수 있었다. →** 즉, ES6 이전의 모든 함수는 callable 이면서 constructor이다.
- [[Call]: 호출할 수 있는 함수 객체, callable
- [[Construct]]: 인스턴스를 생성할 수 있는 함수 객체 contructor
- ❗ 주의할 점은 ES6 에서 일반적으로 메서드라고 부른 객체에 바인딩된 함수도 callable 이며, constructor 이다. → 그래서 메서드 축약형이 나왔구나…

![image.png](attachment:29504b92-9102-4090-a348-69bbce39e637:image.png)

- 객체에 바인딩된 함수를 생성자 함수로 호출하는 경우가 흔치는 않겠지만 문법상 가능하다는 것은 문제가 있다. → **🌟 이건 성능 면에서도 문제가 있다. →** ❓ **왜일까? 왜냐하면 객체에 바인딩된 함수가 constructor라는 것은 객체에 바인딩된 함수가 prototype 프로퍼티를 가지며, 프로토타입객체도 생성한다는 것이기 때문이다.**
- 이는 보조함수 역할을 하는 콜백 함수도 마찬가지다. 콜백함수도 constructor 이기 때문에 불필요한 프로토타입 객체를 생성한다.

![image.png](attachment:48e74be8-2b17-46fa-8b24-bbe75d558021:image.png)

- 이런 것은 실수를 유발할 가능성도 성능에도 좋지 않아.
- ES6에서는 함수를 사용 목적에 따라 세 가지 종류로 명확히 구분했다.

![image.png](attachment:c1382e01-11c7-444b-88b9-dea0005234dc:image.png)

- 일반 함수는 ES6 이전의 함수와 차이가 없다.
- 하지만 ES6 의 메서드와 화살표 함수는 다르다.

# 26.2 메서드

---

- ES6 이전에는 명확한 정의가 없고, 객체에 바인딩된 함수를 일컫는 의미로 사용되었다. → **ES6 사양에서 메서드는 메서드 축약 표현으로 정의된 함수만을 말한다.**

```tsx
const obj = {
	x: 1,
	foo(){return this.x}, // 메서드
	bar: function {return this.x} // 일반 함수
};
```

- 메서드는 인스턴스를 생성할 수 없는 non-constructor 이기 때문에, 생성자 함수로 호출할 수 없다. → 당연히 prototype 프로퍼티도 프로토타입도 없다.

![image.png](attachment:a34d4b66-66a8-4244-9fce-f8d786e8c9b9:image.png)

![image.png](attachment:b197781d-bb02-4119-837b-efad53ce0064:image.png)

- 참고로 표준 빌트인 객체가 제공하는 프로토타입 메서드와 정적 메서드는 모두 non-constructor다.

![image.png](attachment:b8c60d21-ab97-4747-aa60-544cd08df1b5:image.png)

- ES6 메서드는 자신을 바인딩한 객체를 가르키는 내부 슬롯 [[HomeObject]] 를 가진다. super 참조는 내부 슬롯을 사용하여 수퍼클래스의 메서드를 참조하므로 내부 슬롯 [[HomeObject]] 를 가지는 ES6 메서드는 super 키워드를 사용할 수 있다.
- 당연히 ES6 메서드가 아니면 super 키워드를 사용할 수 없다. 내부 슬롯이 없으니까.
- ✏️**ES6 메서드는 본연의 기능(super)를 추가하고 의미적으로 맞지 않는 기능(constructor)을 제거했다. → 그러니까 메서드를 정의할 때 익명 함수 표현식을 할당하는 것은 사용하지 않는 것이 좋다.**

# 26.3 화살표 함수

---

- ⇒ 을 통해 정의하는 간략한 함수 정의 방식
- 표현만 간략한 것이 아니라 내부 동작도 기존의 함수보다 간략하다.
- ❗특히 화살표 함수는 콜백 함수 내부에서 this 가 전역 객체를 가르키는 문제를 해결하기 위한 대안으로 유용하다.

1. **화살표 함수 정의**

   **[함수 정의]**

   - 화살표 함수는 함수 선언문으로 정의할 수 없고, 함수 표현식으로 정의해야 한다. 호출 방식은 기존 함수와 동일하다.

   **[매개변수 선언]**

   - ( ) 안에 선언한다.
   - 한 개인 경우 소괄호를 생략할 수 있다.

   ```tsx
   const arrow = x = > { ... };
   ```

   - 없는 경우 () 는 써야 함.

   ```tsx
   const arrow = () => {...};
   ```

   **[함수 몸체 정의]**

   - 함수 몸체가 하나의 문으로 구성된다면 함수 몸체를 감싸는 중괄호 {} 를 생략할 수 있다. 이 때 함수 몸체 내부의 문이 값으로 평가될 수 있는 표현식인 문이라면 암묵저으로 반환될 수 있다.

   ```tsx
   const power = (x) => x ** 2;
   power(2); // 4
   ```

   - 함수 몸체가 하나의 문으로 구성된다 하더라고 몸체의 문이 표현식이 아닌 문이라면 중괄호를 생략할 수 없다.

   ```tsx
   const arrow = () => const x = 1; // 문법 에러

   const arrow = () => {const x = 1;};
   ```

   - ❗객체 리터럴 문을 반환할 때는 () 소괄호로 감싸 주어야 한다.
   - 감싸지 않으면 몸체를 감싸는 {} 로 잘못 해석한다.

   ```tsx
   const create = (id, content) => ({ id, content });
   ```

   - 함수 몸체가 여러 개의 문이면 당연히 {} 생략할 수 없다. 이때는 명시적으로 반환 해주어야 한다.

   ```tsx
   const sum = (a, b) => {
     const result = a + b;
     return result;
   };
   ```

   - 화살표 함수도 즉시 실행 함수로 사용할 수 있다.

   ![image.png](attachment:58b91286-4dc2-4cff-b477-1f4492665373:image.png)

   - 화살표 함수도 일급 객체이므로 map, filter, reduce 같은 고차 함수에 인수로 전달할 수 있다. 일반적인 표현식보다 표현이 간결하고 좋다.

   ![image.png](attachment:924ea9a7-8f27-4e6c-b1f3-7f2771e6a8fd:image.png)

   - 또한 표현말고도 내부 구현도 간단한데 한 번 차이를 알아보자.

1. **화살표 함수와 일반 함수의 차이**

   - 차이는 다음과 같다.
     1. **화살표 함수는 인스턴스를 생성할 수 없는 non-constructor다.**

        ![image.png](attachment:40d19e9e-acd6-404f-96c8-c02a9b699974:image.png)

     2. **중복된 매개변수 이름을 선언할 수 없다.**

        ![image.png](attachment:2fa647cc-b196-49b9-83f6-6159034c821b:image.png)

     3. **화살표 함수는 함수 자체의 this. arguments, super, new.target 바인딩을 갖지 않는다.**

        ![image.png](attachment:80d6428d-fa36-4e76-a9f3-9dbaa0573c35:image.png)

1. **this**

   - 일반 함수와 구분되는 가장 큰 특징이 this 이다. 화살표 함수는 다른 함수의 인수로 전달되어 콜백 함수로 전달되는 경우가 많다.
   - **❗🌟 화살표 함수는 일반 함수와 다르게 동작하는데, 이는 callback 함수 내부의 this 문제를 해결하기 위해 의도적으로 설계된 것이다.**
   - 알다시피 this 바인딩은 함수 호출 방식에 의해서 결정된다. 호출된 방식에 의해 동적으로 결정된다.
   - 주의해야 할 것은 일반 함수로서 호출되는 콜백 함수의 경우다. 고차 함수의 인수로 전달되어 고차 함수 내부에서 호출되는 콜백 함수도 중첩 함수라고 할 수 있다.

   ![image.png](attachment:3f00228d-7ca0-4d54-bf32-1ff118e73947:image.png)

   - 위 예제에서 콜백 함수의 내부에서 this 가 undefined 를 가르켜 에러가 발생한다.
   - **⚠️ 이는 Array.prototype.map 메서드가 콜백 함수를 일반 함수로서 호출하기 때문이다.**
   - **일반 함수로서 호출되는 모든 함수 내부의 this 는 전역 객체를 가르킨다. 하지만 클래스 내부의 모든 코드 내부에는 strict mode 가 적용되기 때문에 map 콜백 함수에도 적용되어 undefined 가 바인딩 된다.**
   - 해결하기 위해 ES6 이전에는 아래와 같은 방법을 사용했다.
     1. **add 메서드를 호출한 prefixer 객체를 가르키는 this 를 일단 회피시킬 후에 콜백 함수 내부에서 사용한다.**

        ![image.png](attachment:be76b87a-e39d-46f5-92a6-df21b8522546:image.png)

     2. **map 의 두 번째 인수로 prefixer 객체를 가르키는 this 를 전달한다.**

        - 두 번째 인수로 콜백 함수 내에서 사용할 this 를 전달할 수 있다.

        ![image.png](attachment:743659d2-bb67-4d75-bd2b-2c7f66e765ca:image.png)

     3. **bind 메서드를 사용하여 this 바인딩하기**

        ![image.png](attachment:c3684a19-8350-482f-95f0-1751166a099a:image.png)
   - ES6 에서는 화살표 함수를 사용하여 해당 문제를 해결할 수 있다.

   ![image.png](attachment:49f13792-c79d-4253-ad2a-0d953be04f4f:image.png)

   - **화살표 함수는 자체의 this 바인딩을 가지지 않아, 내부에서 this 를 참조하면 상위 스코프의 this 를 그대로 참조한다. 이를 Lexical this 라고 한다. →** 마치 렉시컬 스코프와 같이 정의된 위치에 의해 결정된다는 의미이다! ← 오 이해하기 쉬운데
   - 화살표 함수는 this 를 참조하면 일반적인 식별자처럼 스코프체인을 통해 상위 스코프에서 this 를 탐색한다.

   ![image.png](attachment:181144b1-27d0-455a-97ba-95dd35e69a5e:image.png)

   - 만약 화살표 함수가 중첩되어 있다면, 상위 화살표 함수에도 this 바인딩이 없으므로 스코프 체인 상에서 가장 가까운 상위 함수 중 화살표 함수가 아닌 함수의 this 를 참조한다.

   ![image.png](attachment:0aef2199-8921-40e9-ab51-234c092e70af:image.png)

   - 만약 전역 함수라면 this 는 전역 객체
   - 프로퍼티에 할당한 화살표 함수도 스코프 체인 상 가장 가까운 상위 함수 중의 this 를 참조

   ![image.png](attachment:3864d511-b5db-435e-9319-cd78250beaf8:image.png)

   - **❗❗❗ 화살표 함수는 자체의 this 바인딩이 없기 때문에 call , apply, this 를 통한 명시적 바인딩으로 교체할 수 없다.**

   ![image.png](attachment:38b0ed0d-7665-41c7-854d-75c22173f9ce:image.png)

   - **메서드를 화살표 함수로 정의하는 것은 피해야 한다.**

   ![image.png](attachment:dee0e50f-6056-4df8-b518-7c0172fcb5b1:image.png)

   - 위와 같은 상황 때문에 메서드를 화살표 함수로 정의하는 것은 권장하지 않는다.
   - ES6 축약 표현으로 정의한 메서드 사용하는 것이 좋다.

   ![image.png](attachment:fc1e91a2-562b-4061-a8ad-17c684947627:image.png)

   - 프로토타입 객체의 프로퍼티에 화살표 함수를 할당하는 경우도 동일한 문제가 발생한다.

   ![image.png](attachment:281b639c-ea9e-4fc4-b28e-311da2f1918d:image.png)

   - 다만, 메서드 축약 표현은 사용 불가하니 일반 함수를 할당한다.

   ![image.png](attachment:c8356d1a-6b30-48cc-921e-8e78dc818ffd:image.png)

   - 만약, ES6 메서드를 동적으로 추가하고 싶다면 객체 리터럴을 바인딩하고 프로토타입의 contructor 프로퍼티와 생성자 함수 간의 연결을 재설정 한다. → 굳이 이렇게까지?

   ![image.png](attachment:7d1b6a16-b125-4926-a395-14f095fec3c7:image.png)

   - ❓그냥 클래스 쓸거면 ~~화살표 함수 쓸 생각하지 마라, 필드에 정의하는 것 constructor 에 정의하는 것이 다른 것은데,~~ → **그냥 ES6 메서드 써라**

   ![image.png](attachment:cfcec578-5c21-4ef8-bf28-f660e7dc8e39:image.png)

1. **super**

   - super 역시 this 와 같이 상위 스코프의 super 를 참조한다.

   ![image.png](attachment:c2a62deb-3a6a-4798-93f1-106ede5f638d:image.png)

   - super 는 내부 슬롯 [[HomeObject]] 를 가지는 ES6 메서드 내에서만 사용할 수 있는 키워드다. sayHi 클래스 필드에 할당한 화살표 함수는 ES6 메서드는 아니지만 함수 자체의 super 바인딩을 갖지 않으므로 super 를 참조해도 에러가 발생하지 않고 constructor 의 super 바인딩을 참조한다.

1. **arguments**

   - 화살표 함수는 arguments 를 가지지 않고, this 와 마찬가지로 상위 스코프의 arguments 를 참조한다.

   ![image.png](attachment:26bb63c1-b7a7-4b71-869e-c027577799f6:image.png)

   - 근데 상위 arguments 의 참조를 해봤자, 화살표 함수 자신에게 전달된 인수 목록을 확인할 수 있는게 아니라 그닥 도움되지 않는다.
   - 따라서 rest 파라미터 사용해라.

# 26.4 Rest 파라미터

---

1. **기본 문법**

   - rest 파라미터는 함수에 전달된 인수들의 목록을 배열로 전달받는다.
   - **파라미터와 함께 사용할 수 있고, 반드시 마지막 파라미터여야 하며, 단 하나만 선언할 수 있다.**
   - Rest 함수 정의 시 선언한 매개변수 개수를 나타내는 length 프로퍼티에 영향을 주지 않는다.

   ```tsx
   function foo(a, b, ...rest) {
     console.log(a, b); //1 2
     console.log(rest); // [3,4,5]
   }
   foo(1, 2, 3, 4, 5);
   console.log(foo.length); // 2
   ```

2. **Rest 파라미터와 arguments 객체**
   - ES5 까지는 가변 인자 함수의 경우 arguments 객체를 활용했는데, 이는 유사 배열 객체였기 때문에 배열로 변환해줘야 하는 번거로움 등이 있었다.
   - 하지만 Rest 파라미터를 사용하면 이런 번거로움을 피할 수 있다.
   - 함수와 ES6 메서드는 Rest 파라미터와 arguments 객체를 모두 사용할 수 있지만, 화살표 함수에서는 arguments 객체가 없으니까, 반드시 Rest 파라미터 사용해야 한다.

# 26.5 매개변수 기본값

---

- 함수를 호출할 때 매개변수 개수만큼 인수를 전달하는 것이 바람직하지만 그렇지 않은 경우에도 에러가 발생하지 않는다. → 엔진이 따로 체크 안하거든

![image.png](attachment:30d111b6-e830-446b-bd97-371da7bd786c:image.png)

- 그래서 방어 코드가 필요하다.

![image.png](attachment:a9966d8e-a314-4c54-acd6-8f1bb1d5ff77:image.png)

- ES6 에서 도입된 매개변수 기본값을 사용하면 함수 내에서 수행하던 인수 체크 및 초기화를 간단하게 할 수 있다.

![image.png](attachment:b795ee45-5c7f-4ceb-8974-0cf195afec2c:image.png)

- 전달하지 않거나 undefined 인 경우에만 유효하다.

![image.png](attachment:3e448a4b-ec9e-4bf6-a776-dfa317d40d18:image.png)

- rest 파라미터는 기본값 지정 불가능

![image.png](attachment:cac9c7d7-dcf9-46db-a491-595da647b0cb:image.png)

- 매개변수 기본값은 함수 정의 시 선언한 매개변수 개수를 나타내는 length 나 arguments 에 아무런 영향을 주지 않는다.
