# useEffect hooks

là 1 hook mạnh mẽ trong react, cho phép bạn thực hiện side effects trong function component.
Nó kết hợp các chức năng mà trước đây chỉ có thể thực hiện được trong các lifecycle method của class component như `componentDidMount, componentDidUpdate,componentWillUnmount`

## side effects

- side effects là 1 khái niệm chung trong lập trình, được hiểu là khi có 1 tác động xảy ra thì dẫn tới việc dữ liệu của chương trình bị thay đổi
- trong React các function component sử dụng các props/state để tính toán return dữ liệu đầu ra.Còn nếu component thực hiện việc tính toán không nhắm tới mục tiêu đầu ra thì việc tính toán này gọi là side effects

```js
import React, { useEffect, useState } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);

  useEffect(() => {
    // Đây là nơi thực hiện side effect. mỗi lần count thay đổi thì 'useEffect' sẽ chạy và cập nhật tiêu đề trang
    document.title = `Count: ${count}`;
  });

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
    </div>
  );
};
```

## Đặc điểm useEffect

trong React chúng ta ưu tiên luồng xử lý để làm sao render UI ra màn hình nhanh nhất

do vậy, tất cả những vấn đề side-effects chúng ta đưa vào useEffect để xử lý
- useEffect cho phép bạn thực hiện các hiệu ứng phụ trong các components của bạn
- effects sẽ chạy sau khi component đã rendering

## Khi nào thì dùng useEffect

- update DOM
- call API
- events: add, remove event listener
- Observer pattern: Subscribe and unsubscribe
- Closure
- Timer: setTimeout, setInterval, clearTimeout, clearInterval
- Mounted/unmounted

## Cách dùng

useEffect có 2 tham số, tham số thứ 2 là tùy chọn

>useEffect(setup, [dependencies])

chi tiết như dưới đây

```js
useEffect(()=>{

    // Thực hiện tác vụ phụ ở đây
    // ...

    // Hủy bỏ tác vụ phụ nếu cần thiết
    return () => {
      // ...
    };

}, [dependencies);
```

thì qua đó chúng ta có 3 TH xảy ra khi sử dụng useEffect

TH1. không có dependency

cách viết này nó đại diện cho giai đoạn `Mounted` trong lifeCycle component 
cú pháp
```js
// mounted - lifeCycle
useEffect(() => {
  //side effects sẽ chạy mỗi khi component Render
});
```

khi nào thì dùng
- khi các side-effects cần thực hiện sau khi component render xong
- và muốn nó thực hiện lại mỗi khi render

TH2. dependency là 1 mảng rỗng

lúc này side effect chỉ chạy 1 lần khi component mount
cú pháp
```js
// mounted - lifeCycle
useEffect(() => {
  //side effects sẽ chạy mỗi khi component Render
}, []); // dependency là 1 array rỗng
```

Khi nào thì dùng
- khi side effects cần thực hiện sau khi component render xong
- và muốn nó thực hiện duy nhất trong lần đầu tiên component render

TH3. dependency là 1 props hoặc state

cách viết này nó đại diện cho giai đoạn `Updating` trong lifeCycle

cú pháp
```js
// updating - lifeCycle
useEffect(() => {
  // side effect chỉ chạy trong lần đầu tiên component render
  // và nó chạy lại mỗi khi props, state thay đổi giá trị
}, [prop,state]); // dependency là 1 props or state
```

Khi nào thì dùng
- khi side effects cần thực hiện sau khi component render xong
- và muốn nó thực hiện ngay trong lần đầu tien component render
- và muốn nó thực hiện lại mỗi khi state, props thay đổi giá trị


>** lưu ý: luôn đúng cho cả 3 TH trên **
>
> - callback luôn được gọi sau khi component đã mounted
> - cleanup luôn được gọi trước khi component unmounted

## Effect cleanup(unmouting lifeCycle)

- sử dụng để hủy effects ( mục đích chống tràn bộ nhớ - memory leaks)

cú pháp
```js
useEffect(() => {
  // Thực hiện tác vụ phụ ở đây
  // ...

  // Hủy bỏ tác vụ phụ nếu cần thiết
  return () => {
    // ...
  };
}, [dependencies]);
```
1. cleaup useEffect with DOM event

