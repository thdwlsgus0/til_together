### 자바스크립트 배열
  - 여러 개의 값을 순차적으로 나열한 자료구조입니다.
  ```javascript
    const arr = ["apple", "banana", "orange"];
  ```

  - 자바스크립트에서 배열은 좀 더 배열처럼 동작하도록 구현하였습니다.
  - 배열이 훨씬 빠릅니다.
  ```javascript

  const arr = [];

  console.time('Array Performance Test');

  for(let i = 0; i < 10000000; i++) {
	  arr[i] = i;
  }

  console.timeEnd('Array Performance Test');

  const obj = {};

  console.time('Object Performace Test');

  for(let i = 0; i < 10000000; i++) {
	  obj[i] = i;
  }

  console.timeEnd('Object Performance Test');
  ```

  - 배열 생성
  - Array.from 유사 배열 객체 또는 이터러블 객체를 인수로 전달받아 배열로 변환하여 반환합니다.
  ```javascript
    console.log(Array.from({length: 2, 0: 'a', 1: 'b'}));   
  ```