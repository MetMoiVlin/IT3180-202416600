# Hướng dẫn Thiết kế Prompt AI & Báo cáo Phân tích Repository SCM (IT3180)

> Tài liệu thực hiện theo yêu cầu **Bài 05 - Quản lý cấu hình phần mềm (IT3180 - Nhập môn CNPM)**.  
> Sinh viên thực hiện: **Nguyễn Hoàng Thái Sơn** – MSSV: **202416600**  
> Đối tượng phân tích: 
> - Nhóm 7: `https://github.com/neveraccept/IT3180_2025.2_Nhom7`
> - Nhóm 4: `https://github.com/khanh201104/Bai_tap_lon_IT3180_nhom_4`

---

## PHẦN 1: THIẾT KẾ PROMPT AI ĐỂ PHÂN TÍCH REPOSITORY

### 1.1 Mục tiêu thiết kế
Prompt được xây dựng theo kỹ thuật Prompt Engineering nâng cao (Role prompting + Few-shot context + Structured output), cho phép các mô hình AI (như ChatGPT, Gemini, Claude, Cursor) đóng vai trò Chuyên gia Đánh giá Quản lý Cấu hình Phần mềm (SCM Auditor) để mổ xẻ repository qua 7 tiêu chí bắt buộc.

### 1.2 Master Prompt Template

```text
[VAI TRÒ & NGỮ CẢNH]
Bạn là một Chuyên gia Cấp cao về Quản lý Cấu hình Phần mềm (Senior SCM Engineer & DevOps Auditor) kiêm Giảng viên bộ môn Nhập môn Công nghệ Phần mềm (IT3180). 
Nhiệm vụ của bạn là đánh giá toàn diện kho lưu trữ mã nguồn (Git Repository) của sinh viên thực hiện bài tập lớn, nhằm xác định mức độ trưởng thành về quy trình phát triển phần mềm, kỷ luật Git và khả năng bảo trì mã nguồn.

[DỮ LIỆU ĐẦU VÀO]
Link Repository cần phân tích: {{REPO_URL}}
(Nếu AI có công cụ duyệt web/GitHub API, hãy thu thập cấu trúc tệp, git log, nhánh, PRs, workflow. Nếu không có kết nối trực tiếp, phân tích dựa trên snapshot thông tin được cung cấp bên dưới).

[YÊU CẦU ĐÁNH GIÁ 7 TIÊU CHÍ BẮT BUỘC]
Hãy phân tích chi tiết và cho điểm từ 1 đến 10 cho từng tiêu chí sau:

1. Cấu trúc thư mục (Folder Structure & Architecture):
   - Có phân tách rõ ràng các tầng kiến trúc (Frontend, Backend, Docs, Tests, Scripts) hay dồn hết vào một thư mục gốc?
   - Cấu trúc có tuân theo thông lệ tốt của ngôn ngữ/framework sử dụng không?

2. Chất lượng tài liệu (Documentation & README):
   - README.md có đầy đủ: Mô tả dự án, kiến trúc hệ thống, hướng dẫn cài đặt môi trường (Prerequisites), cách cấu hình (.env), lệnh chạy dev/prod, tài khoản mẫu?
   - Tài liệu có sơ đồ kiến trúc / use case không, hay chỉ vỏn vẹn một vài dòng sơ sài?

3. Quy trình sử dụng Git (Branching Model & Collaboration):
   - Nhóm sử dụng mô hình nào (GitFlow, GitHub Flow, Release Flow) hay "Trunk-based nguy hiểm" (tất cả thành viên commit/push trực tiếp vào nhánh main/master)?
   - Có sử dụng nhánh tính năng (feature branches), Pull Request (PR) có gắn reviewer và giải quyết thảo luận trước khi merge không?

4. Chất lượng Commit (Commit Discipline & Traceability):
   - Commit message có tuân theo chuẩn (như Conventional Commits: feat, fix, docs, refactor, chore...) và mang ý nghĩa nghiệp vụ rõ ràng (“Thêm chức năng đăng nhập Google OAuth2”) hay là các commit vô nghĩa (“fix”, “update”, “code moi”, “aaa”, “123”)?
   - Kích thước commit (atomic commits) có vừa phải hay một commit nhồi nhét hàng nghìn dòng mã?

5. Kiểm thử & Tự động hóa CI/CD (Testing & CI/CD Pipeline):
   - Có thư mục chứa mã kiểm thử tự động (Unit Test, Integration Test) không?
   - Có thiết lập pipeline CI/CD (.github/workflows/*.yml, .gitlab-ci.yml) để tự động build, lint và chạy test khi mở PR không?

6. Quản lý phụ thuộc & Cấu hình môi trường (Dependencies & Hygiene):
   - Có tệp khai báo phụ thuộc chuẩn hóa (requirements.txt / package.json / pom.xml) với phiên bản cụ thể (lock file)?
   - Có tệp .gitignore hợp lý ngăn chặn việc đẩy rác build, tệp IDE cá nhân (.idea, .vscode, __pycache__, node_modules) hoặc rò rỉ mã khóa bí mật (.env, credentials)?

7. Nhịp độ làm việc & Tinh thần đồng đội (Cadence & Team Velocity):
   - Lịch sử commit trải đều theo các tuần/sprint suốt cả kỳ học, hay dồn cục 100 commit vào 2 ngày trước deadline nộp bài?
   - Mức độ đóng góp của các thành viên có đồng đều hay chỉ 1 người gánh team ("Bus Factor = 1")?

[ĐỊNH DẠNG ĐẦU RA YÊU CẦU]
1. Bảng tóm tắt điểm số (Scorecard 7 tiêu chí, Thang điểm 10).
2. Phân tích chi tiết từng tiêu chí kèm dẫn chứng cụ thể từ repo.
3. Điểm mạnh nổi bật (Strengths) & Lỗ hổng nghiêm trọng (Critical Weaknesses/Smells).
4. Kế hoạch hành động cải tiến (Actionable Recommendations) để đạt điểm A SCM.
```

