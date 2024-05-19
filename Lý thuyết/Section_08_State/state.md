# State and Lifecycle

>** Nội dung cần nhớ **
>
>- State là gì?
>- Khái niệm One-way/ Two-way binding
>- State và Lifecycle
>- Cách update 1 state


## State

> State: a component's memory

- doc : <https://react.dev/learn/state-a-components-memory>

State giống như 1 kho lưu trữ dữ liệu cho các component trong Reactjs.
State là 1 khái niệm quan trong để lưu trữ và quản lý dữ liệu của 1 component khi người dùng thực hiện 1 số hành động như click, nhập văn bản, thao tác phím ... làm thay đổi kết quả hiển thị ra màn hình.

State được sử dụng để lưu trữ các giá trị dữ liệu , có thể thay đổi, như thông tin người dùng nhập vào, kết quả của 1 tác vụ bất đồng bộ hoặc bất kỳ dữ liệu nào mà thành phần cần theo dõi và sử dụng trong quá trình render lại giao diện người dùng.

Khi giá trị của state thay đổi  thì react sẽ thực hiện quá trình render lại những thành phần thay đổi cần thiết của giao diện người dùng, không phải render lại toàn bộ gọi là `re-render`

Đặc điểm của state:

- `riêng tư`: nó chỉ hoạt động trong phạm vi của component đó thôi. ( không thể truy cập từ các component khác)
- `Có thể thay đổi `: có thể thay đổi dữ liệu dựa theo phản ứng lại các sự kiện của người dùng hoặc dữ liệu đầu vào.
- `Khởi tạo `: state thường khởi tạo trong constructor(với class component) hoặc bằng hook 'useState` (với function component).

## Khởi tạo 1 state

vd có biến count và 1 button, khi click button thì count tăng lên 1 giá trị
```js
import React from 'react';
//import { useState } from 'react'; // cách 2: sử dụng hook 'useState' 
export default function Count() {
    // Tạo một State count, sử dụng hook useState
    const [count, setCount] = React.useState(0); // cách 1
    //const [count, setCount] = useState(0); // cách 2
    const increase()=> {
        setCount(count + 1);
    }
    return (
        <h1>{count}</h1>
        <button onClick={increase()}>
        Increase
      </button>
    )
}
```

cú pháp tạo 1 State( bằng cách sử dụng array destrucuring)
```js
// const [state,setState] = React.useState(initialState);
const [count, setCount] = React.useState(0);
```

bản chất `React.useState(0)` là 1 function return về 1 mảng gồm 2 phần tử.
`[count, setCount]` là đang sử dụng cú pháp `array destrucuring` của javascript
- count: tên của state
- setCount: là phương thức để thay đổi giá trị của state tương ứng.

### khi nào thì cần đến State

Bất cứ khi nào dữ liệu thay đổi trong 1 component, state có thể được sử dụng

-vd: tử ẩn sang hiện, từ không thành có... thay đổi trạng thái before after
-vd: nhập dữ liệu đầu vào trong form. lúc này người dùng thay đổi thì cần phải `re-rendering` của component, ta phải sử dụng `state`

TH1. quản lý dữ liệu người dùng nhập vào

```js
const MyForm = () => {
  const [inputValue, setInputValue] = useState('');

  const handleChange = (event) => {
    setInputValue(event.target.value);
  };

  return (
    <input type="text" value={inputValue} onChange={handleChange} />
  );
};
```

TH2. theo dõi trạng thái tương tác của người dùng

các tương tác như click, hover, scroll có thể thay đổi giao diện người dùng, lúc này bạn sẽ sử dụng state để quản lý thay đổi
```js
const ToggleButton = () => {
  const [isToggled, setIsToggled] = useState(false);

  const handleClick = () => {
    setIsToggled(!isToggled);
  };

  return (
    <button onClick={handleClick}>
      {isToggled ? 'ON' : 'OFF'}
    </button>
  );
};
```

TH3. quản lý dữ liệu từ API

khi bạn lấy dữ liệu từ 1 API hoặc bất kỳ 1 nguồn dữ liệu bên ngoài thì bạn cần sử dụng state để lưu trữ và hiển thị nó.

```js
import React, { useState, useEffect } from 'react';

const DataFetcher = () => {
  const [data, setData] = useState([]);

  useEffect(() => {
    fetch('https://api.example.com/data')
      .then(response => response.json())
      .then(data => setData(data));
  }, []);

  return (
    <ul>
      {data.map(item => (
        <li key={item.id}>{item.name}</li>
      ))}
    </ul>
  );
};
```

