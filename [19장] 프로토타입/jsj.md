# 19장 프로토타입

- 자바 스크립트는 명령형, 함수형, 프로토타입 기반, 객체지향 프로그래밍을 지원하는 멀티 패러다음 프로그래밍 언어다.

> ❓ 명령형, 함수형, 프로토타입기반, 객체지향 프로그래밍
> 

## 19.1 객체지향 프로그래밍

- 전통적인 명령형 프로그래밍에서 벗어나 객체의 집합으로 프로그램을 표현하려는 프로그래밍 패러다임
- 실세계의 사물이나 개념을 프로그래밍에 접목하고자 하며, 이들은 특징이나 성질을 나타내는 **속성**을 가지고 있어 인식하거나 구별할 수 있다.
- 추상화 : 다양한 속성중에서 프로그램에 필요한 속성만 뽑아내는 것
- 객체는 상태 데이터와 동작을 하나의 논리적인 단위로 묶은 복합적인 자료구조이다.

> ❓객체지향 4가지 특징
> 

## 19.2 상속과 프로토타입

- 상속
    - 객체지향의 핵심 개념
    - 어떤 객체의 프로퍼티 또는 메서드를 다른 객체가 상속받아 그대로 사용할 수 있음
    - 자바 스크립트는 프로토타입을 기반으로 상속을 구현하여 불필요한 중복을 제거
- 생성자 함수로 생성된 인스턴스들은 동일한 메서드를 중복 소유하게 되고, 이는 메모리를 불필요하게 낭비한다.
- 상속을 통해 불필요한 중복을 제거할 수 있다. (자바 스크립트는 프로토타입으로 상속을 구현하기 때문)
- 자신의 상태를 나타내는 프로퍼티 등만 개별적으로 소유하고 내용이 동일한 메서드는 상속을 토해 공유하여 사용한다.

## 19.3 프로토타입 객체

- 어떤 객체의 상위(부모)객체의 역할을 하는 객체로서 다른 객체에 공유 프로퍼티(메서드 포함)을 제공한다.
- 프로토타입을 상속받은 하위(자식) 객체는 상위 객체의 프로퍼티를 자신의 프로퍼티 처럼 자유롭게 사용할 수 있다.
- 모든 객체는 `[[Prototype]]` 라는 내부 슬롯을 가지며, 이 내부 슬롯의 값은 프로토타입의 참조다.
- 모든 객체는 하나의 프로토타입를 가지며, 모든 프로토타입은 생성자 함수와 연결되어 있다.
    
    ![image.png](19%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%90%E1%85%A9%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%201bff78a2bff180fabe3af53b958495bf/image.png)
    
    이를 통해 `[[Prototype]]` 내부 슬롯에는 직접 접근할 수 없지만, `__ proto__` 접근자 프로퍼티를 통해 자신의 프로토타입에 간접 접근할 수 있다.
    프로토 타입은 자신의 `constructor` 프로퍼티를 통해 생성자 함수에 접근할 수 있고, 생성자 함수는 자신의 `prototype` 프로퍼티를 통해 프로토타입에 접근할 수 있다.
    
- `__ proto__`  접근자 프로퍼티
    - 모든 객체는 `__ proto__` 접근자 프로퍼티를 통해 자신의 프로토타입 ( `[[Prototype]]` 내부 슬롯)에 간접적으로 접근할 수 있다.
    - `__ proto__` 접근자 프로퍼티를 통해 프로토타입에 접근하는 이유는 상호 참조를 통해 프로토타입 체인이 생성되는 것을 방지하기 위해서이다.
    - **함수 객체만이 소유하는 `prototype` 프로퍼티는 생성자 함수가 생성할 인스턴스의 프로토타입을 가리킨다.**
    - **모든 객체가 가지고 있는** `__ proto__` **접근자 프로퍼티와 함수 객체만이 가지고 있는 prototype 프로퍼티는 결국 동일한 프로토타입을 가리킨다.**  하지만 이들은 프로퍼티를 사용하는 주체가 다르다.
        
        ![image.png](19%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%90%E1%85%A9%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%201bff78a2bff180fabe3af53b958495bf/image%201.png)
        

## 19.4 리터럴 표기법에 의해 생성된 객체의 생성자 함수와 프로토타입

