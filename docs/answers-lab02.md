# Lab 02 – Trả lời câu hỏi thảo luận

Sinh viên: Nguyễn Hoàng Thái Sơn - MSSV: 202416600

## 1. Vì sao phải đồng bộ `master` vào nhánh trước khi gửi PR?

- Nhánh `feature/search-by-title` (Dev 2) được tạo từ `master` **cũ**, khi
  `print_cart` chưa có. Trong lúc đó PR `feature/cart-print` đã merge, nên
  `origin/master` đi trước nhánh của Dev 2.
- Nếu không đồng bộ, reviewer chỉ thấy mã của nhánh trên nền cũ. Tổ hợp *mã mới +
  master hiện tại* chưa từng được chạy thử, và lỗi tích hợp chỉ lộ ra sau khi đã
  merge vào `master`.
- Khi `git merge origin/master` (hoặc rebase) **trên nhánh của mình**:
  - xung đột (nếu có) được giải quyết ở nhánh riêng, không làm bẩn `master`;
  - chạy lại `python main.py` và test ngay trên kết quả tích hợp, nên thứ được
    review chính là thứ sẽ được tích hợp;
  - PR hiển thị đúng phần thay đổi so với `master` mới, dễ review hơn.
- Đây cũng là quy tắc "nhánh sống ngắn, càng gần master càng tốt" của Release Flow,
  giúp tránh "merge hell".

## 2. Khi nào **không** được rebase?

Rebase viết lại lịch sử: mỗi commit được tạo lại với SHA mới. Vì vậy:

- **Không rebase commit đã chia sẻ** mà người khác có thể đã dựa vào, ví dụ đã push
  lên nhánh chung, hoặc nhánh có người khác cùng làm. Họ sẽ có lịch sử lệch, phải
  force-pull, và dễ mất commit.
- **Tuyệt đối không rebase `master` và `release/*`.** Đây là baseline và dòng tích
  hợp chung, nên chúng được bảo vệ, cấm force push.
- Không rebase khi cần giữ nguyên dấu vết merge, ví dụ khi muốn revert cả tính năng
  bằng một lệnh `git revert -m 1`.
- Chỉ rebase nhánh **riêng của mình, chưa ai dùng**. Nếu đã push nhánh riêng thì
  sau khi rebase phải `git push --force-with-lease`, không dùng `--force`.

## 3. `feature/*` khác `topic/*` ra sao?

| | `feature/*` | `topic/*` |
|---|---|---|
| Mục đích | Thêm **tính năng** cho sản phẩm (thay đổi hành vi mã nguồn) | Chủ đề **không phải tính năng**: tài liệu, cấu hình, quy trình, công cụ |
| Ví dụ trong repo | `feature/cart-print`, `feature/search-by-title` | `topic/pr-template`, `topic/use-case-diagram`, `topic/answers-lab02` |
| Commit thường gặp | `feat:` | `docs:`, `chore:`, `ci:` |
| Ảnh hưởng phiên bản | Thường tăng MINOR (SemVer) | Thường không đổi số phiên bản |

Cả hai đều tạo từ `master`, merge vào `master` qua PR, và xóa sau khi merge. Tên
nhánh nói lên mục đích ("một nhánh, một mục đích").

## 4. So sánh 3 kiểu merge (quan sát trên lịch sử repo)

| | Merge commit (PR #4 `cart-print`) | Squash (PR #5 `search-by-title`) | Rebase (PR #6 `use-case-diagram`) |
|---|---|---|---|
| Giữ commit chi tiết | Có (3 commit + 1 merge commit) | Không, gộp thành 1 commit | Có, nhưng SHA mới |
| Lịch sử tuyến tính | Không | Có | Có |
| Revert cả tính năng | 1 lệnh `git revert -m 1 <merge>` | 1 lệnh `git revert <sha>` | Phải revert từng commit |
| `git branch -d` sau merge | Được | Báo *not fully merged* → xác nhận PR đã merge rồi dùng `-D` | Tương tự squash |
