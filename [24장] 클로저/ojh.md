- 함수를 입급 객체로 취급하는 함수형 프로그래밍 언어에서 사용되는 중요한 특성임.
- **MDN: 클로저는 함수와 그 함수가 선언된 렉시컬 환경과의 조합이다.**

![image.png](attachment:4233a569-f269-4a32-9c03-c5313b538054:image.png)

- 위와 같은 이유는 렉시컬 스코프를 따르는 언어이기 때문

# 24.1 렉시컬 스코프

---

- 🌟**함수 정의한 위치에 따라 상위 스코프 결정 → 렉시컬 스코프 ( 정적 스코프 )**
- 외부 렉시컬 환경에 대한 참조에 저장할 참조값은 상위 스코프에 대한 참조 → 함수 정의가 평가되는 시점에 정의된 환경에 의해 결정됨.

# 24.2 함수 객체의 내부 슬롯 [[Environment]]

---

- **함수는 자신의 내부슬롯 [[Environment]] 에 자신이 정의된 환경, 즉 상위 스코프의 참조를 저장한다.**
- 이 말은 즉 평가되는 순간 자신이 정의된 환경에 의해 내부슬롯에 저장하는데, 이는 현재 실행 중인 렉시컬 환경을 가르킨다. → \***\*🌟 **왜냐하면 함수가 정의되어 객체를 생성하는 시점은 상위 함수가 평가 또는 실행되고 있는 시점이기 때문이다.\*\*
- 🌟**핵심 문구**
  ![image.png](attachment:e33d669b-b1b8-4a7e-968d-9e0a7e3112ce:image.png)

![image.png](attachment:ee06e930-7554-44b0-872f-823a18b58cfe:image.png)

![image.png](attachment:640b2360-b953-434e-ad53-e3247b10ea23:image.png)

- 위 예제의 실행 컨텍스트이다.
- foo , bar 모두 전역 함수에서 함수 선언문으로 정의되어, 전역 코드가 평가되는 시점에 함수 객체가 생성되고 window 전역 객체의 메서드가 된다. 이 때 생성된 [[Environment]] 에는 함수 정의가 쳥가된 시점, 즉 전역 코드 평가 시점에 실행 중인 실행 컨텍스트의 렉시컬 환경인 전역 렉시컬 환경의 참조가 저장된다.
- **❗🌟 이후 함수 코드가 평가되고 외부 렉시컬 환경에 대한 참조를 결정할 때 함수 객체의 내부 슬롯에 저장된 렉시컬 환경의 참조가 할당된다.**

# 24.3 클로저와 렉시컬 환경

---

![image.png](attachment:e78c5a1c-d97e-4103-b2d2-c2b481147d7b:image.png)

- 위 코드에서 outer 함수의 실행 컨텍스트가 제거되었으므로 outer 함수의 지역 변수도 유효하지 않게 되어 접근 방법이 없어 보이지만, x 가 부활이라도 한 듯 동작하고 있다.
- **🌟 이처럼 외부 함수보다 중첩 함수가 더 오래 유지되는 경우 중첩 함수는 이미 생명 주기가 종료한 외부 함수의 변수를 참조할 수 있는 것 → 이러한 중첩 함수를 클로저라고 한다.**

![image.png](attachment:ec50f3fa-8f5b-4096-95f8-240a09858116:image.png)

- outer 함수가 호출되면 outer 함수의 렉시컬 환경이 생성되고 외부 렉시컬 환경에 대한 참조에 내부 슬롯에 저장된 값을 할당한다. 이후 inner 함수가 평가된다. (표현식이기 때문!!!)

![image.png](attachment:1750585c-5f5a-4a3a-8c47-b6cec05c46b2:image.png)

- outer 함수는 종료하고 inner 함수를 반환하며 스택에서 제거된다. 하지만 outer 함수의 렉시컬 환경은 소멸하지 않는다. ← **inner 함수 내부슬롯에 의해 참조되고 있고 inner 함수는 전역 함수 innerFunc 에 의해 참조되고 있으므로 가비지 컬렉션 대상이 되지 않기 때문이다.**

![image.png](attachment:55d3b508-f613-4b77-a9f0-5d41c0284ecb:image.png)

- 이제 inner 함수를 호출하면 실행 컨텍스트가 실행되고 외부 렉시컬 환경에는 [[Environment]] 내부 슬롯에 저장된 참조값이 할당된다.

![image.png](attachment:90ff86a5-c38c-4778-aa87-a3323e1b1ba1:image.png)

