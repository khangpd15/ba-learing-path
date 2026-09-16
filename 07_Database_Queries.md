# 07. DANH MỤC CÂU LỆNH TRUY VẤN DỮ LIỆU NGHIỆP VỤ (DATABASE QUERIES & DML SCRIPTS)
# HỆ THỐNG REMICARE OPHTHALMIC POST-OP PLATFORM

> **Dự án:** RemiCare Ophthalmic Post-Op Platform (Nền tảng Hướng dẫn và Giám sát Chăm sóc Hậu phẫu Nhãn khoa)  
> **Doanh nghiệp mục tiêu:** Công ty Cổ phần Tập đoàn Y khoa VISI (VISI Medical Group)  
> **Phiên bản tài liệu:** V1 (Tách biệt độc lập khỏi Use Case đặc tả nghiệp vụ)  
> **Hệ quản trị CSDL:** PostgreSQL 16+  
> **Ngày phê duyệt:** 15/09/2026  
> **Mục tiêu kỹ thuật:** Cung cấp toàn bộ các câu lệnh SQL mẫu (SELECT, INSERT, UPDATE, DELETE, Transaction blocks, CTEs, Joins) tương ứng với từng Use Case và màn hình giao diện, giúp Lập trình viên Backend/Database tối ưu hóa hiệu năng truy vấn và bảo đảm an toàn dữ liệu y tế.  

---

## 1. NGUYÊN TẮC KỸ THUẬT & CHUẨN MỰC TRUY VẤN

1. **Chuẩn mã hóa và Khóa chính:** Mọi bảng đều sử dụng UUIDv4 (`gen_random_uuid()`) làm Primary Key.
2. **Kiểm soát múi giờ:** Toàn bộ trường thời gian lưu trữ kiểu `TIMESTAMPTZ` (UTC) và chuyển đổi sang múi giờ Việt Nam (`Asia/Ho_Chi_Minh` - UTC+7) ở tầng hiển thị.
3. **Nguyên tắc Xóa Mềm (Soft Delete):** Dữ liệu bệnh nhân và hồ sơ y tế không bao giờ dùng `DELETE` vật lý mà chuyển trạng thái `status = 'ARCHIVED'` và cập nhật `archived_at = CURRENT_TIMESTAMP` (BR25).
4. **Phân vùng dữ liệu đa chi nhánh (Multi-Branch Isolation):** Mọi truy vấn danh sách người bệnh hoặc phân quyền nhân sự bắt buộc phải có mệnh đề lọc `WHERE facility_id = :current_user_facility_id` (BR26), ngoại trừ tài khoản Ban Giám đốc / GCMO cấp tập đoàn.
5. **Giao dịch Toàn vẹn (ACID Transactions):** Các luồng nghiệp vụ phức tạp như nhân bản Care Plan, sinh mã QR hoặc tiếp nhận cấp cứu Red Flag bắt buộc phải thực thi trong khối `BEGIN ... COMMIT` với mức cô lập `READ COMMITTED`.

### 1.1 Dữ Liệu Khởi Tạo Cơ Sở Y Tế (Facilities Seed Data - 5 Chi Nhánh VISI)
Khởi tạo danh mục 5 chi nhánh thuộc Hệ thống Bệnh viện Mắt VISI phục vụ cô lập dữ liệu theo BR26:

```sql
INSERT INTO facilities (facility_id, facility_code, facility_name, address, phone, hotline, is_active, created_at)
VALUES 
  ('fac-001-thu-duc', 'VISI-TD', 'Bệnh viện Mắt Kỹ thuật cao VISI Thủ Đức', '215 Võ Văn Ngân, P. Linh Chiểu, TP. Thủ Đức, TP. HCM', '028 3896 1234', '0395 151 151', TRUE, CURRENT_TIMESTAMP),
  ('fac-002-hai-phong', 'VISI-HP', 'Bệnh viện Mắt VISI Hải Phòng', '45 Lạch Tray, Ngô Quyền, Hải Phòng', '0225 385 5678', '0395 151 152', TRUE, CURRENT_TIMESTAMP),
  ('fac-003-da-nang', 'VISI-DN', 'Bệnh viện Mắt Quốc tế VISI Đà Nẵng', '128 Nguyễn Văn Linh, Q. Hải Châu, TP. Đà Nẵng', '0236 365 4321', '0395 151 153', TRUE, CURRENT_TIMESTAMP),
  ('fac-004-can-tho', 'VISI-CT', 'Bệnh viện Mắt Sài Gòn - VISI Cần Thơ', '71 đường 30/4, P. An Phú, Q. Ninh Kiều, TP. Cần Thơ', '0292 373 8888', '0395 151 154', TRUE, CURRENT_TIMESTAMP),
  ('fac-005-ha-noi', 'VISI-HN', 'Bệnh viện Mắt Công nghệ cao VISI Hà Nội', '19 Bà Triệu, P. Tràng Tiền, Q. Hoàn Kiếm, Hà Nội', '024 3936 9999', '0395 151 155', TRUE, CURRENT_TIMESTAMP)
ON CONFLICT (facility_code) DO UPDATE 
SET facility_name = EXCLUDED.facility_name,
    address = EXCLUDED.address,
    hotline = EXCLUDED.hotline,
    updated_at = CURRENT_TIMESTAMP;

-- Truy vấn danh sách cơ sở y tế đang hoạt động
SELECT facility_id, facility_code, facility_name, address, phone, hotline
FROM facilities
WHERE is_active = TRUE
ORDER BY facility_code ASC;
```

