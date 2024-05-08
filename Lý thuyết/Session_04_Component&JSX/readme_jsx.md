# JSX(Javascript XML)

## Giới thiệu

- Là 1 cú pháp mở rộng cho Javascript được sử dụng trong React để xây dựng giao diện người dùng.
- jsx cho phép chúng ta viết mã HTML tương ứng và sử dụng các thành phần React để xây dựng giao diện
- Giúp chúng ta tạo ra các đối tượng React Element, mô tả cách giao diện người dùng sẽ được hiển thị.
- jsx kết hợp các component và HTML trong 1 cú pháp duy nhất, giúp mã nguồn trở nên dễ đọc và dễ hiểu hơn.

## Tài liệu 

- <https://www.w3schools.com/REACT/react_jsx.asp>
- <https://react.dev/learn/writing-markup-with-jsx>

## Cách sử dụng JSX

- Trong html ta sử dụng 

```html
<h1 class="greeting">Hello</h1>
```

- Trong React ta phải sử dụng phương thức Javascript để tạo các phần tử HTML:

```js
const element = React.createElement('h1',{className: 'greeting' }, 'Hello');
```
Cách viết như trên thì khá phức tạp và tốn thời gian để tạo ra 1 UI. 
Thay vào đó người ta phát triển ra 1 thư viện giúp bạn đơn giản hóa hơn cách code trên là JSX

Như vậy JSX giúp bạn ra các element trong javascript như code HTML thuần túy

- Trong file `App.tsx` 
```js
// chèn html 1 dòng
const element = <h1 className="greeting">Hello</h1>
```
```js
// chèn khối html lớn
const element = (
  <>
    <h1 className="greeting">Hello!</h1>
    <h2>Good to see you here.</h2>
  </>
);
```

Khi ứng dụng chạy thì JSX được biên dịch ngược lại cú pháp của Javascript như ví dụ trên.
Bạn có thể render ra giao diện người dùng 

```js
ReactDOM.createRoot(document.getElementById('root')!).render(
  element,
)
```

## Biểu thức trong JSX

Bằng cách khai báo nó như 1 biến 

```js
// Biểu thức trong JSX 
const name = 'Hoàng';
const element = <h1>Hello, {name}</h1>;

const myelement = <h1>phép cộng 2 số  {2 + 5} </h1>;
```

## Ưu điểm của JSX trong react

- Dễ đọc và dễ viết : cú pháp giống như HTML giúp viết các thành phần React 1 cách dễ dàng và tự nhiên
- Tích hợp code Javascript: có thể sử dụng các biểu thức Javascript để tích hợp logic vào trong mã JSX
- Tối ưu hóa mã: JSX giúp viết mã gọn gàng và tổ chức tốt hơn, dễ dàng bảo trì và phát triển.

## 1 số lưu lý trong JSX

1. Luôn trả về 1 root Element ( hay còn gọi là container tag)

- khi trả về nhiều dòng thì phải đặt chúng trong 1 thẻ cha

```js
return (
	<>
		...
		...
	</>
)
```

2. Các tag `<img>` `<input>` thì buộc phải có thẻ đóng như sau `<img />` `<input />`

3. Sử dụng cú pháp camelCase trong mọi trường hợp

- class: `className`
- jsx style: background-image ==> `backgroundImage` or background-color ==> `backgroundColor`

4. Sử dụng Curly Braces ( cú pháo Javascript trong html jsx)

Doc: <https://beta.reactjs.org/learn/javascript-in-jsx-with-curly-braces>

- Sử dụng cặp dấu ngặc nhọn `{ ... }` , bạn có thể sử dụng cú pháp Javascript bên trong nó 

```js
const name = 'Hoàng';
const element = <h1>Hello, {name}</h1>;
```

==> Như vậy bạn có thể sử dụng Biến ngay trong HTML với hỗ trợ của JSX

- sử dụng double curlies `{{... }}`

**🔹 inline CSS**

```js
let Courses = (
    <ul style={{
      backgroundColor: 'black',
      color: 'pink'
    }}>
      <li>Javascript</li>
      <li>React JS</li>
      <li>Node JS</li>
    </ul>
  );
```

=> ccs inline là 1 object với properties của Css thì chuyển qua dạng camelCase nếu nó có 2 từ trở lên

**🔹 Javascript Object**

```js
const info = {
	name: 'abc'
	address: 'abc@gmail.com'
}
let Avatar = (
	<>
		<h1>{info.name}</h1>
		<p>{info.address}</p>
	</>
)
```