- inner 가 outer 보다 더 오래 생존했다. 외부 함수보다 더 오래 생존한 중첩함수는 외부 함수의 생존 여부와 상관없이 자신이 정의된 위치에 의해 결정된 상위 스코프를 기억한다. → 🌟 **상위 스코프의 식별자를 참조할 수 있고 식별자의 값을 변경할 수도 있다.**
- 자바스크립트의 모든 함수는 상위 스코프를 기억하므로 이론적으로 클로저이지만, 일반적으로 모든 함수를 클로저라고 하지는 않는다.

![image.png](attachment:ce7002d7-3979-43a8-bad3-4c29b98f2620:image.png)

- 위 예제에서 bar 는 외부 함수 foo 보다 더 오래 유지되지만 상위 스코프의 어떤 식별자도 참조하지 않는다. → **❗🌟 이처럼 상위 스코프의 어떤 식별자도 참조하지 않는 경우 대부분의 모던 브라우저는 최적화를 통해 다음과 같이 상위 스코프를 기억하지 않는다.** → 이는 메모리 낭비이기 때문. 따라서 bar 는 클로저 아님.

![image.png](attachment:909d7c2c-acb4-4682-b068-491536f24a10:image.png)

- 위 예제는 중첩 함수 bar 가 상위 스코프의 식별자를 참조하고 있어 클로저지만 foo 에 의해 bar 가 반환되지 않는다.
- bar 는 클로저였지만 외부 함수보다 일찍 소멸되어 생명 주기가 종료된 외부 함수의 식별자를 참조할 수 있다는 클로저의 본질에 부합하지 않는다. 따라서 bar 는 일반적으로 클로저 아님.

![image.png](attachment:5531f849-2ef8-46dd-be7e-42388c5e0c29:image.png)

- 위 예제는 상위 스코프 식별자도 참조하고 더 오래 살아남기 때문에 bar 는 클로저다.
- **클로저는 중첩 함수가 상위 스코프의 식별자를 참조하고 있고 중첩 함수가 외부 함수보다 더 오래 유지되는 경우에 한정하는 것이 일반적이다.**
- 다만 x, y 중 x 만 참조하고 있는데, **🌟 대부분의 모던 브라우저는 최적화를 통해 클로저가 참조하는 식별자만을 기억한다.**
- **클로저에 의해 참조되는 상위 스코프의 변수를 자유변수라고 부른다.**
- 자유 변수에 묶여있는 함수?
- 클로저는 자바스크립트의 강력한 기능으로 적극 활용해야 한다.

# 24.4 클로저의 활용

---

- 🌟🌟🌟**클로저는 상태를 안전하게 변경하고 유지하기 위해 사용한다.**
- **의도치 않게 변경되지 않도록 상태를 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하기 위해 사용**한다. (react useState)

![image.png](attachment:4988ba4b-a9f0-457c-9f6d-b12e9d201292:image.png)

- 위 코드는 동작은 하나 2가지 전제가 지켜져야 함.
  1. num 은 함수가 호출되기 전까지 변경되지 않고 유지되어야 한다.
  2. increase 함수만이 num 을 변경할 수 있어야 한다.
- 하지만 누구나 접근하고 변경할 수 있다. → 의도치 않게 변경 되고 오류 발생할 확률 올라감.
- 한 번 그럼 increase 함수만 num 변수를 참조하고 변경하게 할 수 있도록 지역변수로 바꿔보자.

![image.png](attachment:83bc2bbb-3825-4f7e-b282-9183ddf96c9a:image.png)

- 의도치 않은 변경은 방지했지만 계속 0으로 초기화되어 이전 상태를 유지하지 못한다.
- 이번에는 클로저를 사용해보자.

```tsx
const increase = (function () {
  let num = 0;

  return function () {
    return ++num;
  };
})();

console.log(increase()); // 1
console.log(increase()); // 2
console.log(increase()); // 3
```

- 즉시 실행 함수가 호출되고 즉시 실행 함수가 반환한 increase 변수에 할당된다.
- 즉시 실행 함수는 한 번만 실행되므로 increase 가 호출될 때마다 num 변수가 재차 초기화 될 일 없을 것이며, num 변수는 외부에서 접근할 수 없는 private 변수이고 의도치 않게 변경될 걱정도 없으니 더 안정적인 프로그래밍이 가능하다.
- 🌟 **이처럼 클로저는 상태가 의도치 않게 변경되지 않도록 안전하게 은닉하고 특정 함수에게만 상태 변경을 허용하여 상태를 안전하게 변경하고 유지하기 위해 사용한다.**
- 좀 더 발전한 예시

