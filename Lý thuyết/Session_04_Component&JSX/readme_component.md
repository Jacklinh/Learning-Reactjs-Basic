# Components ( Thành phần)

## Giới thiệu

- nó là 1 phần giao diện người dùng được chia thành các phần nhỏ.
- Có thể tái sử dụng và độc lập.
- Giúp bạn chia nhỏ các phần phức tập của ứng dụng thành các phần nhỏ hơn, dễ quản lý và tái sử dụng 
- React được xây dựng trên cơ sở các components. Mỗi component là 1 đoạn mã `Javascript` or `Typescript` độc lập có thể nhận dữ liệu đầu vào và trả về 1 phần giao diện người dùng(UI) cụ thể.
- Khi dữ liệu đầu vào thay đổi, giao diện người dùng sẽ được cập nhật 1 cách tự động mà k cần phải thực hiện lại trang web.
- chúng ta có thể xây dựng giao diện người dùng bằng cách kết hợp các Component lại với nhau

## Doc

- <https://react.dev/learn/your-first-component>
- <https://www.w3schools.com/REACT/react_components.asp>
- <https://react.dev/learn/thinking-in-react>

## Các loại component

- có 2 loại component là `Function Component` và `Class Component`
- Component chúng ta hướng tới đó là 1 thành phần nhỏ, 1 block UI
- bất ký 1 thành phần UI nào hiển thị ra màn hình đều có thể là 1 Component 

## Đặt tên 1 Component

- Đặt tên theo liểu `Pascal Case`( còn được gọi là `Upper Camel Case`): bắt buộc ký tự đầu tiên phải viết Hoa

ví dụ: ButtonReadmore, FooterDefault

## Cách định nghĩa 1 Component

trong file `App.tsx` thêm 1 đoạn code sau

```js
// định nghĩa 1 component
function ButtonReadmore() {
	return <button type="button">Read more</button>;
}
```
=> như vậy khi bạn định nghĩa 1 component trên thì bạn đã tạo ra được 1 function Component trong React

## Sử dụng 1 Component

trong file `App.tsx` phần return của App ta gọi component `ButtonReadmore` vừa định nghĩa vào bằng cách `<ButtonReadmore />`

```js
// sử dụng component
function App() {
  return (
    <>
      <ButtonReadmore />
    </>
  );
}
```
## Các component lồng vào nhau

- Tạo thêm 1 component `GroupButtonReadmore`

```js
// sử dụng component lồng nhau
function GroupButtonReadmore() {
  return (
    <>
      <ButtonReadmore />
	  <ButtonReadmore />
	  <ButtonReadmore />
    </>
  );
}
```
- `App.tsx` sửa lại như sau

```js
// sử dụng component
function App() {
  return (
    <>
      <GroupButtonReadmore />
    </>
  );
}
```

## Import và Export Components

- React nổi bật với việc tái sử dụng, do vậy bạn nên chia nhỏ thành nhiều các component.
- Tạo 1 folder `components` bên trong src để chứa các file component
- Để làm được như thế ta tạo file .js or .jsx or .ts or .tsx và đặt code của component vào trong đó.

ví dụ: Tạo 1 file src/components/ButtonReadmore.tsx ( tên file giống với tên Component)

```js
// sử dụng export
function ButtonReadmore() {
  return <button type="button">Read more</button>;
}
//ES6 syntax
export default ButtonReadmore;
```

Bây giờ ở đâu muốn sử dụng lại `ButtonReadmore` thì chúng ta import vào 

```js
// sử dụng import 
//ES6 import ButtonReadmore
import ButtonReadmore from "/components/ButtonReadmore.tsx";

function App() {
  return (
    <>
      <ButtonReadmore />
    </>
  );
}
```

## Khi nào thì cần tạo 1 Component

- Khi 1 tình năng nhưng thành phần lặp đi lặp lại và nhận thấy có thể tái sử dụng 
- Giao diện cùng 1 kiểu nhưng chỉ khác nhau ở màu nền, màu chữ, icon...
- 1 thành phần có thể chạy độc lập, mà bạn chỉ muốn nó re-Render lại khi cần thiết.
- 1 thành phần thường xuyên thay đổi nội dung
