# 37.1 Set

---

- 중복요소 허용하지 않는 집합

**1️⃣ new Set 생성**

- 이터러블을 인수로 받아 Set 객체 생성
- 배열 중복 요소 없앨 때 유용.

**2️⃣ 요소 개수 확인**

- size
- size 변경 불가

**3️⃣ 요소 추가**

- add 로 추가. 중복되면 추가 안됨.
- ⭐ Set 사용할 때 당연히 유의해야 할 것이 원시값이 아니면 참조가 다르기 때문에 같은 요소가 들어있어도 다른 것으로 인식하기 때문에 주의할 것.
- ❗연속적으로 호출 가능 ← 반환 값이 추가된 Set 객체이기 때문에

**4️⃣ 요소 존재 여부 확인**

- has
- 불리언 값 반환

**5️⃣ 요소 삭제**

- delete
- 삭제 성공 여부에 따라 불리언 값 반환
- 인수 전달 해야 함.
- ❗반환 여부가 boolean 이기 때문에 연속 호출 불가능

**6️⃣ 요소 일괄 삭제**

- clear
- 언제나 undefind 반환

**7️⃣ 요소 순회**

- forEach, for … of , 스프레드 문법, 디스트럭처링 할당 모두 가능.
- 단, forEach 에서 배열의 인덱스는 의미가 없다.
- ⭐순회하는 순서는 추가된 순서이다.

**8️⃣ 집합 연산 (처음 알았음!)**

- 교집합을 아래와 같이 만들 수 있다.
  ![image.png](attachment:a76ba14e-309e-4ded-a279-fd0ea645f068:image.png)
  ![image.png](attachment:eba05ece-48c4-4f0d-8159-9fa5cc7b6917:image.png)
- 합집합은 아래와 같이 만들 수 있다.
  ![image.png](attachment:00f9502f-af8e-4d5b-b5bf-c89c7027aef0:image.png)
- 차집합은 아래와 같이 만들 수 있다.
  ![image.png](attachment:773344ee-f5f3-4109-a518-c793615aef98:image.png)
- 부분 집합과 상위 집합은 다음과 같이 만들 수 있다.
  ![image.png](attachment:4127935e-8a84-4b39-9280-8f5570153c62:image.png)

# 37.2 Map

---

- 키와 값 쌍으로 이루어진 컬렉션 객체와 아래와 같은 차이가 있다.
  ![image.png](attachment:bdf0a8f7-5500-47d8-9062-24ac0b942a60:image.png)

**1️⃣ Map 객체의 생성**

- new Map ()
- 이터러블 인수로 전달받아 생성. → 단 이때 키와 값의 쌍으로 이루어져 있어야 한다.
  ![image.png](attachment:272a3f22-bbf9-4833-a940-21952fa11afc:image.png)

**2️⃣ 요소 개수 확인**

- size 통해 크기 확인 가능.

**3️⃣ 요소 추가**

- set 으로 key value 설정
- 반환값이 Map 객체 이므로 연속적 호출 가능
- 중복된 키를 추가하면 덮어써짐.
- 객체와 달리 모든 값을 키로 사용 가능하다.

**4️⃣ 요소 취득**

- get
- 키에 해당하는 value 반환

**5️⃣ 요소 존재 여부 확인**

- has

**6️⃣ 요소 삭제**

- delete 사용해서 삭제
- 불리언 값 반환. 연속적인 호출 불가능
- 없는 값 삭제하라 해도 에러 X

7️⃣ **요소 일괄 삭제**

- clear
- 언제나 undefined

**8️⃣ 요소 순회**

- forEach 로 순회 가능
- ⚠️ ⚠️ value , key, 자기 자신 순 (값 ,인덱스 이렇게 생각하면 편할듯)
- 역시나 … , 디스트럭처링 할당, for … of 문 등 사용 가능.
- for … of 하면 [key,value] 형태로 반환함.
- Map 객체는 이터러블이면서 동시에 이터레이터인 객체를 반환하는 메서드를 제공한다!
  ![image.png](attachment:7e99fbc5-8b97-4b0b-90e2-a72a0e28d271:image.png)
  ![image.png](attachment:15cbe679-e348-480c-bd3a-f04c4f942355:image.png)
- 역시 순서를 보장해준다.
