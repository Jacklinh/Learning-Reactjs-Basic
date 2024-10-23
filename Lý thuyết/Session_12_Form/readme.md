# Form

Trong React, mỗi khi người dùng nhập liệu vào form, chúng ta sử dụng state để theo dõi giá trị của các input fields. Điều này đảm bảo rằng giao diện (UI) luôn nhất quán với dữ liệu trong component

doc : <https://react.dev/learn/reacting-to-input-with-state>

## Cách Lấy dữ liệu từ các Input trong Form

Ví dụ bạn có một component `SimpleForm` như sau:

```jsx 
const SimpleForm = () => {
  return (
    <from>
      <input type="text" name="username" placeholder="Username" value="" />
      <button type="submit">Submit</button>
    </from>
  );
};
```
Để lấy được giá trị từ input username. Bạn cần một `state` để quản lý trạng thái thay đổi dữ liệu (value) của nó.

```jsx
import { useState } from "react";
const SimpleForm = () => {
    // tạo 1 state 
  const [userName, setUserName] = useState("");
  return (
    <from>
      <input
        type="text"
        name="username"
        placeholder="Username"
        value={userName}
        onChange={(e) => {
          //Lấy giá trị nhập từ input, gán lại cho state userName
          setUserName(e.target.value);
        }}
      />
      <button type="submit">Submit</button>
    </from>
  );
};
```

Giải thích:

- Khi bạn nhập dữ liệu vào input username. Sự kiện `onChange` được kích hoạt, và nó gọi hàm `setUserName` để cập nhật lại giá trị mới cho state `UserName`
- `value={userName}`: để input luôn được nhận giá trị từ người dùng nhập vào.

Tuy nhiên khi bạn click nút `submit` thì form nó làm load lại page. ==> Bạn không kiểm soát được Form.

Bạn cần làm bước tiếp theo: tạo sự kiện `onSubmit`

```jsx
import { useState } from "react";
const SimpleForm = () => {
  const [userName, setUserName] = useState("");
  return (
    <from
      onSubmit={(e) => {
        e.preventDefault(); //Ngăn không cho load lại page
        //Bạn có thể kiểm soát giá trị lấy được userName từ đây.
        console.log(userName);
      }}
    >
      <input
        type="text"
        name="username"
        placeholder="Username"
        value={userName}
        onChange={(e) => {
          //Lấy giá trị nhập từ input, gán lại cho state userName
          setUserName(e.target.value);
        }}
      />
      <button type="submit">Submit</button>
    </from>
  );
};
```

## Input Value quản lý một State tương ứng

Dưới đây là một ví dụ về form trong React, trong đó mỗi loại input sẽ được lưu trữ trong một **state** riêng biệt tương ứng với từng trường nhập liệu:

```jsx
import React, { useState } from "react";

function SeparateStateForm() {
  // Khai báo state riêng cho từng input
  const [username, setUsername] = useState("");
  const [email, setEmail] = useState("");
  const [password, setPassword] = useState("");
  const [gender, setGender] = useState("");
  const [description, setDescription] = useState("");
  const [agreement, setAgreement] = useState(false);

  // Xử lý sự thay đổi giá trị của từng input
  const handleSubmit = (e) => {
    e.preventDefault();
    alert(`
      Tên người dùng: ${username}
      Email: ${email}
      Mật khẩu: ${password}
      Giới tính: ${gender}
      Mô tả: ${description}
      Đồng ý điều khoản: ${agreement ? "Có" : "Không"}
    `);
  };

  return (
    <form onSubmit={handleSubmit}>
      {/* Input cho Username */}
      <label>
        Username:
        <input
          type="text"
          value={username}
          onChange={(e) => setUsername(e.target.value)}
        />
      </label>
      <br />

      {/* Input cho Email */}
      <label>
        Email:
        <input
          type="email"
          value={email}
          onChange={(e) => setEmail(e.target.value)}
        />
      </label>
      <br />

      {/* Input cho Password */}
      <label>
        Password:
        <input
          type="password"
          value={password}
          onChange={(e) => setPassword(e.target.value)}
        />
      </label>
      <br />

      {/* Input cho Gender (radio button) */}
      <label>
        Giới tính:
        <label>
          <input
            type="radio"
            value="Nam"
            checked={gender === "Nam"}
            onChange={(e) => setGender(e.target.value)}
          />{" "}
          Nam
        </label>
        <label>
          <input
            type="radio"
            value="Nữ"
            checked={gender === "Nữ"}
            onChange={(e) => setGender(e.target.value)}
          />{" "}
          Nữ
        </label>
      </label>
      <br />

      {/* Input cho Textarea */}
      <label>
        Mô tả về bản thân:
        <textarea
          value={description}
          onChange={(e) => setDescription(e.target.value)}
        />
      </label>
      <br />

      {/* Input cho Checkbox */}
      <label>
        <input
          type="checkbox"
          checked={agreement}
          onChange={(e) => setAgreement(e.target.checked)}
        />
        Tôi đồng ý với các điều khoản
      </label>
      <br />

      <button type="submit">Gửi</button>
    </form>
  );
}

export default SeparateStateForm;
```
Giải thích:
- **State riêng cho mỗi input**:
  - `username`, `email`, `password`, `gender`, `description`, và `agreement` là những state riêng biệt, mỗi state lưu giá trị của một input.
