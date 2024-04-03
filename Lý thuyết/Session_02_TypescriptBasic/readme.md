# TypeScript Basic

## Giới thiệu

- nó là 1 ngôn ngữ mã nguồn mỡ của Javascript.
- hỗ trợ "static typing" : là có thể khai báo kiểu cho biến và trính biên dịch sẽ giảm được tỷ lệ gán sai kiểu của các giá trị.
- Ưu điểm 

`
ràng buộc chặt chẽ type các biến. function giúp việc viết code được minh bạch, rõ ràng hơn , 
tiết kiệm thời gian kiểm tra lại code, giảm va chạm lỗi trong thời gian vận hành.
`

`
khắc phục tình trạng xuất hiện lỗi và dễ đọc hơn
`

`
tự động bắt lỗi ngữ pháp như dùng sai tên biến, function,...
`

`
tự động chuyển đổi các cú phát es6 -> es5 như arrow function...
`

`
hỗ trợ tối ưu hóa quy trình làm việc giúp tiệt kiệm thời gian và công sức
`

- nhược điểm

`
bắt buộc phải dùng biên dịch: vì nó thể chạy trực tiếp trên browser nên cần phải có 1 công cụ chuyển dổi nó thành js để hoạt động trên browser
`

- Document 

`
https://www.w3schools.com/typescript/
https://www.typescriptlang.org/docs/handbook/typescript-tooling-in-5-minutes.html
`

## Môi trường chạy

- Cài đặt Nodejs : `https://nodejs.org/en/download`
- Cài đặt TypeScript
chạy lệnh bằng rerminal git bash

b1: `npm install -g typescript và npm init -y or yarn init -y `
=> sinh ra 1 file package.js (chú ý nên chạy "npm install -g typescript " để tránh bị bug khi biên dịch.)

b2: `npm install typescript --save-dev or yarn add typescript --save-dev ` 
=> sinh ra 1 folder node_modules và 1 package-lock.json

b3: cấu hình typescript `npx tsc --init`
=> sinh ra 1 file tsconfig.json 
và cấu hình lại file như sau

```bash
target: es2016
  module: commonjs
  strict: true
  esModuleInterop: true
  skipLibCheck: true
  forceConsistentCasingInFileNames: true
```
b4: tạo file .ts

b5: Biên dịch file ts -> js
`tsc index.ts `
=> sẽ tự động build 1 file .js . nếu file đó tồn tại sẽ bị ghi đè 

b6: chay file js `node index.js`

## Type

1. Khai báo biến trong ts 
`let [tên biến]: [kiểu dữ liệu của biến] = giá trị của biến`

2. Các kiểu dữ liệu

- `boolean, number, string`
- `any` (hạn chế dùng): chưa xác định kiểu dữ liệu. có thể là string, number, hay bất kỳ kiểu dữ liệu nào. và với any thì có thể thay đổi kiểu dữ liệu cho giá trị của biến
- `array` có 2 kiểu number[] hoặc string[]; nếu muốn ngăn chặn việc thay đổi mảng thì thêm keyword "readonly" `let [tên biến]: readonly string[] = ['abc'];`

3. mảng hổn hợp trong TS gọi là tuple

```bash
	let myTuple: [string, number, boolean];
	myTuple = ['tuple',2,true];
	console.log(myTuple)
```

4. object: nên khai báo kiểu tách ra riêng

```bash
let myCar : {type: string, model: string, year: number} =  {
    type: 'Toyota',
    model: 'Corolla',
    year: 2020
}
// or
type carType = {
    type: string,
    model: string,
    year: number
}
let newMyCar: carType = {
    type: 'Toyota',
    model: 'Corolla',
    year: 2024
}
console.log('myCar',myCar)
console.log('newmyCar',newMyCar)
```

5. `union(or type | othertype)`: vừa có thể là kiểu dữ liệu number hoặc string 

```bash
let myUnion: number | string = 'abc';
```

6. `interface` kiểu dữ liệu kế thừa kiểu dữ liệu kết hợp với extends

```bash
interface carTypeBasic {
    type: string,
    model: string,
    year: number
}
interface carTypeContainer extends carTypeBasic {
    weight: string,
    cod: boolean
}
let carContainer: carTypeContainer = {
    type: 'Toyota',
    model: 'Corolla',
    year: 2024,
    weight: '10T',
    cod: true
}
console.log('carContainer',carContainer);
```

7. `void`: kiểu dữ liệu cho 1 hàm không return 

```bash
function myName(name: string):void {
    console.log('myname void',name)
}
myName('osaka');
function myName2(name: string = 'shizuoka'): void {
    console.log('myname2 void',name)
}
myName2();
type profileType = {
    name: string,
    gender: string
}
let myProfile = {
    name: 'join',
    gender: 'male'
}
let getProfile = (myProfile: profileType) => {
    let msg = `Name ${myProfile.name} - gender ${myProfile.gender}`
    console.log(msg)
}
getProfile(myProfile)
```