- 생성자 함수가 아닌 리터럴 표기법에 의해 생성된 객체도 프로토타입이 존재하는데, 이 경우 프로토타입의 `constructor` 프로퍼티가 가리키는 생성자 함수가 반드시 객체를 생성한 생성자 함수라고 단정할 수는 없다.
- **프로토타입과 생성자 함수는 단독으로 존재할 수 없고 언제나 쌍으로 존재한다.**

## 19.5 프로토타입의 생성 시점

- **프로토타입은 생성자 함수가 생성되는 시점에 더불어 생성된다.**
- 내부 메서드 `[[Construct]]` 를 갖는 함수 객체 (화살표 함수나 ES6의 메서드 축약 표현이 아닌 함수 선언문, 함수 표현식으로 정의한)는 `new` 연산자와 함께 생성자 함수로 호출할 수 있다.
- **생성자 함수로서 호출할 수 있는 함수, 즉 `constructor`는 함수 정의가 평가되어 함수 객체를 생성하는 시점에 프로토타입도 더불어 생성된다.**
- 생성자 함수로서 호출할 수 없는 함수는 프로토타입이 생기지 않는다..
- 빌트인 생성자 함수 (Object, String 등)도 일반 함수와 마찬가지로 빌트인 생성자 함수가 생성되는 시점에 프로토타입이 생성된다. 모든 빌트인 생성자 함수는 전역 객체가 생성되는 시점에 생성되기 때문. 생성된 프로토 타입은 빌트인 생성자 함수의 `prototype` 프로퍼티에 바인딩된다.
- 객체가 생성되기 이전에 생성자 함수와 프로토타입은 이미 객체화 하여 존재한다. **이후 생성자 함수 또는 리터럴 표기법으로 객체를 생성하면 프로토타입은 생성된 객체의 `[[Prototype]]` 내부 슬롯에 할당된다.** 이를 통해 객체는 프로토타입을 상속받는다.

## 19.6 객체 생성 방식과 프로토타입의 결정

- 다양한 방법으로 생성된 객체는 각 방식마다 세부적인 객체 생성 방식의 차이는 있으나 추상 연산에 의해 생성된다는 공통점이 있다.
- 객체 리터럴인 경우
    - 자바 스크립트 엔진은 객체 리터럴을 평가하여 객체를 생성할 때 추상 연산 OrdinaryObjectCreate를 호출한다. 이때 전달되는 프로토타입은 Object.prototype이다.
    - 객체 리터럴에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게되어 Object.prototype을 상속받는다.
- Object 생성자 함수인 경우
    - 마찬가지로  추상 연산 OrdinaryObjectCreate를 호출한다.
    - 따라서 Object 생성자 함수에 의해 생성된 obj 객체는 Object.prototype을 프로토타입으로 갖게되어 Object.prototype을 상속받는다.
    - 하지만 객체 리터럴과 달리 프로퍼티를 추가하는 방식에 차이가 있다.
        - 객체 리터럴 : 객체 리터럴 내부에 프로퍼티 추가
        - Object 생성자 함수 : 빈 객체를 생성하고 프로퍼티 추가
- 생성자 함수인 경우
    - 마찬가지로 추상 연산 OrdinaryObjectCreate를 호출한다.
    - 하지만 이때 전달되는 프로토타입은 생성자 함수의 prototype 프로퍼티에 바인딩 되어있는 객체다.
    - 생성자 함수에 의해 생성되는 객체의 프로토타입은 위와 다르게 생성자 함수의 prototype 프로퍼티에 바인딩되어 있는 객체이다.
    - 프로토타입도 객체이기 때문에 일반 객체와 같이 프로토타입에도 프로퍼티를 추가/삭제 할 수 있으며 하위 객체가 상속 받을 수 있다. 또한 이는 프로토타입 체인에 즉각 반영된다.

## 19.7 프로토타입 체인

- **자바 스크립트에서 객체의 프로퍼티에 접근하려고 할 때 해당 객체에 해당 프로퍼티가 없으면 `[[Prototype]]` 내부 슬롯의 참조를 따라 자신의 부모 역할을 하는 프로토타입의 프로퍼티를 순차적으로 검색하는 것을 말한다.**
- **이것이 자바스크립트가 객체지향 프로그래밍의 상속을 구현하는 메커니즘이다.**
- **프로토타입 최상위에 위치하는 객체는 언제나 Object.prototype이다.**
- 프로토타입 체인은 상속과 프로퍼티 검색을 위한 메커니즘
- 스코프 체인은 식별자 검색을 위한 메커니즘

## 19.8 오버라이딩과 프로퍼티 섀도잉

