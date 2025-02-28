# Redux toolkit

## Bước 1

Cài đặt Redux Toolkit and React-Redux

```bash
npm install @reduxjs/toolkit react-redux
# or
yarn add @reduxjs/toolkit react-redux
```

Cài thêm Tool Debug

```bash
yarn add --dev redux-devtools-extension
yarn add --dev @types/node
```

## Bước 2: Tạo Redux Store

Tạo file `src/redux/store.ts`

```ts
import { configureStore } from "@reduxjs/toolkit";

const store = configureStore({
  reducer: {},
  devTools: process.env.NODE_ENV !== "production",
});

export default store;

export type RootState = ReturnType<typeof store.getState>;
```

Store là nơi lưu trữ Global State cho toàn bộ ứng dụng

## Bước 3: Cấu hình Provider Redux Store cho React

Tại file main.tsx (React Vite)

```ts
import React from "react";
import ReactDOM from "react-dom";
import "./index.css";
import App from "./App";
import store from "./redux/store";
import { Provider } from "react-redux";

ReactDOM.render(
  <Provider store={store}>
    <App />
  </Provider>,
  document.getElementById("root")
);
```

Về bản chất Provider này là useContext, mang store đến với các component con.

## Bước 4: Tạo một Redux State Slice

`Redux State Slice` được gọi là một lát cắt, thể hiện một tính năng cần sử dụng global state.

Tạo mới một file `src/redux/features/counter/counterSlice.ts`

```ts
import { createSlice, PayloadAction } from "@reduxjs/toolkit";

interface CounterState {
  count: number;
}

const initialState: CounterState = {
  count: 0, //giá trị khởi tạo
};

export const counterSlice = createSlice({
  name: "counter", //đặt tên cho slice, không trùng lặp
  initialState,
  reducers: {
    //Hàm tăng count lên 1
    increment: (state) => {
      state.count += 1;
    },
    //Hàm giảm count đi 1
    decrement: (state) => {
      state.count -= 1;
    },
    //Hàm tăng bởi một số tùy chọn
    incrementByAmount: (state, action: PayloadAction<number>) => {
      state.count += action.payload;
    },
  },
});

// Export các action ra để các component có thể sử dụng được.
export const { increment, decrement, incrementByAmount } = counterSlice.actions;

export default counterSlice.reducer;
```

Trong đó:

- initialState: là giá trị khởi tạo của các state, có thể có nhiều state và được tổ chức thành object
- reducers: là một object chứa các hàm xử lý logic để thay đổi giá trị của state.

## Bước 5: Thêm Slice Reducers vào Store chung

Ứng dụng của bạn có thể có nhiều Slice, cứ mỗi Slice như vậy bạn cần cấu hình vào Store chung để toàn bộ app có thể truy cập vào Store.

Sửa file `src/redux/store.ts`

```ts
import { configureStore } from "@reduxjs/toolkit";
import counterReducer from "./features/counter/counterSlice";

const store = configureStore({
  reducer: {
    counter: counterReducer,
  },
  devTools: process.env.NODE_ENV !== "production",
});

export default store;

export type RootState = ReturnType<typeof store.getState>;
```

## Bước 6: Sử dụng Redux State cho Các Component

Tạo file `src/redux/features/counter/Counter.tsx`

```tsx
import { useSelector, useDispatch } from "react-redux";
//Lấy các action từ Slice vào
import { decrement, increment } from "./counterSlice";
import { RootState } from "../../store";

export function Counter() {
  //Sử dụng các hooke của react-redux để truy vập vào Store chung
  const count = useSelector((state: RootState) => state.counter.count);
  const dispatch = useDispatch();

  return (
    <div>
      <h2>Couter Slice</h2>
      <div>
        <button
          aria-label="Increment value"
          onClick={() => dispatch(increment())}
        >
          Increment
        </button>
        <span>{count}</span>
        <button
          aria-label="Decrement value"
          onClick={() => dispatch(decrement())}
        >
          Decrement
        </button>
      </div>
    </div>
  );
}
```

## Bước 7: Import Counter vào App

Import vào và chạy thử

==> Xem ví dụ đầy đủ tại: `2.Examples\example-redux-toolkit`