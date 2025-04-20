- 스프레드 문법을 사용할 수 있는 대상은 Array, String, Map, Set, DOM 컬렉션, arguments 와 같이 for … of 문으로 순회할 수 있는 이터러블에 한정
- 스프레드 문법의 결과는 값이 아니기 때문에 변수에 할당할 수 없다.

![image.png](attachment:4c8e81d9-5879-475c-89ca-8f0a99f38abe:image.png)

- 함수 호출문의 인수 목록
- 배열 리터럴의 요소 목록
- 객체 리터럴의 프로퍼티 목록

# 35.1 함수 호출문의 인수 목록에서 사용하는 경우

---

![image.png](attachment:ce55d7b4-f5eb-438b-88eb-1cf4344c0dc4:image.png)

- rest 파라미터와 형태가 동일하여 혼동할 수 있으니 주의하자.
  ![image.png](attachment:00b85015-84c1-461a-bf34-e3d44132e6bd:image.png)

# 35.2 배열 리터럴 내부에서 사용하는 경우

---

**1️⃣ concat**

- 2개의 배열을 결합하고 싶을 때 사용하던 concat 메서드를 스프레드 문법을 사용하면 1개로 결합할 수 있다.
  ![image.png](attachment:3366f4f2-7abc-447e-90fb-53dd42f19a2f:image.png)

**2️⃣ splice**

- splice 메서드는 3번 째 인자로 배열을 전달해 중간에 요소를 추가할 수 있다. 배열 자체가 요소로 추가되는 데 여기서, 스프레드 연산자를 사용하면 가독성있게 추가할 수 있다.
  ![image.png](attachment:a3b213e2-3125-4244-bf27-5d116c5abee3:image.png)

**3️⃣ 배열 복사**

- slice 메서드를 사용하여 배열을 복사하는데
- 스프레드 연산자로 간결하고 가독성 좋게 표현 가능
- 단 얕은 복사임!
  ![image.png](attachment:338617b2-619f-4cfa-8ebd-9c5e70d5ff51:image.png)

**4️⃣ 이터러블을 배열로 반환**

- slice 를 사용해서 변환하여 반환할 수 있는데
- 스프레드 문법으로 더욱 간편하게 이터러블을 배열로 변환할 수 있다.
- 이터러블이 아닌 유사 객체 배열을 변환하기 위해서는 → Array.from 사용

# 35.3 객체 리터럴 내부에서 사용하는 경우

---

- 일반 객체 대상으로도 스프레드 문법 사용 허용한다.
- 스프레드 메서드가 제안되기 전에는 Object.assign 메서드를 사용하여 병합하거나 변경 또는 추가했다.
- 스프레드 연산자로 대체 가능
  ![image.png](attachment:523f6854-a502-4374-91f8-f50ff8dad1e2:image.png)
