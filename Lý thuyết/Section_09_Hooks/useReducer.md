# useReducer

Tài liệu : <https://react.dev/reference/react/useReducer>

> useReducer nó cung cấp thay thế cho useState cho phép quản lý các trạng thái phức tạp hoặc logic trạng thái phức tạp hơn

useReducer là 1 hook cho phép bạn quản lý state các thành phần sử dụng 1 hàm reducer

Các state được lưu trữ ở 1 nơi gọi là kho (store) và sử dụng chung cho nhiều component

useReducer cung cấp cho bạn 1 thêm 1 lựa chọn nữa để quản lý state trong function component
- những gì `useState` làm được thì `useReducer` làm được và ngược lại

## cú pháp

```js
import { useReducer } from "react";
const [state, dispatch] = useReducer(reducer, initialArg, init?)
```

- `reducer`: là 1 function chứa logic xử lý cập nhật state có đối số là `state` và `action`, lúc này hàm sẽ return về `next state`
- `initialArg` giá trị khởi tạo mặc định state
- `[init]` : hàm khởi tạo sẽ trả về state ban đầu. hàm này có hoặc không
- `Dispatch ` kích hoạt 1 active trong function
=> trả về 1 state hiện tại và 1 dispatch method 

## Khi nào dùng useReducer

UseState
- thường dùng với những components có state đơn giản
- state có kiểu dữ liệu nguyên thủy: number, string, boolean, hoặc object, array đơn giản
- số lượng state trong 1 component ít

useReducer
- thường dùng với những components có state phức tạp: array, object nhiều lớp
- số lượng state trong 1 component nhiều
- state sau lại cần kết quả của state trước để thực hiện việc tính toàn, xử lý logic

phân tích cách thực hiện
```js
//useState
// 1. Init state: 0
// 2. Actions: Up (state + 1), Down (state - 1 )


//useReducer
// 1. Init state: 0
// 2. Actions: Up (state + 1), Down (state - 1)
// 3. Tạo Reducer (Xử lý logic để thay đổi State)
// 4. Dispatch (Kích hoạt một action)
```

## ví dụ

1. Coutdown sử dụng `useState`
```js
const CountApp = () => {

  const [count,setCount] = React.useState(0);

  const handlerDown = () => {
      setCount(prev => prev - 1);
  }
  const handlerUp = () => {
    setCount(prev => prev + 1);
  }
  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handlerDown}>Down</button><button onClick={handlerUp}>Up</button>
    </div>
  )
}
```
cũng bài toán trên ta sử dụng `useReducer`

```js
//Init State
// Giá trị khởi tạo lúc đầu là 0
const initialState = 0;

//Actions

const ACTION_UP = 'up';
const ACTION_DOWN = 'down';

/**
 * 
 * @param state state hiện tại
 * @param action hành động thay đổi state
 * reducer sẽ dự vào action để thực hiện hành động tương ứng, sau đó trả về state mới (cùng kiểu dữ liệu với initialState)
 */
const reducer = (state, action) =>{
  // Lúc đầu reducer nó chưa chạy
  // Cho đến khi dispatch được gọi
  console.log('reducer running');
  switch(action) {
    case ACTION_UP:
      return state + 1;
    case ACTION_DOWN:
      return state - 1;
    default:
      throw new Error(`Action invalid`);
  }
}

//dispatch sử dụng bên trong components

const CountApp = () => {

  /**
   * useReducer là một hàm nhận 3 tham số đầu vào, chủ yếu dùng 2.
   * Tham số 1: reducer
   * Tham số 2: initialState
   * 
   * useReducer chạy và tạm thời để reducer ở đó, nó chạy giá trị khởi tạo initialState trước và trả về mảng có 2 phần tử:
   * - state hiện tại (count)
   * - dispatch (dùng nó để kích hoạt action, DOWN hay UP để có hành động thay đổi state tương ứng)
   * 
   * 
   * 
   */
  const [count,dispatch] = React.useReducer(reducer,initialState);

  const handlerDown = () => {
      dispatch(ACTION_DOWN);
  }
  const handlerUp = () => {
    dispatch(ACTION_UP);
  }
  return (
    <div>
      <h1>{count}</h1>
      <button onClick={handlerDown}>Down</button><button onClick={handlerUp}>Up</button>
    </div>
  )
}
```

2. ví du Todolist sử dụng useState và useReducer ( add, edit, save, deleted)
<https://react.dev/learn/extracting-state-logic-into-a-reducer#>

3. sử dụng useReducer kết hợp với useContex
<https://react.dev/learn/scaling-up-with-reducer-and-context>

4. 1 số ví dụ khác

<https://devtrium.com/posts/how-to-use-react-usereducer-hook>

## Hạn chế

để vận hành được `useReducer` trong 1 ứng dụng lớp rất phức tạp, khó bảo trì code.
Tuy nhiên đã có 1 vài giải pháp khác đơn giản nhưng vẫn đạt hiệu quả tương tự như
- react Redux
- redux Toolkit
- redux saga
- zustand(đơn giản mà hiệu quả)