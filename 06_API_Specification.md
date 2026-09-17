# 06. ĐẶC TẢ GIAO DIỆN LẬP TRÌNH ỨNG DỤNG (RESTFUL API SPECIFICATION)
# HỆ THỐNG REMICARE OPHTHALMIC POST-OP PLATFORM

> **Dự án:** RemiCare Ophthalmic Post-Op Platform (Nền tảng Hướng dẫn và Giám sát Chăm sóc Hậu phẫu Nhãn khoa)  
> **Doanh nghiệp mục tiêu:** Công ty Cổ phần Tập đoàn Y khoa VISI (VISI Medical Group)  
> **Phiên bản tài liệu:** V1 (Chuẩn hóa toàn diện — Tách biệt độc lập khỏi Use Case nghiệp vụ)  
> **Ngày phê duyệt:** 15/09/2026  
> **Tiêu chuẩn kỹ thuật:** RESTful API Level 3 (Richardson Maturity Model), OpenAPI 3.1, JSON:API, JWT Authentication (RFC 7519), Nghị định 13/2023/NĐ-CP  

---

## 1. NGUYÊN TẮC THIẾT KẾ VÀ QUY CHUẨN KỸ THUẬT CHUNG

### 1.1 Thông Tin Môi Trường & Base URLs
* **Production Gateway:** `https://api.remicare.visi.vn/api/v1`
* **Staging / UAT Gateway:** `https://staging-api.remicare.visi.vn/api/v1`
* **Local Development:** `http://localhost:8080/api/v1`

### 1.2 Định Danh & Xác Thực (Authentication & Security)
* Toàn bộ các API (ngoại trừ các endpoint công khai như gửi OTP, xác thực 2FA) đều yêu cầu Header:
  ```http
  Authorization: Bearer <JWT_ACCESS_TOKEN>
  Content-Type: application/json
  Accept: application/json
  X-Facility-ID: <FACILITY_UUID>
  ```
* **Thời gian sống của Token:**
  * Access Token: 15 phút (chứa `user_id`, `role`, `facility_id`).
  * Refresh Token: 7 ngày (lưu trữ HttpOnly Cookie an toàn, xoay vòng - Token Rotation).
* **Phân quyền RBAC:** Áp dụng chặt chẽ 7 vai trò người dùng (Giám đốc Bệnh viện, Bác sĩ, Điều dưỡng, CSKH, Caregiver, Admin, Care Recipient).

### 1.3 Cấu Trúc Khung Phản Hồi Chuẩn (Standard Response Envelope)

#### Phản Hồi Thành Công (HTTP 200 OK / HTTP 201 Created):
```json
{
  "success": true,
  "code": 200,
  "message": "Thao tác thành công",
  "data": {},
  "meta": {
    "timestamp": "2026-09-15T15:30:00.000Z",
    "request_id": "req-8f4b2c1a-9e3d"
  }
}
```

#### Phản Hồi Phân Trang (Paginated Response):
```json
{
  "success": true,
  "code": 200,
  "data": [],
  "pagination": {
    "page": 1,
    "limit": 20,
    "total_records": 145,
    "total_pages": 8
  }
}
```

#### Phản Hồi Lỗi Chuẩn (Error Response Envelope):
```json
{
  "success": false,
  "code": 400,
  "error_code": "INVALID_BUFFER_TIME",
  "message": "Thời gian đệm giữa các loại thuốc nhỏ mắt tối thiểu phải là 5 phút (BR23)",
  "details": [
    {
      "field": "buffer_interval_minutes",
      "issue": "Must be greater than or equal to 5"
    }
  ],
  "meta": {
    "timestamp": "2026-09-15T15:30:00.000Z",
    "request_id": "req-8f4b2c1a-9e3d"
  }
}
```

### 1.4 Đặc Tả Giới Hạn Tần Suất Truy Cập (Rate Limiting Specification)
Áp dụng cơ chế Token Bucket / Sliding Window tại API Gateway (Redis-backed) nhằm chống tấn công Brute-force, Spam SMS OTP và DoS:

| Phạm Vi Áp Dụng | Ngưỡng Giới Hạn (Threshold) | Cơ Chế Phạt Khi Vi Phạm | Căn Cứ Nghiệp Vụ |
|:---|:---|:---|:---|
| **Yêu cầu OTP (`/auth/otp/request`)** | Tối đa **5 OTP / SĐT / giờ**<br>Tối đa **10 OTP / Địa chỉ IP / giờ** | Trả về HTTP 429 `TOO_MANY_REQUESTS`, khóa gửi OTP tạm thời 60 phút | Chống spam SMS Brandname và cạn kiệt chi phí viễn thông (BR-NEW-04) |
| **Xác thực OTP (`/auth/otp/verify`)** | Tối đa **5 lần nhập sai liên tiếp** | Khóa phiên OTP hiện tại, hủy mã, yêu cầu chờ 15 phút mới được xin mã mới | Chống tấn công dò mã 6 số (BR2, UC-001) |
| **Đăng nhập Nhân viên (`/auth/staff/login`)** | Tối đa **5 lần sai mật khẩu liên tiếp** | Khóa tạm thời tài khoản 15 phút (`status = 'LOCKED'`), gửi cảnh báo email | Chống brute-force mật khẩu nhân sự y tế (BR-NEW-05) |
| **API Nghiệp vụ chung (Authenticated)** | **120 requests / phút / Account** | HTTP 429, Header `Retry-After: <seconds>` | Bảo vệ tài nguyên máy chủ cơ sở dữ liệu |
| **Báo động Red Flag (`/alerts/red-flag`)** | **Không giới hạn (Unthrottled)** | Không chặn cuộc gọi cấp cứu | Đảm bảo tính mạng người bệnh tuyệt đối |

