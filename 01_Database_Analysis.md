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
| **I** | **Người dùng & Xác thực** | `UserAccount` | Tài khoản người dùng chung trong hệ thống (Caregiver, Doctor, `[FUTURE / ADMIN]: Admin`) | UC-01, UC-11 |
| | | `DoctorProfile` | Thông tin định danh chuyên môn của Bác sĩ điều trị | UC-11, UC-12, UC-13 |
| | | `CaregiverProfile` | Thông tin người chăm sóc bệnh nhân | UC-01, UC-02 |
| | | `OtpVerification` | Mã OTP và phiên xác thực đăng nhập số điện thoại | UC-01 |
| **II** | **Hồ sơ Bệnh nhân & Liên kết** | `PatientProfile` | Hồ sơ hành chính và đặc điểm lâm sàng của bệnh nhân | UC-12 (12.1 → 12.5) |
| | | `CaregiverPatientLink` | Quan hệ ủy quyền liên kết giữa Caregiver và Bệnh nhân qua mã QR | UC-02, UC-12.3 |
| **III** | **Master Care Plan Template** | `CarePlanTemplate` | Gói phác đồ chăm sóc mẫu gắn với loại phẫu thuật chuyên khoa | UC-13 (13.1 → 13.5) |
| | | `TemplateLearningModule` | Bài học hướng dẫn chăm sóc trong lộ trình mẫu | UC-14 (14.1 → 14.5) |
| | | `TemplateQuizQuestion` | Câu hỏi trắc nghiệm Mini Quiz (3 câu) kiểm tra sau bài học | UC-03, UC-14.1, UC-14.3 |
| | | `TemplateMedication` | Thuốc mẫu kèm nhận diện trực quan và lưu ý lâm sàng | UC-15 (15.1 → 15.4) |
| | | `TemplateRecoveryMilestone` | Mốc thời gian khảo sát phục hồi chuẩn (Ngày 1, 3, 7...) | UC-16 (16.1 → 16.4) |
| | | `TemplateRecoveryQuestion` | Câu hỏi khảo sát triệu chứng (3–5 câu) và tiêu chí cảnh báo | UC-16 (16.1 → 16.4) |
| | | `TemplateRedFlag` | Tiêu chí dấu hiệu nguy hiểm khẩn cấp và hotline hỗ trợ | UC-17 (17.1 → 17.4) |
| | | `TemplateDoDontItem` | Hướng dẫn hành vi Nên làm / Cần tránh trong sinh hoạt | UC-18 (18.1 → 18.4) |
| **IV** | **Kế Hoạch Chăm Sóc Bệnh Nhân** | `PatientCarePlan` | Bản sao thực tế được kích hoạt và cá nhân hóa cho từng bệnh nhân | UC-19 |
| | | `PatientMedication` | Đơn thuốc thực tế được bác sĩ tùy biến liều lượng cho bệnh nhân | UC-06, UC-19 |
| | | `PatientFollowupAppointment` | Lịch hẹn tái khám thực tế của bệnh nhân | UC-07, UC-19 |
| | | `PatientQRCode` | Mã QR token bảo mật gắn với Care Plan của bệnh nhân | UC-02, UC-20 |
| **V** | **Giao Dịch & Sự Kiện Lâm Sàng** | `MedicationLog` | Nhật ký Caregiver xác nhận uống/nhỏ thuốc theo cữ | UC-06 |
| | | `CaregiverQuizSubmission` | Lịch sử làm bài trắc nghiệm Mini Quiz của Caregiver | UC-03 |
| | | `RecoveryCheckSubmission` | Lượt nộp bảng kiểm phục hồi định kỳ của Caregiver | UC-08, UC-21 |
| | | `RecoveryCheckAnswer` | Chi tiết câu trả lời cho từng câu hỏi phục hồi | UC-08, UC-21 |
| | | `RedFlagIncident` | Sự kiện kích hoạt cảnh báo khẩn cấp hoặc gọi hotline | UC-09, UC-21 |
| **VI** | **Mở Rộng Quản Trị `[FUTURE / ADMIN]`** | `AuditLog` | Nhật ký kiểm toán các thao tác dữ liệu trọng yếu | `[FUTURE / ADMIN]`, BR5, BR20 |

---

## 3. Phân Tích Chi Tiết Thuộc Tính & Khóa (Attributes, PK, FK)

### 3.1 Phân Hệ Người Dùng & Định Danh

#### 1. Thực thể `UserAccount` (`accounts`)
* **Mục đích:** Quản lý danh tính đăng nhập tập trung cho toàn bộ hệ thống.
* **Thuộc tính:**
  * `account_id` (PK, UUID/INT): Định danh tài khoản duy nhất.
  * `phone_number` (VARCHAR(20), Unique, Nullable): Số điện thoại dùng đăng nhập OTP (Caregiver) hoặc liên hệ (Doctor).
  * `username` (VARCHAR(50), Unique, Nullable): Tên đăng nhập (Bác sĩ, `[FUTURE / ADMIN]: Admin`).
  * `email` (VARCHAR(100), Unique, Nullable): Hòm thư công vụ.
  * `password_hash` (VARCHAR(255), Nullable): Mật khẩu băm an toàn (BCrypt/Argon2) cho Bác sĩ và Quản trị viên.
  * `role` (ENUM: `'CAREGIVER'`, `'DOCTOR'`, `'[FUTURE / ADMIN]: ADMIN'`): Vai trò tài khoản.
  * `status` (ENUM: `'ACTIVE'`, `'LOCKED'`, `'INACTIVE'`): Trạng thái tài khoản.
  * `created_at`, `updated_at` (TIMESTAMP): Dấu thời gian hệ thống.