---

## 2. DANH MỤC CÂU LỆNH TRUY VẤN CHI TIẾT THEO USE CASE

### 2.1 Phân Hệ Xác Thực & Phân Quyền (Auth & RBAC)

#### UC-001: Xác Minh OTP và Truy Vấn Caregiver
```sql
-- 1. Kiểm tra mã OTP còn hiệu lực và chưa sử dụng
SELECT otp_id, otp_code, expires_at, is_used, attempts_count
FROM otp_verifications
WHERE phone = :phone 
  AND purpose = 'CAREGIVER_LOGIN'
  AND is_used = FALSE 
  AND expires_at > CURRENT_TIMESTAMP
ORDER BY created_at DESC 
LIMIT 1;

-- 2. Cập nhật OTP đã sử dụng
UPDATE otp_verifications 
SET is_used = TRUE, verified_at = CURRENT_TIMESTAMP 
WHERE otp_id = :otp_id;

-- 3. Truy vấn hoặc tạo mới Caregiver Profile
INSERT INTO caregiver_profiles (caregiver_id, phone, full_name, created_at)
VALUES (gen_random_uuid(), :phone, :full_name, CURRENT_TIMESTAMP)
ON CONFLICT (phone) DO UPDATE 
SET last_login_at = CURRENT_TIMESTAMP 
RETURNING caregiver_id, full_name, phone;
```

#### UC-002: Xác Thực Nhân Viên Y Tế và Quyền Hạn Chi Nhánh
```sql
-- Xác thực thông tin đăng nhập và kiểm tra trạng thái hoạt động
SELECT a.account_id, a.phone, a.email, a.role, a.status, a.two_factor_secret, a.facility_id,
       f.facility_name, f.facility_code,
       dp.doctor_id, dp.title, dp.license_number, dp.department
FROM accounts a
LEFT JOIN facilities f ON a.facility_id = f.facility_id
LEFT JOIN doctor_profiles dp ON a.account_id = dp.account_id
WHERE (a.email = :username_or_email OR a.phone = :username_or_email)
  AND a.status = 'ACTIVE'
  AND a.role IN ('DOCTOR', 'NURSE', 'CSKH', 'GCMO', 'ADMIN');
```

#### UC-026.1: Khởi Tạo Tài Khoản Nhân Viên Y Tế & Gán Chi Nhánh
```sql
BEGIN;

-- 1. Thêm tài khoản xác thực
INSERT INTO accounts (account_id, email, phone, full_name, password_hash, role, facility_id, status, created_at)
VALUES (:new_account_id, :email, :phone, :full_name, :hashed_temp_password, :role, :facility_id, 'PENDING_ACTIVATION', CURRENT_TIMESTAMP);

-- 2. Nếu vai trò là Bác sĩ (DOCTOR) hoặc GCMO: Thêm hồ sơ định danh chuyên môn
INSERT INTO doctor_profiles (doctor_id, account_id, facility_id, license_number, title, department, created_at)
SELECT gen_random_uuid(), :new_account_id, :facility_id, :license_number, :title, :department, CURRENT_TIMESTAMP
WHERE :role IN ('DOCTOR', 'GCMO');

-- 3. Ghi nhật ký kiểm toán hệ thống
INSERT INTO audit_logs (user_id, action_type, target_entity, target_id, new_data, timestamp)
VALUES (:admin_id, 'CREATE_STAFF_ACCOUNT', 'accounts', :new_account_id::TEXT, 
        jsonb_build_object('email', :email, 'role', :role, 'facility_id', :facility_id), CURRENT_TIMESTAMP);

COMMIT;
```

#### UC-026.2: Tra Cứu và Lọc Danh Sách Nhân Viên Theo Cơ Sở
```sql
SELECT a.account_id, a.full_name, a.role, a.email, a.phone, a.facility_id, f.facility_name, a.status, a.created_at,
       dp.doctor_id, dp.license_number, dp.title
FROM accounts a
LEFT JOIN facilities f ON a.facility_id = f.facility_id
LEFT JOIN doctor_profiles dp ON a.account_id = dp.account_id
WHERE a.role IN ('DOCTOR', 'NURSE', 'CSKH', 'GCMO', 'ADMIN')
  AND (:facility_id IS NULL OR a.facility_id = :facility_id)
  AND (:role IS NULL OR a.role = :role)
  AND (:status IS NULL OR a.status = :status)
ORDER BY a.full_name ASC
LIMIT :limit OFFSET :offset;
```

#### UC-026.4: Khóa / Vô Hiệu Hóa Tài Khoản Nhân Viên (Deactivate/Lock)
```sql
BEGIN;

-- 1. Chuyển trạng thái tài khoản sang LOCKED
UPDATE accounts 
SET status = 'LOCKED', updated_at = CURRENT_TIMESTAMP 
WHERE account_id = :account_id;

-- 2. Ghi nhật ký kiểm toán an ninh nghiêm ngặt
INSERT INTO audit_logs (user_id, action_type, target_entity, target_id, new_data, timestamp)
VALUES (:admin_id, 'LOCK_STAFF_ACCOUNT', 'accounts', :account_id::TEXT, 
        jsonb_build_object('reason', :reason, 'locked_at', CURRENT_TIMESTAMP), CURRENT_TIMESTAMP);

COMMIT;
```

