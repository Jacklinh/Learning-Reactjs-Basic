# useMemo

tài liệu <https://react.dev/reference/react/useMemo>

- là 1 hook trong react được sử dụng để ghi nhớ giá trị của 1 biểu thức hoặc 1 hàm tính toán phức tạp 
- mục đích tránh việc tính toán lại k cần thiết khi có sự thay dổi trong dependency.
- có thể cải thiện hiệu suất của ứng dụng bằng cách giảm thiểu số lần tính toán lại những giá trị phức tạp

## cú pháp

```js
const cachedValue = useMemo(calculateValue, dependencies)
// chi tiêt
const cachedValue = useMemo(() => {
    // useMemo
}, [dependencies])
```

## Khi nào thì dùng useMemo

- khi mà component này độc lập với componet chứa nó (Cha).
- khi cha cần re-render nhưng con không cần thiết phải re-render thì dùng.
- Tùy từng ngữ cảnh cụ thể, có thể component này k dùng state
- tính toán 1 giá trị phức tạp hoặc tốn thời gian
- giá trị đó k cần tình toán lại trừ khi 1 trong các dependencies thay đổi

vd tính toán phức tạp: bạn có 1 hàm tính toán phức tạp mà k muốn thực hiện lại mỗi lần component render, trừ khi các giá trị đầu vào thay đổi

```js
import React, { useState, useMemo } from 'react';

const ExpensiveComponent = () => {
  const [count, setCount] = useState(0);
  const [otherState, setOtherState] = useState(false);
// thực hiện khi giá trị của count thay đổi. nếu "otherState" thay đổi thì "expensiveCalculation" k được thực hiện lại nhờ "useMemo"
  const expensiveCalculation = (num) => {
    console.log('Calculating...');
    // Giả sử đây là một tính toán phức tạp
    for (let i = 0; i < 1000000000; i++) {}
    return num * 2;
  };

  const memoizedValue = useMemo(() => expensiveCalculation(count), [count]);

  return (
    <div>
      <p>Count: {count}</p>
      <p>Memoized Value: {memoizedValue}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <button onClick={() => setOtherState(!otherState)}>Toggle Other State</button>
    </div>
  );
};

export default ExpensiveComponent;
```

vd Tối ưu hóa render của 1 danh sách: nếu bạn có 1 danh sách, bạn muốn tránh tính toán lại danh sách đã lọc hoặc sắp xếp mỗi khi component render, ta sử dụng `useMemo`

```js
import React, { useState, useMemo } from 'react';

const ListComponent = () => {
  const [filter, setFilter] = useState('');
  const [items] = useState([
    'apple',
    'banana',
    'cherry',
    'date',
    'elderberry',
    'fig',
    'grape',
  ]);
// danh sách filteredItems chỉ được tính toán lại khi "filter" hoặc "items" thay đổi. tránh tính toán lại danh sách khi các thay đổi k liên quan xảy ra trong component
  const filteredItems = useMemo(() => {
    console.log('Filtering items...');
    return items.filter(item => item.includes(filter));
  }, [filter, items]);

  return (
    <div>
      <input
        type="text"
        value={filter}
        onChange={(e) => setFilter(e.target.value)}
        placeholder="Filter items"
      />
      <ul>
        {filteredItems.map((item, index) => (
          <li key={index}>{item}</li>
        ))}
      </ul>
    </div>
  );
};

export default ListComponent;
```

>** lưu ý khi sử dung useMemo **
>
>- không lạm dụng `useMemo` cho mọi giá trị tính toán. sử dụng chỉ khi gặp vấn đề về hiệu suất và xác định được rằng tính toán lại giá trị là nguyên nhân
>- kiểm tra các dependency: đảm bảo rằng dependency phụ thuộc để tránh các kết quả không mong muốn
>- useMemo chỉ là 1 tối ưu hóa hiệu suất. trong 1 số TH, react có thể bỏ qua `useMemo` và tính toán lại giá trị.