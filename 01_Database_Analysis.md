# 01. Phân Tích Mô Hình Dữ Liệu (Database Analysis)

> **Dự án:** Hệ Thống Hướng Dẫn & Theo Dõi Chăm Sóc Bệnh Nhân Hậu Phẫu Mắt (Post-Op Eye Care Platform)  
> **Tài liệu tham chiếu:** [file_use_case_spec.md](file:///d:/DOC_BA/docs_of_projects/file_use_case_spec.md) | [file_user_story.md](file:///d:/DOC_BA/docs_of_projects/file_user_story.md)  
> **Người thực hiện:** Đội ngũ BA  
> **Ngày lập:** 11/09/2026 | **Trạng thái:** Bản chuẩn hóa phân tích (Baseline)

---

## 1. Tổng Quan & Mục Tiêu Phân Tích

Tài liệu này thực hiện trích xuất, phân tích và mô hình hóa toàn bộ các đối tượng dữ liệu (Entities), thuộc tính (Attributes), mối quan hệ (Relationships) và quy tắc nghiệp vụ dữ liệu xuất phát trực tiếp từ tài liệu **Use Case Specification** của hệ thống chăm sóc hậu phẫu.

### Nguyên tắc Phân tích Dữ liệu:
1. **Không over-design:** Chỉ thiết kế các thực thể và thuộc tính có căn cứ nghiệp vụ từ Use Case Spec và User Story.
2. **Độc lập giữa Template và Instance (BR17, BR18):** Phân tách tuyệt đối giữa cấu hình mẫu chuẩn (*Master Care Plan Template*) và kế hoạch chăm sóc áp dụng cho từng bệnh nhân cụ thể (*Patient Care Plan Instance*).
3. **Linh hoạt theo loại phẫu thuật:** Sử dụng cơ chế lưu trữ dữ liệu lâm sàng tùy biến theo ca mổ (Phaco, Lác, LASIK/ICL, Cắt dịch kính...) mà không làm phá vỡ cấu trúc quan hệ chuẩn.
4. **Định hướng mở rộng cho Admin tương lai `[FUTURE / ADMIN]`:** Tích hợp bảng tài khoản thống nhất và nhật ký kiểm toán (*Audit Log*) sẵn sàng cho 2 nhóm tính năng CRUD Account & CRUD Audit Log của Admin trong tương lai mà không làm ảnh hưởng đến các Use Case hiện tại.

---

## 2. Danh Mục Các Thực Thể Nghiệp Vụ (Entity Inventory)

Dựa trên phân tích 21 nhóm Use Case, hệ thống được cấu trúc thành **6 nhóm thực thể cốt lõi**:

| STT | Nhóm Thực Thể | Tên Thực Thể (Entity) | Diễn Giải Nghiệp Vụ | Căn Cứ Use Case |
|---|---|---|---|---|
| **I** | **Người dùng & Xác thực** | `Facility` | Quản lý hệ sinh thái 5 cơ sở/chi nhánh y tế VISI (BR26) | UC-002, UC-026, BR26 |
| | | `UserAccount` | Tài khoản người dùng tập trung (Caregiver, Care Recipient, Doctor, Nurse, CSKH, GCMO, Admin) | UC-001, UC-002, UC-026 |
| | | `DoctorProfile` | Thông tin định danh chuyên môn Bác sĩ/Điều dưỡng/CSKH theo chi nhánh | UC-002, UC-004, UC-005, UC-026 |
| | | `CaregiverProfile` | Thông tin người chăm sóc bệnh nhân | UC-001, UC-003, UC-025 |
| | | `OtpVerification` | Mã OTP và phiên xác thực đăng nhập số điện thoại | UC-001 |
| **II** | **Hồ sơ Bệnh nhân & Liên kết** | `PatientProfile` | Hồ sơ hành chính và đặc điểm lâm sàng tối thiểu của bệnh nhân (NĐ 13) | UC-004 |
| | | `CaregiverPatientLink` | Quan hệ ủy quyền liên kết Caregiver - Bệnh nhân qua mã QR (tối đa 3, phân vai Chính/Phụ) | UC-003, UC-004, UC-010c |
| **III** | **Master Care Plan Template** | `CarePlanTemplate` | Gói phác đồ chăm sóc mẫu gắn với loại phẫu thuật (Phaco, SILK), quản lý phiên bản và duyệt chuyên môn | UC-005, UC-006 |
| | | `TemplateLearningModule` | Bài học hướng dẫn & Infographic trong lộ trình mẫu | UC-009, UC-017 |
| | | `TemplateQuizQuestion` | Câu hỏi trắc nghiệm Mini Quiz (Mở rộng Phase 2 theo F-014) | UC-017 (Phase 2) |
| | | `TemplateMedication` | Thuốc mẫu kèm nhận diện trực quan và Drop Interval Timer 5–10p | UC-007 |
| | | `TemplateRecoveryMilestone` | Mốc thời gian khảo sát phục hồi chuẩn (Day 1..7, 14, 30) | UC-008 |
| | | `TemplateRecoveryQuestion` | Câu hỏi khảo sát triệu chứng (3–5 câu) và tiêu chí 3 mức Xanh/Vàng/Đỏ | UC-008 |
| | | `TemplateRedFlag` | Tiêu chí dấu hiệu nguy hiểm khẩn cấp và Hotline 0395 151 151 | UC-008, UC-020 |
| | | `TemplateDoDontItem` | Hướng dẫn hành vi Nên làm / Cần tránh 2 cột màu trong sinh hoạt | UC-009, UC-016 |
| **IV** | **Kế Hoạch Chăm Sóc Bệnh Nhân** | `PatientCarePlan` | Bản sao thực tế được kích hoạt và cá nhân hóa cho từng bệnh nhân (<30s) theo chi nhánh | UC-010, UC-010b, UC-010c |
| | | `PatientMedication` | Đơn thuốc thực tế được Bác sĩ tùy biến liều lượng cho bệnh nhân | UC-010, UC-014, UC-015 |
| | | `PatientFollowupAppointment` | Lịch hẹn tái khám thực tế 5 mốc chuẩn VISI của bệnh nhân | UC-010, UC-018 |
| | | `PatientQRCode` | Mã QR token bảo mật gắn với Care Plan của bệnh nhân (Bác sĩ/Điều dưỡng cấp) | UC-003, UC-011, UC-012, UC-013 |
| **V** | **Giao Dịch & Sự Kiện Lâm Sàng** | `MedicationLog` | Nhật ký Caregiver xác nhận uống/nhỏ thuốc theo cữ (kèm timestamp) | UC-015 |
| | | `CaregiverQuizSubmission` | Lịch sử làm bài trắc nghiệm Mini Quiz của Caregiver (Phase 2) | UC-017 (Phase 2) |
| | | `RecoveryCheckSubmission` | Lượt nộp bảng kiểm phục hồi định kỳ của Caregiver | UC-019, UC-021 |
| | | `RecoveryCheckAnswer` | Chi tiết câu trả lời cho từng câu hỏi phục hồi 3 mức | UC-019, UC-021 |
| | | `RedFlagIncident` | Sự kiện kích hoạt cảnh báo khẩn cấp hoặc gọi Hotline 0395 151 151, hỗ trợ leo thang tự động | UC-020, UC-022, UC-022b |
| | | `CallInterventionLog` | Nhật ký can thiệp cuộc gọi CSKH/Điều dưỡng xử lý cảnh báo y tế (SLA <5p) | UC-023, BR12 |
| **VI** | **Mở Rộng Quản Trị, Giao Tiếp & Kiểm Toán** | `Notification` | Nhật ký gửi thông báo đa kênh (SMS, ZNS, Push, In-App) cho bệnh nhân & người chăm sóc | UC-018, UC-022b, BR24 |
| | | `AuditLog` | Nhật ký kiểm toán bất biến các thao tác dữ liệu trọng yếu (Audit Trail) | UC-027, F-024, BR21 |

---

## 3. Phân Tích Chi Tiết Thuộc Tính & Khóa (Attributes, PK, FK)

### 3.1 Phân Hệ Người Dùng & Định Danh

#### 0. Thực thể `Facility` (`facilities`)
* **Mục đích:** Quản lý thông tin định danh 5 cơ sở/chi nhánh bệnh viện trực thuộc Tập đoàn Y khoa VISI, làm nền tảng kiểm soát dữ liệu đa chi nhánh (BR26).
* **Thuộc tính:**
  * `facility_id` (PK, UUID): Định danh duy nhất của chi nhánh/bệnh viện.
  * `facility_code` (VARCHAR(20), Unique, Not Null): Mã viết tắt cơ sở (ví dụ: `VISI-HN`, `VISI-DN`, `VISI-HCM-Q1`, `VISI-HCM-Q7`, `VISI-CT`).
  * `facility_name` (VARCHAR(150), Not Null): Tên cơ sở y tế đầy đủ (ví dụ: *Bệnh viện Mắt Quốc tế VISI Hà Nội*).
  * `address` (TEXT, Not Null): Địa chỉ thực tế của bệnh viện/phòng khám.
  * `hotline` (VARCHAR(20), Not Null, Default: `'0395 151 151'`): Số hotline cấp cứu 24/7 của cơ sở.
  * `status` (ENUM: `'ACTIVE'`, `'INACTIVE'`, Default: `'ACTIVE'`): Trạng thái hoạt động của cơ sở.
  * `created_at`, `updated_at` (TIMESTAMP): Dấu thời gian hệ thống.

#### 1. Thực thể `UserAccount` (`accounts`)
* **Mục đích:** Quản lý danh tính đăng nhập tập trung cho toàn bộ hệ thống (Caregiver, Care Recipient, Bác sĩ, Điều dưỡng, CSKH, GCMO, Admin).
* **Thuộc tính:**
  * `account_id` (PK, UUID): Định danh tài khoản duy nhất.
  * `facility_id` (FK -> `facilities.facility_id`, Nullable): Cơ sở y tế trực thuộc (Bắt buộc với nhân viên chi nhánh; NULL với GCMO cấp tập đoàn và Caregiver/Bệnh nhân) (BR26).
  * `phone_number` (VARCHAR(20), Unique, Nullable): Số điện thoại dùng đăng nhập OTP (Caregiver) hoặc liên hệ (Doctor/Staff).
  * `username` (VARCHAR(50), Unique, Nullable): Tên đăng nhập (Bác sĩ, Điều dưỡng, CSKH, GCMO, Admin).
  * `email` (VARCHAR(100), Unique, Nullable): Hòm thư công vụ.
  * `password_hash` (VARCHAR(255), Nullable): Mật khẩu băm an toàn (BCrypt/Argon2) cho nhân viên y tế và quản trị viên.
  * `role` (ENUM: `'CAREGIVER'`, `'CARE_RECIPIENT'`, `'DOCTOR'`, `'NURSE'`, `'CSKH'`, `'GCMO'`, `'ADMIN'`): Vai trò tài khoản theo phân quyền RBAC 7 cấp.
  * `status` (ENUM: `'ACTIVE'`, `'LOCKED'`, `'INACTIVE'`): Trạng thái tài khoản (BR3, BR14).
  * `created_at`, `updated_at` (TIMESTAMP): Dấu thời gian hệ thống.

#### 2. Thực thể `DoctorProfile` (`doctor_profiles`)
* **Mục đích:** Lưu trữ hồ sơ hành chính chuyên môn của Bác sĩ / Nhân sự lâm sàng.
* **Thuộc tính:**
  * `doctor_id` (PK, FK -> `accounts.account_id`): Mã bác sĩ / nhân sự y tế.
  * `facility_id` (FK -> `facilities.facility_id`, Not Null): Chi nhánh bệnh viện công tác (BR26).
  * `full_name` (VARCHAR(100), Not Null): Họ và tên Bác sĩ / Nhân sự y tế.
  * `license_number` (VARCHAR(50), Unique): Số chứng chỉ hành nghề y tế.
  * `department` (VARCHAR(100)): Khoa chuyên môn (Khoa Mắt, Khúc xạ, Glaucoma, Hậu phẫu...).
  * `hospital_name` (VARCHAR(150)): Tên bệnh viện / cơ sở y tế trực thuộc.
  * `phone_number` (VARCHAR(20)): Số điện thoại công vụ.

#### 3. Thực thể `CaregiverProfile` (`caregiver_profiles`)
* **Mục đích:** Lưu trữ hồ sơ người chăm sóc.
* **Thuộc tính:**
  * `caregiver_id` (PK, FK -> `accounts.account_id`): Mã người chăm sóc.
  * `full_name` (VARCHAR(100), Nullable): Họ tên người chăm sóc (hoàn thiện sau khi liên kết).
  * `relationship_with_patient` (VARCHAR(50), Nullable): Mối quan hệ với bệnh nhân (Con, Bố mẹ, Vợ/Chồng...).

#### 4. Thực thể `OtpVerification` (`otp_verifications`)
* **Mục đích:** Phục vụ luồng đăng nhập an toàn không mật khẩu của Caregiver (UC-001).
* **Thuộc tính:**
  * `otp_id` (PK, UUID): Định danh phiên OTP.
  * `phone_number` (VARCHAR(20), Not Null): Số điện thoại nhận OTP.
  * `otp_code` (VARCHAR(10), Not Null): Mã OTP sinh ngẫu nhiên.
  * `expired_at` (TIMESTAMP, Not Null): Thời điểm hết hạn (BR2: 5 phút).
  * `is_used` (BOOLEAN, Default: FALSE): Trạng thái đã xác thực hay chưa.
  * `attempt_count` (INT, Default: 0): Số lần nhập sai (chống brute-force).
  * `created_at` (TIMESTAMP): Thời gian gửi mã.

---

### 3.2 Phân Hệ Hồ Sơ Bệnh Nhân & Liên Kết

#### 5. Thực thể `PatientProfile` (`patients`)
* **Mục đích:** Lưu thông tin nhân thân, hành chính và bệnh lý của người bệnh (UC-004, NĐ 13).
* **Thuộc tính:**
  * `patient_id` (PK, VARCHAR(30)): Mã bệnh nhân duy nhất (ví dụ: `BN-202609-001`).
  * `facility_id` (FK -> `facilities.facility_id`, Not Null): Chi nhánh bệnh viện tiếp nhận mổ và điều trị (BR26).
  * `full_name` (VARCHAR(100), Not Null): Họ và tên đầy đủ (BR15).
  * `date_of_birth` (DATE, Not Null): Ngày tháng năm sinh (BR15).
  * `gender` (ENUM: `'MALE'`, `'FEMALE'`, `'OTHER'`, Not Null): Giới tính (BR15).
  * `phone_number` (VARCHAR(20), Nullable): Số điện thoại liên hệ bệnh nhân/người nhà.
  * `address` (TEXT, Nullable): Địa chỉ cư trú.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (Phaco, Lác, LASIK/ICL, Cắt dịch kính...) (BR15).
  * `surgery_date` (DATE, Nullable): Ngày thực hiện phẫu thuật.
  * `operated_eye` (ENUM: `'LEFT'`, `'RIGHT'`, `'BOTH'`, Not Null): Mắt phẫu thuật (Mắt Trái / Phải / Cả hai).
  * `clinical_custom_data` (JSONB, Nullable): **Trường tùy biến động theo ca mổ** (UC-004) lưu các thông số đặc thù: Công suất thể thủy tinh nhân tạo IOL, độ lác trước mổ, khúc xạ trước/sau mổ, đặc điểm vết rạch giác mạc, chỉ tiêu nhãn áp ban đầu...
  * `medical_notes` (TEXT, Nullable): Ghi chú chẩn đoán lâm sàng bổ sung.
  * `status` (ENUM: `'ACTIVE'`, `'ARCHIVED'`, Default: `'ACTIVE'`): Trạng thái hồ sơ (BR25).
  * `created_by_doctor_id` (FK -> `doctor_profiles.doctor_id`, Not Null): Bác sĩ tiếp nhận.
  * `created_at`, `updated_at` (TIMESTAMP): Dấu thời gian kiểm toán.

#### 6. Thực thể `CaregiverPatientLink` (`caregiver_patient_links`)
* **Mục đích:** Lưu trữ quan hệ ủy quyền chăm sóc được xác lập sau khi quét mã QR (UC-003).
* **Thuộc tính:**
  * `link_id` (PK, UUID): Định danh liên kết.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Người chăm sóc.
  * `patient_id` (FK -> `patients.patient_id`, Not Null): Bệnh nhân được chăm sóc.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Not Null): Kế hoạch chăm sóc cụ thể được gán.
  * `role` (ENUM: `'PRIMARY'`, `'SECONDARY'`, Default: `'PRIMARY'`): Phân loại người chăm sóc chính hay phụ (BR5: tối đa 3 Caregiver/bệnh nhân).
  * `linked_at` (TIMESTAMP, Not Null): Thời điểm quét mã xác nhận thành công.
  * `status` (ENUM: `'ACTIVE'`, `'REVOKED'`, Default: `'ACTIVE'`): Trạng thái liên kết (BR5).
  * `revoked_at` (TIMESTAMP, Nullable): Thời điểm hủy quyền nếu có.