---

### 2.2 Phân Hệ Quản Lý Hồ Sơ Bệnh Nhân (Patient Profiles CRUD)

#### UC-004.1: Tạo Mới Hồ Sơ Bệnh Nhân
```sql
-- Tạo mã số bệnh án tự động: PAT-YYYYMM-XXXX
WITH new_seq AS (
  SELECT COALESCE(MAX(SUBSTRING(medical_record_code FROM 12 FOR 4)::INTEGER), 0) + 1 AS next_num
  FROM patients
  WHERE created_at >= DATE_TRUNC('month', CURRENT_TIMESTAMP)
)
INSERT INTO patients (
  patient_id, medical_record_code, full_name, display_name, phone, birth_year, gender,
  surgery_eye, surgery_type, surgery_date, facility_id, allergies_note, status, created_at
)
SELECT 
  gen_random_uuid(),
  'PAT-' || TO_CHAR(CURRENT_TIMESTAMP, 'YYYYMM') || '-' || LPAD(next_num::TEXT, 4, '0'),
  :full_name,
  :display_name,  -- Tên viết tắt bảo mật NĐ 13/2023 (ví dụ: 'Ng. V. An')
  :phone,
  :birth_year,
  :gender,
  :surgery_eye,
  :surgery_type,
  :surgery_date,
  :facility_id,
  :allergies_note,
  'ACTIVE',
  CURRENT_TIMESTAMP
FROM new_seq
RETURNING patient_id, medical_record_code, display_name;
```

#### UC-004.2: Tra Cứu và Lọc Danh Sách Bệnh Nhân Theo Cơ Sở
```sql
SELECT 
  p.patient_id, p.medical_record_code, p.display_name, p.birth_year, p.gender,
  p.surgery_eye, p.surgery_type, p.surgery_date, p.status,
  pcp.care_plan_id, pcp.status AS care_plan_status,
  COALESCE(
    (SELECT alert_level FROM recovery_check_submissions rcs 
     WHERE rcs.care_plan_id = pcp.care_plan_id 
     ORDER BY rcs.submitted_at DESC LIMIT 1), 
    'GREEN'
  ) AS latest_alert_level
FROM patients p
LEFT JOIN patient_care_plans pcp ON p.patient_id = pcp.patient_id AND pcp.status = 'ACTIVE'
WHERE p.facility_id = :facility_id
  AND p.status != 'ARCHIVED'
  AND (:surgery_type IS NULL OR p.surgery_type = :surgery_type)
  AND (:search IS NULL OR p.display_name ILIKE '%' || :search || '%' OR p.phone LIKE '%' || :search)
ORDER BY p.surgery_date DESC, p.created_at DESC
LIMIT :limit OFFSET :offset;
```

#### UC-004.3: Xem Toàn Diện Hồ Sơ Bệnh Nhân 360 Độ
```sql
-- 1. Thông tin bệnh nhân và Care Plan chính
SELECT p.*, pcp.care_plan_id, pcp.template_name, pcp.status AS plan_status, pcp.start_date
FROM patients p
LEFT JOIN patient_care_plans pcp ON p.patient_id = pcp.patient_id
WHERE p.patient_id = :patient_id;

-- 2. Lịch dùng thuốc thực tế
SELECT pms.schedule_id, pms.medication_name, pms.dosage_form, pms.dose_amount, 
       pms.target_eye, pms.frequency_schedule, pms.buffer_interval_minutes, pms.cap_color
FROM patient_medication_schedules pms
WHERE pms.care_plan_id = :care_plan_id;

-- 3. Danh sách Caregiver đã quét liên kết (tối đa 3 người - BR5)
SELECT cpl.link_id, cpl.relationship, cpl.linked_at, cp.full_name, cp.phone
FROM caregiver_patient_links cpl
JOIN caregiver_profiles cp ON cpl.caregiver_id = cp.caregiver_id
WHERE cpl.patient_id = :patient_id AND cpl.status = 'ACTIVE';
```

#### UC-004.4: Cập Nhật Thông Tin Hồ Sơ Bệnh Nhân
```sql
UPDATE patients
SET phone = :phone,
    allergies_note = :allergies_note,
    updated_at = CURRENT_TIMESTAMP
WHERE patient_id = :patient_id 
  AND status = 'ACTIVE';
```

#### UC-004.5: Lưu Trữ / Xóa Mềm Hồ Sơ Bệnh Nhân (Soft Delete)
```sql
BEGIN;

-- Kiểm tra không có Red Flag đang kích hoạt
DO $$
BEGIN
  IF EXISTS (
    SELECT 1 FROM red_flag_incidents rfi
    JOIN patient_care_plans pcp ON rfi.care_plan_id = pcp.care_plan_id
    WHERE pcp.patient_id = :patient_id AND rfi.status = 'TRIGGERED'
  ) THEN
    RAISE EXCEPTION 'Không thể lưu trữ hồ sơ đang có biến chứng Báo động Đỏ chưa xử lý (BR12)';
  END IF;
END $$;

-- Cập nhật trạng thái ARCHIVED
UPDATE patients 
SET status = 'ARCHIVED', archived_at = CURRENT_TIMESTAMP 
WHERE patient_id = :patient_id;

-- Thu hồi mã QR chưa liên kết
UPDATE patient_care_plans 
SET status = 'ARCHIVED', updated_at = CURRENT_TIMESTAMP 
WHERE patient_id = :patient_id;

COMMIT;
```

