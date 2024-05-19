# Destrucuring trong javascript

là 1 cý pháp trong javascript(giới thiệu trong ECMAScript 6_ES6) giúp bạn trích xuất dữ liệu từ mãng hoặc đối tượng và gán chúng vào các biến 1 cách ngắn gọn, rõ ràng hơn.

## Destrucuring array

vd cơ bản

```js
const numbers = [1, 2, 3];
const [first, second, third] = numbers;

console.log(first); // 1
console.log(second); // 2
console.log(third); // 3
```

- bạn có thể bỏ qua 1 số phần tử trong mảng

```js
const numbers = [1, 2, 3];
const [, second, ] = numbers;

console.log(second); // 2
```

- bạn cũng có thể gán giá trị mặc định nếu phần tử k tồn tại

```js
const numbers = [1, 2];
const [first, second, third = 3] = numbers;

console.log(third); // 3
```
- mảng lồng nhau

```js
const numbers = [1, [2, 3], 4];
const [first, [second, third], fourth] = numbers;

console.log(second); // 2
console.log(third); // 3
```

- trích xuất ra 1 array mới bằng cách sử dụng ...Rest Property

khi sử dụng `const [a, b, ...rest] = array` thì rest tạo ra 1 array mới chứa các phần tử còn lại của array

```js
const numbers = [1, 2, 3, 4, 5];

const [first, second, ...rest] = numbers; // cú pháp của destructuring

console.log(first); // 1
console.log(second); // 2
console.log(rest); // [3, 4, 5] // tức là rest chưa các phần tử còn lại của mảng numbers
```

## Destrucuring object

vd cơ bản

```js
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

const { name, age, city } = person;

console.log(name); // John
console.log(age); // 30
console.log(city); // New York
```

- bạn có thể gán thuộc tính của đối tượng vào biến với tên khác

```js
const person = {
  name: 'John',
  age: 30,
  city: 'New York'
};

const { name: personName, age: personAge, city: personCity } = person;

console.log(personName); // John
console.log(personAge); // 30
console.log(personCity); // New York
```

- Bạn có thể gán giá trị mặc định

```js
const person = {
  name: 'John',
  age: 30
};

const { name, age, city = 'Unknown' } = person;

console.log(city); // Unknown
```

- object lồng nhau

```js
const person = {
  name: 'John',
  address: {
    city: 'New York',
    zip: 10001
  }
};

const { name, address: { city, zip } } = person;

console.log(city); // New York
console.log(zip); // 10001
```

- trích xuất ra 1 object mới bằng cách sử dụng ...Rest Property

khi sử dụng `const [a, b, ...rest] = object` thì rest tạo ra 1 object mới chứa các phần tử còn lại của object

```js
const person = {
  name: 'John',
  age: 30,
  city: 'New York',
  country: 'USA'
};

const { name, age, ...rest } = person;

console.log(name); // John
console.log(age); // 30
console.log(rest); // { city: 'New York', country: 'USA' }
```

## Destrucuring function

destrucuring cũng rất hữu ích khi làm việc với các đối số của hàm

```js
const printPerson = ({ name, age }) => {
  console.log(`Name: ${name}, Age: ${age}`);
};

const person = {
  name: 'John',
  age: 30
};

printPerson(person); // Name: John, Age: 30

// fucntion với array
const sum = ([a, b]) => a + b;

const numbers = [1, 2];
console.log(sum(numbers)); // 3

```