#### 2. Thực thể `DoctorProfile` (`doctor_profiles`)
* **Mục đích:** Lưu trữ hồ sơ hành chính chuyên môn của Bác sĩ.
* **Thuộc tính:**
  * `doctor_id` (PK, FK -> `accounts.account_id`): Mã bác sĩ.
  * `full_name` (VARCHAR(100), Not Null): Họ và tên Bác sĩ.
  * `license_number` (VARCHAR(50), Unique): Số chứng chỉ hành nghề y tế `[ASSUMPTION]`.
  * `department` (VARCHAR(100)): Khoa chuyên môn (Khoa Mắt, Khúc xạ, Glaucoma...).
  * `hospital_name` (VARCHAR(150)): Tên bệnh viện / cơ sở y tế.
  * `phone_number` (VARCHAR(20)): Số điện thoại công vụ.

#### 3. Thực thể `CaregiverProfile` (`caregiver_profiles`)
* **Mục đích:** Lưu trữ hồ sơ người chăm sóc.
* **Thuộc tính:**
  * `caregiver_id` (PK, FK -> `accounts.account_id`): Mã người chăm sóc.
  * `full_name` (VARCHAR(100), Nullable): Họ tên người chăm sóc (hoàn thiện sau khi liên kết).
  * `relationship_with_patient` (VARCHAR(50), Nullable): Mối quan hệ với bệnh nhân (Con, Bố mẹ, Vợ/Chồng...).

#### 4. Thực thể `OtpVerification` (`otp_verifications`)
* **Mục đích:** Phục vụ luồng đăng nhập an toàn không mật khẩu của Caregiver (UC-01).
* **Thuộc tính:**
  * `otp_id` (PK, UUID): Định danh phiên OTP.
  * `phone_number` (VARCHAR(20), Not Null): Số điện thoại nhận OTP.
  * `otp_code` (VARCHAR(10), Not Null): Mã OTP sinh ngẫu nhiên.
  * `expired_at` (TIMESTAMP, Not Null): Thời điểm hết hạn (BR2).
  * `is_used` (BOOLEAN, Default: FALSE): Trạng thái đã xác thực hay chưa.
  * `attempt_count` (INT, Default: 0): Số lần nhập sai (chống brute-force).
  * `created_at` (TIMESTAMP): Thời gian gửi mã.

---

### 3.2 Phân Hệ Hồ Sơ Bệnh Nhân & Liên Kết

#### 5. Thực thể `PatientProfile` (`patients`)
* **Mục đích:** Lưu thông tin nhân thân, hành chính và bệnh lý của người bệnh (UC-12).
* **Thuộc tính:**
  * `patient_id` (PK, VARCHAR(30)): Mã bệnh nhân duy nhất (ví dụ: `BN-202609-001`).
  * `full_name` (VARCHAR(100), Not Null): Họ và tên đầy đủ (BR15).
  * `date_of_birth` (DATE, Not Null): Ngày tháng năm sinh (BR15).
  * `gender` (ENUM: `'MALE'`, `'FEMALE'`, `'OTHER'`, Not Null): Giới tính (BR15).
  * `phone_number` (VARCHAR(20), Nullable): Số điện thoại liên hệ bệnh nhân/người nhà.
  * `address` (TEXT, Nullable): Địa chỉ cư trú.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (Phaco, Lác, LASIK/ICL, Cắt dịch kính...) (BR15).
  * `surgery_date` (DATE, Nullable): Ngày thực hiện phẫu thuật.
  * `operated_eye` (ENUM: `'LEFT'`, `'RIGHT'`, `'BOTH'`, Not Null): Mắt phẫu thuật (Mắt Trái / Phải / Cả hai).
  * `clinical_custom_data` (JSONB, Nullable): **Trường tùy biến động theo ca mổ** (UC-12.1 Step 2) lưu các thông số đặc thù: Công suất thể thủy tinh nhân tạo IOL, độ lác trước mổ, khúc xạ trước/sau mổ, đặc điểm vết rạch giác mạc, chỉ tiêu nhãn áp ban đầu...
  * `medical_notes` (TEXT, Nullable): Ghi chú chẩn đoán lâm sàng bổ sung.
  * `status` (ENUM: `'ACTIVE'`, `'ARCHIVED'`, Default: `'ACTIVE'`): Trạng thái hồ sơ (BR25).
  * `created_by_doctor_id` (FK -> `doctor_profiles.doctor_id`, Not Null): Bác sĩ tiếp nhận.
  * `created_at`, `updated_at` (TIMESTAMP): Dấu thời gian kiểm toán.

#### 6. Thực thể `CaregiverPatientLink` (`caregiver_patient_links`)
* **Mục đích:** Lưu trữ quan hệ ủy quyền chăm sóc được xác lập sau khi quét mã QR (UC-02).
* **Thuộc tính:**
  * `link_id` (PK, UUID): Định danh liên kết.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Người chăm sóc.
  * `patient_id` (FK -> `patients.patient_id`, Not Null): Bệnh nhân được chăm sóc.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Not Null): Kế hoạch chăm sóc cụ thể được gán.
  * `linked_at` (TIMESTAMP, Not Null): Thời điểm quét mã xác nhận thành công.
  * `status` (ENUM: `'ACTIVE'`, `'REVOKED'`, Default: `'ACTIVE'`): Trạng thái liên kết (BR5).
  * `revoked_at` (TIMESTAMP, Nullable): Thời điểm hủy quyền nếu có.

---

### 3.3 Phân Hệ Master Care Plan Template (Cấu Hình Mẫu Chuẩn)

