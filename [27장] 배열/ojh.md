# 27.1 배열이란?

---

```tsx
const arr = ["a", "b", "c"];
```

- 리터럴로 생성, 가지고 있는 값은 **요소**, 모든 값은 요소 가능
- 0 부터 인덱스
- 대괄호 표기법 접근
- length 프로퍼티 가짐. for 문을 통해 순차적 요소 접근 가능
- 배열 타입은 없고 객체다. intanceof Array 는 true 임.
- typeof arr → object.
- **배열은 리터럴, Array 생성자 함수, Array.of, Array.from 메서드로 생성할 수 있다.**
- 배열의 프로토타입 객체는 Array.prototype, 배열을 위한 빌트인 메서드 제공.

![image.png](attachment:263e6f37-f054-4456-9dbd-84e4b6f46054:image.png)

- 배열은 객체지만 일반 객체와는 구분되는 특징인 값의 순서와 length 프로퍼티가 있다. → 덕분에 순차적으로 값에 접근하기 적합하다.

# 27.2 자바스크립트 배열은 배열이 아니다.

---

- ❗🌟**자료구조에서 흔히 말하는 배열은 동일한 크기의 메모리 공간이 빈틈없이 연속적으로 나열된 자료구조를 말한다. →** 이러한 배열을 **밀접 배열이라 한다.**

![image.png](attachment:99880458-628e-421f-800e-c0f89ddfb83f:image.png)

- 이처럼 일반적인 의미의 배열은 각 요소가 동일한 데이터 크기를 가지며, 빈틈없이 연속적으로 이어져 있으므로 다음과 같이 인덱스를 통해 단 한 번의 연산으로 임의의 요소에 접근할 수 있다. 이는 매우 효율적이고, 고속으로 동작한다.

![image.png](attachment:38d8b0f5-bf30-499e-be02-624e54fe5756:image.png)

- 이처럼 배열은 인덱스를 통해 효율적으로 접근할 수 있다는 장점이 있다.
- 하지만 정렬되지 않은 요소에서는 O(n) 만큼의 시간이 소요됨. 선형 검색.
- 또한 배열에 요소를 삽입하거나 삭제하는 경우 배열의 요소를 연속적으로 유지하기 위해 이동시켜야 한다는 단점도 있다. (중간 요소를)

![image.png](attachment:c8f2e51c-ee75-4a46-a07d-a4a40420b3ef:image.png)

- ❗자바스크립트 배열은 지금까지 살펴본 자료구조에서 말하는 일반적인 배열과 다르다. 🌟배열의 요소를 위한 각각의 메모리 공간은 동일한 크기를 갖지 않아도 되며, 연속적으로 이어져 있지 않을 수 있다. → 이러한 배열을 **희소배열**이라 한다.
- ⚠️ **자바스크립트 배열은 일반적인 배열의 동작을 흉내 낸 특수한 객체이다.**

![image.png](attachment:110515b3-809f-4c1b-83c3-137abbaef2d8:image.png)

- 🌟 자바스크립트 배열은 인덱스를 나타내는 문자열을 프로퍼티 키로 가지며, length 프로퍼티를 갖는 특수한 객체다. 배열 요소는 프로퍼티 값이다. 모든 값이 객체의 프로퍼티 값이 될 수 있는 것처럼 어떤 타입도 배열의 요소가 될 수 있다.
- **❗자바스크립트 배열과 일반 배열의 차이와 장단점**
    - 일반적인 배열은 인덱스로 요소에 빠르게 접근할 수 있지만 요소를 삽입 삭제하는 경우에는 효율적이지 않다.
    - 자바스크립트 배열은 해시 테이블로 구현된 객체이므로 인덱스에 접근하는 경우 일반적인 배열보다 성능적인 면에서 느릴 수 밖에 없지만, 요소를 삽입, 삭제하는 경우에는 일반적인 배열보다 빠른 성능을 기대할 수 있다.
- **자바스크립트 배열은 인덱스로 접근하는 것보다 삽입, 삭제 성능을 택한 것이다. → 왜일까?**
- 인덱스로 배열 요소에 접근하는 것이 일반 배열보다 느리다는 구조적인 단점을 보완하기 위해 대부분의 모던 자바스크립트 엔진은 배열을 일반 객체와 구분하여 좀 더 배열처럼 동작하도록 최적화하여 구현했다.
- **🙀일반 객체보다 약 2배 빠르다..!!!**

```tsx
const arr = [];
console.time("Array Test");

for (let i = 0; i < 100000000; i++) {
  arr[i] = i;
}

console.timeEnd("Array Test");  // 1.3 초

const obj = {};
console.time("Object Test");

for (let i = 0; i < 100000000; i++) {
  obj[i] = i;
}

console.timeEnd("Object Test"); // 2.1초
```

# 27.3 length 프로퍼티와 회소 배열

---

- length 프로퍼티: 요소의 개수, 배열의 길이. 빈 배열이면 0
- length 프로퍼티의 값은 0과 2^32 - 1 미만의 양의 정수다. 최대 이만큼 가질 수 있다.
- 배열 요소를 추가하거나 삭제하면 자동 갱신된다.

![image.png](attachment:f0abee6d-02f7-4fdb-9fd3-c649508f8948:image.png)

- length 프로퍼티에 직접 값을 할당할 수 있다.
- **1️⃣ 작은 숫자 값을 할당하는 경우 → 배열의 길이가 줄어든다.**
    
    ![image.png](attachment:57b8f167-5858-4837-943c-d499d3afafd0:image.png)
    
- **2️⃣ 더 큰 숫자 값을 할당하는 경우 → 늘어나지 않고 empty 라고 출력됨. 값이 없다는 의미.**
    
    ![image.png](attachment:ef7ed9d5-d24c-46c9-bd0f-879530fd0ebe:image.png)
    
    - length 프로퍼티 값은 성공적으로 변경되지만, 실제 배열에는 아무런 변화가 없다. → 즉, 메모리 공간도 확보하지 않고 빈 요소를 생성하지도 않는다.
    
    ![image.png](attachment:69fe9cfe-edc2-4a62-9b29-3304f43cac4d:image.png)
    
