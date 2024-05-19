# Props 

## Giới thiệu

- Props là các tham số được truyền vào 1 React Components
- Props có thể sử dụng như là 1 HTML attribute
- Luồng xử lý đơn luồng từ component cha đến con
## Tại sao cần đến Props

để trả lời chúng ta cùng tạo ra 2 button như sau

```js
function ButtonAddToCart() {
  return <button type="button">Thêm giỏ hàng</button>;
}
function ButtonCall() {
  return <button type="button">Gọi tư vấn</button>;
}
```
Với kiến thức đã học component thì bạn sẽ tạo ra 2 button trên với cách code như vậy
tuy nhiên nó lại mất đi đặc tính nổi bật của component là TÁI SỬ DỤNG
Ở vd trên ta thấy 2 button trên hoàn toàn giống nhau, nó chỉ khác phần `label` tên hiển thị
=> chính lúc này ta cần đến 1 biến trong react gọi là `props`

Khi đó giờ ta chỉ cần tạo ra 1 component và nhận tham số

```js
//Button nhận vào một tham số có tên mặc định trong React là props ==> nó là một object
function Button(props) {
  return <button type="button">{props.label}</button>;
}
// trong file app.tsx khi đó để tạo 2 button chúng ta chỉ việc gọi component button của nó
<Button label="Thêm giỏ hàng" />
<Button label="Gọi Tư Vấn" />
```
Trên đây ta sử dụng `label="Thêm giỏ hàng"` thì react nó tự động thêm vào `props` 1 phần tử có key là `label` và `value` là giá trị trong dấu `=`.
Khi đó trong component Button chúng ta truy cập tới phần tử `label` để lấy giá trị của nó `props.label` như object bình thường.

## Truyền props cho 1 component

- React components sử dụng các props để giao tiếp với nhau. 
component Cha cần truyền thông tin đến component Con bằng cách sử dụng props.
- props sử dụng như html attributes, nhưng có thể truyền bất kỳ giá trị Javascript thông qua chúng ( bao gồm cả object, arrays, và function)
như vậy để truyền nhiều giá trị vào props ta sử dụng kỹ thuật `Destructuring` của 1 object

```js
// cú pháp Destructuring 1 object
// props = {label, fontSize, bgColor, customStyle}
// {label, fontSize, bgColor, customStyle} = props
function Button({ label, fontSize, bgColor, customStyle }) {
  return <button type="button">{props.label}</button>;
}
// và khi qua App.tsx sẽ gọi như sau
<Button label="Thêm giỏ hàng" fontSize={18} bgColor="#222" customStyle={{fontWeight: "bold" , textTransform: "uppercase"}} />
<Button label="Gọi Tư Vấn" />
```

## default props

- Trong react, chúng ta sử dụng function để tạo component, cũng như ES6 react component chúng ta có thể định nghĩa thuộc tính `defaultProps`

```js
function Button(props) {
  return <button type="button">{props.label}</button>;
}
// Kiểu như là đặt giá trị dự phòng, nếu không truyền thì nó lấy giá trị mặc định hiển thị.
Button.defaultProps = {
  label: "Button Label",
  color: "#fff",
  bgColor: "#ff6700",
};
```
## props children

- bạn có thể lồng các component vào với nhau giống như các thẻ html vậy. điều đó làm cho jsx trông giống như html.và các component hay nội dung được lồng nhau ở trong component được gọi là children

child component

```js
// ở đây ta có 3 component Son, chúng sẽ là props.children của component Dad
<Dad>
  <Son />
  <Son />
  <Son />
</Dad>
// khi đó Dad sẽ như sau
function Dad(props){
  return (
    <div>
      {props.children}
    <div>
  )
}
// or 
function Dad({children}){
  return (
    <div>
      {children}
    <div>
  )
}
```

## props types ( trường hợp đi theo hướng javascript còn typescript thì k cần sử dụng)

 Nó giúp chúng ta kiểm tra các kiểu dữ liệu của các props mà component nhận vào. Nó tránh được những lỗi khó chịu và cải thiện được tính tái sử dụng component của bạn rất nhiều

- Tài liệu : <https://legacy.reactjs.org/docs/typechecking-with-proptypes.html> <https://www.npmjs.com/package/prop-types>
- cách cài đặt Package
```bash
npm install --save prop-types
//or
yarn add prop-type
```

1. importing

```js
import PropTypes from 'prop-types'; // ES6
var PropTypes = require('prop-types'); // ES5 with npm
```

## Thêm style vào dự án

trong react bạn sử dụng css class với tên là `className`. nó giống như html class attribute

1. css stylesheet
đơn giản là bạn sẽ import file css vào component bằng cách

```js
import "./App.css";
```
lưu ý: nếu bạn import css vào trong file App thì css đó sẽ có tính toàn cục cho toàn bộ trang