### 1.5 Đặc Tả Giao Tiếp Thời Gian Thực (WebSocket & SSE Specification)
Phục vụ bảng điều khiển trực quan tại phòng trực điều dưỡng và bàn CSKH cơ sở (UC-021, UC-022, UC-022b):

* **Giao thức:** WebSocket (`wss://api.remicare.visi.vn/ws/v1`) hoặc Server-Sent Events (`GET /api/v1/stream/alerts`).
* **Endpoint kết nối chi nhánh:**
  ```http
  wss://api.remicare.visi.vn/ws/v1/facilities/{facility_id}/alerts?token=<JWT_ACCESS_TOKEN>
  ```
* **Sự kiện phát sóng (Broadcast Events):**
  1. `RED_FLAG_TRIGGERED`: Kích hoạt ngay khi bệnh nhân nộp bảng kiểm có câu Đỏ hoặc bấm nút SOS tại nhà. Frontend lập tức phát chuông báo động cấp 1 và chớp đỏ màn hình.
     ```json
     {
       "event": "RED_FLAG_TRIGGERED",
       "facility_id": "fac-001-uuid",
       "incident_id": "inc-0091-uuid",
       "patient_id": "BN-2026-00012",
       "patient_name_masked": "Ng. V. An",
       "surgery_type": "PHACO",
       "triggered_at": "2026-09-15T08:15:30Z",
       "sla_countdown_seconds": 300,
       "emergency_hotline": "0395 151 151"
     }
     ```
  2. `ESCALATION_TRIGGERED`: Tự động kích hoạt sau 15 phút sự cố không có người tiếp nhận. Frontend nâng mức chuông cảnh báo cấp 2 (UC-022b).
  3. `ALERT_CLAIMED`: Khi một nhân viên CSKH/Bác sĩ bấm "Tiếp nhận ca", chuông ngừng kêu trên toàn bộ các máy trạm khác cùng chi nhánh.

---

## 2. DANH MỤC API CHI TIẾT THEO PHÂN HỆ NGHIỆP VỤ

### 2.1 Phân Hệ Xác Thực & Quản Trị Nhân Sự (Auth & Staff RBAC)

#### [POST] `/api/v1/auth/otp/request` — Yêu Cầu Gửi Mã OTP (UC-001)
* **Mô tả:** Gửi mã OTP 6 số qua SMS/ZNS cho Caregiver hoặc Bệnh nhân đăng nhập.
* **Quyền hạn:** Public
* **Request Body:**
  ```json
  {
    "phone": "0987654321",
    "role": "CAREGIVER"
  }
  ```
* **Response (200 OK):**
  ```json
  {
    "success": true,
    "code": 200,
    "message": "Mã OTP đã được gửi đến số điện thoại",
    "data": {
      "expires_in_seconds": 120,
      "retry_after_seconds": 60
    }
  }
  ```

#### [POST] `/api/v1/auth/otp/verify` — Xác Thực OTP & Đăng Nhập (UC-001)
* **Request Body:**
  ```json
  {
    "phone": "0987654321",
    "otp_code": "123456"
  }
  ```
* **Response (200 OK):**
  ```json
  {
    "success": true,
    "code": 200,
    "data": {
      "access_token": "eyJhbGciOi...",
      "token_type": "Bearer",
      "expires_in": 900,
      "user": {
        "user_id": "9b1deb4d-3b7d-4bad-9bdd-2b0d7b3dcb6d",
        "phone": "0987654321",
        "role": "CAREGIVER",
        "full_name": "Nguyễn Văn Thân"
      }
    }
  }
  ```

#### [POST] `/api/v1/auth/staff/login` — Đăng Nhập Nhân Viên Y Tế (UC-002)
* **Request Body:**
  ```json
  {
    "username": "bacsi.kien@visi.vn",
    "password": "SecurePassword123!"
  }
  ```
* **Response (200 OK):** Cấp token tạm thời yêu cầu bước 2FA.

#### [POST] `/api/v1/auth/staff/2fa` — Xác Thực 2FA Nhân Viên Y Tế (UC-002)
* **Request Body:**
  ```json
  {
    "temp_token": "eyJhbGciOi...",
    "two_factor_code": "482910"
  }
  ```
* **Response (200 OK):** Trả về Access Token và quyền hạn chi nhánh (`facility_id`).

#### [POST] `/api/v1/staff` — Khởi Tạo Tài Khoản Nhân Viên (UC-026.1)
* **Quyền hạn:** `ADMIN`
* **Request Body:**
  ```json
  {
    "full_name": "BS. Lê Thị Bích",
    "email": "bich.lt@visi.vn",
    "phone": "0912345678",
    "role": "DOCTOR",
    "facility_id": "fac-001-thu-duc"
  }
  ```
* **Response (201 Created):**
  ```json
  {
    "success": true,
    "code": 201,
    "data": {
      "account_id": "acc-9921-uuid",
      "status": "PENDING_ACTIVATION"
    }
  }
  ```

#### [GET] `/api/v1/staff` — Danh Sách Nhân Viên Theo Cơ Sở (UC-026.2)
* **Quyền hạn:** `ADMIN`, `HOSPITAL_DIRECTOR`
* **Query Params:** `facility_id`, `role`, `status`, `page`, `limit`
* **Response (200 OK):** Danh sách nhân sự phân trang.

#### [PUT] `/api/v1/staff/{id}` — Cập Nhật Thông Tin & Vai Trò RBAC (UC-026.3)
* **Quyền hạn:** `ADMIN`
* **Request Body:**
  ```json
  {
    "full_name": "BS. Lê Thị Bích",
    "role": "DOCTOR",
    "facility_id": "fac-002-hai-phong"
  }
  ```

