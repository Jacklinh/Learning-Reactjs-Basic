# useContext

tài liệu: <https://react.dev/reference/react/useContext>

> useContext là cách để quản lý state global

## Cú pháp

```js
import { useContext } from 'react';
const value = useContext(SomeContext)
```

## khi nào thì sử dụng `useContext`

- khi component cha cần truyền giá trị xuống cho component con có nhiều cấp lồng vào nhau sâu( lúc này có 2 cách để thực hiện)
    - Cách bình thường: truyền props lần lượt vào các component cha xuống con
    - Cách khác: dùng useContext
- Giúp đơn giản hóa việc truy cập các giá trị của context, đặc biệt là khi bạn muốn tránh các render lồng nhau gây rối.

## các bước thực hiện khi sử dụng `useContext`

- Bước 1: tạo folder `src/context`
- Bước 2: trong thư mục `src/context` tạo file `userContext.tsx`
```js
import React from "react";

interface UserType {
  id: number;
  name: string;
  avatarUrl: string;
}
//Bước 1: Tạo context
//createContext nhận vào giá trị khởi tạo, mặc đinh là null
export const userContext = (React.createContext < UserType) | (null > null);

// Bước 2: Tạo Provider context
//userContext.Provider sử dụng props value mặc định: chứa thông tin cần truyền xuống children
export const UserProvider = ({
  user,
  children,
}: {
  user: UserType,
  children?: React.ReactNode,
}) => {
  return <userContext.Provider value={user}>{children}</userContext.Provider>;
};
```
`Provider ` sẽ cung cấp giá trị của context cho component con
`
- Bước 3: sửa code App lại như sau
```js
//Import Provider vào App
import { UserProvider } from "./context/userContex";

function App() {
  console.log("App rendered");
  return (
    <div>
      <UserProvider user={userInfo}>
        <HomePage />
      </UserProvider>
    </div>
  );
}

export default App;
```

- Bước 4: đê component `userInfo` lấy thông tin user

```js
import React from "react";
import { userContext } from "../../../../context/userContext";

function UserInfo() {
  const userInfo = React.useContext(userContext);
  return (
    <>
      <span>Username: {userInfo.name}</span>
    </>
  );
}
```

sử dụng `useContext` để truy cập giá trị của context trong bất kỳ function component nào

## Ví dụ

- b1: tạo context bằng cách sử dụng `React.createContext`. file MyProvider.js

```js
import React, { createContext, useState } from 'react';

// Tạo context
const MyContext = createContext();

const MyProvider = ({ children }) => {
  const [value, setValue] = useState('Hello, World!');

  return (
    <MyContext.Provider value={{ value, setValue }}>
      {children}
    </MyContext.Provider>
  );
};

export { MyContext, MyProvider };
```

- b2: sử dụng `useContext` trong 1 component. file MyComponent.js

sử dụng `useContext` để truy cập giá trị của context trong 1 function component

```js
import React, { useContext } from 'react';
import { MyContext } from './MyProvider';

const MyComponent = () => {
  const { value, setValue } = useContext(MyContext);

  return (
    <div>
      <p>{value}</p>
      <button onClick={() => setValue('New Value')}>Change Value</button>
    </div>
  );
};

export default MyComponent;
```
- b3: hiển thị MyComponent ra màn hình . file app.js

```js
import React from 'react';
import MyComponent from './MyComponent';

const App = () => {
  return (
    <div>
      <h1>Context API Example</h1>
      <MyComponent />
    </div>
  );
};

export default App;
```

- b4. sử dụng  `provider` để bao bọc các component mà bạn muốn chúng có thể truy cập vào context. file index.js

```js
import React from 'react';
import ReactDOM from 'react-dom';
import App from './App';
import { MyProvider } from './MyProvider';

ReactDOM.render(
  <MyProvider>
    <App />
  </MyProvider>,
  document.getElementById('root')
);
```

>** Lưu ý **
>
>- `useContext` giúp truy cập context 1 cách dễ dàng trong các function component mà k cần dùng đến các render props
>- cung cấp 1 cách tiện lợi để quản lý state toàn cục trong úng dụng 
>- Giảm thiểu việc phải truyền props qua nhiều cấp component , giúp mã nguồn trở nên sạch sẽ và dễ quản lý hơn