# 34.1 이터레이션 프로토콜

---

- ES6 에서 도입된 이터레이션 프로토콜은 순회 가능한 데이터 컬렉션을 만들기 위해 ECMAScript 사양에 정의하여 미리 약속한 규칙이다.
- ES6 이전의 순회 가능한 데이터 컬렉션. 즉 배열, 문자열, 유사 배열 객체, DOM 컬렉션 등은 통일된 규약 없이 각자 나름의 구조를 가지고 for 문, for … in 문 등 다양한 방법으로 순회할 수 있었다. → ES6 에서는 순회 가능한 데이터 컬렉션을 이터레이션 프로토콜을 준수하는 이터러블로 통일하여 for … of 문, 스프레드 문법, 배열 디스트럭처링 할당의 대상으로 사용할 수 있도록 일원화 했다.
  ![image.png](attachment:a0bb72c7-3066-4a00-97c9-d8aaeb0a6755:image.png)

![image.png](attachment:df9135ff-9341-4cb7-8e95-895daf8e2b52:image.png)

### 1️⃣ 이터러블

- 이터러블 프로토콜을 준수한 객체.
- Symbol.iterator를 프로퍼티 키로 사용한 메서드를 직접 구현하거나 프로토타입 체인을 통해 상속받은 객체를 말함.
- 이터러블인지 확인하는 함수는 다음과 같이 구현할 수 있다.
  ![image.png](attachment:681f1ed8-f48d-4bf4-a326-c016ee6861d2:image.png)
- 예를 들어, 배열은 Array.prototype 의 Symbol.iterator 메서드를 상속받는 이터러블이다. 이터러블은 for … of 문으로 순회할 수 있으며, 스프레드 문법과 배열 디스트럭처링 할당의 대상으로 사용할 수 있다.
  ![image.png](attachment:8e724fed-8d23-494c-bf8f-ab48600128eb:image.png)
- Symbol.iterator 메서드를 직접 구현하지 않거나 상속 받지 않은 일반 객체는 이터러블 프로토콜을 준수한 이터러블이 아니다. → 일반 객체는 for … of 문 순회 불가능 하며, 스프레드 연산자나 배열 디스트럭처링 할당 불가
- 단, 21 년 에는 객체에 스프레드 문법의 사용을 허용한다.
  ![image.png](attachment:27e5c7ff-d735-4b2e-8d79-b2fe69e5a886:image.png)

### 2️⃣ 이터레이터

- 이터러블의 Symbol.iterator 메서드를 호출하면 이터레이터 프로토콜을 준수한 이터레이터를 반환한다. **이터러블의 Symbol.iterator 메서드가 반환한 이터레이터는 next 메서드를 갖는다.**
  ![image.png](attachment:45298def-3fa9-4fbe-996b-77294818f1b9:image.png)
- 이터레이터의 next 메서드는 이터러블의 각 요소를 순회하기 위한 포인터의 역할을 한다. 즉, next 매서드를 호출하면 이터러블을 순차적으로 한 단계씩 순회하며 **순회 결과를 나타내는 iterator result 객체를 반환한다.**
- value 를 통해 현재 순회 중인 이터러블의 값을 나타내며 done 프로퍼티는 이터러블의 순회 완료 여부를 나타낸다.
  ![image.png](attachment:538796bb-e7d6-41cf-a5d4-febdd11c6ee7:image.png)

# 34.2 빌트인 이터러블

---

- 자바스크립트는 이터레이션 프로토콜을 준수한 객체인 빌트인 이터러블을 제공한다.
- 아래는 표준 빌트인 객체 중 빌트인 이터러블이다.
  ![image.png](attachment:b7366313-6983-41e1-893d-771daf77ecac:image.png)

# 34.3 for … of 문

---