#### [DELETE] `/api/v1/staff/{id}` — Khóa / Vô Hiệu Hóa Tài Khoản (UC-026.4)
* **Quyền hạn:** `ADMIN`
* **Request Body:** `{"reason": "Nhân viên nghỉ việc"}`
* **Response (200 OK):** Chuyển `status = 'LOCKED'` và đẩy token vào blacklist.

#### [GET] `/api/v1/facilities` — Danh Sách Cơ Sở / Chi Nhánh Y Tế VISI (BR26)
* **Quyền hạn:** Public / Authenticated
* **Mô tả:** Lấy danh sách 5 cơ sở thuộc Hệ thống Bệnh viện Mắt VISI phục vụ bộ lọc đa chi nhánh, chọn cơ sở xuất viện hoặc hiển thị hotline cấp cứu địa phương.
* **Query Params:** `is_active` (boolean, mặc định `true`)
* **Response (200 OK):**
  ```json
  {
    "success": true,
    "code": 200,
    "message": "Lấy danh sách cơ sở y tế thành công",
    "data": [
      {
        "facility_id": "fac-001-thu-duc",
        "facility_code": "VISI-TD",
        "facility_name": "Bệnh viện Mắt Kỹ thuật cao VISI Thủ Đức",
        "address": "215 Võ Văn Ngân, P. Linh Chiểu, TP. Thủ Đức, TP. HCM",
        "phone": "028 3896 1234",
        "hotline": "0395 151 151",
        "is_active": true
      },
      {
        "facility_id": "fac-002-hai-phong",
        "facility_code": "VISI-HP",
        "facility_name": "Bệnh viện Mắt VISI Hải Phòng",
        "address": "45 Lạch Tray, Ngô Quyền, Hải Phòng",
        "phone": "0225 385 5678",
        "hotline": "0395 151 152",
        "is_active": true
      }
    ]
  }
  ```

---

### 2.2 Phân Hệ Quản Lý Hồ Sơ Bệnh Nhân (Patient Profiles CRUD)

#### [POST] `/api/v1/patients` — Tạo Mới Hồ Sơ Bệnh Nhân (UC-004.1)
* **Quyền hạn:** `NURSE`, `RECEPTIONIST`, `DOCTOR`
* **Mô tả:** Nhập hồ sơ bệnh nhân mới (<30s). Bắt buộc họ tên, năm sinh, SĐT, mắt mổ, loại mổ.
* **Request Body:**
  ```json
  {
    "full_name": "Nguyễn Văn An",
    "phone": "0901234567",
    "birth_year": 1958,
    "gender": "MALE",
    "surgery_eye": "RIGHT_OD",
    "surgery_type": "PHACO",
    "surgery_date": "2026-09-15",
    "facility_id": "fac-001-thu-duc",
    "allergies_note": "Tiền sử dị ứng Aspirin"
  }
  ```
* **Response (201 Created):**
  ```json
  {
    "success": true,
    "code": 201,
    "data": {
      "patient_id": "pat-7712-42da-912b-bc22e89",
      "display_name": "Ng. V. An",
      "medical_record_code": "PAT-202609-0142",
      "status": "ACTIVE"
    }
  }
  ```

#### [GET] `/api/v1/patients` — Tra Cứu và Lọc Danh Sách Bệnh Nhân (UC-004.2)
* **Quyền hạn:** `DOCTOR`, `NURSE`, `CSKH`, `HOSPITAL_DIRECTOR`
* **Query Params:** `facility_id`, `surgery_type`, `alert_level`, `search`, `page`, `limit`
* **Response (200 OK):** Danh sách bệnh nhân kèm chỉ số cảnh báo Đỏ/Vàng/Xanh.

#### [GET] `/api/v1/patients/{id}` — Xem Chi Tiết Hồ Sơ & Dòng Thời Gian (UC-004.3)
* **Quyền hạn:** `DOCTOR`, `NURSE`, `CSKH` (chỉ xem thông tin cần thiết), `HOSPITAL_DIRECTOR` (theo quyền được cấp)
* **Response (200 OK):** Toàn bộ dữ liệu 360 độ: hành chính, Care Plan, lịch thuốc, khảo sát và danh sách Caregiver liên kết.

#### [PUT] `/api/v1/patients/{id}` — Cập Nhật Thông Tin Hồ Sơ (UC-004.4)
* **Quyền hạn:** `NURSE`, `DOCTOR`
* **Mô tả:** Cho phép cập nhật SĐT, tên hiển thị, ghi chú dị ứng. Khóa sửa loại phẫu thuật nếu Care Plan đã kích hoạt.
* **Request Body:**
  ```json
  {
    "phone": "0909888777",
    "allergies_note": "Cơ địa dị ứng bụi phấn hoa"
  }
  ```

#### [DELETE] `/api/v1/patients/{id}` — Lưu Trữ / Xóa Mềm Hồ Sơ (UC-004.5)
* **Quyền hạn:** `DOCTOR` (Trưởng khoa), `NURSE` (Điều dưỡng trưởng)
* **Request Body:** `{"reason": "Hoàn tất thời gian theo dõi hậu phẫu 90 ngày"}`
* **Response (200 OK):** Chuyển `status = 'ARCHIVED'`. Chặn nếu ca bệnh có Red Flag chưa xử lý.

#### [POST] `/api/v1/patient-links/claim` — Quét QR Bàn Giao Liên Kết Hồ Sơ (UC-003)
* **Quyền hạn:** `CAREGIVER`
* **Request Body:**
  ```json
  {
    "qr_token": "visi_qr_token_8a7f1c9e2b4d",
    "relationship": "CHILD"
  }
  ```
