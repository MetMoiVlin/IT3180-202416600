# Lab 05 – Trả lời câu hỏi thảo luận

Sinh viên: Nguyễn Hoàng Thái Sơn - MSSV: 202416600

## 1. Vì sao không merge cả nhánh hotfix vào `master`?

- Nhánh `hotfix/search-case-insensitive` tạo từ `release/lab04` (dòng 1.0.x) và có
  2 commit:
  1. **commit sửa lỗi** (`search.py` + test): cần có ở mọi dòng phiên bản bị ảnh
     hưởng;
  2. **commit nâng phiên bản** (`__version__ = "1.0.1"` + mục `[1.0.1]` trong
     CHANGELOG): chỉ đúng cho dòng 1.0.x.
- Nếu merge cả nhánh vào `master` (đang là 1.1.0), commit 2 sẽ đặt `1.0.1` đè lên
  `1.1.0`. GitHub báo *Can't automatically merge* vì xung đột ở `__init__.py`. Kể cả
  sửa được thì `master` cũng bị kéo theo lịch sử riêng của dòng 1.0.x.
- Vì vậy chỉ **lan truyền đúng commit sửa lỗi** bằng `git cherry-pick -x <FIX_SHA>`.
  Tùy chọn `-x` ghi `(cherry picked from commit <FIX_SHA>)` vào message, giữ được
  truy vết nguồn gốc. Đây cũng là lý do commit sửa lỗi phải **tách riêng** khỏi
  commit nâng số phiên bản.

## 2. Vì sao ở Phần A chưa gắn tag `v1.1.0`?

- Tag là định danh **bất biến** của một phiên bản đã phát hành: đã chia sẻ thì không
  xóa, không di chuyển.
- Ở Phần A, `release/lab05` vừa được tạo và chưa giao cho ai. Lỗi phát hiện ở Phần B
  ảnh hưởng cả dòng 1.1, nên bản sửa được cherry-pick vào `release/lab05` **trước**,
  rồi mới gắn `v1.1.0`. Nhờ vậy 1.1.0 phát hành ra đã có bản sửa.
- Nếu đã gắn `v1.1.0` (và đã push) thì không được di chuyển tag. Bản sửa phải phát
  hành thành **`v1.1.1`** (PATCH), giống như `v1.0.0` → `v1.0.1`.

## 3. Ưu, nhược điểm của *upstream first* so với *release first*

| | Upstream first (Release Flow của Microsoft) | Release first (học phần) |
|---|---|---|
| Cách làm | Sửa trên `master` trước (qua PR), rồi cherry-pick xuống `release/*` | Sửa trên `release/*` trước (hotfix từ bản đã phát hành), rồi cherry-pick lên `master` và các dòng khác |
| Ưu điểm | Không bao giờ quên sửa ở `master`, nên lỗi không "tái xuất" ở bản sau; bản sửa được review và test trên dòng phát triển chính; một quy tắc duy nhất | Sửa và test **đúng trên mã người dùng đang chạy**; phát hành bản vá nhanh khi khẩn cấp; hợp khi mã ở release đã khác xa `master` |
| Nhược điểm | Bản vá cho người dùng phải chờ PR vào `master`; nếu `master` đã thay đổi nhiều thì cherry-pick xuống release dễ xung đột | Dễ **quên** lan truyền ngược lên `master` hoặc sang dòng khác (regression ở bản sau); cần kiểm toán đa phiên bản (Phần F) |

## 4. Nếu quên lan truyền bản sửa sang `release/lab05`, lệnh nào ở Phần F phát hiện?

- `git log --all --oneline --grep "cherry picked from commit <FIX_SHA>"` chỉ ra
  **1** bản cherry-pick (trên `master`) thay vì **2**.
- `git cherry -v origin/release/lab05 origin/release/lab04` sẽ hiện commit sửa lỗi
  với dấu **`+`**, nghĩa là `release/lab05` chưa có bản tương đương. Với `master`
  (đã cherry-pick) thì dòng này là `-`.
- Vòng lặp kiểm tra hành vi in **`0`** cho `v1.1.0` (tìm `"lion"` không ra
  *The Lion King*), trong khi `v1.0.1` và `master` in `1`.
- `git tag --contains <FIX_SHA>` chỉ cho biết các tag chứa **chính** commit đó
  (`v1.0.1`). Cherry-pick tạo commit mới nên lệnh này không đủ để kiểm toán; phải
  dùng `git cherry` hoặc `--grep` như trên.
