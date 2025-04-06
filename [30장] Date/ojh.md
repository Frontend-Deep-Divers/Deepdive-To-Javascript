- 표준 빌트인 객체인 Date 는 날짜와 시간을 위한 메서드를 제공하는 빌트인 객체이면서 생성자 함수다.
- UTC 국제 표준시는 GMT (그리니치 평균시) 라고도 한다.
- KST 한국 표준시는 UTC + 9 시간이다. 즉 한국에 9시간 더 빠르다.
- 현재 날짜와 시간은 자바스크립트 코드가 실행된 시스템의 시계에 의해 결정된다.

# 30.1 Date 생성자 함수

---

- Date 생성자 함수로 생성한 객체는 날짜와 시간을 나타내는 정수값을 갖는다.
- 1970년 1월 1일 00:00:00 기점으로 Date 객체가 나타내는 날짜와 시간까지의 밀리초를 나타낸다.
- Date 생성자 함수로 생성한 데이트 객체는 현재 날짜와 시간을 나타내는 정수값을 가진다. 현재 날짜와 시간이 아닌 다른 날짜와 시간을 다루고 싶은 경우 명시적으로 해당 날짜와 시간 정보를 인수로 저장한다.
- Date 생성자 함수로 객체를 생성하는 방법 4가지

**1️⃣ new Date();**

- 현재 날짜와 시간을 가지는 Date 객체를 반환한다.
- Date 객체는 내부적으로 날짜와 시간을 나타내는 정수값을 가지지만, Date 객체를 콘솔에 출력하면 기본적으로 날짜와 시간 정보를 출력한다.
- new 연산자 없이 호출하면 Date 객체가 아닌 날짜와 시간 정보를 나타내는 문자열을 반환한다.

```tsx
console.log(new Date()); // Sun Apr 06 2025 17:09:10 GMT+0900 (한국 표준시)
console.log(Date()); // Sun Apr 06 2025 17:09:10 GMT+0900 (한국 표준시)
```

- 위 결과는 브라우저 결과이다.
- ❓**브라우저는..?**
  맞아요! 😎
  `Date()` vs `new Date()`는 **Node.js와 브라우저 환경에서 출력 결과가 다르게 보일 수 있습니다.**
  동작 원리는 같지만, **출력 방식(콘솔 출력 형식)**에서 차이가 나는 거예요.
  ***
  ## ✅ 브라우저 vs Node.js 차이
  ### 1️⃣ 브라우저
  ```
  js
  복사편집
  console.log(new Date()); // Sun Apr 06 2025 17:09:10 GMT+0900 (한국 표준시)
  console.log(Date());     // Sun Apr 06 2025 17:09:10 GMT+0900 (한국 표준시)

  ```
  둘 다 사람이 읽기 쉬운 형식으로 출력됨
  > 브라우저 콘솔이 Date 객체의 .toString()을 자동 호출해서 예쁘게 보여줌
  ***
  ### 2️⃣ Node.js
  ```
  js
  복사편집
  console.log(new Date()); // 2025-04-06T08:09:10.000Z
  console.log(Date());     // Sun Apr 06 2025 17:09:10 GMT+0900 (대한민국 표준시)

  ```
  - `new Date()` → **Date 객체** → Node에서는 기본적으로 `.toISOString()`과 비슷하게 출력
  - `Date()` → **문자열** → 브라우저와 동일하게 `toString()` 형태 출력됨
  ***
  ## 🧪 `typeof`도 확인해보자
  ```
  js
  복사편집
  console.log(typeof new Date()); // object
  console.log(typeof Date());     // string

  ```
  둘의 타입은 브라우저나 Node.js나 똑같이 동작해요.
  ***
  ## 🔍 왜 출력이 다를까?
  - 브라우저는 **`console.log()`가 객체에 대해 toString()을 호출해서 보기 좋게 출력**
  - Node.js는 **`console.log()`가 객체 자체를 출력할 때 `.toISOString()` 스타일로 출력**
    - 즉, `console.log(new Date())`는 내부적으로 `util.inspect()`를 통해 ISO 형식으로 보여줌
  ***
  ## ✅ 정리 요약
  | 구분          | `new Date()`                        | `Date()`          |
  | ------------- | ----------------------------------- | ----------------- |
  | 브라우저 출력 | 사람 친화적 형식 (`toString()`)     | 동일한 형식       |
  | Node.js 출력  | ISO 8601 (`toISOString()`처럼 보임) | `toString()` 결과 |
  | 타입          | 객체 (`object`)                     | 문자열 (`string`) |
  ***
  ## 💬 한 줄 요약
  > Date()는 현재 시간을 문자열로 리턴,
  >
  > `new Date()`는 **날짜 객체를 생성**,
  >
  > 출력 형식은 **환경에 따라 달라 보일 수 있음 (Node vs 브라우저)** 😎

