# Managing State

Tài liệu: <https://react.dev/learn/managing-state>

Các cách nâng cao để tối ưu và quản lý state đúng và hiệu suất

## state trong component

ta có 1 ví dụ nhập số điện thoại để tư vấn gồm 1 input số điện thoại và 1 button đăng ký. lúc này sẽ xử lý như sau: 
- lúc đầu input là trống
- khi chưa nhập sdt thì button đăng ký k click được và khi nhập thì button mới cho ckick
- khi click xong thì ta phải disable ngay. để tránh người dùng click nhiều lần
- sau khi click thành công có 2 trạng thái có thể xảy ra
    - thành công
    - thất bại, có lỗi

code như sau
```js
import { useState } from 'react';

export default function CallForm() {
    // khai báo các state 
  const [mobile, setMobile] = useState('');
  const [error, setError] = useState(null);
  const [status, setStatus] = useState('typing');

  if (status === 'success') {
    return <h1>That's right!</h1>
  }
// dùng async await catch để xử lý các trường hợp khi click 
  async function handleSubmit(e) {
    e.preventDefault();
    setStatus('submitting');
    try {
      await submitForm(mobile);
      setStatus('success');
    } catch (err) {
      setStatus('typing');
      setError(err);
    }
  }

  function handleInputChange(e) {
    setMobile(e.target.value);
  }

  return (
    <>
      <h2>City quiz</h2>
      <p>
        In which city is there a billboard that turns air into drinkable water?
      </p>
      <form onSubmit={handleSubmit}>
        <input
          value={mobile}
          onChange={handleInputChange}
          disabled={status === 'submitting'}
        />
        <br />
        <button disabled={
          mobile.length === 0 ||
          status === 'submitting'
        }>
          Đăng ký
        </button>
        {error !== null &&
          <p className="Error">
            {error.message}
          </p>
        }
      </form>
    </>
  );
}

function submitForm(mobile) {
  // Pretend it's hitting the network.
  return new Promise((resolve, reject) => {
    setTimeout(() => {
      let shouldError = mobile.length > 0 && mobile.length !== 10;
      if (shouldError) {
        reject(new Error('Số điện thoại không hợp lệ'));
      } else {
        resolve();
      }
    }, 1500);
  });
}
```

## Choosing the State Structure (Cấu trúc hợp lý cho state)

> lựa chọn cấu trúc hợp lý cho state

Các nguyên tắc để tạo ra 1 cấu trúc state chuẩn

1. group related state ( nhòm các state liên quan)

tài liệu: <https://react.dev/learn/choosing-the-state-structure#group-related-state>

nếu bạn luôn update 2 hoặc nhiều state cùng 1 lúc thì nên gộp chúng thành 1 state duy nhất bằng cách dùng `object`

vd 
```js
const [x, setX] = useState(0);
const [y, setY] = useState(0);
```
thay thế bằng
```js
const [position, setPosition] = useState({
    x: 0,
    y: 0
});
```

code sẽ như sau: 
```js
import { useState } from 'react';

export default function MovingDot() {
  const [position, setPosition] = useState({
    x: 0,
    y: 0
  });
  return (
    <div
      onPointerMove={e => {
        setPosition({
          x: e.clientX,
          y: e.clientY
        });
      }}
      style={{
        position: 'relative',
        width: '100vw',
        height: '100vh',
      }}>
      <div style={{
        position: 'absolute',
        backgroundColor: 'red',
        borderRadius: '50%',
        transform: `translate(${position.x}px, ${position.y}px)`,
        left: -10,
        top: -10,
        width: 20,
        height: 20,
      }} />
    </div>
  )
}
```

2. Avoid contradictions in state ( tránh sự hiểu nhầm giữa các state)

tài liệu: <https://react.dev/learn/choosing-the-state-structure#avoid-contradictions-in-state>