- 이처럼 연속적으로 존재하지 않고 일부가 비어있는 것을 회소 배열이라 한다. 자바스크립트는 문법적으로 허용한다.
    
    ![image.png](attachment:d83f9e5c-8bf7-4972-9f87-5f8b39f188fc:image.png)
    
- 일반적인 배열의 length 와 요소의 개수는 언제나 일치한다. 하지만 희소 배열은 일치하지 않는다. 언제나 length 가 요소 개수보다 크다.
- 자바스크립트 문법적으로는 희소 배열을 허용하지만 희소 배열은 사용하지 않는 것이 좋다. 의도적으로 희소 배열을 만들어야 하는 상황은 발생하지 않는다. 연속적인 값의 집합이라는 배열의 개념과 맞지 않고, 성능에도 좋지 않은 영향을 미친다.
- **❗최적화가 잘 되어 있는 모던 자바스크립트 엔진은 요소의 타입이 일치하는 배열을 생성할 때, 일반적인 배열처럼 연속된 메모리 공간을 확보하는 것으로 알려져 있다**. → 🌟 희소 배열을 생성하지 않도록 주의해라. 배열에는 같은 타입의 요소를 연속적으로 위치시키는 것이 최선이다.

# 27.4 배열 생성

---

**1️⃣ 배열 리터럴**

- 배열 리터럴 [ ] 대괄호로 묶는다 , 쉼표와 함께.
- 키가 없고 값만 존재한다.
- 요소를 생략하면 희소 배열 → 주의해라!

**2️⃣ Array 생성자 함수**

- 인수의 개수에 따라 동작이 다르니 주의하자.
1. 전달된 개수가 1개이고 숫자면 length 프로퍼티 값이 인수인 배열을 생성한다. → 이 때 생성된 배열은 희소 배열이다.
    
    ![image.png](attachment:c666258a-e437-45df-adcb-3b5410a1173f:image.png)
    
2. 배열은 요소를 최대 2^32 - 1 개 가질 수 있다. 따라서 Array 생성자 함수에 전달할 인수는 이 범위 안의 정수여야 한다. 벗어나면 RangeError.
    
    ![image.png](attachment:280b3d4b-6389-4bc3-a0e7-dce4b90e56ba:image.png)
    
3. 인수가 없는 경우 빈 배열 생성  === [ ];
    
    ![image.png](attachment:6b22d2f8-76a4-4699-99df-d3ff2165b83e:image.png)
    
4. 전달된 인수가 2개 이상이거나 숫자가 아닌 경우 인수를 요소로 갖는 배열을 생성한다.
    
    ![image.png](attachment:7613828a-b71c-492a-b853-0538800ae9dd:image.png)
    
5. new 연산자와 함께 호출하지 않아도 일반 함수로서 호출해도 생성자 함수로 동작한다. new.target 을 확인하기 때문

**3️⃣ Array.of**

- ES6 에서 도입된 Array.of 메서드는 전달된 인수를 요소로 갖는 배열을 생성한다. Array 생성자 함수가 1개이고 숫자더라도 인수를 요소로 갖는 배열을 생성한다.

![image.png](attachment:62ee4570-de7f-48e6-92e3-d064c285c5a7:image.png)

**4️⃣ Array.from (애용중)**

- ES6 에서 도입됨.
- 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환해 반환한다.
    
    ![image.png](attachment:8ab73b73-92fb-4f24-9f20-e54236695fdb:image.png)
    
- 두 번째 인수로 전달한 콜백 함수를 통해 값을 만들면서 요소를 채울 수 있다. 두 번째 인수를 전달한 콜백 함수에 첫 번째 인수에 의해 생성된 배열의 요소값과 인덱스를 순차적으로 전달하면서 호출하고, 콜백 함수의 반환값으로 구성된 배열을 반환한다. → 내가 많이 쓰는 방식
    
    ![image.png](attachment:2fc1fb06-4db7-40c6-ba22-29dc5220c05e:image.png)
    

![image.png](attachment:08d815a8-f82f-49a7-a4a0-dc96f2d277da:image.png)

# 27.5 배열 요소의 참조

---

- 참조할 때는 [] 대괄호 표기법 인덱스로 참조한다.
- 정수로 평가되는 표현식이라면 인덱스 대신 사용 가능. 객체의 프로퍼티 키와 같은 역할.
- 존재하지 않는 요소에 접근하면 undefined
- 희소 배열도 존재하지 않는 값 참조하면 undefined

![image.png](attachment:3d987c6e-b9d4-4002-9242-b743e919e50a:image.png)

# 27.6 배열 요소의 추가와 갱신

---

- 추가하면 자동으로 length 갱신됨.

![image.png](attachment:28e7a879-123a-4b72-99f8-a63e41a27cc0:image.png)

- 현재 배열의 length 프로퍼티 값보다 큰 인덱스로 새로운 요소를 추가하면 희소 배열이 된다.

![image.png](attachment:c42dd9ab-d8a6-4af9-aedd-c8fee726bd7c:image.png)

- 이 때 인데스로 요소에 접근하여 명시적으로 값을 할당하지 않은 요소는 생성되지 않는다.

![image.png](attachment:64229475-7c34-4ad6-bb84-c3b886cef60f:image.png)

- 이미 존재하는 요소에 값을 재할당하면 요소값 갱신된다.
- ❗**반드시 인덱스는 정수여야 함. 만약 정수 이외의 값을 사용해 할당하면, 요소가 아니라 프로퍼티가 생성된다. 이때 추가된 프로퍼티는 length 에 영향을 주지 않는다**.
    
    ![image.png](attachment:3bd4378a-dc8e-4f2d-9d04-a184f46fef40:image.png)
    

# 27.7 배열 요소의 삭제

---

- 배열 요소는 사실 객체라 특정 요소를 삭제하기 위해 delete 연산자 사용 가능
    
    ![image.png](attachment:311e0223-a1d4-479a-b7f0-5d687fbf03a6:image.png)
    
- delete 연산자는 객체의 프로퍼티를 삭제하는 것이기에 ‘1’ 인 프로퍼티를 삭제한다. 이때 배열은 희소 배열이 되며 length 프로퍼티 값은 변하지 않는다. 따라서 희소 배열을 만드는 delete 연산자는 사용하지 않는 것이 좋다.
- 희소 배열을 만들지 않고 배열의 특정 요소를 완전히 삭제하려면 Array.prototype.splice 메서드 사용.
    
    ![image.png](attachment:cf0b8411-91c9-4d92-800a-14c31c1e1db0:image.png)
    

