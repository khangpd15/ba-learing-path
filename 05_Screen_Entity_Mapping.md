# 05. MA TRẬN ÁNH XẠ NGHIỆP VỤ (SCREEN - ENTITY - DATABASE MAPPING)
# MA TRẬN TRUY XUẤT 4 TẦNG TOÀN DIỆN — VISI MEDICAL GROUP

> **Dự án:** RemiCare Ophthalmic Post-Op Platform (Nền tảng Hướng dẫn và Giám sát Chăm sóc Hậu phẫu Nhãn khoa)  
> **Doanh nghiệp mục tiêu:** Công ty Cổ phần Tập đoàn Y khoa VISI (VISI Medical Group)  
> **Phiên bản:** V1 (Đồng bộ toàn diện theo tài liệu chuẩn US_US_V1 - Ánh xạ 100% 38 Use Cases UC-001 đến UC-028 bao gồm phân rã CRUD)  
> **Ngày phê duyệt:** 15/09/2026  
> **Nguyên tắc kỹ thuật:** End-to-End Traceability (Màn hình ↔ Ca sử dụng ↔ Thực thể ↔ Bảng vật lý CSDL)  

---

## 1. MỤC TIÊU & PHƯƠNG PHÁP LUẬN TRUY VẾT

Ma trận ánh xạ 4 tầng này đóng vai trò là "xương sống" bảo đảm tính nhất quán tuyệt đối giữa:
1. **Tầng Yêu cầu Nghiệp vụ (Requirement Layer):** 31 Ca sử dụng (`UC-001` đến `UC-028`, `UC-010b`, `UC-010c`, `UC-022b`) trong [US_US_V1.md](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/US_US_V1.md) và [file_use_case_spec.md](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/file_use_case_spec.md).
2. **Tầng Giao diện Người dùng (Presentation Layer):** 24 Màn hình giao diện (`SCR-CG-01` đến `SCR-CG-12`, `SCR-DOC-01` đến `SCR-DOC-12`) trong [04_Screen_Flow.md](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/04_Screen_Flow.md).
3. **Tầng Mô hình Thực thể Logic (Logical Entity Layer):** 20 Thực thể nghiệp vụ trong [01_Database_Analysis.md](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/01_Database_Analysis.md) và [02_ERD.md](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/02_ERD.md).
4. **Tầng Cơ sở Dữ liệu Vật lý (Physical Database Layer):** 24 Bảng CSDL PostgreSQL trong [03_Database_Design.md](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/03_Database_Design.md).

### Mục tiêu kiểm tra chất lượng (Quality Gates):
* **Zero Orphan Screens:** Mọi màn hình giao diện đều giải quyết ít nhất một Use Case cụ thể.
* **Zero Dead Tables:** Mọi bảng CSDL đều có luồng CRUD phát sinh từ hành động của người dùng.
* **Zero Data Loss:** Dữ liệu nhập từ Caregiver hoặc Nhân viên y tế được lưu vết toàn vẹn và có thể truy nguyên qua Audit Trail.

---

## 2. MA TRẬN ÁNH XẠ 4 TẦNG TOÀN DIỆN (TRACEABILITY MATRIX)