TH4. quản lý trạng thái giao diện

state cũng thường được sử dụng để điều khiển các thành phần giao diện động như `modal, dropdown, tooltip` hoặc bất kỳ phần tử nào cần hiển thị, ẩn dựa trên trạng thái hiện tại

```js
const ModalExample = () => {
  const [isOpen, setIsOpen] = useState(false);

  const toggleModal = () => {
    setIsOpen(!isOpen);
  };

  return (
    <div>
      <button onClick={toggleModal}>Toggle Modal</button>
      {isOpen && (
        <div className="modal">
          <p>Modal Content</p>
        </div>
      )}
    </div>
  );
};
```

TH5. Theo dõi và quản lý trạng thái của component phức tạp

khi component của bạn có nhiều trạng thái khác nhau, ảnh hưởng đến cách nó hiển thị hoặc hoạt động. thì ta sử dụng state để quản lý các trạng thái này

```js
const MultiStateComponent = () => {
  const [step, setStep] = useState(1);

  const nextStep = () => {
    setStep(step + 1);
  };

  return (
    <div>
      <p>Current Step: {step}</p>
      <button onClick={nextStep}>Next Step</button>
    </div>
  );
};
```

### State hoạt động như thế nào?

State là 1 đối tượng chứa thông tin dữ liệu và trạng thái của 1 component

State có thể là bất kỳ kiểu dữ liệu nào, bao gồm cả number, string, array, object, boolean.

khi State thay đổi, react sẽ tự động render lại giao diện người dùng của component để phàn ánh các thay đổi mới.( tức là React sẽ so sánh giá trị cũ và mới của state và chỉ cập nhật những phần cần thiết của giao diện người dùng)

Để thay đổi giá trị của state, ta cần sử dụng phương thức `setState()`. khi gọi `setState()` react sẽ thực hiện quá trình so sánh và cập nhật giao diện người dùng nếu cần.

## One-Way / Two-way binding

1. One-way data binding(ràng buộc dữ liệu 1 chiều)

One-way nó đề cập đến hiển thị và quản lý dữ liệu trong các component di chuyển theo 1 hướng: từ nguồn dữ liệu(data model) đến giao diện người dùng(UI). từ phần tử cha `parent component` xuống các phần tử con `child component`.

Lúc này, bất kỳ thay đổi nào từ dữ liệu hoặc `parent component` sẽ được cập nhật lại giao diện người dùng và `child component`. Nhưng thay đổi từ phần tử con thì sẽ không tự động cập nhật lại dữ liệu cha => gọi là `đơn luồng`

trong react, One-way data binding là cơ chế mặc định. dữ liệu trong component được truyền từ state/props vào giao diện thông qua phương thức `render`

Đặc điểm của One-way data binding

- đơn luồng: dữ liệu di chuyển theo 1 hướng duy nhất từ nguồn đến giao diện.
- dễ kiểm soát: do dữ liệu chỉ duy chuyển theo 1 hướng , nên việc theo dõi và fix bug các thay đổi dữ liệu trở nên dễ dàng hơn
- cấu trúc rõ ràng: giúp tách biệt rõ ràng giữa logic và giao diện. giữ mã nguồn dễ đọc và bảo trì.

vd
```js
import React, { useState } from 'react';

const MyComponent = () => {
  const [message, setMessage] = useState('Hello, World!');

  return (
    <div>
      <p>{message}</p>
    </div>
  );
};

export default MyComponent;
```

trong ví dụ trên

- `message` là state của component
- `message` được truyền vào giao diện thông qua JSX `<p>{message}</p>`
- khi giá trị của `message` thay đổi, giao diện sẽ tự động cập nhật để phản ánh sự thay đổi của nó.

2. two-way data binding ( ràng buộc dữ liệu 2 chiều)

là việc đồng bộ dữ liệu giữa state và UI(ví dụ như nhập dữ liệu vào form) sẽ cập nhật state và mọi thay đổi trong state sẽ cập nhật lại UI.cho phép tạo ra các form và giao diện người dùng tương tác 1 cách dễ dàng và hiệu quả.

Để thực hiện two-way data binding trong react, bạn có thể sử dụng các thư viện như `react-redux` hoặc `formik` để quản lý trạng thái và đồng bộ hóa dữ liệu giữa component và giao diện người dùng (UI)