# 27.8 배열 메서드

---

- 자바스크립트는 배열을 다룰 때 유용한 다양한 빌트인 메서드를 제공한다.
- Array 생성자 함수는 정적 메서드를 제공하며, Array.prototype 프로토타입은 프로토타입 메서드를 제공한다.
- ❗**❗(매우매우 중요)**❗**❗배열 메서드에는 원본 배열을 직접 변경하는 메서드와 직접 변경하지 않고 새로운 배열을 반환하는 메서드가 있다.**

```tsx
const arr =[1];
arr.push(2); // push 는 원본 배열 변경
console.log(arr); // [1,2]

const result = arr.concat(3); // concat 은 새로운 배열 반환
console.log(arr); // [1,2]
console.log(result); // [1,2,3]
```

- ES5 부터 도입된 배열 메서드는 대부분 원본 배열을 직접 변경하지 않지만 초창기 배열 메서드는 원본 배열을 직접 변경하는 경우가 많다. → 🌟 **원본 배열을 직접 변경하는 메서드는 외부 상태를 직접 변경하는 부수효과가 있으므로 사용할 때 주의해야 한다. → 가급적 원본 배열을 직접 변경하지 않는 메서드를 사용하는 편이 좋다.**

### 1. Array.isArray

- Array 의 정적 메서드.
- 전달된 인수가 배열이면 true 아니면 false 반환.
    
    ![image.png](attachment:428ceed1-b786-496a-ab90-d460899122d1:image.png)
    

### 2. Array.prototypeof.indexOf

- 프로토타입 메서드
- 원본 배열에 인수로 전달 요소와 중복되는 요소가 **여러 개 있다면 첫 번째로 검색된 요소**의 인덱스를 반환한다. → **없다면 -1 반환**
    
    ![image.png](attachment:04476a65-176e-4799-b870-a74dd23596c3:image.png)
    
- 배열에 특정 요소가 존재하는지 확인할 때 유용하다.
    
    ![image.png](attachment:496430f7-aa8f-421e-9dca-8e91dca7ac47:image.png)
    
- ES7 에서 도입된 includes 메서드를 사용하면 가독성이 더 좋다.
    
    ![image.png](attachment:f4dfda59-17f6-4084-a300-8ac2086dab6a:image.png)
    

### 3. Array.prototype.push

- 전달 받은 **모든 값**을 원본 배열의 마지막 요소로 추가하고, 변경된 length 프로퍼티 값 반환 ← 몰랐네;;
- 원본 배열 직접 변환
    
    ![image.png](attachment:2a0f04fe-8856-41f8-9a3c-58a76a8fdbfe:image.png)
    
- 🌟❗🌟❗🌟❗**push 메서드는 성능 면에서 좋지 않다… 마지막 요소로 추가할 요소가 하나뿐이라면 push 메서드를 사용하지 않고 length 프로퍼티를 사용하여 배열의 마지막에 요소를 직접 추가하는 것이 push 메서드 보다 빠르다.**
- **[** 🧑‍⚖️ **push 와 마지막 배열에 직접 추가 비교 ]**
    
    ### ✅ 정말 `arr[arr.length] = value`가 더 빠르냐?
    
    **👉 맞아요.** 기술적으로는 `arr[arr.length] = value` 방식이 **조금 더 빠릅니다**.
    
    ```
    js
    복사편집
    arr[arr.length] = 42; // 배열의 끝에 직접 추가
    arr.push(42);         // 내부적으로 함수 호출 발생
    
    ```
    
    - `push()`는 내부적으로 몇 가지 검사를 하고, `length`를 갱신하며 요소를 추가해요.
    - 반면 `arr[arr.length] =`는 그냥 메모리에 쓱 할당해버리는 수준.
    - 그래서 아주 미세하게 빠릅니다. **(나노초 단위의 차이)**
    
    ---
    
    ### ❗ 그럼에도 불구하고 `push()`를 더 많이 쓰는 이유
    
    ### 1. 🔍 **가독성**
    
    ```
    js
    복사편집
    arr.push(42);       // 누구나 이해 가능
    arr[arr.length] = 42; // "이거 뭐지?" 싶은 사람도 있을 수 있음
    
    ```
    
    - 협업 시에는 `push()`가 훨씬 **명확하고 직관적**이에요.
    
    ---
    
    ### 2. 🧠 **안정성**
    
    - `push()`는 배열이 아닌 경우에도 오류를 덜 내고 처리하거나 명확한 예외를 던짐.
    - `length` 직접 조작은 실수로 이상한 값이 들어갈 수도 있어요.
    
    ```
    js
    복사편집
    arr.length = -1;    // 💥 배열이 망가질 수 있음
    
    ```
    
    ---
    
    ### 3. 🔬 **성능 차이는 미세**
    
    실제로 브라우저 콘솔이나 벤치마크로 돌려봐도,
    
    > 100만 번 루프를 돌리지 않는 이상 거의 차이 없음
    > 
    
    즉, 일반적인 사용에서는 **사람이 읽기 쉬운 코드가 더 중요**해요.
    
    ---
    
    ### 🤖 언제 `arr[arr.length] = value`를 써야 할까?
    
    정말 성능이 **극도로 민감한 상황**이면 쓸 수 있습니다.
    
    예를 들어:
    
    - 대용량 배열을 밀리초 단위로 채워야 하는 상황
    - DOM 작업처럼 반복적으로 실행되는 코드의 내부
    
    그 외에는:
    
    > 가독성 좋고 안전한 push()를 쓰는 게 현실적이고 유지보수에 좋습니다.
    > 
    - 근데 반복 횟수가 늘어나면 좀 차이가 나기 시작함. (1000만 정도)
    
    ```tsx
    console.log("===== push() 성능 측정 =====");
    console.time("push");
    let arr1 = [];
    for (let i = 0; i < 100000000; i++) {
      arr1.push(i);
    }
    console.timeEnd("push"); // 3.4s
    
    console.log("===== length 방식 성능 측정 =====");
    console.time("length");
    let arr2 = [];
    for (let i = 0; i < 100000000; i++) {
      arr2[arr2.length] = i;
    }
    console.timeEnd("length"); //1.7s
    ```
    
    ![image.png](attachment:059b9f73-8b97-45e8-b170-0e58bf62f0b9:image.png)
    
