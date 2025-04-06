- 표준 빌트인 객체 Math 는 수학적인 상수와 함수를 위한 프로퍼티와 메서드를 제공한다.
- ❗Math 는 생성자 함수가 아니고, 정적 프로퍼티와 정적 메서드만을 제공한다.

# 29.1 Math 프로퍼티

---

**1️⃣ Math.PI**

- 원주율 PI 값 반환

![image.png](attachment:77d5d074-b5a0-4861-8f92-8e88419fd48f:image.png)

# 29.2 Math 메서드

---

### 1. Math.abs

- 절대값 반환.
- 반드시 0 또는 양수
  ![image.png](attachment:afa58352-2c51-4489-99f8-32732e45f72b:image.png)

### 2. Math.round

- 인수로 전달된 숫자의 소숫점 이하를 반올림한 정수를 반환한다.
  ![image.png](attachment:8e3f4827-1728-4eec-abd5-a22e8adff730:image.png)

### 3. Math.ceil

- 인수로 전달된 숫자의 소숫점 이하를 올림한 정수를 반환한다.
- 1.4 → 2 , -1.4 → 1
  ![image.png](attachment:3d52f159-5265-490a-863b-67c853ab0f3a:image.png)

### 4. Math.floor

- 내림.
  ![image.png](attachment:28d75082-b72f-43fd-b34d-a921157fa31d:image.png)

### 5. Math.sqrt

- 제곱근
  ![image.png](attachment:4633855b-2f5d-45b9-8d38-2ff0393fc8d4:image.png)

### 6. Math.randon

- 임의의 난수 생성
- 범위는 0~1 미만의 실수다.
- 🌟 0 포함, 1 미포함.
  ![image.png](attachment:c7794a96-ca2a-4c60-9d3f-e106bf5b5b47:image.png)

### 7. Math.pow

- 첫 번째 인수를 밑, 두 번째 인수를 지수로 거듭 제곱한 결과를 반환한다.
  ![image.png](attachment:1f831ce3-4803-4664-a95e-b21c9a695642:image.png)
- ES7 도입된 지수 연산자 \*\* 을 사용하면 가독성이 더 좋다.
  ![image.png](attachment:e4d0c8f4-1bd4-4be1-b469-1a75fedee2c5:image.png)

### 8. Math.max

- 가장 큰 수 반환
- ❗인수 없으면 -Infinity 반환
- 배열은 스프레드 연산자.
  ![image.png](attachment:a4d141b7-27f8-4f99-8b10-e413faffde97:image.png)

### 9. Math.min

- 가장 작은 수 반환
- 인수 없으면 Infinity
- 배열 스프레드 연산자.
