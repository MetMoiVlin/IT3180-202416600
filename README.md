# AIMS-mini: Quản lý Cấu hình Phần mềm – IT3180

Ứng dụng mẫu và báo cáo thực hành hoàn chỉnh cho học phần **IT3180 – Quản lý cấu hình phần mềm / Nhập môn Công nghệ Phần mềm** (Học kỳ 20261 - HUST).

- **Sinh viên thực hiện**: **Nguyễn Hoàng Thái Sơn** – **MSSV**: **202416600**
- **Ngôn ngữ & Môi trường**: Python ≥ 3.10 (chỉ sử dụng thư viện chuẩn, không phụ thuộc bên ngoài).
- **Hệ thống CI/CD**: GitHub Actions tự động kiểm thử trên Ubuntu Linux.

---

## 1. Cấu trúc Dự án

Repository được tổ chức theo chuẩn phân tầng tách bạch giữa mã nguồn, kiểm thử, tài liệu và quy trình tự động hóa:

```text
aims-mini/
├── .github/
│   ├── pull_request_template.md    # Mẫu tạo Pull Request chuẩn hóa
│   └── workflows/
│       └── ci.yml                  # Pipeline CI tự động chạy Unit Test
├── aims/                           # Package mã nguồn chính của ứng dụng
│   ├── __init__.py                 # Khai báo phiên bản hệ thống (__version__)
│   ├── cart.py                     # Lớp Cart quản lý giỏ hàng và sắp xếp
│   ├── dvd.py                      # Lớp DigitalVideoDisc đại diện cho đĩa DVD
│   └── search.py                   # Các hàm tìm kiếm theo tiêu đề và thể loại
├── docs/                           # Tài liệu thiết kế, thảo luận và báo cáo AI
│   ├── answers-lab01.md            # Trả lời câu hỏi thảo luận Lab 01
│   ├── answers-lab02.md            # Trả lời câu hỏi thảo luận Lab 02
│   ├── answers-lab03.md            # Trả lời câu hỏi thảo luận Lab 03
│   ├── answers-lab04.md            # Trả lời câu hỏi thảo luận Lab 04
│   ├── answers-lab05.md            # Trả lời câu hỏi thảo luận Lab 05
│   ├── use-case.md                 # Biểu đồ và mô tả Use Case chi tiết (Mermaid)
│   ├── AI_REPO_ANALYSIS_GUIDE.md   # Hướng dẫn thiết kế Prompt AI & Báo cáo SCM
│   └── Su_dung_AI_phan_tich_repo_Nhom_7.html # Giao diện HTML báo cáo phân tích Repo
├── tests/                          # Bộ kiểm thử tự động (Unit Tests)
│   ├── test_cart.py                # Kiểm thử giỏ hàng (sức chứa, sắp xếp)
│   ├── test_discount.py            # Kiểm thử tính toán giảm giá
│   └── test_search.py              # Kiểm thử tìm kiếm theo tiêu đề & thể loại
├── .gitattributes                  # Cấu hình chuẩn hóa ký tự xuống dòng (LF)
├── .gitignore                      # Loại bỏ file rác Python/IDE
├── CHANGELOG.md                    # Lịch sử thay đổi chuẩn Keep a Changelog
├── main.py                         # Điểm chạy demo ứng dụng dòng lệnh
└── README.md                       # Tài liệu hướng dẫn sử dụng
```

---

## 2. Hướng dẫn Chạy Chương trình & Kiểm thử

### 2.1 Chạy chương trình demo
```bash
python main.py
```
*Kết quả hiển thị:*
```text
AIMS-mini v1.1.0
Đã thêm đĩa vào giỏ hàng
Đã thêm đĩa vào giỏ hàng
Đã thêm đĩa vào giỏ hàng
*********************** CART ***********************
Ordered Items:
1. DVD - The Lion King - Animation - Roger Allers - 87: 19.95 $
2. DVD - Star Wars - Science Fiction - George Lucas - 87: 24.95 $
3. DVD - Aladdin - Animation -  - 0: 18.99 $
Total cost: 63.89 $
****************************************************
```