- push 메서드는 원본 배열을 변환하는 부수 효과가 있다. 따라서 push 메서드보다는 ES6 스프레드 연산자를 사용하는 편이 좋다. 이걸 사용하면 함수 호출 없이 마지막에 요소를 추가할 수 있으며, 부수 효과도 없다. → **❗다만 이걸 대체한다고 생각하기에는 매번 n 만큼은 시간을 들여서 복사하고 메모리를 차지하기 때문에 불변성이 중요한 경우에만 사용하는 것이 좋아 보인다.**
    
    ![image.png](attachment:8cb73599-bc25-43ba-a714-f951b11a2f14:image.png)
    

### 4. Array.prototype.pop

- 원본 배열의 마지막 요소를 제거하고 제거한 요소를 반환한다. 빈 배열이면 undefined 반환, 원본 배열 직접 변경
    
    ![image.png](attachment:fa2753cf-29fa-46ac-83c0-fc34aa48dca4:image.png)
    
- push 와 pop 을 사용하여 쉽게 스택을 구현할 수 있다.
    
    ```tsx
    const Stack = (function () {
      function Stack(array = []) {
        if (!Array.isArray(array)) {
          // 47장 "에러 처리" 참고
          throw new TypeError(`${array} is not an array.`);
        }
        this.array = array;
      }
    
      Stack.prototype = {
        // 19.1.2절 "생성자 함수에 의한 프로토타입의 교체" 참고
        constructor: Stack,
    
        // 스택의 가장 마지막에 데이터를 밀어 넣는다.
        push(value) {
          return this.array.push(value);
        },
    
        // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼낸다.
        pop() {
          return this.array.pop();
        },
    
        // 스택의 복사본 배열을 반환한다.
        entries() {
          return [...this.array];
        },
      };
    
      return Stack;
    })();
    ```
    
    ![image.png](attachment:c0d82841-634f-4ec5-8d21-63ecf175412a:image.png)
    
    ```tsx
    class Stack {
      #array; // private class member
    
      constructor(array = []) {
        if (!Array.isArray(array)) {
          throw new TypeError(`${array} is not an array.`);
        }
        this.#array = array;
      }
    
      // 스택의 가장 마지막에 데이터를 밀어 넣는다.
      push(value) {
        return this.#array.push(value);
      }
    
      // 스택의 가장 마지막 데이터, 즉 가장 나중에 밀어 넣은 최신 데이터를 꺼낸다.
      pop() {
        return this.#array.pop();
      }
    
      // 스택의 복사본 배열을 반환한다.
      entries() {
        return [...this.#array];
      }
    }
    
    const stack = new Stack([1, 2]);
    console.log(stack.entries()); // [1, 2]
    ```
    

### 5. Array.prototype.unshift

- unshift 메서드는 인수로 전달받은 모든 값을 원본 배열의 선두에 요소로 추가하고 변경된 length 값을 반환한다, 원본 배열 직접 변경
- push 반대임
    
    ![image.png](attachment:7652928a-f495-4059-b1f9-7f260fe681b4:image.png)
    
- 역시 배열을 직접 변경하는 부수 효과가 있어서 스프레드 연산자 사용하는 것이 좋다. (원본 배열 유지를 위해)
    
    ![image.png](attachment:aeaad2d4-584c-4fb7-8c34-ac9b7c04026b:image.png)
    

### 6. Array.prototype.shift

- shift 연산자는 배열에서 첫 번째 요소를 제거하고 제거한 요소 반환한다. 없으면 undefined, 원본 배열 변경
- pop 반대임.
    
    ![image.png](attachment:da1d7ff1-6f56-4e59-a957-d2fc7cfe0823:image.png)
    
- ⚠️ **shift 와 push 를 사용해서 쉽게 큐를 구현할 수 있다고 하지만….. →하지만 push 연산자 O(1) 이지만 shift 는 O(n) 이다 배열 요소를 다 옮겨야 하기 때문에 그래서 자바스크립트에서 queue 를 구현하려면 연결리스트로 구현해야 한다**

### 7. Array.prototype.concat

- concat 메서드는 인수로 전달된 값들을 원본 배열 마지막 요소로 추가한 새로운 배열을 반환한다.
- 원본 배열은 변경되지 않는다.
- ❗만약 인수로 전달된 값이 배열이면 해체하여 새로운 배열의 요소로 추가한다.
    
    ![image.png](attachment:b5a6b5f9-55d0-469e-85f4-99ed26db3c3d:image.png)
    
- push 와 unshift 메서드는 concat 메서드로 대체할 수 있다. 이는 유사하지만 다음과 같은 미묘한 차이가 있다.
    - push 와 unshift 메서드는 원본 배열을 변경하지만 concat 메서드는 새로운 배열을 반환한다. 따라서 push 와 unshift 메서드를 사용할 경우 원본 배열을 반드시 변수에 저장해 두어야 하며, concat 메서드를 사용할 땐 반드시 환경 변수에 할당받아야 한다.
        
        ![image.png](attachment:6a848c3b-2299-420d-8407-b8a84dd69b28:image.png)
        
    - 인수로 전달받은 값이 배열이면 push 와 unshift 는 배열을 그대로 요소로서 추가하지만 concat 은 해체하고 요소들을 추가한다.
        
        ![image.png](attachment:5954f7d2-6f79-4dca-82a6-e23dcf5bd41f:image.png)
        
- concat 메서드는 ES6 스프레드 문법으로 대체할 수 있다.
    
    ![image.png](attachment:79db802f-6a1f-4483-b565-a65d6e7dab9a:image.png)
    
