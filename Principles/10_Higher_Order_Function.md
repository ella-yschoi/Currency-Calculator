# What is Higher Order Function?

<br/>

## 1. 고차함수

### (1) 용어 정리

- 고차함수
  - 인자로 함수를 받거나(콜백함수)함수를 반환하는 함수
- 함수형 프로그래밍
  - 어떤 특정한 일을 수행하는 함수 단위로 엮어 두는 것
- 순수함수
  - 함수 안에서 불변성을 유지하는 것
- 불변성
  - 함수 밖에서 함수 내부의 특정한 상태 혹은 함수 안에서 전달받은 매개변수를 수정하지 않음
  - 함수 내부에서 side effect가 일어나지 않도록 함
- 정리하자면,
  - 순수함수들을 묶어 연결해 놓은 것을 함수형 프로그래밍
  - 함수형 프로그래밍 시, 에러의 가능성을 낮추고 가독성을 높일 수 있음.
  - 함수형 프로그래밍 시, 데이터 변경 ❌, 변수 사용 ❌, 조건문 ❌, 반복문 ❌

### (2) 배열을 돌면서 원하는 것(콜백함수)을 할때

```javascript
const fruits = ['🍌', '🍓', '🍇', '🍓'];

// forEach의 인자로 콜백함수를 전달하며 배열의 요소마다 한번씩 호출됨
fruits.forEach(function (value) {
  console.log(value);
});

// 화살표 함수 버전
fruits.forEach((value) => console.log(value));
```

### (3) 조건에 맞는(콜백함수) 아이템을 찾을 때

```javascript
const item1 = { name: '🥛', price: 2 };
const item2 = { name: '🍪', price: 3 };
const item3 = { name: '🍙', price: 1 };
const products = [item1, item2, item3, item2];

// find: 제일 먼저 조건에 맞는 아이템을 반환
let result = products.find((item) => item.name === '🍪');
console.log(result); // {name: '🍪', price: 3}

// findIndex: 제일 먼저 조건에 맞는 아이템의 인덱스를 반환
result = products.findIndex((item) => item.name === '🍪');
console.log(result); // 1

// 배열의 아이템들이 부분적으로 조건(콜백함수)에 맞는지 확인
result = products.some((item) => item.name === '🍪');
// 하나라도 조건에 맞으면 true
console.log(result); // true

// 배열의 아이템들이 전부 조건(콜백함수)에 맞는지 확인
result = products.every((item) => item.name === '🍪');
console.log(result); // false

// 조건에 맞는 모든 아이템들을 새로운 배열로
result = products.filter((item) => item.name === '🍪');
// const products = [item1, item2, item3, item2];에 item2가 2개였으므로
console.log(result); // [{name: '🍪', price: 3}, {name: '🍪', price: 3}]
```

```javascript
// Map: 배열의 아이템들을 각각 다른 아이템으로 매핑(변환)해서 새로운 배열 생성
const nums = [1, 2, 3, 4, 5];

result = nums.map((item) => item * 2);
console.log(result); // [2, 4, 6, 8, 10]

result = nums.map((item) => {
  if (item % 2 === 0) {
    return item * 2;
  } else {
    return item;
  }
});
console.log(result); // [1, 4, 3, 8, 5]

// Flatmap: 중첩된 배열을 쫙 펴서 새로운 배열로
// map을 하긴 하는데 map 대상이 배열이라면 펴는 것
result = nums.map((item) => [1, 2]);
console.log(result); // [[1, 2], [1, 2], [1, 2], [1, 2], [1, 2]]

result = nums.flatMap((item) => [1, 2]);
console.log(result); // [1, 2, 1, 2, 1, 2, 1, 2, 1, 2]

result = ['co', 'ding'].flatMap((text) => text.split(''));
console.log(result); // ['c', 'o', 'd', 'i', 'n', 'g']
```