#### 7. Thực thể `CarePlanTemplate` (`care_plan_templates`)
* **Mục đích:** Gói cấu hình quy trình mẫu do bác sĩ/bệnh viện thiết lập theo bệnh học (UC-13).
* **Thuộc tính:**
  * `template_id` (PK, UUID): Định danh mẫu.
  * `template_name` (VARCHAR(150), Not Null): Tên mẫu (ví dụ: *Phác đồ Chăm sóc Hậu phẫu Phaco Chuẩn Quốc Tế*).
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật liên kết duy nhất (BR16).
  * `clinical_description` (TEXT, Nullable): Mô tả mục tiêu lâm sàng và hướng dẫn tổng quan.
  * `status` (ENUM: `'DRAFT'`, `'ACTIVE'`, `'INACTIVE'`, Default: `'DRAFT'`): Trạng thái vòng đời template.
  * `created_by_doctor_id` (FK -> `doctor_profiles.doctor_id`, Not Null): Bác sĩ phụ trách cấu hình.
  * `created_at`, `updated_at` (TIMESTAMP): Dấu thời gian.

#### 8. Thực thể `TemplateLearningModule` (`template_learning_modules`)
* **Mục đích:** Bài học hướng dẫn thao tác chăm sóc trong Learning Path của Template (UC-14).
* **Thuộc tính:**
  * `module_id` (PK, UUID): Định danh bài học.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `title` (VARCHAR(200), Not Null): Tiêu đề bài học (ví dụ: *Hướng dẫn nhỏ mắt đúng cách và tránh nhiễm khuẩn*).
  * `category` (VARCHAR(50), Not Null): Phân nhóm chủ đề (24h đầu, Vệ sinh mắt, Dinh dưỡng...).
  * `content_text` (TEXT, Not Null): Nội dung văn bản hướng dẫn chi tiết.
  * `media_type` (ENUM: `'VIDEO'`, `'IMAGE'`, `'INFOGRAPHIC'`, `'TEXT'`, Default: `'VIDEO'`): Định dạng media minh họa.
  * `media_url` (VARCHAR(500), Nullable): Đường dẫn tệp video/hình ảnh.
  * `display_order` (INT, Not Null): Thứ tự bài học trong lộ trình (UC-14.5).
  * `created_at`, `updated_at` (TIMESTAMP).

#### 9. Thực thể `TemplateQuizQuestion` (`template_quiz_questions`)
* **Mục đích:** 3 câu hỏi trắc nghiệm kiểm tra nhanh kiến thức gắn ở cuối mỗi bài học (UC-03, UC-14.1).
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
* **Mục đích:** Cấu hình danh mục thuốc mẫu cho loại phẫu thuật (UC-15).
* **Thuộc tính:**
  * `medication_template_id` (PK, UUID): Định danh thuốc mẫu.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (UC-15.1 Step 2).
  * `drug_name` (VARCHAR(150), Not Null): Tên biệt dược, hoạt chất và hàm lượng (ví dụ: *Tobradex 5ml*).
  * `drug_form` (ENUM: `'EYE_DROP'`, `'ORAL'`, Not Null): Thuốc nhỏ mắt hay thuốc uống.
  * `dosage` (VARCHAR(50), Not Null): Liều dùng (1 giọt, 1 viên...).
  * `frequency` (VARCHAR(50), Not Null): Tần suất dùng (4 lần/ngày...).
  * `times_of_day` (JSONB / VARCHAR(100), Not Null): Các cữ trong ngày (Sáng, Trưa, Chiều, Tối).
  * `meal_relation` (ENUM: `'BEFORE_MEAL'`, `'AFTER_MEAL'`, `'NONE'`, Default: `'NONE'`): Uống trước/sau ăn.
  * `duration_days` (INT, Not Null): Số ngày dùng dự kiến.
  * `order_index` (INT, Not Null): Thứ tự dùng thuốc trong cữ (quan trọng với thuốc nhỏ mắt).
  * `visual_identification` (TEXT, Not Null): **Mô tả nhận diện về thuốc** (Màu sắc vỏ, nắp lọ, viên nén/nang, độ trong đục dung dịch) (UC-15.1 Step 2).
  * `clinical_cautions` (TEXT, Not Null): **Lưu ý về loại thuốc** (Lắc kỹ trước khi dùng, bảo quản ngăn mát tủ lạnh, giãn cách tối thiểu 5 phút...) (BR23, UC-15.1 Step 2).
  * `created_at`, `updated_at` (TIMESTAMP).

#### 11. Thực thể `TemplateRecoveryMilestone` (`template_recovery_milestones`)
* **Mục đích:** Thiết lập các mốc thời gian khảo sát phục hồi chuẩn trong template (UC-16).
* **Thuộc tính:**
  * `milestone_template_id` (PK, UUID): Định danh mốc mẫu.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (UC-16.1 Step 2).
  * `milestone_name` (VARCHAR(50), Not Null): Tên mốc hiển thị (ví dụ: *Ngày 1*, *Ngày 3*, *Ngày 7*).
  * `days_post_op` (INT, Not Null): Số ngày sau phẫu thuật cần kích hoạt khảo sát (1, 3, 7, 14...).
  * `description` (TEXT, Nullable): Ý nghĩa lâm sàng của mốc theo dõi.

#### 12. Thực thể `TemplateRecoveryQuestion` (`template_recovery_questions`)
* **Mục đích:** Bộ câu hỏi 3–5 câu của từng mốc phục hồi (UC-16).
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
* **Mục đích:** Thiết lập các dấu hiệu nguy hiểm và phản ứng khẩn cấp (UC-17).
* **Thuộc tính:**
  * `red_flag_template_id` (PK, UUID): Định danh tiêu chí Red Flag.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (UC-17.1 Step 2).
  * `sign_name` (VARCHAR(150), Not Null): Tên dấu hiệu (ví dụ: *Đau nhức dữ dội lan nửa đầu, Đột ngột mờ mắt*).
  * `warning_level` (ENUM: `'EMERGENCY'`, `'SAME_DAY_EXAM'`, Default: `'EMERGENCY'`): Mức độ cảnh báo (Cấp cứu khẩn cấp / Khám trong ngày).
  * `trigger_condition` (TEXT, Nullable): Điều kiện kích hoạt từ kết quả Recovery Check.
  * `first_aid_instructions` (TEXT, Not Null): Các bước sơ cứu cần làm ngay (không dụi mắt, đeo khiên bảo vệ...).
  * `emergency_hotline` (VARCHAR(20), Not Null): Số điện thoại đường dây nóng 24/7 của bệnh viện (BR11, UC-17.1).
  * `order_index` (INT, Default: 1).