- 결론적으로 push/unshift 메서드와 concat 메서드를 사용하는 대신 ES6 스프레드 문법을 일관성있게 사용하는 것을 권장한다….??
- 🙅 **당연히 그럴리가….**
    
    > “항상 스프레드(...)를 쓰는 게 무조건 좋은 건 아니다. 상황에 따라 장단점을 알고 선택해야 한다.”
    > 
    
    하지만 왜 **스프레드를 일관되게 쓰자는 말이 나왔는지** 그 맥락을 한번 짚어볼게요.
    
    ---
    
    ## ✅ 스프레드 문법이 추천되는 이유들
    
    ### 1. **불변성 유지에 적합**
    
    ```
    js
    복사편집
    const newArr = [...arr, value]; // 기존 arr 변경 ❌
    
    ```
    
    - `push()`, `unshift()`, `concat()`은 원본 배열을 변경할 수도 있음 (`push`, `unshift`는 특히 그렇죠).
    - 함수형 프로그래밍 스타일, React 상태 관리 등에서는 **불변성이 핵심**이므로 스프레드가 더 안전하고 예측 가능함.
    
    ---
    
    ### 2. **일관된 문법 스타일**
    
    - `concat()` 대신 `[...arr1, ...arr2]`
    - `push()` 대신 `[...arr, newItem]`
    - `unshift()` 대신 `[newItem, ...arr]`
    
    → 모두 스프레드 하나로 처리 가능 = **코드 스타일 통일**
    
    ---
    
    ### 3. **명확한 의도 표현**
    
    ```
    js
    복사편집
    const newArr = [...arr, value]; // 새로운 배열을 만들겠다는 의도가 명확
    
    ```
    
    - 가독성 면에서도 좋고,
    - 원본이 바뀌지 않는다는 걸 **명시적으로 보여줌**
    
    ---
    
    ## ❗하지만 단점도 존재
    
    | 단점 | 설명 |
    | --- | --- |
    | ❌ 성능 이슈 | 배열을 복사하고 새로운 배열을 생성함 (O(n)) |
    | ❌ 메모리 사용 증가 | 매번 새 배열 생성 = 가비지 증가 |
    | ❌ 대량 데이터 처리에 부적합 | 반복적으로 처리 시 push보다 비효율 |
    
    ---
    
    ## 🔍 정리: 언제 스프레드가 좋은가?
    
    | 상황 | 추천 방식 |
    | --- | --- |
    | 리액트 상태 업데이트 | ✅ 스프레드 (불변성 유지) |
    | 함수형 프로그래밍 / 순수 함수 | ✅ 스프레드 |
    | 대량 처리 루프 | ❌ `push()` 추천 (성능상 유리) |
    | 작은 배열 병합 | ✅ 스프레드 or `concat()` (큰 차이 없음) |
    
    ---
    
    ## ✅ 결론
    
    > 일관되게 스프레드를 사용하는 건 "불변성"과 "가독성"을 중요하게 여길 때 유효한 전략이에요.
    > 
    > 
    > 하지만 **성능이나 반복 추가가 중요한 경우엔 `push()` 같은 메서드가 더 적합**하니, 상황에 따라 균형 있게 사용하는 게 좋습니다.
    > 

### 8. Array.prototype.splice

- 원본 배열의 중간에 **요소를 추가**하거나 중간에 있는 요소를 제거하는 경우 splice 메서드를 사용한다. → **🌟추가할 수 있는거 몰랐다;;;**
- **splice 메서드의 매개변수 3가지**
    - **start**: 원본 배열 요소를 제거하기 시작할 인덱스, start 만 지정하면 start 부터 모든 요소 삭제, 음수인 경우에는 배열 끝에서의 인덱스임. ( -1 은 마지막 요소)
    - **deleteCount**: start 부터 제거할 요소의 개수, 0 인 경우 아무런 요소도 삭제하지 않는다.
    - **items**: 제거한 위치에 삽입할 요소들의 목록이다. 생략할 경우 원본 배열에서 제거만 함.
    
    ![image.png](attachment:1d2558cf-b488-40e4-980d-46a5383bdd6b:image.png)
    
- ❗splice 두 번째 인수를, 0으로 지정하면 새로운 요소를 삽입만 한다.
    
    ![image.png](attachment:6d07bd12-6b22-4cd8-8889-533b9cbfe40a:image.png)
    
- splice 인수에서 세 번째 인수를 전달하지 않으면 삭제만 한다. 🌟**참고로 제거된 요소들을 반환된다.**
    
    ![image.png](attachment:2ba77518-c20d-4ed8-a968-102e6b82c871:image.png)
    
- splice 두 번째 인수를 생략하면 시작 인덱스부터 모두 삭제함.
    
    ![image.png](attachment:05783468-181a-4cfa-8138-19f54a8c9f11:image.png)
    
- 배열에서 특정 요소를 취득한 다음 제거하는 방법 2가지
    1. indexOf 메서드를 통해 특정 요소 인덱스를 취하고 splice 로 삭제한다.
        
        ![image.png](attachment:8654f26b-322d-4cfd-9593-bcb0ba2e9719:image.png)
        
    2. filter 메서드로 특정 요소 제거. 다만 이건 특정 요소가 여러개인 경우 모두 제거한다.
        
        ![image.png](attachment:59d91f52-f0c1-471b-893a-fb47afc2d2f8:image.png)
        

### 9. Array.prototype.slice

- slice 메서드는 인수로 전달된 범위의 요소들을 복사하여 배열로 반환한다. 원본 배열 변경 안함.
- slice 매개변수
    - start: 복사를 시작할 인덱스. 음수인 경우 배열의 끝에서의 인덱스
    - end: 복사를 종료할 인덱스, 인덱스 이전까지 복사, 생략 가능, 생략 시 기본값 length
    
    ![image.png](attachment:9039f656-76cb-4f0e-9cc1-2d144d744d2f:image.png)
    
- end 생략 시 인덱스 부터 끝까지 복사하여 반환.
- 음수인 경우 배열 끝에서부터 요소 복사하여 반환
    
    ![image.png](attachment:87b972f5-f6f5-4b7c-9c30-dc958de2aa3b:image.png)
    
- ❗slice 메서드의 인수를 모두 생략하면 원본 배열의 복사본 생성. ← 🌟 얕은 복사를 통해 생성됨.
    
    ![image.png](attachment:f748461e-acfe-4b88-8559-6b6b266552f6:image.png)
    
    ![image.png](attachment:7586982f-0ec3-4c2e-b0ec-d5b1a00886d4:image.png)
    
- **🌟 얕은 복사와 깊은 복사 → 얕은 복사는 한 단계 까지만 복사하고, 깊은 복사는 중첩된 객체까지 복사한다.**
    - slice, 스프레드 연산자, Object.assign 메서드 모두 얕은 복사 수행.
    - lodash 라이브러리의 cloneDeep 은 깊은 복사 수행.
