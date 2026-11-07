# Lab 01 – Trả lời câu hỏi thảo luận

Sinh viên: Nguyễn Hoàng Thái Sơn - MSSV: 202416600

## Câu hỏi nhanh: cấu hình `--global` được lưu ở đâu? Khác gì so với `--local`?

- `--global` được ghi vào tệp cấu hình của người dùng: `~/.gitconfig` (hoặc
  `~/.config/git/config`). Trên Windows là `C:\Users\<tên>\.gitconfig`. Nó áp dụng
  cho **mọi** kho của người dùng đó trên máy.
- `--local` (mặc định khi đứng trong một kho) được ghi vào `.git/config` của kho
  hiện tại và chỉ áp dụng cho kho đó.
- Thứ tự ưu tiên: `--local` > `--global` > `--system`. Giá trị ở cấp hẹp hơn ghi đè
  cấp rộng hơn. Lệnh `git config --list --show-origin` cho biết mỗi giá trị đến từ
  tệp nào.

## 1. Vì sao dùng `git mv` thay vì đổi tên bằng hệ điều hành?

- Git không lưu thao tác "đổi tên". Nó chỉ lưu nội dung từng snapshot, rồi khi xem
  lịch sử thì **suy ra** việc đổi tên bằng cách so độ giống nhau giữa tệp bị xóa và
  tệp được thêm.
- `git mv a b` tương đương `mv a b` + `git rm a` + `git add b` trong một bước. Cả
  phần xóa lẫn phần thêm đều vào staging cùng lúc, nên không bị quên stage tệp cũ.
  Nhờ vậy commit chứa trọn vẹn thao tác đổi tên.
- Vì Git dựa vào độ giống nhau, commit đổi tên nên **chỉ đổi tên, không sửa nội
  dung**. Đó là lý do bài tách thành hai commit: "move sources into aims package"
  rồi mới "update imports in main.py". Kết quả là `git log --follow -- aims/cart.py`
  vẫn thấy lịch sử từ trước khi đổi tên, và tab *Files changed* của PR hiển thị
  **renamed** thay vì xóa + thêm.

## 2. `release/lab01` có phải là baseline không?

- Theo định nghĩa SCM, baseline là phiên bản đã được thống nhất và chỉ được thay đổi
  qua quy trình chính thức. `release/lab01` được tạo từ `master` sau khi PR #1 đã
  merge, và đó là phiên bản được nộp. Vì vậy nó đóng vai trò **baseline nộp bài**
  của Lab 01.
- Tuy nhiên đây mới là baseline "theo quy ước". Nhánh là con trỏ có thể di chuyển,
  và ở Lab 01 `release/*` chưa được bảo vệ. Định danh **bất biến** cho baseline là
  một annotated tag (ở Lab 04 sẽ gắn `v0.1.0` vào `origin/release/lab01`). Nhánh
  release là dòng bảo trì, còn tag mới là điểm cố định của phiên bản.

## 3. Vì sao không commit thẳng lên `master` dù làm một mình?

- `master` là cấu hình tích hợp, luôn phải chạy được. Đi qua nhánh + PR nghĩa là mọi
  thay đổi đều có **yêu cầu thay đổi** (PR mô tả mục đích, thay đổi, cách kiểm thử)
  trước khi được tích hợp. Đây chính là hoạt động *kiểm soát thay đổi*.
- PR để lại dấu vết truy vết: ai đổi, vì sao, đã kiểm thử ra sao, merge lúc nào.
  Commit thẳng thì mất toàn bộ ngữ cảnh này.
- Nếu thay đổi hỏng, chỉ nhánh bị ảnh hưởng. Có thể bỏ nhánh, hoặc revert cả PR bằng
  một lệnh, mà không làm bẩn `master`.
- Thói quen này cần sẵn khi làm nhóm. Từ Lab 02, `master` được bảo vệ và CI ở Lab 05
  chạy test trên PR, nên luồng làm việc phải giống nhau dù có một người hay nhiều
  người.