bài toán: scroll show/hide go to top.
```js
const Greet = () => {
  const [show, setShow] = useState(false);

  useEffect(() => {
    const handleGoTop = ()=> {
        console.log(window.scrollY);

        if(window.scrollY >= 200){
            console.log('set state');
            setShow(true)
        }else{
            setShow(false);
        }

    }
    window.addEventListener('scroll', handleGoTop);

    // cleanup this component ( để tránh memory leak thì nên remove event )
    // return () => {
    //   window.removeEventListener('scroll', handleGoTop);
    // };
  }, []);

  console.log('re-render');

  return (
    <div>
      {show && <button 
        style={{ 
            position: 'fixed',
            right: 20,
            bottom: 20,
        }}
      >Go To</button>}
    </div>
  );
};

export default IntervalExample;
```
nếu để như vậy thì khi componet được unmouted ra khỏi app thì sự kiện  `scroll ` ở cấp độ window vẫn đang được lắng nghe. sự kiện này được lưu trữ bởi 1 giá trị tham chiếu trong bộ nhớ.

khi Mount, unmount liên tục -> mỗi lần như vậy bố nhớ lại cấp phát ra thêm 1 giá trị tham chiếu mới cho sự kiện. các giá trị tham chiếu đó bạn k dùng lại được nữa.

mỗi lần component update state thì xuất hiện lỗi memory leak ở console

==> phương pháp giải quyết. ta phải thêm xử lý cleanup effect

```js
return () => {
    window.removeEventListener('scroll', handleGoTop);
};
```

2. useEffect width timer function 

vd toggle button sinh ra vấn đề memory leak nếu k sử dụng cleanup

```js
function Greet(){
    const [count, setCount] = useState(0);

    useEffect(()=> {
        // side effects
        setInterval(() => {
            setCount((count) => count + 1);
            console.log('This will run every second!');
        }, 1000);
        // cleanup effects
        // return () => clearInterval(interval);

    },[]);
    return (
        <h1>{count}</h1>
    )
}
```

vd open and close dialog 

```js
useEffect(() => {
  const dialog = dialogRef.current;
  dialog.showModal();
  return () => dialog.close();
}, []);
```

vd triggering animations

```js
useEffect(() => {
  const node = ref.current;
  node.style.opacity = 1; // Trigger the animation
  return () => {
    node.style.opacity = 0; // Reset to the initial value
  };
}, []);
```

3. sử dụng `AbortController` để hủy bỏ fetch api

```js
import React, { useEffect, useState } from 'react';

const FetchComponent = () => {
  const [data, setData] = useState(null);

  useEffect(() => {
    const controller = new AbortController();
    const signal = controller.signal;

    fetch('https://jsonplaceholder.typicode.com/posts', { signal })
      .then((response) => response.json())
      .then((data) => setData(data))
      .catch((error) => {
        if (error.name === 'AbortError') {
          console.log('Fetch aborted');
        } else {
          console.error('Fetch error:', error);
        }
      });

    // Hàm cleanup
    return () => controller.abort();
  }, []);


  return (
    <div>
      {data ? <ul>{data.map(post => <li key={post.id}>{post.title}</li>)}</ul> : 'Loading...'}
    </div>
  );
};

export default FetchComponent;
```

4. sử dụng thư viện `Redux,Zustand,React Query ` để quản lý bộ nhớ và dọn dẹp side effects 1 cách hiệu quả

## Multiple useEffect hooks

bạn có thể sử dụng nhiều useEffect hooks trong 1 component để tách biệt các logic khác nhau

```js
import React, { useEffect, useState } from 'react';

const MyComponent = () => {
  const [count, setCount] = useState(0);
  const [text, setText] = useState('');

  useEffect(() => {
    console.log('Count has changed:', count);
  }, [count]);

  useEffect(() => {
    console.log('Text has changed:', text);
  }, [text]);

  return (
    <div>
      <p>Count: {count}</p>
      <button onClick={() => setCount(count + 1)}>Increment</button>
      <input value={text} onChange={(e) => setText(e.target.value)} />
    </div>
  );
};

export default MyComponent;
```

## không cần dùng useEffect khi

tài liệu : <https://react.dev/learn/you-might-not-need-an-effect>
- 1 số logic chỉ chạy 1 lần khi ứng dụng khởi chạy và nó k liên quan đến state, props của component thì bạn nên đặt chúng ra ngoài component

```js
if (typeof window !== 'undefined') { // Check if we're running in the browser.
  checkAuthToken();
  loadDataFromLocalStorage();
}

function App() {
  // ...
}
```