#### 14. Thực thể `TemplateDoDontItem` (`template_do_dont_items`)
* **Mục đích:** Danh mục hướng dẫn sinh hoạt Nên làm / Cần tránh trong template (UC-18).
* **Thuộc tính:**
  * `do_dont_template_id` (PK, UUID): Định danh chỉ dẫn mẫu.
  * `template_id` (FK -> `care_plan_templates.template_id`, Not Null): Thuộc template nào.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật áp dụng (UC-18.1 Step 2).
  * `item_type` (ENUM: `'DO'`, `'DONT'`, Not Null): Phân loại (NÊN LÀM / CẦN TRÁNH) (BR22).
  * `category` (ENUM: `'HYGIENE'`, `'ACTIVITY'`, `'DIET'`, `'SLEEP'`, Not Null): Nhóm sinh hoạt (Vệ sinh, Vận động, Ăn uống, Giấc ngủ).
  * `behavior_title` (VARCHAR(200), Not Null): Tên hành vi (ví dụ: *Đeo khiên bảo vệ mắt khi đi ngủ*).
  * `clinical_explanation` (TEXT, Not Null): Giải thích y khoa tại sao nên làm hoặc cần tránh.
  * `applicable_duration` (VARCHAR(100), Not Null): Khoảng thời gian áp dụng (ví dụ: *7 ngày đầu sau mổ*).
  * `order_index` (INT, Default: 1).

---

### 3.4 Phân Hệ Kế Hoạch Chăm Sóc Bệnh Nhân Thực Tế (Patient Care Plan Instance)

#### 15. Thực thể `PatientCarePlan` (`patient_care_plans`)
* **Mục đích:** Thể hiện Kế hoạch chăm sóc độc lập được nhân bản từ Template để gán riêng cho từng bệnh nhân (UC-19, BR18, BR19).
* **Thuộc tính:**
  * `care_plan_id` (PK, UUID): Định danh Care Plan của bệnh nhân.
  * `patient_id` (FK -> `patients.patient_id`, Not Null): Thuộc hồ sơ bệnh nhân nào.
  * `source_template_id` (FK -> `care_plan_templates.template_id`, Not Null): Template gốc được nhân bản.
  * `surgery_type` (VARCHAR(50), Not Null): Loại phẫu thuật tại thời điểm gán.
  * `status` (ENUM: `'DRAFT'`, `'ACTIVE'`, `'COMPLETED'`, `'ARCHIVED'`, Default: `'DRAFT'`): Trạng thái kế hoạch (BR19: tối đa 1 Active/bệnh nhân).
  * `activated_at` (TIMESTAMP, Nullable): Mốc thời gian bác sĩ bấm kích hoạt Care Plan.
  * `completed_at` (TIMESTAMP, Nullable): Mốc thời gian hoàn thành đợt theo dõi.
  * `created_by_doctor_id` (FK -> `doctor_profiles.doctor_id`, Not Null): Bác sĩ chỉ định.
  * `created_at`, `updated_at` (TIMESTAMP).

#### 16. Thực thể `PatientMedication` (`patient_medications`)
* **Mục đích:** Đơn thuốc thực tế của bệnh nhân (được tùy biến liều lượng, loại thuốc từ template khi tạo Care Plan) (UC-06, UC-19).
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
* **Mục đích:** Lịch hẹn tái khám cụ thể do bác sĩ thiết lập trong Care Plan (UC-07, UC-19).
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
* **Mục đích:** Lưu trữ mã định danh bảo mật QR in trên phiếu xuất viện cho Caregiver quét liên kết (UC-02, UC-20, BR4, BR20).
* **Thuộc tính:**
  * `qr_id` (PK, UUID): Định danh bản ghi mã QR.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Unique, Not Null): Gắn với Care Plan cụ thể.
  * `qr_token` (VARCHAR(255), Unique, Not Null): Chuỗi mã hóa token ngẫu nhiên bảo mật (HMAC/UUID).
  * `issued_by_doctor_id` (FK -> `doctor_profiles.doctor_id`, Not Null): Bác sĩ phát hành.
  * `issued_at` (TIMESTAMP, Not Null): Thời điểm sinh mã.
  * `status` (ENUM: `'ACTIVE'`, `'REVOKED'`, Default: `'ACTIVE'`): Trạng thái mã QR.
  * `print_count` (INT, Default: 1): Số lần in ấn lại phiếu (UC-20 A1).

---

### 3.5 Phân Hệ Giao Dịch & Ghi Nhận Sự Kiện Thực Tế (Operational & Transaction Logs)

#### 19. Thực thể `MedicationLog` (`medication_logs`)
* **Mục đích:** Ghi nhận sự kiện Caregiver xác nhận cho bệnh nhân uống/nhỏ thuốc (UC-06).
* **Thuộc tính:**
  * `log_id` (PK, UUID): Định danh nhật ký dùng thuốc.
  * `patient_medication_id` (FK -> `patient_medications.patient_medication_id`, Not Null): Loại thuốc nào.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Ai xác nhận.
  * `scheduled_time` (TIMESTAMP, Not Null): Giờ quy định dùng thuốc theo lịch.
  * `confirmed_at` (TIMESTAMP, Not Null): Thời gian thực tế nhấn "Đánh dấu đã dùng".
  * `status` (ENUM: `'TAKEN'`, `'SKIPPED'`, Default: `'TAKEN'`): Trạng thái cữ thuốc.
  * `notes` (VARCHAR(255), Nullable): Ghi chú của người chăm sóc nếu có phản ứng phụ nhẹ.

