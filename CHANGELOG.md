# Changelog

Mọi thay đổi đáng chú ý của AIMS-mini được ghi lại trong tệp này.

Định dạng theo [Keep a Changelog](https://keepachangelog.com/en/1.1.0/),
đánh số phiên bản theo [Semantic Versioning](https://semver.org/spec/v2.0.0.html).

## [Unreleased]

## [1.1.0] - 2026-11-06

Bản phát hành Lab 05 (`release/lab05`).

### Added
- `Cart.sort_by_title()` và `Cart.sort_by_cost()` sắp xếp giỏ hàng
- `aims.search.search_by_category()` tìm đĩa theo thể loại
- CI với GitHub Actions: chạy unit test và smoke test tự động

### Fixed
- `search_by_title()` không phân biệt hoa thường (cherry-picked từ bản vá 1.0.1)

## [1.0.1] - 2026-11-06

Bản vá cho dòng 1.0.x (`release/lab04`).

### Fixed
- `search_by_title()` không phân biệt hoa thường: tìm "lion" ra "The Lion King"

## [1.0.0] - 2026-11-06

Phiên bản ổn định đầu tiên (Lab 04).

### Added
- `aims.__version__` và dòng in phiên bản `AIMS-mini v1.0.0` khi chạy `main.py`
- `CHANGELOG.md`
- Hoàn thiện tính năng Cart, Search, Discount và kiểm thử tự động

## [0.3.0] - 2026-11-05

Bản nộp Lab 03 (`release/lab03`).

### Added
- `Cart.total_cost_with_discount(percent)` tính tổng tiền sau giảm giá
- Thông báo giỏ hàng bằng tiếng Việt
- Bộ kiểm thử tự động `tests/test_cart.py`, `tests/test_discount.py`

### Fixed
- Giỏ hàng nhận đĩa thứ 21 dù giới hạn là 20
- Sửa công thức tính giảm giá và kiểm tra percent trong khoảng 0–100

## [0.2.0] - 2026-11-05

Bản nộp Lab 02 (`release/lab02`).

### Added
- `Cart.print_cart()` in chi tiết giỏ hàng
- `aims.search.search_by_title()` tìm đĩa theo tiêu đề
- Biểu đồ use case `docs/use-case.md`
- Pull Request template `.github/pull_request_template.md`

## [0.1.0] - 2026-11-05

Bản nộp Lab 01 (`release/lab01`).

### Added
- Lớp `DigitalVideoDisc`, `Cart` và chương trình demo `main.py`

### Changed
- Chuyển mã nguồn vào package `aims/` bằng `git mv`