---

### 3.3 Phân Hệ Master Care Plan Template (Cấu Hình Mẫu Chuẩn)

#### 7. Thực thể `CarePlanTemplate` (`care_plan_templates`)
* **Mục đích:** Gói cấu hình quy trình mẫu do bác sĩ/bệnh viện thiết lập theo bệnh học (UC-005, UC-006).
* **Thuộc tính:**
  * `template_id` (PK, UUID): Định danh mẫu.
  * `template_name` (VARCHAR(150), Not Null): Tên mẫu (ví dụ: *Phác đồ Chăm sóc Hậu phẫu Phaco Chuẩn Quốc Tế*).
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật liên kết duy nhất (BR16).
  * `clinical_description` (TEXT, Nullable): Mô tả mục tiêu lâm sàng và hướng dẫn tổng quan.
  * `version` (VARCHAR(10), Default: `'1.0'`): Phiên bản của phác đồ mẫu (v1.0, v1.1...) phục vụ copy-on-write khi sửa template đã ban hành (BR15).
  * `parent_template_id` (FK -> `care_plan_templates.template_id`, Nullable): Tham chiếu template gốc khi tạo bản sao phiên bản mới.
  * `status` (ENUM: `'DRAFT'`, `'PENDING_APPROVAL'`, `'ACTIVE'`, `'INACTIVE'`, `'ARCHIVED'`, Default: `'DRAFT'`): Trạng thái vòng đời template (UC-005, UC-006).
  * `created_by_doctor_id` (FK -> `doctor_profiles.doctor_id`, Not Null): Bác sĩ phụ trách cấu hình.
  * `approved_by` (FK -> `accounts.account_id`, Nullable): Bác sĩ Trưởng khoa / GCMO ký duyệt ban hành lâm sàng (UC-006).
  * `approved_at` (TIMESTAMP, Nullable): Thời điểm phê duyệt lâm sàng.
  * `created_at`, `updated_at` (TIMESTAMP): Dấu thời gian.