- 프로토타입이 소유한 프로퍼티(메소드 포함)을 프로토타입 프로퍼티, 인스턴스가 소유한 프로퍼티를 인스턴스 프로퍼티라고 한다.
- 프로토타입 프로퍼티와 같은 이름의 프로퍼티를 인스턴스에 추가하면 프로토타입 체인을 따라 검색하여 프로토타입 프로퍼티를 덮어쓰는 것이 아니라, 인스턴스 프로퍼티로 추가한다.
- 이렇게 인스턴스 프로퍼티가 프로토타입 프로퍼티를 오버라이딩하면서 프로토타입 프로퍼티가 가려지게 되는데, 이렇게 상속 관계에 의해 프로퍼티가 가려지는 현상을 **프로퍼티 섀도잉**이라고 한다.
- 프로퍼티 삭제의 경우도 프로토타입 프로퍼티가 아닌 인스턴스 프로퍼티가 삭제되게 된다.
- 하위 객체를 통해 프로토타입의 프로퍼티를 변경/삭제하는 것은 불가능하기 때문이다. (하위 객체를 통해 get은 되어도 set 액세스는 허용되지 않음)

## 19.9 프로토타입의 교체

- 생성자 함수에 의한 프로토타입 교체
    - 생성자 함수의 프로토타입에 다른 객체를 할당하여 교체가 가능하다.
    
    ```jsx
    const Person = (function (){
    	function Person(name){
    		this.name = name;
    	}
    	
    	//생성자 함수의 prototype 프로퍼티를 통해 프로토타입을 객체리터럴로 교체
    	Person.prototype = {
    		sayHello(){
    			console.log(`Hi, my name is ${this.name}`);
    		}
    	};
    	
    	return Person;
    }());
    
    const me = new Person('Lee');
    ```
    
    - 위처럼 프로토타입으로 교체한 객체 리터럴에는 constructor 프로퍼티가 없다. (자바 스크립트 엔진이 프로토타입을 생성할 때 암묵적으로 추가한 프로퍼티이기 때문)
    - 따라서 위의 me 객체의 생성자 함수를 검색하면 Person이 아닌 Object가 나온다. (constructor 프로퍼티와 생성자 함수간의 연결이 파괴되었기 때문)
    - 이를 위해 아래처럼 constructor 프로퍼티를 추가하여 프로토타입의 constructor 프로퍼티를 되살릴 수 있다.
        
        ```jsx
        Person.prototype = {
        		constructor : Person,
        		sayHello(){
        			console.log(`Hi, my name is ${this.name}`);
        		}
        	};
        ```
        
- 인스턴스에 의한 프로토타입 교체
    - 생성자 함수의 prototype 프로퍼티 말고 인스턴스의 `__ proto__` 접근자 프로퍼티 (또는 Object.getPrototypeOf 메서드)를 통해 접근할 수 있으며, 프로토타입을 교체할 수도 있다.
    - 생성자 함수의 prototype 프로퍼티에 다른 임의의 객체를 바인딩하는 것은 미래에 생성할 인스턴스의 프로토타입을 교체하는 것이지만, `__ proto__` 접근자 프로퍼티를 통해 교체하는 것은 이미 생성된 객체의 프로토타입을 교체하는 것이다.
        
        ![image.png](19%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%90%E1%85%A9%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%201bff78a2bff180fabe3af53b958495bf/image%202.png)
        

## 19.10 instanceof 연산자

- instanceof 연산자는 다음과 같이 쓰인다.
    
    ```jsx
    객체 instance of 생성자 함수
    //우변이 함수가 아닌 경우 TypeError 발생
    ```
    
- **우변의 생성자 함수의 prototype에 바인딩된 객체가 좌변의 객체의 프로토타입 체인 상에 존재하면 true로, 그렇지 않으면 false로 평가된다.**
- instanceof 연산자는 프로토타입의 constructo 프로퍼티가 가리키는 생성자 함수를 찾는 것이 아닌, **생성자 함수의 prototype에 바인딩된 객체가 프로토타입 체인 상에 존재하는지 확인한다.**
- 이로 인해 생성자 함수에 의해 프로토타입이 교체되어 constructor 프로퍼티와 생성자 함수간 연결이 파괴되어도 생성자 함수의 prototype 프로퍼티와 프로토타입의 ㅇ연결은 그대로이므로 instanceof 는 영향을 받지 않는다.

## 19.11 직접 상속