2. inline styling

trong react, inline styles không được viết dưới dạng string như html thông thường. thay vào đó nó sẽ được viết dưới dạng object với key được viết theo kiểu camelCased còn style của value sẽ thường là string

```js
<div style={{ backgroundColor: "white", color: "red" }}>Hello</div>
```
Ngoài ra, chúng ta cũng có thể tạo một biến lưu trữ giá trị css rồi truyền nó vào các element như sau:

```js
const styleObject = {backgroundColor: 'white', color: 'red'}

<div style={styleObject}>Hello</div>
```

3. css Module

để tránh xung đột style, chúng ta sử dụng css module kết hợp với đóng gói 1 component

vd css module
```js
//Cách thực hiện: Tạo một file css có tên
ComponentName.module.
// đặt nó cùng với file component sử dụng nó
/* Import  */
import styles from "./ComponentName.module.scss";

...
return (
    <div className={styles.container}>

    </div>
);
```

** Đóng gói 1 component (Encapsulte a component) ** 

Qua phần lý thuyết trên thì chúng ta thấy

- component là 1 UI có thể chạy độc lập và tái sử dụng
- component có thể có style đi kèm
=> vậy làm thế nào để có thể CHUẨN HÓA 1 component để tái sử dụng khắp mọi nơi?

* Đóng gói component *
- trong folder components tạo 1 folder Button( đặt bằng chính tên Component)

``` bash
-src
  |-components
        |-Button
            |-index.js // file chứa code component. lúc import component thì react tự hiểu sẽ lấy file index.js
            |-Button.module.css // chứa style của component button
```

- import component như sau:

```js
import Button from "./components/Button";
```
4. style component (Advanced)

Nó là 1 thư viện dành cho react và react native cho phép bạn viết style ở cấp độ component trong ứng dụng của bạn

tài liệu <https://styled-components.com/docs/basics#installation> & <https://viblo.asia/p/su-dung-styled-component-trong-react-63vKjjWyK2R>

5. css framework
các thư viện css framework như
- tailwindCss: <https://tailwindcss.com/docs/installation/>
- BoostrapCss: <https://react-bootstrap.netlify.app/>

6. Sass(Advanced)

SASS/SCSS là 1 chương trình tiền xử lý css(css preprocessor). nó giúp bạn viết css theo cách của 1 ngôn ngữ lập trình, có cấu trúc rõ ràng, rành mạch

- học cú pháp: <https://www.w3schools.com/sass/default.php>
- dùng với react: <https://www.w3schools.com/react/react_sass_styling.asp>

cài đặt

```bash
npm i sass
#hoặc
yarn add sass
```

sau khi cài đặt chuyển tất cả file `.css` thành `.scss` hoặc `.sass`

vd 
```js
import "./App.scss";
```

sau đó tổ chức kế thừa:

bạn tạo folder `src/styles` sau đó tạo 2 file `_variables.scss` và `_mixins.scss`
trong đó
- `_variables.scss` chứa các biến toàn cục
- `_mixins.scss` chứa các phần code css lặp lại nhiều, cần tái sử dụng

dùng các biến và mixin trong file `app.scss`

```js
@import "./styles/_variables";
@import "./styles/_mixins";

/* Định nghĩa một Class */
.wrapper {
  background-color: $background-color; //dùng biến từ _variables
  color: $text-color;
  padding: $block-padding;

  &__btns {
    @include vertical-list; //Kế thừa lại đoạn đã định nghĩa vertical-list từ _mixins
    button {
      width: $btn-width;
      height: $btn-height;
    }
   }
}
```


## Embed icon font react

sử dụng chèn icon vào react

- tài liệu react icons: <https://react-icons.github.io/react-icons>

Cài đặt
```bash
npm install react-icons --save
```

Sử dụng
```js
import { FaBeer } from 'react-icons/fa';
function MyComponent() {
  render() {
    return <h3> Lets go for a <FaBeer />? </h3>
  }
}
```

- Tài liệu font awesome : <https://docs.fontawesome.com/web/use-with/react/>

## Chèn image vào react

b1. lưu tất cả image vào 1 folder images trong thư mục public
b2. cách sử dụng
```html
<img src="./images/ten-hinh.png" alt="" />
```
Khi chạy ứng dụng thì thư mục public là thư mục gốc

hoặc với cách sau:

```js
//Import dưới dạng biến images
import images from "./images/ten-hinh.png";
<img src={images} alt="" />;
```

hoặc nếu sử dụng với React Vite bạn cần cấu hình thêm vite.config.ts

```js
//Sửa hàm defineConfig thành như sau
export default defineConfig({
  root: "./",
  build: {
    outDir: "dist",
  },
  publicDir: "public",
  plugins: [react()],
});
```