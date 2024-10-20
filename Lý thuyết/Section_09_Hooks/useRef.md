# useRef hook

Tài liệu: <https://react.dev/reference/react/useRef>

các ví dụ sử dung useRef: <https://www.robinwieruch.de/typescript-react-useref/>

là 1 hook thường được sử dụng để lưu trữ và truy cập vào các tham chiếu đến các phần tử DOM haowcj các giá trị khác mà không gây ra việc render lại 1 component

## cú pháp

```js
import { useRef } from "react";
const ref = useRef(initialValue)
```
initialValue là giá trị khời tạo của đối tượng `ref`. nó sẽ được lưu trong thuộc tính `current` của đổi tượng `ref`

## khi nào thì sử dụng `useRef`

- để có thể tương tác trực tiếp với các phần tử DOM
- lưu trữ 1 giá trị mà không cần phải re-render component khi giá trị đó thay đổi
- cho phép bạn duy trì các giá trị giữa các lần render

## ví dụ
1. sử dụng `useRef` truy cập vào DOM, để thay đổi giá trị của 1 input

```js
import React, { useRef } from 'react';

function MyComponent() {
  const inputRef = useRef();

  console.log('render');

  const handleClick = () => {
    // sử dụng inputRef.current.value = 'value' để thay đổi giá trị của input bằng giá trị mới.
    inputRef.current.value = 'Hello, React!';
  };

  return (
    <div>
      <input type="text" ref={inputRef} />
      <button onClick={handleClick}>Change Value</button>
    </div>
  );
}
```

chúng ta sử dụng useRef để lưu trữ tham chiếu đến phần tử input. khi người dùng click `Change Value` thì hàm `handleClick` được gọi. 
Lúc này bạ thấy component k bị re-render khi giá trị input thay đổi

2. Đếm số lần Render

```js
import React, { useEffect, useRef, useState } from 'react';

function Counter() {
    // dùng useRef để khởi tạo biến renderCount và gán giá trị ban đầu là 0
  const renderCount = useRef(0);
  const [count, setCount] = useState(0);

  useEffect(() => {
    renderCount.current += 1;
  });

  return (
    <div>
      <p>Count: {count}</p>
      <p>Render count: {renderCount.current}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
}

export default Counter;
```

Mỗi khi component re-render, chúng ta tăng giá trị của biến `renderCount` bằng cách truy cập vào `renderCount.current`
khi click `Increment` thì giá trị biến `count` sẽ tăng lên 1 và component re-render. Tuy nhieen giá trị của biến `renderCount` không thay đổi sau mỗi lần re-render. do đó nó sẽ đếm số lần re-render của component mà k cần re-render lại chính nó

3. tracking state changes (lưu trữ và sử dụng giá trị trước và sau render)

```js
import React, { useRef, useEffect } from 'react';

function MyComponent() {
  const countRef = useRef(0);
  const prevCountRef = useRef(0);

  useEffect(() => {
    prevCountRef.current = countRef.current;
  });

  const handleIncrement = () => {
    countRef.current += 1;
    console.log('Previous count:', prevCountRef.current);
    console.log('Current count:', countRef.current);
  };

  return (
    <div>
      <button onClick={handleIncrement}>Increment</button>
    </div>
  );
}

```

ở ví dụ trên , chúng ta sử dụng 2 giá trị tham chiếu `countRef` và `prevCountRef`. khi click `Increment` giá trị của `countRef` sẽ tăng lên 1 và in ra màn hình cùng với giá trị trước đó của `countRef` thông qua `prevCountRef.current`


>** sự khác nhau giữa `useRef` và `useState`
>
>- `useRef` không gây re-render component khi giá trị `.current` thay đổi
>- `useState` gây re-render component mỗi khi giá trị state thay đổi