#### UC-003: Quét QR Liên Kết, Lựa Chọn Care Recipient & Lịch Sử Chăm Sóc (F-003, F-004)
```sql
-- 1. Quét QR liên kết Caregiver với Bệnh nhân (Kiểm soát tối đa 03 Caregiver theo BR5)
BEGIN;

DO $$
DECLARE
  current_caregiver_count INT;
  target_patient_id VARCHAR(30);
BEGIN
  -- Lấy patient_id từ token QR hợp lệ
  SELECT pcp.patient_id INTO target_patient_id
  FROM patient_qr_codes pqr
  JOIN patient_care_plans pcp ON pqr.care_plan_id = pcp.care_plan_id
  WHERE pqr.qr_token = :qr_token 
    AND pqr.status = 'ISSUED' 
    AND pqr.expires_at > CURRENT_TIMESTAMP;

  IF target_patient_id IS NULL THEN
    RAISE EXCEPTION 'Mã QR không hợp lệ, đã hết hạn hoặc đã bị thu hồi';
  END IF;

  -- Đếm số lượng Caregiver đang liên kết
  SELECT COUNT(*) INTO current_caregiver_count
  FROM caregiver_patient_links
  WHERE patient_id = target_patient_id AND status = 'ACTIVE';

  IF current_caregiver_count >= 3 THEN
    RAISE EXCEPTION 'Hồ sơ bệnh nhân đã đạt giới hạn tối đa 03 Người chăm sóc (BR5)';
  END IF;

  -- Tạo liên kết mới
  INSERT INTO caregiver_patient_links (link_id, caregiver_id, patient_id, relationship, role, status, linked_at)
  VALUES (gen_random_uuid(), :caregiver_id, target_patient_id, :relationship, 'SECONDARY_CAREGIVER', 'ACTIVE', CURRENT_TIMESTAMP)
  ON CONFLICT (caregiver_id, patient_id) DO UPDATE 
  SET status = 'ACTIVE', linked_at = CURRENT_TIMESTAMP;
END $$;

COMMIT;

-- 2. Truy vấn danh sách Care Recipients của Caregiver (Tab Đang chăm sóc vs Tab Lịch sử)
SELECT 
  p.patient_id, p.display_name, p.birth_year, p.surgery_eye, p.surgery_type, p.surgery_date,
  f.facility_name, f.hotline AS facility_hotline,
  cpl.relationship, cpl.role AS caregiver_role, cpl.linked_at,
  pcp.care_plan_id, pcp.status AS plan_status,
  CASE 
    WHEN pcp.status = 'ACTIVE' THEN 'ACTIVE'
    ELSE 'COMPLETED'
  END AS recipient_group,
  CURRENT_DATE - p.surgery_date AS post_op_days,
  -- Tỷ lệ hoàn thành cữ thuốc hôm nay
  (
    SELECT COUNT(*) 
    FROM medication_logs ml
    WHERE ml.care_plan_id = pcp.care_plan_id 
      AND ml.actual_taken_at::DATE = CURRENT_DATE
  ) AS today_taken_doses,
  -- Mức cảnh báo gần nhất
  COALESCE(
    (SELECT alert_level FROM recovery_check_submissions rcs 
     WHERE rcs.care_plan_id = pcp.care_plan_id 
     ORDER BY rcs.submitted_at DESC LIMIT 1),
    'GREEN'
  ) AS latest_alert_level
FROM caregiver_patient_links cpl
JOIN patients p ON cpl.patient_id = p.patient_id
JOIN facilities f ON p.facility_id = f.facility_id
LEFT JOIN patient_care_plans pcp ON p.patient_id = pcp.patient_id AND pcp.status IN ('ACTIVE', 'COMPLETED', 'ARCHIVED')
WHERE cpl.caregiver_id = :caregiver_id 
  AND cpl.status = 'ACTIVE'
ORDER BY 
  CASE WHEN pcp.status = 'ACTIVE' THEN 0 ELSE 1 END,
  p.surgery_date DESC;

-- 3. Truy vấn Dòng thời gian Lịch sử Chăm sóc Chi tiết (Care History Timeline - F-003, F-009, F-016)
-- Kết hợp lịch sử dùng thuốc (kèm tên người đã cho uống) và lịch sử khảo sát phục hồi
WITH med_history AS (
  SELECT 
    ml.actual_taken_at AS event_time,
    'MEDICATION_TAKEN' AS event_type,
    'Đã dùng thuốc: ' || pms.medication_name AS event_title,
    jsonb_build_object(
      'medication_name', pms.medication_name,
      'dosage_form', pms.dosage_form,
      'dose_amount', pms.dose_amount,
      'target_eye', pms.target_eye,
      'scheduled_time', ml.scheduled_time,
      'confirmed_by_role', ml.confirmed_by_role,
      'confirmed_by_name', COALESCE(cp.full_name, 'Bệnh nhân tự xác nhận')
    ) AS event_details
  FROM medication_logs ml
  JOIN patient_medication_schedules pms ON ml.schedule_id = pms.schedule_id
  LEFT JOIN caregiver_profiles cp ON ml.confirmed_by_user_id = cp.caregiver_id
  WHERE ml.care_plan_id = :care_plan_id
),
survey_history AS (
  SELECT 
    rcs.submitted_at AS event_time,
    'RECOVERY_CHECK' AS event_type,
    'Khảo sát phục hồi: Mức ' || rcs.alert_level AS event_title,
    jsonb_build_object(
      'submission_id', rcs.submission_id,
      'alert_level', rcs.alert_level,
      'answers_count', (SELECT COUNT(*) FROM recovery_check_answers rca WHERE rca.submission_id = rcs.submission_id)
    ) AS event_details
  FROM recovery_check_submissions rcs
  WHERE rcs.care_plan_id = :care_plan_id
)
SELECT event_time, event_type, event_title, event_details
FROM med_history
UNION ALL
SELECT event_time, event_type, event_title, event_details
FROM survey_history
ORDER BY event_time DESC
LIMIT :limit OFFSET :offset;
```

