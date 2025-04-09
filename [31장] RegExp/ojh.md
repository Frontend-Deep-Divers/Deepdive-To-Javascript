# 31.1 정규 표현식이란?

---

- 정규 표현식은 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어다.
- 자바스크립트만의 고유 문법이 아님.
- ES3 부터 도입
- **패턴 매칭 기능**
  ![image.png](attachment:bee5e27e-23f0-4f87-b9a3-465759e5bff2:image.png)
- 정규 표현식을 사용하여 반복문과 조건문 없이 패턴을 정의하고 테스트하는 것으로 간단히 체크 가능.
- ❗다만 정규 표현식은 주석이나 공백을 허용하지 않고 여러 기호를 혼합하여 사용하기 때문에 가독성이 좋지 않다!

# 31.2 정규 표현식의 생성

---

- 정규 표현식 생성 방법은 정규 표현식 리터럴과 RegExp 생성자 함수 사용이다.
- 리터럴 사용 방식
  - **패턴과 플래그로 구성**
  ![image.png](attachment:442966e5-a531-41f5-ae9f-974b1dde9144:image.png)
- 생성
  ![image.png](attachment:759c1208-7851-454f-a7d0-9350dc5769e7:image.png)
- RegExp 생성자 함수를 사용하여 변수를 사용해 동적으로 RegExp 객체 생성 가능

# 31.3 RegExp 메서드

---

![image.png](attachment:4476f424-6aa6-42b2-84a2-bda6e9619211:image.png)

### 1. RegExp.prototype.exec

- exec 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다. 매칭 결과가 없는 경우 null 반환
  ![image.png](attachment:4ebd567a-9e26-43af-937c-5c419e2e400c:image.png)
- exec 메세드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 결과만 반환하므로 주의

### 2. RegExp.prototype.test

- test 메서드는 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환한다.
  ![image.png](attachment:6f33c350-a74c-44fc-bc22-a12c7b8e4749:image.png)

### 3. String.prototype.match

- String 표준 빌트인 객체가 제공하는 match 메서드는
- exec 메서드는 문자열 내의 모든 패턴을 검색하는 g 플래그를 지정해도 첫 번째 결과만 봔환하지만 String.prototype.match 메서드는 g 플래그가 지정되면 모든 매칭 결과를 배열로 반환한다.
  ![image.png](attachment:e82d9f62-c556-4f25-8606-c63fdd55266b:image.png)

# 31.4 플래그

---

- 플래그는 정규 표현식의 검색 방식을 설정하기 위해 사용한다.
- 플래그는 총 6개이며, 그 중 3개를 살펴보자.
  ![image.png](attachment:09fa1a49-3d56-4701-91f6-09b7bdf097a0:image.png)
- 선택적으로 사용, 하나 이상 동시에 설정 가능
- 어떠한 것도 지정하지 않는 경우 대소문자 구분하고, 1개 이상 존재해도 첫 번째 매칭한 대상만 검색
  ![image.png](attachment:65cd0e15-e43a-4413-971d-671e68b44698:image.png)

# 31.5 패턴

---

**1️⃣ 문자열 검색**

- 매칭 결과를 구하면 대소문자를 구분하여 매칭된 1 번째 결과만 변환
- i , g 사용하여 구분 X , 결과 여러개.

**2️⃣ 임의의 문자열**

- . 은 임의의 문자 한 개를 의미한다.
- … 은 내용 상관없이 3자리 문자열과 매치함.

![image.png](attachment:f9653a27-b7f3-444a-938d-c9475112bfb0:image.png)

**3️⃣ 반복 검색**

- {m,n} 은 앞의 패턴이 최소 m 번부터 최대 n 번 반복되는 문자열 의미한다.
  ![image.png](attachment:b007f614-c4ed-41e1-b5a3-9de6cf6a717f:image.png)
- {n} 은 딱 n 번 반복되는 문자열
  ![image.png](attachment:23c2e8c2-a9e5-4525-8bcb-fa04de330d6a:image.png)
- - 는 최소 1번 이상

    ![image.png](attachment:61d72b53-2e2c-49fb-b743-4b18448d0cbb:image.png)
- ? 는 최대 1번 이상 반복되는 문자열 (있거나 없거나)
  ![image.png](attachment:3c3a5b34-2095-4cdc-b99b-839ecfbf92db:image.png)

**4️⃣ OR 검색**

- | 는 or 의 의미임. /A|B/ 는 A 나 B 의미
  ![image.png](attachment:06161cfc-68fe-476d-9405-688c5a33a9b1:image.png)
- 분해되지 않는 단어 레벨 검색
  ![image.png](attachment:10786ed3-bc91-4241-912b-6a08de7464d4:image.png)
- 위의 간단하게 표현하면 아래와 같다. [] 사용
  ![image.png](attachment:ac5453e3-4b60-4dfa-a692-4a2a53d80892:image.png)
- 범위를 지정하려면 [] 내에 - 사용
  ![image.png](attachment:55a70507-8135-4119-98be-429923aeb92c:image.png)
- 대소문자를 구별하지 않고 알파벳을 검색하는 방법
  ![image.png](attachment:4169282b-37e2-4811-9808-48c8449aa3b8:image.png)
- 숫자와 쉼표 합치기 그리고 \d 이의 반대인 \D
  ![image.png](attachment:ccca9197-26d7-4a0d-a1f2-1f1c1db73d31:image.png)
- \w 알파벳, 숫자, 언더 스코어
- \W 는 위의 반대.
  ![image.png](attachment:74343bd1-5de7-42f0-9d46-7c2d58f5c790:image.png)

**5️⃣ NOT 검색**

- [ … ] 내의 ^ 은 not 의 의미를 가진다.
  ![image.png](attachment:f4f2fa83-8028-457e-bdfd-4a86be94a64e:image.png)

**6️⃣ 시작 위치로 검색**

- [ … ] 밖의 ^ 는 문자열의 시작을 의미한다.
  ![image.png](attachment:e482256b-6fca-4647-9055-e5d84679058d:image.png)

**7️⃣ 마지막 위치로 검색**

- $ 는 문자열의 마지막을 의미한다.
  ![image.png](attachment:4e70fcc4-cd49-4b84-9ea5-574719e0bd68:image.png)

# 31.6 자주 사용하는 정규 표현식

---

![image.png](attachment:e38d42b5-dec8-44e8-a56a-b1b7fb18bbf4:image.png)

![image.png](attachment:b57fed06-bffd-43e7-80d7-cb6bbe602c74:image.png)

![image.png](attachment:137ba6c6-1d36-4996-ac3e-d242817ebf98:image.png)