#### 8. Thực thể `TemplateLearningModule` (`template_learning_modules`)
* **Mục đích:** Bài học hướng dẫn thao tác chăm sóc trong Learning Path của Template (UC-009).
* **Thuộc tính:**
  * `module_id` (PK, UUID): Định danh bài học.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `title` (VARCHAR(200), Not Null): Tiêu đề bài học (ví dụ: *Hướng dẫn nhỏ mắt đúng cách và tránh nhiễm khuẩn*).
  * `category` (VARCHAR(50), Not Null): Phân nhóm chủ đề (24h đầu, Vệ sinh mắt, Dinh dưỡng...).
  * `content_text` (TEXT, Not Null): Nội dung văn bản hướng dẫn chi tiết.
  * `media_type` (ENUM: `'VIDEO'`, `'IMAGE'`, `'INFOGRAPHIC'`, `'TEXT'`, Default: `'VIDEO'`): Định dạng media minh họa.
  * `media_url` (VARCHAR(500), Nullable): Đường dẫn tệp video/hình ảnh.
  * `display_order` (INT, Not Null): Thứ tự bài học trong lộ trình (UC-009).
  * `created_at`, `updated_at` (TIMESTAMP).

#### 9. Thực thể `TemplateQuizQuestion` (`template_quiz_questions`)
* **Mục đích:** 3 câu hỏi trắc nghiệm kiểm tra nhanh kiến thức gắn ở cuối mỗi bài học (UC-017 / Phase 2).
* **Thuộc tính:**
  * `question_id` (PK, UUID): Định danh câu hỏi.
  * `module_id` (FK -> `template_learning_modules.module_id`, Not Null): Gắn với bài học nào.
  * `question_text` (TEXT, Not Null): Nội dung câu hỏi trắc nghiệm.
  * `option_a` (TEXT, Not Null): Phương án A.
  * `option_b` (TEXT, Not Null): Phương án B.
  * `option_c` (TEXT, Not Null): Phương án C.
  * `option_d` (TEXT, Not Null): Phương án D.
  * `correct_option` (ENUM: `'A'`, `'B'`, `'C'`, `'D'`, Not Null): Đáp án đúng y khoa.
  * `clinical_explanation` (TEXT, Not Null): Lời giải thích y khoa ngắn gọn khi trả lời (BR7).
  * `display_order` (INT, Not Null): Thứ tự câu hỏi (1, 2, 3).

#### 10. Thực thể `TemplateMedication` (`template_medications`)
* **Mục đích:** Cấu hình danh mục thuốc mẫu cho loại phẫu thuật (UC-007).
* **Thuộc tính:**
  * `medication_template_id` (PK, UUID): Định danh thuốc mẫu.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (UC-007).
  * `drug_name` (VARCHAR(150), Not Null): Tên biệt dược, hoạt chất và hàm lượng (ví dụ: *Tobradex 5ml*).
  * `drug_form` (ENUM: `'EYE_DROP'`, `'ORAL'`, Not Null): Thuốc nhỏ mắt hay thuốc uống.
  * `dosage` (VARCHAR(50), Not Null): Liều dùng (1 giọt, 1 viên...).
  * `frequency` (VARCHAR(50), Not Null): Tần suất dùng (4 lần/ngày...).
  * `times_of_day` (JSONB / VARCHAR(100), Not Null): Các cữ trong ngày (Sáng, Trưa, Chiều, Tối).
  * `meal_relation` (ENUM: `'BEFORE_MEAL'`, `'AFTER_MEAL'`, `'NONE'`, Default: `'NONE'`): Uống trước/sau ăn.
  * `duration_days` (INT, Not Null): Số ngày dùng dự kiến.
  * `order_index` (INT, Not Null): Thứ tự dùng thuốc trong cữ (quan trọng với thuốc nhỏ mắt).
  * `visual_identification` (TEXT, Not Null): **Mô tả nhận diện về thuốc** (Màu sắc vỏ, nắp lọ, viên nén/nang, độ trong đục dung dịch) (UC-007).
  * `clinical_cautions` (TEXT, Not Null): **Lưu ý về loại thuốc** (Lắc kỹ trước khi dùng, bảo quản ngăn mát tủ lạnh, giãn cách tối thiểu 5 phút...) (BR23, UC-007).
  * `created_at`, `updated_at` (TIMESTAMP).

#### 11. Thực thể `TemplateRecoveryMilestone` (`template_recovery_milestones`)
* **Mục đích:** Thiết lập các mốc thời gian khảo sát phục hồi chuẩn trong template (UC-008).
* **Thuộc tính:**
  * `milestone_template_id` (PK, UUID): Định danh mốc mẫu.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (UC-008).
  * `milestone_name` (VARCHAR(50), Not Null): Tên mốc hiển thị (ví dụ: *Ngày 1*, *Ngày 3*, *Ngày 7*).
  * `days_post_op` (INT, Not Null): Số ngày sau phẫu thuật cần kích hoạt khảo sát (1, 3, 7, 14...).
  * `description` (TEXT, Nullable): Ý nghĩa lâm sàng của mốc theo dõi.

#### 12. Thực thể `TemplateRecoveryQuestion` (`template_recovery_questions`)
* **Mục đích:** Bộ câu hỏi 3–5 câu của từng mốc phục hồi (UC-008).
* **Thuộc tính:**
  * `question_template_id` (PK, UUID): Định danh câu hỏi mẫu.
  * `milestone_template_id` (FK -> `template_recovery_milestones.milestone_template_id`, Not Null): Gắn với mốc nào.
  * `question_text` (TEXT, Not Null): Nội dung câu hỏi (ví dụ: *Mắt bệnh nhân có bị đau buốt tăng dần không?*).
  * `answer_type` (ENUM: `'YES_NO'`, `'SINGLE_CHOICE'`, Default: `'YES_NO'`): Dạng câu trả lời.
  * `options_json` (JSONB, Not Null): Danh sách các phương án lựa chọn.
  * `normal_criteria` (VARCHAR(100), Not Null): Giá trị đáp án được tính là Bình thường.
  * `attention_criteria` (VARCHAR(100), Nullable): Giá trị đáp án kích hoạt trạng thái "Cần chú ý".
  * `red_flag_criteria` (VARCHAR(100), Nullable): Giá trị đáp án kích hoạt "Red Flag" nguy hiểm.
  * `order_index` (INT, Not Null): Thứ tự câu hỏi (1 đến 5) (BR9).

#### 13. Thực thể `TemplateRedFlag` (`template_red_flags`)
* **Mục đích:** Thiết lập các dấu hiệu nguy hiểm và phản ứng khẩn cấp (UC-008).
* **Thuộc tính:**
  * `red_flag_template_id` (PK, UUID): Định danh tiêu chí Red Flag.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (UC-008).
  * `sign_name` (VARCHAR(150), Not Null): Tên dấu hiệu (ví dụ: *Đau nhức dữ dội lan nửa đầu, Đột ngột mờ mắt*).
  * `warning_level` (ENUM: `'EMERGENCY'`, `'SAME_DAY_EXAM'`, Default: `'EMERGENCY'`): Mức độ cảnh báo (Cấp cứu khẩn cấp / Khám trong ngày).
  * `trigger_condition` (TEXT, Nullable): Điều kiện kích hoạt từ kết quả Recovery Check.
  * `first_aid_instructions` (TEXT, Not Null): Các bước sơ cứu cần làm ngay (không dụi mắt, đeo khiên bảo vệ...).
  * `emergency_hotline` (VARCHAR(20), Not Null): Số điện thoại đường dây nóng 24/7 của bệnh viện (BR11, UC-008, UC-020).
  * `order_index` (INT, Default: 1).

#### 14. Thực thể `TemplateDoDontItem` (`template_do_dont_items`)
* **Mục đích:** Danh mục hướng dẫn sinh hoạt Nên làm / Cần tránh trong template (UC-009).
* **Thuộc tính:**
  * `do_dont_template_id` (PK, UUID): Định danh chỉ dẫn mẫu.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (UC-009).
  * `item_type` (ENUM: `'DO'`, `'DONT'`, Not Null): Phân loại (NÊN LÀM / CẦN TRÁNH) (BR22).
  * `category` (ENUM: `'HYGIENE'`, `'ACTIVITY'`, `'DIET'`, `'SLEEP'`, Not Null): Nhóm sinh hoạt (Vệ sinh, Vận động, Ăn uống, Giấc ngủ).
  * `behavior_title` (VARCHAR(200), Not Null): Tên hành vi (ví dụ: *Đeo khiên bảo vệ mắt khi đi ngủ*).
  * `clinical_explanation` (TEXT, Not Null): Giải thích y khoa tại sao nên làm hoặc cần tránh.
  * `applicable_duration` (VARCHAR(100), Not Null): Khoảng thời gian áp dụng (ví dụ: *7 ngày đầu sau mổ*).
  * `order_index` (INT, Default: 1).

---

### 3.4 Phân Hệ Kế Hoạch Chăm Sóc Bệnh Nhân Thực Tế (Patient Care Plan Instance)

#### 15. Thực thể `PatientCarePlan` (`patient_care_plans`)
* **Mục đích:** Thể hiện Kế hoạch chăm sóc độc lập được nhân bản từ Template để gán riêng cho từng bệnh nhân theo cơ sở (UC-010, BR18, BR19, BR26).
* **Thuộc tính:**
  * `care_plan_id` (PK, UUID): Định danh Care Plan của bệnh nhân.
  * `facility_id` (FK -> `facilities.facility_id`, Not Null): Chi nhánh bệnh viện quản lý điều trị (BR26).
  * `patient_id` (FK -> `patients.patient_id`, Not Null): Thuộc hồ sơ bệnh nhân nào.
  * `source_template_id` (FK -> `care_plan_templates.template_id`, Not Null): Template gốc được nhân bản.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật tại thời điểm gán.
  * `status` (ENUM: `'DRAFT'`, `'ACTIVE'`, `'COMPLETED'`, `'ARCHIVED'`, Default: `'DRAFT'`): Trạng thái kế hoạch (BR19: tối đa 1 Active/bệnh nhân).
  * `activated_at` (TIMESTAMP, Nullable): Mốc thời gian bác sĩ/điều dưỡng bấm kích hoạt Care Plan.
  * `completed_at` (TIMESTAMP, Nullable): Mốc thời gian hoàn thành đợt theo dõi.
  * `created_by_doctor_id` (FK -> `doctor_profiles.doctor_id`, Not Null): Bác sĩ chỉ định.
  * `created_at`, `updated_at` (TIMESTAMP).