---

## PHẦN 2: BÁO CÁO PHÂN TÍCH ĐỐI SÁNH: NHÓM 7 VS NHÓM 4

### 2.1 Bảng tổng hợp điểm đánh giá SCM (Scorecard)

| Tiêu chí đánh giá | Nhóm 7 (`IT3180_2025.2_Nhom7`) | Nhóm 4 (`Bai_tap_lon_IT3180_nhom_4`) | Đánh giá so sánh |
| :--- | :---: | :---: | :--- |
| **1. Cấu trúc thư mục** | **7.5 / 10** | **4.0 / 10** | Nhóm 7 tách biệt mã nguồn tốt hơn; Nhóm 4 dồn nhiều file vào root |
| **2. Chất lượng tài liệu** | **7.0 / 10** | **3.0 / 10** | Nhóm 7 có README rõ ràng hơn; Nhóm 4 chỉ vài dòng cơ bản |
| **3. Quy trình Git** | **5.0 / 10** | **3.5 / 10** | Cả hai nhóm đều mắc lỗi push thẳng `main`, ít tận dụng PR review |
| **4. Chất lượng commit** | **6.0 / 10** | **3.0 / 10** | Nhóm 7 có mô tả tiếng Việt có nghĩa; Nhóm 4 nhiều commit "update", "fix" |
| **5. Kiểm thử & CI/CD** | **4.0 / 10** | **2.0 / 10** | Cả hai nhóm đều thiếu tự động hóa CI/CD; kiểm thử rất mỏng |
| **6. Quản lý phụ thuộc** | **8.0 / 10** | **5.0 / 10** | Nhóm 7 có `.gitignore` và `package.json`/`pom.xml` tốt hơn |
| **7. Nhịp độ làm việc** | **6.0 / 10** | **4.0 / 10** | Cả hai đều có hiện tượng "dồn toa" sát hạn nộp, nhưng Nhóm 7 rải rác hơn |
| **TỔNG KẾT (TRUNG BÌNH)** | **6.2 / 10 (Khá)** | **3.5 / 10 (Yếu)** | **Nhóm 7 vượt trội về tổ chức mã nguồn và quản lý tài liệu** |

---

## PHẦN 3: BÀI HỌC ÁP DỤNG VÀO BÀI THỰC HÀNH AIMS-MINI

Để không mắc phải những điểm yếu của các nhóm BTL kỳ trước, repository **AIMS-mini** này của sinh viên **Nguyễn Hoàng Thái Sơn (MSSV 202416600)** được thiết kế và thực thi chuẩn mực:
1. **Kiến trúc phân tầng chuẩn**: Package `aims/` độc lập, tách riêng `tests/`, `docs/`, `.github/`.
2. **Quy trình Release Flow**: Tạo các nhánh phát hành `release/lab01` đến `release/lab05`, sử dụng `feature/` và `topic/` với Pull Request template.
3. **Kỷ luật Commit**: 100% commit tuân thủ quy ước **Conventional Commits** (`feat:`, `fix:`, `docs:`, `chore:`, `ci:`, `refactor:`).
4. **Kiểm thử & CI/CD**: Xây dựng bộ Unit Test hoàn chỉnh (`test_cart.py`, `test_discount.py`, `test_search.py`) và pipeline GitHub Actions tự động kiểm tra mã nguồn.
5. **Phiên bản hóa ngữ nghĩa (SemVer)**: Đánh thẻ annotated tag cố định `v0.1.0`, `v0.2.0`, `v0.3.0`, `v1.0.0`, `v1.0.1`, `v1.1.0` kèm `CHANGELOG.md` chuẩn Keep a Changelog.