---

### 2.3 Phân Hệ Cấu Hình Master Care Plan Template (Templates CRUD)

#### UC-005.1: Khởi Tạo Master Template Bản Thảo (DRAFT)
```sql
INSERT INTO care_plan_templates (
  template_id, template_name, surgery_type, version, clinical_description, 
  followup_days, status, created_by_doctor_id, created_at
)
VALUES (
  gen_random_uuid(), :template_name, :surgery_type, 'v1.0', :clinical_description,
  :followup_days, 'DRAFT', :doctor_id, CURRENT_TIMESTAMP
)
RETURNING template_id, version, status;
```

#### UC-005.4: Chỉnh Sửa Hoặc Nhân Bản Phiên Bản Mới (Copy-on-Write BR15)
```sql
-- Trường hợp 1: Template đang ở trạng thái DRAFT -> Cho phép sửa trực tiếp
UPDATE care_plan_templates
SET template_name = :template_name,
    clinical_description = :clinical_description,
    followup_days = :followup_days,
    updated_at = CURRENT_TIMESTAMP
WHERE template_id = :template_id AND status = 'DRAFT';

-- Trường hợp 2: Template đang ACTIVE -> Cơ chế Copy-on-Write: Nhân bản sang phiên bản mới (v1.1)
BEGIN;

INSERT INTO care_plan_templates (
  template_id, template_name, surgery_type, version, clinical_description,
  followup_days, status, parent_template_id, created_by_doctor_id, created_at
)
SELECT 
  :new_template_id, :template_name, surgery_type, 'v1.1', :clinical_description,
  :followup_days, 'DRAFT', template_id, :doctor_id, CURRENT_TIMESTAMP
FROM care_plan_templates
WHERE template_id = :old_template_id AND status = 'ACTIVE';

-- Sao chép thuốc mẫu sang phiên bản mới
INSERT INTO template_medications (med_id, template_id, medication_name, dosage_form, dose_amount, target_eye, frequency_schedules, buffer_interval_minutes, cap_color, instructions, display_order)
SELECT gen_random_uuid(), :new_template_id, medication_name, dosage_form, dose_amount, target_eye, frequency_schedules, buffer_interval_minutes, cap_color, instructions, display_order
FROM template_medications WHERE template_id = :old_template_id;

-- Sao chép các mốc khảo sát, tiêu chí Red Flag, bài học Do/Don't tương tự...
COMMIT;
```

#### UC-006: Quy Trình Gửi Duyệt & Phê Duyệt Phác Đồ Mẫu (GCMO Approval BR15)
```sql
-- 1. Bác sĩ gửi yêu cầu phê duyệt sau khi hoàn thiện bản thảo DRAFT
UPDATE care_plan_templates
SET status = 'PENDING_APPROVAL',
    updated_at = CURRENT_TIMESTAMP
WHERE template_id = :template_id 
  AND status = 'DRAFT'
  -- Điều kiện tiên quyết: Phải có ít nhất 1 loại thuốc mẫu và 1 mốc khảo sát
  AND EXISTS (SELECT 1 FROM template_medications WHERE template_id = :template_id)
  AND EXISTS (SELECT 1 FROM template_recovery_milestones WHERE template_id = :template_id);

-- 2. Giám đốc Chuyên môn (GCMO) thẩm định và ký duyệt số hóa đưa vào sử dụng
BEGIN;

UPDATE care_plan_templates
SET status = 'ACTIVE',
    approved_by = :gcmo_account_id,
    approved_at = CURRENT_TIMESTAMP,
    approval_notes = :approval_notes,
    updated_at = CURRENT_TIMESTAMP
WHERE template_id = :template_id 
  AND status = 'PENDING_APPROVAL';

-- Lưu vết kiểm toán hành động phê duyệt phác đồ
INSERT INTO audit_logs (user_id, action_type, target_entity, target_id, new_data, timestamp)
VALUES (:gcmo_account_id, 'APPROVE_CARE_PLAN_TEMPLATE', 'care_plan_templates', :template_id::TEXT,
        jsonb_build_object('approval_notes', :approval_notes, 'approved_at', CURRENT_TIMESTAMP), CURRENT_TIMESTAMP);

COMMIT;
```

#### UC-005.5: Cập Nhật Trạng Thái Vòng Đời Template
```sql
UPDATE care_plan_templates
SET status = :new_status,
    updated_at = CURRENT_TIMESTAMP
WHERE template_id = :template_id;
```

#### UC-007.1: Thêm Thuốc Mẫu Vào Master Template (Drop Buffer Timer BR23)
```sql
INSERT INTO template_medications (
  med_id, template_id, medication_name, dosage_form, dose_amount,
  target_eye, frequency_schedules, buffer_interval_minutes, cap_color, instructions, display_order
)
VALUES (
  gen_random_uuid(), :template_id, :medication_name, :dosage_form, :dose_amount,
  :target_eye, :frequency_schedules::JSONB, :buffer_interval_minutes, :cap_color, :instructions, :display_order
);
```

