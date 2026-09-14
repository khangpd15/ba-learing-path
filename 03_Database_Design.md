# 03. Thiết Kế Cơ Sở Dữ Liệu Vật Lý (Physical Database Design)

> **Dự án:** Hệ Thống Hướng Dẫn & Theo Dõi Chăm Sóc Bệnh Nhân Hậu Phẫu Mắt (Post-Op Eye Care Platform)  
> **Tài liệu tham chiếu:** [01_Database_Analysis.md](file:///d:/DOC_BA/docs_of_projects/01_Database_Analysis.md) | [02_ERD.md](file:///d:/DOC_BA/docs_of_projects/02_ERD.md)  
> **Hệ quản trị CSDL mục tiêu:** PostgreSQL 15+ / Quan hệ chuẩn hóa  
> **Người thực hiện:** Đội ngũ BA  
> **Ngày lập:** 11/09/2026 | **Trạng thái:** Bản chuẩn hóa thiết kế vật lý (Baseline)

---

## 1. Nguyên Tắc Thiết Kế Vật Lý

1. **Chuẩn hóa dữ liệu (Normalization):** Đạt chuẩn 3NF đối với các thực thể giao dịch và nghiệp vụ chuẩn để tránh dư thừa dữ liệu và bất thường khi cập nhật.
2. **Xử lý linh hoạt trường lâm sàng tùy biến (Dynamic Clinical Data):** Sử dụng kiểu dữ liệu `JSONB` cho các trường dữ liệu lâm sàng biến đổi theo ca mổ (Phaco, Lác, LASIK/ICL, Cắt dịch kính...) để không phải tạo quá nhiều bảng con thưa thớt (sparse tables) gây phức tạp hệ thống.
3. **Hiệu năng cao cho truy vấn thời gian thực:** Đánh chỉ mục (Index) trên các trường tra cứu tần suất cao (Mã token QR, số điện thoại đăng nhập, mức độ cảnh báo Red Flag, tìm kiếm bệnh nhân).
4. **Bảo mật & Toàn vẹn y tế:** Ràng buộc chặt chẽ khóa ngoại, áp dụng xóa mềm (*Soft Delete*) cho hồ sơ bệnh nhân, sẵn sàng cấu trúc kiểm toán `[FUTURE / ADMIN]`.

---

## 2. Đặc Tả Chi Tiết Các Bảng Dữ Liệu (Table Schema Specifications)

---

### PHÂN HỆ I: NGƯỜI DÙNG & ĐỊNH DANH (AUTHENTICATION & USERS)

#### 1. Bảng `accounts` (Tài khoản người dùng tập trung)
Lưu trữ thông tin xác thực cho Caregiver, Doctor và Quản trị viên `[FUTURE / ADMIN]`.

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `account_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính định danh tài khoản duy nhất |
| `phone_number` | `VARCHAR(20)` | Unique, Nullable | NULL | Số điện thoại dùng đăng nhập OTP (Caregiver/Doctor) |
| `username` | `VARCHAR(50)` | Unique, Nullable | NULL | Tên đăng nhập (Bác sĩ, `[FUTURE / ADMIN]: Admin`) |
| `email` | `VARCHAR(100)` | Unique, Nullable | NULL | Email liên hệ công vụ |
| `password_hash` | `VARCHAR(255)` | Nullable | NULL | Mật khẩu mã hóa BCrypt/Argon2 (Bác sĩ, Admin) |
| `role` | `VARCHAR(20)` | Not Null | `'CAREGIVER'` | ENUM: `'CAREGIVER'`, `'DOCTOR'`, `'ADMIN'` `[FUTURE / ADMIN]` |
| `status` | `VARCHAR(20)` | Not Null | `'ACTIVE'` | ENUM: `'ACTIVE'`, `'LOCKED'`, `'INACTIVE'` (BR3, BR14) |
| `created_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm tạo tài khoản |
| `updated_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm cập nhật cuối cùng |

* **Chỉ mục (Indexes):**
  * `idx_accounts_phone` (B-Tree) trên `phone_number`: Tối ưu hóa đăng nhập OTP (UC-01).
  * `idx_accounts_username` (B-Tree) trên `username`: Tối ưu hóa đăng nhập Bác sĩ (UC-11).

---

#### 2. Bảng `doctor_profiles` (Hồ sơ Bác sĩ điều trị)
Thông tin chuyên môn của Bác sĩ phụ trách lâm sàng.

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `doctor_id` | `UUID` | **PK**, **FK** -> `accounts(account_id)` | — | Khóa chính đồng thời là khóa ngoại trỏ về `accounts` |
| `full_name` | `VARCHAR(100)` | Not Null | — | Họ và tên Bác sĩ |
| `license_number` | `VARCHAR(50)` | Unique, Not Null | — | Số chứng chỉ hành nghề y khoa `[ASSUMPTION]` |
| `department` | `VARCHAR(100)` | Not Null | `'Khoa Mắt'` | Chuyên khoa công tác (Khúc xạ, Đục thủy tinh thể...) |
| `hospital_name` | `VARCHAR(150)` | Not Null | — | Tên bệnh viện / trung tâm y tế |
| `phone_number` | `VARCHAR(20)` | Nullable | — | Số điện thoại liên hệ chuyên môn |

---

#### 3. Bảng `caregiver_profiles` (Hồ sơ Người chăm sóc)
Thông tin của thân nhân / người chăm sóc trực tiếp bệnh nhân.

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `caregiver_id` | `UUID` | **PK**, **FK** -> `accounts(account_id)` | — | Khóa chính đồng thời là khóa ngoại trỏ về `accounts` |
| `full_name` | `VARCHAR(100)` | Nullable | NULL | Họ và tên người chăm sóc |
| `relationship_with_patient` | `VARCHAR(50)` | Nullable | NULL | Mối quan hệ với bệnh nhân (Con, Bố mẹ, Vợ/Chồng...) |

---

#### 4. Bảng `otp_verifications` (Mã xác thực OTP đăng nhập)
Quản lý vòng đời mã OTP gửi qua tin nhắn SMS (UC-01).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `otp_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính phiên OTP |
| `phone_number` | `VARCHAR(20)` | Not Null | — | Số điện thoại nhận mã |
| `otp_code` | `VARCHAR(10)` | Not Null | — | Mã OTP sinh ngẫu nhiên (ví dụ 6 chữ số) |
| `expired_at` | `TIMESTAMPTZ` | Not Null | — | Thời gian hết hạn hiệu lực (BR2: thường 3–5 phút) |
| `is_used` | `BOOLEAN` | Not Null | `FALSE` | Trạng thái đã sử dụng xác thực |
| `attempt_count` | `INT` | Not Null | `0` | Số lần nhập sai mã (tối đa 5 lần) |
| `created_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm phát sinh mã OTP |

* **Chỉ mục (Indexes):**
  * `idx_otp_phone_valid` trên `(phone_number, is_used, expired_at)`: Tìm kiếm nhanh mã OTP còn hiệu lực.

---

### PHÂN HỆ II: HỒ SƠ BỆNH NHÂN & ỦY QUYỀN CHĂM SÓC

#### 5. Bảng `patients` (Hồ sơ Bệnh nhân)
Quản lý thông tin định danh, bệnh lý và các trường lâm sàng tùy biến theo loại mổ (UC-12).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `patient_id` | `VARCHAR(30)` | **PK**, Not Null | — | Mã bệnh nhân duy nhất (ví dụ: `BN-2026-00012`) (BR15) |
| `full_name` | `VARCHAR(100)` | Not Null | — | Họ và tên bệnh nhân (BR15) |
| `date_of_birth` | `DATE` | Not Null | — | Ngày tháng năm sinh (BR15) |
| `gender` | `VARCHAR(10)` | Not Null | — | ENUM: `'MALE'`, `'FEMALE'`, `'OTHER'` (BR15) |
| `phone_number` | `VARCHAR(20)` | Nullable | NULL | Số điện thoại liên hệ bệnh nhân |
| `address` | `TEXT` | Nullable | NULL | Địa chỉ cư trú |
| `surgery_type` | `VARCHAR(50)` | Not Null | — | Loại phẫu thuật: `'Phaco'`, `'Lác'`, `'LASIK'`, `'Cắt dịch kính'`... (BR15) |
| `surgery_date` | `DATE` | Nullable | NULL | Ngày phẫu thuật |
| `operated_eye` | `VARCHAR(10)` | Not Null | — | ENUM: `'LEFT'`, `'RIGHT'`, `'BOTH'` (Mắt phẫu thuật) |
| `clinical_custom_data`| `JSONB` | Nullable | `'{}'` | **Trường tùy biến lâm sàng động theo ca mổ** (UC-12.1) |
| `medical_notes` | `TEXT` | Nullable | NULL | Ghi chú y khoa bổ sung |
| `status` | `VARCHAR(20)` | Not Null | `'ACTIVE'` | ENUM: `'ACTIVE'`, `'ARCHIVED'` (Xóa mềm - BR25) |
| `created_by_doctor_id`| `UUID` | **FK** -> `doctor_profiles(doctor_id)`, Not Null | — | Bác sĩ tiếp nhận hồ sơ (BR14) |
| `created_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm tạo hồ sơ |
| `updated_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm chỉnh sửa hồ sơ |

* **Đặc tả cấu trúc `clinical_custom_data` (JSONB):**
  ```json
  {
    "iol_power": "+21.5D",
    "target_refraction": "-0.50D",
    "pre_op_strabismus_angle": "25 PD",
    "corneal_incision_type": "Clear corneal 2.2mm",
    "pre_op_intraocular_pressure": "16 mmHg",
    "anesthesia_type": "Tê bề mặt / Nhỏ tê"
  }
  ```
* **Chỉ mục (Indexes):**
  * `idx_patients_search` trên `(full_name, phone_number)`: Tìm kiếm nhanh bệnh nhân (UC-12.2).
  * `idx_patients_status_type` trên `(status, surgery_type)`: Lọc theo trạng thái và loại phẫu thuật.

---

#### 6. Bảng `caregiver_patient_links` (Liên kết Caregiver - Bệnh nhân)
Lưu quan hệ ủy quyền sau khi quét mã QR thành công (UC-02).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `link_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính bản ghi liên kết |
| `caregiver_id` | `UUID` | **FK** -> `caregiver_profiles(caregiver_id)`, Not Null | — | Người chăm sóc được phân quyền |
| `patient_id` | `VARCHAR(30)` | **FK** -> `patients(patient_id)`, Not Null | — | Bệnh nhân được chăm sóc |
| `care_plan_id` | `UUID` | **FK** -> `patient_care_plans(care_plan_id)`, Not Null | — | Care Plan đang hoạt động của bệnh nhân |
| `linked_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Mốc thời gian quét QR thành công (BR5) |
| `status` | `VARCHAR(20)` | Not Null | `'ACTIVE'` | ENUM: `'ACTIVE'`, `'REVOKED'` |
| `revoked_at` | `TIMESTAMPTZ` | Nullable | NULL | Mốc thời gian hủy quyền liên kết nếu có |

* **Chỉ mục (Indexes):**
  * `idx_links_caregiver_patient` (Unique) trên `(caregiver_id, patient_id, care_plan_id)`: Chống liên kết trùng lặp.

---

### PHÂN HỆ III: MASTER CARE PLAN TEMPLATE (CẤU HÌNH MẪU CHUẨN)

#### 7. Bảng `care_plan_templates` (Master Care Plan Template)
Gói phác đồ mẫu gốc gắn với loại phẫu thuật (UC-13).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `template_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính template |
| `template_name` | `VARCHAR(150)` | Not Null | — | Tên gói template chuẩn (BR16) |
| `surgery_type` | `VARCHAR(50)` | Not Null | — | Loại phẫu thuật liên kết duy nhất (BR16) |
| `clinical_description`| `TEXT` | Nullable | NULL | Mô tả mục tiêu lâm sàng |
| `status` | `VARCHAR(20)` | Not Null | `'DRAFT'` | ENUM: `'DRAFT'`, `'ACTIVE'`, `'INACTIVE'` |
| `created_by_doctor_id`| `UUID` | **FK** -> `doctor_profiles(doctor_id)`, Not Null | — | Bác sĩ cấu hình gói mẫu |
| `created_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm tạo |
| `updated_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm cập nhật cuối |

* **Chỉ mục (Indexes):**
  * `idx_templates_surgery_status` trên `(surgery_type, status)`: Tra cứu mẫu áp dụng cho ca phẫu thuật.
* **Ràng buộc duy nhất (Unique Constraints):**
  * `uq_template_name_surgery` trên `(template_name, surgery_type)`: Chống trùng tên template trong cùng loại phẫu thuật (UC-13.1 E1).

---

#### 8. Bảng `template_learning_modules` (Bài học trong Learning Path)
Các bài học đào tạo Caregiver theo lộ trình phục hồi (UC-14).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `module_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính bài học |
| `template_id` | `UUID` | **FK** -> `care_plan_templates(template_id)`, On Delete Cascade | — | Thuộc gói template nào |
| `title` | `VARCHAR(200)` | Not Null | — | Tiêu đề bài học |
| `category` | `VARCHAR(50)` | Not Null | — | Phân loại chủ đề |
| `content_text` | `TEXT` | Not Null | — | Văn bản mô tả hướng dẫn chi tiết |
| `media_type` | `VARCHAR(20)` | Not Null | `'VIDEO'` | ENUM: `'VIDEO'`, `'IMAGE'`, `'INFOGRAPHIC'`, `'TEXT'` |
| `media_url` | `VARCHAR(500)` | Nullable | NULL | Đường dẫn video/ảnh minh họa |
| `display_order` | `INT` | Not Null | `1` | Thứ tự xuất hiện trong Learning Path (UC-14.5) |
| `created_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm tạo |
| `updated_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm sửa |

---

#### 9. Bảng `template_quiz_questions` (Câu hỏi trắc nghiệm Mini Quiz)
Bộ 3 câu hỏi trắc nghiệm kiểm tra kiến thức ở cuối bài học (UC-03, UC-14.1).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `question_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính câu hỏi trắc nghiệm |
| `module_id` | `UUID` | **FK** -> `template_learning_modules(module_id)`, On Delete Cascade | — | Thuộc bài học nào |
| `question_text` | `TEXT` | Not Null | — | Nội dung câu hỏi trắc nghiệm |
| `option_a` | `TEXT` | Not Null | — | Nội dung phương án A |
| `option_b` | `TEXT` | Not Null | — | Nội dung phương án B |
| `option_c` | `TEXT` | Not Null | — | Nội dung phương án C |
| `option_d` | `TEXT` | Not Null | — | Nội dung phương án D |
| `correct_option` | `VARCHAR(2)` | Not Null | — | ENUM: `'A'`, `'B'`, `'C'`, `'D'` (Đáp án đúng) |
| `clinical_explanation`| `TEXT` | Not Null | — | Lời giải thích y khoa khi chấm điểm (BR7) |
| `display_order` | `INT` | Not Null | `1` | Thứ tự câu hỏi (1, 2, 3) |

---

#### 10. Bảng `template_medications` (Thuốc mẫu trong Medication Template)
Danh mục đơn thuốc chuẩn hóa kèm mô tả nhận diện và lưu ý lâm sàng (UC-15).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `medication_template_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính thuốc mẫu |
| `template_id` | `UUID` | **FK** -> `care_plan_templates(template_id)`, On Delete Cascade | — | Thuộc template nào |
| `surgery_type` | `VARCHAR(50)` | Not Null | — | Loại phẫu thuật áp dụng (UC-15.1 Step 2) |
| `drug_name` | `VARCHAR(150)` | Not Null | — | Tên thuốc, hoạt chất và hàm lượng |
| `drug_form` | `VARCHAR(20)` | Not Null | — | ENUM: `'EYE_DROP'`, `'ORAL'` |
| `dosage` | `VARCHAR(50)` | Not Null | — | Liều lượng (1 giọt, 1 viên...) |
| `frequency` | `VARCHAR(50)` | Not Null | — | Tần suất dùng (ví dụ: 4 lần/ngày) |
| `times_of_day` | `JSONB` | Not Null | `'["Sáng", "Trưa", "Chiều", "Tối"]'` | Các mốc cữ uống trong ngày |
| `meal_relation` | `VARCHAR(20)` | Not Null | `'NONE'` | ENUM: `'BEFORE_MEAL'`, `'AFTER_MEAL'`, `'NONE'` |
| `duration_days` | `INT` | Not Null | `7` | Số ngày chỉ định dùng thuốc |
| `order_index` | `INT` | Not Null | `1` | Thứ tự sử dụng trong cữ |
| `visual_identification`| `TEXT` | Not Null | — | **Mô tả nhận diện về thuốc** (vỏ, nắp, màu) (UC-15.1 Step 2) |
| `clinical_cautions` | `TEXT` | Not Null | — | **Lưu ý về loại thuốc** (lắc kỹ, bảo quản lạnh, giãn cách 5 phút...) (BR23, UC-15.1 Step 2) |
| `created_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm tạo |
| `updated_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm cập nhật |

---

#### 11. Bảng `template_recovery_milestones` (Mốc khảo sát Recovery Check mẫu)
Mốc thời gian khảo sát phục hồi chuẩn theo ngày (UC-16).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `milestone_template_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính mốc mẫu |
| `template_id` | `UUID` | **FK** -> `care_plan_templates(template_id)`, On Delete Cascade | — | Thuộc template nào |
| `surgery_type` | `VARCHAR(50)` | Not Null | — | Loại phẫu thuật áp dụng (UC-16.1 Step 2) |
| `milestone_name` | `VARCHAR(50)` | Not Null | — | Tên mốc khảo sát (ví dụ: "Ngày 1", "Ngày 3", "Ngày 7") |
| `days_post_op` | `INT` | Not Null | — | Số ngày sau mổ kích hoạt khảo sát (1, 3, 7, 14...) |
| `description` | `TEXT` | Nullable | NULL | Mục tiêu đánh giá của mốc |

* **Ràng buộc kiểm tra (Check Constraints):**
  * `chk_days_post_op_positive`: `CHECK (days_post_op > 0)` — Số ngày sau mổ phải là số dương.
* **Ràng buộc duy nhất (Unique Constraints):**
  * `uq_milestone_template_day` trên `(template_id, days_post_op)`: Không cho phép trùng mốc ngày trong cùng một template.

---

#### 12. Bảng `template_recovery_questions` (Câu hỏi khảo sát phục hồi mẫu)
Bộ 3–5 câu hỏi khảo sát triệu chứng kèm quy chuẩn cờ cảnh báo (UC-16).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `question_template_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính câu hỏi mẫu |
| `milestone_template_id`| `UUID` | **FK** -> `template_recovery_milestones(...)`, On Delete Cascade | — | Thuộc mốc ngày nào |
| `question_text` | `TEXT` | Not Null | — | Nội dung câu hỏi khảo sát |
| `answer_type` | `VARCHAR(20)` | Not Null | `'YES_NO'` | ENUM: `'YES_NO'`, `'SINGLE_CHOICE'` |
| `options_json` | `JSONB` | Not Null | `'[{"label":"Có", "value":"YES"}, {"label":"Không", "value":"NO"}]'` | Danh sách phương án lựa chọn |
| `normal_criteria` | `VARCHAR(100)` | Not Null | — | Đáp án ứng với trạng thái Bình thường |
| `attention_criteria` | `VARCHAR(100)` | Nullable | NULL | Đáp án kích hoạt trạng thái "Cần chú ý" |
| `red_flag_criteria` | `VARCHAR(100)` | Nullable | NULL | Đáp án kích hoạt trạng thái "Red Flag" (BR10) |
| `order_index` | `INT` | Not Null | `1` | Thứ tự câu hỏi (1 đến 5) (BR9) |

---

#### 13. Bảng `template_red_flags` (Dấu hiệu nguy hiểm Red Flag mẫu)
Danh mục cảnh báo biến chứng nguy hiểm khẩn cấp và hotline bệnh viện (UC-17).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `red_flag_template_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính dấu hiệu Red Flag |
| `template_id` | `UUID` | **FK** -> `care_plan_templates(template_id)`, On Delete Cascade | — | Thuộc template nào |
| `surgery_type` | `VARCHAR(50)` | Not Null | — | Loại phẫu thuật áp dụng (UC-17.1 Step 2) |
| `sign_name` | `VARCHAR(150)` | Not Null | — | Tên dấu hiệu nguy hiểm (ví dụ: *Đau nhức dữ dội lan nửa đầu*) |
| `warning_level` | `VARCHAR(20)` | Not Null | `'EMERGENCY'` | ENUM: `'EMERGENCY'`, `'SAME_DAY_EXAM'` |
| `trigger_condition` | `TEXT` | Nullable | NULL | Điều kiện kích hoạt từ kết quả Recovery Check |
| `first_aid_instructions`| `TEXT` | Not Null | — | Hướng dẫn sơ cứu khẩn cấp ban đầu (BR11) |
| `emergency_hotline` | `VARCHAR(20)` | Not Null | — | Số điện thoại đường dây nóng cấp cứu 24/7 (UC-17.1) |
| `order_index` | `INT` | Not Null | `1` | Thứ tự ưu tiên hiển thị |

---

#### 14. Bảng `template_do_dont_items` (Chỉ dẫn Nên làm / Cần tránh mẫu)
Danh mục hành vi sinh hoạt được phép và cấm kỵ hậu phẫu (UC-18).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `do_dont_template_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính mục Do & Don't |
| `template_id` | `UUID` | **FK** -> `care_plan_templates(template_id)`, On Delete Cascade | — | Thuộc template nào |
| `surgery_type` | `VARCHAR(50)` | Not Null | — | Loại phẫu thuật áp dụng (UC-18.1 Step 2) |
| `item_type` | `VARCHAR(10)` | Not Null | — | ENUM: `'DO'`, `'DONT'` (Nên làm / Cần tránh) (BR22) |
| `category` | `VARCHAR(20)` | Not Null | — | ENUM: `'HYGIENE'`, `'ACTIVITY'`, `'DIET'`, `'SLEEP'` |
| `behavior_title` | `VARCHAR(200)` | Not Null | — | Tên hành vi sinh hoạt |
| `clinical_explanation`| `TEXT` | Not Null | — | Giải thích y khoa lý do cần thực hiện/kiêng cữ |
| `applicable_duration` | `VARCHAR(100)` | Not Null | — | Thời gian áp dụng (ví dụ: *7 ngày đầu sau mổ*) |
| `order_index` | `INT` | Not Null | `1` | Thứ tự hiển thị |

---

### PHÂN HỆ IV: THỰC THỂ KẾ HOẠCH BỆNH NHÂN (PATIENT CARE PLAN INSTANCE)

#### 15. Bảng `patient_care_plans` (Kế hoạch chăm sóc bệnh nhân thực tế)
Bản thể hiện độc lập được kích hoạt cho từng bệnh nhân cụ thể (UC-19).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `care_plan_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính Care Plan bệnh nhân |
| `patient_id` | `VARCHAR(30)` | **FK** -> `patients(patient_id)`, Not Null | — | Gắn với hồ sơ bệnh nhân cụ thể |
| `source_template_id` | `UUID` | **FK** -> `care_plan_templates(template_id)`, Not Null | — | Template gốc được nhân bản |
| `surgery_type` | `VARCHAR(50)` | Not Null | — | Loại phẫu thuật thực hiện |
| `status` | `VARCHAR(20)` | Not Null | `'DRAFT'` | ENUM: `'DRAFT'`, `'ACTIVE'`, `'COMPLETED'`, `'ARCHIVED'` |
| `activated_at` | `TIMESTAMPTZ` | Nullable | NULL | Thời điểm Bác sĩ kích hoạt kế hoạch |
| `completed_at` | `TIMESTAMPTZ` | Nullable | NULL | Thời điểm kết thúc đợt theo dõi |
| `created_by_doctor_id`| `UUID` | **FK** -> `doctor_profiles(doctor_id)`, Not Null | — | Bác sĩ chỉ định |
| `created_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm tạo |
| `updated_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm cập nhật |

* **Chỉ mục độc nhất có điều kiện (Partial Unique Index - BR19):**
  ```sql
  CREATE UNIQUE INDEX idx_patient_single_active_plan 
  ON patient_care_plans(patient_id) 
  WHERE status = 'ACTIVE';
  ```
  *(Đảm bảo trong 1 thời điểm mỗi bệnh nhân chỉ có duy nhất tối đa 1 Care Plan đang hoạt động).*

---

#### 16. Bảng `patient_medications` (Đơn thuốc thực tế của bệnh nhân)
Danh mục thuốc riêng của bệnh nhân sau khi Bác sĩ đã tùy biến liều lượng (UC-06, UC-19).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `patient_medication_id`| `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính mục thuốc của bệnh nhân |
| `care_plan_id` | `UUID` | **FK** -> `patient_care_plans(care_plan_id)`, On Delete Cascade | — | Thuộc Care Plan nào |
| `drug_name` | `VARCHAR(150)` | Not Null | — | Tên biệt dược, hàm lượng thực tế |
| `drug_form` | `VARCHAR(20)` | Not Null | — | ENUM: `'EYE_DROP'`, `'ORAL'` |
| `dosage` | `VARCHAR(50)` | Not Null | — | Liều lượng bác sĩ kê riêng |
| `frequency` | `VARCHAR(50)` | Not Null | — | Tần suất dùng |
| `times_of_day` | `JSONB` | Not Null | — | Danh sách cữ dùng thuốc trong ngày |
| `meal_relation` | `VARCHAR(20)` | Not Null | `'NONE'` | Trước/sau bữa ăn |
| `duration_days` | `INT` | Not Null | — | Số ngày dùng thuốc |
| `order_index` | `INT` | Not Null | `1` | Thứ tự nhỏ/uống trong cữ |
| `visual_identification`| `TEXT` | Not Null | — | Nhận diện màu sắc, hình dáng bao bì |
| `clinical_cautions` | `TEXT` | Not Null | — | Lưu ý sử dụng và giãn cách tối thiểu 5 phút (BR23) |
| `start_date` | `DATE` | Not Null | — | Ngày bắt đầu dùng thuốc |
| `notes` | `TEXT` | Nullable | NULL | Ghi chú dặn dò riêng của bác sĩ |

---

#### 17. Bảng `patient_followup_appointments` (Lịch hẹn tái khám của bệnh nhân)
Lịch hẹn cụ thể do bác sĩ thiết lập cho đợt điều trị (UC-07, UC-19).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `appointment_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính lịch hẹn |
| `care_plan_id` | `UUID` | **FK** -> `patient_care_plans(care_plan_id)`, On Delete Cascade | — | Thuộc Care Plan nào |
| `appointment_date` | `DATE` | Not Null | — | Ngày hẹn tái khám |
| `appointment_time` | `TIME` | Not Null | — | Giờ hẹn tái khám |
| `doctor_id` | `UUID` | **FK** -> `doctor_profiles(doctor_id)`, Not Null | — | Bác sĩ phụ trách khám lại |
| `clinic_location` | `VARCHAR(150)` | Not Null | — | Địa điểm phòng khám / cơ sở |
| `preparation_notes` | `TEXT` | Nullable | NULL | Dặn dò chuẩn bị trước khám |
| `reminder_24h_sent` | `BOOLEAN` | Not Null | `FALSE` | Đã gửi thông báo nhắc trước 24h (BR24) |
| `reminder_2h_sent` | `BOOLEAN` | Not Null | `FALSE` | Đã gửi thông báo nhắc trước 2h (BR24) |
| `status` | `VARCHAR(20)` | Not Null | `'SCHEDULED'` | ENUM: `'SCHEDULED'`, `'COMPLETED'`, `'CANCELLED'` |

---

#### 18. Bảng `patient_qr_codes` (Mã QR bảo mật định danh Care Plan)
Token QR mã hóa gắn liền với phiếu xuất viện (UC-02, UC-20).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `qr_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính bản ghi QR |
| `care_plan_id` | `UUID` | **FK** -> `patient_care_plans(care_plan_id)`, Unique, On Delete Cascade | — | Gắn với Care Plan (tỷ lệ 1 : 1) |
| `qr_token` | `VARCHAR(255)` | Unique, Not Null | — | Token mã hóa ngẫu nhiên chống giả mạo (BR20) |
| `issued_by_doctor_id`| `UUID` | **FK** -> `doctor_profiles(doctor_id)`, Not Null | — | Bác sĩ cấp phát |
| `issued_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm tạo mã |
| `status` | `VARCHAR(20)` | Not Null | `'ACTIVE'` | ENUM: `'ACTIVE'`, `'REVOKED'` (BR4) |
| `print_count` | `INT` | Not Null | `1` | Số lần in ấn / tạo lại phiếu (UC-20 A1) |

* **Chỉ mục (Indexes):**
  * `idx_qr_token_active` trên `(qr_token, status)`: Quét và giải mã QR cực nhanh khi Caregiver scan camera.

---

### PHÂN HỆ V: GIAO DỊCH & SỰ KIỆN LÂM SÀNG THỰC TẾ

#### 19. Bảng `medication_logs` (Nhật ký xác nhận dùng thuốc)
Ghi nhận Caregiver đánh dấu hoàn thành cữ thuốc (UC-06).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `log_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính nhật ký uống thuốc |
| `patient_medication_id`| `UUID` | **FK** -> `patient_medications(...)`, Not Null | — | Loại thuốc nào |
| `caregiver_id` | `UUID` | **FK** -> `caregiver_profiles(...)`, Not Null | — | Người xác nhận |
| `scheduled_time` | `TIMESTAMPTZ` | Not Null | — | Giờ cữ thuốc theo lịch quy định |
| `confirmed_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Giờ thực tế người chăm sóc bấm xác nhận |
| `status` | `VARCHAR(20)` | Not Null | `'TAKEN'` | ENUM: `'TAKEN'`, `'SKIPPED'` |
| `notes` | `VARCHAR(255)` | Nullable | NULL | Ghi chú nếu có dấu hiệu bất thường nhẹ |

* **Chỉ mục (Indexes):**
  * `idx_medlog_medication_time` trên `(patient_medication_id, scheduled_time)`: Tối ưu tra cứu lịch sử uống thuốc theo cữ.
  * `idx_medlog_caregiver` trên `(caregiver_id, confirmed_at)`: Tra cứu nhật ký theo người chăm sóc.

---

#### 20. Bảng `caregiver_quiz_submissions` (Kết quả Mini Quiz của Caregiver)
Ghi nhận kết quả làm bài trắc nghiệm sau khi hoàn thành bài học Learning Path (UC-03).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `submission_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính lượt nộp quiz |
| `caregiver_id` | `UUID` | **FK** -> `caregiver_profiles(...)`, Not Null | — | Người chăm sóc làm bài |
| `module_id` | `UUID` | **FK** -> `template_learning_modules(...)`, Not Null | — | Thuộc bài học nào |
| `correct_answers_count`| `INT` | Not Null | — | Số câu làm đúng (ví dụ: 3/3 hoặc 2/3) |
| `total_questions` | `INT` | Not Null | `3` | Tổng số câu hỏi kiểm tra (mặc định 3 câu) |
| `answers_detail` | `JSONB` | Not Null | — | Chi tiết các phương án Caregiver đã chọn |
| `submitted_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm hoàn tất nộp bài |

* **Chỉ mục (Indexes):**
  * `idx_quiz_caregiver_module` trên `(caregiver_id, module_id, submitted_at)`: Tối ưu tra cứu lịch sử làm bài Quiz của Caregiver theo bài học.

---

#### 21. Bảng `recovery_check_submissions` (Lượt nộp bảng kiểm phục hồi)
Ghi nhận kết quả khảo sát triệu chứng phục hồi theo mốc (UC-08, UC-21).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `submission_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính lượt nộp bảng kiểm |
| `care_plan_id` | `UUID` | **FK** -> `patient_care_plans(...)`, Not Null | — | Kế hoạch chăm sóc liên quan |
| `milestone_template_id`| `UUID` | **FK** -> `template_recovery_milestones(...)`, Not Null | — | Thuộc mốc ngày nào (Day 1, Day 3...) |
| `caregiver_id` | `UUID` | **FK** -> `caregiver_profiles(...)`, Not Null | — | Caregiver thực hiện nộp |
| `submitted_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm nộp bảng kiểm |
| `overall_status` | `VARCHAR(20)` | Not Null | — | ENUM: `'NORMAL'`, `'NEEDS_ATTENTION'`, `'RED_FLAG'` (BR10) |
| `doctor_viewed` | `BOOLEAN` | Not Null | `FALSE` | Bác sĩ đã xem xét kết quả hay chưa |
| `doctor_viewed_at` | `TIMESTAMPTZ` | Nullable | NULL | Thời điểm Bác sĩ mở xem xét (UC-21) |
| `doctor_notes` | `TEXT` | Nullable | NULL | Nhận xét chuyên môn của Bác sĩ |

* **Chỉ mục (Indexes):**
  * `idx_recovery_doctor_alert` trên `(doctor_viewed, overall_status, submitted_at)`: Tối ưu Bảng điều khiển Bác sĩ lọc các ca có cờ cảnh báo Red Flag chưa xem xét (UC-21).

---

#### 22. Bảng `recovery_check_answers` (Chi tiết câu trả lời khảo sát)
Lưu đáp án từng câu hỏi trong bảng kiểm phục hồi (UC-08, UC-21).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `answer_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính câu trả lời |
| `submission_id` | `UUID` | **FK** -> `recovery_check_submissions(...)`, On Delete Cascade | — | Thuộc lượt nộp nào |
| `question_template_id` | `UUID` | **FK** -> `template_recovery_questions(...)`, Not Null | — | Câu hỏi khảo sát nào |
| `answer_value` | `VARCHAR(100)` | Not Null | — | Giá trị Caregiver chọn |
| `flag_level` | `VARCHAR(20)` | Not Null | `'NORMAL'` | ENUM: `'NORMAL'`, `'NEEDS_ATTENTION'`, `'RED_FLAG'` |

---

#### 23. Bảng `red_flag_incidents` (Sự kiện cảnh báo khẩn cấp)
Theo dõi các tình huống khẩn cấp xảy ra với bệnh nhân (UC-09, UC-21).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `incident_id` | `UUID` | **PK**, Not Null | `gen_random_uuid()` | Khóa chính sự kiện |
| `care_plan_id` | `UUID` | **FK** -> `patient_care_plans(...)`, Not Null | — | Kế hoạch chăm sóc của bệnh nhân |
| `caregiver_id` | `UUID` | **FK** -> `caregiver_profiles(...)`, Not Null | — | Người phát hiện / liên hệ |
| `trigger_source` | `VARCHAR(20)` | Not Null | — | ENUM: `'RECOVERY_CHECK'`, `'MANUAL_BUTTON'` (UC-09) |
| `red_flag_template_id` | `UUID` | **FK** -> `template_red_flags(...)`, Nullable | NULL | Dấu hiệu nguy hiểm tương ứng nếu xác định được |
| `triggered_at` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm phát sinh cảnh báo |
| `call_initiated` | `BOOLEAN` | Not Null | `FALSE` | Đã bấm nút gọi đường dây nóng hay chưa (UC-09 Step 3) |
| `call_initiated_at` | `TIMESTAMPTZ` | Nullable | NULL | Thời điểm bấm gọi |
| `acknowledged_by_doctor_id`| `UUID` | **FK** -> `doctor_profiles(...)`, Nullable | NULL | Bác sĩ tiếp nhận ca cấp cứu |
| `acknowledged_at` | `TIMESTAMPTZ` | Nullable | NULL | Thời điểm tiếp nhận |
| `clinical_resolution` | `TEXT` | Nullable | NULL | Kết luận và hướng xử trí lâm sàng |

---

### PHÂN HỆ VI: MỞ RỘNG QUẢN TRỊ `[FUTURE / ADMIN]`

#### 24. Bảng `audit_logs` (Nhật ký kiểm toán hệ thống) `[FUTURE / ADMIN]`
Sẵn sàng hỗ trợ tính năng CRUD Audit Log của Admin trong tương lai (BR5, BR20).

| Tên Cột (Column) | Kiểu Dữ Liệu | Ràng Buộc | Giá Trị Mặc Định | Diễn Giải & Quy Tắc Nghiệp Vụ |
|---|---|---|---|---|
| `audit_id` | `BIGSERIAL` / `BIGINT` | **PK**, Not Null | Tự tăng | Khóa chính tự tăng nhật ký kiểm toán |
| `user_id` | `UUID` | **FK** -> `accounts(account_id)`, Nullable | NULL | Tài khoản thực hiện hành động |
| `action_type` | `VARCHAR(50)` | Not Null | — | Hành động (`'LOGIN'`, `'CREATE_PATIENT'`, `'LINK_QR'`, `'ACTIVATE_PLAN'`...) |
| `target_entity` | `VARCHAR(50)` | Not Null | — | Tên bảng chịu tác động (`'patients'`, `'patient_care_plans'`...) |
| `target_id` | `VARCHAR(100)` | Not Null | — | Khóa định danh của đối tượng bị thay đổi |
| `old_data` | `JSONB` | Nullable | NULL | Dữ liệu cũ trước khi sửa |
| `new_data` | `JSONB` | Nullable | NULL | Dữ liệu mới sau khi sửa |
| `ip_address` | `VARCHAR(45)` | Nullable | NULL | Địa chỉ IP máy khách |
| `user_agent` | `VARCHAR(255)` | Nullable | NULL | Thông tin trình duyệt / thiết bị |
| `timestamp` | `TIMESTAMPTZ` | Not Null | `CURRENT_TIMESTAMP` | Thời điểm ghi nhận kiểm toán |

* **Chỉ mục (Indexes):**
  * `idx_audit_entity_time` trên `(target_entity, target_id, timestamp)`: Tra cứu lịch sử thay đổi của một hồ sơ y tế.
  * `idx_audit_user_action` trên `(user_id, action_type, timestamp)`: Kiểm tra hoạt động của tài khoản người dùng.