```javascript
// sort: 배열의 아이템들을 정렬
// 문자열 형태의 오름차순으로 요소를 정렬하고,
// (map처럼 새로운 배열을 만드는 것이 아닌) 기존의 배열을 수정
const texts = ['hi', 'abc'];
texts.sort();
console.log(texts); // ['abc', 'hi']
```

```javascript
const numbers = [0, 5, 4, 2, 1, 10];
// ⚠️주의: 숫자의 문자열화
numbers.sort();
console.log(numbers); // [0, 1, 10, 2, 4, 5]

// < 0 a가 앞으로 정렬, 오름차순
// > 0 b가 앞으로 정렬, 내림차순
numbers.sort((a, b) => a - b);
console.log(numbers); // [0, 1, 2, 4, 5, 10]

// reduce: 배열의 요소들을 접고 접어서 값을 하나로
// sum 인자는 계속 합해진 값을 가지고 있을 것임
// value는 각각 전달받을 인자
// sum이라는 변수를 초기화 할 값: 0 (0 → 1 → 3 → 6 → 10 → 최종적으로 15)
result = [1, 2, 3, 4, 5].reduce((sum, value) => (sum += value), 0);
console.log(result); // 15
```

<br/>

## 2. 응용

### (1) Quiz.1

- 주어진 배열 안의 딸기 아이템을 키위로 교체하는 함수를 만들기
- 단, 주어진 배열을 수정하지 않고, 새로운 배열 반환

```javascript
// input: ['🍌', '🍓', '🍇', '🍓']
// output: [ '🍌', '🥝', '🍇', '🥝' ]
function replace(array, from, to) {
  // 배열을 돌면서 맵핑
  return array.map((item) => (item === from ? to : item));
}

const array = ['🍌', '🍓', '🍇', '🍓'];
const result = replace(array, '🍓', '🥝');
console.log(result);
```

### (2) Quiz.2

- 배열과 특정한 요소를 전달받아,
- 배열안에 그 요소가 몇개나 있는지 카운트 하는 함수 만들기

```javascript
// input: [ '🍌', '🥝', '🍇', '🥝' ], '🥝'
// output: 2
function count(array, item) {
  // 우리가 찾고있는 것만 필터링 해주는게 더 효율적
  // filter를 호출하면 찾고자 하는 요소들만 들어있는 새로운 배열이 만들어짐
  return array.filter((value) => value === item).length;
  // 고차함수에서 if문 쓰는 것 보단,,
  /*  return array.reduce((count, value) => {
    if (value === item) {
      count++;
    }
    return count;
  }, 0); */
}

console.log(count(['🍌', '🥝', '🍇', '🥝'], '🥝'));
```

### (3) Quiz.3

- 배열1, 배열2 두개의 배열을 전달받아,
- 배열1 아이템중 배열2에 존재하는 아이템만 담고 있는 배열 반환

```javascript
// input: ['🍌', '🥝', '🍇'],  ['🍌', '🍓', '🍇', '🍓']
// output: [ '🍌', '🍇' ]

function match(array1, array2) {
  // includes가 true인 것들만 리턴
  return array1.filter((item) => array2.includes(item));
}
console.log(match(['🍌', '🥝', '🍇'], ['🍌', '🍓', '🍇', '🍓']));
```

### (4) Quiz.4

- 5이상(보다 큰)의 숫자들의 평균

```javascript
const nums = [3, 16, 5, 25, 4, 34, 21];

const result2 = nums //
  // 5보다 큰 숫자들을 filter해서 새로운 배열을 만들어 준 다음
  .filter((num) => num > 5) // [16, 25, 34, 21]

  // 더하고 더해서 "하나의 값: avg", 즉 평균값을 도출해야 하니 reduce 사용
  // reduce 함수에서는 index, array도 인자로 사용할 수 있음
  // num에는 16, 25, 34, 21이 순차적으로 들어와 계속 더해나감
  .reduce((avg, num, _, array) => avg + num / array.length, 0);
  
console.log(result2); // 24
```