#### UC-008.1: Thêm Mốc Khảo Sát & Bộ Câu Hỏi Recovery Check (BR24)
```sql
BEGIN;

-- 1. Thêm mốc khảo sát
INSERT INTO template_recovery_milestones (milestone_id, template_id, milestone_name, day_offset, reminder_time)
VALUES (:milestone_id, :template_id, :milestone_name, :day_offset, :reminder_time);

-- 2. Thêm câu hỏi phân loại 3 mức Xanh/Vàng/Đỏ
INSERT INTO template_recovery_questions (question_id, milestone_id, question_text, options_json, display_order)
VALUES (gen_random_uuid(), :milestone_id, :question_text, :options_json::JSONB, 1);

COMMIT;
```

#### UC-008.5: Thêm Tiêu Chí Báo Động Đỏ Red Flag (Hotline 0395 151 151 BR11)
```sql
INSERT INTO template_red_flags (
  red_flag_id, template_id, symptom_name, first_aid_guide, emergency_hotline, sla_minutes
)
VALUES (
  gen_random_uuid(), :template_id, :symptom_name, :first_aid_guide, '0395 151 151', 5
);
```

---

### 2.4 Phân Hệ Khởi Tạo Care Plan & Vòng Đời QR Bàn Giao

#### UC-010: Nhân Bản Master Template Thành Care Plan Cá Nhân Hóa (<30s BR18)
```sql
BEGIN;

-- 1. Tạo Kế hoạch chăm sóc độc lập cho bệnh nhân
INSERT INTO patient_care_plans (
  care_plan_id, patient_id, template_id, template_name, surgery_type,
  start_date, end_date, status, created_by_staff_id, created_at
)
SELECT 
  :new_care_plan_id, p.patient_id, t.template_id, t.template_name, t.surgery_type,
  CURRENT_DATE, CURRENT_DATE + (t.followup_days || ' days')::INTERVAL, 'ACTIVE', :staff_id, CURRENT_TIMESTAMP
FROM patients p, care_plan_templates t
WHERE p.patient_id = :patient_id AND t.template_id = :template_id;

-- 2. Nhân bản danh mục thuốc mẫu (cho phép Bác sĩ tùy biến liều)
INSERT INTO patient_medication_schedules (
  schedule_id, care_plan_id, medication_name, dosage_form, dose_amount,
  target_eye, frequency_schedule, buffer_interval_minutes, cap_color, instructions
)
SELECT 
  gen_random_uuid(), :new_care_plan_id, tm.medication_name, tm.dosage_form, 
  COALESCE(:custom_dose_amount, tm.dose_amount),
  tm.target_eye, tm.frequency_schedules, tm.buffer_interval_minutes, tm.cap_color, tm.instructions
FROM template_medications tm
WHERE tm.template_id = :template_id;

-- 3. Tạo sẵn 5 mốc lịch hẹn tái khám chuẩn VISI (Ngày 1, 3, 7, 14, 30)
INSERT INTO patient_followup_appointments (
  appointment_id, care_plan_id, milestone_name, scheduled_date, facility_id, status
)
SELECT 
  gen_random_uuid(), :new_care_plan_id, 
  CASE day_offset 
    WHEN 1 THEN 'Tái khám Ngày 1 sau mổ'
    WHEN 3 THEN 'Tái khám Ngày 3 sau mổ'
    WHEN 7 THEN 'Tái khám Ngày 7 sau mổ'
    WHEN 14 THEN 'Tái khám 2 Tuần sau mổ'
    WHEN 30 THEN 'Tái khám 1 Tháng sau mổ'
  END,
  CURRENT_DATE + (day_offset || ' days')::INTERVAL,
  p.facility_id,
  'SCHEDULED'
FROM (VALUES (1), (3), (7), (14), (30)) AS offsets(day_offset), patients p
WHERE p.patient_id = :patient_id;

COMMIT;
```

#### UC-011: Tạo và Phát Hành Mã QR Xuất Viện (Điều dưỡng / Bác sĩ UC-011)
```sql
INSERT INTO patient_qr_codes (
  qr_id, care_plan_id, qr_token, status, issued_by_account_id, issued_at, expires_at
)
VALUES (
  gen_random_uuid(), :care_plan_id, :generated_hmac_token, 'ISSUED', :staff_account_id,
  CURRENT_TIMESTAMP, CURRENT_TIMESTAMP + INTERVAL '30 days'
)
RETURNING qr_id, qr_token;
```

#### UC-013: Thu Hồi & Cấp Lại Mã QR Bàn Giao
```sql
BEGIN;

-- 1. Thu hồi mã QR cũ
UPDATE patient_qr_codes 
SET status = 'REVOKED', revoked_at = CURRENT_TIMESTAMP, revoke_reason = :reason
WHERE care_plan_id = :care_plan_id AND status = 'ISSUED';

-- 2. Sinh mã QR mới
INSERT INTO patient_qr_codes (qr_id, care_plan_id, qr_token, status, issued_by_account_id, issued_at, expires_at)
VALUES (gen_random_uuid(), :care_plan_id, :new_qr_token, 'ISSUED', :staff_account_id, CURRENT_TIMESTAMP, CURRENT_TIMESTAMP + INTERVAL '30 days');

COMMIT;
```