#### 20. Thực thể `CaregiverQuizSubmission` (`caregiver_quiz_submissions`)
* **Mục đích:** Lưu kết quả kiểm tra 3 câu Mini Quiz sau khi xem xong bài học Learning Path (UC-03, BR8).
* **Thuộc tính:**
  * `submission_id` (PK, UUID): Định danh lượt nộp bài.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Người làm bài.
  * `module_id` (FK -> `template_learning_modules.module_id`, Not Null): Thuộc bài học nào.
  * `correct_answers_count` (INT, Not Null): Số câu trả lời đúng (ví dụ: 3/3 hoặc 2/3).
  * `total_questions` (INT, Default: 3): Tổng số câu hỏi kiểm tra.
  * `answers_detail` (JSONB, Not Null): Chi tiết các phương án Caregiver đã chọn.
  * `submitted_at` (TIMESTAMP, Not Null): Thời điểm nộp bài.

#### 21. Thực thể `RecoveryCheckSubmission` (`recovery_check_submissions`)
* **Mục đích:** Lưu kết quả mỗi lượt Caregiver trả lời bảng kiểm phục hồi định kỳ (UC-08, UC-21).
* **Thuộc tính:**
  * `submission_id` (PK, UUID): Định danh lượt nộp bảng kiểm.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Not Null): Thuộc Care Plan nào.
  * `milestone_template_id` (FK -> `template_recovery_milestones.milestone_template_id`, Not Null): Tại mốc ngày nào.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Người nộp.
  * `submitted_at` (TIMESTAMP, Not Null): Thời điểm nộp bảng kiểm.
  * `overall_status` (ENUM: `'NORMAL'`, `'NEEDS_ATTENTION'`, `'RED_FLAG'`, Not Null): Đánh giá tổng quan tự động phân loại (BR10).
  * `doctor_viewed` (BOOLEAN, Default: FALSE): Bác sĩ đã xem xét hay chưa (UC-21).
  * `doctor_viewed_at` (TIMESTAMP, Nullable): Mốc thời gian bác sĩ mở xem.
  * `doctor_notes` (TEXT, Nullable): Ghi chú chuyên môn của bác sĩ sau khi xem xét.

#### 22. Thực thể `RecoveryCheckAnswer` (`recovery_check_answers`)
* **Mục đích:** Chi tiết từng câu trả lời trong bảng kiểm phục hồi (UC-08, UC-21).
* **Thuộc tính:**
  * `answer_id` (PK, UUID): Định danh câu trả lời.
  * `submission_id` (FK -> `recovery_check_submissions.submission_id`, Not Null): Thuộc lượt nộp nào.
  * `question_template_id` (FK -> `template_recovery_questions.question_template_id`, Not Null): Trả lời cho câu hỏi nào.
  * `answer_value` (VARCHAR(100), Not Null): Giá trị Caregiver đã chọn (Có/Không, Lựa chọn A/B...).
  * `flag_level` (ENUM: `'NORMAL'`, `'NEEDS_ATTENTION'`, `'RED_FLAG'`, Not Null): Mức độ cảnh báo tự động gắn cờ cho câu hỏi này (BR10).

#### 23. Thực thể `RedFlagIncident` (`red_flag_incidents`)
* **Mục đích:** Lưu vết các sự kiện khẩn cấp xảy ra với bệnh nhân để phục vụ y khoa và pháp lý (UC-09, UC-21).
* **Thuộc tính:**
  * `incident_id` (PK, UUID): Định danh sự kiện khẩn cấp.
  * `care_plan_id` (FK -> `patient_care_plans.care_plan_id`, Not Null): Bệnh nhân liên quan.
  * `caregiver_id` (FK -> `caregiver_profiles.caregiver_id`, Not Null): Người phát hiện/thực hiện.
  * `trigger_source` (ENUM: `'RECOVERY_CHECK'`, `'MANUAL_BUTTON'`, Not Null): Nguồn kích hoạt cảnh báo (từ bảng kiểm hay do bấm nút báo động).
  * `red_flag_template_id` (FK -> `template_red_flags.red_flag_template_id`, Nullable): Dấu hiệu nguy hiểm cụ thể nếu xác định được.
  * `triggered_at` (TIMESTAMP, Not Null): Thời điểm phát sinh sự kiện.
  * `call_initiated` (BOOLEAN, Default: FALSE): Caregiver đã bấm gọi đường dây nóng cấp cứu hay chưa (UC-09 Step 3).
  * `call_initiated_at` (TIMESTAMP, Nullable): Thời gian bấm gọi.
  * `acknowledged_by_doctor_id` (FK -> `doctor_profiles.doctor_id`, Nullable): Bác sĩ tiếp nhận xử lý.
  * `acknowledged_at` (TIMESTAMP, Nullable): Mốc thời gian tiếp nhận.
  * `clinical_resolution` (TEXT, Nullable): Kết luận xử trí y tế (ví dụ: *Bệnh nhân đã đến viện cấp cứu kịp thời*).

---

### 3.6 Phân Hệ Mở Rộng Quản Trị `[FUTURE / ADMIN]`