#### 16. Thực thể `PatientMedication` (`patient_medications`)
* **Mục đích:** Đơn thuốc thực tế của bệnh nhân (được tùy biến liều lượng, loại thuốc từ template khi tạo Care Plan) (UC-010, UC-014, UC-015).
* **Thuộc tính:**
  * `patient_medication_id` (PK, UUID): Định danh mục thuốc thực tế.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Not Null): Thuộc Care Plan nào.
  * `drug_name` (VARCHAR(150), Not Null): Tên thuốc thực tế.
  * `drug_form` (ENUM: `'EYE_DROP'`, `'ORAL'`, Not Null): Dạng nhỏ mắt / uống.
  * `dosage` (VARCHAR(50), Not Null): Liều dùng bác sĩ chỉ định cụ thể.
  * `frequency` (VARCHAR(50), Not Null): Số lần dùng trong ngày.
  * `times_of_day` (JSONB / VARCHAR(100), Not Null): Các cữ dùng (Sáng, Trưa, Chiều, Tối).
  * `meal_relation` (ENUM: `'BEFORE_MEAL'`, `'AFTER_MEAL'`, `'NONE'`, Default: `'NONE'`).
  * `duration_days` (INT, Not Null): Số ngày dùng.
  * `order_index` (INT, Not Null): Thứ tự sử dụng trong cữ.
  * `visual_identification` (TEXT, Not Null): Nhận diện trực quan (vỏ, nắp, màu sắc).
  * `clinical_cautions` (TEXT, Not Null): Lưu ý sử dụng (lắc kỹ, bảo quản lạnh, cách cữ 5 phút...).
  * `start_date` (DATE, Not Null): Ngày bắt đầu dùng thuốc.
  * `notes` (TEXT, Nullable): Lời dặn dò riêng của bác sĩ cho bệnh nhân này.

#### 17. Thực thể `PatientFollowupAppointment` (`patient_followup_appointments`)
* **Mục đích:** Lịch hẹn tái khám cụ thể do bác sĩ thiết lập trong Care Plan (UC-018, UC-010).
* **Thuộc tính:**
  * `appointment_id` (PK, UUID): Định danh lịch hẹn.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Not Null): Thuộc Care Plan nào.
  * `appointment_date` (DATE, Not Null): Ngày hẹn khám lại.
  * `appointment_time` (TIME, Not Null): Giờ hẹn cụ thể.
  * `doctor_id` (FK -> `doctor_profiles.doctor_id`, Not Null): Bác sĩ phụ trách khám.
  * `clinic_location` (VARCHAR(150), Not Null): Phòng khám / Địa chỉ bệnh viện.
  * `preparation_notes` (TEXT, Nullable): Hướng dẫn trước khám (mang theo sổ, nhịn ăn, nhỏ thuốc...).
  * `reminder_24h_sent` (BOOLEAN, Default: FALSE): Cờ ghi nhận đã gửi thông báo trước 24h (BR24).
  * `reminder_2h_sent` (BOOLEAN, Default: FALSE): Cờ ghi nhận đã gửi thông báo trước 2h (BR24).
  * `status` (ENUM: `'SCHEDULED'`, `'COMPLETED'`, `'CANCELLED'`, Default: `'SCHEDULED'`).

#### 18. Thực thể `PatientQRCode` (`patient_qr_codes`)
* **Mục đích:** Lưu trữ mã định danh bảo mật QR in trên phiếu xuất viện cho Caregiver quét liên kết (UC-003, UC-011, UC-012, BR4, BR20).
* **Thuộc tính:**
  * `qr_id` (PK, UUID): Định danh bản ghi mã QR.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Unique, Not Null): Gắn với Care Plan cụ thể.
  * `qr_token` (VARCHAR(255), Unique, Not Null): Chuỗi mã hóa token ngẫu nhiên bảo mật (HMAC/UUID).
  * `issued_by_account_id` (FK -> `accounts.account_id`, Not Null): Bác sĩ hoặc Điều dưỡng phát hành mã QR (UC-011).
  * `issued_at` (TIMESTAMP, Not Null): Thời điểm sinh mã.
  * `status` (ENUM: `'ACTIVE'`, `'REVOKED'`, Default: `'ACTIVE'`): Trạng thái mã QR.
  * `print_count` (INT, Default: 1): Số lần in ấn lại phiếu (UC-012, UC-013).

---

### 3.5 Phân Hệ Giao Dịch & Ghi Nhận Sự Kiện Thực Tế (Operational & Transaction Logs)

#### 19. Thực thể `MedicationLog` (`medication_logs`)
* **Mục đích:** Ghi nhận sự kiện Caregiver xác nhận cho bệnh nhân uống/nhỏ thuốc (UC-014, UC-015).
* **Thuộc tính:**
  * `log_id` (PK, UUID): Định danh nhật ký dùng thuốc.
  * `patient_medication_id` (FK -> `patient_medications.patient_medication_id`, Not Null): Loại thuốc nào.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Ai xác nhận.
  * `scheduled_time` (TIMESTAMP, Not Null): Giờ quy định dùng thuốc theo lịch.
  * `confirmed_at` (TIMESTAMP, Not Null): Thời gian thực tế nhấn "Đánh dấu đã dùng".
  * `status` (ENUM: `'TAKEN'`, `'SKIPPED'`, Default: `'TAKEN'`): Trạng thái cữ thuốc.
  * `notes` (VARCHAR(255), Nullable): Ghi chú của người chăm sóc nếu có phản ứng phụ nhẹ.

#### 20. Thực thể `CaregiverQuizSubmission` (`caregiver_quiz_submissions`)
* **Mục đích:** Lưu kết quả kiểm tra 3 câu Mini Quiz sau khi xem xong bài học Learning Path (UC-017 / Phase 2).
* **Thuộc tính:**
  * `submission_id` (PK, UUID): Định danh lượt nộp bài.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Người làm bài.
  * `module_id` (FK -> `template_learning_modules.module_id`, Not Null): Thuộc bài học nào.
  * `correct_answers_count` (INT, Not Null): Số câu trả lời đúng (ví dụ: 3/3 hoặc 2/3).
  * `total_questions` (INT, Default: 3): Tổng số câu hỏi kiểm tra.
  * `answers_detail` (JSONB, Not Null): Chi tiết các phương án Caregiver đã chọn.
  * `submitted_at` (TIMESTAMP, Not Null): Thời điểm nộp bài.

#### 21. Thực thể `RecoveryCheckSubmission` (`recovery_check_submissions`)
* **Mục đích:** Lưu kết quả mỗi lượt Caregiver trả lời bảng kiểm phục hồi định kỳ (UC-019, UC-021).
* **Thuộc tính:**
  * `submission_id` (PK, UUID): Định danh lượt nộp bảng kiểm.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Not Null): Thuộc Care Plan nào.
  * `milestone_template_id` (FK -> `template_recovery_milestones.milestone_template_id`, Not Null): Tại mốc ngày nào.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Người nộp.
  * `submitted_at` (TIMESTAMP, Not Null): Thời điểm nộp bảng kiểm.
  * `overall_status` (ENUM: `'NORMAL'`, `'NEEDS_ATTENTION'`, `'RED_FLAG'`, Not Null): Đánh giá tổng quan tự động phân loại (BR10).
  * `doctor_viewed` (BOOLEAN, Default: FALSE): Bác sĩ đã xem xét hay chưa (UC-021).
  * `doctor_viewed_at` (TIMESTAMP, Nullable): Mốc thời gian bác sĩ mở xem.
  * `doctor_notes` (TEXT, Nullable): Ghi chú chuyên môn của bác sĩ sau khi xem xét.

#### 22. Thực thể `RecoveryCheckAnswer` (`recovery_check_answers`)
* **Mục đích:** Chi tiết từng câu trả lời trong bảng kiểm phục hồi (UC-019, UC-021).
* **Thuộc tính:**
  * `answer_id` (PK, UUID): Định danh câu trả lời.
  * `submission_id` (FK -> `recovery_check_submissions.submission_id`, Not Null): Thuộc lượt nộp nào.
  * `question_template_id` (FK -> `template_recovery_questions.question_template_id`, Not Null): Trả lời cho câu hỏi nào.
  * `answer_value` (VARCHAR(100), Not Null): Giá trị Caregiver đã chọn (Có/Không, Lựa chọn A/B...).
  * `flag_level` (ENUM: `'NORMAL'`, `'NEEDS_ATTENTION'`, `'RED_FLAG'`, Not Null): Mức độ cảnh báo tự động gắn cờ cho câu hỏi này (BR10).