- **Xử lý sự kiện**:
  - Khi người dùng nhập liệu hoặc thay đổi giá trị trong input, ta sử dụng hàm `onChange` để cập nhật state tương ứng.
  - Ví dụ: `setUsername(e.target.value)` cập nhật state `username` với giá trị mà người dùng đã nhập.
- **Submit form**:
  - Khi người dùng bấm nút submit, hàm `handleSubmit` sẽ ngăn trang reload (bằng `e.preventDefault()`) và hiển thị thông tin từ các state.

Form này bao gồm nhiều loại input: text, email, password, radio button, textarea và checkbox, với mỗi loại input tương ứng với một state riêng để quản lý giá trị.

## Lấy value từ Multi Checkbox

```js
import React, { useState } from "react";
const courses = [
  { id: 1, name: "Html" },
  { id: 2, name: "Javascript" },
  { id: 3, name: "React Js" },
];
export default function myForm() {
    // mảng chứa id khi checked 
  const [checked, setChecked] = useState([]);

  const handelCheck = (id) => {
    setChecked((prev) => {
        // dùng includes để kiểm tra id có nằm trong mảng checked hay không?
      const isCheck = checked.includes(id);
      //Bỏ check nếu đã check(id đã tồn tại , dùng filter để loại bỏ id đó ra khỏi checked )
      if (isCheck) {
        return checked.filter((item) => item !== id);
      }
      //Còn lại thêm mới để check bằng cách dùng destructuring array 
      return [...prev, id];
    });
  };

  const handleSubmit = () => {
    console.log(checked);
  };

  return (
    <div>
      {courses.map((course) => {
        <label key={course.id}>
          <input
            type="checkbox"
            checked={checked.includes(course.id)}
            onChange={() => handelCheck(course.id)}
          />
          {course.name}
        </label>;
      })}

      <button type="submit" onClick={handleSubmit}>
        Submit
      </button>
    </div>
  );
}
```

## Khi Form có quá nhiều trường

Có một giải pháp để quản lý State được tối ưu hơn là gom tất cả các state thành một Object.

```js
import React, { ChangeEvent, FormEvent, useState } from 'react';

function SignupFormWithValidation() {
    // sử dụng state object 
  const [inputs, setInputs] = useState({
    userName: '',
    passwords: '',
    gender: '',
    favoriteFruit: '',
    acceptTerms: false,
    comments: '',
  });

  //Dùng 1 hàm Handle dữ liệu chung
  const handleChange = (e: ChangeEvent<HTMLInputElement | HTMLSelectElement | HTMLTextAreaElement>) => {
    if (e.target.type === 'checkbox') {
      const target = e.target as HTMLInputElement; // Kiểm tra kiểu
      setInputs((values) => ({ ...values, [target.name]: target.checked }));
    } else {
      const target = e.target as HTMLInputElement | HTMLSelectElement | HTMLTextAreaElement; // Kiểm tra kiểu
      setInputs((values) => ({ ...values, [target.name]: target.value }));
    }
  };

  //Tạo một hàm check thông tin cơ bản
  const validateForm = () => {
    const newErrors = {};
    if (!inputs.userName) newErrors.userName = 'Username là bắt buộc';
    if (inputs.password.length < 6) newErrors.password = 'Mật khẩu phải có ít nhất 6 ký tự';
    //Thêm logic ở đây
    return newErrors;
  };

  const handleSubmit = (event: FormEvent<HTMLFormElement>) => {
    event.preventDefault();
    const validationErrors = validateForm();
    if (Object.keys(validationErrors).length > 0) {
      setErrors(validationErrors);
    } else {
      console.log('Dữ liệu hợp lệ:', formData);
    }
  };

  return (
    <form onSubmit={handleSubmit}>
      <label>
        Enter your name:
        <input type="text" name="userName" value={inputs.userName} onChange={handleChange} />
      </label>
      <br />
      <label>
        Enter your password:
        <input type="password" name="passwords" value={inputs.passwords} onChange={handleChange} />
      </label>
      <br />
      <label>
        Select your gender:
        <select name="gender" value={inputs.gender} onChange={handleChange}>
          <option value="">Select</option>
          <option value="male">Male</option>
          <option value="female">Female</option>
          <option value="other">Other</option>
        </select>
      </label>
      <br />
      <label>
        Select your favorite fruit:
        <input
          type="radio"
          name="favoriteFruit"
          value="apple"
          checked={inputs.favoriteFruit === 'apple'}
          onChange={handleChange}
        />
        Apple
        <input
          type="radio"
          name="favoriteFruit"
          value="banana"
          checked={inputs.favoriteFruit === 'banana'}
          onChange={handleChange}
        />
        Banana
        <input
          type="radio"
          name="favoriteFruit"
          value="orange"
          checked={inputs.favoriteFruit === 'orange'}
          onChange={handleChange}
        />
        Orange
      </label>
      <br />
      <label>
        Accept terms and conditions:
        <input
          type="checkbox"
          name="acceptTerms"
          checked={inputs.acceptTerms}
          onChange={handleChange}
        />
      </label>
      <br />
      <label>
        Enter your comments:
        <textarea name="comments" value={inputs.comments} onChange={handleChange} />
      </label>
      <br />
      <button type="submit">Submit</button>
    </form>
  );
}

export default SignupFormWithValidation;

```

