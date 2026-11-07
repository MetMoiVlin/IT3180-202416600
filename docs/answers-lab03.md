# Lab 03 – Trả lời câu hỏi thảo luận

Sinh viên: Nguyễn Hoàng Thái Sơn - MSSV: 202416600

## 1. Merge base là gì?

- **Merge base** là tổ tiên chung gần nhất của hai nhánh, tức commit cuối cùng mà cả
  hai cùng có trước khi tách ra. Xem bằng `git merge-base A B`.
- Trong bài, `feature/vietnamese-messages` (Dev 1) và `feature/show-added-title`
  (Dev 2) cùng tạo từ `master` tại `d144597`. Đó là merge base của chúng:
  ```
  $ git merge-base origin/feature/vietnamese-messages origin/feature/show-added-title
  d144597...
  ```
- Git merge theo kiểu **three-way merge**, so sánh 3 phiên bản: merge base, phía
  "ours" và phía "theirs". Với từng vùng mã:
  - chỉ một bên đổi so với base → lấy thay đổi của bên đó (tự động);
  - cả hai bên đổi **cùng một vùng** theo cách khác nhau → **xung đột**.
- Ở đây, tại base dòng in là `print("The disc has been added")`. Dev 1 đổi thành
  tiếng Việt, Dev 2 đổi thành f-string có tiêu đề. Cả hai cùng sửa một dòng nên Git
  không thể tự quyết, và ta phải kết hợp ý đồ của cả hai:
  `print(f"Đã thêm đĩa '{dvd.title}' vào giỏ hàng")`.
- Merge base cũng giải thích vì sao PR bugfix phải chọn đúng base. Nếu
  `bugfix/discount-calculation` mở PR vào `master` thay vì `feature/discount`, PR sẽ
  hiện cả các commit của nhánh tính năng ("nhiều commit lạ").

## 2. Vì sao `master` / `release/*` phải dùng `revert` mà không dùng `reset`?

- `reset` **dời con trỏ nhánh** về commit cũ, tức là viết lại lịch sử: các commit
  sau đó biến mất khỏi nhánh. `revert` **tạo commit mới** đảo ngược thay đổi, còn
  lịch sử cũ giữ nguyên.
- `master` và `release/*` là lịch sử **đã chia sẻ**. Người khác đã pull, nhánh khác
  đã tách ra từ đó, và tag/baseline trỏ vào đó. Nếu reset rồi force push:
  - bản clone của người khác lệch với remote, và lần pull sau sẽ đem commit "đã xóa"
    quay lại;
  - mất dấu vết kiểm toán: không còn biết thay đổi đã từng được tích hợp rồi gỡ ra
    vì sao;
  - với `release/*`, baseline đã giao cho người dùng bị thay đổi âm thầm, vi phạm
    nguyên tắc SCM.
- Hơn nữa, quy tắc bảo vệ nhánh cấm force push lên `master`/`release/*`. Ta chỉ có
  thể thay đổi qua PR, nên `revert` (qua nhánh `bugfix/revert-...` + PR) là cách
  duy nhất hợp lệ, và bản thân việc hoàn tác cũng được review như mọi thay đổi khác.
- Khi revert một **merge commit** phải chỉ ra "mainline" bằng `-m 1`, nghĩa là giữ
  phía `master` (parent 1) và gỡ phần do nhánh tính năng mang vào. Thiếu `-m`, Git
  báo `commit ... is a merge but no -m option was given`.
- `reset`, `--amend`, `rebase` chỉ dùng cho commit **chưa push**, như trong bài
  luyện tập trên nhánh nháp `topic/undo-practice` (không push). Commit "mất" sau
  `reset --hard` vẫn tìm lại được bằng `git reflog`.
