# 05. MA TRẬN ÁNH XẠ NGHIỆP VỤ (SCREEN - ENTITY - DATABASE MAPPING)
# MA TRẬN TRUY XUẤT 4 TẦNG TOÀN DIỆN — VISI MEDICAL GROUP

> **Dự án:** RemiCare Ophthalmic Post-Op Platform (Nền tảng Hướng dẫn và Giám sát Chăm sóc Hậu phẫu Nhãn khoa)  
> **Doanh nghiệp mục tiêu:** Công ty Cổ phần Tập đoàn Y khoa VISI (VISI Medical Group)  
> **Phiên bản:** V1 (Đồng bộ toàn diện theo tài liệu chuẩn US_US_V1 - Ánh xạ 100% 31 Use Cases UC-001 đến UC-028)  
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
| **UC-002** | Đăng nhập Nhân viên Y tế & 2FA | `SCR-DOC-01`: Đăng nhập Nhân viên Y tế | `UserAccount`<br>`DoctorProfile` | `accounts`<br>`doctor_profiles` | **R**: Xác thực tài khoản nội bộ và mã 2FA; phân quyền vai trò RBAC (`DOCTOR`, `NURSE`, `CSKH`) và giới hạn phạm vi cơ sở bệnh viện (`facility_id`). |
| **UC-003** | Quét QR Bàn Giao Liên Kết Hồ Sơ | `SCR-CG-03`: Quét QR liên kết BN | `CaregiverPatientLink`<br>`PatientCarePlan`<br>`PatientProfile` | `caregiver_patient_links`<br>`patient_care_plans`<br>`patients` | **C, R**: Giải mã QR token, kiểm tra tính hợp lệ; kiểm tra giới hạn ≤3 Caregiver; lưu bản ghi liên kết Caregiver - Bệnh nhân. |
| **UC-004** | Quản lý Hồ sơ Định danh Bệnh Nhân | `SCR-DOC-03`, `SCR-DOC-04`, `SCR-DOC-05`, `SCR-DOC-06` | `PatientProfile` | `patients` | **C, R, U**: Nhập hồ sơ bệnh nhân mới (<30s); lưu trữ tên viết tắt bảo mật theo NĐ 13/2023; cập nhật thông tin; chuyển lưu trữ `ARCHIVED` (xóa mềm). |
| **UC-005** | Quản lý Danh mục Master Template | `SCR-DOC-07`: Danh mục Master Template<br>`SCR-DOC-08`: Không gian cấu hình | `CarePlanTemplate` | `care_plan_templates` | **C, R, U**: Khởi tạo và quản lý phiên bản (v1.0, v1.1) các gói phác đồ chuẩn hóa theo loại phẫu thuật (Phaco, SILK); quản lý trạng thái vòng đời. |
| **UC-006** | Phê Duyệt Lâm Sàng Master Template | `SCR-DOC-09`: Thẩm định & Vòng đời Template | `CarePlanTemplate`<br>`SystemAuditLog` | `care_plan_templates`<br>`system_audit_logs` | **U, C**: Giám đốc Chuyên môn (BS.CKII Trần Bá Kiền) ký duyệt điện tử, chuyển trạng thái template sang `ACTIVE` ban hành toàn chuỗi; ghi vết kiểm toán. |
| **UC-007** | Cấu Hình Thuốc Mẫu & Bộ Đếm Giãn Cách | `SCR-DOC-08` (Tab 2: Danh mục thuốc) | `TemplateMedication` | `template_medications` | **C, R, U, D**: Thiết lập danh mục biệt dược mẫu, liều lượng, cữ dùng, tải ảnh vỏ lọ và cấu hình thời gian giãn cách đệm mặc định 5–10 phút. |
| **UC-008** | Cấu Hình Recovery Check & Red Flag | `SCR-DOC-08` (Tab 3 & Tab 4: Khảo sát & Báo động) | `TemplateRecoveryMilestone`<br>`TemplateRecoveryQuestion`<br>`TemplateRedFlag` | `template_recovery_milestones`<br>`template_recovery_questions`<br>`template_red_flags` | **C, R, U, D**: Cài đặt các mốc theo dõi, 3–5 câu hỏi khảo sát phân loại 3 mức Xanh/Vàng/Đỏ và các tiêu chí nguy cấp kích hoạt Hotline 0395 151 151. |
| **UC-009** | Cấu Hình Cẩm Nang 24h & Do/Don't | `SCR-DOC-08` (Tab 1 & Tab 5: Cẩm nang & Quy tắc) | `TemplateLearningModule`<br>`TemplateDoDontItem` | `template_learning_modules`<br>`template_do_dont_items` | **C, R, U, D**: Thiết lập cẩm nang 24h đầu, bảng quy tắc Nên làm / Cần tránh 2 cột màu phân loại theo sinh hoạt và ngân hàng tình huống FAQ lâm sàng. |
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
| **UC-026** | Quản Lý Nhân Viên & Phân Quyền Cơ Sở | `SCR-DOC-01` (Admin Portal) | `UserAccount`<br>`DoctorProfile` | `accounts`<br>`doctor_profiles` | **C, R, U, D**: Khởi tạo tài khoản nhân viên y tế, gán vai trò RBAC và gán chi nhánh bệnh viện trực thuộc (5 cơ sở VISI). |
| **UC-027** | Tra Cứu Nhật Ký Kiểm Toán Hệ Thống | `SCR-DOC-01` (Module Audit Log) | `SystemAuditLog` | `system_audit_logs` | **R**: Truy vấn nhật ký thao tác bất biến (IP, timestamp, hành vi lâm sàng, diff dữ liệu) phục vụ thanh tra y tế và an toàn thông tin. |
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