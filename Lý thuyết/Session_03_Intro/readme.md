# Tạo dự án mới bằng Reactjs

## Kết hợp vite (khuyên dùng)

1. tài liệu (https://duthanhduoc.com/blog/tao-du-an-react-vite-typescript-eslint)

2. chạy các lệnh sau đây
b0: New responsitories trên GitHub 
b1: tạo project mới 
- Với npm: `npm create vite@latest`
- với yarn: `yarn create vite`
b2. sau khi chạy lệnh trên xong thì nó sẽ yêu cầu nhập tên dự án
Lưu ý: Tên dự án trùng với tên responsitories vừa mới tạo
b3: chọn Framework: chọn `React`
b4: chọn template: ở đây chúng ta chọn `TypeScript + SWC`
Vì WC sẽ thay thế Babel cho việc biên dịch code Typescript/Javascript. SWC có tốc độ nhanh hơn 20 lần khi so với Babel
 => lúc này sẽ hiển thị thư mục vừa được Vite tạo ra -> dùng lệnh `cd <tên dự án>` để chuyển tới folder dự án
b5. Cài đặt các package cần thiết như : `yarn` or `npm i`
b6. chạy lệnh `yarn dev` để chạy dự án lên trình duyệt
=> lúc nảy sẽ xuất hiện local đường dẫn `http://localhost:5173/`

3. Ngoài chạy bằng vite. chúng ta có thể tạo project bằng node hoặc yarn

Bằng Node 

- Tạo project: 
```js
npx create-react-app name-of-app --template typescript
```
- các lệnh cơ bản của npm
```html
npm init –y #khởi tạo project mới , tạo ra file package.json

npm ls #xem danh sách gói 
npm ls -g #xem danh sách gói ở Global

npm install yarn -g # cài vào global
npm install axios --save # cài vào dependencies
npm install sass --save-dev # cài vào devDependencies

npm uninstall package_name # gở cài đặt 
npm update package_name #update gói

npm start # chạy trình duyệt
npm run build
npm run eject
```

Bằng yarn

- Tạo project 
```js
yarn create react-app my-app --template typescript
```
- Các lệnh cơ bản của yarn
```html
yarn init #Khởi tạo project mới, tạo ra file package.json

yarn add #Thêm gói cài đặt vào dependencies
yarn add --dev #Thêm gói cài đặt vào devDependencies

#Install packages globally on your operating system
yarn global add [package_name]

#Removing a dependency
yarn remove [package_name]

yarn up [package_name] #Upgrading a dependency

#Installing all the dependencies
yarn
yarn install

== chạy project
yarn start
yarn build
yarn test
yarn eject
```

## deploy lên github

Sau khi bạn đã push repository code máy tính bạn với github thì ta deploy lên github như sau

b1: chạy lệnh `npm install gh-pages --save-dev` , lúc nảy file package.json sẽ sinh thêm 1 thư viện `"gh-pages": "^6.1.1",`
b2. edit file package.json
- add thêm ` "homepage": "https://Jacklinh.github.io/my-app"
- add thêm trong scripts đoạn mã sau
```js 
"predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
```
đây là kết quả sau khi mình edit file package.json
( Với my-app là tên reponsitory trùng với name folder dự án)
```js
"scripts": {
    "dev": "vite",
    "build": "tsc && vite build",
    "lint": "eslint . --ext ts,tsx --report-unused-disable-directives --max-warnings 0",
    "preview": "vite preview",
    "predeploy": "npm run build",
    "deploy": "gh-pages -d dist"
  },
  "homepage": "https://Jacklinh.github.io/CV_Linhlv",
  "dependencies": {
    "react": "^18.2.0",
    "react-dom": "^18.2.0"
  },
```

b3: trong file vite.config.ts add thêm
`base: "/my-app",`

b4: chạy lệnh để deploy nó lên github.io
`npm run deploy`

=> lúc nảy lên git hub bạn chạy link homepage

