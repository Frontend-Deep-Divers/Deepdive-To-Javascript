# 32.1 String 생성자 함수

---

- String 생성자 함수에 인수를 전달하지 않고 new 연산자와 함께 호출하면 [[StringData]] 내부 슬롯에 빈 문자열을 할당한 String 래퍼 객체를 생성한다.
- PrimitiveValue ⇒ StringData
- **❗String 래퍼 객체는 배열과 마찬가지로 length 프로퍼티와 인덱스를 나타내는 숫자 형식의 문자열을 프로퍼티 키로, 각 문자를 프로퍼티 값으로 갖는 유사 배열 객체이면서 이터러블이다.**
- 배열처럼 인덱스를 사용해 접근할 수는 있지만, → 원시값이므로 변경할 수는 없다.
- 문자열이 아닌 값을 인수로 전달하면 문자열로 강제로 변환 후 StringData 내부 슬롯에 할당한 String 래퍼 객체를 생성한다.
  ![image.png](attachment:35ef4977-ea58-4dda-80f1-db1ee1cf8598:image.png)
- new 연산자 안쓰면 문자열 반환, 이를 사용해 명시적 타입 변환 가능.
  ![image.png](attachment:c187d370-177d-41f2-8804-354c07d5df91:image.png)

# 32.2 length 프로퍼티

---

- length 프로퍼티는 문자열의 문자 개수를 반환한다.
- String 래퍼는 length 프로퍼티를 가지고 인덱스를 나타내는 숫자를 프로퍼티 키로 가진 유사 배열 객체이다.

# 32.3 String 메서드

---

- String 객체에는 원본 String 래퍼 객체를 직접 변경하는 메서드는 없고, 언제나 새로운 문자열 반환한다.
- **🌟 문자열은 변경 불가능한 원시 값이기 때문에 String 래퍼 객체도 읽기 전용 객체로 제공된다.**
  ![image.png](attachment:8b16624b-6ce1-4a3f-972a-bd11f0f7e342:image.png)

### 1. String.prototype.indexOf

- indexOf 는 대상 문자열에서 인수로 전달 받은 문자열을 검색하여 첫 인덱스를 반환한다.
- 실패하면 - 1 반환
- ❗2번 째 인수로 시작할 인덱스 설정 가능.
- ES6 에 도입된 includes 메서드를 사용하면 가독성이 더욱 좋다!
  ![image.png](attachment:61882a7e-d084-4093-af43-99b9921edbf7:image.png)

### 2. String.prototype.search

- search 메서드는 대상 문자열에서 인수로 전달 받은 정규 표현식과 매치하는 문자열을 검색하여 일치하는 문자열의 인덱스를 반환한다. 실패하면 -1
  ![image.png](attachment:7a9d1ce5-dde0-480d-95e2-d2d29f44927b:image.png)

### 3. String.prototype.includes

- ES6 에 도입됨.
- 인수로 전달 받은 문자열이 포함되어 있는지 확인 여부 true / false
- 두 번째 인수로 검색 시작할 인덱스 설정 가능.
  ![image.png](attachment:1185de44-bc57-41bb-bc67-bfc89c379e77:image.png)

### 4. String.prototype.startWith

- ES6 로 도입된 해당 문자열이 인수로 전달 받은 문자열로 시작하는 지 확인
- true / false
- 2번재 인수로 시작할 인덱스 설정 가능.
  ![image.png](attachment:b73026c0-c27d-40cb-8ab3-441f0fd00a3f:image.png)

### 5. String.prototype.endsWith

- ES6 에서 도입된 해당 메서드는 인수로 전달 받은 문자열로 끝나는지 확인
- true / false
  ![image.png](attachment:4e532592-7efb-445a-87cd-c9a74e1b0ce2:image.png)

### 6. String.prototype.charAt

- 전달 받은 인덱스에 위치한 문자를 검색하여 반환.
- 문자열의 범위가 벗어난 인덱스로 검색하면 빈 문자열 반환.
  ![image.png](attachment:80abbb4c-0bca-4a14-b1ba-9d589a1fffa1:image.png)

### 7. String.prototype.subString

- 부분 문자열 환함.
- 첫 번째 인수 ~ 두 번째 인수 - 1
  ![image.png](attachment:7af0f4c0-b7bb-4124-afc4-faaeb060262c:image.png)
- 두 번째 생략하면 마지막 문자까지
  ![image.png](attachment:32c768a8-7312-4434-a671-b95134df81dc:image.png)
- subString 메서드의 첫 번째 인수는 두 번째 인수보다 작은 정수여야 하지만, 아래 경우에는 정상 동작한다.
  ![image.png](attachment:daf23689-9ccf-42e5-9bb1-877932936bf5:image.png)
  ![image.png](attachment:e2b9a2ac-ab48-4d4d-b82f-35eb7e6c1cda:image.png)

### 8. String.prototype.slice

- subString 과 동일하게 동작하지만 음수를 인수로 전달할 수 있다.
  ![image.png](attachment:bc154b45-f4b1-4e1c-834b-fee30991305d:image.png)

### 9. String.prototype.toUpperCase

- 대상 문자열을 모두 대문자로 변경한 문자열 반환

### 10. String.prototype.toLowerCase

- 대상 문자열 모두 소문자로 변경한 문자열 반환.

### 11. String.prototype.repeat

- ES6 에서 도입됨. 대상 문자열을 전달받은 인수만큼 반복해 연결한 새로운 문자열 반환.
- **❗0 이면 빈 문자열을, 음수면 RangeError 를 발생시킨다. 인수를 생략하면 기본값 0 이다.**
  ![image.png](attachment:2e27ece7-43cd-4807-960b-39fbe737a2e6:image.png)

### 12. String.prototype.replace

- 첫 인수로 받은 문자열 또는 정규식을 두 번째 인수로 치환한 문자열 반환한다.
  ![image.png](attachment:22611197-06b5-4678-97a7-9adcf4ac0f48:image.png)
- 정규 표현식도 전달할 수 있다.
  ![image.png](attachment:dd771b6e-e36a-4cbd-bb16-4126cad19bd9:image.png)
- 두 번째 인수로 치환 함수를 전달할 수 있다. → 첫 번째 인수로 전달한 문자열, 정규 표현식에 매치한 결과를 두 번째 인수로 전달한 치환 함수의 인수로 전달하면서 호출하고 치환 함수가 반환한 결과와 매치 결과를 치환한다.
  ![image.png](attachment:2c197be2-96c6-4fae-897a-1c938279712b:image.png)

### 13. String.prototype.split

- 해당 문자로 구분하여 배열을 반환한다.
- 인수 생략 시 전체 문자열 반환
- 두 번째 인수로 배열 길이 지정 가능.
  ![image.png](attachment:e9c7dca5-1e08-45fc-8816-82229be1583d:image.png)