| Mã UC | Tên Use Case | Màn hình UI (`SCR-*`) | Thực thể Nghiệp vụ | Bảng CSDL Vật lý | Thao tác CRUD & Hành vi Dữ liệu |
| :--- | :--- | :--- | :--- | :--- | :--- |
| **UC-001** | Đăng ký & Đăng nhập Caregiver qua OTP | `SCR-CG-01`: Đăng nhập OTP Caregiver | `CaregiverProfile`<br>`UserAccount`<br>`OtpVerification` | `caregiver_profiles`<br>`accounts`<br>`otp_verifications` | **C, R, U**: Xác thực số điện thoại, sinh và kiểm tra OTP; cấp phiên JWT; khởi tạo hồ sơ Caregiver mới nếu chưa có. |
| **UC-003.1** | Quét QR Bàn Giao Liên Kết Hồ Sơ Bệnh Nhân | `SCR-CG-01`, `SCR-CG-02`, `SCR-CG-03` | `CaregiverPatientLink`<br>`PatientCarePlan` | `caregiver_patient_links`<br>`patient_care_plans` | **C, R**: Quét giải mã QR token phiếu xuất viện; kiểm tra trần ≤3 Caregiver (BR5); lưu liên kết người chăm sóc. |
| **UC-003.2** | Xem & Lựa Chọn Care Recipient Đang Chăm Sóc | `SCR-CG-02`, `SCR-CG-04` | `CaregiverPatientLink`<br>`PatientProfile`<br>`PatientCarePlan` | `caregiver_patient_links`<br>`patients`<br>`patient_care_plans` | **R, U**: Truy vấn danh sách người thân theo 2 Tab (Active vs Past Patients); lưu `active_patient_id` chuyển đổi ngữ cảnh 1-chạm trên Header. |
| **UC-003.3** | Xem Dòng Thời Gian Chăm Sóc & Ca Mổ Cũ | `SCR-CG-02`, `SCR-CG-04` | `MedicationLog`<br>`RecoveryCheckSubmission`<br>`PatientCallLog` | `medication_logs`<br>`recovery_check_submissions`<br>`patient_call_logs` | **R**: Truy vấn Care Action Timeline hiển thị người xác nhận thuốc (chống nhỏ trùng liều), kết quả kiểm tra hồi phục và tra cứu hồ sơ phẫu thuật cũ (IOL, đơn thuốc). |
| **UC-004.2** | Tra Cứu & Lọc Bệnh Nhân (Read/List) | `SCR-DOC-03`: Danh sách bệnh nhân | `PatientProfile` | `patients` | **R**: Truy vấn danh sách bệnh nhân phân trang theo cơ sở (`facility_id`), lọc theo loại mổ và mức cảnh báo. |
| **UC-004.3** | Xem Chi Tiết Hồ Sơ & Timeline (Read/Detail)| `SCR-DOC-05`, `SCR-DOC-06` | `PatientProfile`<br>`PatientCarePlan` | `patients`<br>`patient_care_plans` | **R**: Tải toàn diện hồ sơ bệnh nhân 360 độ: tiến trình dùng thuốc, lịch sử nộp khảo sát, Caregiver liên kết. |
| **UC-004.4** | Cập Nhật Hồ Sơ Bệnh Nhân (Update) | `SCR-DOC-05`, `SCR-DOC-06`: Chỉnh sửa hồ sơ BN | `PatientProfile` | `patients` | **U**: Cập nhật SĐT liên hệ, ghi chú cơ địa/dị ứng; khóa loại mổ nếu Care Plan đã kích hoạt để an toàn y khoa. |
| **UC-004.5** | Lưu Trữ / Vô Hiệu Hóa (Archive/Soft Delete)| `SCR-DOC-03`, `SCR-DOC-05` | `PatientProfile` | `patients` | **D**: Chuyển `status = 'ARCHIVED'`, vô hiệu hóa mã QR chưa liên kết, bảo toàn dữ liệu y khoa theo quy chế viện. |
| **UC-005.1** | Tạo Mới Master Template (Create) | `SCR-DOC-07`: Tạo mẫu mới | `CarePlanTemplate` | `care_plan_templates` | **C**: Khởi tạo khung phác đồ mẫu ở trạng thái `DRAFT` v1.0 gắn với loại phẫu thuật chuẩn hóa (BR16). |
| **UC-005.2** | Xem Danh Sách Master Template (Read/List)| `SCR-DOC-07`: Danh mục Template | `CarePlanTemplate` | `care_plan_templates` | **R**: Hiển thị danh mục toàn bộ các phác đồ mẫu, hỗ trợ lọc theo loại phẫu thuật và trạng thái vòng đời. |
| **UC-005.3** | Xem Chi Tiết Cấu Hình (Read/Detail) | `SCR-DOC-08`: Không gian cấu hình | `CarePlanTemplate` | `care_plan_templates` | **R**: Tải toàn bộ 4 cấu phần: thuốc mẫu, mốc câu hỏi recovery check, red flag, cẩm nang 24h & Do/Don't. |
| **UC-005.4** | Chỉnh Sửa Thông Tin Chung (Update) | `SCR-DOC-08` (Tab 1: Thông tin) | `CarePlanTemplate` | `care_plan_templates` | **U**: Sửa tên, mô tả lâm sàng, số ngày theo dõi chuẩn; tự động nhân bản v1.1 nếu template đã ACTIVE (BR15). |
| **UC-005.5** | Kích Hoạt / Lưu Trữ Template (Status) | `SCR-DOC-08` (Header duyệt) | `CarePlanTemplate` | `care_plan_templates` | **U**: Chuyển trạng thái `PENDING_APPROVAL`, `ACTIVE`, `INACTIVE` theo thẩm quyền Giám đốc Lâm sàng (GCMO). |
| **UC-006** | Phê Duyệt Lâm Sàng Master Template | `SCR-DOC-09`: Thẩm định & Vòng đời Template | `CarePlanTemplate`<br>`SystemAuditLog` | `care_plan_templates`<br>`system_audit_logs` | **U, C**: Giám đốc Chuyên môn (BS.CKII Trần Bá Kiền) ký duyệt điện tử, chuyển trạng thái template sang `ACTIVE` ban hành toàn chuỗi; ghi vết kiểm toán. |
| **UC-007.1** | Thêm Thuốc Mẫu & Timer Đệm (Create) | `SCR-DOC-08` (Tab 2: Thuốc mẫu) | `TemplateMedication` | `template_medications` | **C**: Thêm thuốc mẫu, đặt số giọt, khung giờ 4 cữ và cấu hình thời gian đệm 5–10 phút chống rửa trôi (BR23). |
| **UC-007.2** | Xem Danh Sách Thuốc Mẫu (Read/List) | `SCR-DOC-08` (Tab 2: Thuốc mẫu) | `TemplateMedication` | `template_medications` | **R**: Hiển thị bảng danh mục thuốc, thứ tự nhỏ mắt ưu tiên (nước trước, mỡ sau) và thời gian đệm giữa các lọ. |
| **UC-007.3** | Chỉnh Sửa Thuốc Mẫu & Đệm (Update) | `SCR-DOC-08` (Tab 2: Thuốc mẫu) | `TemplateMedication` | `template_medications` | **U**: Cập nhật liều lượng, số lần dùng hoặc điều chỉnh thời gian đệm giãn cách an toàn giữa các lần nhỏ. |
| **UC-007.4** | Xóa Thuốc Mẫu Khỏi Template (Delete)| `SCR-DOC-08` (Tab 2: Thuốc mẫu) | `TemplateMedication` | `template_medications` | **D**: Gỡ bỏ loại thuốc mẫu không còn phù hợp khỏi phác đồ nháp đang xây dựng. |
| **UC-008.1** | Thêm Mốc & Câu Hỏi Recovery Check (Create)| `SCR-DOC-08` (Tab 3: Khảo sát) | `TemplateRecoveryMilestone`<br>`TemplateRecoveryQuestion` | `template_recovery_milestones`<br>`template_recovery_questions` | **C**: Thiết lập mốc theo dõi định kỳ (Ngày 1, 3, 7, 14, 30) kèm bộ 3–5 câu hỏi phân loại 3 màu Xanh/Vàng/Đỏ (BR24). |
| **UC-008.2** | Xem Danh Sách Mốc & Câu Hỏi (Read/List) | `SCR-DOC-08` (Tab 3: Khảo sát) | `TemplateRecoveryMilestone`<br>`TemplateRecoveryQuestion` | `template_recovery_milestones`<br>`template_recovery_questions` | **R**: Truy xuất danh sách các mốc khảo sát và ma trận câu hỏi đánh giá phục hồi thị lực. |
| **UC-008.3** | Chỉnh Sửa Mốc & Câu Hỏi (Update) | `SCR-DOC-08` (Tab 3: Khảo sát) | `TemplateRecoveryMilestone`<br>`TemplateRecoveryQuestion` | `template_recovery_milestones`<br>`template_recovery_questions` | **U**: Cập nhật văn phong câu hỏi, đổi ngày áp dụng hoặc tinh chỉnh mức độ phân loại màu cảnh báo. |
| **UC-008.4** | Xóa Mốc / Câu Hỏi Recovery Check (Delete)| `SCR-DOC-08` (Tab 3: Khảo sát) | `TemplateRecoveryMilestone`<br>`TemplateRecoveryQuestion` | `template_recovery_milestones`<br>`template_recovery_questions` | **D**: Gỡ bỏ mốc khảo sát hoặc câu hỏi thừa khỏi cấu hình template nháp. |
| **UC-008.5** | Thêm Tiêu Chí Red Flag & Hotline (Create)| `SCR-DOC-08` (Tab 3: Red Flag) | `TemplateRedFlag` | `template_red_flags` | **C**: Cài đặt dấu hiệu báo động đỏ biến chứng khẩn cấp gắn cố định Hotline VISI 0395 151 151 và SLA <5p (BR11, BR12). |
| **UC-008.6** | Xem Danh Sách Tiêu Chí Red Flag (Read)| `SCR-DOC-08` (Tab 3: Red Flag) | `TemplateRedFlag` | `template_red_flags` | **R**: Hiển thị bảng danh mục các dấu hiệu nguy hiểm kèm hướng dẫn sơ cứu tức thì cho người nhà. |
| **UC-008.7** | Chỉnh Sửa Tiêu Chí Red Flag (Update)| `SCR-DOC-08` (Tab 3: Red Flag) | `TemplateRedFlag` | `template_red_flags` | **U**: Hiệu chỉnh mô tả triệu chứng hoặc bổ sung lời dặn xử trí cấp cứu tức thì. |
| **UC-008.8** | Xóa Tiêu Chí Red Flag Khỏi Template (Delete)| `SCR-DOC-08` (Tab 3: Red Flag) | `TemplateRedFlag` | `template_red_flags` | **D**: Xóa tiêu chí Red Flag khỏi template draft; kiểm tra ràng buộc bắt buộc giữ lại ≥1 tiêu chí (BR11). |
| **UC-009.1** | Thêm Cẩm Nang 24h & Infographic (Create)| `SCR-DOC-08` (Tab 4: Cẩm nang) | `TemplateLearningModule` | `template_learning_modules` | **C**: Thêm bài học đồ họa tĩnh, đánh dấu cẩm nang 24h sống còn và tải ảnh Infographic chuẩn tối ưu CDN. |
| **UC-009.2** | Xem Danh Mục Cẩm Nang (Read/List) | `SCR-DOC-08` (Tab 4: Cẩm nang) | `TemplateLearningModule` | `template_learning_modules` | **R**: Hiển thị danh mục bài học Infographic tĩnh theo thứ tự lộ trình chăm sóc phục hồi. |
| **UC-009.3** | Chỉnh Sửa Bài Học Cẩm Nang (Update) | `SCR-DOC-08` (Tab 4: Cẩm nang) | `TemplateLearningModule` | `template_learning_modules` | **U**: Cập nhật tiêu đề, thay đổi ảnh Infographic hoặc hiệu chỉnh tóm tắt y khoa 3 gạch đầu dòng. |
| **UC-009.4** | Xóa Bài Học Khỏi Lộ Trình (Delete) | `SCR-DOC-08` (Tab 4: Cẩm nang) | `TemplateLearningModule` | `template_learning_modules` | **D**: Gỡ bỏ bài học khỏi template nháp và sắp xếp lại thứ tự bài học còn lại. |
| **UC-009.5** | Thêm Quy Tắc Do/Don't 2 Cột Màu (Create)| `SCR-DOC-08` (Tab 4: Do/Don't) | `TemplateDoDontItem` | `template_do_dont_items` | **C**: Thêm quy tắc sinh hoạt vào Cột Xanh (Nên làm - DO) hoặc Cột Đỏ (Cần tránh - DON'T) phân theo nhóm (BR16). |
| **UC-009.6** | Xem Danh Sách Quy Tắc Do/Don't (Read)| `SCR-DOC-08` (Tab 4: Do/Don't) | `TemplateDoDontItem` | `template_do_dont_items` | **R**: Hiển thị bảng ma trận 2 cột màu trực quan phân theo các nhóm sinh hoạt: Ăn uống, Vệ sinh, Vận động. |
| **UC-009.7** | Chỉnh Sửa Quy Tắc Do/Don't (Update) | `SCR-DOC-08` (Tab 4: Do/Don't) | `TemplateDoDontItem` | `template_do_dont_items` | **U**: Cập nhật mô tả quy tắc hoặc bổ sung giải thích lý do y khoa ngăn biến chứng lệch vạt giác mạc. |
| **UC-009.8** | Xóa Quy Tắc Do/Don't Khỏi Template (Delete)| `SCR-DOC-08` (Tab 4: Do/Don't) | `TemplateDoDontItem` | `template_do_dont_items` | **D**: Gỡ bỏ một quy tắc sinh hoạt không còn áp dụng khỏi phác đồ mẫu. |
| **UC-009.9** | Quản Lý Ngân Hàng FAQ Lâm Sàng (All CRUD)| `SCR-DOC-08` (Tab 4: FAQ) | `TemplateDoDontItem` (FAQ Meta)| `template_do_dont_items` | **C, R, U, D**: Quản trị kho tình huống hỏi đáp khẩn cấp (dính nước, quên nhỏ thuốc) hiển thị nhanh tại SCR-CG-07. |
| **UC-010** | Khởi Tạo & Cá Nhân Hóa Care Plan BN | `SCR-DOC-10`: Khởi tạo Care Plan 3 bước | `PatientCarePlan`<br>`PatientMedicationSchedule`<br>`PatientAppointment` | `patient_care_plans`<br>`patient_medication_schedules`<br>`patient_appointments` | **C**: Nhân bản Master Template thành Care Plan thực tế trong <30s; hỗ trợ Bác sĩ tùy biến liều lượng độc lập không đổi template gốc (BR18). |
| **UC-010b** | Thực Hiện Bàn Giao Phòng Lưu Viện | `SCR-DOC-11`: Phiếu xuất viện QR | `PatientCarePlan`<br>`PatientProfile` | `patient_care_plans`<br>`patients` | **R, U**: Điều dưỡng dán khiên mắt bảo hộ, trao phiếu xuất viện kèm mã QR và trực tiếp hướng dẫn Caregiver quét mã trước khi rời viện. |
| **UC-010c** | Xác Nhận Hoàn Tất Bàn Giao Lâm Sàng | `SCR-DOC-03`, `SCR-DOC-10` | `CaregiverPatientLink`<br>`PatientCarePlan` | `caregiver_patient_links`<br>`patient_care_plans` | **U**: Hệ thống ghi nhận trạng thái Care Plan chuyển sang `ACTIVE` toàn diện, Caregiver đã liên kết; đẩy hồ sơ lên Dashboard CSKH (F-018). |
| **UC-011** | Tạo & Phát Hành Mã QR Xuất Viện | `SCR-DOC-10`, `SCR-DOC-11` | `PatientCarePlan` | `patient_care_plans` | **C, U**: Sinh chuỗi token ngẫu nhiên mã hóa an toàn (UUIDv4 + HMAC-SHA256), gán trạng thái `ISSUED` gắn liền với `plan_id`. |
| **UC-012** | In Phiếu Hướng Dẫn Xuất Viện Kèm QR | `SCR-DOC-11`: In phiếu xuất viện | `PatientCarePlan`<br>`PatientProfile`<br>`DoctorProfile` | `patient_care_plans`<br>`patients`<br>`doctor_profiles` | **R**: Xuất lệnh in trực tiếp 1 chạm ra máy in tại quầy lưu viện khổ A5/A4 chứa mã QR sắc nét (≥3x3 cm) và Hotline VISI 0395 151 151. |
| **UC-013** | Cấp Lại hoặc Thu Hồi Mã QR Bàn Giao | `SCR-DOC-11`: Cấp lại mã QR | `PatientCarePlan`<br>`SystemAuditLog` | `patient_care_plans`<br>`system_audit_logs` | **U, C**: Chuyển mã QR cũ sang `REVOKED`, tự động sinh mã QR mới, in lại phiếu xuất viện và ghi nhật ký kiểm toán. |
| **UC-014** | Xem Lịch Dùng Thuốc & Hướng Dẫn Nhỏ | `SCR-CG-04`, `SCR-CG-09`: Lịch dùng thuốc | `PatientMedicationSchedule` | `patient_medication_schedules` | **R**: Hiển thị cữ thuốc 4 khung giờ Sáng/Trưa/Chiều/Tối, ảnh nhận diện vỏ lọ, số giọt, mắt mổ và video kỹ thuật kéo mi dưới không chạm đầu lọ. |
| **UC-015** | Xác Nhận Thuốc & Đếm Lùi Giãn Cách 5-10p | `SCR-CG-09`: Xác nhận cữ thuốc | `PatientMedicationLog`<br>`PatientMedicationSchedule` | `patient_medication_logs`<br>`patient_medication_schedules` | **C, U**: Bấm đã nhỏ thuốc lưu timestamp; tự động kích hoạt timer đếm lùi 5–10 phút tạm khóa lọ thứ hai để chống rửa trôi thuốc; chuông/rung báo hết giờ. |
| **UC-016** | Xem Cẩm Nang 24h & Bảng Do/Don't | `SCR-CG-04`, `SCR-CG-08`: Nên làm & Cần tránh | `TemplateDoDontItem`<br>`CarePlanTemplate` | `template_do_dont_items`<br>`care_plan_templates` | **R**: Tra cứu cẩm nang 24h sống còn và bảng 2 cột màu (Xanh: Nên làm / Đỏ: Cần tránh) hỗ trợ lọc theo nhóm sinh hoạt. |
| **UC-017** | Xem Infographic Học Viện Caregiver | `SCR-CG-05`, `SCR-CG-06`, `SCR-CG-07` | `TemplateLearningModule` | `template_learning_modules` | **R**: Đọc các Infographic đồ họa tĩnh tinh gọn giải thích tiến trình hồi phục mắt; lưu vết hoàn thành bài học (không bắt buộc làm quiz trong MVP). |
| **UC-018** | Xem Lịch Tái Khám 5 Mốc & Nhắc Hẹn | `SCR-CG-10`: Lịch hẹn tái khám | `PatientAppointment`<br>`CaregiverNotificationLog` | `patient_appointments`<br>`caregiver_notification_logs` | **R, U**: Theo dõi 5 mốc tái khám chuẩn VISI; gửi thông báo tự động trước 24 giờ; Caregiver bấm xác nhận sẽ đến khám (`CONFIRMED`). |
| **UC-019** | Thực Hiện Khảo Sát Recovery Check | `SCR-CG-11`: Bảng kiểm phục hồi | `RecoveryCheckSubmission`<br>`PatientCarePlan` | `recovery_check_submissions`<br>`patient_care_plans` | **C, U**: Nộp 3–5 câu hỏi mỗi sáng; tự động đối chiếu phân loại 3 mức: Xanh (Bình thường), Vàng (Chú ý), Đỏ (Nguy hiểm Red Flag). |
| **UC-020** | Kích Hoạt Cấp Cứu Red Flag Khẩn Cấp | `SCR-CG-12`: Cảnh báo Đỏ khẩn cấp | `EmergencyAlertEvent`<br>`PatientCarePlan` | `emergency_alert_events`<br>`patient_care_plans` | **C, U**: Chuyển giao diện đỏ toàn màn hình; cung cấp nút gọi 1 chạm Hotline VISI 0395 151 151; bắn tín hiệu báo động khẩn lên Dashboard viện. |
| **UC-021** | Giám Sát Dashboard Phục Hồi Tập Trung | `SCR-DOC-02`, `SCR-DOC-12`: Dashboard cơ sở | `PatientCarePlan`<br>`PatientProfile`<br>`PatientMedicationLog`<br>`RecoveryCheckSubmission` | `patient_care_plans`<br>`patients`<br>`patient_medication_logs`<br>`recovery_check_submissions` | **R**: Kết xuất danh sách bệnh nhân xuất viện theo 3 tầng trạng thái (Đỏ, Vàng, Xanh); theo dõi tỷ lệ tuân thủ thuốc và nộp recovery check. |
| **UC-022** | Tiếp Nhận & Xử Lý Cảnh Báo Red Flag | `SCR-DOC-12`: Xử lý cảnh báo khẩn | `EmergencyAlertEvent`<br>`PatientCarePlan` | `emergency_alert_events`<br>`patient_care_plans` | **U**: Bật chuông báo động, ghim ca lên đầu; CSKH bấm nhận ca, dừng chuông và gọi điện thoại can thiệp lâm sàng trong SLA <5 phút. |
| **UC-022b** | Tự Động Leo Thang Cảnh Báo Quá Hạn | `SCR-DOC-12` (Bộ giám sát Daemon hệ thống) | `EmergencyAlertEvent`<br>`SystemAuditLog` | `emergency_alert_events`<br>`system_audit_logs` | **U**: Tự động phát chuông cấp 2 và gửi tin nhắn SMS khẩn cấp tới Bác sĩ trực cơ sở nếu ca Red Flag chưa được xử lý sau 15 phút. |
| **UC-023** | Ghi Nhận Nhật Ký Cuộc Gọi Can Thiệp | `SCR-DOC-12`: Popup ghi nhận cuộc gọi | `CallInterventionLog`<br>`EmergencyAlertEvent` | `call_intervention_logs`<br>`emergency_alert_events` | **C, U**: CSKH nhập kết quả cuộc gọi tư vấn, lời dặn y tế; chuyển ca bệnh sang `RESOLVED` hoặc chuyển Bác sĩ chuyên khoa. |
| **UC-024** | Tra Cứu FAQ Tình Huống Khẩn Cấp | `SCR-CG-07`: Hỏi đáp & FAQ tình huống | `TemplateDoDontItem` (Metadata FAQ) | `template_do_dont_items` | **R**: Tra cứu giải đáp y khoa chuẩn VISI cho các tình huống tại nhà: dính nước, quên nhỏ thuốc, cộm ngứa mắt. |
| **UC-025** | Kích Hoạt Chế Độ Trợ Năng Nhãn Khoa | `SCR-CG-01` đến `SCR-CG-12` (Toàn bộ UI) | `CaregiverProfile` (Cấu hình hiển thị) | `caregiver_profiles` | **U, R**: Phóng to chữ (≥18pt), tăng tương phản High Contrast nền đen chữ vàng, nút bấm lớn ≥48px và bật Audio Guide đọc tiếng Việt. |
| **UC-026.1** | Khởi Tạo Tài Khoản Nhân Viên & Cơ Sở (Create)| `SCR-DOC-01` (Module Quản trị Nhân sự): Tạo tài khoản | `UserAccount`<br>`DoctorProfile` | `accounts`<br>`doctor_profiles` | **C**: Tạo tài khoản nhân sự mới, gán vai trò RBAC (`DOCTOR`, `NURSE`, `CSKH`, `GCMO`) và gán cơ sở (BR26). |
| **UC-026.2** | Xem Danh Sách Nhân Viên Theo Cơ Sở (Read)| `SCR-DOC-01` (Module Quản trị Nhân sự): Danh bạ nhân sự | `UserAccount`<br>`DoctorProfile` | `accounts`<br>`doctor_profiles` | **R**: Tra cứu, lọc danh bạ nhân sự y tế phân trang theo 5 chi nhánh VISI và trạng thái hoạt động. |
| **UC-026.3** | Chỉnh Sửa Thông Tin & Vai Trò RBAC (Update)| `SCR-DOC-01` (Module Quản trị Nhân sự): Sửa nhân viên | `UserAccount`<br>`DoctorProfile` | `accounts`<br>`doctor_profiles` | **U**: Điều chuyển chi nhánh cơ sở, cập nhật quyền hạn chuyên môn; kích hoạt cơ chế thu hồi phiên làm việc cũ. |
| **UC-026.4** | Khóa / Vô Hiệu Hóa Tài Khoản (Deactivate)| `SCR-DOC-01` (Module Quản trị Nhân sự): Khóa tài khoản | `UserAccount` | `accounts` | **D**: Khóa tài khoản (`status = 'LOCKED'`), ngắt phiên làm việc tức thì trên mọi thiết bị khi nhân viên nghỉ việc. |
| **UC-027** | Tra Cứu Nhật Ký Kiểm Toán Hệ Thống | `SCR-DOC-01` (Module Quản trị Nhân sự / Audit Log) | `SystemAuditLog` | `system_audit_logs` | **R**: Truy vấn nhật ký thao tác bất biến (IP, timestamp, hành vi lâm sàng, diff dữ liệu) phục vụ thanh tra y tế và an toàn thông tin. |
| **UC-028** | Kết Xuất Báo Cáo Vận Hành & KPI | `SCR-DOC-02`: Báo cáo quản trị | Tổng hợp từ `patient_care_plans`<br>`patient_medication_logs`<br>`patient_appointments` | `patient_care_plans`<br>`patient_medication_logs`<br>`patient_appointments` | **R**: Tổng hợp các chỉ số KPIs: tỷ lệ kích hoạt QR (mục tiêu ≥85%), tỷ lệ tuân thủ thuốc đúng giờ, tỷ lệ tái khám theo từng chi nhánh. |

