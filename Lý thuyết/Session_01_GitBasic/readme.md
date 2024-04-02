# Cách sử dụng Git cơ bản

## I. Giới thiệu
- Git là một hệ thống quản lý phiên bản phân tán (distributed version control system) được sử dụng rộng rãi trong quản lý mã nguồn của dự án phần mềm.
- Git cho phép nhiều người làm việc trên cùng một dự án mà không gây xung đột và dễ dàng hợp nhất (merge) các thay đổi.
- Git lưu trữ dữ liệu dự án trong các repository, mỗi người có thể sao chép (clone) một repository và làm việc độc lập.

## II. Cài đặt Git

Để biết máy mình cài Git chưa

Nhập lệnh sau vào cửa sổ terminal hoặc command line

```bash
git --version
```

Nếu thấy có nội dùng: git version 2.44.0.windows.1 thì thành công

Nếu chưa thì cài đặt như sau:

- Truy cập trang chủ Git (https://git-scm.com/download) và tải xuống phiên bản phù hợp với hệ điều hành của bạn.
- Cài đặt Git theo hướng dẫn trên trang web.


Đối với MacOs: nếu chưa cài thì có thể cài bằng xCode hoặc brew

```bash
brew install git
```

Nếu chưa cào brew thì cài bằng cách dán đoạn sau vào terminal

```bash
/bin/bash -c "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/HEAD/install.sh)"

## II. Cấu hình Git 

Sau khi cài xong git bạn cấu hình.

Trước tiên Truy cập trang GitHub (https://github.com) tạo một tài khoản sau đó mở Terminal hoặc Command Prompt

```bash
git config --global user.name "User Name"
git config --global user.email "username@gmail.com"
```

- Thay Username thành tên các bạn
- Thay username@gmail.com thành email mà bạn đã đăng ký tài khoản github

## III. Tạo repository trên GitHub và liên kết máy tính của bạn 

1. Tạo repository trên GitHub (hoặc dịch vụ tương tự):
   - Truy cập trang GitHub (https://github.com) và đăng nhập vào tài khoản của bạn.
   - Click vào nút "New repository".
   - Đặt tên cho repository, chọn các cài đặt khác (public/private) và nhấp vào nút "Create repository".
   * chú ý: khi tạo xong repository thì giữ nguyên màn hình hoặc copy phần hướng dẫn git ban đầu=> mục đích dùng để thao tác liên kết git với repository trên máy tính của bạn
2. Tạo repository trên máy tính của bạn:
	- tạo 1 folder đặt tên trùng với tên của repository mới tạo.
	- Mở Terminal hoặc Command Prompt
	- Di chuyển đến thư mục dự án của bạn bằng lệnh `cd <đường_dẫn_đến_thư_mục>`.
   - Gõ lệnh `git init` để khởi tạo một repository Git mới, tạo kho lưu trữ cục bộ.
   - Tạo mới 1 file readme.md hoặc thêm mới nội dung bất kỳ
   - Gõ lệnh `git add <file>` hoặc `git add . `: thêm file mới thay đổi hoặc tất cả các file đã thay đổi để chuẩn bị commit
   - `git commit -m "<message>"` : tạo 1 commit với message mô tả thay đổi đã thực hiện
   - `git branch -M main`: Tạo 1 nhánh mới với tên là main 
   - `git remote add origin <url của repository>.git `: tạo kết nối giữa repository trên GitHub và folder máy tính của bạn 
   - `git push -u origin main`: đồng bộ thay đổi (push) 1 nhánh từ máy tính của bạn lên GitHub
   ( lúc nảy reload lại repository trên GitHub bạn sẽ thấy các nội dung thay đổi được update) 
   
3. Tương tự khi cần thay đổi nội dung ta lại thực hiện như dưới đây
   b1: `git add . `
   b2. `git commit -m "<message>"`
   b3. `git push -u origin main`
   
### IV. Các lệnh cơ bản của Git

1. `git log`: Hiển thị danh sách các commit đã thực hiện.
2. `git status`: Hiển thị trạng thái hiện tại của repository.
3. `git pull`: Lấy (pull) các commit mới nhất từ repository trên máy chủ từ xa về máy cục bộ.
4. `git pull origin <branch_name>`: Đồng bộ thay đổi (pull) một nhánh từ máy chủ từ xa xuống máy cục bộ.
5. `git clone <url của repository>`: Sao chép (clone) một repository từ máy chủ từ xa về máy cục bộ.
6. `git remote -v` : Kiểm tra phiên bản URL hiện tại
7. `git remote set-url origin <url của repository>`: Thay đổi URL kết nối giữa máy chủ cục bộ và máy chủ từ xa
8. `git branch`: Liệt kê các nhánh hiện có trong repository.
9. `git branch <branch_name>`: Tạo một nhánh mới với tên là `<branch_name>`.
10. `git checkout <branch_name>`: Chuyển đổi sang nhánh có tên là `<branch_name>`.
11. `git merge <branch_name>`: Hợp nhất (merge) các thay đổi từ nhánh `<branch_name>` vào nhánh hiện tại.

