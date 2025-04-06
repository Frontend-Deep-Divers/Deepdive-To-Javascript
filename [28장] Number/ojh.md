- 표준 빌트인 객체 Number 에 대해서 이야기해보자.

# 28.1 Number 생성자 함수

---

- Number 객체 new 연산자와 함께 Number 인스턴스 생성 가능.
- 인수 전달 안하면 [[NumberData]] 내부 슬롯에 0을 할당한 래퍼 객체 생성.

![image.png](attachment:17c56e57-01b3-4619-8e52-ea2c62559671:image.png)

- 개발자 도구에서 실행하면 [[PrimitiveValue]] 가 보이는데 이게 [[NumberData]] 내부 슬롯을 가르킨다. ES5 에서는 [[NumberData]] 를 [[PrimitiveValue]] 라고 불렸다.
- 숫자 이외의 값을 할당하면 숫자로 강제 변환 후 할당한다. 만약 숫자로 변화시킬 수 없으면 NaN 을 할당한다.
- new 연산자를 사용하지 않으면 원시값 숫자를 반환한다. 이를 사용해 명시적으로 타입 변환함.
  ![image.png](attachment:10fe11c0-64cc-4a00-a4ae-0cee218c100c:image.png)

# 28.2 Number 프로퍼티

---

**1️⃣ Number.EPSILON**

- ES6 에서 도입되었고, 1과 1보다 큰 숫자 중에서 가장 작은 숫자와의 차이와 같다.
- 약 2.2 X 10^-16 이다.
- 이걸 사용하는 경우는 부동소수점 산술 연산이 정확한 결과를 기대하기 어렵기 때문이다. ← **🌟 정수는 2진법으로 오차 없이 저장 가능하지만 부동소수점을 표현하기 위해 가장 널리 쓰이는 표준인 IEEE 754 는 2진법으로 변환했을 때 무한 소수가 되어 미세한 오차가 발생할 수 밖에 없는 구조적 한계가 있다. → 이건 자바나 c++ 도 마찬가지래용…**
  ![image.png](attachment:06b887c4-8707-430d-b247-5a67d6600a19:image.png)
- 위와 같은 방법으로 사용할 수 있다.

**2️⃣ Number.MAX_VALUE**

- 자바스크립트에서 표현할 수 있는 가장 큰 양수 값

![image.png](attachment:62a11564-3b39-4cdf-8189-88c7f6ff887a:image.png)

- 이거보다 큰 숫자는 Infinity
- **[Infinty 가 있는데 왜 굳이..??]**
  "왜 그냥 `Infinity`를 안 쓰고, `Number.MAX_VALUE` 같은 걸 굳이 따로 쓰는가?"
  → 이건 **“무한대인지 아닌지”, “오차 범위 안의 숫자인지”** 를 명확히 나누기 위해서예요.
  ***
  ## ✅ 핵심 차이
  | 비교      | `Infinity`             | `Number.MAX_VALUE`                 |
  | --------- | ---------------------- | ---------------------------------- |
  | 의미      | 무한대 (실제 수 아님)  | 유한한 실수 중 가장 큰 값          |
  | 계산 결과 | 계산이 "넘쳤다"는 신호 | "아직 유효한 수다"는 범위 표시     |
  | 사용 목적 | **에러 상태**를 표현   | **계산 한계** 또는 기준값으로 사용 |
  | 비교 연산 | 대부분의 숫자보다 큼   | 실수 중 최댓값으로 비교 가능       |
  ***
  ## 📌 왜 `Infinity`를 그냥 안 쓰고 `MAX_VALUE`를 쓰냐면?
  ### 1️⃣ **Infinity는 계산 실패의 결과값이야**
  ```
  js
  복사편집
  1 / 0           // Infinity → 정의 불가능한 수
  Number.MAX_VALUE * 2 // Infinity → 오버플로우 발생

  ```
  → 이건 **"정상적인 수"가 아님**
  → 그래서 이걸로 뭔가 비교하거나 최소/최대 값을 초기화하면 **의미가 흐려짐**
  ***
  ### 2️⃣ **Number.MAX_VALUE는 "최댓값 기준"으로 사용 가능**
  ```
  js
  복사편집
  let max = -Infinity;
  for (let n of arr) {
    if (n > max) max = n;
  }
  // 이것도 되지만...

  let max = Number.MIN_VALUE; // 더 직관적일 수 있음

  ```
  ### 예: 값을 제한할 때
  ```
  js
  복사편집
  if (value > Number.MAX_VALUE) {
    throw new Error("값이 너무 큽니다!");
  }

  ```
  → `Infinity`까지 가기 전에 미리 감지 가능!
  → 즉, **"계산 가능하지만 위험 수준"**일 때 활용 가능
  ***
  ### 3️⃣ **Infinity는 수학적으로 의미 없음**
  ```
  js
  복사편집
  Infinity + 1 === Infinity
  Infinity * 2 === Infinity
  Infinity - Infinity === NaN ❗

  ```
  → 연산 자체가 **정확한 수학적 의미를 잃어버림**
  → 그래서 **계산용으로 부적절**
  ***
  ## 🧠 요약
  > Infinity는 경고·오류 상태를 뜻하는 특수 값이고,
  >
  > `Number.MAX_VALUE`, `MIN_VALUE`는 **“계산 가능한 수의 경계선”**이기 때문에
  >
  > 실전에서는 **비교 기준, 범위 체크, 초기값** 등에 `MAX_VALUE` 같은 상수를 더 많이 씁니다.
  ***
  ## 💬 비유하자면
  | 상황               | 비유                                          |
  | ------------------ | --------------------------------------------- |
  | `Number.MAX_VALUE` | “이 이상이면 위험해요”라는 **정상 범위의 끝** |
  | `Infinity`         | “이미 망했어요…”라는 **에러 상태**            |

