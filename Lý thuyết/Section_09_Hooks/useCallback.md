# useCallback

tài liệu : <https://react.dev/reference/react/useCallback>
- là 1 hook trong react, để ghi nhớ 1 hàm.
- tối ưu hóa hiệu suất ứng dụng bằng cách ngăn chặn việc các hàm k cần thiết trong mỗi lần render
- giúp các component con không bị re-render khi không cần thiết.

## Cú pháp

```js
import { useCallback } from "react";
const cachedFn = useCallback(fn, [dependencies])

// chi tiêt
const cachedFn = useCallback(() => {
  // xử lý useCallback
}, [dependencies]);
```

## khi nào thì sử dụng `useCallback`

- useCallback dùng cache(ghi nhớ) 1 function có sử dụng state
- hook chỉ chạy khi [dependencies] thay đổi ==> điều này làm cải thiện performance
- dùng khi `component con` cần truyền 1 sự kiện callback ra cho `component cha` và không muốn `component con` render lại khi state của `component cha` thay đổi.
- `component con` cần thêm `useMemo` nữa mới đạt hiệu quả.
- [dependencies] có thể là các hook như `useEffect,useMemo`

Ví dụ 1
```js
import React, { useState, useCallback } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');
  // handleClick là 1 callback được ghi nhớ và chỉ thay đổi khi count thay đổi. như vậy hàm handleClick sẽ k bị tạo lại nếu các props khác của component thay dổi
  const handleClick = useCallback(() => {
    setCount(count + 1);
  }, [count]);

  const handleChange = useCallback((event) => {
    setText(event.target.value);
  }, []);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={handleClick}>Increment</button>
      <input type="text" value={text} onChange={handleChange} />
    </div>
  );
};

export default MyComponent;

```

ví dụ 2

```js
//App.js

import { useState, useCallback } from "react";
import Todos from "./Todos";

const App = () => {
  const [count, setCount] = useState(0);
  const [todos, setTodos] = useState([]);

  const increment = () => {
    setCount((c) => c + 1);
  };

  const addTodo = () => {
    setTodos((t) => [...t, "New Todo"]);
  };

  return (
    <>
      <Todos todos={todos} addTodo={addTodo} />
      <hr />
      <div>
        Count: {count}
        <button onClick={increment}>+</button>
      </div>
    </>
  );
};

// Todos.js

import { memo } from "react";

const Todos = ({ todos, addTodo }) => {
  console.log("child render");
  return (
    <>
      <h2>My Todos</h2>
      {todos.map((todo, index) => {
        return <p key={index}>{todo}</p>;
      })}
      <button onClick={addTodo}>Add Todo</button>
    </>
  );
};

export default Todos;
//export default memo(Todos);
```

Ở đây xảy ra 1 vấn đề nếu ta k dùng useCallback:
- khi click `increment` để tăng biến `count` lên thì bạn xem console.log và thấy component todo được re-render lại 1 cách k cần thiết.
- biến `count` thay đổi -> component cha app.js bị re-render lại
- function `addTodo` khởi tạo 1 giá trị tham chiếu mới. do đó bạn được hiểu là hàm `addTodo` đã thay đổi => dẫn đến component con `todo.js` render .
=> ở đây có 1 khái niệm là `referential equality`( Bình đẳng tham chiếu)

GIải thích `referential equality`
- là 1 khái niệm liên quan đến việc so sánh các tham chiếu(referential) của đối tượng, thay vì so sánh giá trị của đối tượng. 
- trong react sử dụng cơ chế so sánh tham chiếu để quyết định khi nào cần `re-render` của 1 component
- trong javascript: kiêm rtra 2 biến có cùng tham chiếu đến 1 đối tượng trong bộ nhớ hay k? chỉ khi 2 biến chỉ được xem bằng nhau nếu chúng trỏ đến cùng 1 địa chỉ bộ nhớ.
```js
const obj1 = { name: 'Alice' };
const obj2 = { name: 'Alice' };
const obj3 = obj1;

console.log(obj1 === obj2); // false, vì chúng là hai đối tượng khác nhau trong bộ nhớ
console.log(obj1 === obj3); // true, vì chúng trỏ đến cùng một đối tượng trong bộ nhớ

```

- Trong react: sử dụng các hook như `useCallback ` và `useMemo` dựa vào cơ chế để ghi nhớ giá trị hoặc hàm, giúp tránh việc tạo lại chúng nếu `dependecies` không thay đổi.
```js
const MyComponent = () => {
  const [count, setCount] = useState(0);

  const handleClick = useCallback(() => {
    setCount(prevCount => prevCount + 1);
  }, []);

  // Component con nhận callback handleClick dưới dạng prop
  return <ChildComponent onClick={handleClick} />;

  // hoặc sử dụng React.memo` để tối ưu hóa component bằng cách ghi nhờ kết quả render trước đó
  // lúc này React.memo, ChildComponent chỉ re-render nếu các props thay đổi 
  const ChildComponent = React.memo(({ onClick }) => {
    console.log('ChildComponent re-rendered');
    return <button onClick={onClick}>Click me</button>;
  });
};
```

Trở lại ví dụ 2. như vậy để giải quyết vấn đề todos re-render ta cần sửa lại hàm addTodo như sau
```js
// ghi nhớ (cache) lại
// Nó chỉ thay đổi khi todos có thay đổi
const addTodo = useCallback(() => {
  setTodos((t) => [...t, "New Todo"]);
}, [todos]);
```
useCallback phát huy tác dụng khi nó đi kèm với memo()
```js
export default memo(Todos);
```