vd cơ bản với input

```js
import React, { useState } from 'react';

const MyComponent = () => {
  const [text, setText] = useState(''); // khởi tạo state

  const handleChange = (event) => {
    setText(event.target.value); // xử lý sự kiện và cập nhật lại state
  };

  return (
    <div>
      <input type="text" value={text} onChange={handleChange} /> // lắng nghe sự kiện thay đổi và truyền hàm `handleChange`
      <p>{text}</p> // cập nhật UI
    </div>
  );
};

export default MyComponent;
```
lúc này mỗi khi state `text` thay đổi thì react sẽ re-render component và cập nhật giá trị hiển thị trong `p`.

vd với form phức tạp
lúc này chúng ta có thể quản lý nhiều state và các trường dữ liệu bằng cách sử dụng các đối tượng và nhiều hàm xử lý sự kiện
```js
import React, { useState } from 'react';

const MyForm = () => {
    // khởi tạo state formDate chứa username,email
  const [formData, setFormData] = useState({
    username: '',
    email: ''
  });
// mỗi khi giá trị của 1 input thay đổi thì hàm handleChange được gọi
// name và value của input được lấy từ event.target
// setFormData cập nhaath state `formData` với giá trị tương ứng 
  const handleChange = (event) => {
    const { name, value } = event.target;
    setFormData({
      ...formData,
      [name]: value
    });
  };

  return (
    <div>
      <form>
        // gán giá trị cho input
        <label>
          Username:
          <input
            type="text"
            name="username"
            value={formData.username}
            onChange={handleChange}
          />
        </label>
        <br />
        <label>
          Email:
          <input
            type="email"
            name="email"
            value={formData.email}
            onChange={handleChange}
          />
        </label>
      </form>
      <h3>Entered Information:</h3>
      // cập nhật UI, mỗi khi formData thay đổi thì react sẽ re-render component và cập nhật lại giá trị hiển thị
      <p>Username: {formData.username}</p>
      <p>Email: {formData.email}</p>
    </div>
  );
};

export default MyForm;

```

## Lifecycle(Vòng đời của 1 component)

1. Re-render trong react là gì

khi nói về performances của react, có 2 giai đoạn chính mà chúng ta cần quan tâm:
 - `initial render`: khởi chạy App, react gọi Root component bằng cách gọi `createRoot` tạo DOM và chạy hàm `render` để render component hiển thị ra màn hình.
 - `Re-render`: xảy ra khi react cần update app 1 số giá trị mới. Thông thường, điều này xảy ra khi người dùng tương tác với ứng dụng (event handing) hoặc 1 số dữ liệu bên ngoài đến thông qua 1 yêu cầu bất đồng bộ như call api lấy data đổ về.

 2. khi nào và tại sao 1 component phải render?

 có 2 lý do để 1 component render
 - render lần đầu tiên (initial render)
 - state của component hoặc component cha của nó thay đổi

 3. Các bước liên quan đến việc hiển thị 1 thành phần trên màn hình

 quá trình xử lý yêu cầu tương tác từ giao diện người dùng có 3 bước

 - triggering a render (vd như nhận yêu cầu order từ khách đưa cho nhà bếp)
 - Rendering the component (Nhà bếp chuẩn bị theo order)
 - Committing to the DOM(Mang món ra bàn cho khách)

 chu trình này trong react component còn được hiểu với khái niệm đó là `lifeCycle`

 - `Mounting` ( component được sinh ra - gọi món)
 - `Updating` ( component tồn tại và thay đổi - chuẩn bị món)
 - `Unmounting` ( component mất đi - mang món ăn cho khách)

image mô hình lifeCycle với class component : <https://projects.wojtekmaj.pl/react-lifecycle-methods-diagram/>
Tài liệu đọc thêm <https://viblo.asia/p/lifecycle-component-trong-reactjs-gGJ59jzxKX2>

Như vậy lifeCycle nói trên sẽ diễn ra như sau:

step 1: kích hoạt render

- initial render

Khởi chạy app, react gọi Root Component bằng cách gọi createRoot tạo DOM và chạy hàm render để render component hiển thị ra màn hình

```js
import Image from "./Image.js";
import { createRoot } from "react-dom/client";

const root = createRoot(document.getElementById("root"));
root.render(<Image />);
```

- state update