* **Response (200 OK):** Xác nhận liên kết thành công. Kiểm soát tối đa 03 Caregiver (BR5).

#### [GET] `/api/v1/caregiver/patients` — Danh Sách Bệnh Nhân & Lịch Sử Các Ca Chăm Sóc (F-003, UC-003)
* **Quyền hạn:** `CAREGIVER`
* **Mô tả:** Lấy toàn bộ danh sách các bệnh nhân mà Caregiver hiện tại đã liên kết, phân tách giữa các ca đang theo dõi (`ACTIVE`) và các ca đã hoàn tất trong quá khứ (`COMPLETED` / `ARCHIVED`).
* **Query Params:** `status` (tùy chọn: `ACTIVE`, `COMPLETED`, `ALL` - mặc định `ACTIVE`)
* **Response (200 OK):**
  ```json
  {
    "success": true,
    "code": 200,
    "data": {
      "active_patients": [
        {
          "patient_id": "pat-7712-42da-912b-bc22e89",
          "display_name": "Ng. V. An",
          "relationship": "CHILD",
          "surgery_type": "PHACO",
          "surgery_eye": "RIGHT_EYE",
          "surgery_date": "2026-09-15",
          "post_op_day": 2,
          "doctor_name": "BS.CKII Trần Bá Kiền",
          "facility_name": "Bệnh viện Mắt Kỹ thuật cao VISI Thủ Đức",
          "is_current_active": true,
          "today_medication_progress": {
            "completed_doses": 2,
            "total_doses": 4
          },
          "latest_alert_level": "GREEN"
        }
      ],
      "completed_patients": [
        {
          "patient_id": "pat-6601-uuid",
          "display_name": "Tr. T. Bình",
          "relationship": "CHILD",
          "surgery_type": "SILK",
          "surgery_eye": "LEFT_EYE",
          "surgery_date": "2026-06-10",
          "completed_at": "2026-07-10T17:00:00Z",
          "doctor_name": "BS. Lê Hoàng Nam",
          "facility_name": "Bệnh viện Mắt VISI Hải Phòng"
        }
      ]
    }
  }
  ```

#### [PUT] `/api/v1/caregiver/active-patient/{patient_id}` — Chuyển Đổi Bệnh Nhân Đang Kích Hoạt Chăm Sóc (F-003)
* **Quyền hạn:** `CAREGIVER`
* **Mô tả:** Chuyển đổi ngữ cảnh làm việc sang một người thân khác khi Caregiver chăm sóc nhiều bệnh nhân cùng lúc (Switch Active Care Recipient).
* **Response (200 OK):**
  ```json
  {
    "success": true,
    "code": 200,
    "message": "Đã chuyển đổi ngữ cảnh chăm sóc thành công",
    "data": {
      "active_patient_id": "pat-7712-42da-912b-bc22e89",
      "display_name": "Ng. V. An",
      "surgery_type": "PHACO",
      "post_op_day": 2
    }
  }
  ```

#### [GET] `/api/v1/caregiver/patients/{patient_id}/care-history` — Xem Dòng Thời Gian Lịch Sử Chăm Sóc Chi Tiết (F-003, F-009, F-016)
* **Quyền hạn:** `CAREGIVER`, `CARE_RECIPIENT`
* **Mô tả:** Trả về toàn bộ dòng thời gian (Timeline) các cữ thuốc đã nhỏ (kèm thông tin Caregiver nào đã xác nhận, thời điểm chính xác để tránh trùng liều giữa các thành viên gia đình), lịch sử kết quả Recovery Check và biên bản tư vấn của CSKH.
* **Query Params:** `from_date`, `to_date`, `type` (tùy chọn: `ALL`, `MEDICATION`, `RECOVERY_CHECK`, `CLINICAL_CALL`)
* **Response (200 OK):**
  ```json
  {
    "success": true,
    "code": 200,
    "data": [
      {
        "event_type": "MEDICATION_TAKEN",
        "timestamp": "2026-09-15T08:03:12Z",
        "title": "Nhỏ mắt cữ Sáng: Tobrex 0.3%",
        "details": {
          "medication_name": "Tobrex 0.3%",
          "target_eye": "SURGERY_EYE",
          "dose_amount": "1 giọt",
          "confirmed_by": "Nguyễn Thị Mai (Con gái)",
          "confirmed_by_role": "CAREGIVER",
          "buffer_duration_seconds": 300
        }
      },
      {
        "event_type": "RECOVERY_CHECK_SUBMITTED",
        "timestamp": "2026-09-15T08:15:00Z",
        "title": "Khảo sát phục hồi Sáng Ngày 1",
        "details": {
          "alert_level": "GREEN",
          "answers_summary": "Không đau nhức, thị lực sáng dần, không chảy mủ"
        }
      }
    ]
  }
  ```

---

### 2.3 Phân Hệ Master Care Plan Template (Cấu Hình Mẫu Chuẩn)

#### [POST] `/api/v1/templates` — Tạo Mới Master Template (UC-005.1)
* **Quyền hạn:** `DOCTOR`, `GCMO`
* **Mô tả:** Bác sĩ có quyền sao chép (clone) hoặc tạo mới template.
* **Request Body:**
  ```json
  {
    "template_name": "Phác đồ Chăm sóc Hậu phẫu Phaco Chuẩn Quốc Tế VISI",
    "surgery_type": "PHACO",
    "followup_days": 30,
    "clinical_description": "Áp dụng cho phẫu thuật đục thủy tinh thể phương pháp Phaco"
  }
  ```
