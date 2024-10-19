# React Router

để tạo ra nhiều trang nội dung như html (top, free, login, dashboard, product...)- tạo tài liệu bên reactjs

## thêm react router vào dự án
tài liệu <https://www.npmjs.com/package/react-router-dom>
<https://reactrouter.com/en/main>

```bash
npm i -D react-router-dom
yarn add -D react-router-dom
```
## Ý tưởng sitemap

Ví dụ bạn muốn khi URL:

- / thì hiển thị trang chủ Dashboard
- /categories thì hiển thị trang quản lý danh mục
- /productss thì hiển thị trang quản lý sản phẩm

## Tổ chức cấu trúc thư mục tương ứng với từng trang

1. Tạo một folder pages, trong thư mục src
2. Tạo các Component Page tương ứng cho từng trang và đặt vào trong thư mục pages

```js

src/
├─ pages/
│  ├─ DashboardPage.js
│  ├─ CategoryPage.js
│  ├─ ProductPage.js
│  ├─ NoPage.js

```
## Cấu hình Routes 

tại file app.tsx
```tsx
// import react router dom
import { BrowserRouter, Route, Routes } from "react-router-dom";
// import pages
import LayoutAdmin from "./components/layouts/LayoutAdmin";
import DashboardPage from "./pages/DashboardPage";
import LoginPage from "./pages/LoginPage";
import NoPage from "./pages/NoPage";
import './App.css'

function App() {


  return (
    <>
      <BrowserRouter>
            <Routes>
                <Route path="/" element={<LayoutAdmin />}>
                    <Route index element={<DashboardPage />} />
                </Route>
                <Route path="/login" element={<LoginPage />} />
                <Route path="*" element={<NoPage />} />
            </Routes>
      </BrowserRouter>
      
    </>
  )
}

export default App

```

Giải thích:

- / => Load nội dung LayoutAdmin lên
- index : mặc định là trang DashboardPage 
- login : Load nội dung trang login lên
- `*` : Không tìm thấy url khớp với router thì load NoPage lên
