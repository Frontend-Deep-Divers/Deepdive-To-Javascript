# 10장 객체 리터럴

## 10.1 객체란?

- 자바 스크립트에서 원시 값을 제외한 나머지 값들은 모두 객체다.
- **원시 값은 변경 불가능한 값이지만 객체는 변경 가능한 값이다.**
- 객체는 0개 이상의 프로퍼티로 구성된 집합이며, 프로퍼티는 키, 값(밸류)로 구성된다.
- 자바 스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이 될 수 있으며, 자바 스크립트의 함수는 일급 객체이므로 값으로 취급할 수 있기에 함수 또한 프로퍼티 값이 될 수 있다.
- 프로퍼티 값이 함수일 경우, 이를 메서드라 부른다.
- 객체는 프로퍼티, 메서드로 구성된 집합체이다. 이를 통해 상태와 동작을 하나의 단위로 구조화할 수 있어서 유용하다.
    - 프로퍼티 : 객체의 상태를 나타내는 값
    - 메서드 : 프로퍼티(상태 데이터)를 참조하고 조작할 수 있는 동작

> ❓**일급객체**
> 
> - 변수를 데이터 구조에 할당할 수 있다.
> - 함수의 인자로 전달할 수 있다.
> - 함수의 반환값으로 사용할 수 있다.

## 10.2 객체 리터럴에 의한 객체 생성

- 객체 리터럴은 중괄호 `{}` 내에 0개 이상의 프로퍼티를 정의한다.
- 변수에 할당되는 시점에 자바 스크립트 엔진은 객체 리터럴을 해석해 객체를 생성한다.

## 10.3 프로퍼티

- **객체는 프로퍼티의 집합이며, 프로퍼티는 키와 값으로 구성된다.**
- 프로퍼티 키 : 빈 문자열을 포함하는 모든 문자열 또는 심벌 값
- 프로퍼티 값 : 자바 스크립트에서 사용할 수 있는 모든 값
- 프로퍼티 키는 반드시 식별자 네이밍 규칙을 따라야 하는 것은 아니지만 **식별자 네이밍 규칙을 따르지 않는다면 반드시 따옴표를 사용해야 한다.**

## 10.4 메서드

- 프로퍼티 값이 함수일 경우 일반 함수와 구분하기 위해 메서드라 불린다.
- 자바 스크립트에서 사용할 수 있는 모든 값은 프로퍼티 값이다.
- 자바 스크립트의 함수는 객체(일급 객체)로써, 값으로 취급할 수 있기 때문에 플퍼티 값으로 사용될 수 있다.

## 10.5 프로퍼티 접근

- 프로퍼티에 접근하는 방법은 2가지 이다.
    - 마침표 표기법 : 마침표 프로퍼티 접근 연산자 (`.`)
    - 대괄호 표기법 : 대괄호 프로퍼티 접근 연산자 (`[ … ]`)
- 대괄포 프로퍼티 접근 연산자 내부에 지정하는 프로퍼티 키는 반드시 따옴표로 감싼 문자열 이어야 한다.
- 객체에 존재하지 않는 프로퍼티에 접근하면 undefined를 반환한다.

## 10.6 프로퍼티 값 갱신

- 이미 존재하는 프로퍼티에 값을 할당하면 프로퍼티 값이 갱신된다.

## 10.7 프로퍼티 동적 생성

- 존재하지 않는 프로퍼티에 값을 할당하면 프로퍼티가 동적으로 생성되어 추가된다.

## 10.8 프로퍼티 삭제

- `delete` 연산자는 객체의 프로퍼티를 삭제한다. 피연산자는 프로퍼티 값에 접근할 수 있는 표현식이어야 한다.
- 존재하지 않는 프로퍼티를 삭제하면 아무런 에러 없이 무시된다.

## 10.9 ES6에서 추가된 객체 리터럴의 확장 기능

- 객체 리터럴의 프로퍼티는 프로퍼티 키 - 값으로 구성된다.
- 프로퍼티 축약 표현
    - ES6에서 프로퍼티 값으로 변수를 사용하는 경우 변수 이름과 프로퍼티 키가 동일한 이름일 때 프로퍼티 키를 생략할 수 있다. 이때 프로퍼티 키는 변수 이름으로 자동 생성 된다.
    
    ```jsx
    let x = 1, y = 2;
    
    const obj = {x, y}; //프로퍼티 축약 표현
    
    console.log(obj); // {x: 1, y: 2}
    ```
    
- 계산된 프로퍼티 이름
    - 문자열 또는 문자열로 타입 변환할 수 있는 값으로 평가되는 표현식을 사용해 프로퍼티 키를 동적으로 생성할 수 있다.
        
        ```jsx
        var prefix = 'prop';
        var i = 0;
        var obj = {};
        
        obj[prefix+ '-' + ++i] = i; //계산된 프로퍼티 이름으로 프로퍼티 키 동적 생성
        
        obj //{prop-1:1,...}
        ```
        
- 메서드 축약 표현
    - ES6부터는 메서드를 정의할 때 function 키워드를 생략한 축약 표현을 사용할 수 있다.
    
    ```jsx
    const obj = {
    	name : 'Lee',
    	//일반 함수 표현
    	sayHi : function(){
    		console.log('Hi! '+this.name);
    	}
    	//메서드 축약 표현
    	sayHi() {
    		console.log('Hi! '+this.name);
    	}
    };
    ```