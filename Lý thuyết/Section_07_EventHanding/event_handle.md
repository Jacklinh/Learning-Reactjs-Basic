# Event handle

## Responding to Events(phản hồi sự kiện)

ví dụ khi bạn click, rê chuột, focus vào 1 input... thì đó là những sự kiện.
React cho phép bạn tạo ra các phản hồi lại giao diện người dùng tương ứng với từng sự kiện.

tài liệu <https://react.dev/learn/responding-to-events>

Handling events trong react elements rất giống với handing events trong DOM. chỉ khác nhau cú pháp

- React events có tên đặt theo kiểu camelCase
- Với JSX bạn truyền 1 function như 1 event handler, hơn là chuỗi.

DOM events javascript: <https://www.w3schools.com/jsref/dom_obj_event.asp>
Cú pháp Typescript cho events: <https://react-typescript-cheatsheet.netlify.app/docs/basic/getting-started/forms_and_events/>

Ví dụ 1 sự kiện click trong html

```js
<button onclick="activateLasers()">
  Activate Lasers
</button>
```
## event mouse ( sự kiện chuột)

```js
function handleClick() {
    alert('You clicked me!');
}
// ở đây ta truyền hàm `handleClick` vào sự kiện onclick . chứ không gọi 
<button onClick={handleClick}> 
  Click me
</button>

// typescript event
<button onClick={(e: React.MouseEvent<HTMLButtonElement, MouseEvent>) => {
   e.preventDefault();
  console.log('You clicked me!');
}}> Click Me</button>
```

>** Lưu ý **
>
> để truyền 1 event handlers thì ta truyền chứ không được gọi

## keyboardEvent ( sự kiện bàn phím)

Tham khảo Typescript cho event keyboard: <https://felixgerschau.com/react-typescript-onkeyup-event-type/>

```js
import React, { KeyboardEvent } from 'react';

const KeyboardEventsExample = () => {
 
  const handleKeyDown = (event:  KeyboardEvent<HTMLInputElement>) => {
    console.log('Bạn đã nhấn phím', event.key);
  };

  const handleKeyUp = (event:  KeyboardEvent<HTMLInputElement>) => {
    console.log('Bạn đã rời tay khỏi phím', event.key);
  };

  return (
    <div>
      <input
        type="text"
        onKeyDown={handleKeyDown}
        onKeyUp={handleKeyUp}
        placeholder="Nhấn phím bất kỳ vào đây..."
      />
    </div>
  );
};

export default KeyboardEventsExample;
```

## Event handlers có sử dụng tham số

ví dụ tạo 1 tham số number

```js
const EventHandlerWithParameterExample = () => {

  const handlePlay = (number) => {
    console.log('Bạn đã play bài ',number)
  };

  const handleNext = (number) => {
    console.log('Bạn đã next sang bài ',number)
  };

  return (
    <div>
      <h1>Play Music</h1>
      <button onClick={() => handlePlay(5)}>Play bài số 5</button>
      <button onClick={() => handleNext(2)}>Nhảy sang bài số 2</button>
    </div>
  );
};

export default EventHandlerWithParameterExample;
```

## Truyền event handlers như là Props

1. default

```js
type ButtonTypeProps = {
  onClick: () => void;
  children?: React.ReactNode;
}

function Button({ onClick, children } : ButtonTypeProps) {
    return (
      <button onClick={onClick}>
        {children}
      </button>
    );
  }
```
 ở trên ta thấy sự kiện `Click` nhưng khi dùng cách này thì tên của nó bắt buộc thêm `on` ở trước `onClick`
 `onClick` function event handler dùng như 1 props, được lấy từ props.

2. Handler dưới dạng callback

vd có component con

```js
function Button({label}) {
  function handlePlayClick() {
    console.log(`Playing + tên của phim `);
  }

  return (
    <button onClick={handlePlayClick}>
      {label}
    </button>
  );
}
```
và component Cha

```js
function PlayMovies(){
  const movieTitle = 'Captain America';
  return <Button label="Play" />
}
```

- mong muốn khi click button play thì in ra `Playing + tên của phim`
- nhưng tên của phim thì nằm ở component cha

để làm được yêu cầu trên có thể sửa lại như sau