- slice 메서드가 복사본 생성하는 것을 이용하여 arguments,HTMLCollection, NodeList 같은 유사 배열 객체를 배열로 변환할 수 있다.
    
    ![image.png](attachment:58785952-2ebb-40f3-aa8b-40e1b7b1cf4d:image.png)
    
- Array.from 메서드를 사용하여 더욱 간단하게 유사 배열 객체를 배열로 변환할 수 있다.
    
    ![image.png](attachment:3bc03704-447c-4838-b56d-01d3b3ed27c2:image.png)
    
- ❗arguments 객체는 유사 배열 객체이면서 이터러블이다. 이터러블 객체는 ES6 스프레드 문법을 사용하여 간단하게 배열로 변환할 수 있다. → set, Map 도 그런데 이터러블인듯.
    
    ![image.png](attachment:af43c482-83f1-4416-8a99-9703b176b96c:image.png)
    

### 10. Array.prototype.join

- join 메서드는 원본 배열의 모든 요소를 문자열로 변환한 후, 구분자로 연결한 문자열을 반환한다. **구분자 생략 가능하며 기본은 , 이다.**
    
    ![image.png](attachment:3af94217-172d-4bd8-aee9-0eeb04d014c9:image.png)
    

### 11. Array.prototype.reverse

- reverse 원본 배열의 순서를 반대로 뒤집는다. 원본 배열 변경. **반환된 값은 변경된 배열이다. (얕은 복사)**
    
    ![image.png](attachment:ba9bdee5-5c13-45d2-9b0b-5ea257194d48:image.png)
    

### 12. Array.prototype.fill

- ES6 부터 도입된 fill 메서드는 인수로 전달받은 값을 처음부터 끝까지 요소로 채운다. 원본 배열 변경.
    - ❗**두 번째 인수로 요소 채우기 시작할 인덱스 전달 가능 → 몰랐다..**
    - ❗**세 번째 인수로 요소 채우기 멈출 인덱스 전달 가능 → 몰랐다..**
    
    ![image.png](attachment:c48e6e22-e213-474c-bdb7-61474ea785ac:image.png)
    
- 배열을 생성하면서 특정 값으로 채울 수 있다. fill 메서드는 요소를 하나의 값만으로 채울 수밖에 없다는 단점이 있다.
    
    ![image.png](attachment:80127458-c606-4cd0-ba27-f91a87f89514:image.png)
    
- Array.from 메서드를 사용하면 두 번째 인수로 전달한 콜백 함수를 통해 요소값을 만들면서 배열을 채울 수 있다. **(배열의 요소, 인덱스) → 되게 자주 사용**
    
    ![image.png](attachment:10c5b01c-b315-4002-92a6-2f2fb2e83d92:image.png)
    

### 13. Array.prototype.includes

- ES7 에서 도입된 includes 메서드는 배열 내에 특정 요소가 포함되어 있는지 여부에 따라 true, false 를 반환한다. 첫 번째 인수로 검색할 대상을 지정한다.
    
    ![image.png](attachment:d7a08e5e-4240-4db1-885d-8cb304a9acf7:image.png)
    
- **❗두 번째 인수로 검색을 시작할 인덱스를 전달할 수 있다. 기본값은 0, 만약 음수를 전달하면 해당 인덱스부터 검색 시작. → 몰랐다..**
    
    ![image.png](attachment:f5c47eac-0fc7-4ec1-ba45-5791cf897466:image.png)
    
- indexOf 메서드를 사용해도 특정 요소가 포함되어 있는지 확인할 수 있지만 indexOf 를 사용하면 -1 인지 확인해야 하고, NaN 은 확인 불가능하다.
    
    ![image.png](attachment:e5a49854-4c19-4c50-b671-fb5313341b7c:image.png)
    

### 14. Array.prototype.flat

- ES10 (2019) 에서 도입된 flat 는 인수로 전달한 깊이만큼 재귀적으로 배열을 평탄화한다. ← **👀처음 본다..**
- 평탄화할  깊이를 인수로 전달할 수 있고, 기본값은 1이다.
- 인수로 Infinity를 전달하면 중첩 배열을 모두 평탄화한다.
    
    ![image.png](attachment:3905a84d-6b9f-4c37-a9a5-2ff3cd8c7aa3:image.png)
    

# 27.9 배열 고차 함수

---

- 고차 함수란 함수를 인수로 전달받거나 반환하는 함수. 자바스크립트의 함수는 일급 객체이므로 전달하고 반환 가능하다.
- 고차 함수는 외부 상태의 변경이나 가변 데이터를 피하고 **불변성을 지향**하는 함수형 프로그래밍에 기반을 두고 있다.
- 함수형 프로그래밍은 순수 함수와 보조 함수의 조합을 통해 로직 내에 존재하는 **조건문과 반복문**을 제거하여 복잡성을 해결하고, **변수의 사용을 억제**하여 상태 변경을 피하려는 패러다임이다.
- 조건문이나 반복문은 로직의 흐름을 이해하기 어렵게하여 가독성을 해치고, 변수는 누군가에 의해 언제든 변경될 수 있어 오류 발생의 근본적인 원인이 될 수 있기 때문이다. 함수형 프로그래밍은 결국 **순수 함수를 통해 부수 효과를 최대한 억제**하여 오류를 피하고 프로그래밍 안정성을 높이려는 노력의 일환이라고 할 수 있다.
- 자바스크립트는 고차 함수를 다수 지원한다. ( 진짜 많다! ). 배열은 유용한 고차 함수를 많이 지원하므로 기억하도록!

### 1. Array.prototype.sort

- sort 요소 정렬하고, 원본 배열 변경함. 정렬된 배열 반환한다.
- 🌟**기본적으로 오름차순 정렬함. (숫자가 아니라 유니코드 코드 포인트 순서) →  아주 중요!  → 숫자도 일시적으로 문자열로 변환 후 정렬한다.**
    
    ![image.png](attachment:9a6f7e19-c517-419b-8a5d-0668f8faac5f:image.png)
    
    ![image.png](attachment:fd061e7e-a01b-40a2-960e-03fefecda0fa:image.png)
    
    ![image.png](attachment:8a352416-ea61-402d-b05c-4dcc2dede546:image.png)
    
    ![image.png](attachment:b9bfcd7a-6e76-4340-b2d8-356ae8a90ef7:image.png)
    