* **Response (201 Created):**
  ```json
  {
    "success": true,
    "code": 201,
    "data": {
      "template_id": "tpl-8812-uuid",
      "version": "v1.0",
      "status": "DRAFT"
    }
  }
  ```

#### [GET] `/api/v1/templates` — Xem Danh Sách Master Template (UC-005.2)
* **Query Params:** `surgery_type`, `status`, `page`, `limit`
* **Response (200 OK):** Danh sách các template kèm số lượng thuốc và bài học liên kết.

#### [GET] `/api/v1/templates/{id}` — Xem Chi Tiết Cấu Hình Master Template (UC-005.3)
* **Response (200 OK):** Tải toàn diện 4 tab cấu hình: Thuốc mẫu, Khảo sát, Red Flag, Cẩm nang.

#### [PUT] `/api/v1/templates/{id}` — Chỉnh Sửa Thông Tin Chung Template (UC-005.4)
* **Request Body:**
  ```json
  {
    "template_name": "Phác đồ Phaco VISI Cập Nhật 2026",
    "followup_days": 30
  }
  ```
* **Ghi chú:** Nếu template đã `ACTIVE`, tự động nhân bản thành version `v1.1` ở trạng thái `DRAFT` (BR15).

#### [PATCH] `/api/v1/templates/{id}/status` — Kích Hoạt / Lưu Trữ Template (UC-005.5)
* **Quyền hạn:** `GCMO`
* **Request Body:** `{"status": "ACTIVE"}`

#### [POST] `/api/v1/templates/{id}/submit-approval` — Gửi Yêu Cầu Phê Duyệt Phác Đồ Mẫu (UC-006.1)
* **Quyền hạn:** `DOCTOR`
* **Mô tả:** Bác sĩ soạn thảo xong template ở trạng thái `DRAFT` gửi lên Ban Giám Đốc Chuyên Môn (GCMO) thẩm định.
* **Validation:** Kiểm tra bắt buộc có ít nhất 1 loại thuốc mẫu và 1 mốc khảo sát Recovery Check (BR15).
* **Response (200 OK):** Chuyển `status = 'PENDING_APPROVAL'`.

#### [POST] `/api/v1/templates/{id}/approve` — Phê Duyệt Lâm Sàng Ban Hành (UC-006.2)
* **Quyền hạn:** `GCMO` (Giám Đốc Chuyên Môn / Trưởng Khoa Mắt)
* **Mô tả:** Thẩm định nội dung y khoa, ký duyệt số hóa để chính thức đưa template vào danh mục áp dụng (`ACTIVE`).
* **Request Body:**
  ```json
  {
    "approval_action": "APPROVE",
    "approval_notes": "Đã thẩm định chuẩn y khoa theo tiêu chuẩn Bộ Y tế",
    "digital_signature_pin": "992811"
  }
  ```
* **Response (200 OK):**
  ```json
  {
    "success": true,
    "code": 200,
    "message": "Phác đồ mẫu đã được phê duyệt và kích hoạt thành công",
    "data": {
      "template_id": "tpl-8812-uuid",
      "version": "v1.0",
      "status": "ACTIVE",
      "approved_by": "acc-gcmo-uuid",
      "approved_at": "2026-09-15T09:00:00Z"
    }
  }
  ```

---

### 2.4 Phân Hệ Cấu Hình Chi Tiết Trong Template (Sub-Resources)

#### [POST] `/api/v1/templates/{id}/medications` — Thêm Thuốc Mẫu & Timer Đệm (UC-007.1)
* **Quyền hạn:** `DOCTOR`
* **Request Body:**
  ```json
  {
    "medication_name": "Tobrex 0.3%",
    "dosage_form": "EYE_DROPS",
    "dose_amount": "1 giọt",
    "target_eye": "SURGERY_EYE",
    "frequency_schedules": ["08:00", "12:00", "16:00", "20:00"],
    "buffer_interval_minutes": 5,
    "cap_color": "#FFC107",
    "instructions": "Kéo mi dưới nhỏ 1 giọt, nhắm mắt nhẹ 30 giây"
  }
  ```
* **Validation:** Bắt buộc `buffer_interval_minutes >= 5` đối với thuốc nhỏ mắt (BR23).

#### [GET] `/api/v1/templates/{id}/medications` — Danh Sách Thuốc Mẫu (UC-007.2)
#### [PUT] `/api/v1/templates/{id}/medications/{med_id}` — Chỉnh Sửa Thuốc Mẫu (UC-007.3)
#### [DELETE] `/api/v1/templates/{id}/medications/{med_id}` — Xóa Thuốc Mẫu (UC-007.4)

#### [POST] `/api/v1/templates/{id}/milestones` — Thêm Mốc & Câu Hỏi Recovery Check (UC-008.1)
* **Quyền hạn:** `DOCTOR`
* **Request Body:**
  ```json
  {
    "milestone_name": "Khảo sát Sáng Ngày 1 sau mổ",
    "day_offset": 1,
    "reminder_time": "08:00",
    "questions": [
      {
        "question_text": "Mắt phẫu thuật của bạn sáng nay có cảm thấy đau nhức dữ dội không?",
        "options": [
          {"text": "Không đau hoặc chỉ hơi cộm nhẹ", "alert_level": "GREEN"},
          {"text": "Đau âm ỉ nhưng giảm sau khi nghỉ ngơi", "alert_level": "YELLOW"},
          {"text": "Đau buốt dữ dội lan lên nửa đầu", "alert_level": "RED"}
        ]
      }
    ]
  }
  ```

#### [GET] `/api/v1/templates/{id}/milestones` — Danh Sách Mốc & Câu Hỏi (UC-008.2)
#### [PUT] `/api/v1/templates/{id}/milestones/{id}` — Cập Nhật Mốc Khảo Sát (UC-008.3)
#### [DELETE] `/api/v1/templates/{id}/milestones/{id}` — Xóa Mốc Khảo Sát (UC-008.4)