ví dụ bạn có 1 khung chat, lúc này gây ra sự hiểu nhầm, không biết lúc nào thì đang gửi, lúc nào thì đã gửi 
```js
const [text, setText] = useState('');
  const [isSending, setIsSending] = useState(false);
  const [isSent, setIsSent] = useState(false);

  async function handleSubmit(e) {
    e.preventDefault();
    setIsSending(true);
    await sendMessage(text);
    setIsSending(false);
    setIsSent(true);
  }
```
giải pháp là hãy tạo ra 1 state status trạng thái duy nhất
```js
const [text, setText] = useState('');
const [status, setStatus] = useState('typing');// thêm 1 state status

  async function handleSubmit(e) {
    e.preventDefault();
    setStatus('sending');
    await sendMessage(text);
    setStatus('sent');
  }
```
or code như sau : 
```js
import { useState } from 'react';

export default function FeedbackForm() {
  const [text, setText] = useState('');
  const [status, setStatus] = useState('typing');

  async function handleSubmit(e) {
    e.preventDefault();
    setStatus('sending');
    await sendMessage(text);
    setStatus('sent');
  }

  const isSending = status === 'sending';
  const isSent = status === 'sent';

  if (isSent) {
    return <h1>Thanks for feedback!</h1>
  }

  return (
    <form onSubmit={handleSubmit}>
      <p>How was your stay at The Prancing Pony?</p>
      <textarea
        disabled={isSending}
        value={text}
        onChange={e => setText(e.target.value)}
      />
      <br />
      <button
        disabled={isSending}
        type="submit"
      >
        Send
      </button>
      {isSending && <p>Sending...</p>}
    </form>
  );
}

// Pretend to send a message.
function sendMessage(text) {
  return new Promise(resolve => {
    setTimeout(resolve, 2000);
  });
}
```

3. Avoid redundant state ( tránh tình trạng dư thừa state)

tài liệu: <https://react.dev/learn/choosing-the-state-structure#avoid-redundant-state>

ví dụ có 1 form cần điền vào 2 trường first name , lastname , sau đó xuất ra fullname

có thể bạn nghĩ sẽ cần đến 3 state để làm việc này
```js
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');
const [fullName, setFullName] = useState('');
```

nhưng không, ta chỉ cần dùng 2 state là ok. chỉ cần lấy fullname đưa ra hiển thị

```js
const [firstName, setFirstName] = useState('');
const [lastName, setLastName] = useState('');

const fullName = firstName + ' ' + lastName;
```
4. Avoid duplication in state (Tránh trùng lặp state )

tài liệu: <https://react.dev/learn/choosing-the-state-structure#avoid-duplication-in-state>

ví dụ có 1 danh sách menu được tạo ra từ object
```js
import { useState } from 'react';

const initialItems = [
  { title: 'pretzels', id: 0 },
  { title: 'crispy seaweed', id: 1 },
  { title: 'granola bar', id: 2 },
];

export default function Menu() {
    // items nhận initialItems làm giá trị khởi tạo
  const [items, setItems] = useState(initialItems);
  //selectedItem mặc định là giá trị đầu tiên trong mảng items
  const [selectedItem, setSelectedItem] = useState(
    items[0]
  );

  return (
    <>
      <h2>What's your travel snack?</h2>
      <ul>
        {items.map(item => (
          <li key={item.id}>
            {item.title}
            {' '}
            <button onClick={() => {
              setSelectedItem(item);
            }}>Choose</button>
          </li>
        ))}
      </ul>
      <p>You picked {selectedItem.title}.</p>
    </>
  );
}
```

khi bạn dùng cách này thì đang gặp vấn đề trùng lặp state ( phần tử thứ 0 đang trùng lặp lại chính nó ở 2 nơi)
- selectedItem là 1 object, là phần tử thứ 0 của items
- rồi trong items cũng có phần tử thứ 0 đó nữa 