### 2.2 Chạy bộ kiểm thử tự động (Unit Tests)
```bash
python -m unittest discover -s tests -v
```

---

## 3. Lộ trình Hoàn thiện 5 Bài Thực hành (Lab 01 – Lab 05)

Dự án tuân thủ nghiêm ngặt chiến lược phân nhánh **Microsoft Release Flow**:

| Bài Lab | Nhánh Release | Tag Baseline | Các tính năng & Thay đổi chính |
| :---: | :---: | :---: | :--- |
| **Lab 01** | `release/lab01` | `v0.1.0` | Tái cấu trúc mã nguồn ban đầu vào package `aims/` bằng `git mv` để bảo toàn lịch sử; giải thích khái niệm baseline. |
| **Lab 02** | `release/lab02` | `v0.2.0` | Bổ sung `Cart.print_cart()`, `search_by_title()`, thiết lập PR Template và sơ đồ Use Case (`docs/use-case.md`). |
| **Lab 03** | `release/lab03` | `v0.3.0` | Thêm tính năng giảm giá (`total_cost_with_discount`), fix bug sức chứa tối đa 20 đĩa, việt hóa thông báo và thêm bộ Unit Test đầu tiên. |
| **Lab 04** | `release/lab04` | `v1.0.0`<br>`v1.0.1` | Phát hành phiên bản chính thức đầu tiên `1.0.0` theo SemVer. Thực hiện hotfix tìm kiếm không phân biệt hoa thường và vá lên bản `1.0.1`. |
| **Lab 05** | `release/lab05` | `v1.1.0` | Lan truyền bản vá hotfix về `master` bằng `git cherry-pick -x`. Bổ sung sắp xếp giỏ hàng (`sort_by_title`, `sort_by_cost`), tìm theo thể loại (`search_by_category`) và thiết lập GitHub Actions CI. |

---

## 4. Phần Đánh giá & Phân tích Repository bằng AI (Theo yêu cầu Bài 05)

Theo yêu cầu bài học, repository này cung cấp đầy đủ:
1. **Thiết kế Prompt AI hoàn chỉnh**: Nằm trong [`docs/AI_REPO_ANALYSIS_GUIDE.md`](docs/AI_REPO_ANALYSIS_GUIDE.md) cho phép sử dụng AI để đánh giá bất kỳ repository nào theo 7 tiêu chí SCM.
2. **Báo cáo đối sánh chi tiết giữa Nhóm 7 và Nhóm 4**: Phân tích sâu về cấu trúc thư mục, tài liệu, quy trình Git, kỷ luật commit, kiểm thử, quản lý phụ thuộc và nhịp độ làm việc.
3. **Báo cáo Dashboard trực quan**: Mở tệp [`docs/Su_dung_AI_phan_tich_repo_Nhom_7.html`](docs/Su_dung_AI_phan_tich_repo_Nhom_7.html) trên trình duyệt để xem giao diện đánh giá trực quan.

---

## 5. Hướng dẫn Đẩy Repo lên GitHub Cá nhân để Nộp Bài

Để nộp bài vào link Google Sheets của giảng viên, bạn thực hiện các bước sau:

1. Đăng nhập vào tài khoản [GitHub](https://github.com) của bạn và bấm **New repository** (đặt tên ví dụ: `it3180-202416600-aims` hoặc `it3180-aims-mini`). **Không** tích chọn thêm README hay .gitignore (để repo trống).
2. Mở terminal tại thư mục này và liên kết remote:
   ```bash
   git remote add origin https://github.com/<tên_tài_khoản_của_bạn>/it3180-202416600-aims.git
   ```
3. Đẩy nhánh `master` và toàn bộ các nhánh release lên GitHub:
   ```bash
   git push -u origin master
   git push origin --all
   ```
4. Đẩy tất cả các thẻ phiên bản (Tags) lên GitHub:
   ```bash
   git push origin --tags
   ```
5. Sao chép đường dẫn repository trên GitHub (ví dụ: `https://github.com/your-username/it3180-202416600-aims`) và điền vào form Google Sheets của môn học.