---

### 2.5 Phân Hệ Vận Hành Chăm Sóc Hàng Ngày (Care Operations)

#### UC-015: Ghi Nhận Dùng Thuốc & Kích Hoạt Buffer Timer Giãn Cách
```sql
-- 1. Lưu bản ghi lịch sử dùng thuốc
INSERT INTO medication_logs (
  log_id, care_plan_id, schedule_id, confirmed_by_user_id, confirmed_by_role, 
  scheduled_time, actual_taken_at, notes
)
VALUES (
  gen_random_uuid(), :care_plan_id, :schedule_id, :user_id, :role,
  :scheduled_time, CURRENT_TIMESTAMP, :notes
)
RETURNING log_id, actual_taken_at;

-- 2. Kiểm tra cữ dùng thuốc tiếp theo và thời gian đệm
SELECT pms.medication_name, pms.buffer_interval_minutes
FROM patient_medication_schedules pms
WHERE pms.schedule_id = :schedule_id;
```

#### UC-019: Lưu Kết Quả Khảo Sát Phục Hồi Định Kỳ (Recovery Check)
```sql
BEGIN;

-- 1. Lưu bản ghi nộp khảo sát tổng quát
INSERT INTO recovery_check_submissions (
  submission_id, care_plan_id, milestone_id, submitted_at, alert_level
)
VALUES (
  :new_submission_id, :care_plan_id, :milestone_id, CURRENT_TIMESTAMP, :calculated_alert_level
);

-- 2. Lưu chi tiết từng câu trả lời
INSERT INTO recovery_check_answers (answer_id, submission_id, question_id, selected_option_text, alert_level)
VALUES (gen_random_uuid(), :new_submission_id, :question_id, :option_text, :option_alert_level);

-- 3. Nếu có câu trả lời Mức ĐỎ: tự động kích hoạt sự cố Red Flag (UC-020)
IF :calculated_alert_level = 'RED' THEN
  INSERT INTO red_flag_incidents (
    incident_id, care_plan_id, trigger_source, symptom_summary, status, triggered_at
  )
  VALUES (
    gen_random_uuid(), :care_plan_id, 'RECOVERY_CHECK', :red_symptom_summary, 'TRIGGERED', CURRENT_TIMESTAMP
  );
END IF;

COMMIT;
```

#### UC-020: Kích Hoạt Cảnh Báo Khẩn Cấp Báo Động Đỏ Red Flag
```sql
INSERT INTO red_flag_incidents (
  incident_id, care_plan_id, trigger_source, symptom_summary, emergency_contact_phone, status, triggered_at
)
VALUES (
  gen_random_uuid(), :care_plan_id, 'CAREGIVER_EMERGENCY_BUTTON', :symptom_summary, :contact_phone, 'TRIGGERED', CURRENT_TIMESTAMP
)
RETURNING incident_id, triggered_at;
```

---

### 2.6 Phân Hệ Giám Sát Lâm Sàng & Điều Phối Cảnh Báo (Clinical Dashboard)

#### UC-021: Truy Vấn Dashboard Giám Sát Tập Trung 3 Tầng Màu
```sql
WITH latest_checks AS (
  SELECT rcs.care_plan_id, rcs.alert_level, rcs.submitted_at,
         ROW_NUMBER() OVER(PARTITION BY rcs.care_plan_id ORDER BY rcs.submitted_at DESC) AS rn
  FROM recovery_check_submissions rcs
),
pending_red_flags AS (
  SELECT care_plan_id, incident_id, triggered_at, status AS alert_status
  FROM red_flag_incidents
  WHERE status IN ('TRIGGERED', 'IN_PROGRESS')
)
SELECT 
  pcp.care_plan_id, p.display_name, p.phone, p.surgery_type, p.surgery_eye, p.surgery_date,
  COALESCE(prf.alert_status, 'NORMAL') AS incident_state,
  CASE 
    WHEN prf.incident_id IS NOT NULL THEN 'RED'
    WHEN lc.alert_level = 'YELLOW' THEN 'YELLOW'
    ELSE 'GREEN'
  END AS patient_status_tier,
  COALESCE(prf.triggered_at, lc.submitted_at) AS last_event_time
FROM patient_care_plans pcp
JOIN patients p ON pcp.patient_id = p.patient_id
LEFT JOIN latest_checks lc ON pcp.care_plan_id = lc.care_plan_id AND lc.rn = 1
LEFT JOIN pending_red_flags prf ON pcp.care_plan_id = prf.care_plan_id
WHERE p.facility_id = :facility_id 
  AND pcp.status = 'ACTIVE'
ORDER BY 
  CASE 
    WHEN prf.incident_id IS NOT NULL THEN 1
    WHEN lc.alert_level = 'YELLOW' THEN 2
    ELSE 3
  END ASC,
  last_event_time DESC NULLS LAST;
```

#### UC-022: Tiếp Nhận Xử Lý Red Flag (SLA <5p BR12)
```sql
UPDATE red_flag_incidents
SET status = 'IN_PROGRESS',
    acknowledged_by_doctor_id = :staff_id,
    acknowledged_at = CURRENT_TIMESTAMP
WHERE incident_id = :incident_id 
  AND status = 'TRIGGERED';
```