#### 23. Thực thể `RedFlagIncident` (`red_flag_incidents`)
* **Mục đích:** Lưu vết các sự kiện khẩn cấp xảy ra với bệnh nhân để phục vụ y khoa và pháp lý (UC-020, UC-022, UC-022b).
* **Thuộc tính:**
  * `incident_id` (PK, UUID): Định danh sự kiện khẩn cấp.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Not Null): Bệnh nhân liên quan.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Người phát hiện/thực hiện.
  * `trigger_source` (ENUM: `'RECOVERY_CHECK'`, `'MANUAL_BUTTON'`, Not Null): Nguồn kích hoạt cảnh báo (từ bảng kiểm hay do bấm nút báo động).
  * `red_flag_template_id` (FK -> `template_red_flags.red_flag_template_id`, Nullable): Dấu hiệu nguy hiểm cụ thể nếu xác định được.
  * `triggered_at` (TIMESTAMP, Not Null): Thời điểm phát sinh sự kiện.
  * `call_initiated` (BOOLEAN, Default: FALSE): Caregiver đã bấm gọi đường dây nóng cấp cứu hay chưa (UC-020 Step 2).
  * `call_initiated_at` (TIMESTAMP, Nullable): Thời gian bấm gọi.
  * `acknowledged_by_doctor_id` (FK -> `doctor_profiles.doctor_id`, Nullable): Bác sĩ tiếp nhận xử lý.
  * `acknowledged_at` (TIMESTAMP, Nullable): Mốc thời gian tiếp nhận.
  * `escalated_at` (TIMESTAMP, Nullable): Mốc thời gian tự động kích hoạt leo thang cấp 2 sau 15 phút không tiếp nhận (UC-022b).
  * `escalation_level` (INT, Default: 1): Cấp độ leo thang (1: Tiếp nhận CSKH cơ sở; 2: Leo thang Bác sĩ trực; 3: Ban Giám đốc cơ sở).
  * `clinical_resolution` (TEXT, Nullable): Kết luận xử trí y tế (ví dụ: *Bệnh nhân đã đến viện cấp cứu kịp thời*).

#### 24. Thực thể `CallInterventionLog` (`call_intervention_logs`)
* **Mục đích:** Ghi nhận nhật ký cuộc gọi can thiệp của CSKH/Điều dưỡng xử lý cảnh báo y tế theo cam kết SLA <5 phút (UC-023, BR12).
* **Thuộc tính:**
  * `log_id` (PK, UUID): Định danh bản ghi cuộc gọi can thiệp.
  * `incident_id` (FK -> `red_flag_incidents.incident_id`, Not Null): Sự cố cảnh báo cần xử lý.
  * `patient_id` (FK -> `patients.patient_id`, Not Null): Bệnh nhân liên quan.
  * `caller_account_id` (FK -> `accounts.account_id`, Not Null): Nhân sự y tế/CSKH thực hiện cuộc gọi.
  * `call_time` (TIMESTAMP, Not Null): Thời điểm bắt đầu gọi điện thoại.
  * `duration_seconds` (INT, Default: 0): Thời lượng cuộc gọi tính bằng giây.
  * `call_status` (ENUM: `'ANSWERED'`, `'NO_ANSWER'`, `'BUSY'`, `'FAILED'`, Not Null): Trạng thái kết nối cuộc gọi.
  * `notes` (TEXT, Not Null): Nội dung trao đổi chuyên môn, tình trạng thực tế người bệnh tại nhà.
  * `next_action` (ENUM: `'CONTINUE_MONITORING'`, `'REQUIRE_HOSPITAL_EXAM'`, `'EMERGENCY_DISPATCH'`, `'RECALL_IN_15M'`, Not Null): Hướng xử lý tiếp theo sau cuộc gọi.
  * `created_at` (TIMESTAMP): Dấu thời gian ghi nhận.

---

### 3.6 Phân Hệ Quản Trị, Giao Tiếp Đa Kênh & Kiểm Toán

#### 25. Thực thể `Notification` (`notifications`)
* **Mục đích:** Lưu trữ lịch sử toàn bộ các thông báo đa kênh tự động (SMS Brandname VISI, ZNS Zalo, Mobile Push, In-App Web) phục vụ nhắc lịch thuốc, hẹn tái khám và cảnh báo khẩn cấp (UC-018, UC-022b, BR24).
* **Thuộc tính:**
  * `notification_id` (PK, UUID): Định danh thông báo.
  * `recipient_account_id` (FK -> `accounts.account_id`, Nullable): Tài khoản nhận thông báo (nếu có).
  * `patient_id` (FK -> `patients.patient_id`, Not Null): Bệnh nhân nhận thông báo / đối tượng liên quan.
  * `channel` (ENUM: `'SMS'`, `'ZNS'`, `'PUSH'`, `'IN_APP'`, Not Null): Kênh truyền thông báo.
  * `title` (VARCHAR(200), Not Null): Tiêu đề thông báo.
  * `body` (TEXT, Not Null): Nội dung chi tiết tin nhắn / cảnh báo.
  * `status` (ENUM: `'PENDING'`, `'SENT'`, `'DELIVERED'`, `'FAILED'`, Default: `'PENDING'`): Trạng thái gửi thông báo.
  * `sent_at` (TIMESTAMP, Nullable): Mốc thời gian gửi đi qua Gateway đối tác.
  * `read_at` (TIMESTAMP, Nullable): Mốc thời gian người dùng mở xem trên ứng dụng.
  * `created_at` (TIMESTAMP): Thời gian tạo yêu cầu gửi thông báo.

#### 26. Thực thể `AuditLog` (`audit_logs`) `[FUTURE / ADMIN]`
* **Mục đích:** Phục vụ quản trị viên (Admin) quản lý nhật ký kiểm toán (CRUD Audit Log) và đảm bảo an toàn pháp lý y tế (BR5, BR20, BR21).
* **Thuộc tính:**
  * `audit_id` (PK, BIGINT/UUID): Mã nhật ký kiểm toán.
  * `user_id` (FK -> `accounts.account_id`, Nullable): Ai thực hiện hành vi.
  * `action_type` (VARCHAR(50), Not Null): Hành động (`'LOGIN'`, `'CREATE_PATIENT'`, `'ACTIVATE_PLAN'`, `'GENERATE_QR'`, `'LINK_QR'`, `'TRIGGER_RED_FLAG'`).
  * `target_entity` (VARCHAR(50), Not Null): Tên thực thể chịu tác động (`'patients'`, `'patient_care_plans'`, `'patient_qr_codes'`).
  * `target_id` (VARCHAR(100), Not Null): Khóa của bản ghi bị tác động.
  * `old_data` (JSONB, Nullable): Dữ liệu trước khi sửa đổi.
  * `new_data` (JSONB, Nullable): Dữ liệu sau khi sửa đổi.
  * `ip_address` (VARCHAR(45), Nullable): Địa chỉ IP thực hiện.
  * `user_agent` (VARCHAR(255), Nullable): Thiết bị/trình duyệt.
  * `timestamp` (TIMESTAMP, Not Null): Mốc thời gian phát sinh.

---

## 4. Mối Quan Hệ & Bản Số Thực Thể (Relationships & Cardinality)