**2️⃣ new Date(milliseconds)**

- 숫자 타입의 밀리초를 인수로 전달하면 1970 년 1월 1일 00:00:00 (UTC) 기점으로 인수로 전달된 밀리초만큼 경과한 날짜와 시간을 나타내는 Date 객체를 반환한다.
  ![image.png](attachment:3cef7a24-219a-4d79-b9a4-28f9c9abbe08:image.png)

**3️⃣ new Date(dataString)**

- Date 생성자 함수와 날짜와 시간을 나타내는 문자열을 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체를 반환한다.
- 이 때 인수로 전달한 문자열은 Date.parse 메서드에 의해 해석 가능한 형식이여야 한다. → 곧 나올 예정
- **[🤔해석 가능한 형식]**
  ## 📊 `Date.parse()` / `new Date(string)` 지원 날짜 형식 정리표
  | 입력 형식 예시                | 포맷 설명              | 시간대 기준   | 사용 가능 여부        | 특징                                |
  | ----------------------------- | ---------------------- | ------------- | --------------------- | ----------------------------------- |
  | `'2025-04-06'`                | `YYYY-MM-DD`           | 로컬(Local)   | ✅ 표준               | 날짜만 → 00:00:00 로 처리           |
  | `'2025-04-06T12:00:00'`       | `ISO 8601` (시간 포함) | 로컬(Local)   | ✅ 권장               | 안정적 파싱                         |
  | `'2025-04-06T12:00:00Z'`      | `ISO 8601 UTC`         | **UTC**       | ✅ 권장               | Z = Zulu = UTC                      |
  | `'2025-04-06T12:00:00+09:00'` | ISO + 타임존 명시      | 명시된 타임존 | ✅ 권장               | 시간대 명확                         |
  | `'Apr 6, 2025 12:00:00'`      | 긴 날짜 문자열         | 로컬(Local)   | ⚠️ 가능하지만 권장 ❌ | 브라우저/환경별 파싱 차이 발생 가능 |
  | `'2025/04/06 12:00:00'`       | 슬래시 구분 날짜       | 로컬(Local)   | ⚠️ 가능하지만 권장 ❌ | 일부 구형 브라우저 비호환           |
  | `'Jan 2, 1970 00:00:00 UTC'`  | 영어 날짜 + UTC 명시   | **UTC**       | ✅                    | 타임존 직접 지정 가능               |
  | `'Jan 2, 1970 09:00:00'`      | 영어 날짜 (시간 포함)  | 로컬(Local)   | ✅                    | 시간대 미지정 시 로컬 기준          |
  | `'Invalid format'`            | 임의 문자열            | N/A           | ❌                    | `Invalid Date` 반환                 |
  ***
  ## 🧠 꿀팁
  - **가능한 한 `ISO 8601` 형식을 사용하세요**
    (`YYYY-MM-DD`, `YYYY-MM-DDTHH:mm:ss[Z]`)
  - 슬래시(`/`)나 영어 월(`Jan`, `Feb`, …) 등은 **환경마다 다르게 해석**될 수 있어서
    👉 **파싱 오류 발생 위험**이 있습니다.
  ***
  ## ✅ 요약
  | 질문                       | 답변                                 |
  | -------------------------- | ------------------------------------ |
  | 표준적으로 안전한 형식은?  | ✅ ISO 8601 형식                     |
  | 시간대 명시 안 하면?       | ✅ 로컬 시간 기준                    |
  | 타임존 지정하려면?         | `Z` 또는 `+09:00` 등으로 명시해야 함 |
  | 슬래시(`/`)나 영어 날짜는? | ⚠️ 파싱은 되지만 **환경 따라 위험**  |
  ![image.png](attachment:afc23258-99d0-4ce6-ad8e-6c7e1110ce8d:image.png)