#### 24. Thực thể `AuditLog` (`audit_logs`) `[FUTURE / ADMIN]`
* **Mục đích:** Phục vụ tương lai cho Quản trị viên (Admin) quản lý nhật ký kiểm toán (CRUD Audit Log) và đảm bảo an toàn pháp lý y tế (BR5, BR20).
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
| `UserAccount` | **1 : 1** | Có hồ sơ chuyên môn | `DoctorProfile` | Một tài khoản Bác sĩ gắn với duy nhất một hồ sơ bác sĩ |
| `UserAccount` | **1 : 1** | Có hồ sơ người chăm sóc | `CaregiverProfile` | Một tài khoản Caregiver gắn với một hồ sơ người chăm sóc |
| `DoctorProfile` | **1 : N** | Tiếp nhận & quản lý | `PatientProfile` | Một Bác sĩ tạo và quản lý nhiều hồ sơ bệnh nhân |
| `DoctorProfile` | **1 : N** | Biên soạn | `CarePlanTemplate` | Một Bác sĩ có thể cấu hình nhiều Care Plan Template mẫu |
| `DoctorProfile` | **1 : N** | Phụ trách khám lại | `PatientFollowupAppointment` | Một Bác sĩ có thể phụ trách nhiều buổi hẹn tái khám |
| `DoctorProfile` | **1 : N** | Phát hành mã | `PatientQRCode` | Một Bác sĩ phát hành mã QR cho nhiều bệnh nhân |
| `DoctorProfile` | **0..1 : N** | Tiếp nhận xử lý | `RedFlagIncident` | Bác sĩ tiếp nhận ca cấp cứu (có thể chưa có bác sĩ tiếp nhận) |
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
| `CaregiverProfile` | **N : M** | Chăm sóc & Theo dõi | `PatientProfile` | Một Caregiver có thể chăm sóc nhiều bệnh nhân (qua `CaregiverPatientLink`) |
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

---

## 5. Ánh Xạ Quy Tắc Nghiệp Vụ Vào Dữ Liệu (Business Rules Mapping)

| Mã Quy Tắc | Tên Quy Tắc Nghiệp Vụ | Ràng Buộc Dữ Liệu Cụ Thể (Database Constraints & Logic) |
|---|---|---|
| **BR1, BR2** | Xác thực SĐT & OTP | `OtpVerification`: `expired_at > NOW()`, `is_used = FALSE`, khóa tạm nếu `attempt_count > 5`. |
| **BR4, BR20** | Tính hợp lệ & bảo mật QR | `patient_qr_codes.status = 'ACTIVE'` VÀ `patient_care_plans.status = 'ACTIVE'`. `qr_token` mã hóa ngẫu nhiên không thể đoán trước. |
| **BR5** | Toàn vẹn Caregiver - Bệnh nhân | Bảng trung gian `caregiver_patient_links` lưu `caregiver_id`, `patient_id`, `linked_at` để truy vết ủy quyền rõ ràng. |
| **BR6, BR7** | Nguồn gốc nội dung y khoa | Toàn bộ các bảng `template_*` và `patient_*` bắt buộc tham chiếu khóa ngoại đến `doctor_id` người tạo, không cho phép nội dung vô chủ (orphaned data). |
| **BR8** | Quiz không chặn chức năng | Kết quả bài kiểm tra lưu ở `caregiver_quiz_submissions` độc lập, không đặt cờ khóa quyền truy cập của bệnh nhân. |
| **BR9** | Bắt buộc hoàn thành 3–5 câu | Giao dịch nộp bảng kiểm chỉ hợp lệ khi số bản ghi `recovery_check_answers` bằng đúng số câu hỏi bắt buộc của mốc đó. |
| **BR10** | Tự động phân loại lâm sàng | `overall_status` trong `recovery_check_submissions` được tính tự động dựa trên mức độ cảnh báo cao nhất của các câu trả lời (`RED_FLAG` > `NEEDS_ATTENTION` > `NORMAL`). |
| **BR11** | Ưu tiên quy trình khẩn cấp | Khi `overall_status = 'RED_FLAG'`, hệ thống tự động sinh bản ghi trong `red_flag_incidents` và kích hoạt luồng khẩn cấp. |
| **BR14** | Phân quyền bác sĩ | Ràng buộc khóa ngoại `created_by_doctor_id` phải trỏ về tài khoản có `role = 'DOCTOR'` và `status = 'ACTIVE'`. |
| **BR15** | Thông tin bệnh nhân bắt buộc | Cột `full_name`, `date_of_birth`, `gender`, `surgery_type`, `operated_eye` đều mang ràng buộc `NOT NULL` tại bảng `patients`. |
| **BR16** | Ràng buộc loại phẫu thuật | Cột `surgery_type` bắt buộc có giá trị xác định tại bảng `care_plan_templates` và các bảng cấu phần con. |
| **BR17, BR18** | Tính cô lập của Care Plan | Phân chia thành 2 cấu trúc bảng riêng biệt: Master Template (`template_medications`, `template_recovery_questions`...) và Patient Care Plan (`patient_medications`...). Sửa đổi ở Master Template hoàn toàn không làm biến đổi bản ghi trong Patient Care Plan đã tạo. |
| **BR19** | Duy nhất 1 Care Plan Active | Tạo chỉ mục độc nhất có điều kiện (Partial Unique Index) trên `patient_care_plans`: `UNIQUE(patient_id) WHERE status = 'ACTIVE'`. |
| **BR22** | Danh mục Do & Don't | `template_do_dont_items.item_type` bắt buộc thuộc ENUM (`'DO'`, `'DONT'`) và có mốc thời gian áp dụng rõ ràng. |
| **BR23** | Khoảng cách nhỏ mắt 5 phút | `patient_medications.clinical_cautions` lưu chỉ dẫn thao tác; giao diện căn cứ vào trường `drug_form = 'EYE_DROP'` và `order_index` để đếm lùi thời gian giãn cách giữa các thuốc nhỏ mắt. |
| **BR24** | Nhắc hẹn tái khám 24h & 2h | Hai cờ `reminder_24h_sent` và `reminder_2h_sent` tại bảng `patient_followup_appointments` để điều khiển dịch vụ gửi thông báo đẩy. |
| **BR25** | Xóa mềm hồ sơ bệnh nhân | Bảng `patients` áp dụng `status = 'ARCHIVED'` (Soft Delete), ngăn chặn câu lệnh `DELETE` cứng nếu bệnh nhân đã phát sinh liên kết QR hoặc Care Plan. |