```js
function Button({label, name}) {
  function handlePlayClick() {
    console.log(`Playing + ${name} `);
  }

  return (
    <button onClick={handlePlayClick}>
      {label}
    </button>
  );
}
function PlayMovies(){
  const movieTitle = 'Captain America';
  return <Button label="Play" name={movieTitle} />
}
```
Nhưng như thế thì nó lại mất đi tính `tái sử dụng ` của component button
ví dụ muốn tạo thêm 1 button Next và công việc của nó là đi nhảy sang bài kế tiếp thì sao?

==> Như thế ta nên chú ý

- component nên chỉ đảm nhận việc hiển thị UI
- Xử lý logic nên tách ra ngoài

khi đó ta sửa code lại như sau

```js
function Button({onHandleClick, label, name}) {
  return (
    <button onClick={onHandleClick}>
      {label}
    </button>
  );
}
function PlayMovies(){
  const movieTitle = 'Captain America';
  //chuyển handler xử lí qua cho component CHA
  function handlePlayClick() {
    console.log(`Playing + ${name} `);
  }
  return <Button onClick={handlePlayClick} label="Play" name={movieTitle} />
}
```

=> ở component con mà thực hiện 1 event ở component cha(callback)

>** Lưu ý **
>
>- ở trong PlayMovies chúng ta dùng thuộc tính `onClick`
>- Nhưng ở phần định nghĩa props cho component button , chúng ta lại khai báo là `onHandleClick` mà không phải là `onClick`( tất nhiên có thể dùng tên onClick)
>- điều đó không quan trọng , react chỉ yêu cầu tên bắt buộc phải là `on`

## event propagation(lan truyền sự kiện)

- doc event propagation javascript : <https://hanoiict.edu.vn/lan-truyen-su-kien-trong-javascript>

event propagation là cách định nghĩa thứ tự của html element khi event xảy ra
có 2 giai đoạn để sự kiện được lan truyền (event propagation) trong html dom: `capturing` và `bubbling`

vd nếu ta có 1 phần tử(sự kiện B) `<p>` bên trong 1 phần tử `<div>` ( sự kiện A)

```html
<!-- Trong Html -->
<div onclick="suKienA">
    <p onclick="suKienB"></p>
</div>
```
nếu khi người dùng ckick lên phần tử `<p>`, thì sự kiện `click` của phần tử nào sẽ được xử lý trước ?

- Trong `capturing` thì sự kiện của phần tử bên ngoài cùng `<div>` sẽ được xử lý trước , sau đó mới tới `<p>`
- Trong `bubbling` thì sự kiện của phần tử bên trong cùng `<p>` sẽ được xử lý trước , sau đó mới tới `<div>`


## stopping propagation( dừng lan truyền sự kiện)

xem : <https://react.dev/learn/responding-to-events#stopping-propagation>
để ngăn chặn lan truyền sự kiện của 1 phần tử nào đó. lúc này ta sử dụng `e.stopProgation()` tại sự kiện cần stop

vd 
```js
<button onClick={(e: React.MouseEvent<HTMLButtonElement, MouseEvent>) => {
   e.stopPropagation();
  console.log('You clicked me!');
}}> Click Me</button>
```

## preventing default behavior(dừng sự kiện mặc định của trình duyệt)

xem: <https://react.dev/learn/responding-to-events#preventing-default-behavior>
để ngăn chặn 1 vài sự kiện (submit ...)  thì ta dùng `e.preventDefault()`
vd 
```js
<button onClick={(e: React.MouseEvent<HTMLButtonElement, MouseEvent>) => {
   e.preventDefault();
  console.log('You clicked me!');
}}> Click Me</button>
```

với 1 function handler

```js
const handlerClick = (e: React.MouseEvent<HTMLButtonElement, MouseEvent>) => {
    e.preventDefault();
    alert('Clicked!');
  }

<button onClick={(e: React.MouseEvent<HTMLButtonElement, MouseEvent>) => handlerClick(e)}>
 Click Me
 </button>
```


>** Nội dung cần nhớ **
>
>- các cách khác nhau để tạo 1 event handle
>- làm thế nào để truyền event handling logic từ 1 component cha
>- thế nào là 1 sự kiện lan truyền và cách khắc phục