```tsx
const counter = (function () {
  let num = 0;

  return {
    increase() {
      return ++num;
    },
    decrease() {
      return num > 0 ? --num : 0;
    },
  };
})();

console.log(counter.increase()); // 1
console.log(counter.increase()); // 2
console.log(counter.increase()); // 3
console.log(counter.decrease()); // 2
console.log(counter.decrease()); // 1
```

- 위 예제에서 객체가 생성된다. 이 때 객체의 메서드도 함수 객체로 생성된다.
- 위 예제의 increase, decrease 메서드의 상위 스코프는 메서드가 평가되는 시점에 실행 중인 실행 컨텍스트인 즉시 실행 함수의 렉시컬 환경이다. 따라서 즉시 실행 함수의 식별자를 참조할 수 있다.
- 이를 생성자 함수로 표현하면 다음과 같다.

![image.png](attachment:3fa8a3a1-518d-4f5b-a470-d273841b93f6:image.png)

![image.png](attachment:296e6bd6-32e0-4c5d-a1f5-d82dfe248d2a:image.png)

- 위 예제의 num 은 프로퍼티가 아니라 즉시 실행 함수 내에서 선언된 변수다. 만약 인스턴스의 프로퍼티라면 public 프로퍼티가 된다. 하지만 저거는 은닉된 변수다.
- 생성자 함수 Counter 는 프로토 타입을 통해 increase, decrease 메서드를 상속받는 인스턴스를 생성한다. 이도 역시 클로저이다. 프로토 타입으로 상속되어도 자유 변수 num 참조 가능.
- ❗변수 값은 누군가에 의해 언제든지 변경될 수 있어 오류 발생의 근본적 원인이 될수 있다. → **외부 상태 변경이나 가변 데이터를 피하고 불변성을 지향하는 함수형 프로그래밍에서 부수효과를 최대한 억제하여 오류를 피하고 프로그램의 안정성을 높이기 위해 클로저는 적극적으로 사용된다.**

```tsx
function makeCouter(aux) {
  let count = 0;

  return function () {
    count = aux(count);
    return count;
  };
}

function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}

const increaser = makeCouter(increase);
const decreaser = makeCouter(decrease);

console.log(increaser()); // 1
console.log(increaser()); // 2
console.log(decreaser()); // -1
console.log(decreaser()); // -2
```

- makeCounter 함수는 고차함수다. makeCounter 함수는 인자로 전달받은 보조 함수를 합성하여 자신이 반환하는 함수의 동작을 변경할 수 있다.
- ❗❗❗**이 때 주의해야 할 것은 makeCounter 함수를 호출해 함수를 반환할 때 반환된 함수는 자신만의 독립된 렉시컬 환경을 갖는다는 것이다.**
- **🌟❗이는 함수를 호출할 때마다 새로운 makeCounter 함수 실행 컨텍스트의 렉시컬 환경이 생성되기 때문이다.**

![image.png](attachment:b9627324-1025-4a54-a7d8-1710873e1837:image.png)

![image.png](attachment:2e27e5f2-2118-47bd-ac51-bcc202ca299d:image.png)

- 위 예제에서 전역 변수 increase 와 decrease 에 할당된 함수는 각각 자신만의 독립된 렉시컬 환경을 갖기 때문에 카운트를 유지하기 위해 자유 변수 count 는 공유하지 않아 증감이 연동되지 않는다.
- **만약 연동하게 만들고 싶다면 렉시컬 환경을 공유하는 클로저를 만들어야 한다!!!**

```tsx
const counter = (function () {
  let count = 0;

  return function (aux) {
    count = aux(count);
    return count;
  };
})();

function increase(n) {
  return ++n;
}

function decrease(n) {
  return --n;
}

console.log(counter(increase)); // 1
console.log(counter(increase)); // 2
console.log(counter(increase)); // 3
console.log(counter(increase)); // 4
console.log(counter(decrease)); // 3
console.log(counter(decrease)); // 2
```

# 24.5 캡슐화와 정보 은닉

---

- **캡슐화는 객체의 상태를 나타내는 프로퍼티와 이를 참조하고 조작하는 메서드를 하나로 묶는 것.**
- **캡슐화를 특정 프로퍼티나 메서드를 감출 목적으로 사용하기도 하는데 이를 정보 은닉이라고 한다.**
- 정보 은닉은 외부에 공개할 필요가 없는 구현의 일부를 외부에 공개되지 않도록 감추어 적절치 못한 접근으로부터 객체의 상태가 변경되는 것을 방지해 정보를 보호하고, 객체 간의 상호 의존성, 즉 결합도를 낮추는 효과가 있다.
- 대부분의 객체지향 프로그래밍 언어는 클래스를 정의하고 멤버에 대하여 접근 제한자를 선언하여 공개 범위를 한정할 수 없지만, 자바스크립트는 그런거 없다.
- 기본적으로 public 이라는 의미다.