- 이터러볼을 순회하면서 이터러블 요소를 변수에 할당한다.
- for … in 문과 형식이 유사한데 → for in 문은 객체의 프로토 타입 체인 상 존재하는 모든 프로토타입 어트리뷰트 [[Enumerable]] 값이 true 인 프로퍼티를 순회하며 열거한다. 이때 프로퍼티 키가 심벌인 프로퍼티는 열거하지 않는다.
- for … of 는 내부적으로 이터레이터의 next 메서드를 호출하여 이터러블을 순회하며 next 가 반환한 이터레이터 리절트 객체의 value 프로퍼티 값을 for … of 문의 변수에 할당한다. 그리고 done 이 true 면 순회를 중단한다.
- 내부 동작
  ![image.png](attachment:9e9a9617-a5e7-4554-9043-e4a5b1416d73:image.png)

# 34.4 이터러블과 유사 배열 객체

---

- 유사 배열 객체는 마치 배열처럼 인덱스를 프로퍼티로 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다.
- length 프로퍼티를 갖기 때문에 for 문으로 순회할 수 있고, 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있다.
  ![image.png](attachment:aa72de2d-cb0f-4d8c-b63b-d7b22ad12e19:image.png)
- 유사 배열 객체는 이터러블이 아닌 일반 객체이다. Symbol.iterator 메서드가 없기 때문에 for … of 문으로 순회할 수 없다.
  ![image.png](attachment:e3592cee-5d54-40b9-ad1b-44c2d130f12a:image.png)
- 단, arguments, NodeList, HTMLCollection 은 유사 배열 객체이면서 이터러블이다. → 정확히는 ES6 에서 Symbol.iterator 메서드를 구현하여 이터러블이 되었다.
- **❗유사 배열 객체는 Array.from 메서드를 사용하여 간단히 배열로 변환시킬 수 있다. → Array.from 메서드는 유사 배열 객체 또는 이터러블을 인수로 받아 배열로 변환하여 반환한다.**
  ![image.png](attachment:9dbe0452-e8a9-4940-be80-a044a2e5781a:image.png)

# 34.5 이터레이션 프로토콜의 필요성

---

![image.png](attachment:8a22cc9f-7a85-4ee2-bb2b-a9abcdf9ab09:image.png)

- **이터레이션 프로토콜은 다양한 데이터 공급자가 하나의 순회 방식을 갖도록 규정하여 데이터 소비자가 효율적으로 다양한 데이터 공급자를 사용할 수 있도록 연결하는 인터페이스 역할을 한다.**

# 34.6 사용자 정의 이터러블

---

### 1. 사용자 정의 이터러블 구현

- 이터레이션 프로토콜을 준수하지 않는 일반 객체로 이터레이션 프로토콜을 준수하도록 구현하면 사용자 정의 이터러블이 된다.
  ![image.png](attachment:80711f69-dce6-43b2-bc32-43cc921b769b:image.png)
- 사용자 정의 이터러블은 이터레이션 프로토콜을 준수하도록 Symbol.iterator 메서드를 구현하고 Symbol.iterator 메서드가 next 메서드를 갖는 이터레이터를 반환하도록 한다.
- next 메서드에서는 done 과 value 프로퍼티를 가지는 이터레이터 리절트 객체를 반환한다.
- for … of 문 뿐 아니라 스프레드 문법, 배열 디스트럭처링 할당에도 사용할 수 있다.
  ![image.png](attachment:8f1a02e9-b138-4068-ba91-91a87ebe5c8b:image.png)

### 2. 이터러블을 생성하는 함수

- 위에서 본 finbonacci 이터러블은 내부에 수열의 최대값 max 를 가지고 있는데, 이 수열의 최대값은 고정된 값으로 외부에서 전달한 값으로 변경할 방법이 없다는 아쉬움이 있다. → ❗ 수열의 최대값을 인수로 받는 함수로 만들면 된다.
  ![image.png](attachment:5705a8ea-e2a1-4912-93ca-af659018537d:image.png)

### 3. 이터러블이면서 이터레이터인 객체를 생성하는 함수