---

## 3. PHÂN TÍCH ĐỘ BAO PHỦ & ĐÁNH GIÁ CHẤT LƯỢNG (COVERAGE ANALYSIS)

### 3.1 Độ bao phủ màn hình giao diện (UI Coverage: 100%)

* **Phân hệ Caregiver Mobile PWA:** 12/12 màn hình (`SCR-CG-01` đến `SCR-CG-12`) được ánh xạ đầy đủ với các Use Case từ `UC-001` đến `UC-020`, `UC-024`, `UC-025`.
* **Phân hệ Hospital Clinical Portal:** 12/12 màn hình (`SCR-DOC-01` đến `SCR-DOC-12`) được ánh xạ chính xác với các Use Case từ `UC-002` đến `UC-013`, `UC-021`, `UC-022`, `UC-023`, `UC-026`, `UC-027`, `UC-028`.
* **Zero Orphan Screens:** 100% màn hình đều gắn liền với mục tiêu nghiệp vụ rõ ràng.

### 3.2 Độ bao phủ cơ sở dữ liệu (Database Coverage: 100%)

* **20 Thực thể Logic & 24 Bảng CSDL Vật lý** đều được truy xuất (Create/Read/Update/Delete) qua các thao tác nghiệp vụ.
* Các bảng liên quan đến Quiz (`template_quiz_questions`, `caregiver_quiz_submissions`) được bảo lưu ở trạng thái mở rộng Phase 2 theo F-014.

### 3.3 Bảng Tổng Hợp Ràng Buộc Tính Toàn Vẹn Dữ Liệu (Integrity Constraints Summary)

1. **Ràng buộc Caregiver - Bệnh nhân (BR5):** Tối đa 3 Caregiver cùng liên kết vào 1 `PatientCarePlan`.
2. **Ràng buộc Bộ đếm thời gian đệm (BR23):** Khóa cữ thuốc thứ hai từ 5–10 phút để chống rửa trôi thuốc mắt.
3. **Ràng buộc Cam kết SLA Red Flag (BR12):** Bắt buộc CSKH can thiệp <5 phút; tự động leo thang sau 15 phút.
4. **Ràng buộc Xóa mềm (BR25):** Không xóa vật lý bản ghi bệnh nhân đang có Care Plan hoạt động; chỉ chuyển sang `ARCHIVED`.