## sử dụng thư viện hỗ trợ form 

Thay vì đi làm việc thủ công với FORM như vậy thì có các thư viện giúp xử lí form nhanh hơn, kèm theo tính năng validate form, báo lỗi....

### React hook form

```bash
npm install react-hook-form
```

Tài liệu : <https://react-hook-form.com/get-started/#Quickstart>

React Hook Form with Yup Validation

Bạn cần cài thêm

```bash
npm install @hookform/resolvers yup
```

Dưới đây là một ví dụ về dùng React Hook Form + Validate dữ liệu lấy từ Inputs

```js
import { useForm } from "react-hook-form";
import { yupResolver } from "@hookform/resolvers/yup";
import * as yup from "yup";

//validate with yup
const schema = yup
  .object({
    firstName: yup.string().required(),
    age: yup.number().positive().integer().required(),
  })
  .required();

//Typescript for Form Data
type FormData = yup.InferType<typeof schema>;

export default function MyForm() {
  //Su dụng React hook form
  const {
    register,
    handleSubmit,
    formState: { errors },
  } = useForm <
  FormData >
  {
    resolver: yupResolver(schema),
  };
  //Bắt sự kiện Onsubmit Form
  const onSubmit = (data: FormData) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("firstName")} />
      <p>{errors.firstName?.message}</p>

      <input {...register("age")} />
      <p>{errors.age?.message}</p>

      <input type="submit" />
    </form>
  );
}
```

Để thêm một trường mới, bạn chỉ cần thêm 2 dòng này:

```jsx
<input {...register("age")} />
<p>{errors.age?.message}</p>
```

Trong đó:

- `{...register("age")}` là cú pháp bạn khai báo `name` cho input
- `{errors.age?.message}` để hiển thị lỗi khi dữ liệu bạn nhập vào input không hợp lệ.

Nếu bạn muốn sử dụng các tính năng validation cơ bản của HTML5 bạn có thể làm như sau:

```jsx
<input {...register("age", {require: true, min: 18, max: 100 })} />
<p>{errors.age?.message}</p>
```

==> Bạn thêm vào hàm `register` tham số thứ 2 là một Object

Chi tiết xem: https://react-hook-form.com/get-started#Registerfields

Để bắt các trạng thái submit trong React Hook Form, bạn có thể sử dụng thuộc tính `handleSubmit` và `isSubmitting` được cung cấp bởi React Hook Form. Dưới đây là một ví dụ:

```jsx

export default function MyForm() {
  const { register, handleSubmit, formState: { errors, isSubmitting } } = useForm<FormData>({
    resolver: yupResolver(schema)
  });
  const onSubmit = (data: FormData) => console.log(data);

  return (
    <form onSubmit={handleSubmit(onSubmit)}>
      <input {...register("firstName")} />
      <p>{errors.firstName?.message}</p>

      <input {...register("age")} />
      <p>{errors.age?.message}</p>

       <button type="submit" disabled={isSubmitting}>
        {isSubmitting ? 'Submitting...' : 'Submit'}
      </button>
    </form>
  );

}

export default MyForm;
```

Trong ví dụ trên, chúng ta sử dụng `useForm` từ React Hook Form để tạo ra các phương thức và thuộc tính cần thiết cho form.