- 앞에서 살펴본 fibonacci 함수는 이터러블을 반환한다. 만약 이터레이터를 생성하려면 이터러블의 Symbol.iterator 메서드를 호출해야 한다.
  ![image.png](attachment:66adfe75-63e9-4b08-8dfa-e3ac80f4c43d:image.png)
- 이터러블이면서 이터레이터인 객체를 생성하면 Symbol.iterator 메서드를 호출하지 않아도 된다.

![image.png](attachment:33e096aa-644b-49c7-a6d3-5208b6d53d8c:image.png)

### 4. 무한 이러터블과 지연 평가

- 무한 이터러블을 생성하는 함수를 정의하면 쉽게 무한 수열을 구현할 수 있다.
  ![image.png](attachment:e24acbf8-45a4-4dc5-8c6b-c5d42317c09f:image.png)
- 배열이나 문자열 등은 모든 데이터를 메모리에 미리 확보한 다음 데이터를 공급한다.
- 하지만 이터러블은 지연 평가를 통해 데이터를 생성한다.
- 지연 평가는 데이터가 필요한 시점 이전까지 미리 데이터를 생성하지 않다가 데이터가 필요한 시점이 되면 그때야 비로소 데이터를 생성하는 기법이다. 즉, 평가가 결과가 필요할 때까지 평가를 늦추는 기법이다.
- 이처럼 지연 평가를 사용하면 불필요한 데이터를 미리 생성하지 않고 필요한 데이터를 필요한 순간에 생성하므로 빠른 속도를 기대할 수 있고 불필요한 메모리를 소비하지 않으며, 무한도 표현할 수 있다는 장점이 있다.

# 내가 궁금해서 찾아본

## ✅ 이터러블 프로토콜이란?

> "이터러블 프로토콜(iterable protocol)"이란 어떤 객체가 for...of 문 등에서 순회 가능하게 하려면 따라야 하는 약속(규칙)을 말합니다.

즉, 자바스크립트가 **"너가 순회 가능한 객체라면, 이 규칙대로 행동해줘"** 라고 정해놓은 거예요.

---

## 🧩 그 규칙은 뭔데?

이터러블 프로토콜을 따르기 위해서는 다음을 만족해야 합니다:

### 1. `Symbol.iterator`라는 키의 메서드를 가지고 있어야 함

```
js
복사편집
const iterable = {
  [Symbol.iterator]() {
    return {
      next() {
        // { value: 값, done: 불린값 } 형태를 반환
      }
    }
  }
}

```

### 2. `Symbol.iterator` 메서드를 호출하면, **"이터레이터(iterator)" 객체를 반환**해야 함

이터레이터는 반드시 `next()` 메서드를 가지고 있어야 하고, 이 `next()`는 다음 형태의 **객체**를 반환해야 해요:

```
js
복사편집
{ value: 어떤값, done: true or false }

```

---

## 💡 그래서 이터러블 프로토콜은…

정리하면,

| 구성 요소         | 설명                                               |
| ----------------- | -------------------------------------------------- |
| `Symbol.iterator` | 이터러블임을 나타내는 메서드 이름 (예약된 심볼)    |
| 반환값            | `next()` 메서드를 가진 이터레이터 객체             |
| `next()`          | 순회할 때마다 호출되어 `{ value, done }` 객체 반환 |

---

## 📌 예: 직접 만든 이터러블

```
js
복사편집
const myIterable = {
  [Symbol.iterator]() {
    let i = 0;
    return {
      next() {
        if (i < 3) {
          return { value: i++, done: false };
        } else {
          return { value: undefined, done: true };
        }
      }
    }
  }
};

for (const num of myIterable) {
  console.log(num); // 0, 1, 2
}

```

- 이 객체는 `Symbol.iterator` 메서드를 구현했기 때문에 **이터러블**이고,
- 해당 메서드가 반환하는 객체는 `next()` 메서드를 가지고 있어서 **이터레이터**입니다.
- 그래서 `for...of`, 스프레드 연산자 등에서 사용 가능해요.