**4️⃣ new Date(year,month[day,hour,minute,second,millisecond])**

- Date 생성자 함수에 연,월,일,시,분,초,밀리초를 의미하는 숫자를 인수로 전달하면 지정된 날짜와 시간을 나타내는 Date 객체 반환한다.
- 이때 연 월은 반드시 지정되어야 한다. 지정하지 않으면 0또는 1이 된다.
  ![image.png](attachment:34ac684c-b785-40a5-aa2a-a3a92c930321:image.png)
- **인수 종류.**
  ![image.png](attachment:5417409d-cbca-4dda-b8ae-ce1979922f3f:image.png)

# 30.2 Date 메서드

---

### 1. Date.now

- 1970 … 기준점으로부터 현재 시간까지 경과한 밀리초 숫자로 반환.
  ![image.png](attachment:12517ceb-926e-4abb-889d-6b010c540893:image.png)

### 2. Date.parse

- 기준으로부터 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환.
- ❗**new Date(dataString) 의 인수와 동일**
  ![image.png](attachment:b074fa8a-6e7e-40d7-bf19-8874db0e0d40:image.png)

### 3. Date.UTC

- 기점으로 인수로 전달된 지정 시간까지의 밀리초를 숫자로 반환한다.
- ❗**new Date(year,month[day,hour,minute,second,millisecond]) 인수와 동일**
- month 는 0 부터 시작함.
  ![image.png](attachment:d7e9e2fd-deee-4c58-b93b-2714af9180e6:image.png)

### 4. Date.prototype.getFullYear

- 연도 나타내는 정수 반환

### 5. Date.prototype.setFullYear

- 연도 나타내는 정수 설정. 연도 이외의 옵션으로 월, 일도 설정할 수 있다.
  ![image.png](attachment:f2638560-272e-4b4e-bf6f-061def45aded:image.png)

### 6. Date.prototype.getMonth

- **월을 나타내는 0~11 사이의 정수 반환 →** ⚠️ **1부터 아닌거 유의**

### 7. Date.prototype.setMonth

- 월 설정. 월 이외에 옵션으로 일도 설정 가능하다.
  ![image.png](attachment:adb6fb73-bdf9-4e20-9ce0-35786ab25718:image.png)

### 8. Date.prototype.getDate

- Date 객체의 날짜를 나타내는 정수를 반환한다.

### 9. Date.prototype.setSate

- Date 객체에 날짜를 나타내는 정수를 설정한다.
  ![image.png](attachment:d5e3f5c6-55af-41ad-af2b-e9daeb25ee37:image.png)

### 10. Date.prototype.getDay

- Date 객체의 요일을 나타내는 정수를 반환한다.
- 일요일부터 0 ~ 6 (토요일)
  ![image.png](attachment:ff4d6db0-c4f6-4fb9-b5e5-f315d16c6ead:image.png)

```tsx
console.log(new Date("2020-07-24").getDay()); // 5 금요일이라고 하네요.
```

### 11. Date.prototype.getHours

- 객체의 시간 ( 0~23 ) 나타내는 정수 반환

### 12. Date.prototype.setHours

- 객체의 시간 (0 ~ 23)을 나타내는 정수 설정.
- 시간 이외에도 옵션으로 분, 초, 밀리초도 설정할 수 있다.
  ![image.png](attachment:008ea968-b155-4fa3-8cb2-f16d91789fb7:image.png)

### 13. Date.prototype.getMinutes

- 객체의 분 (0~59) 정수 반환

### 14. Date.prototype.setMinutes