![image.png](attachment:7b61f7f4-5f97-46d9-ace7-909ea71beaa6:image.png)

3️⃣ **Number.MIN_VALUE**

- 자바스크립트에서 표현할 수 있는 가장 작은 양수
  ![image.png](attachment:30a89d62-fd23-4804-a18a-a3690ef62e13:image.png)
- 이거보다 작은 숫자는 0.

4️⃣ **Number.MAX_SAFE_INTEGER**

- 자바스크립트에서 안전하게 표현할 수 있는 가장 큰 정수 값
- 9007199254740991

![image.png](attachment:c747457e-72a2-40ba-8f0e-4010faf98bff:image.png)

**5️⃣ Number.MIN_SAFE_INTEGER**

- 자바스크립트에서 안전하게 표현할 수 있는 가장 작은 정수 값.
- -9007199254740991

![image.png](attachment:395efef6-30e7-41b4-b21a-68fd67a08836:image.png)

**6️⃣ Number.POSITIVE_INFINITY**

- 양의 무한대 infinty

**7️⃣ Number.NEGATIVE_INFINITY**

- 음의 무한대 -infinity

**8️⃣ Number.NaN**

- 숫자가 아님을 나타내는 숫자값.

# 28.3 Number 메서드

---

**1️⃣ Number.isFinite**

- ES6 에서 도입됨. 인수로 전달된 숫자값이 정상적인 유한수. 즉,Infinity 또는 -Infinity 가 아닌지 검사.
- NaN 도 false

![image.png](attachment:10dbc830-2e78-42af-8c6b-3ba103a0ea1e:image.png)

- 빌트인 전역 함수 isFinite 와 의 차이점.
  - isFinite 는 전달받은 인수를 암묵적 타입 변환하여 검사를 수행하지만,
  - Number.isFinite 는 전달받은 인수를 숫자로 암묵적 타입 변환하지 않는다.
  - 따라서 숫자가 아닌 값은 언제나 false.

![image.png](attachment:bab0c177-cfcb-488f-867c-cbdba2e65c53:image.png)

**2️⃣ Number.isInteger**

- 전달된 인수가 정수인지 검사, 암묵적 타입 변환 X

**3️⃣ Number.isNaN**

- 인수로 전달된 숫자값이 NaN 인지 검사하여 불리언 값 반환.
- 암묵적 타입 변환 X → 즉, 숫자가 아닌 인수면 반환값 언제나 false.

**4️⃣ Number.isSafeInteger**

- ES6 에서 도입됨.
- 인수로 전달된 숫자값이 안전한 정수인지 확인, 타입 변환 X
- 범위 -(2^53 - 1) ~ 2^53 - 1 사이의 정수

**5️⃣ Number.prototype.toExponential**

- 숫자를 지수표기법 으로 변환하여 문자열로 반환함.
- 지수 표기법: 매우 크거나 작은 숫자를 표기할 때 주로 사용 e+n 승
- 인수로 소수점 이하로 표현할 자릿수 전달 가능.

![image.png](attachment:3ac9a596-b2d5-4bf1-9488-93b58ed5dc49:image.png)

- ❗ **위의 상황이 발생하는 이유!**
  - **숫자 뒤의 . 은 의미가 모호함. 프로퍼티 접근자나, 숫자 구분일수도 있다. → 자바스크립트 엔진은 소수 구분 기호로 해석한다. → 그렇기에 소수 구분 기호로는 해석할 수 없기에 에러가 발생한다.**
- ⚠️**그러니까 그룹 연산자 사용을 권장한다.**

![image.png](attachment:7a95479e-e6ce-45fb-a9b2-870e2d3a9def:image.png)

**6️⃣ Number.prototype.toFixed**

- toFixed 는 숫자 메서드를 반올림하여 **문자열**로 반환한다. 인수는 0~20 까지 가능.
- 생략하면 0

![image.png](attachment:175d2a92-86bd-4c51-8d2b-aa6ffdc6779c:image.png)

**7️⃣ Number.prototype.toPrecision**

- toPrecision 메서드는 인수로 전달받은 전체 자릿수까지 유효하도록 나머지 자릿수를 반올림하여 문자열로 반환함.
- 전달 받은 전체 자릿수 표현이 불가능한 겨우 지수 표기법으로 결과 반환한다.
- 0~21 사이의 정수. 생략하면 기본값 0

![image.png](attachment:18f94166-6936-49b7-bf54-9c699c8ba6b7:image.png)

**8️⃣ Number.prototype.toString**

- toString 메서드는 숫자를 문자열로 변환하여 반환.
- 진법을 나타내는 2~36 사이의 정수값을 인수로 전달할 수 있다.
- 인수를 생략하면 기본값 10진법이 지정된다.
  ![image.png](attachment:9b3809d1-fc8c-4260-ba32-3ff4593e4e2b:image.png)