- **숫자든 뭐든 비교하고 싶다면 정렬 순서를 정의하는 비교 함수를 인수로 전달해야 한다. → 비교 반환 값이 0보다 작으면 첫 번째 인수를 우선으로  정렬하고, 0보다 크면 두 번째 인수를 우선하여 정렬하며, 0인경우 정렬하지 않는다.**
    
    ![image.png](attachment:603f16f4-ca03-4896-8e81-db31d2d2b1c3:image.png)
    
- 객체 정렬 예제.
    
    ![image.png](attachment:92c4070a-ccba-40e6-befb-40ea6f2aea00:image.png)
    
- 😄 **sort 메서드의 정렬 알고리즘은 timSort 알고리즘이다. → 병합 정렬 + 삽입 정렬. ← 기존에는 quickSort 였지만, 동일한 요소가 중복된 경우 초기 순서와 변경될 수 있는 불안정한 알고리즘으로 알려저 있기 때문.**

### 2. Array.prototype.forEach

- 함수형 프로그래밍은 순수 함수와 보조 함수의 조합으로 로직 내에 존재하는 조건문과 반복문을 제거하여 복잡성을 제거하고 변수의 사용을 억제하여 상태 변경을 피하려는 프로그래밍 패러다임이다.
- for 반복문은 조건식과 증감식으로 이루어져 있어 함수형 프로그래밍이 추구하는 바와 맞지 않는다.
- forEach 문은 이러한 for 문은 대체할 수 있는 고차함수다. 콜백함수로 전달 받아 반복 호출한다.
    
    ![image.png](attachment:e603b8c0-6df2-4726-8e33-efec8ac359e3:image.png)
    
- ❗❗**forEach 메서드의 콜백 함수는 배열의 요소값과 인덱스, 호출한 배열 자체 3개의 인수를 전달받을 수 있다.**
    - 요소
    - index
    - 자기 자신 this
    
    ![image.png](attachment:f556995f-66be-495e-a87c-ef8681a26b34:image.png)
    
- JSON.stringify: 객체를 문자열로 출력하기 위함. → 뒤에서 자세히
- forEach 자체는 원본 배열을 변경하지 않지만, **콜백함수를 통해 변경할 수 있다.** 반환 값은 언제나 undefined 이다.
    
    ![image.png](attachment:4fbb30cb-7d30-47fd-ae3c-4d38cf6a0b4c:image.png)
    
- forEach 메서드의 두 번째 인수로 콜백 함수 내부에서 사용할 this 객체를 전달할 수 있다. → 아래 예시 처럼.
    
    ![image.png](attachment:4afcd687-c2e2-44f5-9685-c739ac498a14:image.png)
    
- 근데 그냥 화살표 함수 써라.
    
    ![image.png](attachment:66df978f-d8ce-463a-be08-eb96ef427ea2:image.png)
    
- ❗**내부 폴리필을 보자. 내부에서는 for 문을 통해 순회하고 있다. 단, 반복문을 메서드 내부로 은닉하여 로직의 흐름을 이해하기 쉽게 하고 복잡성을 해결한다.**
    
    ![image.png](attachment:2babe9d6-f226-4ff7-b6af-635f64863b49:image.png)
    
- **🌟 forEach 문은 break 나 continue 문을 사용할 수 없다. 즉 빠짐없이 순회하고 중단 불가하다.**
    
    ![image.png](attachment:97f926b5-ef06-4fb6-aa80-125fe5ad3ee5:image.png)
    
- 희소 배열의 경우 존재하지 않는 요소는 애초에 순회 대상에서 제외된다.
- **⚠️ forEach 메서드는 for 문에 비해 성능이 좋지 않지만 가독성은 더 좋다. 그렇기에 대단히 많은 배열을 순회하거나 시간이 오래 걸리는 코드가 아니라면 for 문 대신 forEach 사용할 것을 권장한다.**
- **과연 얼마나 차이날까?**
    
    ```tsx
    const arr = Array(10000000).fill(0);
    
    // for loop
    console.time("for");
    for (let i = 0; i < arr.length; i++) {
      // arr[i] += 1;
    }
    console.timeEnd("for"); // 대략 10ms
    
    // reset
    arr.fill(0);
    
    // forEach
    console.time("forEach");
    arr.forEach((_, i) => {
      // arr[i] += 1;
    });
    console.timeEnd("forEach"); // 대략 60~70ms
    ```
    
    - node.js 환경에서 생각보다 차이가 좀 난다. 다만 절대값이 크지 않기 때문에 가독성과 함수형 패러다임을 위해 그렇게 크지 않은 경우 forEach 를 사용하는 듯.

### 3. Array.prototype.map

- **map 은 이제 콜백 함수의 반환값들로 구성된 새로운 배열을 반환한다. → 참 많이 쓰죠. 원본 배열 변경 안함.**
    
    ![image.png](attachment:aaa6a4cd-ac3a-4219-8571-4fbbe5e7ae85:image.png)
    
- forEach 랑 반환하냐 안하냐의 차이임. → 목적이 forEach 는 단순 반복, map 은 다른 값으로 매핑한 새로운 배열 생성.
- map 으로 반환한 배열의 length는 당연히 기존 배열의 length와 일치.
- 3개의 매개 변수
    - 요소
    - 인덱스
    - this
    
    ![image.png](attachment:8b5d7a91-f218-488d-8650-0ededa5906e7:image.png)
    
- 두 번째 인수로 콜백 함수 내부에서 사용할 this 객체를 전달할 수 있다. → 역시나 ES6의 화살표 함수를 사용해서 메서드 내부에서 사용하는 것이 좋다.
    
    ![image.png](attachment:1f34b8a0-c352-4dd4-9a22-bfb602813035:image.png)
    

### 4. Array.prototype.filter

- **filter 함수는 콜백 함수의 반환값이 true 인 요소들로만 구성된 배열을 반환한다.**❗ **원본 배열 변경하지 않는다.**
    
    ![image.png](attachment:a2518ac9-3997-47a3-8676-08b63922b515:image.png)
    