![image.png](attachment:24b647a8-3c50-45de-b0a5-6569d06e4ca8:image.png)

- 위 예제에서 name 프로퍼티는 공개되어 있어 참조하거나 변경할 수 있지만 (public ), \_age 는 지역 변수 이므로 person 생성자 함수 외부에서 참조하거나 변경할 수가 없다. (private)
- 하지만 위 예제에선 sayHi 메서드가 중복 생성되기 때문에 이를 방지하기 위해 sayhi 를 프로토타입 메서드로 변경해보자

![image.png](attachment:17206831-74f5-49b8-985b-b6f5279e3cd5:image.png)

- 하지만 \_age 에 참조할 수 없는 문제가 발생한다.
- ❗ 이것도 해결해 보기 위해 즉시 실행 함수를 사용하여 Person 생성자 함수와 Person.prototype.sayHi 메서드를 하나의 함수 내에 모아보자.

![image.png](attachment:57555b55-f58f-49e9-b09e-8c9d66e4f2bc:image.png)

- 이렇게 하면 자바스크립트에서도 정보 은닉이 가능한 것처럼 보인다.
- 하지만 위 코드에도 문제가 있다…..
- 바로 Person 생성자 함수가 여러 개의 인스턴스를 생성할 경우 다음과 같이 \_age 변수의 상태가 유지되지 않는 것이다.

![image.png](attachment:bd378f9a-8b4d-45cc-bc8c-36ef3ae679ea:image.png)

- me 인데 30 살이 되어버림…
- 이는 sayHi 메서드가 단 한 번 생성되는 클로저이기 때문에 발생하는 현상이다. 즉시 실행 함수가 호출되는 순간 정의되어 참조를 내부슬롯에 저장한다. 그렇기 때문에 모든 인스턴스가 상속을 통해 호출하는 메서드의 상위 스코프는 언제나 동일한 상위 스코프를 사용하게 된다. → 문제 발생
- 이처럼 자바스크립트는 정보 은닉을 완전하게 지원하지 않는다.
  - 인스턴스 메서드 사용 흉내는 내지만 → 메모리 낭비
  - 프로토타입 메서드 → 인스턴스 생성시 마다 바뀜.
- ES6 에서 흉내 냈지만 근본적인 해결책 X
- 다행히 private 필드를 정의할 수 있는 표준 사양이 제안 되었다. node.js 12 이상에는 이미 구현되었고, 이후 장에서 배워보도록 하자.

# 24.6 자주 발생하는 실수

---

![image.png](attachment:0246e642-7416-4804-9a2a-4d1aca3c7b99:image.png)

- 일단 3,3,3 출력함.
- 클로저를 사용해보자.

![image.png](attachment:df08515f-195f-4f03-9aa7-d1cf9833a130:image.png)

- 해결할 수 있지만 번거롭다.
- ES6 부터는 let 을 사용하면 깔끔하게 해결된다.

![image.png](attachment:9263bf2a-9cd2-43a8-90b4-af0d87fa18f5:image.png)

- for 문이 반복될 때마다 새로운 코드 블록의 렉시컬 환경이 생성된다. 만약 for 문의 코드 블록 내에서 정의한 함수가 있다면 이 함수의 상위 스코프는 for 문의 코드 블록이 반복될 때마다 생성된 블록의 렉시컬 스코프 환경이다.

![image.png](attachment:8e0b678d-2e40-4019-b80c-3f7f775461f6:image.png)

- 이처럼 let 이나 const 키워드를 사용하는 반복문은 for, for of for in 등 은 코드 블록을 반복 실행할 때마다 새로운 렉시컬 환경을 생성하여 스냅숏을 찍는 것처럼 저장한다. 단, 이는 반복문의 코드 블록 내부에서 함수를 정의할 때 의미가 있다. ⚠️ **만약 함수 정의가 없다면 반복문이 생성하는 렉시컬 환경은 반복 직후, 아무도 참조하지 않기 때문에 가비지 컬렉션의 대상이 된다.**
- 또 다른 방법은 고차 함수를 사용하는 방법이다. 이 방법은 반복문 사용을 억제할 수 있어 오류를 줄이고 가독성을 높인다. ← 참고만 하자.

```
const func = Array.from({ length: 3 }, (_, i) => () => i);
func.forEach((f) => console.log(f())); // 0 1 2
```
