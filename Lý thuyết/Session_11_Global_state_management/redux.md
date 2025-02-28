# Giới thiệu về React Redux

https://react-redux.js.org/introduction/getting-started

## Tại sao phải dùng Redux?

## Cài đặt Redux.

```bash
yarn add redux react-redux
```

Công cụ gở lỗi

```bash
yarn add --dev redux-devtools-extension
```

## Cách hoạt động của Redux

Các thành phần của Redux bao gồm:

- Store: Store đơn giản là 1 object chứa tất cả state toàn cục của ứng dụng. Nhưng thay vì lưu các state, nó lưu các reducer, sẽ được nói sau.
- Các Action: Khi ta định nghĩa các action, ta khai báo các tên của hành động trong ứng dụng. Lấy ví dụ ta có 1 state là counter và cần 2 phương thức để tăng và giảm giá trị của counter. Lúc này ta định nghĩa 2 action có tên là 'INCREMENT' và 'DECREMENT' và chỉ vậy thôi, việc xử lý thay đổi state của counter sẽ nhường cho reducer.
- Các Reducer: 1 reducer tương đương với 1 state nhưng kèm theo các mô tả state sẽ thay đổi như thế nào khi các action khác nhau được gọi. Trong ví dụ ta có reducer là counter, nó lưu state của counter và kiểm tra action vừa được gọi là INCREMENT hay DECREMENT và trả về state mới là state+1 hay state-1 tương ứng.
- Các Dispatch: Khi cần dùng 1 action ở component, ta gọi action đó đơn giản bằng cách sử dụng phương thức dispatch. VD: dispatch(increment()), dispatch(decrement()).

Cách vận hành được tóm gọn bởi sơ đồ sau:

![redux](https://d33wubrfki0l68.cloudfront.net/01cc198232551a7e180f4e9e327b5ab22d9d14e7/b33f4/assets/images/reduxdataflowdiagram-49fa8c3968371d9ef6f2a1486bd40a26.gif)

## Tìm hiểu của ví dụ mẫu

```bash
#React với Vite, TypeScript, Redux
npx degit reduxjs/redux-templates/packages/vite-template-redux my-app
```

## Tìm hiểu cách sử dụng qua một Ví dụ simple Bank

Ứng dụng Simplebank để rút tiền và nạp tiền vào tài khoản ngân hàng. Trong ví dụ này, chúng ta sẽ sử dụng Redux để quản lý trạng thái tài khoản ngân hàng của người dùng.

### Bước 1 - Khởi tạo Redux Store

Đầu tiên, chúng ta sẽ tạo ra Redux Store và các actions để rút tiền và nạp tiền:

Tạo một folder BankApp trong thư mục src, sau đó tạo lần lượt các file sau:

Bước 1.1 Tạo action (các lệnh điều khiển)

```js
// bankActions.ts

//Lệnh rút tiền
// Action Types
export const DEPOSIT = "DEPOSIT";
export const WITHDRAW = "WITHDRAW";

export type DepositAction = {
  type: typeof DEPOSIT,
  payload: number,
};

export type WithdrawAction = {
  type: typeof WITHDRAW,
  payload: number,
};

export type BankActionTypes = DepositAction | WithdrawAction;

// Action Creators
export function depositMoney(amount: number): BankActionTypes {
  return {
    type: DEPOSIT,
    payload: amount,
  };
}

export function withdrawMoney(amount: number): BankActionTypes {
  return {
    type: WITHDRAW,
    payload: amount,
  };
}
```

Bước 1.2 Tạo Reducer

```js
// bankReducer.ts
import { DEPOSIT, WITHDRAW, BankActionTypes } from "./bankActions";

// Initial State
type BankState = {
  balance: number,
};

const initialState: BankState = {
  balance: 0,
};

// Reducer
export function bankReducer(
  state = initialState,
  action: BankActionTypes
): BankState {
  switch (action.type) {
    case DEPOSIT:
      return { balance: state.balance + action.payload };
    case WITHDRAW:
      return { balance: state.balance - action.payload };
    default:
      return state;
  }
}
```

### Bước 2 - Cấu hình Store Provider

```js
import React from "react";
import { Provider } from "react-redux";
import { createStore } from "redux";
import BankAccount from "./BankAccount";

// ... (định nghĩa action types, action creators, và reducer ở đây)

// Create Store
const store = createStore(bankAccount);

function App() {
  return (
    <Provider store={store}>
      <BankAccount />
    </Provider>
  );
}

export default App;
```

### Bước 3 - Tạo Component và Kết nối với Redux Store

```js
// BankAccount.tsx
import React from "react";
import { useSelector, useDispatch } from "react-redux";

const BankAccount = () => {
  const balance = useSelector((state) => state.balance);
  const dispatch = useDispatch();

  return (
    <div>
      <h2>Balance: {balance}</h2>
      <div>
        <button
          onClick={() => {
            console.log("withdrawMoney");
            dispatch(depositMoney(5));
          }}
        >
          -5
        </button>
        <button
          onClick={() => {
            dispatch(withdrawMoney(10));
          }}
        >
          +10
        </button>
      </div>
    </div>
  );
};

export default BankAccount;
```