- `handleSubmit` là một phương thức được cung cấp bởi React Hook Form và được gắn vào sự kiện `onSubmit` của form. Khi form được gửi đi, `handleSubmit` sẽ chạy hàm `onSubmit` được định nghĩa bởi bạn.
- `isSubmitting` là một thuộc tính trong `formState` được cung cấp bởi React Hook Form. Nó sẽ có giá trị `true` khi form đang trong quá trình submit và `false` khi quá trình submit hoàn thành.

Trong phần giao diện của form, chúng ta có một nút submit được kích hoạt hoặc vô hiệu hóa dựa trên giá trị của `isSubmitting`. Khi form đang được submit, nút submit sẽ bị vô hiệu hóa và hiển thị thông báo "Submitting...".

Ngược lại, khi không có quá trình submit nào diễn ra, nút sẽ được kích hoạt và hiển thị "Submit".

==> Giúp tránh cho người dùng nhấn Submit liên tục

### Yup validation

Sử dụng để validate form trong React

```bash
npm install yup --save
```

Cách sử dụng: <https://github.com/jquense/yup>

---

Dưới đây là một ví dụ về đối tượng "user" với nhiều trường và các quy tắc xác thực tương ứng bằng Yup:

```javascript
import * as yup from "yup";

const schema = yup.object().shape({
  username: yup
    .string()
    .min(4, "Tên tối thiểu 4 kí tự")
    .max(20, "Tối đa 2o kí tự")
    .required("Username is required"),

  nickName: yup.string().default("").nullable(), //mặc định là '' nếu ko điền, chấp nhận giá trị null
  email: yup
    .string()
    .lowercase()
    .trim()
    .email("Invalid email")
    .required("Email is required"),
  birthDate: yup.date().nullable().min(new Date(1900, 0, 1)), //kiểu ngày tháng năm, chấp nhật null
  website: yup.string().url().optional(), //kiểu  string | undefine
  age: yup
    .number()
    .integer()
    .min(18, "Age must be at least 18")
    .required("Age is required"),
  password: yup
    .string()
    .min(6, "Password must be at least 6 characters")
    .required("Password is required")
    .matches(
      /^(?=.*\d)(?=.*[a-z])(?=.*[A-Z])[0-9a-zA-Z]{8,}$/,
      "Mật khẩu không đúng định dạng"
    ),
  confirmPassword: yup
    .string()
    .oneOf([yup.ref("password"), null], "Passwords must match")
    .required("Confirm Password is required"),
  gender: yup
    .string()
    .oneOf(["male", "female"], "Invalid gender")
    .required("Gender is required"),
});

const user = {
  username: "john_doe",
  email: "john@example.com",
  age: 25,
  password: "password123",
  confirmPassword: "password123",
  gender: "male",
};

schema
  .validate(user)
  .then((valid) => console.log(valid))
  .catch((error) => console.log(error));
```

Trong ví dụ trên, chúng ta đã sử dụng Yup để tạo một schema đối tượng cho "user". Các trường của "user" bao gồm `username`, `email`, `age`, `password`, `confirmPassword`, và `gender`. Mỗi trường được định nghĩa với các quy tắc xác thực tương ứng.

- `username` phải là một chuỗi bắt buộc, tối thiểu 4 kí tự, tối đa 20 kí tự
- `email` phải là một chuỗi hợp lệ đại diện cho địa chỉ email. covert thành chữ thường, bỏ kí tự trống 2 đầu.
- `nickName`: là chuỗi, mặc định ko điền là `""`, chấp nhận giá trị `null`
- `birthDate`: là kiểu ngày tháng, chấp nhận giá trị `null`, năm tối thiểu 1990
- `website`: là chuỗi, định dạng url, chấp nhận giá trị `undefine`
- `age` phải là một số nguyên dương và ít nhất 18 tuổi.
- `password` phải là một chuỗi có ít nhất 6 ký tự.
- `confirmPassword` phải giống với giá trị của trường `password`.
- `gender` phải là một trong hai giá trị "male" hoặc "female".

Nếu các giá trị của "user" không tuân thủ các quy tắc xác thực tương ứng, Yup sẽ sinh ra các lỗi tương ứng. Bằng cách sử dụng phương thức `validate` của schema, chúng ta có thể kiểm tra xem "user" có hợp lệ hay không và xử lý các lỗi nếu có.

### Formik

Ngoài React Hook Form bạn có thêm một lựa chọn nữa khá tốt là `Formik`

```bash
npm install formik --save
```

Example: <https://formik.org/docs/tutorial#a-simple-newsletter-signup-form>

Formik với Yup Validation

Doc: <https://formik.org/docs/guides/validation>

---