#### [POST] `/api/v1/templates/{id}/red-flags` — Thêm Tiêu Chí Báo Động Đỏ (UC-008.5)
* **Quyền hạn:** `DOCTOR`
* **Request Body:**
  ```json
  {
    "symptom_name": "Đột ngột suy giảm hoặc mất thị lực",
    "first_aid_guide": "Tuyệt đối không dụi mắt, giữ nguyên tư thế và gọi ngay Hotline viện",
    "emergency_hotline": "0395 151 151",
    "sla_minutes": 5
  }
  ```

#### [GET] `/api/v1/templates/{id}/red-flags` — Danh Sách Tiêu Chí Red Flag (UC-008.6)
#### [PUT] `/api/v1/templates/{id}/red-flags/{id}` — Sửa Tiêu Chí Red Flag (UC-008.7)
#### [DELETE] `/api/v1/templates/{id}/red-flags/{id}` — Xóa Tiêu Chí Red Flag (UC-008.8)

#### [POST] `/api/v1/templates/{id}/modules` — Thêm Cẩm Nang & Infographic (UC-009.1)
* **Request Body:**
  ```json
  {
    "module_title": "Hướng dẫn 24 giờ đầu sống còn sau phẫu thuật Phaco",
    "is_critical_24h": true,
    "infographic_url": "https://cdn.visi.vn/academy/phaco-24h.png",
    "summary_bullets": [
      "Đeo kính bảo hộ liên tục kể cả khi ngủ",
      "Không để nước tiếp xúc trực tiếp với mắt",
      "Nghỉ ngơi tuyệt đối, không cúi gập đầu"
    ],
    "display_order": 1
  }
  ```

#### [GET] `/api/v1/templates/{id}/modules` — Danh Sách Bài Học Cẩm Nang (UC-009.2)
#### [PUT] `/api/v1/templates/{id}/modules/{id}` — Cập Nhật Bài Học (UC-009.3)
#### [DELETE] `/api/v1/templates/{id}/modules/{id}` — Xóa Bài Học (UC-009.4)

#### [POST] `/api/v1/templates/{id}/do-dont` — Thêm Quy Tắc Do/Don't 2 Cột Màu (UC-009.5)
* **Request Body:**
  ```json
  {
    "item_type": "DO",
    "category": "HYGIENE",
    "title": "Vệ sinh mi mắt bằng gạc vô trùng nhúng nước muối sinh lý",
    "medical_rationale": "Ngăn ngừa vi khuẩn xâm nhập vào vết rạch giác mạc"
  }
  ```

#### [GET] `/api/v1/templates/{id}/do-dont` — Danh Sách Quy Tắc Do/Don't (UC-009.6)
#### [PUT] `/api/v1/templates/{id}/do-dont/{id}` — Cập Nhật Quy Tắc (UC-009.7)
#### [DELETE] `/api/v1/templates/{id}/do-dont/{id}` — Xóa Quy Tắc (UC-009.8)

#### [GET] `/api/v1/templates/{id}/faqs` — Danh Sách FAQ Lâm Sàng (UC-009.9)
#### [POST] `/api/v1/templates/{id}/faqs` — Thêm Tình Huống FAQ Lâm Sàng (UC-009.9)

---

### 2.5 Phân Hệ Khởi Tạo Care Plan & Bàn Giao Xuất Viện (Care Plan & QR)

#### [POST] `/api/v1/patient-care-plans` — Khởi Tạo Care Plan Cá Nhân Hóa (UC-010)
* **Quyền hạn:** `DOCTOR` (có quyền sao chép template và tùy chỉnh liều lượng theo thể trạng bệnh nhân), `NURSE` (chỉ áp dụng mẫu phác đồ đã duyệt, không được sửa liều lượng).
* **Request Body:**
  ```json
  {
    "patient_id": "pat-7712-42da-912b-bc22e89",
    "template_id": "tpl-8812-uuid",
    "customized_medications": [
      {
        "template_med_id": "tmed-01",
        "custom_dose_amount": "2 giọt",
        "doctor_note": "Tăng liều do phản ứng viêm nhẹ"
      }
    ]
  }
  ```
* **Response (201 Created):** Nhân bản toàn bộ cấu hình vào `patient_care_plans`, `patient_medications`, `patient_followup_appointments` (<1s theo BR18).

#### [POST] `/api/v1/patient-care-plans/{id}/confirm` — Xác Nhận Hoàn Tất Bàn Giao (UC-010c)
* **Quyền hạn:** `DOCTOR`, `NURSE`

#### [POST] `/api/v1/qr/issue` — Tạo & Phát Hành Mã QR Xuất Viện (UC-011)
* **Quyền hạn:** `NURSE`
* **Request Body:** `{"care_plan_id": "pcp-8819-uuid"}`
* **Response (201 Created):**
  ```json
  {
    "success": true,
    "code": 201,
    "data": {
      "qr_token": "visi_qr_token_8a7f1c9e2b4d",
      "qr_image_base64": "data:image/png;base64,...",
      "expires_at": "2026-10-15T23:59:59Z"
    }
  }
  ```

#### [GET] `/api/v1/patient-care-plans/{id}/print-slip` — In Phiếu Xuất Viện Kèm QR (UC-012)
* **Quyền hạn:** `NURSE`
* **Response (200 OK):** Trả về file HTML/PDF sẵn sàng in 1 chạm khổ A5/A4 chứa mã QR kích thước ≥3x3 cm và Hotline VISI `0395 151 151`.