| Thực Thể 1 | Bản Số | Quan Hệ Nghiệp Vụ | Thực Thể 2 | Diễn Giải Chi Tiết |
|---|:---:|---|---|---|
| `Facility` | **1 : N** | Quản lý nhân sự | `UserAccount` | Một chi nhánh bệnh viện có nhiều nhân viên y tế (Bác sĩ, Điều dưỡng, CSKH) |
| `Facility` | **1 : N** | Tiếp nhận điều trị | `PatientProfile` | Một chi nhánh bệnh viện tiếp nhận mổ và quản lý hồ sơ nhiều bệnh nhân |
| `Facility` | **1 : N** | Quản lý đợt chăm sóc | `PatientCarePlan` | Một chi nhánh bệnh viện vận hành nhiều kế hoạch chăm sóc bệnh nhân |
| `UserAccount` | **1 : 1** | Có hồ sơ chuyên môn | `DoctorProfile` | Một tài khoản nhân viên gắn với duy nhất một hồ sơ chuyên môn |
| `UserAccount` | **1 : 1** | Có hồ sơ người chăm sóc | `CaregiverProfile` | Một tài khoản Caregiver gắn với một hồ sơ người chăm sóc |
| `UserAccount` | **1 : N** | Phát hành mã QR | `PatientQRCode` | Bác sĩ hoặc Điều dưỡng phát hành mã QR bàn giao cho người bệnh (UC-011) |
| `UserAccount` | **0..1 : N** | Phê duyệt chuyên môn | `CarePlanTemplate` | GCMO hoặc Bác sĩ Trưởng khoa ký duyệt ban hành Master Template (UC-006) |
| `UserAccount` | **1 : N** | Thực hiện cuộc gọi | `CallInterventionLog` | CSKH hoặc Điều dưỡng thực hiện các cuộc gọi can thiệp sự cố (UC-023) |
| `DoctorProfile` | **1 : N** | Tiếp nhận & quản lý | `PatientProfile` | Một Bác sĩ tạo và quản lý nhiều hồ sơ bệnh nhân |
| `DoctorProfile` | **1 : N** | Biên soạn | `CarePlanTemplate` | Một Bác sĩ có thể cấu hình nhiều Care Plan Template mẫu |
| `DoctorProfile` | **1 : N** | Phụ trách khám lại | `PatientFollowupAppointment` | Một Bác sĩ có thể phụ trách nhiều buổi hẹn tái khám |
| `DoctorProfile` | **0..1 : N** | Tiếp nhận xử lý | `RedFlagIncident` | Bác sĩ tiếp nhận ca cấp cứu (có thể chưa có bác sĩ tiếp nhận) |
| `CarePlanTemplate` | **0..1 : N** | Phiên bản kế thừa | `CarePlanTemplate` | Template tạo bản sao mới (copy-on-write v1.1) qua `parent_template_id` (BR15) |
| `CarePlanTemplate` | **1 : N** | Chứa | `TemplateLearningModule` | Một template có nhiều bài học trong Learning Path |
| `TemplateLearningModule` | **1 : N** (3 câu) | Có câu hỏi kiểm tra | `TemplateQuizQuestion` | Mỗi bài học chứa chính xác 3 câu hỏi trắc nghiệm Mini Quiz |
| `CarePlanTemplate` | **1 : N** | Định nghĩa | `TemplateMedication` | Một template quy định danh mục thuốc mẫu cho loại mổ |
| `CarePlanTemplate` | **1 : N** | Thiết lập các mốc | `TemplateRecoveryMilestone` | Một template có nhiều mốc kiểm tra (Day 1, 3, 7...) |
| `TemplateRecoveryMilestone` | **1 : N** (3–5 câu)| Chứa các câu hỏi | `TemplateRecoveryQuestion` | Mỗi mốc thời gian có từ 3 đến 5 câu hỏi khảo sát |
| `CarePlanTemplate` | **1 : N** | Thiết lập | `TemplateRedFlag` | Một template có nhiều dấu hiệu nguy hiểm được cảnh báo |
| `CarePlanTemplate` | **1 : N** | Hướng dẫn | `TemplateDoDontItem` | Một template có danh mục các việc Nên làm và Cần tránh |
| `PatientProfile` | **1 : N** | Có lịch sử các kế hoạch | `PatientCarePlan` | Bệnh nhân có thể trải qua nhiều đợt chăm sóc (tối đa 1 Active) |
| `CarePlanTemplate` | **1 : N** | Cung cấp mẫu nhân bản | `PatientCarePlan` | Một template gốc được sao chép sang cho nhiều bệnh nhân |
| `PatientCarePlan` | **1 : 1** | Được định danh bảo mật bởi | `PatientQRCode` | Mỗi Care Plan Active có duy nhất một token QR hợp lệ |
| `PatientCarePlan` | **1 : N** | Kê đơn thực tế | `PatientMedication` | Mỗi Care Plan bệnh nhân có danh mục thuốc thực tế riêng |
| `PatientCarePlan` | **1 : N** | Lên lịch hẹn | `PatientFollowupAppointment` | Mỗi Care Plan có một hoặc nhiều buổi hẹn tái khám |
| `CaregiverProfile` | **N : M** | Chăm sóc & Theo dõi | `PatientProfile` | Một Caregiver có thể chăm sóc nhiều bệnh nhân (qua `CaregiverPatientLink`, tối đa 3) |
| `PatientMedication` | **1 : N** | Ghi nhận lần uống | `MedicationLog` | Một loại thuốc được ghi nhận nhiều lần uống qua các ngày |
| `CaregiverProfile` | **1 : N** | Xác nhận dùng thuốc | `MedicationLog` | Một Caregiver ghi nhận nhiều lượt xác nhận cữ thuốc |
| `TemplateLearningModule` | **1 : N** | Đánh giá kiến thức | `CaregiverQuizSubmission` | Một bài học có thể nhận nhiều lượt nộp Quiz từ các Caregiver |
| `CaregiverProfile` | **1 : N** | Nộp bài kiểm tra | `CaregiverQuizSubmission` | Một Caregiver nộp nhiều bài kiểm tra qua các bài học |
| `PatientCarePlan` | **1 : N** | Tiếp nhận kết quả | `RecoveryCheckSubmission` | Một Care Plan tiếp nhận các đợt nộp bảng kiểm theo từng mốc |
| `TemplateRecoveryMilestone` | **1 : N** | Đánh giá tại mốc | `RecoveryCheckSubmission` | Mỗi mốc có thể nhận nhiều lượt nộp bảng kiểm |
| `CaregiverProfile` | **1 : N** | Nộp bảng kiểm | `RecoveryCheckSubmission` | Một Caregiver nộp nhiều lượt bảng kiểm phục hồi |
| `RecoveryCheckSubmission` | **1 : N** | Chứa chi tiết | `RecoveryCheckAnswer` | Mỗi lượt nộp chứa câu trả lời cho từng câu hỏi khảo sát |
| `TemplateRecoveryQuestion` | **1 : N** | Được trả lời bởi | `RecoveryCheckAnswer` | Mỗi câu hỏi mẫu nhận nhiều câu trả lời từ các lượt nộp |
| `PatientCarePlan` | **1 : N** | Phát sinh sự kiện | `RedFlagIncident` | Một bệnh nhân có thể ghi nhận các sự cố khẩn cấp |
| `CaregiverProfile` | **1 : N** | Báo cáo sự cố | `RedFlagIncident` | Một Caregiver có thể báo nhiều sự kiện khẩn cấp |
| `TemplateRedFlag` | **0..1 : N** | Xác định dấu hiệu | `RedFlagIncident` | Mỗi sự kiện có thể khớp với dấu hiệu mẫu cụ thể |
| `RedFlagIncident` | **1 : N** | Kích hoạt can thiệp | `CallInterventionLog` | Một sự cố khẩn cấp có thể có một hoặc nhiều cuộc gọi xử lý từ CSKH |
| `PatientProfile` | **1 : N** | Nhận cuộc gọi | `CallInterventionLog` | Lịch sử các cuộc gọi can thiệp thoại tới bệnh nhân/người nhà |
| `PatientProfile` | **1 : N** | Nhận thông báo | `Notification` | Lịch sử các thông báo đa kênh gửi cho bệnh nhân |
| `UserAccount` | **0..1 : N** | Nhận thông báo app | `Notification` | Thông báo gửi tới tài khoản người dùng cụ thể |

---

## 5. Ánh Xạ Quy Tắc Nghiệp Vụ Vào Dữ Liệu (Business Rules Mapping)

| Mã Quy Tắc | Tên Quy Tắc Nghiệp Vụ | Ràng Buộc Dữ Liệu Cụ Thể (Database Constraints & Logic) |
|---|---|---|
| **BR1, BR2** | Xác thực SĐT & OTP | `OtpVerification`: `expired_at > NOW()`, `is_used = FALSE`, khóa tạm nếu `attempt_count > 5`. |
| **BR4, BR20** | Tính hợp lệ & bảo mật QR | `patient_qr_codes.status = 'ACTIVE'` VÀ `patient_care_plans.status = 'ACTIVE'`. `qr_token` mã hóa ngẫu nhiên không thể đoán trước; `issued_by_account_id` cho phép Điều dưỡng hoặc Bác sĩ phát hành (UC-011). |
| **BR5** | Toàn vẹn Caregiver - Bệnh nhân | Bảng trung gian `caregiver_patient_links` lưu `caregiver_id`, `patient_id`, `role` (`PRIMARY`/`SECONDARY`), `linked_at` để truy vết ủy quyền rõ ràng (tối đa 3 Caregiver/bệnh nhân). |
| **BR6, BR7** | Nguồn gốc nội dung y khoa | Toàn bộ các bảng `template_*` và `patient_*` bắt buộc tham chiếu khóa ngoại đến `doctor_id` người tạo, không cho phép nội dung vô chủ (orphaned data). |
| **BR8** | Quiz không chặn chức năng | Kết quả bài kiểm tra lưu ở `caregiver_quiz_submissions` độc lập, không đặt cờ khóa quyền truy cập của bệnh nhân. |
| **BR9** | Bắt buộc hoàn thành 3–5 câu | Giao dịch nộp bảng kiểm chỉ hợp lệ khi số bản ghi `recovery_check_answers` bằng đúng số câu hỏi bắt buộc của mốc đó. |
| **BR10** | Tự động phân loại lâm sàng | `overall_status` trong `recovery_check_submissions` được tính tự động dựa trên mức độ cảnh báo cao nhất của các câu trả lời (`RED_FLAG` > `NEEDS_ATTENTION` > `NORMAL`). |
| **BR11, BR12** | Quy trình khẩn cấp & SLA CSKH | Khi `overall_status = 'RED_FLAG'`, hệ thống tự động sinh bản ghi trong `red_flag_incidents` và kích hoạt luồng khẩn cấp. CSKH tiếp nhận và ghi nhận nhật ký vào `call_intervention_logs` cam kết SLA <5 phút. |
| **BR14** | Phân quyền RBAC nhân sự | Bảng `accounts.role` phân tách rõ: `DOCTOR`, `NURSE`, `CSKH`, `GCMO`, `ADMIN`, `CAREGIVER`, `CARE_RECIPIENT`. Chỉ `DOCTOR` mới được gán `created_by_doctor_id` trong Care Plan. |
| **BR15** | Thông tin bệnh nhân bắt buộc | Cột `full_name`, `date_of_birth`, `gender`, `surgery_type`, `operated_eye`, `facility_id` đều mang ràng buộc `NOT NULL` tại bảng `patients`. |
| **BR16** | Ràng buộc loại phẫu thuật | Cột `surgery_type` bắt buộc có giá trị xác định tại bảng `care_plan_templates` và các bảng cấu phần con. |
| **BR17, BR18** | Tính cô lập của Care Plan | Phân chia thành 2 cấu trúc bảng riêng biệt: Master Template (`template_medications`, `template_recovery_questions`...) và Patient Care Plan (`patient_medications`...). Sửa đổi ở Master Template hoàn toàn không làm biến đổi bản ghi trong Patient Care Plan đã tạo. |
| **BR19** | Duy nhất 1 Care Plan Active | Tạo chỉ mục độc nhất có điều kiện (Partial Unique Index) trên `patient_care_plans`: `UNIQUE(patient_id) WHERE status = 'ACTIVE'`. |
| **BR22** | Danh mục Do & Don't | `template_do_dont_items.item_type` bắt buộc thuộc ENUM (`'DO'`, `'DONT'`) và có mốc thời gian áp dụng rõ ràng. |
| **BR23** | Khoảng cách nhỏ mắt 5 phút | `patient_medications.clinical_cautions` lưu chỉ dẫn thao tác; giao diện căn cứ vào trường `drug_form = 'EYE_DROP'` và `order_index` để đếm lùi thời gian giãn cách giữa các thuốc nhỏ mắt. |
| **BR24** | Nhắc hẹn tái khám 24h & 2h | Hai cờ `reminder_24h_sent` và `reminder_2h_sent` tại bảng `patient_followup_appointments` kết hợp với bảng `notifications` để điều khiển dịch vụ gửi tin nhắn đa kênh. |
| **BR25** | Xóa mềm hồ sơ bệnh nhân | Bảng `patients` áp dụng `status = 'ARCHIVED'` (Soft Delete), ngăn chặn câu lệnh `DELETE` cứng nếu bệnh nhân đã phát sinh liên kết QR hoặc Care Plan. |
| **BR26** | Cô lập dữ liệu đa chi nhánh | Thực thể `facilities` quản lý 5 chi nhánh. Mọi bảng cốt lõi (`accounts`, `patients`, `patient_care_plans`, `doctor_profiles`) đều mang khóa ngoại `facility_id` phục vụ cô lập dữ liệu theo từng cơ sở (ngoại trừ GCMO xem dữ liệu chuỗi). |
| **BR-NEW-01** | Khóa quyền sửa thuốc của Điều dưỡng | Điều dưỡng (`role = 'NURSE'`) được cấp phát QR, hỗ trợ bệnh nhân nhưng TUYỆT ĐỐI không được sửa liều lượng thuốc `patient_medications` hoặc chỉ định phác đồ. |
| **BR-NEW-02** | Cơ chế phiên bản Master Template | Template đang `ACTIVE` không được sửa trực tiếp; hệ thống tạo bản ghi mới (copy-on-write) với `version` tăng dần và `parent_template_id` trỏ về bản gốc, giữ nguyên các Care Plan đang chạy. |