1 component đã được render trước đó (initial render) bạn có thể kích hoạt lại quá trình render bằng cách thay đổi `state` thông qua phương thức `setState`
(vd như 1 vị khách order món ăn, sau khi gọi món đầu tiền, tùy thuộc vào tình trạng khát hay đói họ có thể gọi thêm)

step 2: Mounting 

sau khi kích hoạt 1 render, react gọi đến component lấy nội dung hiển thị ra màn hình.
Thuật ngữ `rendering` nghĩa là react đang gọi đến component của bạn

- Trong lần render đầu tiên (initial render)
    - React sẽ gọi root component
    - React sẽ tạo các DOM Node
- Các lần render tiếp theo
    - react gọi đến function component có state thay đổi đã kích hoạt render
    - React sẽ tính toán so sánh các thuộc tính của chúng (state), nếu k có bất kỳ thay đổi nào kể từ lần render trước đó thì react bỏ qua và đến giai đoạn tiếp theo commit.
 
Giải thích các thuật ngữ

- `constructor`: được gọi khi 1 component được khởi tạo. đây là nơi bạn có thể khởi tạo `state` và ràng buộc các phương thức
- `render`: bắt buộc phải có, phương thức trả về JSX để hiển thị UI
- `componentDIdMount` : được gọi ngay sau khi component được chèn vào DOM. đây là nơi thích hợp để thực hiện các yêu cầu Ajax

step 3: Updating ( react cập nhật thay đổi đến DOM)

Giai đoạn này xảy ra khi props hoặc state của component thay đổi.
- trong lần render đầu tiên(initial render): React sử dụng phương thức `appendChild()` DOM API để đặt tất cả các DOM nodes vào nó đã tạo vào `<div id="root">` để hiển thị ra màn hình

- Re-render:
    -  React tạo ra `virtual DOM`, react sử dụng thuật toán `Diffing` để nhận biết được đã có điều gì khác hoặc thay đổi trong virtual DOM.
    - Tiếp theo là `Reconciliation`: virtual DOM sẽ được cập nhật lại với kết qua sau khi sử dụng thuật toán Diffing
    - React chỉ update lại những gì thay đổi vào `real DOM` ( DOM thật)

Giải thích các thuật ngữ  thuật toán `Diffing

- thường được gọi là Virtual DOM Diffing Algorithm dùng để cập nhật giao diện người dùng
- mục tiêu của thuật toán này là xác định sự khác biệt giữa `virtual DOM` hiện tại và `virtual DOM` mới để cập nhật `DOM thực tế` với các thay đổi cần thiết mà không cần thay đổi toàn bộ cây DOM
- `virtual DOM` là 1 bản sao nhẹ của `DOM thực ` được giữ trong bộ nhớ và đồng bộ với DOM thực thông qua thuật toán Diffing. khi state hoặc props thay đổi thì react sẽ tạo 1 virtual DOM mới và so sánh với virtual DOM cũ để xác định sự khác biệt
- Để giúp tối ưu hóa quá trình diffing, react khuyến khích sử dụng thuộc tính `key` cho các danh sách phần tử
```js
const listItems = items.map((item) => <li key={item.id}>{item.text}</li>);

