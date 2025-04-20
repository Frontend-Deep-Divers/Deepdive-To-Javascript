이터러블의 값들을 목록으로 만듦 → 값을 만드는 연산자가 아님

## 함수 호출문에서 사용

```jsx
const arr = [1, 2, 3];

const max = Math.max(arr);
```

Rest 파라미터와는 정반대의 역할

## 배열 리터럴 내부에서 사용

### concat

```jsx
const arr = [...arr1, ...arr2];
```

### splice

```jsx
arr.splice(1, 0, ...arr2);
```

### 배열 복사

```jsx
const copy = [...arr];
```

### 이터러블 → 배열

```jsx
const arr = [...iterable]
```

> 유사 배열 객체는 Array.from 활용
> 

## 객체 리터럴 내부에서 사용

```jsx
const obj = { x: 1, y: 2 };
const copy = { ...obj };
```
