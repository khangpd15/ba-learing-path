# VISI MEDICAL GROUP — REMICARE OPHTHALMIC POST-OP PLATFORM
# HỆ THỐNG TÀI LIỆU ĐẶC TẢ YÊU CẦU & THIẾT KẾ HỆ THỐNG (SYSTEM SPECIFICATIONS)

> **Dự án:** RemiCare Ophthalmic Post-Op Platform (Nền tảng Hướng dẫn và Giám sát Chăm sóc Hậu phẫu Nhãn khoa)  
> **Doanh nghiệp mục tiêu:** Công ty Cổ phần Tập đoàn Y khoa VISI (VISI Medical Group)  
> **Phiên bản tài liệu:** V1 (Chuẩn hóa toàn diện — Phê duyệt: 15/09/2026)  
> **Hội đồng Thẩm định Chuyên môn:** BS.CKII Trần Bá Kiền (Giám đốc Chuyên môn Tập đoàn), ThS. Nguyễn Tiến Đức (Giám đốc Vận hành)  

---

## 1. GIỚI THIỆU TỔNG QUAN HỆ THỐNG

**RemiCare** là nền tảng số hóa quy trình hướng dẫn, nhắc nhở và giám sát hồi phục hậu phẫu nhãn khoa chuyên sâu, được thiết kế riêng cho chuỗi 5 bệnh viện thuộc Tập đoàn Y khoa VISI (Bệnh viện Mắt VISI Thủ Đức, VISI Bình Dương, v.v.).

### 4 Vấn đề cốt lõi được giải quyết:
1. **Chống nhầm lẫn và rửa trôi thuốc mắt (F-009, F-010):** Bệnh nhân mổ mắt dùng 3–5 loại thuốc khác nhau. RemiCare cung cấp lịch thuốc trực quan và **Bộ đếm thời gian giãn cách 5–10 phút thông minh (Drop Interval Buffer Timer)** tự động khóa nút tra thuốc thứ 2 để mắt kịp hấp thu thuốc thứ nhất.
2. **Sàng lọc biến chứng & kích hoạt cấp cứu Red Flag (F-016, F-017, F-019):** Bảng kiểm **Recovery Check 3 mức (Xanh / Vàng / Đỏ)** xuất hiện mỗi sáng trong 7 ngày đầu; chế độ Cảnh báo Đỏ toàn màn hình cung cấp nút gọi 1 chạm đến **Hotline VISI 0395 151 151** trong khung giờ vàng.
3. **Số hóa quy trình bàn giao xuất viện dưới 30 giây (F-004, F-008, F-021, F-022):** Thay thế tờ rơi giấy dễ mất bằng mã QR bảo mật in trực tiếp trên Phiếu xuất viện; người nhà quét mã truy cập Web App (PWA) tức thì không cần cài đặt ứng dụng.
4. **Phân định minh bạch trách nhiệm 3 tầng phía bệnh viện:**
   * **Bác sĩ (`ACT-002`):** Cấu hình Master Template chuẩn hóa, sao chép phác đồ cá nhân hóa theo thể trạng bệnh nhân và xử lý ngoại lệ lâm sàng.
   * **Điều dưỡng (`ACT-003`):** Kích hoạt hồ sơ bệnh nhân trong <30s (chỉ nhập thông tin cơ bản, không sửa liều lượng thuốc) và in phiếu QR tại quầy lưu viện.
   * **CSKH / Giám sát lâm sàng (`ACT-004`):** Thường trực Dashboard cơ sở, cam kết gọi điện can thiệp ca Red Flag trong <5 phút, tự động leo thang sau 15 phút.

---

## 2. DANH MỤC TÀI LIỆU KỸ THUẬT (DOCUMENTATION INDEX)

Toàn bộ hệ thống tài liệu trong kho lưu trữ đã được đồng bộ 100% theo tài liệu chuẩn mực trung tâm **[US_US_V1.md](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/US_US_V1.md)**, tuân thủ nguyên tắc tách biệt độc lập giữa Nghiệp vụ (Business), Giao diện lập trình (RESTful APIs) và Cơ sở dữ liệu (Database Queries):

