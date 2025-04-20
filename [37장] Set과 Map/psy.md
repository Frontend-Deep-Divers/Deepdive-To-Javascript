## Set

중복되지 않는 유일한 값들의 집합

요소 순서에 의미가 없고 인덱스로 요소에 접근할 수 없음

### 생성

```jsx
const set = new Set(이터러블);
```

### 요소 개수 확인

```jsx
set.size
```

### 요소 추가

```jsx
set.add(1).add(2).add(3)
```

### 요소 여부

```jsx
set.has(2)
```

### 요소 삭제

```jsx
set.delete(2);

set.clear();
```

### 요소 순회

```jsx
set.forEach((v, v2, set) => console.log(v)); // v === v2

// Set 객체는 이터러블
console.log(...set);
```

## Map

키와 값의 쌍으로 이루어진 컬렉션

모든 값을 키로 사용할 수 있고, 이터러블이다.

> 객체는 문자열 또는 심벌 값만 키로 사용 가능
> 

### 생성

```jsx
const map = new Map();

// 이터러블을 인수로 전달 가능 (키와 값의 쌍으로 이루어져야 함)
const map = new Map([['key1', 'val1'], ['key2', 'val2']]);
```

### 요소 개수 확인

```jsx
map.size
```

### 요소 추가

```jsx
map.set('key', 'val').set('key2', 'val2');
```

### 요소 취득

```jsx
map.get('key');
```

### 요소 여부 확인

```jsx
map.has('key');
```

### 요소 삭제

```jsx
map.delete('key');

map.clear();
```

### 요소 순회

```jsx
map.forEach((val, key, map) => console.log(val, key, map));

// Map은 이터러블
for(const entry of map) {
	console.log(entry); // [key, val]
}

const keys = map.keys();
const values = map.values();
const entries = map.entries();
```

Map 객체를 순회할 때에는 추가된 순서를 따름 → ECMAScript에 규정되어 있지는 않음
