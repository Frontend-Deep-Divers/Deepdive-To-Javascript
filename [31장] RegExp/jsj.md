# 31장 RegExp

## 31.1 정규 표현식이란?

- 일정한 패턴을 가진 문자열의 집합을 표현하기 위해 사용하는 형식 언어
- 패턴 매칭 기능
    - 특정 패턴과 일치하는 문자열을 검색하거나 추출, 치환할 수 있는 기능
    - 정규 표현식은 문자열을 대상으로 이 기능을 제공

## 31.2 정규 표현식의 생성

- 정규 표현식 객체를 생성하기 위해서는 정규 표현식 리터럴과 RegExp 생성자 함수를 사용할 수 있다.
- 정규 표현식 리터럴
    
    ![image.png](31%E1%84%8C%E1%85%A1%E1%86%BC%20RegExp%201d0f78a2bff18089860bfba82ab5c6ff/image.png)
    
    - 패턴과 플래그로 구성된다.
- RegExp 생성자 함수
    
    ```jsx
    new RegExp(pattern[, flags])
    ```
    
    - 변수를 사용해 동적으로 RegExp 객체를 생성할 수 있다.

## 31.3 RegExp 메서드

- RegExp.prototype.exec
    - 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 배열로 반환한다.
    - 첫 번째 매칭 결과만 반환하므로 주의
    
    ```jsx
    const target = 'Is this all there is?';
    const regExp = /is/;
    
    regExp.exec(target);
    /* [
    	'is', //매칭된 문자열
    	index:5, //일치가 시작된 인덱스
    	input: "Is this all there is?", //원본 문자열
    	groups: undefined //g플래그가 없기때문에 여러개를 찾지 않음
    	] */
    ```
    
- RegExp.prototype.test
    - 인수로 전달받은 문자열에 대해 정규 표현식의 패턴을 검색하여 매칭 결과를 불리언 값으로 반환
- RegExp.prototype.match
    - String 표준 빌트인 객체가 제공하는 메서드
    - 대상 문자열과 인수로 받은 정규 표현식과의 매칭 결과를 배열로 반환함
        
        ```jsx
        const target = 'Is this all there is?';
        const regExp = /is/;
        
        regExp.exec(target);
        /* [
        	'is', //매칭된 문자열
        	index:5, //일치가 시작된 인덱스
        	input: "Is this all there is?", //원본 문자열
        	groups: undefined //g플래그가 없기때문에 여러개를 찾지 않음
        	] */
        	
        	//g플래그가 지정되면 exec 메서드와 달리 모든 매칭 결과를 배열로 반환한다.
        	const target = 'Is this all there is?';
        const regExp = /is/;
        
        regExp.exec(target);
        /* [
        	'is', //매칭된 문자열
        	"is",
        	] */
        ```
        

## 31.4 플래그

- 정규 표현식의 검색 방식을 설정하기 위해 사용 (총 6개)
- 주요 플래그
    
    
    | 플래그 | 의미 | 설명 |
    | --- | --- | --- |
    | i | ignore case | 대소문자를 구별하지 않고 패턴을 검색한다. |
    | g | Global | 대상 문자열 내에서 패턴과 일치하는 모든 문자열을 전역 검색 |
    | m | Multi line | 문자열의 행이 바뀌더라도 패턴 검색 계속 |
- 선택적으로 사용이 가능하며 순서와 상관없이 여러개의 플래그를 동시 설정할 수 있음
- 플래그가 설정되지 않으면 대소문자를 구별하여 검색
- 패턴 검색 매칭 대상이 여러개여도 첫번째 매칭 대상만 검색하고 종료함

## 31.5 패턴

- 문자열의 일정한 규칙을 표현하기 위해 사용됨
- `/` 로 열고 닫으며 문자열의 따옴표는 생략
- 문자열 검색
    - 정규 표현식의 패턴에 문자 또는 문자열을 지정하면 검색 대상 문자열에서 패턴으로 지정한 문자 똔ㄴ 문자열을 검색함
    - 위의 RegExp 메서드를 사용하여 검색 대상 문자열과 정규 표현식의 매칭 결과를 구해야 검색이 수행됨
- 임의의 문자열 검색
    - `.` 은 임의의 문자 한개를 의미하며 내용이 무엇이든 상관없음
    - 예시
    
    ```jsx
    const target = 'Is this all there is?';
    const regExp = /.../g; //임의의 3자리 문자열을 대소문자를 구별하여 전역 검색
    target.match(regExp); //["is ","thi","s a", ... "is?"]
    ```
    
- 반복 검색
    - `{m, n}` 은 앞선 패턴이 최소 m번, 최대 n번 반복되는 문자열을 의미함
    - `{n}` 은 앞선 패턴이 n번 반복되는 문자열을 의미한다.
    - `{n, }`은 앞선 패턴이 최소 n번 이상 반복되는 문자열을 의미한다.
    - `+` 는 앞선 패턴이 최소 한번 이상 반복되는 문자열을 의미한다.
    - `?` 는 앞선 패턴이 최대 한 번(0번 포함) 이상 반복되는 문자열을 의미한다.
- OR 검색
    - `|` 은 or의 의미를 갖는다.
    - 분해되지 않은 단어 레벨로 검색하기 위해서는 `+`를 함께 사용한다.
    - 범위를 지정하려면 `[]` 내에 `-` 를 사용한다.
- NOT 검색
    - `[…]` 내의 `^` 은 not의 의미를 갖는다.
- 시작 위치로 검색
    - `[…]`밖의 `^`은 문자열의 시작을 의미한다.
- 마지막 위치로 검색
    - `$` 는 문자열의 마지막을 의미한다.

## 31.6 자주 사용하는 정규표현식

- 특정 단어로 시작하는지 검사
    
    `http://` 또는 `https://` 로 시작하는지 검사
    
    ```jsx
    const url = 'https://example.com';
    /^(http|https):\/\//.test(url); //true
    ```
    
- 특정 단어로 끝나는지 검사
    - `html` 로 끝나는지 검사
    
    ```jsx
    const fileName = 'index.html';
    /html$/.test(fileName); //true
    ```
    
- 숫자로만 이루어진 문자열인지 검사
    
    ```jsx
    const target = '12345';
    /^\d+$/.test(target); //true \d는 숫자를 의미한다
    ```
    
- 하나 이상의 공백으로 시작하는지 검사
    
    ```jsx
    const target = ' Hi!';
    /^[\s]+/.test(target); //true \s는 여러가지 공백문자를 의미한다.
    ```
    
- 아이디로 사용가능한지 검사
    - 문자열이 알파벳 대소문자 또는 숫자로 시작하고 끝나며 4~10자리인지 검사
    
    ```jsx
    const id = 'abc123';
    /^[A-Za-z0-9]{4,10}$/.test(id); //true
    ```
    
- 특수 문자 포함 여부 검사
    
    ```jsx
    
    const target = 'abc#123';
    /[^A-Za-z0-9]/gi.test(target); //true
    
    ```