- 필터링 조건을 만족시키는 새로운 배열을 만들고 싶을 때 사용한다.
- 그렇기에 당연히 length 는 작거나 같다.
- 똑같이 매개변수 3개.
    
    ![image.png](attachment:5d615171-cb44-4b64-a76d-63901f1a0e1c:image.png)
    
- 특정 요소를 제거하기 위해서도 사용할 수도 있죠.
    
    ![image.png](attachment:4fe563c2-52c5-4139-a2d5-d831034de6f3:image.png)
    
- 두 번째 인수로 콜백 함수 내부에서 사용할 this 객체를 전달할 수 있다. → 역시나 ES6의 화살표 함수를 사용해서 메서드 내부에서 사용하는 것이 좋다.

### 5. Array.prototype.reduce

- **reduce 메서드는 콜백 함수의 반환값을 다음 순회 시에 콜백 함수의 첫 번째 인수로 전달하면서 콜백 함수를 호출하여 하나의 결과값을 만들어 반환한다. (설명 굳) , 원본 배열 변경 안함.**
- 즉 4개의 인수이다.
    - ⚠️ **초기값 혹은 콜백 함수 이전의 반환값**
    - 요소
    - 인엑스
    - this

![image.png](attachment:aac599c1-3d16-456b-b124-86441d8eac0a:image.png)

- reduce 메서드는 하나의 결과값을 반환함.
- 모든 요소를 순회하여 하나의 결과값을 구해야 하는 경우에 사용.
- 예제들. 참 다양하게 활용할 수 있다.
    
    ![image.png](attachment:b9946fa2-11f2-46f8-8daa-4b2dc476232c:image.png)
    
    ![image.png](attachment:d223f029-31b1-4b4f-a1ea-c1d4dc800aa3:image.png)
    
- reduce 를 사용해서 map, filter, some, every, find 와 같은 것들을 구현할 수 있다. 참 다양하게 사용할 수 있다.
- **⚠️⚠️ 주의해야 할 사항으로 두 번째 인수로 전달하는 초기값을 생략하지 않는 것이 안전하다**.
    
    ![image.png](attachment:afce566a-6e9a-4b39-a7ee-a62f4fd672b7:image.png)
    
- **아래처럼 객체의 특정 프로퍼티 값을 합산하는 경우를 보자.**
    
    ![image.png](attachment:05a01f4a-d161-42b7-b9dc-0f6ca80f23bc:image.png)
    
- 따로 2번째 인수를 지정해주지 않았기 때문에 초기값인 객체가 처음 acc 가 되고, 이후부터 객체가 아니라 숫자가 전달되기 때문에 문제가 된다.
    
    ![image.png](attachment:354f5d54-9a6b-4009-b70a-a22f85a85ee9:image.png)
    

### 6. Array.prototype.some

- **some 메서드는 자신을 호출한 배열의 요소를 순회하며, 콜백 함수의 값이 단 한 번이라도 true 면 true 를 아니면 false 를 반환한다**.
- ❗단, 배열이 빈 배열인 경우 언제나 false 를 반환한다.
- 약간 만족하는 경우가 있어야 true 인 느낌.
- 역시나 콜백함수 인자 3개
    - 요소
    - 인덱스
    - this
- 두 번째 인수로 콜백 함수 내부에서 사용할 this 객체를 전달할 수 있다. → 역시나 ES6의 화살표 함수를 사용해서 메서드 내부에서 사용하는 것이 좋다.

### 7. Array.prototype.every

- **every 메서드는 콜백 함수의 값이 모두 참이면 true, 아니면 false 를 반환한다.**
- ❗단, 배열이 빈 배열인 경우 언제나 true를 반환한다.
- 약간 만족하지 않는 경우가 있어야 false 인 느낌.
- 역시나 콜백함수 인자 3개
    - 요소
    - 인덱스
    - this
    
    ![image.png](attachment:9956854d-0f4d-4014-b4c0-998fcbcf0e10:image.png)
    
- 두 번째 인수로 콜백 함수 내부에서 사용할 this 객체를 전달할 수 있다. → 역시나 ES6의 화살표 함수를 사용해서 메서드 내부에서 사용하는 것이 좋다.

### 8. Array.prototype.find

- **ES6 에서 도입된 find 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true 인 첫 번째 요소를 반환한다. 없다면 undefined 반환**
- 역시나 콜백함수 인자 3개
    - 요소
    - 인덱스
    - this
    
    ![image.png](attachment:ef280ae5-4f73-4f2b-858b-9608a273432a:image.png)
    
- filter 메서드는 콜백 함수의 호출 결과가 true 인 새로운 배열 반환이고, find 메서드는 콜백 함수의 반환값이 true 인 첫 번째 요소를 반환하는 것임.
- 두 번째 인수로 콜백 함수 내부에서 사용할 this 객체를 전달할 수 있다. → 역시나 ES6의 화살표 함수를 사용해서 메서드 내부에서 사용하는 것이 좋다.

### 9. Array.prototype.findIndex

- **ES6 에서 도입된 findIndex 메서드는 자신을 호출한 배열의 요소를 순회하면서 인수로 전달된 콜백 함수를 호출하여 반환값이 true 인 첫 번째 요소의 인덱스를 반환한다. 없다면 -1 반환**
- 역시나 콜백함수 인자 3개
    - 요소
    - 인덱스
    - this
- 두 번째 인수로 콜백 함수 내부에서 사용할 this 객체를 전달할 수 있다. → 역시나 ES6의 화살표 함수를 사용해서 메서드 내부에서 사용하는 것이 좋다.
    
    ![image.png](attachment:43822686-1b1e-45c0-a4b4-530dae97158d:image.png)
    

### 10. Array.prototype.flatMap

- **ES10 (2019) 에서 도입된 flatMap 메서드는 map 메서드를 통해 생성된 새로운 배열을 평탄화한다. 즉, map 메서드와 flat 메서드를 순차적으로 실행하는 효과가 있다.**
    
    ![image.png](attachment:e0021fb6-c0c0-473f-963c-726d5438a062:image.png)
    
- 단, flat 메서드 마냥 깊이를 지정할 수는 없어서 깊이를 지정하고 싶다면 map 과 flat 같이 호출해야 한다.
    
    ![image.png](attachment:47160ea8-9ff9-4fde-8229-b03ee1a5403b:image.png)