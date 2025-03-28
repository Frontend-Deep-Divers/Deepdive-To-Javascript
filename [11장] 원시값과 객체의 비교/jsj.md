# 11장 원시 값과 객체의 비교

- 원시 타입과 객체 타입의 차이
    - **원시 타입의 값 (원시값)은 변경 불가능한 값이다. 객체 타입의 값 (객체)는 변경 가능한 값이다.**
    - **원시 값을 변수에 할당하면 변수에는 실제 값이 저장된다. 객체를 변수에 할당하면 변수에는 참조 값이 저장된다.**
    - **값에 의한 전달** : 원시 값을 갖는 변수를 다른 변수에 할당하면 원본의 원시 값이 복사되어 전달된다.
    하지만 이건 사실 값을 전달하는 것이 아니라 메모리 주소를 전달하는 것이다. 전달된 메모리 주소를 통해 메모리 공간에 접근하여 값을 참조할 수 있다. 
    두 변수가 같은 원시 값을 참조하다가 어느 한쪽의 변수에 재할당이 이뤄졌을 때 비로소 새로운 메모리 공간에 재할당된 값을 저장한다.
    - **참조에 의한 전달** : 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달된다.

## 11.1 원시값

- **원시 값은 변경 불가능한 값이다. 변수가 아니라 변수에 할당된 값이 변경 불가능하다.**
- 변수는 재할당을 통해 변수 값을 변경(엄밀히 말하면 교체)할 수 있다.
- 상수는 재할당이 금지된 변수를 말한다.
- **불변성** : 원시 값은 변경 불가능한 값이기에 값을 직접 변경하는 것이 아닌 새로운 메모리 공간을 확보하고 재할당한 값을 저장해야함.
- **불변성을 갖는 원시 값을 할당한 변수는 재할당 이외에 변수 값을 변경할 수 있는 방법이 없다.**
    
    ![image.png](11%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8B%E1%85%AF%E1%86%AB%E1%84%89%E1%85%B5%20%E1%84%80%E1%85%A1%E1%86%B9%E1%84%80%E1%85%AA%20%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B4%20%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD%201bcf78a2bff180dfb57be5221283d1cf/image.png)
    

- 문자열과 불변성
    - 문자열은 다른 원시 값과 다르게 몇 개의 문자로 이뤄졌느냐에 따라 필요한 메모리 공간의 크기가 결정된다.
    - 이러한 이유로 C는 문자열을 문자의 배열로, java는 String 객체로 처리한다.
    - 자바 스크립트의 경우 개발자의 편의를 위해 원시 타입인 문자열 타입을 제공한다. 이는 원시 타입이며 변경 불가능하다.
    - 문자열은 유사 배열 객체이면서 이터러블이므로 배열과 유사하게 각 문자에 접근할 수 있다.
        
        ```jsx
        var str = 'string';
        str[0] = 'S'; //대문자로 변경 시도
        console.log(str); //string => 문자열이 원시 값이므로 불변
        ```
        
    
    > ❓ **유사 배열 객체**
    마치 배열처럼 인덱스로 프로퍼티 값에 접근할 수 있고 length 프로퍼티를 갖는 객체를 말한다. 
    문자열은 마치 배열처럼 인덱스를 통해 각 문자에 접근할 수 있으며, length 프로퍼티를 갖기 때문에 유사 배열 객체이고, for문으로 순회할 수도 있다.
    > 
    
    > ❓ 이터러블
    반복(iteration)할 수 있는 객체를 의미함 (`for…of`, `forEach`, `filter` 등등)
    배열, 문자열, Map, Set 등등
    > 

## 11.2 객체

- **객체 타입의 값, 객체는 변경 가능한 값이다**.
- 객체를 할당한 변수가 기억하는 메모리 주소를 통해 메모리 공간에 접근하면 참조 값에 접근할 수 있다. 참조값은 객체가 저장된 메모리 공간의 주소 그 자체다.

![image.png](11%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%8B%E1%85%AF%E1%86%AB%E1%84%89%E1%85%B5%20%E1%84%80%E1%85%A1%E1%86%B9%E1%84%80%E1%85%AA%20%E1%84%80%E1%85%A2%E1%86%A8%E1%84%8E%E1%85%A6%E1%84%8B%E1%85%B4%20%E1%84%87%E1%85%B5%E1%84%80%E1%85%AD%201bcf78a2bff180dfb57be5221283d1cf/image%201.png)

- 원시 값을 할당한 변수를 참조하면 → 메모리에 저장되어 있는 원시 값에 접근
- 객체를 할당한 변수를 참조하면 → 메모리에 저장되어 있는 참조 값을 통해 실제 객체에 접근
- **원시 값은 변경 불가능한 값이므로 원시 값을 갖는 변수의 값을 변경하려면 재할당 외에는 방법이 없다. 하지만 객체는 변경 가능한 값이므로 재할당 없이 객체를 직접 변경할 수 있다.**
- **재할당 없이 프로퍼티를 동적으로 추가할 수도 있고, 프로퍼티 값을 갱신, 삭제 할 수 있다.**
- 원시값과 다르게 **여러 개의 식별자가 하나의 객체를 공유할 수 있다.**
    - 객체를 가리키는 변수를 다른 변수에 할당하면 원본의 참조 값이 복사되어 전달되게 됨 (참조에 의한 전달)
    - 이로 인해 원본과 사본 모두 동일한 객체를 가리키게 되며 **두 개의 식별자가 하나의 객체를 공유하게 됨**
- **값에 의한 전달 과 참조에 의한 전달 은 식별자가 기억하는 메모리 공간에 저장되어 있는 값을 복사해서 전달한다는 면에서 동일**하지만 변수에 저장되어 있는 값이 원시 값이냐 참조값이냐의 차이이다.
- **자바 스크립트에는 참조에 의한 전달은 존재하지 않고 값에 의한 전달만 존재한다.**

> ❓**얕은 복사와 깊은 복사**
> 
> - 객체를 프로퍼티 값으로 갖는 객체의 경우
>     - 얕은 복사 : 한 단계까지만 복사하는 것
>     - 깊은 복사 : 객체에 중첩되어 있는 객체까지 모두 복사
> 
> 얕은 복사와 깊은 복사로 생성된 객체는 원본과는 다른 객체다. 서로 참조 값이 다른 별개의 객체다. 허나,
> 얕은 복사는 객체에 중첩되어 있는 객체의 경우 참조 값을 복사,
> 깊은 복사는 객체에 중첩되어 있는 객체까지 모두 복사하여 완전한 복사본 생성
>