---

## 6. Ma Trận CRUD Dữ Liệu Theo Từng Use Case (Data CRUD Matrix)

Ký hiệu: **C** = Create (Tạo mới), **R** = Read (Đọc/Xem), **U** = Update (Cập nhật), **D** = Delete/Archive (Xóa/Lưu trữ).

| Mã Use Case | Tên Use Case | Bảng Dữ Liệu Tác Động | Hành Động CRUD | Ghi Chú Nghiệp Vụ |
|---|---|---|:---:|---|
| **UC-01** | Đăng nhập Caregiver | `accounts`, `otp_verifications` | **C, R, U** | Đọc tài khoản, tạo mã OTP mới, cập nhật `is_used = TRUE` |
| **UC-02** | Quét QR liên kết bệnh nhân | `patient_qr_codes`, `caregiver_patient_links` | **R, C** | Đọc & xác thực `qr_token`, tạo mới bản ghi liên kết ủy quyền |
| **UC-03** | Learning Path & Mini Quiz | `template_learning_modules`, `template_quiz_questions`, `caregiver_quiz_submissions` | **R, C** | Đọc bài học & câu hỏi trắc nghiệm, tạo bản ghi kết quả nộp bài |
| **UC-05** | Xem hướng dẫn Do & Don't | `template_do_dont_items` | **R** | Đọc danh mục hành vi Nên làm / Cần tránh |
| **UC-06** | Lịch dùng thuốc & Nhắc nhở | `patient_medications`, `medication_logs` | **R, C** | Đọc đơn thuốc, tạo bản ghi xác nhận cữ uống thuốc thực tế |
| **UC-07** | Xem lịch tái khám & Nhắc nhở | `patient_followup_appointments` | **R, U** | Đọc thông tin lịch hẹn, cập nhật cờ nhắc nhở đã gửi |
| **UC-08** | Nộp bảng kiểm Recovery Check | `template_recovery_questions`, `recovery_check_submissions`, `recovery_check_answers`, `red_flag_incidents` | **R, C** | Đọc câu hỏi, lưu bản ghi tổng quan và chi tiết câu trả lời; nếu có Red Flag thì tạo sự kiện khẩn cấp |
| **UC-09** | Xử lý khẩn cấp Red Flag | `template_red_flags`, `red_flag_incidents` | **R, C, U** | Đọc hướng dẫn sơ cứu & hotline, cập nhật thời gian bấm gọi |
| **UC-11** | Đăng nhập Bác sĩ | `accounts`, `doctor_profiles` | **R** | Đối chiếu thông tin đăng nhập, xác thực vai trò `DOCTOR` |
| **UC-12.1** | Tạo mới hồ sơ bệnh nhân | `patients` | **C** | Lưu bệnh nhân mới kèm `clinical_custom_data (JSONB)` động |
| **UC-12.2** | Xem danh sách & tìm kiếm BN | `patients`, `patient_care_plans`, `caregiver_patient_links` | **R** | Đọc danh sách bệnh nhân, trạng thái Care Plan và Caregiver liên kết theo bộ lọc |
| **UC-12.3** | Xem chi tiết hồ sơ bệnh nhân | `patients`, `patient_care_plans`, `caregiver_patient_links`, `caregiver_profiles`, `recovery_check_submissions` | **R** | Đọc toàn diện hồ sơ bệnh nhân, Care Plan, danh sách Caregiver liên kết và lịch sử phục hồi |
| **UC-12.4** | Chỉnh sửa hồ sơ bệnh nhân | `patients` | **U** | Cập nhật thông tin bệnh nhân, mắt mổ, dữ liệu lâm sàng tùy biến |
| **UC-12.5** | Xóa/Lưu trữ hồ sơ bệnh nhân | `patients` | **U / D** | Cập nhật `status = 'ARCHIVED'` (xóa mềm), chặn nếu có Care Plan Active |
| **UC-13.1** | Tạo mới Care Plan Template | `care_plan_templates`, các bảng `template_*` | **C** | Lưu thông tin master template và các cấu phần con ở trạng thái Draft |
| **UC-13.2** | Xem danh sách Care Plan Template | `care_plan_templates` | **R** | Đọc danh sách các gói mẫu theo loại phẫu thuật |
| **UC-13.3** | Xem chi tiết Care Plan Template | `care_plan_templates`, các bảng `template_*` | **R** | Đọc toàn diện cấu hình 5 tab thành phần con |
| **UC-13.4** | Chỉnh sửa Care Plan Template | `care_plan_templates`, các bảng `template_*` | **U, C, D** | Cập nhật thông tin chung và cấu hình trực tiếp trên 5 tab con |
| **UC-13.5** | Kích hoạt / Vô hiệu hóa Template| `care_plan_templates` | **U** | Đổi trạng thái sang `ACTIVE` hoặc `INACTIVE` |
| **UC-14.1** | Thêm bài học & Quiz trắc nghiệm | `template_learning_modules`, `template_quiz_questions` | **C** | Thêm bài học mới và 3 câu hỏi trắc nghiệm kèm đáp án |
| **UC-14.2** | Xem danh sách bài học | `template_learning_modules`, `template_quiz_questions` | **R** | Đọc danh sách bài học và trạng thái bộ câu hỏi Mini Quiz |
| **UC-14.3** | Sửa bài học & Quiz trắc nghiệm | `template_learning_modules`, `template_quiz_questions` | **U** | Chỉnh sửa văn bản, media hoặc câu hỏi/đáp án/giải thích y khoa |
| **UC-14.4** | Xóa bài học khỏi lộ trình | `template_learning_modules`, `template_quiz_questions` | **D** | Xóa bài học và cascade xóa 3 câu hỏi trắc nghiệm đi kèm |
| **UC-14.5** | Sắp xếp thứ tự bài học | `template_learning_modules` | **U** | Cập nhật chỉ số `display_order` |
| **UC-15.1** | Thêm thuốc vào Medication Template | `template_medications` | **C** | Bổ sung thuốc mới kèm loại phẫu thuật, nhận diện & lưu ý |
| **UC-15.2** | Xem danh sách thuốc mẫu | `template_medications` | **R** | Đọc bảng danh mục thuốc kèm nhận diện và lưu ý lâm sàng |
| **UC-15.3** | Sửa thông tin thuốc mẫu | `template_medications` | **U** | Cập nhật liều dùng, cữ uống, nhận diện thuốc hoặc lưu ý sử dụng |
| **UC-15.4** | Xóa thuốc khỏi template | `template_medications` | **D** | Xóa loại thuốc mẫu khỏi phác đồ |
| **UC-16.1** | Tạo mốc & câu hỏi Recovery Check | `template_recovery_milestones`, `template_recovery_questions` | **C** | Thêm mốc theo dõi kèm loại phẫu thuật và 3–5 câu hỏi khảo sát |
| **UC-16.2** | Xem mốc & câu hỏi Recovery Check | `template_recovery_milestones`, `template_recovery_questions` | **R** | Đọc danh mục mốc ngày và chi tiết câu hỏi theo dõi |
| **UC-16.3** | Sửa mốc & câu hỏi Recovery Check | `template_recovery_milestones`, `template_recovery_questions` | **U** | Tinh chỉnh mốc ngày, câu hỏi hoặc tiêu chí cảnh báo Red Flag |
| **UC-16.4** | Xóa mốc / câu hỏi Recovery Check | `template_recovery_milestones`, `template_recovery_questions` | **D** | Xóa mốc hoặc câu hỏi khảo sát khỏi template |
| **UC-17.1** | Thêm dấu hiệu Red Flag | `template_red_flags` | **C** | Lưu dấu hiệu nguy hiểm kèm loại phẫu thuật, sơ cứu & hotline |
| **UC-17.2** | Xem danh sách Red Flag | `template_red_flags` | **R** | Đọc bảng danh mục dấu hiệu cảnh báo khẩn cấp |
| **UC-17.3** | Sửa dấu hiệu Red Flag | `template_red_flags` | **U** | Chỉnh sửa hướng dẫn sơ cứu hoặc số hotline bệnh viện |
| **UC-17.4** | Xóa dấu hiệu Red Flag | `template_red_flags` | **D** | Xóa tiêu chí cảnh báo khỏi cấu hình template |
| **UC-18.1** | Thêm mục Do & Don't | `template_do_dont_items` | **C** | Thêm chỉ dẫn Nên làm/Cần tránh kèm loại phẫu thuật, lý do y khoa |
| **UC-18.2** | Xem danh sách Do & Don't | `template_do_dont_items` | **R** | Đọc bảng chỉ dẫn chia 2 nhóm Xanh (Do) và Đỏ (Don't) |
| **UC-18.3** | Sửa mục Do & Don't | `template_do_dont_items` | **U** | Cập nhật câu từ chỉ dẫn hoặc thời gian áp dụng |
| **UC-18.4** | Xóa mục Do & Don't | `template_do_dont_items` | **D** | Loại bỏ chỉ dẫn khỏi danh mục mẫu |
| **UC-19** | Tạo & tùy biến Care Plan BN | `patient_care_plans`, `patient_medications`, `patient_followup_appointments` | **C, U** | Nhân bản từ template gốc sang Care Plan riêng của bệnh nhân, tùy biến liều thuốc và lập lịch tái khám |
| **UC-20** | Tạo mã QR cho bệnh nhân | `patient_qr_codes` | **C, U** | Sinh token QR mã hóa gắn với Care Plan Active, cập nhật lượt in |
| **UC-21** | Theo dõi Recovery Check của BN | `recovery_check_submissions`, `recovery_check_answers`, `red_flag_incidents` | **R, U** | Đọc danh sách lượt nộp bảng kiểm, xem câu trả lời, đánh dấu `doctor_viewed = TRUE` |