giải pháp khắc phục 
```js
import { useState } from 'react';

const initialItems = [
  { title: 'pretzels', id: 0 },
  { title: 'crispy seaweed', id: 1 },
  { title: 'granola bar', id: 2 },
];

export default function Menu() {
  const [items, setItems] = useState(initialItems);
  // state lưu trữ id của item được chọn
  const [selectedId, setSelectedId] = useState(0);

    // dùng find() bạn có thể xác định phần tử nào được chọn dựa vào selectedId
  const selectedItem = items.find(item =>
    item.id === selectedId
  );

  function handleItemChange(id, e) {
    setItems(items.map(item => {
      if (item.id === id) {
        return {
          ...item,
          title: e.target.value,
        };
      } else {
        return item;
      }
    }));
  }

  return (
    <>
      <h2>What's your travel snack?</h2>
      <ul>
        {items.map((item, index) => (
          <li key={item.id}>
            <input
              value={item.title}
              onChange={e => {
                handleItemChange(item.id, e)
              }}
            />
            {' '}
            <button onClick={() => {
              setSelectedId(item.id);
            }}>Choose</button>
          </li>
        ))}
      </ul>
      <p>You picked {selectedItem.title}.</p>
    </>
  );
}
```

5. Avoid deeply nested state ( tránh dùng state có độ sâu- 1 state có 1 object có nhiều object con)

khi state lồng ghép quá sâu, dẫn đến sự phức tạp và khó quản lý
```js
// Tránh state lồng nhau sâu như này
const [state, setState] = useState({
  user: {
    profile: {
      name: '',
      age: 0
    },
    settings: {
      theme: 'dark'
    }
  }
});

// Thay vào đó, giữ state đơn giản nếu có thể
const [profile, setProfile] = useState({ name: '', age: 0 });
const [settings, setSettings] = useState({ theme: 'dark' });
```

6. Store Only What You Need ( chỉ lưu trữ những gì cần thiết)
không lưu trữ các giá trị có thể tính toàn từ những giá trị khác trong state

```js
// Tránh lưu trữ cả giá trị cộng dồn và các giá trị thành phần
const [items, setItems] = useState([]);
const [total, setTotal] = useState(0); // Không cần thiết

// Thay vào đó chỉ lưu trữ items và tính toán total khi cần
const total = items.reduce((acc, item) => acc + item.price, 0);
```

7. Manage State Immutably( quản lý state bất biến)
không thay đổi trực tiếp state: luôn luôn tạo ra 1 snapshot của state để khi thay đổi vẫn giữ được tính bất biến(không thay đổi dữ liệu ban đầu)

```js
const addItem = (newItem) => {
  setItems(prevItems => [...prevItems, newItem]); // sử dụng descturing để tạo array mới, dễ dàng cập nhật
};

const updateItem = (index, newItem) => {
  setItems(prevItems => prevItems.map((item, i) => i === index ? newItem : item));
};

const removeItem = (index) => {
  setItems(prevItems => prevItems.filter((_, i) => i !== index));
};
```

8. Use Custom Hooks for Reusable Logic ( sử dụng custom hooks để tái sử dụng logic)

sử dụng custom hooks để tách biệt và tái sử dụng logic quản lý state giữa vavs component khác nhau

```js
// custom hooks state 
const useCounter = (initialValue = 0) => {
  const [count, setCount] = useState(initialValue);
  const increment = () => setCount(count + 1);
  const decrement = () => setCount(count - 1);
  return { count, increment, decrement };
};

const CounterComponent = () => {
  const { count, increment, decrement } = useCounter();

  return (
    <div>
      <button onClick={decrement}>-</button>
      <span>{count}</span>
      <button onClick={increment}>+</button>
    </div>
  );
};
```

9. sử dụng thư viên quản lý state khi cần thiết

với các ứng dụng lớn, phức tạp thì việc sử dụng các thư viện quản lý state giúp quản lý state toàn cục và tối ưu hóa hiệu suất

các thư viện như `Redux, MobX, Zustand, React Query`

## Preserving and Resetting State (bảo toàn và khôi phục state)

tài liệu: <https://react.dev/learn/preserving-and-resetting-state>