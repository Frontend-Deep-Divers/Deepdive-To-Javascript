# 13장 스코프

## 13.1 스코프란?

- **모든 식별자(변수, 함수, 클래스들의 이름)의 선언된 위치에 의해 다른 코드가 식별자가 자신을 참조할 수 있는 유효범위**
- **식별자가 유효한 범위**
- **자바 스크립트 엔진이 식별자를 검색할 때 사용하는 규칙**
- 스코프가 없다면 프로그램 전체에서 같은 이름을 갖는 변수는 하나만 있어야한다. (좀 번거로움)
- 스코프를 통해 변수 이름의 충돌을 방지하여 같은 이름은 변수를 사용할 수 있게 한다. 이런 개념으로 봤을 때 스코프는 네임스페이스라고 할 수 있다.

## 13.2 스코프의 종류

- 코드는 전역과 지역으로 구분되며 스코프 또한 이렇게 구분된다.

![image.png](13%E1%84%8C%E1%85%A1%E1%86%BC%20%E1%84%89%E1%85%B3%E1%84%8F%E1%85%A9%E1%84%91%E1%85%B3%201bcf78a2bff180678b3cf036433e4ab9/image.png)

- 전역과 전역 스코프
    - 코드의 가장 바깥 영역이 전역이며, 전역에 변수를 선언하면 전역변수가 된다.
    - 전역 변수는 어디서든지 참조할 수 있다.
- 지역과 지역 스코프
    - 함수 몸체 내부를 지역이라고 한다.
    - 지역 변수는 지역 스코프와 하위 지역 스코프에서 유효하다.

## 13.3 스코프 체인

- 함수가 중첩되는것 처럼 함수의 지역 스코프 또한 중첩될 수 있다.
- 스코프가 함수의 중첩에 의해 계층적인 구조를 가지게 된다는 것이다.
- **스코프 체인** : **스코프가 계층적으로 연결된 것**
- **변수를 참조할 때 자바 스크립트 엔진은 스코프 체인을 통해 변수를 참조하는 코드의 스코프에서 시작하여 상위 스코프 방향으로 이동하며 선언된 변수를 검색한다.**
- **상위 스코프에서 유효한 변수는 하위 스코프에서 자유롭게 참조 가능. 하지만 하위 스코프에서 유효한 변수를 상위 스코프에서 참조할 수는 없다.**

## 13.4 함수 레벨 스코프

- **지역 스코프는 코드 블록이 아닌 함수에 의해서 생성된다.** (지역은 함수 몸체 내부를 말하기 때문)
- 대부분의 프로그래밍 언어들은 함수 몸체 뿐 아니라 모든 코드 블록들이 지역 스코프를 만든다. 이런 스코프를 **블록 레벨 스코프**라고 한다.
- **함수 레벨 스코프** : **var 키워드로 선언된 변수는 오로지 함수 몸체만을 지역 스코프로 인정한다는 특성**
    
    ```jsx
    var x = 1;
    
    if(true){
    	//var 키워드로 선언된 변수는 함수의 몸체만을 지역 스코프로 인정하기에
    	//여기에 선언된 x는 전역 변수로 인지되어 중복 선언된다.
    	//이로 인해 x의 변수 값이 변경된다.
    	var x = 10;
    }
    
    x //10
    ```
    
- ES6에서 도입된 `let,` `const` 키워드는 블록 레벨 스코프를 지원한다.

## 13.5 렉시컬 스코프

```jsx
var x = 1;

function foo(){
	var x = 10;
	bar();
}

function bar(){
	console.log(x);
}

foo();
bar();
```

- 위의 실행 결과는 bar 함수의 상위 스코프가 무엇인지에 따라 결정된다.
    - 함수를 어디서 호출했는지에 따라 함수 상위 스코프 결정 (동적 스코프)
    - 함수를 어디서 정의했는지에 따라 함수 상위 스코프 결정 (렉시컬/정적 스코프)
- **자바 스크립트는 렉시컬 스코프를 따른다**.
- **따라서, 함수를 어디서 호출했는지가 아니라 함수를 어디서 정의했는지에 따라 상위 스코프를 결정한다.**
- **함수가 호출된 위치는 상위 스코프 결정에 어떠한 영향도 주지 않는다. 즉, 함수의 상위 스코프는 언제나 자신이 정의된 스코프이다.**
- **함수의 상위 스코프는 함수 정의가 실행될 때 정적으로 결정된다. 함수 정의가 실행되어 생성된 함수 객체는 이렇게 결정된 상위 스코프를 기억한다. 함수가 호출될 때마다 함수의 상위 스코프를 참조할 필욯가 있기 때문**