---

## 6. Ma Trận CRUD Dữ Liệu Theo Từng Use Case (Data CRUD Matrix)

Ký hiệu: **C** = Create (Tạo mới), **R** = Read (Đọc/Xem), **U** = Update (Cập nhật), **D** = Delete/Archive (Xóa/Lưu trữ).  
*Được chuẩn hóa đồng bộ 100% theo 31 Ca sử dụng của US_US_V1.md:*

| Mã Use Case | Tên Use Case | Bảng Dữ Liệu Tác Động | Hành Động CRUD | Ghi Chú Nghiệp Vụ |
|---|---|---|:---:|---|
| **UC-001** | Đăng ký & Đăng nhập Caregiver qua OTP | `accounts`, `caregiver_profiles`, `otp_verifications` | **C, R, U** | Đọc tài khoản, tạo mã OTP mới, cập nhật `is_used = TRUE`; cấp phiên JWT an toàn NĐ 13. |
| **UC-002** | Đăng nhập Nhân viên Y tế & 2FA | `accounts`, `doctor_profiles` | **R** | Đối chiếu thông tin đăng nhập, xác thực 2FA, cấp phiên làm việc theo cơ sở bệnh viện. |
| **UC-003** | Quét QR Bàn Giao Liên Kết Bệnh Nhân | `caregiver_patient_links`, `patient_care_plans`, `patients` | **R, C** | Đọc & xác thực `qr_token`, kiểm tra Care Plan active, tạo mới liên kết (tối đa 3 Caregiver). |
| **UC-004.1**| Tạo Mới Hồ Sơ Bệnh Nhân (Create) | `patients` | **C** | Lưu bệnh nhân mới (<30s), sinh `patient_id` UUIDv4, lưu tên viết tắt bảo mật NĐ 13/2023. |
| **UC-004.2**| Tra Cứu & Lọc Bệnh Nhân (Read/List) | `patients` | **R** | Truy vấn danh sách bệnh nhân phân trang theo chi nhánh cơ sở (`facility_id`), lọc theo loại mổ. |
| **UC-004.3**| Chi Tiết Hồ Sơ & Timeline (Read/Detail)| `patients`, `patient_care_plans` | **R** | Tải hồ sơ bệnh án 360 độ: tiến trình dùng thuốc, lịch sử nộp khảo sát, Caregiver liên kết. |
| **UC-004.4**| Cập Nhật Thông Tin Bệnh Nhân (Update) | `patients` | **U** | Cập nhật SĐT, ghi chú dị ứng; khóa loại phẫu thuật nếu Care Plan đã kích hoạt bàn giao. |
| **UC-004.5**| Lưu Trữ / Vô Hiệu Hóa (Archive/Soft Delete)| `patients` | **D** | Chuyển `status = 'ARCHIVED'` (xóa mềm), thu hồi mã QR chưa kích hoạt, bảo toàn dữ liệu y khoa. |
| **UC-005.1**| Tạo Mới Master Template (Create) | `care_plan_templates` | **C** | Khởi tạo khung phác đồ mẫu ở trạng thái `DRAFT` v1.0 gắn với loại phẫu thuật chuẩn hóa (BR16). |
| **UC-005.2**| Xem Danh Sách Template (Read/List) | `care_plan_templates` | **R** | Đọc danh mục toàn bộ các gói phác đồ mẫu, hỗ trợ lọc theo loại mổ và trạng thái vòng đời. |
| **UC-005.3**| Xem Chi Tiết Cấu Hình (Read/Detail) | `care_plan_templates`, các bảng `template_*` | **R** | Tải toàn bộ cấu hình: thuốc mẫu, mốc câu hỏi recovery check, red flag, cẩm nang 24h & Do/Don't. |
| **UC-005.4**| Chỉnh Sửa Thông Tin Chung (Update) | `care_plan_templates` | **U** | Sửa tên, mô tả y khoa; tự động nhân bản v1.1 nếu template đã duyệt ACTIVE (BR15). |
| **UC-005.5**| Kích Hoạt / Lưu Trữ Template (Status)| `care_plan_templates` | **U** | Cập nhật trạng thái `PENDING_APPROVAL`, `ACTIVE`, `INACTIVE` theo thẩm quyền GCMO. |
| **UC-006** | Phê Duyệt Lâm Sàng Master Template | `care_plan_templates`, `system_audit_logs` | **U, C** | GCMO ký duyệt điện tử ban hành áp dụng toàn chuỗi 5 bệnh viện VISI, ghi nhật ký kiểm toán. |
| **UC-007.1**| Thêm Thuốc Mẫu & Timer Đệm (Create) | `template_medications` | **C** | Thêm thuốc mẫu, đặt số giọt, cữ dùng và cài đặt thời gian đệm 5–10 phút chống rửa trôi (BR23). |
| **UC-007.2**| Xem Danh Sách Thuốc Mẫu (Read/List) | `template_medications` | **R** | Đọc danh sách thuốc, thứ tự nhỏ mắt ưu tiên (nước trước, mỡ sau) và thời gian đệm giữa các lọ. |
| **UC-007.3**| Chỉnh Sửa Thuốc Mẫu & Đệm (Update) | `template_medications` | **U** | Điều chỉnh liều lượng, số cữ hoặc tinh chỉnh thời gian đệm giãn cách an toàn giữa các lần nhỏ. |
| **UC-007.4**| Xóa Thuốc Mẫu Khỏi Template (Delete)| `template_medications` | **D** | Xóa bản ghi thuốc mẫu không còn phù hợp khỏi phác đồ nháp đang xây dựng. |
| **UC-008.1**| Thêm Mốc & Câu Hỏi Recovery Check (Create)| `template_recovery_milestones`, `template_recovery_questions` | **C** | Thiết lập mốc theo dõi định kỳ (Ngày 1, 3, 7, 14, 30) kèm bộ câu hỏi phân loại 3 màu (BR24). |
| **UC-008.2**| Xem Danh Sách Mốc & Câu Hỏi (Read) | `template_recovery_milestones`, `template_recovery_questions` | **R** | Đọc danh sách mốc khảo sát và ma trận câu hỏi đánh giá phục hồi thị lực. |
| **UC-008.3**| Chỉnh Sửa Mốc & Câu Hỏi (Update) | `template_recovery_milestones`, `template_recovery_questions` | **U** | Cập nhật văn phong câu hỏi, đổi ngày áp dụng hoặc tinh chỉnh mức độ phân loại màu cảnh báo. |
| **UC-008.4**| Xóa Mốc / Câu Hỏi Check (Delete) | `template_recovery_milestones`, `template_recovery_questions` | **D** | Xóa mốc khảo sát hoặc câu hỏi thừa khỏi cấu hình template nháp. |
| **UC-008.5**| Thêm Tiêu Chí Red Flag & Hotline (Create)| `template_red_flags` | **C** | Cài đặt dấu hiệu biến chứng khẩn cấp gắn cố định Hotline 0395 151 151 và SLA <5p (BR11, BR12). |
| **UC-008.6**| Xem Danh Sách Tiêu Chí Red Flag (Read)| `template_red_flags` | **R** | Đọc danh mục các dấu hiệu nguy hiểm kèm hướng dẫn sơ cứu tức thì cho người nhà. |
| **UC-008.7**| Chỉnh Sửa Tiêu Chí Red Flag (Update)| `template_red_flags` | **U** | Hiệu chỉnh mô tả triệu chứng hoặc bổ sung lời dặn xử trí cấp cứu tức thì. |
| **UC-008.8**| Xóa Tiêu Chí Red Flag (Delete) | `template_red_flags` | **D** | Xóa tiêu chí Red Flag khỏi template draft; ràng buộc giữ lại ≥1 tiêu chí (BR11). |
| **UC-009.1**| Thêm Cẩm Nang 24h & Infographic (Create)| `template_learning_modules` | **C** | Thêm bài học đồ họa tĩnh, gắn cờ cẩm nang 24h sống còn và lưu URL ảnh Infographic tối ưu CDN. |
| **UC-009.2**| Xem Danh Mục Cẩm Nang (Read/List) | `template_learning_modules` | **R** | Đọc danh mục bài học Infographic tĩnh theo thứ tự lộ trình chăm sóc phục hồi. |
| **UC-009.3**| Chỉnh Sửa Bài Học Cẩm Nang (Update) | `template_learning_modules` | **U** | Cập nhật tiêu đề, thay đổi ảnh Infographic hoặc hiệu chỉnh tóm tắt y khoa 3 gạch đầu dòng. |
| **UC-009.4**| Xóa Bài Học Khỏi Lộ Trình (Delete) | `template_learning_modules` | **D** | Gỡ bỏ bài học khỏi template nháp và sắp xếp lại thứ tự bài học còn lại. |
| **UC-009.5**| Thêm Quy Tắc Do/Don't 2 Cột Màu (Create)| `template_do_dont_items` | **C** | Thêm quy tắc sinh hoạt vào Cột Xanh (DO) hoặc Cột Đỏ (DON'T) phân theo nhóm (BR16). |
| **UC-009.6**| Xem Danh Sách Quy Tắc Do/Don't (Read)| `template_do_dont_items` | **R** | Đọc bảng ma trận 2 cột màu trực quan phân theo các nhóm sinh hoạt: Ăn uống, Vệ sinh, Vận động. |
| **UC-009.7**| Chỉnh Sửa Quy Tắc Do/Don't (Update) | `template_do_dont_items` | **U** | Cập nhật mô tả quy tắc hoặc bổ sung giải thích lý do y khoa ngăn biến chứng lệch vạt giác mạc. |
| **UC-009.8**| Xóa Quy Tắc Do/Don't Khỏi Template (Delete)| `template_do_dont_items` | **D** | Xóa một quy tắc sinh hoạt không còn áp dụng khỏi phác đồ mẫu. |
| **UC-009.9**| Quản Lý Ngân Hàng FAQ Lâm Sàng (All CRUD)| `template_do_dont_items` | **C, R, U, D** | Quản trị kho tình huống hỏi đáp khẩn cấp (dính nước, quên nhỏ thuốc) hiển thị nhanh trên app. |
| **UC-010** | Khởi Tạo & Cá Nhân Hóa Care Plan | `patient_care_plans`, `patient_medications`, `patient_followup_appointments` | **C, U** | Nhân bản Master Template thành Care Plan thực tế trong <30s; tùy biến liều lượng độc lập (BR18). |
| **UC-010b**| Thực Hiện Bàn Giao Phòng Lưu Viện | `patient_care_plans`, `patients` | **R, U** | Điều dưỡng dán khiên mắt bảo hộ, trao phiếu QR, hướng dẫn quét mã tại phòng lưu viện. |
| **UC-010c**| Xác Nhận Hoàn Tất Bàn Giao Lâm Sàng | `caregiver_patient_links`, `patient_care_plans` | **U** | Chuyển trạng thái Care Plan sang `ACTIVE` toàn diện, đẩy hồ sơ lên Dashboard CSKH. |
| **UC-011** | Tạo và Phát Hành Mã QR Xuất Viện | `patient_care_plans` (trường qr_token, qr_status) | **C, U** | Sinh mã token ngẫu nhiên mã hóa an toàn (UUIDv4/HMAC-SHA256) gán với Care Plan. |
| **UC-012** | In Phiếu Hướng Dẫn Xuất Viện Kèm QR | `patient_care_plans`, `patients`, `doctor_profiles` | **R** | Xuất lệnh in 1 chạm Phiếu xuất viện khổ A5/A4 tại quầy có mã QR sắc nét và Hotline 0395 151 151. |
| **UC-013** | Cấp Lại hoặc Thu Hồi Mã QR Bàn Giao | `patient_care_plans`, `system_audit_logs` | **U, C** | Chuyển mã QR cũ sang `REVOKED`, sinh mã QR mới, in lại phiếu và lưu vết kiểm toán. |
| **UC-014** | Xem Lịch Thuốc & Kỹ Thuật Nhỏ Mắt | `patient_medications` | **R** | Đọc danh sách cữ thuốc Sáng-Trưa-Chiều-Tối, ảnh nhận diện vỏ lọ, số giọt và video kéo mi dưới. |
| **UC-015** | Xác Nhận Thuốc & Kích Hoạt Buffer Timer | `medication_logs`, `patient_medications` | **C, U** | Ghi nhận cữ thuốc hoàn thành; tự động đếm lùi 5–10 phút khóa lọ 2 chống rửa trôi thuốc. |
| **UC-016** | Xem Cẩm Nang 24h & Bảng Do/Don't | `template_do_dont_items`, `care_plan_templates` | **R** | Đọc cẩm nang 24h sống còn và bảng danh mục Nên làm (Xanh) / Cần tránh (Đỏ) theo sinh hoạt. |
| **UC-017** | Xem Infographic Học Viện Caregiver | `template_learning_modules` | **R** | Học tập qua Infographic đồ họa tĩnh tinh gọn (bỏ quiz trắc nghiệm trong MVP theo F-014). |
| **UC-018** | Xem Lịch Tái Khám & Nhận Thông Báo | `patient_followup_appointments` | **R, U** | Theo dõi 5 mốc tái khám chuẩn VISI; gửi thông báo tự động trước 24h; xác nhận lịch hẹn. |
| **UC-019** | Nộp Khảo Sát Phục Hồi Recovery Check | `recovery_check_submissions`, `recovery_check_answers`, `patient_care_plans` | **C, U** | Lưu kết quả khảo sát 3–5 câu mỗi sáng; tự động phân loại 3 mức Xanh/Vàng/Đỏ (BR10). |
| **UC-020** | Kích Hoạt Cấp Cứu Red Flag Khẩn Cấp | `red_flag_incidents`, `patient_care_plans` | **C, U** | Chuyển giao diện đỏ toàn màn hình; nút gọi 1 chạm Hotline VISI 0395 151 151; phát chuông viện. |
| **UC-021** | Giám Sát Dashboard Phục Hồi Tập Trung | `patient_care_plans`, `patients`, `medication_logs`, `recovery_check_submissions` | **R** | Kết xuất danh sách bệnh nhân chi nhánh theo 3 tầng trạng thái (Đỏ, Vàng, Xanh). |
| **UC-022** | Tiếp Nhận & Xử Lý Cảnh Báo Red Flag | `red_flag_incidents`, `patient_care_plans` | **U** | Bật chuông báo động, nhận ca, gọi điện thoại can thiệp lâm sàng trong cam kết SLA <5 phút. |
| **UC-022b**| Tự Động Leo Thang Cảnh Báo Quá Hạn | `red_flag_incidents`, `system_audit_logs` | **U** | Tự động phát chuông cấp 2 và gửi tin nhắn SMS khẩn cấp tới Bác sĩ trực cơ sở sau 15 phút. |
| **UC-023** | Ghi Nhận Nhật Ký Cuộc Gọi Can Thiệp | `call_intervention_logs`, `red_flag_incidents` | **C, U** | Ghi nhận chi tiết kết quả cuộc gọi hỗ trợ, lời dặn y tế và trạng thái ca bệnh. |
| **UC-024** | Tra Cứu FAQ Tình Huống Khẩn Cấp | `template_do_dont_items` | **R** | Tra cứu nhanh chỉ dẫn chuẩn y khoa theo tình huống tại nhà (dính nước, quên nhỏ thuốc). |
| **UC-025** | Kích Hoạt Chế Độ Trợ Năng Nhãn Khoa | `caregiver_profiles` | **U, R** | Cấu hình cỡ chữ to (≥18pt), tương phản cao High Contrast và Audio Guide đọc tiếng Việt. |
| **UC-026.1**| Khởi Tạo Tài Khoản Nhân Viên (Create) | `accounts`, `doctor_profiles` | **C** | Tạo tài khoản nhân sự mới, gán vai trò RBAC (`DOCTOR`, `NURSE`, `CSKH`, `GCMO`) và cơ sở (BR26). |
| **UC-026.2**| Xem Danh Sách Nhân Viên (Read/List) | `accounts`, `doctor_profiles` | **R** | Tra cứu, lọc danh bạ nhân sự y tế phân trang theo 5 chi nhánh VISI và trạng thái hoạt động. |
| **UC-026.3**| Chỉnh Sửa Thông Tin & Vai Trò (Update)| `accounts`, `doctor_profiles` | **U** | Điều chuyển cơ sở làm việc, cập nhật quyền hạn; kích hoạt cơ chế thu hồi phiên làm việc cũ. |
| **UC-026.4**| Khóa / Vô Hiệu Hóa Tài Khoản (Lock) | `accounts` | **D** | Khóa tài khoản (`status = 'LOCKED'`), ngắt phiên làm việc tức thì trên mọi thiết bị (BR26). |
| **UC-027** | Tra Cứu Nhật Ký Kiểm Toán Hệ Thống | `system_audit_logs` | **R** | Truy vấn nhật ký bất biến phục vụ kiểm tra an toàn thông tin và thanh tra pháp lý y tế. |
| **UC-028** | Kết Xuất Báo Cáo Vận Hành KPI | `patient_care_plans`, `medication_logs`, `patient_followup_appointments` | **R** | Tổng hợp các chỉ số KPIs: tỷ lệ kích hoạt QR, tỷ lệ tuân thủ thuốc đúng giờ toàn chuỗi. |