---

## 7. Đánh Giá Khả Năng Mở Rộng Cho Admin Tương Lai `[FUTURE / ADMIN]`

Để chuẩn bị cho 2 nhóm tính năng của Admin mà không làm thay đổi hay phá vỡ thiết kế hiện tại:

1. **Khả năng mở rộng cho Phân hệ Quản lý Tài khoản (Account Management):**
   * Thực thể `UserAccount` (`accounts`) đã được thiết kế sẵn trường `role` hỗ trợ giá trị `'ADMIN'` cùng với `'DOCTOR'` và `'CAREGIVER'`.
   * Bác sĩ và Quản trị viên sử dụng chung cơ chế chứng thực tài khoản có mật khẩu (`password_hash`), cho phép Admin tương lai thực hiện đầy đủ các thao tác: Tạo tài khoản Bác sĩ mới, Đổi mật khẩu, Khóa/Mở khóa tài khoản mà không cần can thiệp tái cấu trúc bảng.

2. **Khả năng mở rộng cho Phân hệ Quản lý Nhật ký Kiểm toán (Audit Log Management):**
   * Thực thể `AuditLog` (`audit_logs`) `[FUTURE / ADMIN]` đã được thiết lập với đầy đủ các trường lưu vết: Người thao tác (`user_id`), loại hành động (`action_type`), bảng dữ liệu (`target_entity`), khóa bản ghi (`target_id`), giá trị cũ (`old_data`), giá trị mới (`new_data`) dạng JSONB và dấu thời gian.
   * Khi Admin tương lai được triển khai, màn hình CRUD Audit Log chỉ cần truy vấn trực tiếp trên bảng `audit_logs` này mà không đòi hỏi thêm bất kỳ bảng phụ trợ nào.