#### [POST] `/api/v1/qr/revoke-reissue` — Cấp Lại hoặc Thu Hồi Mã QR (UC-013)
* **Quyền hạn:** `NURSE`, `ADMIN`
* **Request Body:** `{"care_plan_id": "pcp-8819-uuid", "reason": "Thân nhân làm mất giấy xuất viện"}`

---

### 2.6 Phân Hệ Thao Tác Chăm Sóc Hàng Ngày (Care Operations)

#### [GET] `/api/v1/care-plans/{id}/medications` — Xem Lịch Thuốc & Hướng Dẫn Nhỏ (UC-014)
* **Quyền hạn:** `CAREGIVER`, `CARE_RECIPIENT`, `NURSE`, `DOCTOR`
* **Response (200 OK):** Danh sách 4 cữ thuốc Sáng/Trưa/Chiều/Tối kèm ảnh vỏ lọ và hướng dẫn kỹ thuật kéo mi.

#### [POST] `/api/v1/medication-logs` — Xác Nhận Dùng Thuốc & Buffer Timer (UC-015)
* **Quyền hạn:** `CAREGIVER`, `CARE_RECIPIENT`
* **Request Body:**
  ```json
  {
    "care_plan_id": "pcp-8819-uuid",
    "patient_med_id": "pmed-001",
    "schedule_time": "08:00",
    "confirmed_by_role": "CAREGIVER"
  }
  ```
* **Response (201 Created):**
  ```json
  {
    "success": true,
    "code": 201,
    "data": {
      "log_id": "log-0019-uuid",
      "confirmed_at": "2026-09-15T08:03:12Z",
      "buffer_countdown_seconds": 300,
      "next_allowed_drop_time": "2026-09-15T08:08:12Z"
    }
  }
  ```

#### [GET] `/api/v1/care-plans/{id}/guidelines` — Xem Cẩm Nang 24h & Do/Don't (UC-016)
#### [GET] `/api/v1/care-plans/{id}/academy` — Xem Infographic Lộ Trình Học Viện (UC-017)
#### [GET] `/api/v1/care-plans/{id}/appointments` — Xem Lịch Tái Khám 5 Mốc (UC-018)

#### [POST] `/api/v1/recovery-checks` — Nộp Khảo Sát Recovery Check (UC-019)
* **Quyền hạn:** `CAREGIVER`, `CARE_RECIPIENT`
* **Request Body:**
  ```json
  {
    "care_plan_id": "pcp-8819-uuid",
    "milestone_id": "ms-01",
    "answers": [
      {"question_id": "q-01", "selected_level": "YELLOW"},
      {"question_id": "q-02", "selected_level": "GREEN"}
    ]
  }
  ```
* **Response (201 Created):** Phân loại tự động: Xanh (Bình thường), Vàng (CSKH gọi tư vấn), Đỏ (Tự động kích hoạt Red Flag).

#### [POST] `/api/v1/recovery-checks/upload-image` — Tải Ảnh Chụp Mắt Hậu Phẫu (UC-019)
* **Quyền hạn:** `CAREGIVER`, `CARE_RECIPIENT`
* **Mô tả:** Tải lên hình ảnh chụp mắt thực tế (vết mổ, tình trạng cương tụ, xuất huyết hoặc tiết dịch) để đính kèm vào phiếu khảo sát triệu chứng hàng ngày hoặc gửi bác sĩ đánh giá từ xa.
* **Content-Type:** `multipart/form-data`
* **Request Payload:**
  - `file`: File ảnh (PNG, JPEG, HEIC, tối đa 10MB, tự động nén & tạo thumbnail WebP).
  - `care_plan_id`: UUID phác đồ chăm sóc.
  - `milestone_id`: UUID mốc khảo sát tương ứng.
* **Response (201 Created):**
  ```json
  {
    "success": true,
    "code": 201,
    "data": {
      "image_url": "https://storage.visi.vn/patient-recovery/pat-7712/eye_day1_1726482910.webp",
      "thumbnail_url": "https://storage.visi.vn/patient-recovery/pat-7712/eye_day1_1726482910_thumb.webp",
      "uploaded_at": "2026-09-15T08:20:00Z"
    }
  }
  ```

#### [POST] `/api/v1/alerts/red-flag` — Kích Hoạt Cấp Cứu Red Flag Khẩn Cấp (UC-020)
* **Quyền hạn:** `CAREGIVER`, `CARE_RECIPIENT`
* **Request Body:**
  ```json
  {
    "care_plan_id": "pcp-8819-uuid",
    "symptom_description": "Mắt đột ngột tối sầm, đau nhức dữ dội",
    "contact_phone": "0987654321"
  }
  ```
* **Response (201 Created):**
  ```json
  {
    "success": true,
    "code": 201,
    "data": {
      "incident_id": "inc-0091-uuid",
      "emergency_hotline": "0395 151 151",
      "sla_minutes": 5,
      "alert_status": "TRIGGERED"
    }
  }
  ```

#### [GET] `/api/v1/faqs/search` — Tra Cứu Tình Huống Khẩn Cấp FAQ (UC-024)
* **Query Params:** `query=dính nước`, `category=HYGIENE`

#### [PATCH] `/api/v1/users/a11y` — Cấu Hình Chế Độ Trợ Năng Nhãn Khoa (UC-025)
* **Request Body:**
  ```json
  {
    "font_scale": 1.4,
    "high_contrast_mode": true,
    "audio_guide_enabled": true
  }
  ```

---

### 2.7 Phân Hệ Giám Sát Lâm Sàng & Điều Phối Cảnh Báo (Clinical Monitoring)