```

step 4: Unmounting ( kết thúc và update lại trình duyệt)

sau khi rendering xong và react update lại DOM, trình duyệt sẽ vẻ lại màn hình


## State là 1 Snapshot

Giống như mỗi giây thời gian trôi qua , bạn không thể lấy lại được.

State cũng vậy, mỗi lần component nó render thì nó tạo ra giá trị tham chiếu mới hay còn gọi là `Snapshot` ( 1 ảnh chụp) tại thời điểm đó
tài liệu : <https://react.dev/learn/state-as-a-snapshot#setting-state-triggers-renders>

1. Khi state thay đổi kích hoạt render

1 khi state thay đổi thì component sẽ re-render lại để `setState` theo giá trị mới

2. rendering takes a Snapshot in time (cập nhật giao diện thông qua việc render component)

mỗi lần react render 1 component thì nó "chụp" lại 1 bản sao trang thái hiện tại của component và tạo ra 1 giao diện người dùng dựa trên bản sao tại thời điểm đó.

Khi React re-render lại 1 component
- react gọi đến component fuction 1 lần nữa
- function trả lại 1 jsx snashot mới
- react sau đó update màn hình để khớp với snashot mà bạn trả lại

```js
import React, { useState } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);
  const [snapshot, setSnapshot] = useState(null);

  const saveSnapshot = () => {
    setSnapshot(count); // Lưu giá trị của state vào snapshot
  };

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={saveSnapshot}>Save Snapshot</button>
      <p>State Snapshot: {snapshot}</p> {/* Hiển thị state snapshot */}
    </div>
  );
};
```

## State updates ( cập nhật state)

1. khi state là queueing = hàng đợi

tài liệu: <https://react.dev/learn/queueing-a-series-of-state-updates>

queueing state thường ám chỉ việc sử dụng 1 hàng đợi để quản lý cập nhật state của các component. sử dụng các trường hợp khi cập nhât state thực hiện 1 cách thứ tự cụ thể 

Cách 1: cập nhật liên tục sử dụng `setState(state + number)`

```js
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      // mỗi khi click state sẽ được cập nhật liên tục cộng dồn 1-> 2 -> 3 ...
      <button onClick={() => {
        setNumber(number + 1);
        setNumber(number + 1);
        setNumber(number + 1);
      }}>+3</button>
    </>
  )
}
```

cách 2: Updating the same state multiple times before the next render 
 (cập nhật liên tục state theo bước nhảy kết quả trước)

 lúc này ta sử dụng biến n gán bằng giá trị trước đó `setNumber(n => n + 1)`

 ```js
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      // mỗi khi click state sẽ được cập nhật từ 0-> 3 > 6 ...
      <button onClick={() => {
        setNumber(n => n + 1);
        setNumber(n => n + 1);
        setNumber(n => n + 1);
      }}>+3</button>
    </>
  )
}

```

Cách 3: cập nhật state sau khi thay thế nó

```js
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
      // output: 0 > 6 > 12 > ....
      <button onClick={() => {
        setNumber(number + 5); // ban đầu thay đổi nó lên 0 + 5 = 5
        setNumber(n => n + 1); // lấy giá trị 5 + 1 = 6 -> khi click tiếp theo sẽ là 6 + 6 = 12
      }}>Increase the number</button>
    </>
  )
}
```

cách 4: update giá trị mặc định sau khi thay đổi state

```js
import { useState } from 'react';

export default function Counter() {
  const [number, setNumber] = useState(0);

  return (
    <>
      <h1>{number}</h1>
        // output ra trực tiếp giá trị 42
      <button onClick={() => {
        setNumber(number + 5);
        setNumber(n => n + 1);
        setNumber(42);
      }}>Increase the number</button>
    </>
  )
}
```

2. Khi state là 1 object

khi state là 1 object thì ta update state như sau:

Doc: <https://react.dev/learn/updating-objects-in-state>

```js
//App.js
import { useState } from "react";

export default function Form() {
  const [person, setPerson] = useState({
    firstName: "Barbara",
    lastName: "Hepworth",
    email: "bhepworth@sculpture.com",
  });

  //Method 1: set cứng dữ diệu
  setPerson({
    firstName: "Alexander",
    lastName: "Brahma",
    email: "alexander@gmail.com",
  });
  //Method 2: ... object spread syntax ( dùng destructuring object để tạo 1 object mới "...person" , dễ dàng cập nhật lại dữ liệu)
  setPerson({
    ...person, // Copy the old fields
    firstName: "Alexander", // But override this one
  });
}
```
or 

```js
import React, { useState } from 'react';

const MyComponent = () => {
  // khởi tạo state
  const [user, setUser] = useState({ name: 'John', age: 30 });
  // hàm cập nhật state: sử dụng destructuring object "...prevUser" để sao chép các thuộc tính hiện tại và chỉ cập nhật những thuộc tính cần thiết
  const updateName = (newName) => {
    setUser(prevUser => ({
      ...prevUser,
      name: newName
    }));
  };

  return (
    <div>
      <p>Name: {user.name}</p>
      <p>Age: {user.age}</p>
      <button onClick={() => updateName('Jane')}>Update Name</button> // cập nhật với giá trị mới
    </div>
  );
};

export default MyComponent;
```

> `có thể sử dụng thư viện  immer <https://github.com/immerjs/use-immer> cho phép bạn làm việc với state 1 cách dễ dàng hơn`


3. Khi state là 1 mảng

Doc:  <https://react.dev/learn/updating-arrays-in-state>

