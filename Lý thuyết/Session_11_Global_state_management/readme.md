# Quản lý global state

## State Global là gì

State Global trong React là một cách quản lý trạng thái được chia sẻ giữa nhiều component trong ứng dụng, thay vì chỉ giới hạn trong một component cụ thể. Thông thường, trạng thái (state) trong React chỉ tồn tại trong một component đơn lẻ và không dễ dàng chia sẻ giữa các component khác. Tuy nhiên, khi cần chia sẻ trạng thái giữa nhiều component (đặc biệt là trong các ứng dụng phức tạp), ta sử dụng state global.

Lý do sử dụng State Global:

- Khi nhiều component cần truy cập và thay đổi cùng một trạng thái.
- Để tránh việc truyền props qua quá nhiều component (được gọi là prop drilling).
- Để quản lý trạng thái ứng dụng phức tạp hơn, đặc biệt khi có nhiều nguồn dữ liệu hoặc cần đồng bộ giữa các phần khác nhau của ứng dụng.

## Phương án đối ứng state global

### Phương án 1- đơn giản:  Sử dụng thư viện [Zustand](zustand.md)

### phương án 2 - sử dụng hooks useContext + useReducer 

### phương án 3 - sử dụng Redux [redux](redux.md)

### phương án 3 - sử dụng Redux Toolkit [Redux Toolkit](redux-toolkit.md)

### phương án 3 - sử dụng Redux Saga