#### UC-022b: Truy Vấn Daemon Tự Động Leo Thang Cảnh Báo Quá Hạn (>15 phút)
```sql
-- Tìm các ca Red Flag chưa được tiếp nhận sau 15 phút
SELECT incident_id, care_plan_id, triggered_at,
       EXTRACT(EPOCH FROM (CURRENT_TIMESTAMP - triggered_at))/60 AS minutes_elapsed
FROM red_flag_incidents
WHERE status = 'TRIGGERED'
  AND triggered_at < CURRENT_TIMESTAMP - INTERVAL '15 minutes'
  AND acknowledged_at IS NULL
  AND escalation_level = 1;

-- Cập nhật leo thang cảnh báo sang cấp 2 (Gửi SMS khẩn cấp tới Bác sĩ trực)
UPDATE red_flag_incidents
SET escalated_at = CURRENT_TIMESTAMP,
    escalation_level = 2
WHERE incident_id = :incident_id;
```

#### UC-023: Ghi Nhận & Tra Cứu Nhật Ký Cuộc Gọi Can Thiệp Lâm Sàng
```sql
-- 1. Lưu bản ghi cuộc gọi can thiệp của CSKH / Điều dưỡng
BEGIN;

INSERT INTO call_intervention_logs (
  log_id, incident_id, patient_id, caller_account_id, call_time,
  duration_seconds, call_status, notes, next_action, created_at
)
VALUES (
  gen_random_uuid(), :incident_id, :patient_id, :caller_account_id, CURRENT_TIMESTAMP,
  :duration_seconds, :call_status, :notes, :next_action, CURRENT_TIMESTAMP
);

-- 2. Nếu cuộc gọi giải quyết thành công hoặc yêu cầu tới viện: cập nhật sự cố
UPDATE red_flag_incidents
SET status = CASE 
      WHEN :next_action = 'CONTINUE_MONITORING' THEN 'RESOLVED'
      WHEN :next_action = 'REQUIRE_HOSPITAL_EXAM' THEN 'IN_PROGRESS'
      ELSE status 
    END,
    clinical_resolution = :notes,
    resolved_at = CASE WHEN :next_action = 'CONTINUE_MONITORING' THEN CURRENT_TIMESTAMP ELSE NULL END
WHERE incident_id = :incident_id;

COMMIT;

-- 3. Truy xuất toàn bộ lịch sử các cuộc gọi can thiệp của một sự cố Red Flag
SELECT cil.log_id, cil.call_time, cil.duration_seconds, cil.call_status,
       cil.notes, cil.next_action, cil.created_at,
       a.full_name AS caller_name, a.role AS caller_role
FROM call_intervention_logs cil
JOIN accounts a ON cil.caller_account_id = a.account_id
WHERE cil.incident_id = :incident_id
ORDER BY cil.call_time ASC;
```

---

### 2.7 Phân Hệ Báo Cáo & Kiểm Toán An Toàn Thông Tin (Audit & KPIs)

#### UC-027: Lọc và Truy Vấn Nhật Ký Kiểm Toán (Audit Trail)
```sql
SELECT audit_id, user_id, action_type, target_entity, target_id, 
       old_data, new_data, ip_address, user_agent, timestamp
FROM audit_logs
WHERE (:user_id IS NULL OR user_id = :user_id)
  AND (:action_type IS NULL OR action_type = :action_type)
  AND (:target_entity IS NULL OR target_entity = :target_entity)
  AND (:from_date IS NULL OR timestamp >= :from_date)
  AND (:to_date IS NULL OR timestamp <= :to_date)
ORDER BY timestamp DESC
LIMIT :limit OFFSET :offset;
```

#### UC-028: Kết Xuất Thống Kê Báo Cáo Chỉ Số KPI
```sql
SELECT 
  p.facility_id,
  COUNT(DISTINCT p.patient_id) AS total_patients,
  -- Tỷ lệ kích hoạt mã QR (Mục tiêu ≥85%)
  ROUND(
    COUNT(DISTINCT CASE WHEN cpl.status = 'ACTIVE' THEN p.patient_id END)::NUMERIC / 
    NULLIF(COUNT(DISTINCT p.patient_id), 0) * 100, 2
  ) AS qr_activation_rate,
  -- Tỷ lệ tuân thủ dùng thuốc đúng giờ
  ROUND(
    COUNT(DISTINCT ml.log_id)::NUMERIC / 
    NULLIF(COUNT(DISTINCT pms.schedule_id) * 7, 0) * 100, 2
  ) AS medication_adherence_rate,
  -- Tỷ lệ tái khám đúng hẹn
  ROUND(
    COUNT(DISTINCT CASE WHEN pfa.status = 'COMPLETED' THEN pfa.appointment_id END)::NUMERIC / 
    NULLIF(COUNT(DISTINCT pfa.appointment_id), 0) * 100, 2
  ) AS followup_rate
FROM patients p
JOIN patient_care_plans pcp ON p.patient_id = pcp.patient_id
LEFT JOIN caregiver_patient_links cpl ON p.patient_id = cpl.patient_id
LEFT JOIN patient_medication_schedules pms ON pcp.care_plan_id = pms.care_plan_id
LEFT JOIN medication_logs ml ON pms.schedule_id = ml.schedule_id
LEFT JOIN patient_followup_appointments pfa ON pcp.care_plan_id = pfa.care_plan_id
WHERE p.created_at >= :start_date AND p.created_at <= :end_date
GROUP BY p.facility_id;
```

---