khi thao tác với mảng trong react, bạn cần tránh hạn chế sử dụng cột bên trái `avoid` . nên sử dụng cột bên phải `prefer` thay thế
|               |      avoid (không nên dùng)         |  prefer (nên dùng)  |
| :-----------: | :---------------------------------: | :----------------------------: |
|  **adding**   |          `push`, `unshift`          | concat, [...arr] spread syntax |
| **removing**  |      `pop`, `shift`, `splice`       |       `filter`, `slice`        |
| **replacing** | `splice`, `arr[i] = ... assignment` |             `map`              |
|  **sorting**  |          `reverse`, `sort`          |      copy the array first      |

TH1: thêm phần tử vào mảng
vd: khi dùng `push` thì setState không thể cập nhập lại data ( không nên dùng)
```js
import { useState } from 'react';

let nextId = 0;

export default function List() {
  const [name, setName] = useState('');
  const [artists, setArtists] = useState([]);

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <input
        value={name}
        onChange={e => setName(e.target.value)}
      />
      <button onClick={() => {
        artists.push({
          id: nextId++,
          name: name,
        });
      }}>Add</button>
      <ul>
        {artists.map(artist => (
          <li key={artist.id}>{artist.name}</li>
        ))}
      </ul>
    </>
  );
}
```
chúng ta sẽ sử dụng `...artists` để tạo 1 mảng mới để sao chép các array item hiện tại và cập nhật lại giá trị mới. ta sửa code sẽ như sau

```js
import { useState } from 'react';

let nextId = 0;

export default function List() {
  const [name, setName] = useState('');
  const [artists, setArtists] = useState([]);

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <input
        value={name}
        onChange={e => setName(e.target.value)}
      />
      <button onClick={() => {
        // thêm giá trị mới vào mảng 
        setArtists([
          ...artists, //chứa tất cả các item cũ 
          { id: nextId++, name: name } // push 1 item mới vào cuối mảng
        ]);
      }}>Add</button>
      <ul>
        {artists.map(artist => (
          <li key={artist.id}>{artist.name}</li>
        ))}
      </ul>
    </>
  );
}

```

còn khi cần thêm item vào đầu mảng `unshift()` thì code `setArtists` như sau:

```js
// thêm giá trị mới vào mảng 
setArtists([
  { id: nextId++, name: name } // push 1 item mới vào đầu mảng
  ...artists, //chứa tất cả các item cũ ở cuối mảng
]);
```

TH2: Xóa phần tử của mảng 
vd: ở đây là dùng `filter`

```js
import { useState } from 'react';

// mảng giá trị mặc định
let initialArtists = [
  { id: 0, name: 'Marta Colvin Andrade' },
  { id: 1, name: 'Lamidi Olonade Fakeye'},
  { id: 2, name: 'Louise Nevelson'},
];

export default function List() {
  const [artists, setArtists] = useState(
    initialArtists
  );

  return (
    <>
      <h1>Inspiring sculptors:</h1>
      <ul>
        {artists.map(artist => (
          <li key={artist.id}>
            {artist.name}{' '}
            <button onClick={() => {
              // khi click delete thì sẽ xóa đi có item tương ứng ( bản chất là chỉ lọc và giữ lại những id khác với id đã xóa)
              setArtists(
                artists.filter(a =>
                  a.id !== artist.id
                )
              );
            }}>
              Delete
            </button>
          </li>
        ))}
      </ul>
    </>
  );
}

```

TH3. biến đổi phần tử mảng

```js
import { useState } from 'react';

let initialShapes = [
  { id: 0, type: 'circle', x: 50, y: 100 },
  { id: 1, type: 'square', x: 150, y: 100 },
  { id: 2, type: 'circle', x: 250, y: 100 },
];

export default function ShapeEditor() {
  const [shapes, setShapes] = useState(
    initialShapes
  );

  function handleClick() {
    // sử dụng map để thay đổi giá trị phần tử
    const nextShapes = shapes.map(shape => {
      if (shape.type === 'square') {
        // giữ nguyên, k thay đổi 
        return shape;
      } else {
        // khi click vị trị type sẽ thay đổi xuống dưới 50px
        return {
          ...shape,
          y: shape.y + 50,
        };
      }
    });
    // Re-render with the new array
    setShapes(nextShapes);
  }

  return (
    <>
      <button onClick={handleClick}>
        Move circles down!
      </button>
      {shapes.map(shape => (
        <div
          key={shape.id}
          style={{
          background: 'purple',
          position: 'absolute',
          left: shape.x,
          top: shape.y,
          borderRadius:
            shape.type === 'circle'
              ? '50%' : '',
          width: 20,
          height: 20,
        }} />
      ))}
    </>
  );
}

```