#### [GET] `/api/v1/monitoring/dashboard` — Dashboard Cơ Sở Tập Trung (UC-021)
* **Quyền hạn:** `DOCTOR`, `CSKH`, `NURSE`, `HOSPITAL_DIRECTOR`
* **Query Params:** `facility_id`, `status_tier` (RED, YELLOW, GREEN), `page`, `limit`
* **Response (200 OK):** Thống kê số lượng ca theo 3 tầng màu; danh sách bệnh nhân cần gọi điện can thiệp.

#### [PUT] `/api/v1/alerts/{id}/triage` — Tiếp Nhận & Xử Lý Cảnh Báo Red Flag (UC-022)
* **Quyền hạn:** `CSKH`, `DOCTOR`
* **Request Body:** `{"action": "CLAIM_CASE"}` -> Dừng chuông báo động, đổi trạng thái sang `IN_PROGRESS`.

#### [POST] `/api/v1/alerts/{id}/escalate` — Leo Thang Cảnh Báo Quá Hạn (UC-022b)
* **Quyền hạn:** Daemon Hệ Thống (`SYSTEM_DAEMON`)
* **Mô tả:** Tự động kích hoạt sau 15 phút chưa xử lý (BR12). Bắn SMS khẩn cấp tới Bác sĩ trực cơ sở.

#### [POST] `/api/v1/incidents/{incident_id}/call-logs` — Ghi Nhận Nhật Ký Cuộc Gọi Can Thiệp (UC-023)
* **Quyền hạn:** `CSKH`, `NURSE`, `DOCTOR`
* **Mô tả:** CSKH hoặc Điều dưỡng ghi nhận kết quả liên lạc với bệnh nhân/người chăm sóc sau khi tiếp nhận cảnh báo Red Flag/Yellow Flag (BR10, BR11, SLA <5 phút).
* **Request Body:**
  ```json
  {
    "call_status": "ANSWERED",
    "call_duration_seconds": 180,
    "contact_phone": "0987654321",
    "patient_condition": "Đau nhẹ do dị vật bay vào mắt, đã rửa nước mắt nhân tạo, hiện tại thị lực ổn định",
    "clinical_advice": "Tiếp tục tra thuốc kháng sinh theo lịch, nếu đau buốt lan nửa đầu quay lại viện ngay",
    "resolution_action": "RESOLVED_REMOTE"
  }
  ```
* **Response (201 Created):**
  ```json
  {
    "success": true,
    "code": 201,
    "data": {
      "log_id": "call-log-0012-uuid",
      "incident_id": "inc-0091-uuid",
      "caller_id": "acc-nurse-uuid",
      "call_time": "2026-09-15T08:18:25Z",
      "sla_compliance": true,
      "incident_new_status": "RESOLVED"
    }
  }
  ```

#### [GET] `/api/v1/incidents/{incident_id}/call-logs` — Xem Lịch Sử Các Cuộc Gọi Can Thiệp (UC-023)
* **Quyền hạn:** `CSKH`, `NURSE`, `DOCTOR`, `ADMIN`
* **Mô tả:** Truy xuất toàn bộ lịch sử các cuộc gọi xử lý sự cố (bao gồm cả các cuộc gọi nhỡ `NO_ANSWER`, bận máy `BUSY` hoặc thành công `ANSWERED`) để phục vụ kiểm toán lâm sàng và đánh giá chất lượng phản hồi cấp cứu.
* **Response (200 OK):**
  ```json
  {
    "success": true,
    "code": 200,
    "data": [
      {
        "log_id": "call-log-0011-uuid",
        "incident_id": "inc-0091-uuid",
        "caller_name": "ĐD. Nguyễn Hoàng Nam",
        "caller_role": "NURSE",
        "call_time": "2026-09-15T08:16:10Z",
        "call_status": "NO_ANSWER",
        "call_duration_seconds": 30,
        "patient_condition": null,
        "clinical_advice": "Bệnh nhân không nhấc máy lần 1, tiến hành gọi lại lần 2 sau 1 phút",
        "resolution_action": "CONTINUE_MONITORING"
      },
      {
        "log_id": "call-log-0012-uuid",
        "incident_id": "inc-0091-uuid",
        "caller_name": "ĐD. Nguyễn Hoàng Nam",
        "caller_role": "NURSE",
        "call_time": "2026-09-15T08:18:25Z",
        "call_status": "ANSWERED",
        "call_duration_seconds": 180,
        "patient_condition": "Đau nhẹ do dị vật bay vào mắt, đã rửa nước mắt nhân tạo, hiện tại thị lực ổn định",
        "clinical_advice": "Tiếp tục tra thuốc kháng sinh theo lịch, nếu đau buốt lan nửa đầu quay lại viện ngay",
        "resolution_action": "RESOLVED_REMOTE"
      }
    ]
  }
  ```

---

### 2.8 Phân Hệ Quản Trị Hệ Thống & Báo Cáo KPI (Admin & Analytics)

#### [GET] `/api/v1/audit-logs` — Tra Cứu Nhật Ký Kiểm Toán (UC-027)
* **Quyền hạn:** `ADMIN`
* **Query Params:** `user_id`, `action_type`, `from_date`, `to_date`, `page`, `limit`

#### [GET] `/api/v1/reports/kpi` — Báo Cáo Vận Hành & Tỷ Lệ Tuân Thủ (UC-028)
* **Quyền hạn:** `HOSPITAL_DIRECTOR`, `ADMIN`
* **Query Params:** `facility_id`, `quarter`, `year`
* **Response (200 OK):** Tỷ lệ kích hoạt QR (KPI ≥85%), tỷ lệ tuân thủ thuốc đúng giờ, tỷ lệ tái khám 5 mốc theo từng cơ sở.

---