- Object.create에 의한 직접 상속
    - Object.create 메서드는 명시적으로 프로토타입을 지정하여 새로운 객체를 생성한다. 또한 추상 연산 OrdinaryObjectCreate를 호출한다.
    - 메서드의 첫번째 매개변수로 생성할 객체의 프로토타입으로 지정할 객체를 전달한다. (두번째 매개변수는 생성할 객체의 프로퍼티 키와 프로퍼티 디스크립터 객체로 이뤄진 객체를 전달한다. 근데 이는 옵션이다..)
    - 이를 통해 첫 매개변수 객체의 프로토타입 체인에 속하는 객체를 생성하고, 객체를 생성하면서 직접적으로 상속을 구현한다.
    - 이 방법의 장점
        - new 연산자 없이도 객체를 생성할 수 있다.
        - 프로토타입을 지정하면서 객체를 생성할 수 있다.
        - 객체 리터럴에 의해 생성된 객체도 상속받을 수 있다.
- 객체 리터럴 내부에서 `__ proto__` 에 의한 직접 상속
    
    ```jsx
    const myProto = { x: 10};
    
    const obj = {
    	y : 20,
    	//객체리터럴로 객체를 생성하면서 __ proto__ 접근자 프로퍼티를 통해 객체를 직접 상속받을 수도 있다.
    	__proto__: myProto
    };
    //위 코드는 아래와 동일하다.
    const obj = Object.create(myProto, {
    	y: {value:20, writable: true, enumerable: true, configurable: true}
    });
    ```
    

## 19.12 정적 프로퍼티/메서드

- 생성자 함수로 인스턴스를 생성하지 않아도 참조/호출할 수 있는 프로퍼티/메서드
- 정적 프로퍼티/메서드는 생성자 함수로 생성한 인스턴스가 참조/호출할 수는 없다.
(정적 프로퍼티/메서드는 인스턴스의 프로토타입 체인에 속한 객체의 프로퍼티/메서드가 아니기 때문)
    
    ![image.png](19%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%91%E1%85%B3%E1%84%85%E1%85%A9%E1%84%90%E1%85%A9%E1%84%90%E1%85%A1%E1%84%8B%E1%85%B5%E1%86%B8%201bff78a2bff180fabe3af53b958495bf/image%203.png)
    
- 인스턴스/프로토타입 메서드 내에서 this를 사용하지 않는다면 그 메서드는 정적 메서드로 변경할 수 있다.

## 19.13 프로퍼티 존재 확인

- in 연산자
    - 객체 내에 특정 프로퍼티가 존재하는지 여부를 확인한다.
    - 사용법
        
        ```jsx
        key in object
        ```
        
    - 확인 대상 객체의 프로퍼티 뿐만 아니라 해당 객체가 상속받는 모든 프로토타입의 프로퍼티를 확인한다!
    - in 대신 ES6에서 도입된 Reflect.has 메서드 또한 동일하게 동작한다.
- Object.prototype.hasOwnProperty 메서드
    - 이 메서드 또한 객체에 특정 프로퍼티가 존재하는지 확인할 수 있다.
    - 사용법
    
    ```jsx
    Object.hasOwnProperTy('name');
    //name이라는 프로퍼티 키가 객체 고유의 프로퍼티 키이면 true
    //상속받은 프로토타입의 프로퍼티 키인 경우 false 반환
    ```
    

## 19.14 프로퍼티 열거

- for…in 문
    - 객체의 모든 프로퍼티를 순회하며 열거한다.
        
        ```jsx
        for (변수 선언문 in 객체) {...}
        ```
        
    - 해당 객체의 프로퍼티 뿐만 아니라 상속받은 프로토타입의 프로퍼티까지 열거한다.
    - 하지만 열거할수 없도록 정의된 프로퍼티 (`[[Enumberable]]` 값이 false)의 경우 열거되지 않는다.
    - **for…in 문은 객체의 프로토타입 체인 상에 존재하는 모든 프로토타입의 프로퍼티 중에서 프로퍼티 어트리뷰트** `[[Enumberable]]` **의 값이 true인 프로퍼티를 순회하며 열거한다.**
- Object.keys/values/entries 메서드
    - 이 메서드를 통해 객체 자신의 프로퍼티인지 확인하는 추가 처리가 가능하다.
    - 객체 자신의 열거 가능한 프로퍼티 키를 배열로 반환한다.
    - ES8부터는 프로퍼티 키와 값의 쌍의 배열을 배열에 담아 반환한다.