| STT | Tên Tài Liệu | File Path | Mục Tiêu & Nội Dung Trọng Tâm |
| :---: | :--- | :--- | :--- |
| **01** | **Tài Liệu Đặc Tả Chuẩn (Master Spec)** | [`US_US_V1.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/US_US_V1.md) | **Tài liệu gốc chuẩn hóa V1:** Danh mục 28 Features (`F-001`..`F-028`), 10 Tác nhân (`ACT-001`..`ACT-010`), 35 User Stories (`US-001`..`US-035`) và 31 Use Cases (`UC-001`..`UC-028`) được sắp xếp nghiêm ngặt theo thứ tự ID tăng dần. |
| **02** | **Đặc Tả Câu Chuyện Người Dùng** | [`file_user_story.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/file_user_story.md) | Đặc tả chi tiết 35 User Stories kèm tiêu chí chấp nhận chi tiết (Acceptance Criteria - Gherkin), giá trị nghiệp vụ và quy chuẩn y tế nhãn khoa VISI. |
| **03** | **Đặc Tả Ca Sử Dụng & Quy Tắc Nghiệp Vụ (Phân Rã CRUD)** | [`file_use_case_spec.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/file_use_case_spec.md) | Đặc tả chi tiết 31 Nhóm Use Case, trong đó **toàn bộ 6 nhóm Use Case Quản lý/Cấu hình (`UC-004`, `UC-005`, `UC-007`, `UC-008`, `UC-009`, `UC-026`) được phân rã thành 35 CRUD Sub-Use Cases đơn nguyên** (tổng cộng 60 ca sử dụng) kèm form UI, tiền/hậu điều kiện, luồng ngoại lệ và 26 Business Rules chuẩn VISI (đã tách độc lập đặc tả API và SQL). |
| **04** | **Đặc Tả Luồng Màn Hình (Screen Flow)** | [`04_Screen_Flow.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/04_Screen_Flow.md) | Đặc tả 24 màn hình giao diện (12 màn hình Caregiver Mobile PWA + 12 màn hình Hospital Clinical Portal) kèm 3 sơ đồ tương tác Mermaid. |
| **05** | **Ma Trận Ánh Xạ 4 Tầng (Traceability Matrix)** | [`05_Screen_Entity_Mapping.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/05_Screen_Entity_Mapping.md) | Ma trận ánh xạ toàn diện: 31 Use Cases ↔ 24 Màn hình ↔ 20 Thực thể Logic ↔ 24 Bảng CSDL Vật lý, bảo đảm độ phủ 100% không gãy đứt liên kết. |
| **06** | **Đặc Tả Giao Diện Lập Trình (RESTful API Specification)** | [`06_API_Specification.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/06_API_Specification.md) | **Tài liệu API độc lập:** Toàn bộ đặc tả RESTful API Level 3 cho 31 Use Cases & 35 CRUD Sub-UCs: HTTP Methods, Base URLs, Headers, JWT/RBAC Scopes, Request/Response Payloads, Query Params và mã lỗi HTTP 200/201/400/403/404/409/422. |
| **07** | **Danh Mục Câu Lệnh Truy Vấn CSDL (Database Queries & DML)** | [`07_Database_Queries.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/07_Database_Queries.md) | **Tài liệu SQL độc lập:** Tổng hợp toàn bộ câu lệnh PostgreSQL 16+ tương ứng với từng Use Case: SELECT JOINs có Index, INSERT `gen_random_uuid()`, UPDATE tham số hóa, Xóa mềm `ARCHIVED`, khối giao dịch ACID `BEGIN ... COMMIT` (nhân bản Care Plan, Red Flag escalation). |
| **08** | **Phân Tích Mô Hình Dữ Liệu** | [`01_Database_Analysis.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/01_Database_Analysis.md) | Phân tích 20 thực thể dữ liệu nghiệp vụ, thuộc tính, khóa chính/ngoại, chỉ mục và ma trận thao tác CRUD đối ứng với các ca sử dụng chuẩn. |
| **09** | **Sơ Đồ Thực Thể Quan Hệ (ERD)** | [`02_ERD.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/02_ERD.md) | Sơ đồ Master ERD Mermaid tổng thể hệ thống và 3 sơ đồ phân rã phân hệ dữ liệu, bảng tổng hợp khóa ngoại và hành vi On Delete. |
| **10** | **Thiết Kế Cơ Sở Dữ Liệu Vật Lý** | [`03_Database_Design.md`](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/03_Database_Design.md) | Đặc tả vật lý chi tiết 24 bảng CSDL PostgreSQL (kiểu dữ liệu, ràng buộc, default, indexes tối ưu và phân vùng dữ liệu theo 5 chi nhánh). |

---

## 3. KIẾN TRÚC TRUY VẾT YÊU CẦU & PHÂN TÁCH KỸ THUẬT (TRACEABILITY ARCHITECTURE)

```
[Functional Requirements: F-001 .. F-028]
                   │
                   ▼
       [Actors: ACT-001 .. ACT-010]
                   │
                   ▼
    [User Stories: US-001 .. US-035] ──► file_user_story.md
                   │
                   ▼
    [Use Cases: UC-001 .. UC-028]    ──► file_use_case_spec.md (Thuần Nghiệp Vụ & Rules)
        │                  │                   │
        ├──────────────────┼───────────────────┤
        ▼                  ▼                   ▼
 [RESTful API Spec] [UI Screens Flow]   [Database Queries & DML]
06_API_Specification 04_Screen_Flow      07_Database_Queries
        │                  │                   │
        └──────────────────┼───────────────────┘
                           ▼
                 [Entities & ERD & DDL]
                 01_Database_Analysis.md
                 02_ERD.md
                 03_Database_Design.md
                           │
                           ▼
                [Ma Trận Ánh Xạ 4 Tầng]
              05_Screen_Entity_Mapping.md
```

---

## 4. QUY CHUẨN TUÂN THỦ PHÁP LÝ & AN TOÀN Y TẾ

* **Nghị định 13/2023/NĐ-CP (Bảo vệ dữ liệu cá nhân):** Tên bệnh nhân hiển thị trên ứng dụng và phiếu in được lưu trữ dưới dạng viết tắt bảo mật (`Họ tên viết tắt: Nguyễn V. A.`); không lưu CCCD; mã QR chỉ chứa token mã hóa ngẫu nhiên không lộ dữ liệu thô.
* **Tiêu chuẩn Dược lý Nhãn khoa:** Tự động áp dụng khoảng cách đệm 5–10 phút giữa 2 loại thuốc nhỏ mắt (BR23) để chống hiện tượng rửa trôi thuốc (washout effect).
* **Cam kết SLA Y tế Khẩn cấp:** Hotline cấp cứu 24/7 `0395 151 151`, CSKH tiếp nhận cảnh báo trong <5 phút, tự động kích hoạt cơ chế leo thang (Escalation) gửi tin nhắn khẩn tới Bác sĩ trực nếu quá hạn 15 phút (BR12).
