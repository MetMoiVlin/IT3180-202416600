# Lab 04 – Trả lời câu hỏi thảo luận

Sinh viên: Nguyễn Hoàng Thái Sơn - MSSV: 202416600

## 1. Vì sao chuẩn bị phát hành trên `master` trước, không sửa trên nhánh release?

- Nhánh `release/lab04` là **baseline**, tức là ảnh chụp của `master` tại thời điểm
  phát hành. Nếu tạo nhánh trước rồi mới sửa số phiên bản hay CHANGELOG trên đó, thì
  `release/lab04` có những commit mà `master` không có.
- Theo Release Flow, `release/*` **không bao giờ merge ngược** vào `master`. Những
  sửa đổi đó sẽ bị kẹt lại ở nhánh release: `master` không có CHANGELOG 1.0.0, và
  bản 1.1.0 sau này phải làm lại từ đầu.
- Chuẩn bị qua `topic/prepare-release-1.0.0` → PR → `master` giúp thay đổi được
  review như mọi thay đổi khác. Nhánh release tạo ra **giống hệt** `master`, nên
  `git log master..release/lab04` rỗng, và tag `v1.0.0` trỏ đúng commit đã được
  review.
- Trên nhánh release chỉ còn những thay đổi bắt buộc *sau* phát hành, tức hotfix
  (Lab 05).

## 2. Vì sao `release/*` không merge vào `master`? So sánh với GitFlow

- Trong Release Flow, `master` là nguồn sự thật duy nhất: mọi thay đổi đi vào
  `master` trước, còn `release/*` chỉ **tách ra** từ đó. Nhánh release chỉ chứa:
  - những gì `master` đã có tại thời điểm tách (không cần merge lại);
  - hotfix riêng cho dòng phiên bản đó, ví dụ commit nâng số `1.0.1`. Commit này
    **không được phép** về `master`, vì `master` đã ở `1.1.0`. Bản sửa lỗi được lan
    truyền có chọn lọc bằng `cherry-pick -x`.
- Nếu merge `release/*` vào `master`, ta sẽ kéo cả số phiên bản cũ và CHANGELOG của
  dòng cũ vào, gây xung đột. Lịch sử `master` cũng sẽ rối với nhiều đường merge chéo.
- **GitFlow** thì khác: có `develop` (tích hợp) và `main` (chỉ chứa bản phát hành).
  `release/*` tách từ `develop`, được ổn định, rồi **merge vào cả `main` và
  `develop`**. Hotfix tách từ `main` và cũng merge vào cả hai. GitFlow hợp với
  phát hành định kỳ, mỗi lúc chỉ có một bản đang hỗ trợ. Release Flow đơn giản hơn
  (một nhánh tích hợp) và hợp với việc duy trì **nhiều dòng phát hành song song**
  (`release/lab04`, `release/lab05`...).

| | Release Flow (học phần) | GitFlow |
|---|---|---|
| Nhánh tích hợp | `master` | `develop` |
| Nhánh phát hành tạo từ | `master` | `develop` |
| Phát hành xong | Giữ `release/*` làm dòng bảo trì, **không merge về** | Merge vào `main` và `develop`, rồi xóa |
| Hotfix | Từ `release/*`, lan truyền bằng cherry-pick | Từ `main`, merge vào `main` + `develop` |

## 3. Vì sao lên `1.0.0` mà không phải `0.4.0`?

- Theo SemVer, `0.y.z` là giai đoạn phát triển ban đầu: API có thể thay đổi bất cứ
  lúc nào, chưa có cam kết tương thích. Các bản Lab 01–03 (`v0.1.0`…`v0.3.0`) đúng là
  như vậy: chuyển package, thêm/sửa tính năng, revert.
- Lab 04 là lần **phát hành chính thức đầu tiên** cho người dùng: có CHANGELOG,
  GitHub Release, nhánh release được bảo vệ, và từ đây bắt đầu bảo trì (hotfix ở
  Lab 05). SemVer quy định rằng khi phần mềm đã được dùng thật và có API công khai
  ổn định thì nên là `1.0.0`.
- Từ `1.0.0` trở đi, số phiên bản mang ý nghĩa cam kết: sửa lỗi tăng PATCH
  (`1.0.1`), thêm tính năng tương thích tăng MINOR (`1.1.0`), phá vỡ tương thích tăng
  MAJOR (`2.0.0`). Với `0.4.0` thì không có cam kết nào như vậy.

## 4. Tag và nhánh release dùng thế nào trong bài

- Tag `v1.0.0` (annotated) là định danh **bất biến** của baseline: không bao giờ di
  chuyển, đã chia sẻ thì không xóa. Các tag `v0.1.0`–`v0.3.0` được gắn bù lên
  `origin/release/lab01`–`lab03`.
- Nhánh `release/lab04` là **dòng bảo trì** của 1.0.x. Nó di chuyển khi có hotfix,
  và được bảo vệ (bắt buộc PR, cấm force push, cấm xóa).
- Ghi nhận trạng thái và tái tạo:
  - `git log release/lab04..master` / `master..release/lab04`: so sánh hai dòng;
  - `git describe --tags`: cho biết đang cách tag gần nhất bao nhiêu commit;
  - `git worktree add ../aims-v1.0.0 v1.0.0`: dựng lại đúng phiên bản 1.0.0 để chạy
    test, không ảnh hưởng thư mục làm việc hiện tại.