- 객체의 분을 나타내는 정수 설정.
- 옵션으로 초, 밀리초 설정 가능.
  ![image.png](attachment:1dd1f439-35bf-48a2-8a8d-d45611af666f:image.png)

### 15. Date.prototype.getSeconds

- 객체의 초(0~59) 정수 반환

### 16. Date.prototype.setSeconds

- 객체의 초 정수 설정. 초 이외의 옵션으로 밀리초 설정 가능.
  ![image.png](attachment:8870b62a-ff7f-4207-b2b5-3eb448ff273e:image.png)

### 17. Date.prototype.getMilliseconds

- 객체의 밀리초 (0~999) 나타내는 정수를 반환한다.

### 18. Date.prototype.setMilliseconds

- 정수 설정
  ![image.png](attachment:f8183006-92b9-4e72-91fb-0051bdf2539d:image.png)

### 19. Date.prototype.getTime

- 1970 기점으로 Date 객체까지 경과된 밀리초를 반환한다.

### 20. Date.prototype.setTime

- 1970 기점으로 경과된 밀리초 설정
  ![image.png](attachment:a1f1d167-808e-4426-9c4f-0690b7820908:image.png)

### 21. Date.prototype.getTimezoneOffset

- UTC 와 Date 객체에 지정된 locale 시간과의 차이를 분 단위로 반환한다. KST 는 UTC 에 9시간을 더한 시간이다. 즉 UTC = KST - 9h

```tsx
console.log(new Date().getTimezoneOffset()); // -540
```

### 22. Date.prototype. toDateString

- 사람이 읽을 수 있는 형식의 문자열로 Date 객체의 날짜를 반환한다.
  ![image.png](attachment:7191c251-b75c-457c-b52c-82ae595dd004:image.png)

### 23. Date.prototype.toTimeString

- 사람이 읽을 수 있는 형식으로 Date 객체의 시간을 표현한 문자열을 반환한다.
  ![image.png](attachment:2c309df5-cad8-48df-b2f7-d3d5124625f2:image.png)

### 24. Date.prototype.toISOString

- ISO 8601 형식으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다.
  ![image.png](attachment:a687bb99-dc76-430d-bfcd-90deb0a2e1cf:image.png)

### 25. Date.prototype.toLocaleString

- 인수로 전달한 locale 을 기준으로 Date 객체의 날짜와 시간을 표현한 문자열을 반환한다. 인수를 생략할 경우 브라우저가 동작 중인 시스템의 locale 을 적용한다.
  ![image.png](attachment:12d13f1f-2096-4990-af2d-e5a5943ac32b:image.png)

### 26. Date.prototype.toLocaleTimeString

- 인수를 전달한 locale 기준으로 Date 객체의 시간을 표현한 문자열 반환한다. 생략하면 브라우저가 알아서
  ![image.png](attachment:dd4d60e0-ad0e-4212-9624-4fe7665d4438:image.png)

# 30.3 Date 를 활용한 시계 예제

---

```tsx
(function printNow() {
  const today = new Date();

  const dayNames = [
    "일요일",
    "월요일",
    "화요일",
    "수요일",
    "목요일",
    "금요일",
    "토요일",
  ];

  // getDay 메서드는 해당 요일(0 ~ 6)을 나타내는 정수를 반환한다.
  const day = dayNames[today.getDay()];

  const year = today.getFullYear();
  const month = today.getMonth() + 1;
  const date = today.getDate();
  let hour = today.getHours();
  let minute = today.getMinutes();
  let second = today.getSeconds();
  const ampm = hour >= 12 ? "PM" : "AM";

  // 12시간제로 변경
  hour %= 12;
  hour = hour || 12; // hour가 0이면 12를 재할당

  // 10 미만일 경우 0 붙이기
  minute = minute < 10 ? "0" + minute : minute;
  second = second < 10 ? "0" + second : second;

  const now = `${year}년 ${month}월 ${date}일 ${day} ${hour}:${minute}:${second} ${ampm}`;

  console.log(now);

  // 1초마다 printNow 함수를 재귀 호출한다.
  setTimeout(printNow, 1000);
})();
```

- 진짜 타이머.
