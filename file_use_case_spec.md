# II. ĐẶC TẢ YÊU CẦU NGHIỆP VỤ (REQUIREMENT SPECIFICATIONS)
# DANH MỤC VÀ ĐẶC TẢ CHI TIẾT CÁC USE CASE (FILE_USE_CASE_SPEC.MD)

> **Dự án:** RemiCare Ophthalmic Post-Op Platform (Nền tảng Hướng dẫn và Giám sát Chăm sóc Hậu phẫu Nhãn khoa)  
> **Doanh nghiệp mục tiêu:** Công ty Cổ phần Tập đoàn Y khoa VISI (VISI Medical Group)  
> **Phiên bản:** V1 (Phân rã chi tiết toàn bộ các Use Case Quản lý thành các CRUD Sub-Use Cases theo chuẩn US_US_V1)  
> **Ngày phê duyệt:** 15/09/2026  
> **Mục tiêu kỹ thuật:** Đặc tả chi tiết từng thao tác CRUD (Create, Read/List, Read/Detail, Update, Delete/Archive) giúp Đội ngũ Lập trình (Dev) và Kiểm thử (QA/QC) nắm bắt chính xác logic API, form giao diện, phân quyền vai trò và luồng xử lý ngoại lệ.  

---

## 1. TỔNG QUAN DANH MỤC USE CASE HỆ THỐNG (BAO GỒM PHÂN RÃ CRUD)

Nhằm giúp đội ngũ phát triển phần mềm (Dev) và kiểm thử (QA/QC) dễ dàng thiết kế API endpoints, cơ sở dữ liệu và màn hình giao diện mà không bị nhầm lẫn giữa các thao tác nghiệp vụ, toàn bộ các Use Case dạng "Quản lý / Cấu hình" đã được **phân rã chi tiết thành các ca sử dụng CRUD đơn nguyên**:

### 1.1 Danh mục Các Nhóm Use Case Phân Rã CRUD Chi Tiết

1. **Nhóm UC-004: Quản lý Hồ sơ Định danh Bệnh Nhân (Patient Profiles CRUD):**
   * `UC-004.1`: Tạo mới hồ sơ bệnh nhân (`POST /api/v1/patients`)
   * `UC-004.2`: Tra cứu và lọc danh sách hồ sơ bệnh nhân theo cơ sở (`GET /api/v1/patients`)
   * `UC-004.3`: Xem chi tiết hồ sơ bệnh nhân & Dòng thời gian phục hồi (`GET /api/v1/patients/{id}`)
   * `UC-004.4`: Cập nhật chỉnh sửa thông tin hồ sơ bệnh nhân (`PUT /api/v1/patients/{id}`)
   * `UC-004.5`: Lưu trữ / Vô hiệu hóa hồ sơ bệnh nhân (`DELETE /api/v1/patients/{id}` — Soft Delete)

2. **Nhóm UC-005: Quản lý Danh mục Mẫu Kế Hoạch Chăm Sóc (Care Plan Master Templates CRUD):**
   * `UC-005.1`: Tạo mới Master Template (`POST /api/v1/templates`)
   * `UC-005.2`: Xem danh sách & lọc Master Template theo loại phẫu thuật (`GET /api/v1/templates`)
   * `UC-005.3`: Xem chi tiết cấu hình Master Template (`GET /api/v1/templates/{id}`)
   * `UC-005.4`: Chỉnh sửa thông tin chung Master Template (`PUT /api/v1/templates/{id}`)
   * `UC-005.5`: Kích hoạt / Lưu trữ Master Template (`PATCH /api/v1/templates/{id}/status`)

3. **Nhóm UC-007: Cấu Hình Danh Mục Thuốc Mẫu Trong Template (Template Medications CRUD):**
   * `UC-007.1`: Thêm thuốc mẫu vào Master Template & Cài đặt Timer đệm (`POST /api/v1/templates/{id}/medications`)
   * `UC-007.2`: Xem danh sách thuốc mẫu trong Template (`GET /api/v1/templates/{id}/medications`)
   * `UC-007.3`: Chỉnh sửa thuốc mẫu & thời gian đệm giãn cách (`PUT /api/v1/templates/{id}/medications/{med_id}`)
   * `UC-007.4`: Xóa thuốc mẫu khỏi Master Template (`DELETE /api/v1/templates/{id}/medications/{med_id}`)

4. **Nhóm UC-008: Cấu Hình Bộ Câu Hỏi Recovery Check & Red Flag (Recovery & Red Flags CRUD):**
   * `UC-008.1`: Thêm mốc thời gian & câu hỏi Recovery Check (`POST /api/v1/templates/{id}/milestones`)
   * `UC-008.2`: Xem danh sách mốc & câu hỏi Recovery Check (`GET /api/v1/templates/{id}/milestones`)
   * `UC-008.3`: Chỉnh sửa mốc & câu hỏi Recovery Check (`PUT /api/v1/templates/{id}/milestones/{id}`)
   * `UC-008.4`: Xóa mốc thời gian / câu hỏi Recovery Check (`DELETE /api/v1/templates/{id}/milestones/{id}`)
   * `UC-008.5`: Thêm tiêu chí dấu hiệu nguy hiểm Red Flag & Hotline (`POST /api/v1/templates/{id}/red-flags`)
   * `UC-008.6`: Xem danh sách tiêu chí Red Flag trong Template (`GET /api/v1/templates/{id}/red-flags`)
   * `UC-008.7`: Chỉnh sửa tiêu chí Red Flag & Hướng xử trí lâm sàng (`PUT /api/v1/templates/{id}/red-flags/{id}`)
   * `UC-008.8`: Xóa tiêu chí Red Flag khỏi Template (`DELETE /api/v1/templates/{id}/red-flags/{id}`)

5. **Nhóm UC-009: Cấu Hình Cẩm Nang, Quy Tắc Sinh Hoạt & FAQ Lâm Sàng (Guidelines & FAQ CRUD):**
   * `UC-009.1`: Thêm nội dung Cẩm nang 24h & Infographic bài học (`POST /api/v1/templates/{id}/modules`)
   * `UC-009.2`: Xem danh mục Cẩm nang & Infographic bài học (`GET /api/v1/templates/{id}/modules`)
   * `UC-009.3`: Chỉnh sửa nội dung Cẩm nang & Infographic bài học (`PUT /api/v1/templates/{id}/modules/{id}`)
   * `UC-009.4`: Xóa bài học khỏi lộ trình Cẩm nang (`DELETE /api/v1/templates/{id}/modules/{id}`)
   * `UC-009.5`: Thêm quy tắc Nên làm / Cần tránh 2 cột màu (`POST /api/v1/templates/{id}/do-dont`)
   * `UC-009.6`: Xem danh sách quy tắc Nên làm / Cần tránh (`GET /api/v1/templates/{id}/do-dont`)
   * `UC-009.7`: Chỉnh sửa quy tắc Nên làm / Cần tránh (`PUT /api/v1/templates/{id}/do-dont/{id}`)
   * `UC-009.8`: Xóa quy tắc Nên làm / Cần tránh khỏi Template (`DELETE /api/v1/templates/{id}/do-dont/{id}`)
   * `UC-009.9`: Quản lý ngân hàng tình huống hỏi đáp FAQ lâm sàng (`POST/GET/PUT/DELETE /api/v1/templates/{id}/faqs`)

6. **Nhóm UC-026: Quản Lý Tài Khoản Nhân Viên và Phân Quyền Cơ Sở (Staff Accounts & Multi-Branch RBAC CRUD):**
   * `UC-026.1`: Khởi tạo tài khoản nhân viên y tế & gán chi nhánh (`POST /api/v1/staff`)
   * `UC-026.2`: Xem danh sách và tìm kiếm nhân viên theo cơ sở (`GET /api/v1/staff`)
   * `UC-026.3`: Chỉnh sửa thông tin định danh & vai trò RBAC nhân viên (`PUT /api/v1/staff/{id}`)
   * `UC-026.4`: Khóa / Vô hiệu hóa tài khoản nhân viên (`DELETE /api/v1/staff/{id}` — Deactivate/Lock)

---

### 1.2 Bảng Ma Trận Tổng Hợp Toàn Bộ Ca Sử Dụng (Traceability Index)

| Mã Use Case | Tên Use Case / Thao Tác | Nhóm Nghiệp Vụ / Bản Chất | Tác Nhân Chính | API Endpoint & Method |
| :--- | :--- | :--- | :--- | :--- |
| **UC-001** | Đăng ký & Đăng nhập Caregiver OTP | Xác thực Người dùng (Auth) | Caregiver (ACT-008) | `POST /api/v1/auth/otp/request` & `verify` |
| **UC-002** | Đăng nhập Nhân viên Y tế & 2FA | Xác thực Nhân sự (Auth) | Bác sĩ, Điều dưỡng, CSKH | `POST /api/v1/auth/staff/login` & `2fa` |
| **UC-003** | Quét QR Bàn Giao Liên Kết Bệnh Nhân | Kích hoạt Hồ sơ (Linking) | Caregiver (ACT-008) | `POST /api/v1/patient-links/claim` |
| **UC-004.1**| Tạo Mới Hồ Sơ Bệnh Nhân | **Patient CRUD - Create** | Điều dưỡng (ACT-003) | `POST /api/v1/patients` |
| **UC-004.2**| Tra Cứu & Lọc Danh Sách Bệnh Nhân | **Patient CRUD - Read (List)** | Bác sĩ, Điều dưỡng, CSKH | `GET /api/v1/patients` |
| **UC-004.3**| Xem Chi Tiết Hồ Sơ & Timeline | **Patient CRUD - Read (Detail)**| Bác sĩ, Điều dưỡng, CSKH | `GET /api/v1/patients/{id}` |
| **UC-004.4**| Cập Nhật Thông Tin Bệnh Nhân | **Patient CRUD - Update** | Điều dưỡng (ACT-003) | `PUT /api/v1/patients/{id}` |
| **UC-004.5**| Lưu Trữ / Vô Hiệu Hóa Hồ Sơ | **Patient CRUD - Delete/Archive**| Điều dưỡng trưởng, BS Trưởng | `DELETE /api/v1/patients/{id}` |
| **UC-005.1**| Tạo Mới Master Template | **Template CRUD - Create** | Bác sĩ chuyên khoa, GCMO | `POST /api/v1/templates` |
| **UC-005.2**| Xem Danh Sách & Lọc Master Template | **Template CRUD - Read (List)** | Bác sĩ, Điều dưỡng, GCMO | `GET /api/v1/templates` |
| **UC-005.3**| Xem Chi Tiết Cấu Hình Template | **Template CRUD - Read (Detail)**| Bác sĩ, Điều dưỡng, GCMO | `GET /api/v1/templates/{id}` |
| **UC-005.4**| Chỉnh Sửa Thông Tin Chung Template | **Template CRUD - Update** | Bác sĩ chuyên khoa, GCMO | `PUT /api/v1/templates/{id}` |
| **UC-005.5**| Kích Hoạt / Lưu Trữ Master Template| **Template CRUD - Status** | GCMO (ACT-001) | `PATCH /api/v1/templates/{id}/status` |
| **UC-006** | Phê Duyệt Lâm Sàng Master Template | Phê duyệt Chuyên môn (Approval) | GCMO (ACT-001) | `POST /api/v1/templates/{id}/approve` |
| **UC-007.1**| Thêm Thuốc Mẫu & Buffer Timer | **Medication CRUD - Create** | Bác sĩ chuyên khoa (ACT-002) | `POST /api/v1/templates/{id}/medications` |
| **UC-007.2**| Xem Danh Sách Thuốc Mẫu | **Medication CRUD - Read (List)**| Bác sĩ, Điều dưỡng | `GET /api/v1/templates/{id}/medications` |
| **UC-007.3**| Chỉnh Sửa Thuốc Mẫu & Buffer | **Medication CRUD - Update** | Bác sĩ chuyên khoa (ACT-002) | `PUT /api/v1/templates/{id}/medications/{id}` |
| **UC-007.4**| Xóa Thuốc Mẫu Khỏi Template | **Medication CRUD - Delete** | Bác sĩ chuyên khoa (ACT-002) | `DELETE /api/v1/templates/{id}/medications/{id}` |
| **UC-008.1**| Thêm Mốc & Câu Hỏi Recovery Check | **Milestone CRUD - Create** | Bác sĩ chuyên khoa (ACT-002) | `POST /api/v1/templates/{id}/milestones` |
| **UC-008.2**| Xem Danh Sách Mốc & Câu Hỏi Check | **Milestone CRUD - Read** | Bác sĩ, Điều dưỡng | `GET /api/v1/templates/{id}/milestones` |
| **UC-008.3**| Chỉnh Sửa Mốc & Câu Hỏi Check | **Milestone CRUD - Update** | Bác sĩ chuyên khoa (ACT-002) | `PUT /api/v1/templates/{id}/milestones/{id}` |
| **UC-008.4**| Xóa Mốc Thời Gian / Câu Hỏi Check | **Milestone CRUD - Delete** | Bác sĩ chuyên khoa (ACT-002) | `DELETE /api/v1/templates/{id}/milestones/{id}` |
| **UC-008.5**| Thêm Tiêu Chí Red Flag & Hotline | **Red Flag CRUD - Create** | Bác sĩ chuyên khoa (ACT-002) | `POST /api/v1/templates/{id}/red-flags` |
| **UC-008.6**| Xem Danh Sách Tiêu Chí Red Flag | **Red Flag CRUD - Read** | Bác sĩ, CSKH | `GET /api/v1/templates/{id}/red-flags` |
| **UC-008.7**| Chỉnh Sửa Tiêu Chí Red Flag & Xử Trí| **Red Flag CRUD - Update** | Bác sĩ chuyên khoa (ACT-002) | `PUT /api/v1/templates/{id}/red-flags/{id}` |
| **UC-008.8**| Xóa Tiêu Chí Red Flag Khỏi Template| **Red Flag CRUD - Delete** | Bác sĩ chuyên khoa (ACT-002) | `DELETE /api/v1/templates/{id}/red-flags/{id}` |
| **UC-009.1**| Thêm Cẩm Nang 24h & Infographic | **Module CRUD - Create** | Bác sĩ chuyên khoa (ACT-002) | `POST /api/v1/templates/{id}/modules` |
| **UC-009.2**| Xem Danh Mục Cẩm Nang & Infographic| **Module CRUD - Read** | Bác sĩ, Điều dưỡng | `GET /api/v1/templates/{id}/modules` |
| **UC-009.3**| Chỉnh Sửa Cẩm Nang & Infographic | **Module CRUD - Update** | Bác sĩ chuyên khoa (ACT-002) | `PUT /api/v1/templates/{id}/modules/{id}` |
| **UC-009.4**| Xóa Bài Học Khỏi Lộ Trình Cẩm Nang| **Module CRUD - Delete** | Bác sĩ chuyên khoa (ACT-002) | `DELETE /api/v1/templates/{id}/modules/{id}` |
| **UC-009.5**| Thêm Quy Tắc Nên Làm / Cần Tránh | **Do/Don't CRUD - Create** | Bác sĩ chuyên khoa (ACT-002) | `POST /api/v1/templates/{id}/do-dont` |
| **UC-009.6**| Xem Danh Sách Quy Tắc Do/Don't | **Do/Don't CRUD - Read** | Bác sĩ, Điều dưỡng | `GET /api/v1/templates/{id}/do-dont` |
| **UC-009.7**| Chỉnh Sửa Quy Tắc Do/Don't | **Do/Don't CRUD - Update** | Bác sĩ chuyên khoa (ACT-002) | `PUT /api/v1/templates/{id}/do-dont/{id}` |
| **UC-009.8**| Xóa Quy Tắc Do/Don't Khỏi Template| **Do/Don't CRUD - Delete** | Bác sĩ chuyên khoa (ACT-002) | `DELETE /api/v1/templates/{id}/do-dont/{id}` |
| **UC-009.9**| Quản Lý Ngân Hàng FAQ Lâm Sàng | **FAQ CRUD - All** | Bác sĩ, CSKH | `POST/GET/PUT/DELETE /api/v1/templates/{id}/faqs` |
| **UC-010** | Khởi Tạo & Cá Nhân Hóa Care Plan | Khởi tạo Lâm sàng (Instantiate) | Bác sĩ điều trị (ACT-002) | `POST /api/v1/patient-care-plans` |
| **UC-010b**| Bàn Giao Xuất Viện Phòng Lưu Viện | Bàn giao Hậu phẫu (Handoff) | Điều dưỡng lưu viện (ACT-003)| Giao diện kiểm tra đối soát |
| **UC-010c**| Xác Nhận Hoàn Tất Bàn Giao | Xác nhận Lâm sàng (Confirm) | Bác sĩ điều trị (ACT-002) | `POST /api/v1/patient-care-plans/{id}/confirm`|
| **UC-011** | Tạo & Phát Hành Mã QR Xuất Viện | Quản lý Vòng đời QR | Điều dưỡng lưu viện (ACT-003)| `POST /api/v1/qr/issue` |
| **UC-012** | In Phiếu Xuất Viện Kèm Mã QR | Tác vụ Vật lý (Print Output) | Điều dưỡng lưu viện (ACT-003)| `GET /api/v1/patient-care-plans/{id}/print-slip`|
| **UC-013** | Cấp Lại hoặc Thu Hồi Mã QR | Ngoại lệ Vòng đời QR | Điều dưỡng lưu viện (ACT-003)| `POST /api/v1/qr/revoke-reissue` |
| **UC-014** | Xem Lịch Thuốc & Hướng Dẫn Nhỏ Mắt | Hướng dẫn Tuân thủ (Compliance)| Caregiver, Care Recipient | `GET /api/v1/care-plans/{id}/medications` |
| **UC-015** | Xác Nhận Thuốc & Buffer Timer 5-10p | Giám sát Dùng thuốc (Buffer) | Caregiver, Care Recipient | `POST /api/v1/medication-logs` |
| **UC-016** | Xem Cẩm Nang 24h & Bảng Do/Don't | Tra Cứu Hướng Dẫn (Education) | Caregiver, Care Recipient | `GET /api/v1/care-plans/{id}/guidelines` |
| **UC-017** | Xem Infographic Học Viện Caregiver | Giáo dục Sức khỏe (Academy) | Caregiver, Care Recipient | `GET /api/v1/care-plans/{id}/academy` |
| **UC-018** | Xem Lịch Tái Khám 5 Mốc & Nhắc Hẹn | Theo dõi Tái khám (Follow-up) | Caregiver, Care Recipient | `GET /api/v1/care-plans/{id}/appointments` |
| **UC-019** | Thực Hiện Khảo Sát Recovery Check | Khảo sát Định kỳ (Survey) | Caregiver, Care Recipient | `POST /api/v1/recovery-checks` |
| **UC-020** | Kích Hoạt Cấp Cứu Red Flag | Xử lý Biến chứng (Emergency) | Caregiver, Care Recipient | `POST /api/v1/alerts/red-flag` |
| **UC-021** | Giám Sát Dashboard Phục Hồi Tập Trung | Dashboard Giám sát (Monitoring)| Bác sĩ, CSKH, Điều dưỡng | `GET /api/v1/monitoring/dashboard` |
| **UC-022** | Tiếp Nhận & Phân Loại Cảnh Báo | Điều phối Lâm sàng (Triaging) | CSKH, Bác sĩ trực | `PUT /api/v1/alerts/{id}/triage` |
| **UC-022b**| Tự Động Leo Thang Cảnh Báo Quá Hạn| Xử lý Tự động Hệ thống (Daemon)| VISI Core Alert Daemon | Tự động kích hoạt sau 15p |
| **UC-023** | Ghi Nhật Ký Cuộc Gọi Can Thiệp | Can thiệp Lâm sàng (Call Log) | CSKH, Bác sĩ | `POST /api/v1/call-logs` |
| **UC-024** | Tra Cứu Tình Huống Khẩn Cấp - FAQ | Hỗ trợ Tức thì (Support) | Caregiver, Care Recipient | `GET /api/v1/faqs/search` |
| **UC-025** | Kích Hoạt Trợ Năng Nhãn Khoa | Trải nghiệm Người dùng (A11y) | Caregiver, Care Recipient | Lưu trữ cục bộ + `PATCH /api/v1/users/a11y` |
| **UC-026.1**| Khởi Tạo Tài Khoản Nhân Viên & Gán Chi Nhánh | **Staff CRUD - Create** | System Admin (ACT-006) | `POST /api/v1/staff` |
| **UC-026.2**| Tra Cứu & Lọc Danh Sách Nhân Viên| **Staff CRUD - Read (List)** | System Admin, Giám đốc | `GET /api/v1/staff` |
| **UC-026.3**| Chỉnh Sửa Thông Tin & Vai Trò RBAC| **Staff CRUD - Update** | System Admin (ACT-006) | `PUT /api/v1/staff/{id}` |
| **UC-026.4**| Khóa / Vô Hiệu Hóa Tài Khoản | **Staff CRUD - Delete/Lock** | System Admin (ACT-006) | `DELETE /api/v1/staff/{id}` |
| **UC-027** | Tra Cứu Nhật Ký Kiểm Toán Audit Log | An ninh & Tuân thủ (Compliance)| Quản trị viên, Thanh tra y tế| `GET /api/v1/audit-logs` |
| **UC-028** | Kết Xuất Báo Cáo Vận Hành & KPI | Phân tích Quản trị (Analytics) | Giám đốc Bệnh viện | `GET /api/v1/reports/kpi` |

---

### 1.3 MA TRẬN PHÂN ĐỊNH TRÁCH NHIỆM & RANH GIỚI QUYỀN HẠN CỦA 7 VAI TRÒ (ROLE & RESPONSIBILITY MATRIX)

Để bảo đảm tính an toàn dữ liệu y tế, chuẩn hóa phân quyền truy cập (Role-Based Access Control - RBAC) và tránh chồng chéo nhiệm vụ giữa các bộ phận, hệ thống RemiCare xác lập ma trận chức năng chi tiết cho 7 đối tượng người dùng:

#### 1. Giám đốc Bệnh viện (Hospital Director / GCMO)
* **Xem báo cáo vận hành:** Theo dõi toàn diện các chỉ số KPI vận hành hậu phẫu, tỷ lệ kích hoạt QR, tỷ lệ tuân thủ thuốc và tái khám của cơ sở.
* **Xử lý các trường hợp cần đánh giá chuyên môn cấp cao (Escalated Review):** Tiếp nhận và chỉ đạo xử lý các ca biến chứng nhãn khoa phức tạp vượt quá thẩm quyền của Bác sĩ điều trị.
* **Theo dõi các cảnh báo y tế nghiêm trọng:** Giám sát trực tiếp luồng cảnh báo Red Flag chưa được giải quyết hoặc đã bị leo thang quá hạn SLA.
* **Xem hồ sơ bệnh nhân theo phạm vi được phân quyền:** *Lưu ý quan trọng:* Không mặc định cho Giám đốc xem toàn bộ mọi hồ sơ nếu chưa có quy định phân quyền rõ ràng; chỉ xem hồ sơ bệnh nhân theo đúng phạm vi thẩm quyền được cấp (theo cơ sở hoặc ca bệnh cần hội chẩn).
* **Quản lý và giám sát Bác sĩ, Điều dưỡng:** Đánh giá hiệu suất và tính tuân thủ quy trình lâm sàng của đội ngũ nhân viên y tế tại bệnh viện.
* **Phê duyệt các Care Plan quan trọng:** Thẩm định và ký duyệt điện tử ban hành các gói Master Care Plan Template hoặc các phác đồ điều trị đặc biệt.
* **Theo dõi chất lượng và hiệu quả chăm sóc:** Đo lường tỷ lệ hồi phục thị lực và mức độ hài lòng của người bệnh sau xuất viện.
* **Xem lịch sử hoạt động chuyên môn và báo cáo tổng hợp:** Truy vấn nhật ký can thiệp lâm sàng và kết xuất báo cáo thống kê phục vụ quản trị bệnh viện.

#### 2. Bác sĩ (Ophthalmic Surgeon / Doctor)
* **CRUD Medical Record:** Tạo lập, tra cứu, cập nhật và lưu trữ hồ sơ bệnh án nhãn khoa chuyên môn.
* **Tạo và cập nhật Care Plan:** Có quyền **sao chép (clone) Master Template** để tạo và cá nhân hóa Kế hoạch chăm sóc (Care Plan) cho từng bệnh nhân cụ thể tùy theo thể trạng, cơ địa và đáp ứng lâm sàng thực tế của họ.
* **Cấu hình & Tùy biến Medication:** Cấu hình và điều chỉnh liều lượng thuốc, số giọt, thời gian dùng thuốc trong Care Plan phù hợp với tình trạng phục hồi của từng ca phẫu thuật.
* **Cấu hình Medication trong Care Plan:** Chỉ định danh mục thuốc, liều lượng, số giọt, lịch cữ uống/nhỏ và thời gian đệm giãn cách an toàn.
* **Tạo và quản lý Learning Path:** Thiết lập các bài học, infographic hướng dẫn phục hồi thị lực phù hợp với từng loại phẫu thuật.
* **Thiết lập Recovery Check:** Cài đặt các mốc khảo sát định kỳ và bộ câu hỏi đánh giá triệu chứng mắt mỗi ngày.
* **Thiết lập Red Flags:** Xác định danh mục các triệu chứng báo động đỏ nguy hiểm và hướng dẫn sơ cứu khẩn cấp.
* **Thiết lập hướng dẫn Do & Don’t:** Soạn thảo bảng 2 cột Nên làm / Cần tránh phân nhóm hoạt động sinh hoạt.
* **Thiết lập lịch Follow-up / Tái khám:** Lập lịch hẹn 5 mốc tái khám chuẩn VISI (Ngày 1, Ngày 3, Ngày 7, Ngày 14, Ngày 30).
* **Xuất bản hoặc gửi Care Plan để phê duyệt:** Trình duyệt các mẫu phác đồ chuẩn lên Hội đồng Chuyên môn / Giám đốc Bệnh viện.
* **Theo dõi tiến trình chăm sóc của bệnh nhân:** Giám sát mức độ tuân thủ nhỏ thuốc và sự phục hồi của từng ca phẫu thuật.
* **Xem kết quả Recovery Check và cảnh báo từ Điều dưỡng/Caregiver:** Nhận định sớm các nguy cơ biến chứng qua các câu trả lời khảo sát.
* **Xử lý các trường hợp được Điều dưỡng hoặc hệ thống chuyển cấp:** Trực tiếp liên lạc, thăm khám lại hoặc chỉ định can thiệp cấp cứu cho bệnh nhân có cờ Vàng/Đỏ.

#### 3. Điều dưỡng (Discharge / Clinical Nurse)
* **Xem Medical Record theo quyền được cấp:** Tra cứu hồ sơ hành chính và thông tin phẫu thuật của bệnh nhân tại cơ sở làm việc.
* **Xem Care Plan:** Nắm rõ phác đồ chăm sóc, lịch dùng thuốc và lời dặn của Bác sĩ điều trị.
* **Nhập thông tin bệnh nhân vào hệ thống:** **Chỉ được nhập thông tin cơ bản / hành chính của bệnh nhân** (Họ tên, năm sinh, giới tính, SĐT, mắt mổ, ngày mổ trong <30 giây).
* **Tạo Care Plan ban đầu theo hướng dẫn hoặc mẫu được Bác sĩ phê duyệt:** Kích hoạt gói kế hoạch chăm sóc từ Master Template có sẵn theo đúng hướng dẫn của Bác sĩ; **tuyệt đối không được chỉnh sửa liều lượng thuốc** hay thay đổi danh mục thuốc.
* **Xuất QR để Caregiver liên kết với bệnh nhân:** Sinh mã QR bàn giao an toàn và in trực tiếp lên Phiếu Hướng Dẫn Xuất Viện.
* **Theo dõi việc thực hiện nhiệm vụ chăm sóc:** Kiểm tra xem người bệnh/người chăm sóc đã kích hoạt QR và xác nhận dùng thuốc hay chưa.
* **Cập nhật tình trạng bệnh nhân:** Ghi chú các thông tin phát sinh trong thời gian lưu viện trước khi ra về.
* **Ghi nhận Recovery Check:** Hỗ trợ nhập liệu kết quả khảo sát đối với các bệnh nhân cao tuổi gọi điện thoại nhờ trợ giúp.
* **Theo dõi các dấu hiệu bất thường:** Phát hiện sớm các than phiền về đau nhức, đỏ mắt hoặc giảm thị lực.
* **Gửi cảnh báo đến Bác sĩ:** Chuyển tiếp ngay lập tức thông tin các ca có dấu hiệu bất thường cho Bác sĩ điều trị chính.
* **Cấp lại hoặc vô hiệu hóa mã QR khi QR bị mất hoặc có nguy cơ bị lộ:** Thao tác đổi mã QR mới an toàn cho thân nhân tại quầy lưu viện.
* **Hỗ trợ hướng dẫn Caregiver:** Hướng dẫn trực tiếp thân nhân cài đặt Web App, quét mã QR và bật tính năng trợ năng/đếm lùi giờ thuốc.
* **Theo dõi danh sách Caregiver đã liên kết với bệnh nhân:** Kiểm tra số lượng người chăm sóc đã kết nối vào hồ sơ (tối đa 03 người).
* > [!IMPORTANT]
  > **Lưu ý về quyền tạo Care Plan:** Bác sĩ là người có quyền chuyên môn tối cao tạo, chỉnh sửa và phê duyệt nội dung y tế, có quyền sao chép Master Template để tùy chỉnh liều lượng thuốc theo thể trạng riêng của từng bệnh nhân. Điều dưỡng **chỉ được nhập thông tin cơ bản của bệnh nhân** và áp dụng mẫu Care Plan đã được Bác sĩ phê duyệt; **tuyệt đối không được chỉnh sửa liều lượng thuốc**, danh mục thuốc, tiêu chí Red Flags hoặc hướng dẫn điều trị.

#### 4. Chăm sóc khách hàng (Customer Care - CSKH)
* **Tra cứu thông tin tài khoản:** Tìm kiếm tài khoản người dùng theo số điện thoại để hỗ trợ giải quyết sự cố.
* **Hỗ trợ đăng ký và đăng nhập:** Hướng dẫn người dùng nhận mã OTP, khắc phục lỗi mất sóng hoặc sai định dạng số điện thoại.
* **Hỗ trợ vấn đề liên kết QR:** Trợ giúp thân nhân quét mã không nhận diện, camera mờ hoặc cấp lại liên kết khi đổi điện thoại.
* **Ghi nhận phản hồi và khiếu nại:** Lưu vết toàn bộ thắc mắc, phản ánh về dịch vụ bệnh viện vào hệ thống tiếp nhận.
* **Theo dõi trạng thái yêu cầu hỗ trợ:** Cập nhật tiến độ xử lý yêu cầu từ lúc mở (Open) đến lúc hoàn tất (Resolved).
* **Hướng dẫn người dùng sử dụng hệ thống:** Hướng dẫn xem cẩm nang, bật âm thanh đọc trợ năng và xác nhận cữ thuốc.
* **Chuyển các vấn đề kỹ thuật đến Admin hoặc bộ phận kỹ thuật:** Điều phối các lỗi phần mềm, lỗi máy in hoặc kết nối CSDL cho bộ phận IT.
* **Chuyển các vấn đề liên quan đến y tế cho Điều dưỡng/Bác sĩ:** Chuyển ngay các câu hỏi chuyên sâu về triệu chứng hoặc đổi thuốc cho Bác sĩ chuyên môn.
* > [!CAUTION]
  > **Giới hạn quyền bảo mật:** Nhân viên CSKH **chỉ được phép xem thông tin tài khoản và trạng thái hỗ trợ cần thiết**; tuyệt đối **không được tự ý truy cập toàn bộ hồ sơ y tế, bệnh án nhãn khoa chi tiết** của người bệnh nhằm tuân thủ Nghị định 13/2023/NĐ-CP.

#### 5. Caregiver (Người Chăm Sóc / Thân Nhân)
* **Đăng ký và đăng nhập bằng số điện thoại/OTP:** Truy cập tiện lợi, an toàn không cần ghi nhớ mật khẩu phức tạp.
* **Quét QR để liên kết với bệnh nhân:** Thao tác 1 chạm từ camera điện thoại để kết nối hồ sơ người thân xuất viện.
* **Xem danh sách bệnh nhân đang chăm sóc:** Hỗ trợ một Caregiver có thể chăm sóc cùng lúc nhiều người thân (ví dụ: cả bố và mẹ cùng mổ mắt).
* **Xem Care Plan được cấp quyền:** Theo dõi toàn bộ lộ trình hồi phục, cẩm nang và lịch trình được bệnh viện phân công.
* **Xem Learning Path:** Đọc các bài học hướng dẫn phục hồi thị lực theo từng giai đoạn.
* **Học các bài hướng dẫn chăm sóc:** Xem infographic đồ họa tĩnh về tư thế ngủ, cách đeo kính bảo hộ và kỹ thuật kéo mi tra thuốc.
* **Thực hiện Recovery Check:** Trả lời bộ 3–5 câu hỏi khảo sát ngắn vào mỗi buổi sáng trong 7 ngày đầu.
* **Xác nhận đã cho bệnh nhân uống thuốc:** Bấm nút xác nhận đã cho uống thuốc hoặc nhỏ mắt để lưu vết giờ dùng thuốc thực tế.
* **Xem lịch dùng thuốc:** Theo dõi danh sách thuốc chia theo 4 khung giờ Sáng / Trưa / Chiều / Tối với màu nắp lọ nhận diện rõ ràng.
* **Nhận thông báo khi đến giờ uống thuốc:** Nhận thông báo đẩy (Push Notification) hoặc tin nhắn nhắc nhở trước 15 phút.
* **Xem bộ đếm ngược thời gian dùng thuốc nhỏ mắt:** Kích hoạt timer đếm lùi 5–10 phút giữa 2 loại thuốc nhỏ mắt để chống rửa trôi dược chất.
* **Xem lịch tái khám:** Theo dõi 5 mốc tái khám quan trọng kèm thông báo nhắc hẹn trước 24 giờ.
* **Nhận cảnh báo Red Flag:** Nhận tín hiệu cảnh báo màu đỏ kèm nút gọi khẩn cấp 1 chạm đến Hotline VISI `0395 151 151`.
* **Gửi phản hồi hoặc ghi chú chăm sóc:** Nhập ghi chú về biểu hiện hàng ngày của người bệnh cho nhân viên y tế theo dõi.
* **Cập nhật tình trạng thực hiện nhiệm vụ chăm sóc:** Đánh dấu hoàn thành các hướng dẫn vệ sinh mắt và sinh hoạt.
* **Theo dõi lịch sử chăm sóc của bệnh nhân:** Xem lại biểu đồ tuân thủ thuốc và các câu trả lời khảo sát những ngày trước.
* **Quản lý tối đa 03 Caregiver cho mỗi bệnh nhân nếu được cấp quyền:** Caregiver chính (người quét QR đầu tiên) có quyền phê duyệt hoặc xóa quyền của Caregiver phụ (tối đa 3 người theo BR5).
* **Xem lịch sử các bệnh nhân đã từng chăm sóc:** Lưu trữ hồ sơ các ca phẫu thuật đã hoàn thành theo dõi để tra cứu lại khi cần.

#### 6. Admin (Quản Trị Viên Hệ Thống)
* **CRUD tài khoản người dùng:** Khởi tạo, xem danh sách, chỉnh sửa thông tin và khóa/vô hiệu hóa tài khoản nhân viên y tế.
* **Quản lý vai trò và quyền truy cập:** Cấu hình ma trận phân quyền RBAC và gán phạm vi cơ sở dữ liệu theo 5 chi nhánh VISI (BR26).
* **Khóa/mở khóa tài khoản:** Xử lý tức thì các tài khoản nhân sự nghỉ việc hoặc có nguy cơ mất an toàn thông tin (Session Revocation).
* **CRUD Audit Log:** Tra cứu, lọc, xuất báo cáo và giám sát nhật ký kiểm toán toàn hệ thống phục vụ thanh tra an toàn thông tin.
* **Xem lịch sử hoạt động hệ thống:** Giám sát tài nguyên máy chủ, hàng đợi gửi tin nhắn OTP/SMS và tiến trình tự động leo thang cảnh báo Red Flag.
* **Quản lý cấu hình hệ thống:** Thiết lập tham số hệ thống toàn cục: thời gian timeout phiên làm việc, SLA cảnh báo, tích hợp cổng SMS/ZNS.

#### 7. Care Recipient (Bệnh Nhân / Người Thụ Hưởng Chăm Sóc)
* **Đăng ký và đăng nhập bằng số điện thoại/OTP:** Đăng nhập trực tiếp trên điện thoại cá nhân thông qua mã OTP bảo mật.
* **Xem thông tin cá nhân và hồ sơ y tế của bản thân:** Tự tra cứu thông tin hành chính, loại phẫu thuật và mắt mổ của chính mình.
* **Xem Care Plan được Bác sĩ hoặc Điều dưỡng cấp quyền:** Theo dõi tiến trình điều trị và hướng dẫn chăm sóc chuyên biệt cho bản thân.
* **Xem danh sách thuốc và hướng dẫn sử dụng thuốc:** Xem tên thuốc, hình ảnh lọ thuốc, số giọt và công dụng dược lý.
* **Nhận thông báo khi đến giờ uống thuốc:** Nhận chuông báo hoặc thông báo nhắc nhở các cữ thuốc trong ngày.
* **Xác nhận bản thân đã uống thuốc hoặc sử dụng thuốc nhỏ mắt:** Tự bấm nút xác nhận khi tự thực hiện việc tra thuốc.
* **Xem bộ đếm ngược thời gian dùng thuốc nhỏ mắt:** Theo dõi bộ đếm ngược thời gian giãn cách giữa các lần dùng thuốc nhỏ mắt (Buffer Timer 5–10 phút) để chống hiện tượng rửa trôi thuốc và bảo đảm an toàn dược lý nhãn khoa.
* **Xem lịch tái khám:** Tra cứu thời gian và địa điểm tái khám tại cơ sở VISI đã phẫu thuật.
* **Nhận thông báo và nhắc lịch tái khám:** Nhận tin nhắn thông báo tự động trước ngày hẹn khám lại.
* **Xem Learning Path:** Tiếp cận lộ trình kiến thức phục hồi mắt được cá nhân hóa theo loại phẫu thuật.
* **Học các bài hướng dẫn chăm sóc:** Đọc bài học, xem infographic phóng to chữ hoặc nghe đọc hướng dẫn âm thanh (Audio Guide - F-025).
* **Thực hiện Recovery Check:** Tự đánh giá mức độ cộm xốn, đau nhức hoặc thị lực của mắt hàng ngày nếu không có người thân bên cạnh.
* **Cập nhật tình trạng sức khỏe hằng ngày:** Nhập nhanh cảm nhận về sự tiến triển của mắt sau mổ.
* **Gửi ghi chú về triệu chứng hoặc tình trạng bất thường:** Mô tả hiện tượng lạ gửi thẳng đến Dashboard theo dõi của bệnh viện.
* **Nhận cảnh báo Red Flag:** Hiển thị màn hình cảnh báo đỏ khẩn cấp và gọi Hotline `0395 151 151` chỉ với 1 chạm.
* **Gửi phản hồi hoặc yêu cầu hỗ trợ:** Đặt câu hỏi thắc mắc cho nhân viên chăm sóc khách hàng.
* **Xem lịch sử chăm sóc và quá trình điều trị của bản thân:** Xem lại dòng thời gian từ ngày phẫu thuật đến hiện tại.
* **Xem tiến độ hoàn thành nhiệm vụ chăm sóc:** Theo dõi tỷ lệ % hoàn thành uống thuốc và kiểm tra sức khỏe mỗi ngày.
* **Cập nhật thông tin cá nhân trong phạm vi được phép:** Chỉnh sửa số điện thoại liên lạc khẩn cấp hoặc địa chỉ nhận thuốc.
* **Tải lên hình ảnh hoặc tài liệu y tế của bản thân nếu được cấp quyền:** Chụp ảnh mắt mổ gửi bác sĩ đánh giá từ xa khi có nghi ngờ nhiễm trùng/chảy dịch.

---

## 2. ĐẶC TẢ CHI TIẾT TỪNG USE CASE VÀ CÁC THAO TÁC CRUD

### UC-001: Đăng ký và Đăng nhập Caregiver qua OTP (Caregiver Authentication)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-001** | | |
| **Tên Use Case (Use Case Name)** | Đăng ký và Đăng nhập Caregiver qua OTP (Caregiver Authentication) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-009 (SMS / ZNS Gateway) | | |
| **Tính năng liên quan (Features)** | F-001, F-003 | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả quy trình Caregiver đăng nhập hoặc đăng ký tài khoản lần đầu bằng Số điện thoại di động và mã OTP gửi qua SMS/ZNS để truy cập vào hệ thống chăm sóc hậu phẫu. | | |
| **Mục tiêu (Goal)** | Xác thực số điện thoại và định danh an toàn cho Caregiver không cần mật khẩu; thiết lập phiên làm việc bảo mật theo Nghị định 13/2023/NĐ-CP. | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver mở ứng dụng Web App (PWA) hoặc quét mã QR trên phiếu xuất viện khi chưa có phiên đăng nhập. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Caregiver có thiết bị di động có kết nối internet và trình duyệt web.<br>2. Số điện thoại sẵn sàng nhận tin nhắn SMS hoặc Zalo ZNS.<br>3. Cổng viễn thông ACT-009 đang hoạt động bình thường. | | |
| **Điều kiện sau (Post-conditions)** | 1. Phiên làm việc bảo mật (JWT) được thiết lập trên thiết bị.<br>2. Hồ sơ Caregiver được tạo mới hoặc cập nhật.<br>3. Chuyển tiếp vào Trang chủ hoặc luồng liên kết bệnh nhân. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver nhập số điện thoại di động (10 chữ số) và nhấn 'Tiếp tục' | Hệ thống kiểm tra định dạng số điện thoại Việt Nam (BR1). Nếu hợp lệ, tạo mã OTP 6 chữ số ngẫu nhiên có hiệu lực 5 phút, gọi SMS/ZNS Gateway để gửi mã và hiển thị màn hình nhập OTP. |
| | 2 | Caregiver kiểm tra tin nhắn và nhập mã OTP 6 chữ số vào ứng dụng | Hệ thống kiểm tra tính chính xác và thời hạn của OTP (BR2). Nếu đúng, cấp phát JWT token, tạo mới hồ sơ nếu là người dùng mới (F-003), và chuyển tiếp vào ứng dụng. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Đăng nhập sau khi quét QR xuất viện** | 1 | Caregiver quét mã QR trên phiếu xuất viện khi chưa đăng nhập | Hệ thống lưu tạm token QR vào bộ nhớ đệm, hoàn tất đăng nhập OTP xong thì tự động thực hiện liên kết bệnh nhân (UC-003) và mở thẳng Care Plan. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Số điện thoại không đúng định dạng** | 1 | Caregiver nhập thiếu/thừa số hoặc ký tự lạ | Hệ thống hiển thị thông báo lỗi 'Số điện thoại không hợp lệ' và yêu cầu nhập lại. |
| **E2: Sai mã OTP** | 1 | Caregiver nhập mã không khớp | Hệ thống thông báo 'Mã OTP không chính xác' và hiển thị số lần thử còn lại. |
| **E3: Vượt quá giới hạn thử OTP** | 1 | Nhập sai quá 5 lần liên tiếp | Hệ thống tạm khóa yêu cầu OTP trong 15 phút để chống tấn công brute-force. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR1 (Kiểm tra SĐT), BR2 (Xác thực OTP), BR3 (Phân quyền Caregiver) | | |

---

---

---

### UC-002: Đăng nhập Nhân viên Y tế và Xác thực 2FA (Staff Authentication & 2FA)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-002** | | |
| **Tên Use Case (Use Case Name)** | Đăng nhập Nhân viên Y tế và Xác thực 2FA (Staff Authentication & 2FA) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-002 (Bác sĩ), ACT-003 (Điều dưỡng), ACT-004 (CSKH) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-007 (System Admin) | | |
| **Tính năng liên quan (Features)** | F-002, F-023 | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả quy trình nhân viên y tế (Bác sĩ, Điều dưỡng, CSKH) đăng nhập vào cổng thông tin bệnh viện RemiCare Portal bằng tài khoản nội bộ cấp phát kết hợp xác thực 2 lớp (2FA). | | |
| **Mục tiêu (Goal)** | Xác thực danh tính chuyên môn nhân viên y tế theo vai trò và cơ sở bệnh viện (5 bệnh viện chuỗi VISI), bảo vệ dữ liệu bệnh án nhạy cảm. | | |
| **Tác nhân kích hoạt (Trigger)** | Nhân viên y tế truy cập cổng quản trị `portal.remicare.visi.vn` trên máy trạm bệnh viện. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Tài khoản đã được Quản trị viên (ACT-007) cấp phát và gán vai trò RBAC cùng chi nhánh bệnh viện (F-023).<br>2. Tài khoản đang ở trạng thái Hoạt động (`ACTIVE`). | | |
| **Điều kiện sau (Post-conditions)** | 1. Nhân viên đăng nhập thành công vào phân hệ được phân quyền tương ứng (Bác sĩ -> Cấu hình/bệnh án, Điều dưỡng -> Quầy lưu viện/in QR, CSKH -> Dashboard giám sát).<br>2. Ghi nhận nhật ký đăng nhập vào Audit Trail. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Nhân viên nhập Tên đăng nhập và Mật khẩu nội bộ | Hệ thống kiểm tra thông tin đăng nhập, xác định vai trò và chi nhánh công tác. |
| | 2 | Nhân viên nhập mã xác thực 2 lớp (2FA) từ ứng dụng Authenticator hoặc OTP SMS | Hệ thống xác minh mã 2FA, cấp phiên làm việc có chữ ký số và điều hướng vào bảng điều khiển theo vai trò (BR14, BR15). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Sai thông tin đăng nhập hoặc 2FA** | 1 | Nhập sai mật khẩu hoặc mã 2FA | Hệ thống báo lỗi và ghi nhận số lần thất bại; khóa tài khoản sau 5 lần sai liên tiếp. |
| **E2: Tài khoản bị khóa / hết hạn** | 1 | Nhân viên đã nghỉ việc hoặc bị đình chỉ | Hệ thống từ chối truy cập và yêu cầu liên hệ IT Bệnh viện. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR14 (Kiểm soát quyền truy cập nhân viên), BR15 (Phân tách dữ liệu theo cơ sở bệnh viện) | | |

---

---

---

### UC-003: Quét Mã QR Bàn Giao Liên Kết Hồ Sơ Bệnh Nhân (Caregiver-Patient Linking)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-003** | | |
| **Tên Use Case (Use Case Name)** | Quét Mã QR Bàn Giao Liên Kết Hồ Sơ Bệnh Nhân (Caregiver-Patient Linking) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-003 (Điều dưỡng), ACT-006 (Bệnh nhân) | | |
| **Tính năng liên quan (Features)** | F-004, F-021 | | |
| **Mô tả tóm tắt (Brief Description)** | Caregiver sử dụng camera điện thoại quét mã QR in trên Phiếu xuất viện do Điều dưỡng bàn giao để gắn tài khoản chăm sóc vào bệnh nhân. | | |
| **Mục tiêu (Goal)** | Giải mã QR token từ phiếu xuất viện và thiết lập liên kết điện tử bảo mật giữa Caregiver và Kế hoạch chăm sóc bệnh nhân; hỗ trợ tối đa 3 Caregiver. | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver nhấn nút 'Quét mã QR' trên ứng dụng RemiCare. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Caregiver đã đăng nhập ứng dụng.<br>2. Phiếu xuất viện có in mã QR hợp lệ đã được Điều dưỡng kích hoạt.<br>3. Trình duyệt được cấp quyền truy cập camera. | | |
| **Điều kiện sau (Post-conditions)** | 1. Bản ghi liên kết được tạo trong `caregiver_patient_links`.<br>2. Caregiver được phân quyền xem lịch thuốc, cẩm nang và nộp Recovery Check của bệnh nhân.<br>3. Đồng bộ dữ liệu chăm sóc. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver nhấn nút 'Quét mã QR' trên màn hình chính | Ứng dụng kích hoạt camera và hiển thị khung quét mã QR. |
| | 2 | Caregiver hướng camera vào mã QR trên Phiếu xuất viện | Hệ thống quét, giải mã token bảo mật và truy vấn dữ liệu Care Plan tương ứng (BR4). |
| | 3 | Caregiver kiểm tra thông tin tóm tắt bệnh nhân (Họ tên viết tắt, Năm sinh, Mắt phẫu thuật, Bác sĩ mổ) và nhấn 'Xác nhận liên kết' | Hệ thống kiểm tra số lượng Caregiver đã liên kết (BR5). Nếu ≤3, lưu bản ghi liên kết, thông báo thành công và chuyển vào Trang chủ bệnh nhân. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Caregiver thứ 2 hoặc thứ 3 quét mã** | 1 | Thành viên khác trong gia đình quét cùng mã QR | Hệ thống ghi nhận thêm liên kết người chăm sóc đồng hành, đồng bộ dữ liệu cữ thuốc theo thời gian thực. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Mã QR hết hạn hoặc bị thu hồi** | 1 | Quét mã QR cũ đã bị cấp lại | Hệ thống báo lỗi 'Mã QR không hợp lệ hoặc đã bị thu hồi' và hướng dẫn liên hệ Điều dưỡng (BR4). |
| **E2: Đã vượt quá 3 Caregiver** | 1 | Có người thứ 4 cố gắng quét mã liên kết | Hệ thống từ chối liên kết và hiển thị thông báo đã đủ giới hạn tối đa 3 người chăm sóc (BR5). |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR4 (Tính hợp lệ của mã QR), BR5 (Giới hạn tối đa 3 Caregiver liên kết) | | |

---

---

---

### NHÓM UC-004: QUẢN LÝ HỒ SƠ ĐỊNH DANH BỆNH NHÂN (PATIENT PROFILES CRUD)

> **Mô tả nhóm nghiệp vụ:** Quản lý toàn diện dữ liệu định danh bệnh nhân hậu phẫu nhãn khoa tại 5 cơ sở VISI. Nhóm nghiệp vụ này được phân rã thành 5 Use Case CRUD đơn nguyên nhằm giúp đội ngũ phát triển (Dev) phân định rành mạch các API endpoint RESTful, kiểm tra hợp lệ dữ liệu y khoa, và bảo đảm tuân thủ nghiêm ngặt Nghị định 13/2023/NĐ-CP về bảo vệ dữ liệu cá nhân.

#### UC-004.1: Tạo Mới Hồ Sơ Bệnh Nhân (Create Patient Profile — POST /api/v1/patients)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-004.1** | | |
| **Tên Use Case (Use Case Name)** | Tạo Mới Hồ Sơ Bệnh Nhân (Create Patient Profile — POST /api/v1/patients) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Điều dưỡng lưu viện (ACT-003) / Tiếp đón (ACT-007) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Bác sĩ điều trị (ACT-002), Hệ thống VISI Core, CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-004 (Quản lý Hồ sơ Bệnh nhân Hậu phẫu), F-025 (Bảo mật Dữ liệu Y tế NĐ 13/2023) | | |
| **Mô tả tóm tắt (Brief Description)** | Điều dưỡng hoặc lễ tân tiếp đón nhập các thông tin hành chính và phẫu thuật của bệnh nhân vào hệ thống sau khi ca mổ kết thúc tại phòng lưu viện. | | |
| **Mục tiêu (Goal)** | Tạo mới hồ sơ bệnh nhân hậu phẫu trong thời gian <30 giây với tên viết tắt bảo mật và thông tin phẫu thuật chuẩn xác. | | |
| **Tác nhân kích hoạt (Trigger)** | Nhân viên y tế bấm nút '+ Thêm Bệnh Nhân Mới' trên màn hình SCR-DOC-03 (Quản lý Bệnh nhân). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Nhân viên y tế đã đăng nhập thành công vào Hospital Clinical Portal (UC-002) và có vai trò NURSE hoặc RECEPTIONIST tại cơ sở trực thuộc. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi mới được tạo trong bảng `patients` với trạng thái `ACTIVE`, sinh mã định danh duy nhất `patient_id` (UUIDv4) và mã nội bộ `PAT-YYYYMM-XXXX`; sẵn sàng để gán Care Plan. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Điều dưỡng chọn nút '+ Thêm Bệnh Nhân Mới' tại giao diện SCR-DOC-03. | Hệ thống mở modal form 'Tạo Mới Hồ Sơ Bệnh Nhân'. |
| | 2 | Điều dưỡng nhập Họ tên đầy đủ bệnh nhân (ví dụ: 'Nguyễn Văn An'). | Hệ thống tự động sinh tên hiển thị viết tắt theo chuẩn NĐ 13/2023 (ví dụ: 'Ng. V. An') và gợi ý vào trường Tên hiển thị. |
| | 3 | Điều dưỡng nhập Số điện thoại liên hệ (10 chữ số), Năm sinh, Giới tính. | Hệ thống kiểm tra định dạng SĐT Việt Nam hợp lệ và tính toán tuổi bệnh nhân. |
| | 4 | Điều dưỡng chọn Mắt phẫu thuật (Mắt Phải - OD / Mắt Trái - OS / Cả Hai Mắt - OU). | Hệ thống đánh dấu trực quan mắt can thiệp trên sơ đồ nhãn khoa. |
| | 5 | Điều dưỡng chọn Loại phẫu thuật (Phaco, SMILE, Femto-Lasik, SILK) và Ngày phẫu thuật. | Hệ thống tự động gán cơ sở bệnh viện (`facility_id`) theo cơ sở mà điều dưỡng đang công tác (BR26). |
| | 6 | Điều dưỡng kiểm tra thông tin và bấm nút 'Lưu & Chuyển Sang Gán Care Plan'. | Hệ thống gửi `POST /api/v1/patients`, lưu bản ghi vào CSDL, ghi nhận Audit Log (UC-027), trả về HTTP 201 Created và tự động chuyển giao diện sang SCR-DOC-04. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Số điện thoại trùng lặp với hồ sơ đang hoạt động** | 1 | Hệ thống phát hiện SĐT đã tồn tại bệnh nhân đang theo dõi trong vòng 30 ngày. | Hệ thống trả về HTTP 409 Conflict: 'Bệnh nhân với số điện thoại này đang có hồ sơ điều trị hoạt động tại viện. Bạn có muốn xem lại hồ sơ cũ không?'. |
| **Thiếu thông tin phẫu thuật bắt buộc** | 1 | Điều dưỡng bỏ sót trường Mắt mổ hoặc Loại phẫu thuật và bấm Lưu. | Hệ thống trả về HTTP 422 Unprocessable Entity, viền đỏ các trường thiếu và thông báo: 'Vui lòng chọn Mắt mổ và Loại phẫu thuật'. |
| **Mức độ ưu tiên (Priority)** | **Cao (High - Bắt buộc cho luồng xuất viện)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR17 (Bảo mật Tên viết tắt NĐ 13/2023), BR18 (Thời gian nhập liệu chuẩn hóa <30s), BR26 (Phân quyền phạm vi chi nhánh) | | |

---

#### UC-004.2: Tra Cứu và Lọc Danh Sách Hồ Sơ Bệnh Nhân (Read/List Patient Profiles — GET /api/v1/patients)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-004.2** | | |
| **Tên Use Case (Use Case Name)** | Tra Cứu và Lọc Danh Sách Hồ Sơ Bệnh Nhân (Read/List Patient Profiles — GET /api/v1/patients) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Điều dưỡng (ACT-003), Bác sĩ điều trị (ACT-002), Nhân viên CSKH (ACT-004) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-004 (Quản lý Hồ sơ Bệnh nhân Hậu phẫu), F-018 (Dashboard Giám sát Lâm sàng) | | |
| **Mô tả tóm tắt (Brief Description)** | Cung cấp bảng dữ liệu danh sách bệnh nhân phân trang, cho phép lọc theo loại mổ, khoảng ngày phẫu thuật và trạng thái phục hồi cảnh báo (Đỏ / Vàng / Xanh). | | |
| **Mục tiêu (Goal)** | Truy xuất và lọc danh sách bệnh nhân xuất viện theo cơ sở làm việc, hỗ trợ tìm kiếm nhanh theo mã hồ sơ, tên viết tắt hoặc số điện thoại. | | |
| **Tác nhân kích hoạt (Trigger)** | Nhân viên y tế truy cập menu 'Bệnh Nhân Hậu Phẫu' (SCR-DOC-03). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Nhân viên đã đăng nhập và được gắn `facility_id` hợp lệ trong phiên làm việc JWT. | | |
| **Điều kiện sau (Post-conditions)** | Danh sách bệnh nhân hiển thị đúng phạm vi quyền hạn cơ sở, bảo đảm không rò rỉ dữ liệu chéo giữa các chi nhánh bệnh viện (BR26). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Nhân viên truy cập menu 'Danh sách Bệnh nhân' (SCR-DOC-03). | Frontend gửi request `GET /api/v1/patients?facility_id={id}&page=1&limit=20` kèm access token. |
| | 2 | Backend trích xuất `facility_id` từ JWT token. | Hệ thống thực thi truy vấn có điều kiện `WHERE facility_id = :facility_id AND status != 'DELETED'`. |
| | 3 | Hệ thống tải dữ liệu và hiển thị bảng phân trang. | Bảng hiển thị: Mã BN, Tên viết tắt bảo mật, Mắt mổ, Loại mổ, Ngày mổ, Tên gói Care Plan, Huy hiệu trạng thái cảnh báo (Đỏ/Vàng/Xanh). |
| | 4 | Nhân viên nhập từ khóa tìm kiếm (Tên viết tắt hoặc 4 số cuối SĐT) hoặc chọn bộ lọc Loại mổ (ví dụ: 'Phaco'). | Frontend debounce 300ms và gửi request lọc cập nhật bảng danh sách tức thì. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Lọc danh sách ca có cảnh báo Đỏ/Vàng** | 1 | Nhân viên CSKH bấm tab bộ lọc nhanh 'Cần Can Thiệp Gấp'. | Hệ thống hiển thị ưu tiên các bệnh nhân có `alert_level IN ('RED', 'YELLOW')` lên đầu danh sách. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Truy cập vượt quyền cơ sở khác** | 1 | Nhân viên cố tình can thiệp query param `facility_id` của chi nhánh khác. | Hệ thống trả về HTTP 403 Forbidden: 'Bạn không có quyền truy cập dữ liệu bệnh nhân của cơ sở này' và ghi log an ninh. |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR17 (Ẩn danh hóa thông tin cá nhân), BR26 (Ranh giới bảo mật đa cơ sở) | | |

---

#### UC-004.3: Xem Chi Tiết Hồ Sơ Bệnh Nhân & Dòng Thời Gian Phục Hồi (Read/Detail Patient Profile — GET /api/v1/patients/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-004.3** | | |
| **Tên Use Case (Use Case Name)** | Xem Chi Tiết Hồ Sơ Bệnh Nhân & Dòng Thời Gian Phục Hồi (Read/Detail Patient Profile — GET /api/v1/patients/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ điều trị (ACT-002), Điều dưỡng (ACT-003), Nhân viên CSKH (ACT-004) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-004, F-018, F-020 (Xử lý Báo động Đỏ Red Flag) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ hoặc CSKH xem thông tin chi tiết toàn diện của một ca bệnh để đưa ra quyết định chuyên môn hoặc ghi nhận can thiệp qua điện thoại. | | |
| **Mục tiêu (Goal)** | Hiển thị hồ sơ bệnh án số 360 độ của một bệnh nhân cụ thể, bao gồm tiến trình dùng thuốc, lịch sử nộp khảo sát, nhật ký cảnh báo và người chăm sóc liên kết. | | |
| **Tác nhân kích hoạt (Trigger)** | Nhân viên bấm vào một dòng bệnh nhân tại bảng danh sách SCR-DOC-03. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bệnh nhân tồn tại trong hệ thống và thuộc cơ sở nhân viên được phân quyền. | | |
| **Điều kiện sau (Post-conditions)** | Giao diện chi tiết bệnh nhân hiển thị đầy đủ các thẻ thông tin lâm sàng (SCR-DOC-05 / SCR-DOC-06). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Nhân viên bấm chọn bệnh nhân có ID cụ thể. | Frontend gửi request `GET /api/v1/patients/{id}` kèm các liên kết quan hệ. |
| | 2 | Hệ thống xác thực quyền truy cập cơ sở. | Backend tập hợp dữ liệu từ các bảng `patients`, `patient_care_plans`, `patient_medications`, `recovery_check_submissions`, `caregiver_patient_links`. |
| | 3 | Hệ thống trả về HTTP 200 OK kèm JSON chi tiết. | Giao diện hiển thị 4 phân khu lâm sàng: (1) Thẻ định danh & Phẫu thuật; (2) Lịch dùng thuốc & Tỷ lệ tuân thủ; (3) Timeline Recovery Check & Lịch tái khám; (4) Danh sách Caregiver đã liên kết. |
| | 4 | Nhân viên chuyển đổi qua lại giữa các tab dữ liệu. | Hệ thống render mượt mà biểu đồ phục hồi và lịch sử trả lời khảo sát từng ngày. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Bệnh nhân không tồn tại** | 1 | Request gửi `id` không có trong CSDL hoặc đã bị xóa hẳn. | Hệ thống trả về HTTP 404 Not Found: 'Hồ sơ bệnh nhân không tồn tại trên hệ thống'. |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR5 (Tối đa 3 Caregiver liên kết), BR17 (Quy chuẩn bảo mật NĐ 13/2023), BR24 (5 mốc theo dõi chuẩn) | | |

---

#### UC-004.4: Cập Nhật Chỉnh Sửa Thông Tin Hồ Sơ Bệnh Nhân (Update Patient Profile — PUT /api/v1/patients/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-004.4** | | |
| **Tên Use Case (Use Case Name)** | Cập Nhật Chỉnh Sửa Thông Tin Hồ Sơ Bệnh Nhân (Update Patient Profile — PUT /api/v1/patients/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Điều dưỡng lưu viện (ACT-003), Bác sĩ điều trị (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| CSDL PostgreSQL, Hệ thống Audit Trail | | |
| **Tính năng liên quan (Features)** | F-004, F-027 (Nhật ký Kiểm toán Hệ thống) | | |
| **Mô tả tóm tắt (Brief Description)** | Cho phép điều dưỡng sửa các trường thông tin hành chính được phép; khóa cố định Loại phẫu thuật và Mắt mổ nếu Care Plan đã được kích hoạt để đảm bảo an toàn lâm sàng. | | |
| **Mục tiêu (Goal)** | Chỉnh sửa chính xác các sai sót hành chính (SĐT, Tên hiển thị, Năm sinh, Tiền sử dị ứng) mà không làm gián đoạn kế hoạch chăm sóc đang chạy. | | |
| **Tác nhân kích hoạt (Trigger)** | Nhân viên bấm nút 'Chỉnh sửa' tại trang chi tiết hồ sơ bệnh nhân (SCR-DOC-05). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bệnh nhân ở trạng thái `ACTIVE` và thuộc cơ sở nhân viên quản lý. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi `patients` được cập nhật `updated_at`, lưu vết toàn bộ sai khác (diff) vào `system_audit_logs`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Điều dưỡng bấm nút 'Chỉnh sửa Hồ sơ' tại SCR-DOC-05. | Hệ thống mở form chỉnh sửa cho phép sửa SĐT, Tên hiển thị, Địa chỉ liên lạc, Ghi chú cơ địa dị ứng thuốc. |
| | 2 | Điều dưỡng cập nhật số điện thoại mới của người bệnh. | Hệ thống kiểm tra tính hợp lệ của số điện thoại mới. |
| | 3 | Điều dưỡng bấm nút 'Lưu Thay Đổi'. | Frontend gửi request `PUT /api/v1/patients/{id}` kèm payload cập nhật. |
| | 4 | Backend kiểm tra tính toàn vẹn và phân quyền. | Hệ thống ghi nhận thay đổi vào CSDL, ghi nhật ký kiểm toán (Audit Trail) chi tiết giá trị cũ/mới. |
| | 5 | Hệ thống trả về HTTP 200 OK. | Hiển thị thông báo 'Cập nhật thông tin bệnh nhân thành công' và cập nhật lại giao diện. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Thay đổi thông tin lâm sàng cốt lõi khi Care Plan đã chạy** | 1 | Điều dưỡng cố tình sửa Loại mổ khi Care Plan đã bàn giao cho Caregiver. | Hệ thống chặn thao tác, trả về HTTP 400 Bad Request: 'Không thể thay đổi Loại phẫu thuật khi Kế hoạch chăm sóc đã kích hoạt. Vui lòng liên hệ Bác sĩ trưởng khoa nếu cần tái lập Care Plan'. |
| **Số điện thoại mới bị trùng lặp** | 1 | SĐT cập nhật trùng với bệnh nhân khác. | Hệ thống trả về HTTP 409 Conflict: 'Số điện thoại này đã được sử dụng bởi một hồ sơ bệnh nhân khác'. |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR17 (Bảo mật thông tin cá nhân), BR26 (Kiểm soát quyền truy cập chi nhánh) | | |

---

#### UC-004.5: Lưu Trữ / Vô Hiệu Hóa Hồ Sơ Bệnh Nhân (Archive/Soft Delete — DELETE /api/v1/patients/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-004.5** | | |
| **Tên Use Case (Use Case Name)** | Lưu Trữ / Vô Hiệu Hóa Hồ Sơ Bệnh Nhân (Archive/Soft Delete — DELETE /api/v1/patients/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Điều dưỡng trưởng (ACT-003) / Bác sĩ trưởng khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| CSDL PostgreSQL, Hệ thống Audit Trail | | |
| **Tính năng liên quan (Features)** | F-004, F-025, F-027 | | |
| **Mô tả tóm tắt (Brief Description)** | Chuyển trạng thái hồ sơ sang `ARCHIVED`, vô hiệu hóa mã QR bàn giao liên quan nếu chưa kích hoạt, ngắt luồng thông báo tự động và ghi nhận lý do lưu trữ. | | |
| **Mục tiêu (Goal)** | Lưu trữ an toàn hồ sơ bệnh nhân (Soft Delete) đối với các trường hợp nhập sai hoặc kết thúc theo dõi dài hạn, tuyệt đối không xóa cứng dữ liệu y khoa. | | |
| **Tác nhân kích hoạt (Trigger)** | Nhân viên y tế có thẩm quyền chọn chức năng 'Lưu trữ / Hủy Hồ sơ' trong menu thao tác nâng cao. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Hồ sơ bệnh nhân tồn tại, không có sự cố Red Flag đang chờ xử lý (`alert_status = 'TRIGGERED'`). | | |
| **Điều kiện sau (Post-conditions)** | Trường `status` của bệnh nhân chuyển sang `'ARCHIVED'`, `archived_at` ghi nhận thời gian hiện tại; hồ sơ ẩn khỏi danh sách theo dõi thường quy nhưng vẫn lưu vết trong kho lưu trữ dữ liệu y khoa. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Điều dưỡng trưởng chọn hồ sơ cần lưu trữ, bấm 'Lưu trữ Hồ sơ'. | Hệ thống hiển thị hộp thoại xác nhận yêu cầu nhập 'Lý do lưu trữ' (ví dụ: 'Hồ sơ nhập trùng lặp', 'Bệnh nhân chuyển viện điều trị'). |
| | 2 | Điều dưỡng nhập lý do và bấm 'Xác nhận Lưu trữ'. | Frontend gửi request `DELETE /api/v1/patients/{id}` kèm reason. |
| | 3 | Backend kiểm tra điều kiện an toàn lâm sàng. | Hệ thống xác minh không có cảnh báo Red Flag chưa xử lý; cập nhật `status = 'ARCHIVED'`, thu hồi QR code chưa dùng (`status = 'REVOKED'`). |
| | 4 | Hệ thống ghi log `ARCHIVE_PATIENT` vào `system_audit_logs`. | Hệ thống trả về HTTP 200 OK kèm thông điệp 'Đã chuyển hồ sơ bệnh nhân vào kho lưu trữ'. |
| | 5 | Hệ thống điều hướng về danh sách bệnh nhân active. | Hồ sơ vừa lưu trữ không còn xuất hiện trong danh sách theo dõi thường quy. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Hồ sơ đang có cảnh báo Red Flag chưa xử lý** | 1 | Bệnh nhân đang có biến chứng đỏ chưa được can thiệp. | Hệ thống chặn thao tác, trả về HTTP 400 Bad Request: 'Không thể lưu trữ hồ sơ khi đang có cảnh báo Báo động Đỏ chưa được CSKH xử lý hoàn tất' (BR12). |
| **Người thực hiện không có thẩm quyền** | 1 | Nhân viên tiếp đón thông thường thực hiện thao tác xóa. | Hệ thống trả về HTTP 403 Forbidden: 'Chỉ Bác sĩ trưởng khoa hoặc Điều dưỡng trưởng mới có quyền lưu trữ hồ sơ'. |
| **Mức độ ưu tiên (Priority)** | **Thấp (Low - Nghiệp vụ kiểm soát đặc biệt)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR12 (Quy chuẩn phản hồi Red Flag SLA <5p), BR25 (Tuân thủ lưu trữ dữ liệu y khoa), BR26 (RBAC) | | |

---


### NHÓM UC-005: QUẢN LÝ DANH MỤC MẪU KẾ HOẠCH CHĂM SÓC (CARE PLAN MASTER TEMPLATES CRUD)

> **Mô tả nhóm nghiệp vụ:** Quản lý vòng đời và phiên bản của các gói phác đồ mẫu chuẩn hóa cho từng loại phẫu thuật nhãn khoa (Phaco, SMILE, Femto-Lasik, SILK). Phân rã thành 5 Use Case CRUD độc lập giúp phân biệt rõ ràng luồng khởi tạo, tra cứu, xem chi tiết, điều chỉnh và kích hoạt/ngưng sử dụng template.

#### UC-005.1: Tạo Mới Master Template (Create Master Template — POST /api/v1/templates)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-005.1** | | |
| **Tên Use Case (Use Case Name)** | Tạo Mới Master Template (Create Master Template — POST /api/v1/templates) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) / Giám đốc Lâm sàng GCMO (ACT-001) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-005 (Quản lý Master Care Plan Template) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ khởi tạo một Master Template mới, thiết lập tên mẫu, chỉ định loại phẫu thuật áp dụng và mô tả hướng dẫn y khoa cơ bản trước khi sang bước cấu hình chi tiết danh mục thuốc và bộ câu hỏi. | | |
| **Mục tiêu (Goal)** | Tạo mới một khung phác đồ mẫu chuẩn hóa cho loại phẫu thuật mắt cụ thể ở trạng thái Bản thảo (DRAFT) với phiên bản v1.0. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút '+ Tạo Mẫu Phác Đồ Mới' trên giao diện SCR-DOC-07. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đã đăng nhập portal lâm sàng và có quyền quản trị chuyên môn. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi mới được tạo trong `care_plan_templates` với `status = 'DRAFT'`, `version = 'v1.0'`, gán `created_by_doctor_id`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm nút '+ Tạo Mẫu Phác Đồ Mới' tại màn hình Danh mục Master Template (SCR-DOC-07). | Hệ thống hiển thị giao diện biểu mẫu 'Khởi Tạo Mẫu Kế Hoạch Chăm Sóc Mới'. |
| | 2 | Bác sĩ điền Tên phác đồ mẫu (ví dụ: 'Phác đồ Chăm sóc Hậu phẫu Phaco Chuẩn Quốc Tế VISI'). | Hệ thống tự động kiểm tra, đảm bảo tên mẫu không bị trùng lặp với phác đồ đang hoạt động. |
| | 3 | Bác sĩ chọn Loại phẫu thuật áp dụng (Phaco / SMILE / Femto-Lasik / SILK). | Hệ thống tự động gán loại mổ tương ứng và tải lên các thiết lập tiêu chuẩn của loại phẫu thuật đó (BR16). |
| | 4 | Bác sĩ nhập Mục tiêu chăm sóc y khoa và Số ngày theo dõi chuẩn (mặc định 30 ngày). | Hệ thống hiển thị bản tóm tắt thông tin cơ bản của phác đồ mẫu. |
| | 5 | Bác sĩ bấm nút 'Khởi Tạo Bản Nháp (Draft)'. | Hệ thống tạo bản ghi mới ở trạng thái DRAFT v1.0, lưu vào CSDL và tự động mở Không gian Cấu hình Chi tiết (SCR-DOC-08). |
| | 6 | Bác sĩ thiết lập Danh mục Thuốc mẫu & Cài đặt Bộ đếm giãn cách (Tab Thuốc mẫu — F-007, BR23). | Bác sĩ thêm danh sách thuốc nhỏ mắt/thuốc uống, số giọt, lịch cữ (Sáng/Trưa/Chiều/Tối), màu nắp lọ và đặt thời gian đệm 5–10 phút chống rửa trôi thuốc; hệ thống tự động lưu danh mục thuốc. |
| | 7 | Bác sĩ thiết lập Bộ câu hỏi Recovery Check & Dấu hiệu nguy hiểm Red Flag (Tab Khảo sát — F-008, BR11, BR24). | Bác sĩ cài đặt 5 mốc khảo sát định kỳ (Ngày 1, 3, 7, 14, 30) với câu hỏi 3 màu (Xanh/Vàng/Đỏ) và cấu hình tiêu chí báo động đỏ gắn số Hotline VISI 0395 151 151; hệ thống liên kết quy tắc cảnh báo. |
| | 8 | Bác sĩ thiết lập Cẩm nang 24h đầu, Infographic bài học & Bảng Nên làm / Cần tránh (Tab Cẩm nang — F-009, F-016). | Bác sĩ tải ảnh Infographic hướng dẫn, nhập lời dặn tóm tắt và nhập bảng 2 cột màu (Nên làm - DO / Cần tránh - DON'T) phân theo sinh hoạt (Tắm gội, Ăn uống, Vận động); hệ thống lưu trữ nội dung bài học. |
| | 9 | Bác sĩ kiểm tra lại toàn bộ cấu hình và bấm 'Gửi Phê Duyệt Chuyên Môn'. | Hệ thống kiểm tra tính đầy đủ về mặt y tế, chuyển trạng thái Template sang PENDING_APPROVAL và gửi thông báo tới Giám đốc Bệnh viện / GCMO để ký duyệt ban hành (UC-006). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Nhân bản từ một Template hiện có** | 1 | Bác sĩ chọn tùy chọn 'Nhân bản từ Template mẫu có sẵn'. | Hệ thống sao chép toàn bộ thuốc mẫu, câu hỏi và cẩm nang sang bản ghi draft mới với tên mới. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Tên Template bị trùng lặp** | 1 | Tên mẫu trùng với template active khác cùng loại phẫu thuật. | Hệ thống trả về HTTP 409 Conflict: 'Tên Master Template này đã tồn tại trong hệ thống. Vui lòng chọn tên gọi khác hoặc tăng số phiên bản'. |
| **Thiếu thông tin bắt buộc** | 1 | Bỏ trống Tên hoặc Loại phẫu thuật. | Hệ thống trả về HTTP 422 Unprocessable Entity kèm cảnh báo lỗi cụ thể. |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR16 (Tính duy nhất và đặc thù của Template theo loại phẫu thuật) | | |

---

#### UC-005.2: Xem Danh Sách & Lọc Master Template (Read/List Master Templates — GET /api/v1/templates)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-005.2** | | |
| **Tên Use Case (Use Case Name)** | Xem Danh Sách & Lọc Master Template (Read/List Master Templates — GET /api/v1/templates) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002), Giám đốc Lâm sàng (ACT-001), Điều dưỡng (ACT-003) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-005 (Quản lý Master Care Plan Template) | | |
| **Mô tả tóm tắt (Brief Description)** | Hiển thị thư viện các gói phác đồ mẫu dạng thẻ trực quan hoặc bảng dữ liệu, cung cấp thông tin tóm tắt: phiên bản, số loại thuốc, số mốc câu hỏi, trạng thái phê duyệt. | | |
| **Mục tiêu (Goal)** | Truy xuất toàn bộ danh mục các gói phác đồ mẫu, phân loại theo loại mổ và trạng thái vòng đời. | | |
| **Tác nhân kích hoạt (Trigger)** | Người dùng truy cập menu 'Mẫu Phác Đồ Điều Trị' (SCR-DOC-07). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Người dùng đã đăng nhập hệ thống nội bộ bệnh viện. | | |
| **Điều kiện sau (Post-conditions)** | Danh sách phác đồ mẫu hiển thị đầy đủ, hỗ trợ lọc theo loại mổ và trạng thái. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Người dùng truy cập màn hình SCR-DOC-07. | Frontend gửi request `GET /api/v1/templates` kèm bộ lọc mặc định. |
| | 2 | Backend truy vấn bảng `care_plan_templates` kèm đếm số lượng thuốc và bài học liên kết. | Hệ thống trả về danh sách các template. |
| | 3 | Giao diện hiển thị danh sách trực quan. | Mỗi template hiển thị: Tên mẫu, Loại phẫu thuật, Phiên bản (v1.0, v1.1), Bác sĩ tạo, Ngày cập nhật, Huy hiệu trạng thái (`DRAFT` xám, `PENDING_APPROVAL` cam, `ACTIVE` xanh lá, `INACTIVE` đỏ). |
| | 4 | Người dùng chọn bộ lọc Loại mổ (ví dụ: 'SILK') hoặc Trạng thái ('ACTIVE'). | Danh sách được lọc tức thì theo tiêu chí đã chọn. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR16 (Liên kết loại phẫu thuật chuẩn hóa) | | |

---

#### UC-005.3: Xem Chi Tiết Cấu Hình Master Template (Read/Detail Master Template — GET /api/v1/templates/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-005.3** | | |
| **Tên Use Case (Use Case Name)** | Xem Chi Tiết Cấu Hình Master Template (Read/Detail Master Template — GET /api/v1/templates/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002), Giám đốc Lâm sàng (ACT-001), Điều dưỡng (ACT-003) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-005, F-007, F-008, F-009 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ truy cập vào không gian làm việc chuyên sâu để xem cấu hình danh mục thuốc mẫu, bộ câu hỏi phục hồi, tiêu chí Red Flag, cẩm nang 24h và bảng quy tắc Do/Don't. | | |
| **Mục tiêu (Goal)** | Tải và hiển thị toàn bộ 4 cấu phần chi tiết của một Master Template trong Không gian Cấu hình tập trung (SCR-DOC-08). | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút 'Cấu hình' hoặc bấm vào thẻ một Template tại SCR-DOC-07. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template tồn tại trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Tải đầy đủ dữ liệu cấu phần vào 4 tab của giao diện SCR-DOC-08. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn một Template cụ thể tại SCR-DOC-07. | Frontend gọi `GET /api/v1/templates/{id}` cùng các endpoint lấy sub-resources. |
| | 2 | Backend tổng hợp dữ liệu cấu hình từ các bảng `care_plan_templates`, `template_medications`, `template_recovery_milestones`, `template_red_flags`, `template_learning_modules`, `template_do_dont_items`. | Hệ thống trả về payload JSON tổng hợp. |
| | 3 | Giao diện SCR-DOC-08 hiển thị 4 tab điều hướng. | Tab 1: Thông tin chung & Phiên bản; Tab 2: Danh mục Thuốc mẫu & Timer đệm; Tab 3: Recovery Check & Red Flag; Tab 4: Cẩm nang 24h & Bảng Do/Don't. |
| | 4 | Bác sĩ chuyển qua lại giữa các tab để rà soát phác đồ. | Hệ thống hiển thị trạng thái chỉnh sửa tương ứng (nếu `DRAFT` cho phép sửa, nếu `ACTIVE` ở chế độ Read-only kèm nút 'Tạo phiên bản mới'). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Template không tồn tại** | 1 | ID template truyền vào không hợp lệ. | Hệ thống trả về HTTP 404 Not Found. |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 (Quy trình quản lý phiên bản bất biến khi đã duyệt), BR16 (Đặc thù loại mổ) | | |

---

#### UC-005.4: Chỉnh Sửa Thông Tin Chung Master Template (Update Master Template — PUT /api/v1/templates/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-005.4** | | |
| **Tên Use Case (Use Case Name)** | Chỉnh Sửa Thông Tin Chung Master Template (Update Master Template — PUT /api/v1/templates/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002), Giám đốc Lâm sàng (ACT-001) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| CSDL PostgreSQL, Audit Trail | | |
| **Tính năng liên quan (Features)** | F-005, F-027 | | |
| **Mô tả tóm tắt (Brief Description)** | Cho phép bác sĩ cập nhật thông tin chung. Nếu template đã được duyệt ACTIVE, hệ thống áp dụng cơ chế copy-on-write để tạo phiên bản mới v1.1 nhằm bảo vệ tính toàn vẹn của các Care Plan đang chạy (BR15, BR18). | | |
| **Mục tiêu (Goal)** | Cập nhật các thông tin mô tả y khoa, thời lượng theo dõi hoặc đổi tên gọi của Template khi ở trạng thái DRAFT. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm 'Lưu Thông Tin Chung' tại Tab 1 màn hình SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template tồn tại; người dùng có quyền chỉnh sửa. | | |
| **Điều kiện sau (Post-conditions)** | Thông tin chung được cập nhật; nếu tạo bản sao mới thì sinh `version = 'v1.1'` ở trạng thái `DRAFT`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ thay đổi Tên template, Mô tả hướng dẫn lâm sàng hoặc Số ngày theo dõi chuẩn. | Bác sĩ bấm 'Lưu Thay Đổi'. |
| | 2 | Backend kiểm tra trạng thái của template hiện tại. | Nếu template đang `DRAFT`: Cập nhật trực tiếp vào bản ghi và ghi log Audit Trail. Nếu template đang `ACTIVE`: Hệ thống hiển thị modal cảnh báo: 'Template này đã ban hành chính thức. Hệ thống sẽ tạo một phiên bản nháp mới (v1.1) để bạn chỉnh sửa mà không ảnh hưởng tới các bệnh nhân đang điều trị. Bạn có đồng ý không?'. |
| | 3 | Bác sĩ bấm 'Đồng ý tạo phiên bản mới'. | Hệ thống nhân bản toàn bộ cấu hình sang bản ghi mới với `version = 'v1.1'`, `status = 'DRAFT'`. Cập nhật thông tin mới. |
| | 4 | Hệ thống trả về HTTP 200 OK. | Hiển thị thông báo thành công và chuyển ngữ cảnh làm việc sang phiên bản mới. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Trùng tên template trong cùng loại mổ** | 1 | Tên mới sửa trùng với template khác. | Hệ thống trả về HTTP 409 Conflict. |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 (Version Control & Immutability của Master Template), BR18 (Tính độc lập của Care Plan đang chạy) | | |

---

#### UC-005.5: Kích Hoạt / Lưu Trữ Master Template (Patch Template Status — PATCH /api/v1/templates/{id}/status)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-005.5** | | |
| **Tên Use Case (Use Case Name)** | Kích Hoạt / Lưu Trữ Master Template (Patch Template Status — PATCH /api/v1/templates/{id}/status) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Giám đốc Lâm sàng GCMO (ACT-001) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| CSDL PostgreSQL, Audit Trail | | |
| **Tính năng liên quan (Features)** | F-005, F-006 (Phê duyệt Lâm sàng Master Template) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ gửi duyệt template (`PENDING_APPROVAL`), GCMO ký duyệt ban hành áp dụng toàn hệ thống (`ACTIVE`), hoặc chuyển lưu trữ ngưng sử dụng (`INACTIVE`) khi có phác đồ y khoa mới thay thế. | | |
| **Mục tiêu (Goal)** | Thay đổi trạng thái vòng đời của Master Template (`PENDING_APPROVAL`, `ACTIVE`, `INACTIVE`) theo thẩm quyền phê duyệt y khoa. | | |
| **Tác nhân kích hoạt (Trigger)** | GCMO hoặc Bác sĩ bấm nút chuyển trạng thái tương ứng trên header SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Người dùng có vai trò tương ứng theo ma trận phân quyền lâm sàng. | | |
| **Điều kiện sau (Post-conditions)** | Cập nhật cột `status` trong `care_plan_templates`, ghi nhận chữ ký điện tử và thời điểm phê duyệt nếu duyệt ban hành. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | GCMO xem xét toàn bộ cấu hình template tại SCR-DOC-08 và bấm 'Ban Hành Áp Dụng Toàn Viện'. | Hệ thống mở modal xác nhận ký duyệt lâm sàng. |
| | 2 | GCMO nhập mật khẩu / mã xác thực phê duyệt. | Frontend gửi request `PATCH /api/v1/templates/{id}/status` với body `{"status": "ACTIVE"}`. |
| | 3 | Backend kiểm tra điều kiện toàn vẹn lâm sàng: | - Phải có ít nhất 1 loại thuốc mẫu (F-007)
- Phải có ít nhất 1 mốc Recovery Check (F-008)
- Phải có ít nhất 1 tiêu chí Red Flag kèm Hotline VISI (BR11)
- Phải có cẩm nang 24h và bảng Do/Don't (F-009). |
| | 4 | Hệ thống kiểm tra đạt chuẩn toàn bộ tiêu chí. | Cập nhật `status = 'ACTIVE'`, `approved_by = gcmo_id`, `approved_at = CURRENT_TIMESTAMP`. |
| | 5 | Hệ thống ghi nhận Audit Log cấp độ nghiêm ngặt. | Gửi thông báo broadcast tới toàn bộ bác sĩ tại 5 chi nhánh VISI về phác đồ mới ban hành. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Lưu trữ phác đồ cũ không dùng nữa** | 1 | GCMO chọn trạng thái 'INACTIVE' cho template cũ. | Hệ thống chuyển trạng thái sang `INACTIVE`. Template này không còn hiển thị trong danh sách gán mới cho bệnh nhân, nhưng các Care Plan đã gán trước đó vẫn tiếp tục hoạt động bình thường. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Cấu hình chưa đủ tiêu chuẩn an toàn lâm sàng** | 1 | Template thiếu tiêu chí Red Flag hoặc thiếu danh mục thuốc mẫu. | Hệ thống từ chối kích hoạt, trả về HTTP 400 Bad Request kèm danh sách các thiếu sót cần bổ sung trước khi ban hành. |
| **Không đủ thẩm quyền phê duyệt** | 1 | Bác sĩ thông thường thực hiện kích hoạt ban hành. | Hệ thống trả về HTTP 403 Forbidden: 'Chỉ Giám đốc Lâm sàng (GCMO) mới có thẩm quyền ban hành chính thức Master Template' (F-006). |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR11 (Quy chuẩn Hotline Red Flag 0395 151 151), BR15 (Quy trình phê duyệt GCMO), BR16 (Chuẩn hóa phẫu thuật) | | |

---


### UC-006: Phê Duyệt Lâm Sàng Master Template (Clinical Approval Workflow)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-006** | | |
| **Tên Use Case (Use Case Name)** | Phê Duyệt Lâm Sàng Master Template (Clinical Approval Workflow) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-005 (Clinical Approver / GCMO) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-002 (Bác sĩ điều trị) | | |
| **Tính năng liên quan (Features)** | F-007 | | |
| **Mô tả tóm tắt (Brief Description)** | Giám đốc Chuyên môn Tập đoàn (BS.CKII Trần Bá Kiền) thẩm định các nội dung dược lý, tiêu chí Red Flag và ký duyệt điện tử trước khi ban hành. | | |
| **Mục tiêu (Goal)** | Thẩm định chuyên môn y khoa và ban hành chính thức các phiên bản Master Template áp dụng thống nhất cho toàn chuỗi 5 bệnh viện VISI. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ gửi yêu cầu phê duyệt template sang trạng thái `PENDING_APPROVAL`. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Template đã hoàn tất cấu hình đủ 5 thành phần và ở trạng thái chờ duyệt.<br>2. GCMO đã đăng nhập tài khoản có thẩm quyền phê duyệt. | | |
| **Điều kiện sau (Post-conditions)** | 1. Template chuyển trạng thái `ACTIVE` (đã ban hành) hoặc `REJECTED` (trả về chỉnh sửa).<br>2. Thông báo tự động đến Bác sĩ soạn thảo. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | GCMO mở danh sách Template chờ duyệt (SCR-DOC-09) | Hệ thống hiển thị chi tiết phác đồ: danh mục thuốc, liều, khoảng cách đệm, bảng kiểm phục hồi. |
| | 2 | GCMO thẩm định tính an toàn và chuẩn mực chuyên môn nhãn khoa | GCMO đánh giá các chỉ số dược lý và câu hỏi sàng lọc biến chứng. |
| | 3 | GCMO nhấn 'Ký duyệt & Ban hành' | Hệ thống lưu chữ ký số, chuyển template sang trạng thái `ACTIVE` và kích hoạt áp dụng trên toàn chuỗi (BR19). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Yêu cầu chỉnh sửa** | 1 | GCMO phát hiện liều thuốc hoặc thời gian đệm chưa tối ưu | GCMO nhập ghi chú yêu cầu và nhấn 'Trả về chỉnh sửa'; template chuyển trạng thái `DRAFT` kèm lý do. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Xung đột phiên bản** | 1 | Đã có template cùng loại đang active | Hệ thống nhắc nhở việc ban hành phiên bản mới sẽ tự động lưu trữ (archive) phiên bản cũ. |
| **Mức độ ưu tiên (Priority)** | **P1** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR19 (Vòng đời Master Template: DRAFT -> PENDING_APPROVAL -> ACTIVE -> ARCHIVED) | | |

---

---

---

### NHÓM UC-007: CẤU HÌNH DANH MỤC THUỐC MẪU TRONG TEMPLATE (TEMPLATE MEDICATIONS CRUD)

> **Mô tả nhóm nghiệp vụ:** Quản lý danh mục các loại thuốc điều trị hậu phẫu chuẩn hóa được gán vào Master Template. Bao gồm thiết lập liều lượng, số giọt, lịch cữ uống/nhỏ trong ngày, định danh trực quan qua màu nắp lọ và đặc biệt là cài đặt thời gian đệm giãn cách an toàn 5–10 phút (Drop Interval Buffer Timer — BR23) để chống rửa trôi dược chất nhãn khoa.

#### UC-007.1: Thêm Thuốc Mẫu Vào Master Template & Cài Đặt Timer Đệm (Create Template Medication — POST /api/v1/templates/{id}/medications)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-007.1** | | |
| **Tên Use Case (Use Case Name)** | Thêm Thuốc Mẫu Vào Master Template & Cài Đặt Timer Đệm (Create Template Medication — POST /api/v1/templates/{id}/medications) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-007 (Cấu hình Danh mục Thuốc mẫu), F-014 (Lịch Dùng Thuốc), F-015 (Bộ Đếm Giãn Cách 5-10p) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ chọn hoặc nhập thông tin thuốc (kháng sinh, kháng viêm, nước mắt nhân tạo), đặt số giọt, lịch cữ (Sáng, Trưa, Chiều, Tối), màu nắp nhận diện và thiết lập thời gian đệm 5–10 phút. | | |
| **Mục tiêu (Goal)** | Thêm một loại thuốc mẫu vào phác đồ, cấu hình đầy đủ cữ dùng và khoảng đệm thời gian an toàn giữa các lần nhỏ mắt. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút '+ Thêm Thuốc Mẫu' tại Tab 2 (Danh mục Thuốc mẫu) màn hình SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Master Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi mới được thêm vào `template_medications` gắn với `template_id`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn nút '+ Thêm Thuốc Mẫu' tại SCR-DOC-08 Tab 2. | Hệ thống hiển thị modal cấu hình thuốc mẫu. |
| | 2 | Bác sĩ nhập Tên thuốc (biệt dược & hoạt chất), chọn Dạng bào chế (Thuốc nhỏ mắt dung dịch / hỗn dịch / mỡ tra mắt / viên uống). | Hệ thống tự động kích hoạt trường 'Thời gian đệm giãn cách' nếu dạng bào chế là Thuốc nhỏ mắt. |
| | 3 | Bác sĩ chọn Liều lượng: Số giọt mỗi lần (ví dụ: '1 giọt'), Mắt điều trị (Mắt phẫu thuật / Cả 2 mắt). | Hệ thống hiển thị hình ảnh minh họa giọt thuốc nhãn khoa. |
| | 4 | Bác sĩ cấu hình Lịch dùng trong ngày: Chọn các cữ Sáng (08:00), Trưa (12:00), Chiều (16:00), Tối (20:00). | Hệ thống hiển thị timeline phân bổ các cữ trong ngày trực quan. |
| | 5 | Bác sĩ cấu hình 'Thời gian đệm giãn cách (Buffer Interval)': Chọn 5 phút (mặc định cho dung dịch thông thường) hoặc 10 phút (cho hỗn dịch/gel/mỡ tra mắt). | Hệ thống gán quy tắc đệm BR23 vào cấu hình thuốc. |
| | 6 | Bác sĩ chọn Mã màu nắp lọ hoặc tải ảnh vỏ lọ nhận diện. | Hệ thống preview ảnh lọ thuốc với màu nắp tương ứng (ví dụ: Vàng, Cam, Trắng). |
| | 7 | Bác sĩ bấm 'Lưu Thuốc Mẫu'. | Frontend gửi `POST /api/v1/templates/{id}/medications`, backend validate dữ liệu, lưu vào `template_medications` và trả về HTTP 201 Created. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Chọn từ Danh mục Dược thư Chuẩn VISI** | 1 | Bác sĩ gõ tìm kiếm tên thuốc trong danh mục dược thư đã được Bệnh viện phê duyệt sẵn. | Hệ thống tự động điền sẵn Dạng bào chế, hoạt chất, màu nắp và khuyến nghị thời gian đệm chuẩn. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Thời gian đệm không an toàn lâm sàng** | 1 | Bác sĩ nhập thời gian đệm <5 phút đối với hai loại thuốc nhỏ mắt liên tiếp. | Hệ thống cảnh báo lâm sàng (BR23): 'Khuyến cáo: Thời gian đệm giữa các loại thuốc nhỏ mắt tối thiểu phải là 5 phút để tránh rửa trôi thuốc. Bạn có chắc chắn muốn áp dụng không?'. |
| **Template đã ban hành ACTIVE** | 1 | Cố tình thêm thuốc vào template đã duyệt. | Hệ thống từ chối sửa đổi trực tiếp, yêu cầu nhân bản sang phiên bản mới (v1.1) theo BR15. |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR23 (Thời gian đệm giãn cách nhỏ mắt 5–10 phút), BR15 (Quản lý phiên bản bất biến) | | |

---

#### UC-007.2: Xem Danh Sách Thuốc Mẫu Trong Template (Read/List Template Medications — GET /api/v1/templates/{id}/medications)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-007.2** | | |
| **Tên Use Case (Use Case Name)** | Xem Danh Sách Thuốc Mẫu Trong Template (Read/List Template Medications — GET /api/v1/templates/{id}/medications) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002), Điều dưỡng (ACT-003), GCMO (ACT-001) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-007 (Cấu hình Danh mục Thuốc mẫu) | | |
| **Mô tả tóm tắt (Brief Description)** | Cung cấp bảng tổng hợp danh mục thuốc, thứ tự ưu tiên nhỏ mắt (nhỏ thuốc nước trước, thuốc mỡ sau), liều lượng và thời gian đệm. | | |
| **Mục tiêu (Goal)** | Hiển thị danh sách trực quan toàn bộ các loại thuốc mẫu trong template, phân chia theo 4 khung giờ cữ trong ngày. | | |
| **Tác nhân kích hoạt (Trigger)** | Người dùng chuyển sang Tab 'Danh Mục Thuốc Mẫu' tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template tồn tại trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Hiển thị bảng danh mục thuốc đầy đủ, rõ ràng và trực quan. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Người dùng mở Tab 2 tại SCR-DOC-08. | Frontend gọi `GET /api/v1/templates/{id}/medications`. |
| | 2 | Backend truy vấn bảng `template_medications` sắp xếp theo thứ tự ưu tiên dùng thuốc. | Hệ thống trả về danh sách các loại thuốc. |
| | 3 | Giao diện hiển thị bảng thuốc: | Cột 1: Ảnh nắp lọ & Tên thuốc; Cột 2: Dạng bào chế & Liều dùng; Cột 3: Khung giờ các cữ; Cột 4: Thời gian đệm giãn cách (5–10p); Cột 5: Nút thao tác (Sửa, Xóa). |
| | 4 | Người dùng xem chi tiết thứ tự nhỏ thuốc trong từng khung giờ. | Hệ thống hiển thị timeline tuần tự: Ví dụ lúc 08:00 nhỏ Kháng sinh Lọ 1 -> Đếm lùi 5p -> Nhỏ Kháng viêm Lọ 2. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR23 (Cơ chế đếm lùi đệm thuốc) | | |

---

#### UC-007.3: Chỉnh Sửa Thuốc Mẫu & Thời Gian Đệm (Update Template Medication — PUT /api/v1/templates/{id}/medications/{med_id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-007.3** | | |
| **Tên Use Case (Use Case Name)** | Chỉnh Sửa Thuốc Mẫu & Thời Gian Đệm (Update Template Medication — PUT /api/v1/templates/{id}/medications/{med_id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-007, F-015 | | |
| **Mô tả tóm tắt (Brief Description)** | Cho phép bác sĩ thay đổi thông số thuốc trong template draft để tối ưu hóa phác đồ điều trị nhãn khoa. | | |
| **Mục tiêu (Goal)** | Cập nhật liều lượng, số lần dùng hoặc điều chỉnh thời gian đệm giãn cách của một loại thuốc mẫu. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút biểu tượng 'Chỉnh sửa' (bút chì) trên dòng thuốc mẫu tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Thuốc mẫu tồn tại trong template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi `template_medications` được cập nhật thông tin mới. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm icon 'Sửa' trên một loại thuốc mẫu. | Hệ thống mở modal chỉnh sửa với dữ liệu hiện tại của thuốc được điền sẵn. |
| | 2 | Bác sĩ điều chỉnh số giọt (ví dụ từ 1 giọt lên 2 giọt) hoặc tăng thời gian đệm từ 5 phút lên 10 phút. | Hệ thống kiểm tra tính logic của dữ liệu. |
| | 3 | Bác sĩ bấm 'Lưu Cập Nhật'. | Frontend gửi request `PUT /api/v1/templates/{id}/medications/{med_id}`. |
| | 4 | Backend cập nhật CSDL và trả về HTTP 200 OK. | Giao diện cập nhật lại dòng thuốc tương ứng tức thì. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Template không ở trạng thái DRAFT** | 1 | Template đã ban hành chính thức. | Hệ thống chặn cập nhật trực tiếp, yêu cầu nâng phiên bản template (BR15). |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 (Immutability), BR23 (Quy chuẩn buffer timer 5–10p) | | |

---

#### UC-007.4: Xóa Thuốc Mẫu Khỏi Master Template (Delete Template Medication — DELETE /api/v1/templates/{id}/medications/{med_id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-007.4** | | |
| **Tên Use Case (Use Case Name)** | Xóa Thuốc Mẫu Khỏi Master Template (Delete Template Medication — DELETE /api/v1/templates/{id}/medications/{med_id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-007 | | |
| **Mô tả tóm tắt (Brief Description)** | Xóa bản ghi thuốc khỏi cấu hình template đang xây dựng mà không ảnh hưởng đến bất kỳ bệnh nhân thực tế nào đã xuất viện trước đó. | | |
| **Mục tiêu (Goal)** | Gỡ bỏ một loại thuốc không còn phù hợp khỏi phác đồ mẫu. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm icon 'Xóa' (thùng rác) trên dòng thuốc mẫu tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Thuốc mẫu tồn tại trong template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi thuốc bị xóa khỏi bảng `template_medications`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm icon 'Xóa' trên dòng thuốc cần loại bỏ. | Hệ thống hiển thị hộp thoại xác nhận: 'Bạn có chắc chắn muốn xóa thuốc này khỏi Master Template không?'. |
| | 2 | Bác sĩ bấm 'Xác nhận Xóa'. | Frontend gửi request `DELETE /api/v1/templates/{id}/medications/{med_id}`. |
| | 3 | Backend xóa bản ghi trong CSDL và sắp xếp lại thứ tự ưu tiên các thuốc còn lại. | Hệ thống trả về HTTP 200 OK. |
| | 4 | Frontend gỡ bỏ dòng thuốc khỏi bảng danh mục. | Hiển thị thông báo 'Đã xóa thuốc mẫu thành công'. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Xóa loại thuốc cuối cùng trong template đang chờ duyệt** | 1 | Template chỉ còn duy nhất 1 loại thuốc và bị xóa. | Hệ thống cảnh báo: 'Master Template phải có ít nhất một loại thuốc điều trị để đủ điều kiện gửi duyệt lâm sàng'. |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 (Quản lý phiên bản template), BR18 (Độc lập dữ liệu Care Plan) | | |

---


### NHÓM UC-008: CẤU HÌNH BỘ CÂU HỎI RECOVERY CHECK & RED FLAG TRONG TEMPLATE (RECOVERY & RED FLAGS CRUD)

> **Mô tả nhóm nghiệp vụ:** Cấu hình hệ thống giám sát phục hồi lâm sàng đa tầng cho Master Template. Bao gồm thiết lập các mốc thời gian đánh giá định kỳ (Milestones) kèm bộ câu hỏi 3 mức phân loại màu sắc (Xanh: Bình thường, Vàng: Chú ý, Đỏ: Nguy hiểm — F-008, F-019) và cấu hình các tiêu chí báo động đỏ Red Flag khẩn cấp gắn liền với Hotline VISI 0395 151 151 và thời gian cam kết can thiệp lâm sàng SLA <5 phút (BR11, BR12).

#### UC-008.1: Thêm Mốc Thời Gian & Câu Hỏi Recovery Check (Create Milestone & Questions — POST /api/v1/templates/{id}/milestones)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-008.1** | | |
| **Tên Use Case (Use Case Name)** | Thêm Mốc Thời Gian & Câu Hỏi Recovery Check (Create Milestone & Questions — POST /api/v1/templates/{id}/milestones) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-008 (Cấu hình Recovery Check & Red Flag), F-019 (Khảo sát Phục hồi Định kỳ) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ thêm mốc khảo sát, đặt giờ gửi thông báo nhắc nhở (08:00 sáng) và nhập các câu hỏi trắc nghiệm nhanh 3 mức phân loại màu sắc (Xanh, Vàng, Đỏ). | | |
| **Mục tiêu (Goal)** | Thiết lập một mốc khảo sát phục hồi định kỳ (ví dụ: Sáng Ngày 1, Ngày 3, Ngày 7 - BR24) kèm bộ 3–5 câu hỏi lượng giá lâm sàng. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút '+ Thêm Mốc Khảo Sát' tại Tab 3 màn hình SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Master Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi mới trong `template_recovery_milestones` và các bản ghi con trong `template_recovery_questions`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn '+ Thêm Mốc Khảo Sát' tại Tab 3 SCR-DOC-08. | Hệ thống mở modal 'Cấu Hình Mốc Khảo Sát Recovery Check'. |
| | 2 | Bác sĩ nhập Tên mốc (ví dụ: 'Khảo sát Buổi sáng Ngày 1 sau mổ'), Ngày áp dụng (Day 1 sau phẫu thuật), Giờ gửi thông báo (08:00 sáng). | Hệ thống ghi nhận thông số mốc thời gian. |
| | 3 | Bác sĩ thêm câu hỏi số 1: 'Mắt phẫu thuật sáng nay của bạn có cảm thấy đau buốt không?'. | Hệ thống mở form cấu hình 3 mức trả lời. |
| | 4 | Bác sĩ thiết lập 3 mức phản hồi: | - Mức 1 (Xanh lá): 'Không đau hoặc chỉ hơi cộm nhẹ' -> Bình thường.
- Mức 2 (Vàng): 'Đau âm ỉ nhưng giảm sau khi nghỉ ngơi' -> Bác sĩ/CSKH theo dõi.
- Mức 3 (Đỏ): 'Đau nhức dữ dội lan lên nửa đầu, dùng thuốc giảm đau không đỡ' -> Tự động kích hoạt Red Flag Incident (F-020). |
| | 5 | Bác sĩ thêm tiếp các câu hỏi về Đỏ mắt, Thị lực mờ sương, Tiết dịch ghèn (tổng 3–5 câu hỏi theo chuẩn BR24). | Hệ thống hiển thị danh sách câu hỏi xem trước trực quan. |
| | 6 | Bác sĩ bấm 'Lưu Mốc Khảo Sát'. | Frontend gửi `POST /api/v1/templates/{id}/milestones`, backend lưu CSDL và trả về HTTP 201 Created. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Trùng lặp ngày áp dụng mốc khảo sát** | 1 | Bác sĩ cấu hình 2 mốc khảo sát cùng rơi vào Ngày 1 lúc 08:00. | Hệ thống cảnh báo: 'Đã tồn tại mốc khảo sát cho Ngày 1. Vui lòng chọn ngày khác hoặc chỉnh sửa mốc hiện có'. |
| **Thiếu câu hỏi phân loại mức Đỏ** | 1 | Bộ câu hỏi không có bất kỳ câu trả lời nào định tuyến về mức Đỏ. | Hệ thống khuyến cáo lâm sàng: 'Bộ câu hỏi phải có phương án nhận diện dấu hiệu nguy hiểm (Mức Đỏ) để bảo đảm an toàn cho người bệnh' (BR11). |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR11 (Quy chuẩn kích hoạt Red Flag), BR24 (Bộ tiêu chí khảo sát 3–5 câu hỏi, 3 mức phân loại) | | |

---

#### UC-008.2: Xem Danh Sách Mốc & Câu Hỏi Recovery Check (Read/List Milestones — GET /api/v1/templates/{id}/milestones)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-008.2** | | |
| **Tên Use Case (Use Case Name)** | Xem Danh Sách Mốc & Câu Hỏi Recovery Check (Read/List Milestones — GET /api/v1/templates/{id}/milestones) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002), Điều dưỡng (ACT-003), GCMO (ACT-001) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-008 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ và GCMO xem toàn diện ma trận câu hỏi khảo sát phục hồi để thẩm định tính chuẩn xác y khoa trước khi ban hành. | | |
| **Mục tiêu (Goal)** | Hiển thị toàn bộ các mốc khảo sát và chi tiết từng câu hỏi phân loại 3 màu trong template. | | |
| **Tác nhân kích hoạt (Trigger)** | Người dùng chuyển sang Tab 3 'Recovery Check & Red Flag' tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template tồn tại trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Danh sách các mốc hiển thị dạng Timeline tuần tự theo từng ngày sau mổ. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Người dùng mở Tab 3 SCR-DOC-08. | Frontend gửi request `GET /api/v1/templates/{id}/milestones`. |
| | 2 | Backend truy vấn bảng `template_recovery_milestones` kèm join bảng `template_recovery_questions` sắp xếp theo `day_offset`. | Hệ thống trả về danh sách mốc và câu hỏi. |
| | 3 | Giao diện hiển thị danh sách Accordion các mốc: Ngày 1, Ngày 3, Ngày 7, Ngày 14, Ngày 30. | Mỗi mốc mở rộng hiển thị các câu hỏi thành phần kèm 3 nhãn màu Xanh/Vàng/Đỏ trực quan. |
| | 4 | Người dùng bấm vào từng mốc để xem chi tiết. | Hệ thống hiển thị trạng thái câu hỏi và trọng số cảnh báo. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR24 (Lịch trình theo dõi chuẩn) | | |

---

#### UC-008.3: Chỉnh Sửa Mốc & Câu Hỏi Recovery Check (Update Milestone — PUT /api/v1/templates/{id}/milestones/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-008.3** | | |
| **Tên Use Case (Use Case Name)** | Chỉnh Sửa Mốc & Câu Hỏi Recovery Check (Update Milestone — PUT /api/v1/templates/{id}/milestones/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-008 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ cập nhật câu hỏi trong template nháp để câu văn gần gũi, dễ hiểu hơn với người cao tuổi và thân nhân. | | |
| **Mục tiêu (Goal)** | Điều chỉnh nội dung câu hỏi, đổi ngày áp dụng hoặc định nghĩa lại mức cảnh báo màu trong mốc khảo sát. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút 'Chỉnh sửa' trên mốc khảo sát tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Dữ liệu mốc và câu hỏi được cập nhật trong CSDL. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm 'Sửa' trên mốc Ngày 1. | Hệ thống mở modal với dữ liệu hiện tại. |
| | 2 | Bác sĩ chỉnh sửa văn phong câu hỏi cho rõ ràng hơn. | Hệ thống kiểm tra cú pháp và độ dài chuỗi. |
| | 3 | Bác sĩ bấm 'Lưu Thay Đổi'. | Frontend gửi `PUT /api/v1/templates/{id}/milestones/{id}`, backend cập nhật và trả về HTTP 200 OK. |
| | 4 | Giao diện cập nhật lại nội dung câu hỏi tức thì. | Hiển thị thông báo cập nhật thành công. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Template đã ban hành chính thức** | 1 | Cố tình sửa mốc trong template ACTIVE. | Hệ thống từ chối cập nhật trực tiếp theo BR15. |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 (Quy trình quản lý phiên bản) | | |

---

#### UC-008.4: Xóa Mốc Thời Gian / Câu Hỏi Recovery Check (Delete Milestone — DELETE /api/v1/templates/{id}/milestones/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-008.4** | | |
| **Tên Use Case (Use Case Name)** | Xóa Mốc Thời Gian / Câu Hỏi Recovery Check (Delete Milestone — DELETE /api/v1/templates/{id}/milestones/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-008 | | |
| **Mô tả tóm tắt (Brief Description)** | Xóa bản ghi mốc khảo sát và các câu hỏi liên kết trong bảng `template_recovery_milestones`. | | |
| **Mục tiêu (Goal)** | Gỡ bỏ một mốc khảo sát hoặc một câu hỏi không cần thiết khỏi template draft. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút 'Xóa mốc' tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi mốc khảo sát và các câu hỏi phụ thuộc bị xóa khỏi CSDL. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm icon 'Xóa' trên mốc khảo sát. | Hệ thống yêu cầu xác nhận: 'Bạn có chắc chắn muốn xóa mốc khảo sát này cùng toàn bộ các câu hỏi bên trong?'. |
| | 2 | Bác sĩ bấm 'Xác nhận Xóa'. | Frontend gửi request `DELETE /api/v1/templates/{id}/milestones/{id}`. |
| | 3 | Backend xóa mốc và câu hỏi dạng cascade trong CSDL. | Hệ thống trả về HTTP 200 OK và cập nhật lại danh sách. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 (Immutability) | | |

---

#### UC-008.5: Thêm Tiêu Chí Dấu Hiệu Nguy Hiểm Red Flag & Hotline (Create Red Flag Rule — POST /api/v1/templates/{id}/red-flags)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-008.5** | | |
| **Tên Use Case (Use Case Name)** | Thêm Tiêu Chí Dấu Hiệu Nguy Hiểm Red Flag & Hotline (Create Red Flag Rule — POST /api/v1/templates/{id}/red-flags) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-008, F-020, F-022 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ thiết lập danh mục các triệu chứng báo động đỏ nguy kịch (Đột ngột tối sầm mắt, đau buốt dữ dội, chớp sáng, chảy mủ vết mổ); khi người bệnh có triệu chứng này, hệ thống sẽ kích hoạt báo động khẩn cấp. | | |
| **Mục tiêu (Goal)** | Cài đặt các tiêu chí nhận diện triệu chứng cấp cứu nhãn khoa hậu phẫu gắn với Hotline VISI 0395 151 151 và cơ chế leo thang cảnh báo. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm '+ Thêm Tiêu Chí Red Flag' tại Tab 3 phân khu Red Flag màn hình SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Master Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi mới được tạo trong `template_red_flags` với cờ `emergency_hotline = '0395 151 151'` và `sla_minutes = 5`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn '+ Thêm Tiêu Chí Red Flag' tại SCR-DOC-08. | Hệ thống mở modal 'Cấu Hình Dấu Hiệu Báo Động Đỏ Cấp Cứu'. |
| | 2 | Bác sĩ nhập Tên triệu chứng (ví dụ: 'Đột ngột suy giảm hoặc mất hoàn toàn thị lực'). | Hệ thống ghi nhận mô tả triệu chứng lâm sàng. |
| | 3 | Bác sĩ nhập Hướng dẫn sơ cứu tức thì cho người nhà (ví dụ: 'Tuyệt đối không dụi mắt, không nhỏ thêm bất kỳ loại thuốc nào, giữ nguyên tư thế và gọi ngay Hotline viện'). | Hệ thống hiển thị preview khung giao diện cấp cứu của Caregiver. |
| | 4 | Hệ thống mặc định gắn Số Hotline Cấp cứu: `0395 151 151` (cố định theo BR11) và Thời gian cam kết phản hồi SLA: `5 phút` (BR12). | Bác sĩ xác nhận thông số cấp cứu. |
| | 5 | Bác sĩ bấm 'Lưu Tiêu Chí Red Flag'. | Frontend gửi `POST /api/v1/templates/{id}/red-flags`, backend lưu CSDL và trả về HTTP 201 Created. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Thay đổi số Hotline khác số quy chuẩn** | 1 | Bác sĩ cố tình đổi số điện thoại hotline cấp cứu khác số 0395 151 151. | Hệ thống cảnh báo và từ chối: 'Theo Quy tắc Nghiệp vụ BR11 của VISI Medical Group, số Hotline cấp cứu bắt buộc phải là 0395 151 151'. |
| **Mức độ ưu tiên (Priority)** | **Cao (High - Cốt lõi an toàn tính mạng & thị lực)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR11 (Đường dây nóng cấp cứu Red Flag 0395 151 151), BR12 (Cam kết SLA tiếp nhận <5 phút) | | |

---

#### UC-008.6: Xem Danh Sách Tiêu Chí Red Flag Trong Template (Read/List Red Flags — GET /api/v1/templates/{id}/red-flags)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-008.6** | | |
| **Tên Use Case (Use Case Name)** | Xem Danh Sách Tiêu Chí Red Flag Trong Template (Read/List Red Flags — GET /api/v1/templates/{id}/red-flags) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002), Nhân viên CSKH (ACT-004), GCMO (ACT-001) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-008, F-020 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ và điều phối viên CSKH tra cứu các tiêu chí nguy hiểm để phục vụ đào tạo và trực tổng đài cấp cứu. | | |
| **Mục tiêu (Goal)** | Truy xuất và hiển thị danh mục các tiêu chí báo động đỏ đã cấu hình trong phác đồ mẫu. | | |
| **Tác nhân kích hoạt (Trigger)** | Người dùng xem phân khu Red Flag tại Tab 3 SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template tồn tại trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Bảng danh sách tiêu chí Red Flag hiển thị đầy đủ kèm hướng dẫn sơ cứu tức thì. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Người dùng mở phân khu Red Flag tại Tab 3. | Frontend gọi `GET /api/v1/templates/{id}/red-flags`. |
| | 2 | Backend truy vấn bảng `template_red_flags`. | Hệ thống trả về danh sách các tiêu chí báo động đỏ. |
| | 3 | Giao diện hiển thị bảng viền đỏ nổi bật: | Cột 1: Tên triệu chứng biến chứng; Cột 2: Hướng dẫn sơ cứu người nhà; Cột 3: Hotline cấp cứu (0395 151 151); Cột 4: SLA xử lý (<5 phút); Cột 5: Nút thao tác. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR11, BR12 | | |

---

#### UC-008.7: Chỉnh Sửa Tiêu Chí Red Flag & Hướng Xử Trí Lâm Sàng (Update Red Flag — PUT /api/v1/templates/{id}/red-flags/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-008.7** | | |
| **Tên Use Case (Use Case Name)** | Chỉnh Sửa Tiêu Chí Red Flag & Hướng Xử Trí Lâm Sàng (Update Red Flag — PUT /api/v1/templates/{id}/red-flags/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-008, F-022 | | |
| **Mô tả tóm tắt (Brief Description)** | Cho phép bác sĩ tinh chỉnh nội dung hướng dẫn khẩn cấp trong template draft. | | |
| **Mục tiêu (Goal)** | Cập nhật mô tả triệu chứng hoặc hướng dẫn người nhà xử trí ban đầu trong khi chờ liên lạc với bác sĩ. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút 'Chỉnh sửa' trên tiêu chí Red Flag tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi `template_red_flags` được cập nhật trong CSDL. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm 'Sửa' trên một tiêu chí Red Flag. | Hệ thống mở modal chỉnh sửa. |
| | 2 | Bác sĩ bổ sung thêm hướng dẫn sơ cứu cụ thể. | Hệ thống kiểm tra nội dung. |
| | 3 | Bác sĩ bấm 'Lưu Cập Nhật'. | Frontend gửi `PUT /api/v1/templates/{id}/red-flags/{id}`, backend lưu dữ liệu và trả về HTTP 200 OK. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR11, BR12, BR15 | | |

---

#### UC-008.8: Xóa Tiêu Chí Red Flag Khỏi Template (Delete Red Flag — DELETE /api/v1/templates/{id}/red-flags/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-008.8** | | |
| **Tên Use Case (Use Case Name)** | Xóa Tiêu Chí Red Flag Khỏi Template (Delete Red Flag — DELETE /api/v1/templates/{id}/red-flags/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-008 | | |
| **Mô tả tóm tắt (Brief Description)** | Xóa bản ghi tiêu chí Red Flag khỏi bảng `template_red_flags`. | | |
| **Mục tiêu (Goal)** | Gỡ bỏ một tiêu chí Red Flag bị trùng lặp hoặc không phù hợp khỏi template nháp. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm icon 'Xóa' trên tiêu chí Red Flag tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi bị xóa khỏi CSDL. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm 'Xóa' trên tiêu chí Red Flag. | Hệ thống hiển thị cảnh báo an toàn y khoa: 'Bạn có chắc chắn muốn xóa tiêu chí cảnh báo nguy hiểm này?'. |
| | 2 | Bác sĩ bấm 'Xác nhận Xóa'. | Frontend gửi `DELETE /api/v1/templates/{id}/red-flags/{id}`. |
| | 3 | Backend xóa bản ghi trong CSDL và trả về HTTP 200 OK. | Giao diện cập nhật lại bảng tiêu chí. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Xóa tiêu chí Red Flag duy nhất còn lại** | 1 | Template không còn bất kỳ tiêu chí Red Flag nào sau khi xóa. | Hệ thống cảnh báo: 'Template bắt buộc phải có ít nhất một tiêu chí Red Flag để đủ điều kiện gửi duyệt lâm sàng' (BR11). |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR11 (Bắt buộc có tiêu chí Red Flag), BR15 (Quản lý phiên bản) | | |

---


### NHÓM UC-009: CẤU HÌNH CẨM NANG HƯỚNG DẪN, QUY TẮC SINH HOẠT & FAQ LÂM SÀNG (GUIDELINES, DO/DON'T & FAQ CRUD)

> **Mô tả nhóm nghiệp vụ:** Quản lý toàn diện kho nội dung giáo dục sức khỏe và hướng dẫn thực hành cho thân nhân và người bệnh. Phân rã thành 9 Use Case CRUD chi tiết giúp phân tách độc lập việc quản lý cẩm nang 24h đầu & Infographic tĩnh (F-009, F-016, F-017), quản lý bảng 2 cột màu Nên làm / Cần tránh phân nhóm hoạt động sinh hoạt, và cấu hình ngân hàng hỏi đáp tình huống lâm sàng khẩn cấp (F-024).

#### UC-009.1: Thêm Nội Dung Cẩm Nang 24h & Infographic Bài Học (Create Learning Module — POST /api/v1/templates/{id}/modules)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009.1** | | |
| **Tên Use Case (Use Case Name)** | Thêm Nội Dung Cẩm Nang 24h & Infographic Bài Học (Create Learning Module — POST /api/v1/templates/{id}/modules) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL, CDN lưu trữ media | | |
| **Tính năng liên quan (Features)** | F-009 (Cấu hình Cẩm nang & Hướng dẫn), F-016 (Cẩm nang 24h Đầu), F-017 (Infographic Tĩnh) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ tải lên hình ảnh Infographic giải thích tiến trình hồi phục mắt, nhập nội dung văn bản tóm tắt y khoa súc tích và đặt thứ tự bài học trong lộ trình học viện Caregiver. | | |
| **Mục tiêu (Goal)** | Tạo một bài học Infographic tĩnh hoặc tài liệu hướng dẫn 24 giờ đầu sau phẫu thuật được gán vào lộ trình chăm sóc của Template. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút '+ Thêm Bài Học / Infographic' tại Tab 4 màn hình SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi mới được tạo trong `template_learning_modules` với `template_id`, lưu URL ảnh Infographic tĩnh trên CDN. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn '+ Thêm Bài Học' tại Tab 4 SCR-DOC-08. | Hệ thống mở modal 'Cấu Hình Bài Học Cẩm Nang / Infographic'. |
| | 2 | Bác sĩ nhập Tiêu đề bài học (ví dụ: 'Hướng dẫn 24 giờ đầu sống còn sau phẫu thuật Phaco'). | Hệ thống ghi nhận tiêu đề. |
| | 3 | Bác sĩ đánh dấu checkbox: 'Thuộc Cẩm nang 24h đầu sống còn (Critical 24h Guide)' (F-016). | Hệ thống gán cờ ưu tiên hiển thị ngay trên đầu trang chủ Caregiver (SCR-CG-04). |
| | 4 | Bác sĩ tải lên file ảnh Infographic đồ họa tĩnh (PNG/JPG chuẩn tối ưu dung lượng <500KB) hoặc chọn từ Thư viện Đồ họa VISI. | Hệ thống upload lên CDN an toàn và hiển thị hình ảnh xem trước. |
| | 5 | Bác sĩ nhập Tóm tắt y khoa 3 gạch đầu dòng (Lời dặn cốt lõi). | Hệ thống kiểm tra độ dài và định dạng văn bản. |
| | 6 | Bác sĩ bấm 'Lưu Bài Học'. | Frontend gửi `POST /api/v1/templates/{id}/modules`, backend lưu CSDL và trả về HTTP 201 Created. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **File ảnh vượt quá kích thước cho phép** | 1 | Bác sĩ tải file ảnh dung lượng >5MB. | Hệ thống cảnh báo: 'Kích thước file quá lớn. Vui lòng chọn ảnh <2MB để bảo đảm tốc độ tải trang trên thiết bị di động'. |
| **Thiếu tiêu đề bài học** | 1 | Bỏ trống tiêu đề. | Hệ thống trả về HTTP 422 Unprocessable Entity. |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 (Immutability), BR16 (Đặc thù loại phẫu thuật) | | |

---

#### UC-009.2: Xem Danh Mục Cẩm Nang & Infographic Bài Học (Read/List Modules — GET /api/v1/templates/{id}/modules)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009.2** | | |
| **Tên Use Case (Use Case Name)** | Xem Danh Mục Cẩm Nang & Infographic Bài Học (Read/List Modules — GET /api/v1/templates/{id}/modules) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002), Điều dưỡng (ACT-003), GCMO (ACT-001) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-009, F-017 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ và GCMO xem xét danh mục các bài học, thứ tự hiển thị và nội dung tóm tắt y khoa. | | |
| **Mục tiêu (Goal)** | Hiển thị danh sách toàn bộ các bài học cẩm nang và infographic đã cấu hình trong template. | | |
| **Tác nhân kích hoạt (Trigger)** | Người dùng mở phân khu 'Cẩm nang & Infographic' tại Tab 4 SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template tồn tại trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Danh sách bài học hiển thị trực quan dạng lưới kèm hình ảnh thu nhỏ (thumbnail). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Người dùng truy cập phân khu Cẩm nang tại Tab 4. | Frontend gọi `GET /api/v1/templates/{id}/modules`. |
| | 2 | Backend truy vấn bảng `template_learning_modules` sắp xếp theo `display_order`. | Hệ thống trả về danh sách các bài học. |
| | 3 | Giao diện hiển thị danh sách dạng thẻ card trực quan: | Mỗi card hiển thị ảnh Thumbnail, Tiêu đề bài học, Thẻ đánh dấu 'Cẩm nang 24h', Thứ tự hiển thị, Nút sửa/xóa. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15, BR16 | | |

---

#### UC-009.3: Chỉnh Sửa Nội Dung Cẩm Nang & Infographic Bài Học (Update Module — PUT /api/v1/templates/{id}/modules/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009.3** | | |
| **Tên Use Case (Use Case Name)** | Chỉnh Sửa Nội Dung Cẩm Nang & Infographic Bài Học (Update Module — PUT /api/v1/templates/{id}/modules/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-009, F-017 | | |
| **Mô tả tóm tắt (Brief Description)** | Cho phép bác sĩ cập nhật nội dung bài học trong template draft. | | |
| **Mục tiêu (Goal)** | Cập nhật tiêu đề, thay thế file ảnh Infographic hoặc điều chỉnh lời dặn tóm tắt y khoa của bài học. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm nút 'Sửa' trên thẻ bài học tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi `template_learning_modules` được cập nhật. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm 'Sửa' trên một bài học. | Hệ thống mở modal chỉnh sửa với dữ liệu bài học hiện tại. |
| | 2 | Bác sĩ thay đổi nội dung lời dặn tóm tắt. | Hệ thống hiển thị preview tức thì. |
| | 3 | Bác sĩ bấm 'Lưu Thay Đổi'. | Frontend gửi `PUT /api/v1/templates/{id}/modules/{id}`, backend lưu CSDL và trả về HTTP 200 OK. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 | | |

---

#### UC-009.4: Xóa Bài Học Khỏi Lộ Trình Cẩm Nang (Delete Module — DELETE /api/v1/templates/{id}/modules/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009.4** | | |
| **Tên Use Case (Use Case Name)** | Xóa Bài Học Khỏi Lộ Trình Cẩm Nang (Delete Module — DELETE /api/v1/templates/{id}/modules/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-009 | | |
| **Mô tả tóm tắt (Brief Description)** | Xóa bản ghi bài học khỏi bảng `template_learning_modules`. | | |
| **Mục tiêu (Goal)** | Gỡ bỏ một bài học không còn phù hợp khỏi template nháp. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm icon 'Xóa' trên thẻ bài học. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi bị xóa khỏi CSDL. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm 'Xóa' trên bài học. | Hệ thống hiển thị hộp thoại xác nhận: 'Bạn có chắc chắn muốn xóa bài học này khỏi lộ trình cẩm nang?'. |
| | 2 | Bác sĩ bấm 'Xác nhận Xóa'. | Frontend gửi `DELETE /api/v1/templates/{id}/modules/{id}`. |
| | 3 | Backend xóa bản ghi trong CSDL và sắp xếp lại thứ tự bài học còn lại. | Hệ thống trả về HTTP 200 OK. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 | | |

---

#### UC-009.5: Thêm Quy Tắc Nên Làm / Cần Tránh 2 Cột Màu (Create Do/Don't Item — POST /api/v1/templates/{id}/do-dont)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009.5** | | |
| **Tên Use Case (Use Case Name)** | Thêm Quy Tắc Nên Làm / Cần Tránh 2 Cột Màu (Create Do/Don't Item — POST /api/v1/templates/{id}/do-dont) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-009 (Cấu hình Hướng dẫn), F-016 (Bảng Do/Don't 2 Cột Màu) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ định nghĩa các hành vi sinh hoạt được khuyến khích hoặc bị nghiêm cấm sau mổ mắt, phân loại theo nhóm hoạt động: Vệ sinh cá nhân, Tắm gội, Dinh dưỡng, Vận động, Thiết bị điện tử. | | |
| **Mục tiêu (Goal)** | Thêm một quy tắc sinh hoạt hậu phẫu vào bảng 2 cột màu: Cột Xanh (Nên làm - DO) hoặc Cột Đỏ (Cần tránh - DON'T). | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm '+ Thêm Quy Tắc Do/Don't' tại Tab 4 phân khu Do/Don't màn hình SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi mới được tạo trong `template_do_dont_items` với `item_type IN ('DO', 'DONT')`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn '+ Thêm Quy Tắc' tại phân khu Do/Don't SCR-DOC-08. | Hệ thống mở modal 'Cấu Hình Quy Tắc Sinh Hoạt Hậu Phẫu'. |
| | 2 | Bác sĩ chọn Loại quy tắc: 'Nên Làm (DO - Cột Xanh)' hoặc 'Cần Tránh (DON'T - Cột Đỏ)'. | Giao diện modal tự động đổi theme viền Xanh lá hoặc Đỏ cảnh báo tương ứng. |
| | 3 | Bác sĩ chọn Nhóm sinh hoạt (`category`): Vệ sinh mắt / Tắm gội / Ăn uống / Vận động / Sử dụng màn hình điện tử / Giấc ngủ. | Hệ thống hiển thị icon sinh hoạt minh họa tương ứng. |
| | 4 | Bác sĩ nhập Tiêu đề quy tắc (ví dụ: 'Đeo kính bảo hộ cả khi ngủ trong 3 ngày đầu'). | Hệ thống ghi nhận tiêu đề ngắn gọn. |
| | 5 | Bác sĩ nhập Giải thích lý do y khoa (ví dụ: 'Để tránh vô tình đưa tay lên dụi mắt trong lúc ngủ say gây lệch vạt giác mạc'). | Hệ thống ghi nhận giải thích chuyên môn. |
| | 6 | Bác sĩ bấm 'Lưu Quy Tắc'. | Frontend gửi `POST /api/v1/templates/{id}/do-dont`, backend lưu CSDL và trả về HTTP 201 Created. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Thiếu thông tin bắt buộc** | 1 | Bỏ trống loại quy tắc hoặc nội dung. | Hệ thống trả về HTTP 422 Unprocessable Entity. |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR16 (Quy tắc sinh hoạt đặc thù từng loại phẫu thuật nhãn khoa) | | |

---

#### UC-009.6: Xem Danh Sách Quy Tắc Nên Làm / Cần Tránh (Read/List Do/Don't — GET /api/v1/templates/{id}/do-dont)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009.6** | | |
| **Tên Use Case (Use Case Name)** | Xem Danh Sách Quy Tắc Nên Làm / Cần Tránh (Read/List Do/Don't — GET /api/v1/templates/{id}/do-dont) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002), Điều dưỡng (ACT-003), GCMO (ACT-001) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-009, F-016 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem toàn diện bảng hướng dẫn sinh hoạt để đối soát trước khi ký duyệt ban hành. | | |
| **Mục tiêu (Goal)** | Hiển thị bảng ma trận 2 cột màu chuẩn hóa: Cột Xanh (DO) và Cột Đỏ (DON'T) theo từng nhóm sinh hoạt. | | |
| **Tác nhân kích hoạt (Trigger)** | Người dùng xem phân khu Do/Don't tại Tab 4 SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template tồn tại trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Giao diện hiển thị bảng 2 cột màu trực quan phân nhóm rõ ràng. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Người dùng mở phân khu Do/Don't tại Tab 4. | Frontend gửi request `GET /api/v1/templates/{id}/do-dont`. |
| | 2 | Backend truy vấn bảng `template_do_dont_items` nhóm theo `category` và `item_type`. | Hệ thống trả về danh sách quy tắc. |
| | 3 | Giao diện hiển thị bảng chia 2 cột đối xứng: | Cột Trái (Xanh lá): Các điều Nên Làm (DO); Cột Phải (Đỏ gạch): Các điều Tuyệt Đối Tránh (DON'T). |
| | 4 | Hỗ trợ lọc theo từng nhóm sinh hoạt (Ăn uống, Vệ sinh, Vận động). | Người dùng có thể chuyển đổi nhanh các tab danh mục sinh hoạt. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR16 | | |

---

#### UC-009.7: Chỉnh Sửa Quy Tắc Nên Làm / Cần Tránh (Update Do/Don't Item — PUT /api/v1/templates/{id}/do-dont/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009.7** | | |
| **Tên Use Case (Use Case Name)** | Chỉnh Sửa Quy Tắc Nên Làm / Cần Tránh (Update Do/Don't Item — PUT /api/v1/templates/{id}/do-dont/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-009, F-016 | | |
| **Mô tả tóm tắt (Brief Description)** | Cho phép bác sĩ hiệu chỉnh câu chữ cho dễ hiểu và phù hợp hơn với đối tượng thân nhân người Việt. | | |
| **Mục tiêu (Goal)** | Cập nhật nội dung mô tả hoặc giải thích lý do y khoa của một quy tắc Do/Don't. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm 'Sửa' trên một dòng quy tắc tại SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi `template_do_dont_items` được cập nhật. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm 'Sửa' trên quy tắc sinh hoạt. | Hệ thống mở modal chỉnh sửa. |
| | 2 | Bác sĩ chỉnh sửa nội dung văn bản. | Hệ thống lưu trữ tạm thời. |
| | 3 | Bác sĩ bấm 'Lưu Cập Nhật'. | Frontend gửi `PUT /api/v1/templates/{id}/do-dont/{id}`, backend cập nhật và trả về HTTP 200 OK. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 | | |

---

#### UC-009.8: Xóa Quy Tắc Nên Làm / Cần Tránh Khỏi Template (Delete Do/Don't Item — DELETE /api/v1/templates/{id}/do-dont/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009.8** | | |
| **Tên Use Case (Use Case Name)** | Xóa Quy Tắc Nên Làm / Cần Tránh Khỏi Template (Delete Do/Don't Item — DELETE /api/v1/templates/{id}/do-dont/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-009 | | |
| **Mô tả tóm tắt (Brief Description)** | Xóa bản ghi quy tắc khỏi bảng `template_do_dont_items`. | | |
| **Mục tiêu (Goal)** | Gỡ bỏ một quy tắc sinh hoạt không còn áp dụng khỏi template draft. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ bấm icon 'Xóa' trên dòng quy tắc. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái `DRAFT`. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi bị xóa khỏi CSDL. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm 'Xóa' trên quy tắc sinh hoạt. | Hệ thống yêu cầu xác nhận xóa. |
| | 2 | Bác sĩ bấm 'Xác nhận Xóa'. | Frontend gửi request `DELETE /api/v1/templates/{id}/do-dont/{id}`. |
| | 3 | Backend xóa bản ghi trong CSDL và trả về HTTP 200 OK. | Giao diện cập nhật lại bảng 2 cột màu. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Trung bình (Medium)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 | | |

---

#### UC-009.9: Quản Lý Ngân Hàng Tình Huống Hỏi Đáp FAQ Lâm Sàng (Manage Clinical FAQ Bank — POST/GET/PUT/DELETE /api/v1/templates/{id}/faqs)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009.9** | | |
| **Tên Use Case (Use Case Name)** | Quản Lý Ngân Hàng Tình Huống Hỏi Đáp FAQ Lâm Sàng (Manage Clinical FAQ Bank — POST/GET/PUT/DELETE /api/v1/templates/{id}/faqs) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Bác sĩ chuyên khoa (ACT-002), Nhân viên CSKH (ACT-004) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-009, F-024 (Tra cứu Tình huống Khẩn cấp - FAQ) | | |
| **Mô tả tóm tắt (Brief Description)** | Cung cấp bộ công cụ quản lý thư viện FAQ tình huống giúp thân nhân tra cứu 1 chạm tức thì trên điện thoại khi gặp sự cố tại nhà mà không cần hoảng loạn. | | |
| **Mục tiêu (Goal)** | Tạo lập, cập nhật và gán các câu hỏi thường gặp khẩn cấp (Ví dụ: 'Nước vào mắt', 'Quên nhỏ thuốc', 'Mắt cộm xốn') kèm lời dặn xử lý tức thì chuẩn y khoa VISI. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ hoặc CSKH truy cập phân khu 'FAQ Lâm Sàng Tình Huống' tại Tab 4 SCR-DOC-08. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Người dùng có thẩm quyền nội dung chuyên môn. | | |
| **Điều kiện sau (Post-conditions)** | Dữ liệu FAQ được cập nhật, đồng bộ tức thì lên app Caregiver tại màn hình SCR-CG-07. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn '+ Thêm Câu Hỏi FAQ' tại phân khu FAQ. | Hệ thống mở modal nhập liệu tình huống. |
| | 2 | Bác sĩ nhập Tình huống / Câu hỏi: (ví dụ: 'Vô tình bị nước bắn vào mắt khi tắm rửa thì phải xử trí thế nào?'). | Hệ thống ghi nhận tiêu đề tình huống. |
| | 3 | Bác sĩ chọn Từ khóa gợi nhớ (Tags): 'Dính nước', 'Vệ sinh', 'Tắm gội', 'Cộm mắt'. | Hệ thống tạo chỉ mục tìm kiếm nhanh (F-024). |
| | 4 | Bác sĩ nhập Lời khuyên lâm sàng tức thì chuẩn VISI: '1. Tuyệt đối không dụi mắt; 2. Nhỏ ngay 2 giọt nước mắt nhân tạo để đẩy dị vật; 3. Nếu mắt đỏ kèm đau nhức gọi ngay Hotline 0395 151 151'. | Hệ thống hiển thị preview thẻ FAQ hiển thị cho Caregiver. |
| | 5 | Bác sĩ bấm 'Lưu FAQ'. | Frontend gửi `POST /api/v1/templates/{id}/faqs`, backend lưu dữ liệu và trả về HTTP 201 Created. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Chỉnh sửa hoặc xóa FAQ** | 1 | Người dùng bấm nút sửa/xóa trên từng câu hỏi FAQ. | Frontend gửi request `PUT` hoặc `DELETE` tương ứng để cập nhật dữ liệu. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR11 (Đường dây nóng cấp cứu), BR16 (Đặc thù phẫu thuật nhãn khoa) | | |

---


### UC-010: Khởi Tạo và Cá Nhân Hóa Care Plan Bệnh Nhân (Patient Care Plan Instantiation)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-010** | | |
| **Tên Use Case (Use Case Name)** | Khởi Tạo và Cá Nhân Hóa Care Plan Bệnh Nhân (Patient Care Plan Instantiation) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-003 (Điều dưỡng xuất viện) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-002 (Bác sĩ điều trị) | | |
| **Tính năng liên quan (Features)** | F-008 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ hoặc Điều dưỡng tại quầy lưu viện chọn hồ sơ bệnh nhân và áp dụng Master Template tương ứng với loại mổ để sinh ra Care Plan độc lập. Bác sĩ có thể sao chép template để tùy biến liều lượng theo thể trạng bệnh nhân; Điều dưỡng chỉ nhập thông tin cơ bản và không được chỉnh sửa liều lượng thuốc. | | |
| **Mục tiêu (Goal)** | Nhân bản Master Template thành Care Plan thực tế cho bệnh nhân trong <30 giây; cho phép Bác sĩ sao chép và điều chỉnh liều lượng thuốc tùy theo thể trạng bệnh nhân; Điều dưỡng chỉ được nhập thông tin cơ bản và áp dụng mẫu phác đồ đã duyệt. | | |
| **Tác nhân kích hoạt (Trigger)** | Điều dưỡng mở màn hình Khởi tạo Care Plan (SCR-DOC-10) sau khi bệnh nhân hoàn thành ca mổ. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Hồ sơ bệnh nhân đã được tạo (UC-004).<br>2. Có Master Template đang ở trạng thái ACTIVE tương ứng với loại mổ. | | |
| **Điều kiện sau (Post-conditions)** | 1. Bản ghi `patient_care_plans` được tạo ở trạng thái `ACTIVE`.<br>2. Nhân bản dữ liệu thuốc vào `patient_medication_schedules`.<br>3. Chuyển tiếp sang bước in phiếu QR (UC-012). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Điều dưỡng chọn bệnh nhân từ danh sách mổ trong ngày | Hệ thống tải thông tin loại phẫu thuật và mắt can thiệp. |
| | 2 | Điều dưỡng chọn Master Template phù hợp (ví dụ: 'Phaco Tiêu Chuẩn v1.0') | Hệ thống tự động nạp cấu hình thuốc, lịch tái khám và câu hỏi phục hồi. |
| | 3 | Điều dưỡng kiểm tra thông tin và nhấn nút 'Kích hoạt Care Plan & Sinh QR' | Hệ thống nhân bản toàn bộ phác đồ, sinh mã token QR (UC-011), chuyển Care Plan sang trạng thái `ACTIVE` trong <1 giây (BR18). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Bác sĩ tùy biến liều thuốc cá nhân hóa** | 1 | Bác sĩ phẫu thuật điều chỉnh tăng/giảm liều lượng thuốc trước khi kích hoạt | Hệ thống cập nhật riêng cho bệnh nhân mà không làm biến đổi Master Template gốc (BR18). |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Bệnh nhân đã có Care Plan đang hoạt động** | 1 | Hồ sơ bệnh nhân đã được kích hoạt Care Plan trước đó | Hệ thống hiển thị tùy chọn 'Xem Care Plan hiện tại' hoặc 'Thay thế Care Plan mới'. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR18 (Tính độc lập dữ liệu Care Plan cá nhân hóa) | | |

---

---

---

### UC-010b: Thực Hiện Bàn Giao Xuất Viện Tại Phòng Lưu Viện (Discharge Clinical Handoff)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-010b** | | |
| **Tên Use Case (Use Case Name)** | Thực Hiện Bàn Giao Xuất Viện Tại Phòng Lưu Viện (Discharge Clinical Handoff) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-003 (Điều dưỡng xuất viện) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-001 (Caregiver), ACT-006 (Bệnh nhân) | | |
| **Tính năng liên quan (Features)** | F-008, F-022 | | |
| **Mô tả tóm tắt (Brief Description)** | Ca mở rộng vận hành mô tả các bước tương tác lâm sàng trực tiếp giữa Điều dưỡng và Caregiver tại phòng lưu viện nhằm bảo đảm an toàn trước khi về nhà. | | |
| **Mục tiêu (Goal)** | Điều dưỡng trực tiếp kiểm tra mắt, dán khiên bảo hộ, trao phiếu xuất viện kèm mã QR và hướng dẫn người nhà quét mã trước khi rời viện. | | |
| **Tác nhân kích hoạt (Trigger)** | Bệnh nhân hoàn thành thời gian theo dõi hậu phẫu 30–60 phút tại phòng lưu viện và đủ điều kiện xuất viện. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Care Plan đã được kích hoạt và Phiếu xuất viện kèm QR đã in xong.<br>2. Người nhà (Caregiver) có mặt tại quầy lưu viện. | | |
| **Điều kiện sau (Post-conditions)** | 1. Bệnh nhân được trang bị khiên mắt bảo hộ an toàn.<br>2. Người nhà nắm rõ cách truy cập ứng dụng RemiCare. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Điều dưỡng kiểm tra vết mổ, dán khiên bảo vệ mắt hoặc kính bảo hộ cho bệnh nhân | Bảo đảm mắt mổ được bảo vệ cơ học tuyệt đối. |
| | 2 | Điều dưỡng trao Phiếu xuất viện có in mã QR sắc nét cho Caregiver | Giải thích rõ: mã QR này chứa toàn bộ lịch uống thuốc, hướng dẫn chăm sóc và nút gọi cấp cứu. |
| | 3 | Điều dưỡng hướng dẫn Caregiver mở camera quét mã QR và đăng nhập số điện thoại | Caregiver thực hiện quét mã và xác nhận hồ sơ thành công. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Người nhà không mang smartphone** | 1 | Caregiver sử dụng điện thoại cơ bản | Điều dưỡng hướng dẫn dặn dò trên phiếu in giấy truyền thống và ghi chú vào hệ thống. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Camera người nhà không quét được mã** | 1 | Thiết bị cũ hoặc camera mờ | Điều dưỡng hướng dẫn nhập thủ công đường link rút gọn in trên phiếu: `remicare.visi.vn/p/{token}`. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR4 (Xác thực mã QR), BR7 (Dặn dò lâm sàng chuẩn y khoa) | | |

---

---

---

### UC-010c: Xác Nhận Hoàn Tất Bàn Giao Lâm Sàng (Clinical Handoff Confirmation)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-010c** | | |
| **Tên Use Case (Use Case Name)** | Xác Nhận Hoàn Tất Bàn Giao Lâm Sàng (Clinical Handoff Confirmation) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-003 (Điều dưỡng xuất viện) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-001 (Caregiver) | | |
| **Tính năng liên quan (Features)** | F-004, F-008 | | |
| **Mô tả tóm tắt (Brief Description)** | Ca mở rộng ghi nhận mốc kết thúc quy trình xuất viện tại bệnh trạm; chuyển giao hoàn toàn trách nhiệm theo dõi từ phòng lưu viện sang Dashboard giám sát từ xa của CSKH. | | |
| **Mục tiêu (Goal)** | Hệ thống ghi nhận trạng thái Care Plan chuyển sang `ACTIVE` toàn diện và Caregiver đã liên kết thành công, đánh dấu hoàn thành xuất viện. | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver quét mã QR và xác nhận liên kết hồ sơ bệnh nhân thành công trên điện thoại. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Caregiver đã quét QR và đăng nhập OTP thành công.<br>2. Care Plan đã được gán mã định danh Caregiver. | | |
| **Điều kiện sau (Post-conditions)** | 1. Trạng thái bàn giao chuyển sang `HANDOFF_COMPLETED`.<br>2. Kích hoạt lịch trình thông báo tự động (F-027).<br>3. Bệnh nhân xuất hiện trên Dashboard CSKH (F-018). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Hệ thống nhận tín hiệu liên kết thành công từ ứng dụng Caregiver | Cập nhật bản ghi `caregiver_patient_links` với cờ `handoff_verified = TRUE`. |
| | 2 | Màn hình trạm điều dưỡng tự động cập nhật biểu tượng xanh 'Đã bàn giao thành công' | Điều dưỡng hoàn tất thủ tục xuất viện cho ca bệnh trong <30 giây. |
| | 3 | Hệ thống bắt đầu kích hoạt bộ hẹn giờ gửi thông báo nhắc cữ thuốc và lịch tái khám | Dữ liệu được chuyển tiếp sang hàng đợi giám sát của CSKH chi nhánh. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Quá 2 giờ chưa thấy quét mã QR** | 1 | Bệnh nhân đã rời viện nhưng người nhà chưa quét mã | Hệ thống đánh dấu cờ vàng trên Dashboard CSKH để nhân viên chủ động gọi điện nhắc nhở. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR5 (Tính toàn vẹn liên kết bàn giao) | | |

---

---

---

### UC-011: Tạo và Phát Hành Mã QR Xuất Viện (QR Lifecycle Generation)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-011** | | |
| **Tên Use Case (Use Case Name)** | Tạo và Phát Hành Mã QR Xuất Viện (QR Lifecycle Generation) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-003 (Điều dưỡng xuất viện) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-001 (Caregiver), ACT-006 (Bệnh nhân) | | |
| **Tính năng liên quan (Features)** | F-021 | | |
| **Mô tả tóm tắt (Brief Description)** | Tự động tạo mã QR bảo mật cao, không lộ thông tin y tế thô ra ngoài, quản lý trạng thái: Created -> Issued -> Active -> Revoked/Expired. | | |
| **Mục tiêu (Goal)** | Hệ thống sinh mã token mã hóa ngẫu nhiên an toàn (UUIDv4/JWT có chữ ký số) gắn với Care Plan đã ban hành, quản lý trạng thái vòng đời QR. | | |
| **Tác nhân kích hoạt (Trigger)** | Điều dưỡng nhấn nút kích hoạt Care Plan cho bệnh nhân. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Care Plan đã được thiết lập đầy đủ thông tin. | | |
| **Điều kiện sau (Post-conditions)** | Chuỗi token QR bảo mật được lưu trữ và gán với `plan_id`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Hệ thống tiếp nhận lệnh kích hoạt Care Plan | Sinh chuỗi token bảo mật ngẫu nhiên mã hóa HMAC-SHA256 kết hợp UUIDv4. |
| | 2 | Hệ thống tạo bản ghi mã QR với trạng thái `ISSUED` | Tạo file ảnh mã QR vector (SVG/PNG 300 DPI) sẵn sàng cho lệnh in phiếu xuất viện. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Lỗi sinh token mã hóa** | 1 | Lỗi dịch vụ tạo mã | Hệ thống thử lại tự động và thông báo nếu có sự cố máy chủ. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR4 (Tính hợp lệ và bảo mật mã QR) | | |

---

---

---

### UC-012: In Phiếu Hướng Dẫn Xuất Viện Kèm Mã QR (Discharge Slip Print)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-012** | | |
| **Tên Use Case (Use Case Name)** | In Phiếu Hướng Dẫn Xuất Viện Kèm Mã QR (Discharge Slip Print) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-003 (Điều dưỡng xuất viện) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-001 (Caregiver) | | |
| **Tính năng liên quan (Features)** | F-022 | | |
| **Mô tả tóm tắt (Brief Description)** | Điều dưỡng xuất lệnh in phiếu xuất viện chứa logo VISI, tóm tắt lâm sàng, mã QR lớn và số hotline cấp cứu 0395 151 151. | | |
| **Mục tiêu (Goal)** | Xuất lệnh in trực tiếp Phiếu xuất viện khổ chuẩn (A5/A4/decal) có chứa mã QR sắc nét để bàn giao tận tay người nhà trong <3 giây. | | |
| **Tác nhân kích hoạt (Trigger)** | Điều dưỡng nhấn nút 'In Phiếu Xuất Viện' trên màn hình SCR-DOC-11. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Mã QR đã được sinh thành công (UC-011) và máy in tại quầy đã sẵn sàng. | | |
| **Điều kiện sau (Post-conditions)** | Phiếu xuất viện vật lý được in ra hoàn chỉnh để trao cho Caregiver. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Điều dưỡng nhấn 'In Phiếu Xuất Viện' (SCR-DOC-11) | Hệ thống mở hộp thoại in với định dạng tối ưu khổ A5 ngang hoặc A4. |
| | 2 | Điều dưỡng kiểm tra bản in mẫu và nhấn 'In ngay' | Máy in tại quầy in ra phiếu xuất viện sắc nét chứa mã QR ≥3x3 cm, thông tin bệnh nhân viết tắt, hướng dẫn quét và Hotline 0395 151 151 trong <3 giây. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: In thêm tem decal dán sổ khám bệnh** | 1 | Điều dưỡng chọn in tem decal QR | Hệ thống xuất lệnh in ra máy in tem dán trực tiếp lên sổ khám bệnh của bệnh nhân. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Máy in mất kết nối hoặc kẹt giấy** | 1 | Lỗi phần cứng máy in | Hệ thống hiển thị nút 'In lại' và cho phép tải file PDF dự phòng. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR4 (Quy chuẩn kích thước và độ phân giải in mã QR) | | |

---

---

---

### UC-013: Cấp Lại hoặc Thu Hồi Mã QR Bàn Giao (QR Reissue & Revoke)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-013** | | |
| **Tên Use Case (Use Case Name)** | Cấp Lại hoặc Thu Hồi Mã QR Bàn Giao (QR Reissue & Revoke) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-003 (Điều dưỡng), ACT-002 (Bác sĩ) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-001 (Caregiver) | | |
| **Tính năng liên quan (Features)** | F-021, F-022 | | |
| **Mô tả tóm tắt (Brief Description)** | Cho phép Điều dưỡng cấp lại mã QR mới, chuyển mã cũ sang trạng thái `REVOKED`, bảo đảm bệnh nhân luôn tuân thủ đúng đơn thuốc mới nhất. | | |
| **Mục tiêu (Goal)** | Tạo mã QR mới thay thế khi bị mất phiếu hoặc khi Bác sĩ đổi phác đồ thuốc; tự động vô hiệu hóa mã cũ để ngăn ngừa sai sót. | | |
| **Tác nhân kích hoạt (Trigger)** | Người nhà báo mất phiếu hoặc Bác sĩ điều chỉnh đơn thuốc hậu phẫu. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Hồ sơ bệnh nhân đang có mã QR đang hoạt động. | | |
| **Điều kiện sau (Post-conditions)** | 1. Mã QR cũ bị vô hiệu hóa (`REVOKED`).<br>2. Mã QR mới được sinh và in ra phiếu mới.<br>3. Caregiver đã liên kết nhận thông báo tự động. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Điều dưỡng chọn hồ sơ bệnh nhân và nhấn 'Cấp lại mã QR' (SCR-DOC-11) | Hệ thống yêu cầu nhập lý do cấp lại (Mất phiếu / Đổi đơn thuốc / Lý do khác). |
| | 2 | Điều dưỡng xác nhận thao tác | Hệ thống chuyển mã QR cũ sang `REVOKED`, sinh mã QR mới (UC-011), in phiếu mới (UC-012) và ghi nhật ký kiểm toán (F-024). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Quét lại mã cũ đã thu hồi** | 1 | Caregiver quét nhầm phiếu cũ bị hủy | Hệ thống từ chối truy cập và báo lỗi mã đã bị thu hồi do cấp mới (BR4). |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR4 (Tính duy nhất và thu hồi mã QR) | | |

---

---

---

### UC-014: Xem Lịch Dùng Thuốc và Hướng Dẫn Nhỏ Mắt (Medication Schedule & Guide)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-014** | | |
| **Tên Use Case (Use Case Name)** | Xem Lịch Dùng Thuốc và Hướng Dẫn Nhỏ Mắt (Medication Schedule & Guide) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-006 (Bệnh nhân) | | |
| **Tính năng liên quan (Features)** | F-009, F-011 | | |
| **Mô tả tóm tắt (Brief Description)** | Caregiver tra cứu lịch thuốc hàng ngày, nhận diện chính xác từng lọ thuốc và xem video hướng dẫn kỹ thuật kéo mi dưới không chạm đầu lọ. | | |
| **Mục tiêu (Goal)** | Hiển thị cữ thuốc trong ngày theo dòng thời gian (Sáng, Trưa, Chiều, Tối), mắt áp dụng, số giọt, ảnh nhận diện và video hướng dẫn tra thuốc. | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver mở tab 'Lịch Thuốc' trên ứng dụng di động (SCR-CG-09). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Caregiver đã liên kết với Care Plan đang hoạt động của bệnh nhân. | | |
| **Điều kiện sau (Post-conditions)** | Danh sách thuốc được hiển thị rõ ràng, trực quan. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver nhấn vào tab 'Lịch Thuốc' (SCR-CG-09) | Hệ thống hiển thị danh sách các cữ thuốc trong ngày phân theo Sáng (07:00), Trưa (11:30), Chiều (16:30), Tối (20:00). |
| | 2 | Caregiver chạm vào một lọ thuốc để xem chi tiết | Hiển thị ảnh nhận diện vỏ lọ, số giọt cần nhỏ (1 giọt), mắt chỉ định (MP/MT), lưu ý lắc đều hỗn dịch. |
| | 3 | Caregiver bấm 'Xem kỹ thuật nhỏ mắt' | Hệ thống hiển thị video ngắn (15–30s) minh họa thao tác kéo mi dưới vô trùng (F-011, BR7). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Mạng yếu không tải được video** | 1 | Lỗi tải phương tiện truyền thông | Hệ thống tự động hiển thị ảnh tĩnh và hướng dẫn dạng chữ dự phòng. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR7 (Nguồn gốc nội dung y khoa), BR20 (Phân định nhận diện thuốc trực quan) | | |

---

---

---

### UC-015: Xác Nhận Dùng Thuốc và Kích Hoạt Bộ Đếm Giãn Cách (Medication Confirmation & Buffer Timer)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-015** | | |
| **Tên Use Case (Use Case Name)** | Xác Nhận Dùng Thuốc và Kích Hoạt Bộ Đếm Giãn Cách (Medication Confirmation & Buffer Timer) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-006 (Bệnh nhân) | | |
| **Tính năng liên quan (Features)** | F-009, F-010 | | |
| **Mô tả tóm tắt (Brief Description)** | Sau khi nhỏ lọ thứ nhất, Caregiver bấm xác nhận. Hệ thống tự động kích hoạt đồng hồ đếm ngược 10 phút (hoặc 5 phút), tạm khóa lọ thứ hai và báo chuông/rung khi hết giờ. | | |
| **Mục tiêu (Goal)** | Ghi nhận cữ thuốc hoàn thành kèm timestamp đồng bộ; tự động đếm lùi 5–10 phút giữa 2 loại thuốc nhỏ mắt để chống rửa trôi thuốc. | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver bấm nút 'Xác nhận đã nhỏ thuốc' trên thẻ thuốc. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Cữ thuốc đang mở và chưa được xác nhận. | | |
| **Điều kiện sau (Post-conditions)** | 1. Ghi nhận bản ghi dùng thuốc vào `patient_medication_logs`.<br>2. Kích hoạt bộ đếm thời gian giãn cách đệm (F-010).<br>3. Đồng bộ lên thiết bị của các Caregiver khác và Dashboard viện. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver bấm 'Xác nhận đã nhỏ thuốc' cho lọ thuốc thứ nhất | Hệ thống lưu timestamp, chuyển trạng thái thuốc sang `TAKEN` kèm tích xanh. |
| | 2 | Nếu cữ thuốc có từ 2 lọ trở lên, hệ thống tự động kích hoạt Bộ đếm giãn cách 10 phút (hoặc 5 phút theo cấu hình) | Đồng hồ đếm lùi hiển thị hoạt ảnh trực quan; tạm khóa (disable) nút xác nhận lọ thứ hai kèm cảnh báo chống rửa trôi thuốc (BR23). |
| | 3 | Khi đồng hồ đếm lùi về 00:00 | Hệ thống phát âm thanh chuông dịu và rung 3 nhịp, mở khóa nút xác nhận lọ thuốc thứ hai và thông báo đã sẵn sàng nhỏ tiếp (US-018). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Chỉ có 1 loại thuốc trong cữ** | 1 | Cữ dùng chỉ kê 1 loại thuốc đơn lẻ | Hệ thống đánh dấu hoàn tất cữ thuốc ngay mà không cần kích hoạt timer giãn cách. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Người nhà khác đã bấm xác nhận trước đó** | 1 | Caregiver đồng hành đã nhỏ thuốc | Hệ thống hiển thị thông báo cữ thuốc đã được [Tên Caregiver] xác nhận lúc [Thời gian], tránh nhỏ trùng liều. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR21 (Ghi nhận nhật ký tuân thủ thuốc), BR23 (Ràng buộc bộ đếm thời gian giãn cách đệm) | | |

---

---

---

### UC-016: Xem Cẩm Nang 24h Đầu và Bảng Nên Làm / Cần Tránh (Critical 24h Guide & Do/Don't)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-016** | | |
| **Tên Use Case (Use Case Name)** | Xem Cẩm Nang 24h Đầu và Bảng Nên Làm / Cần Tránh (Critical 24h Guide & Do/Don't) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-006 (Bệnh nhân) | | |
| **Tính năng liên quan (Features)** | F-012, F-013 | | |
| **Mô tả tóm tắt (Brief Description)** | Caregiver xem cẩm nang sống còn 24h đầu để bảo vệ mép mổ và tra cứu bảng 2 cột màu sinh động hướng dẫn kiêng nước, không dụi mắt, tư thế ngủ. | | |
| **Mục tiêu (Goal)** | Tra cứu tức thì các hành động cấp thiết trong 24h đầu sau mổ và danh mục sinh hoạt được phép (Nên làm - Xanh) / kiêng cữ (Cần tránh - Đỏ). | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver chọn mục 'Cẩm nang 24h' hoặc 'Nên làm & Cần tránh' (SCR-CG-08). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bệnh nhân đã liên kết Care Plan. | | |
| **Điều kiện sau (Post-conditions)** | Hiển thị đầy đủ hướng dẫn sinh hoạt an toàn. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver mở mục Cẩm nang chăm sóc (SCR-CG-08) | Hệ thống hiển thị Cẩm nang 24h đầu: đeo kính bảo hộ, tư thế nằm ngửa/nghiêng mắt lành, dán khiên mắt khi ngủ, kiêng cúi đầu. |
| | 2 | Caregiver chuyển sang tab 'Nên làm & Cần tránh' | Hệ thống hiển thị 2 cột màu trực quan: Cột Xanh (Nên làm) và Cột Đỏ (Tuyệt đối tránh) kèm icon và giải thích y khoa (BR22). |
| | 3 | Caregiver chọn bộ lọc chủ đề sinh hoạt | Hệ thống lọc danh mục theo Vệ sinh cá nhân, Vận động, Ăn uống, Giấc ngủ. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR7 (Chuẩn y khoa VISI), BR22 (Hiển thị trực quan phân biệt 2 cột màu Do & Don't) | | |

---

---

---

### UC-017: Xem Lộ Trình Học Viện Caregiver - Infographic Tĩnh (Caregiver Micro-Academy)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-017** | | |
| **Tên Use Case (Use Case Name)** | Xem Lộ Trình Học Viện Caregiver - Infographic Tĩnh (Caregiver Micro-Academy) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-006 (Bệnh nhân) | | |
| **Tính năng liên quan (Features)** | F-014 | | |
| **Mô tả tóm tắt (Brief Description)** | Cung cấp thư viện đồ họa thông tin (Infographics) giải thích sinh lý phục hồi mắt mổ, giúp người nhà an tâm đồng hành. | | |
| **Mục tiêu (Goal)** | Học tập kiến thức chăm sóc mắt qua các infographic trực quan tinh gọn theo tiến trình hồi phục; không bắt buộc làm bài kiểm tra quiz trong MVP. | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver chọn mục 'Học Viện Caregiver' (SCR-CG-05, SCR-CG-06). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Caregiver đã đăng nhập ứng dụng. | | |
| **Điều kiện sau (Post-conditions)** | Tiến trình xem bài học được ghi nhận. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver mở danh mục bài học Học Viện Caregiver (SCR-CG-05) | Hệ thống hiển thị danh sách các chủ đề kiến thức theo giai đoạn phục hồi (Day 1, Tuần 1, Tháng 1). |
| | 2 | Caregiver chọn một chủ đề cần xem | Hệ thống hiển thị Infographic đồ họa tĩnh trực quan kèm văn bản tóm tắt tinh gọn. |
| | 3 | Caregiver xem xong và nhấn 'Đã hiểu' | Hệ thống đánh dấu bài học hoàn thành mà không yêu cầu làm quiz trắc nghiệm (F-014). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P1** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR6 (Phạm vi nội dung được cấp quyền), BR7 (Nội dung chuẩn y khoa) | | |

---

---

---

### UC-018: Xem Lịch Tái Khám và Nhận Thông Báo Nhắc Hẹn (Follow-Up Tracker & Notifications)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-018** | | |
| **Tên Use Case (Use Case Name)** | Xem Lịch Tái Khám và Nhận Thông Báo Nhắc Hẹn (Follow-Up Tracker & Notifications) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-009 (SMS / ZNS Gateway), ACT-004 (CSKH) | | |
| **Tính năng liên quan (Features)** | F-015, F-027 | | |
| **Mô tả tóm tắt (Brief Description)** | Hệ thống hiển thị lộ trình tái khám, gửi thông báo nhắc hẹn trước 24h qua Push/SMS/ZNS và hỗ trợ Caregiver xác nhận hẹn khám. | | |
| **Mục tiêu (Goal)** | Theo dõi 5 mốc tái khám chuẩn VISI (Day 1, 7, Month 1, 3, 6) và nhận thông báo nhắc lịch tự động trước 24 giờ. | | |
| **Tác nhân kích hoạt (Trigger)** | Hệ thống tự động kích hoạt thông báo trước 24h hoặc Caregiver chủ động mở tab 'Lịch Tái Khám' (SCR-CG-10). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Care Plan đã tạo lịch tái khám theo 5 mốc chuẩn VISI. | | |
| **Điều kiện sau (Post-conditions)** | Caregiver nắm rõ ngày giờ, địa chỉ chi nhánh khám và xác nhận cuộc hẹn. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver mở tab 'Lịch Tái Khám' (SCR-CG-10) | Hiển thị 5 mốc chuẩn: Ngày 1, Ngày 7, Tháng 1, Tháng 3, Tháng 6 kèm địa chỉ cơ sở VISI đã mổ. |
| | 2 | Trước ngày khám 24 giờ, hệ thống gửi thông báo nhắc lịch tự động qua Push/SMS/Zalo (BR24) | Caregiver nhận thông báo kèm nút bấm xác nhận hẹn. |
| | 3 | Caregiver nhấn 'Xác nhận sẽ đến khám' | Hệ thống cập nhật trạng thái hẹn `CONFIRMED` lên Dashboard viện. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Caregiver yêu cầu đổi giờ khám** | 1 | Caregiver bấm 'Yêu cầu hỗ trợ đổi giờ' | Thông tin chuyển về Dashboard CSKH (UC-021) để nhân viên liên hệ sắp xếp. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR24 (Thời gian kích hoạt thông báo nhắc hẹn trước 24 giờ) | | |

---

---

---

### UC-019: Thực Hiện Khảo Sát Đánh Giá Phục Hồi Định Kỳ (Submit Recovery Check Survey)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-019** | | |
| **Tên Use Case (Use Case Name)** | Thực Hiện Khảo Sát Đánh Giá Phục Hồi Định Kỳ (Submit Recovery Check Survey) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-006 (Bệnh nhân), ACT-002 (Bác sĩ) | | |
| **Tính năng liên quan (Features)** | F-016 | | |
| **Mô tả tóm tắt (Brief Description)** | Caregiver nộp bài kiểm tra ngắn về tình trạng mắt mổ (đau, mờ, đỏ, chảy dịch). Hệ thống phân loại nguy cơ tức thì. | | |
| **Mục tiêu (Goal)** | Trả lời 3–5 câu hỏi sàng lọc định kỳ mỗi sáng trong 7 ngày đầu để hệ thống tự động phân loại 3 mức: Xanh (Bình thường), Vàng (Chú ý), Đỏ (Nguy hiểm). | | |
| **Tác nhân kích hoạt (Trigger)** | Hệ thống thông báo lúc 08:00 sáng hoặc Caregiver mở màn hình Recovery Check (SCR-CG-11). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bệnh nhân đang trong mốc theo dõi khảo sát. | | |
| **Điều kiện sau (Post-conditions)** | 1. Câu trả lời được lưu vào `recovery_check_submissions`.<br>2. Phân loại trạng thái Xanh/Vàng/Đỏ.<br>3. Điều hướng an tâm hoặc kích hoạt cấp cứu Red Flag. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver mở bảng kiểm Recovery Check (SCR-CG-11) | Hệ thống hiển thị 3–5 câu hỏi trắc nghiệm phù hợp với ngày hồi phục của bệnh nhân (BR9). |
| | 2 | Caregiver chọn câu trả lời cho từng câu hỏi và nhấn 'Gửi đánh giá' | Hệ thống kiểm tra đã trả lời đủ câu; đối chiếu với ma trận tiêu chí phân loại lâm sàng (BR10). |
| | 3 | Nếu kết quả bình thường (Mức Xanh) | Hệ thống hiển thị thông điệp phản hồi an tâm (US-025) và cập nhật tiến trình ổn định lên Dashboard. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Kết quả ở Mức Vàng (Cần chú ý)** | 1 | Phát hiện triệu chứng nhẹ (cộm xốn, mắt hơi đỏ) | Hệ thống đưa ra hướng dẫn theo dõi đặc thù và đánh dấu cờ vàng trên Dashboard để CSKH lưu ý. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Phát hiện dấu hiệu Mức Đỏ (Red Flag)** | 1 | Xuất hiện triệu chứng nguy hiểm (đau dữ dội, mờ đột ngột) | Hệ thống lập tức kích hoạt luồng Cảnh báo Đỏ khẩn cấp (UC-020). |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR9 (Bắt buộc trả lời đủ câu hỏi), BR10 (Phân loại 3 mức Xanh/Vàng/Đỏ) | | |

---

---

---

### UC-020: Kích Hoạt Xử Lý Biến Chứng Báo Động Đỏ (Red Flag Emergency Action)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-020** | | |
| **Tên Use Case (Use Case Name)** | Kích Hoạt Xử Lý Biến Chứng Báo Động Đỏ (Red Flag Emergency Action) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-004 (CSKH), ACT-008 (Cấp cứu ngoại viện) | | |
| **Tính năng liên quan (Features)** | F-017 | | |
| **Mô tả tóm tắt (Brief Description)** | Khi phát hiện dấu hiệu nguy cấp từ Recovery Check hoặc Caregiver bấm nút khẩn cấp, hệ thống hướng dẫn sơ cứu tức thì và hỗ trợ gọi cấp cứu trong khung giờ vàng. | | |
| **Mục tiêu (Goal)** | Chuyển giao diện khẩn cấp toàn màn hình, cung cấp nút gọi 1 chạm đến Hotline VISI 0395 151 151 và đẩy tín hiệu báo động khẩn lên Dashboard viện. | | |
| **Tác nhân kích hoạt (Trigger)** | Recovery Check phát hiện triệu chứng nguy hiểm hoặc Caregiver nhấn 'Báo động đỏ khẩn cấp'. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Caregiver đang sử dụng ứng dụng. | | |
| **Điều kiện sau (Post-conditions)** | 1. Tạo bản ghi sự kiện trong `emergency_alert_events`.<br>2. Bắn tín hiệu WebSocket báo động lên Dashboard viện.<br>3. Màn hình người dùng ở chế độ khẩn cấp. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Hệ thống phát hiện điều kiện Red Flag | Lập tức chuyển giao diện ứng dụng sang màn hình Cảnh báo Đỏ toàn màn hình (SCR-CG-12) (BR11). |
| | 2 | Màn hình hiển thị nút gọi lớn 1 chạm: 'GỌI NGAY HOTLINE VISI: 0395 151 151' kèm chỉ dẫn sơ cứu (dán khiên mắt, không nhỏ thêm thuốc, đến viện ngay) | Caregiver bấm nút gọi; điện thoại tự động kết nối cuộc gọi cấp cứu tới tổng đài VISI. |
| | 3 | Hệ thống đồng thời phát chuông báo động đỏ trên Dashboard trạm viện (UC-022) | Khởi động đồng hồ SLA cam kết can thiệp <5 phút của CSKH. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Ngoài giờ làm việc viện / Bệnh nhân ở xa** | 1 | Bệnh nhân không kịp đến cơ sở VISI | Hệ thống hiển thị thêm địa chỉ cơ sở cấp cứu đa khoa gần nhất (ACT-008). |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR10 (Quy chuẩn kích hoạt Red Flag), BR11 (Ưu tiên quy trình hành động cấp cứu) | | |

---

---

---

### UC-021: Giám Sát Dashboard Phục Hồi Bệnh Nhân Tập Trung (Clinical Monitoring Dashboard)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-021** | | |
| **Tên Use Case (Use Case Name)** | Giám Sát Dashboard Phục Hồi Bệnh Nhân Tập Trung (Clinical Monitoring Dashboard) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-004 (CSKH / Medical Monitor), ACT-003 (Điều dưỡng) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-002 (Bác sĩ điều trị) | | |
| **Tính năng liên quan (Features)** | F-018 | | |
| **Mô tả tóm tắt (Brief Description)** | Giao diện quản lý tập trung cho phép CSKH và Điều dưỡng theo dõi sát sao tình trạng hồi phục, lọc ca nguy cơ và phối hợp xử lý. | | |
| **Mục tiêu (Goal)** | Theo dõi danh sách toàn bộ bệnh nhân của cơ sở theo trạng thái tuân thủ dùng thuốc và mức độ phục hồi trên Dashboard thời gian thực. | | |
| **Tác nhân kích hoạt (Trigger)** | Nhân viên CSKH hoặc Điều dưỡng đăng nhập Dashboard quản trị chi nhánh (SCR-DOC-12). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Nhân viên đã đăng nhập tài khoản hợp lệ của cơ sở bệnh viện. | | |
| **Điều kiện sau (Post-conditions)** | Bảng dữ liệu giám sát thời gian thực được kết xuất. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | CSKH mở Dashboard Giám sát phục hồi (SCR-DOC-12) | Hệ thống kết xuất danh sách bệnh nhân xuất viện thuộc chi nhánh. |
| | 2 | Danh sách hiển thị phân tầng: Ca Đỏ (Red Flag) ghim trên cùng -> Ca Vàng (Chú ý) -> Ca Xanh (Ổn định) | Hiển thị tỷ lệ uống thuốc đúng giờ, tình trạng nộp Recovery Check của từng bệnh nhân. |
| | 3 | Nhân viên sử dụng bộ lọc theo Bác sĩ mổ, Loại phẫu thuật, Ngày xuất viện | Hệ thống lọc và cập nhật danh sách tức thì. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Bác sĩ tra cứu biểu đồ phục hồi khi tái khám (US-030)** | 1 | Bác sĩ mở hồ sơ bệnh nhân đang ngồi khám | Hệ thống hiển thị dòng thời gian tuân thủ thuốc và các ghi nhận triệu chứng tại nhà. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR14 (Kiểm soát truy cập nhân viên), BR15 (Phân vùng dữ liệu theo chi nhánh) | | |

---

---

---

### UC-022: Tiếp Nhận, Phân Loại và Điều Phối Cảnh Báo Red Flag (Alert Triaging & Status Handling)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-022** | | |
| **Tên Use Case (Use Case Name)** | Tiếp Nhận, Phân Loại và Điều Phối Cảnh Báo Red Flag (Alert Triaging & Status Handling) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-004 (CSKH / Medical Monitor) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-002 (Bác sĩ trực), ACT-001 (Caregiver) | | |
| **Tính năng liên quan (Features)** | F-019 | | |
| **Mô tả tóm tắt (Brief Description)** | CSKH tiếp nhận sự kiện báo động đỏ phát chuông trên Dashboard, nhận ca, gọi điện thoại hỗ trợ chuyên môn và phối hợp Bác sĩ. | | |
| **Mục tiêu (Goal)** | Tiếp nhận ca cảnh báo đỏ, chuyển trạng thái xử lý, gọi điện can thiệp khẩn cấp trong cam kết SLA dưới 5 phút. | | |
| **Tác nhân kích hoạt (Trigger)** | Dashboard nhận tín hiệu Red Flag thời gian thực từ ứng dụng bệnh nhân. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Dashboard CSKH đang ở chế độ trực. | | |
| **Điều kiện sau (Post-conditions)** | Ca bệnh chuyển trạng thái `IN_PROGRESS`, thực hiện cuộc gọi can thiệp lâm sàng. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Dashboard phát chuông báo động lớn và ghim ca Red Flag lên đầu danh sách | CSKH bấm nút 'Tiếp nhận ca' trên thẻ bệnh nhân. |
| | 2 | Hệ thống chuyển trạng thái ca sang `IN_PROGRESS` và dừng chuông báo | Giao diện mở thông tin liên hệ Caregiver, tiền sử mổ và triệu chứng nguy cấp vừa ghi nhận. |
| | 3 | CSKH gọi điện thoại ngay cho Caregiver trong <5 phút (BR12) | Thực hiện tư vấn y tế, hướng dẫn sơ cứu và kết nối bác sĩ nếu cần thiết. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Cuộc gọi không có người nhấc máy** | 1 | CSKH gọi lần 1 không liên lạc được | Hệ thống đếm thời gian gọi lại sau 2 phút và gửi tin nhắn SMS khẩn cấp tới số điện thoại dự phòng. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Quá 15 phút chưa được tiếp nhận xử lý** | 1 | Nhân viên trực bận hoặc bỏ lỡ cảnh báo | Hệ thống tự động kích hoạt luồng leo thang cảnh báo (UC-022b). |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR12 (Quy chuẩn thời gian phản hồi SLA Red Flag <5 phút) | | |

---

---

---

### UC-022b: Tự Động Leo Thang Cảnh Báo Red Flag Chưa Xử Lý (Automated Alert Escalation)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-022b** | | |
| **Tên Use Case (Use Case Name)** | Tự Động Leo Thang Cảnh Báo Red Flag Chưa Xử Lý (Automated Alert Escalation) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-007 (System / Admin) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-004 (CSKH), ACT-002 (Bác sĩ trực) | | |
| **Tính năng liên quan (Features)** | F-019 | | |
| **Mô tả tóm tắt (Brief Description)** | Cơ chế bảo vệ an toàn tối thượng chống bỏ sót tai biến y khoa ngoài viện; tự động leo thang khi vi phạm cam kết SLA. | | |
| **Mục tiêu (Goal)** | Tự động phát chuông cấp độ 2 và gửi tin nhắn khẩn cấp lên Bác sĩ trực/Lãnh đạo cơ sở nếu ca đỏ chưa được xử lý sau 15 phút. | | |
| **Tác nhân kích hoạt (Trigger)** | Hệ thống kiểm tra định kỳ phát hiện sự kiện Red Flag tồn tại quá 15 phút ở trạng thái `NEW` hoặc chưa có cuộc gọi ghi nhận. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Sự kiện Red Flag đã phát sinh quá 15 phút. | | |
| **Điều kiện sau (Post-conditions)** | 1. Trạng thái ca chuyển sang `ESCALATED_OVERDUE`.<br>2. Gửi tin nhắn SMS khẩn cấp đến Bác sĩ trực và Trưởng cơ sở.<br>3. Ghi vết vi phạm vào Audit Log. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bộ giám sát hệ thống phát hiện ca Red Flag quá hạn 15 phút | Kích hoạt quy trình leo thang cấp độ 2. |
| | 2 | Hệ thống đổi trạng thái ca bệnh sang `ESCALATED_OVERDUE` và phát chuông cấp độ 2 trên toàn bộ máy trạm chi nhánh | Gọi dịch vụ SMS Gateway gửi tin nhắn khẩn cấp tới Bác sĩ trực cơ sở và Trưởng phòng Chuyên môn. |
| | 3 | Bác sĩ trực tiếp nhận ca và trực tiếp gọi điện can thiệp cho bệnh nhân | Ghi nhận nhật ký xử lý khẩn cấp. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR12 (Cơ chế leo thang cảnh báo quá hạn 15 phút) | | |

---

---

---

### UC-023: Ghi Nhận Nhật Ký Cuộc Gọi và Can Thiệp Lâm Sàng (Call & Clinical Action Logging)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-023** | | |
| **Tên Use Case (Use Case Name)** | Ghi Nhận Nhật Ký Cuộc Gọi và Can Thiệp Lâm Sàng (Call & Clinical Action Logging) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-004 (CSKH), ACT-003 (Điều dưỡng) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-002 (Bác sĩ điều trị) | | |
| **Tính năng liên quan (Features)** | F-020 | | |
| **Mô tả tóm tắt (Brief Description)** | CSKH hoặc Điều dưỡng ghi nhận nội dung cuộc gọi hỗ trợ: thời gian, người nghe máy, tình trạng thực tế và kết luận can thiệp. | | |
| **Mục tiêu (Goal)** | Ghi nhận chi tiết kết quả cuộc gọi tư vấn, lời dặn y tế và trạng thái bệnh nhân vào hồ sơ điện tử. | | |
| **Tác nhân kích hoạt (Trigger)** | CSKH kết thúc cuộc gọi điện thoại hỗ trợ bệnh nhân. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Ca bệnh đang ở trạng thái `IN_PROGRESS`. | | |
| **Điều kiện sau (Post-conditions)** | 1. Lưu bản ghi vào `call_intervention_logs`.<br>2. Cập nhật trạng thái ca bệnh sang `RESOLVED` hoặc `TRANSFERRED_TO_DOCTOR`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | CSKH nhấn 'Ghi nhận kết quả cuộc gọi' trên thẻ ca bệnh | Mở form ghi nhật ký cuộc gọi tích hợp. |
| | 2 | CSKH nhập: Thời lượng gọi, Người tiếp nhận (Caregiver/Bệnh nhân), Tình trạng ghi nhận thực tế, Hướng can thiệp (Đã hướng dẫn kiêng cữ / Đã hẹn khám khẩn cấp / Đã chuyển Bác sĩ) | Hệ thống lưu bản ghi vào `call_intervention_logs`. |
| | 3 | CSKH chọn trạng thái kết thúc: 'Đã giải quyết an toàn' (RESOLVED) | Hệ thống đóng cảnh báo, cập nhật màu thẻ về trạng thái an toàn và lưu vết kiểm toán. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR12 (Quy định lưu vết can thiệp lâm sàng) | | |

---

---

---

### UC-024: Tra Cứu Tình Huống Chăm Sóc Khẩn Cấp - FAQ Lâm Sàng (Contextual Care Quick-Links)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-024** | | |
| **Tên Use Case (Use Case Name)** | Tra Cứu Tình Huống Chăm Sóc Khẩn Cấp - FAQ Lâm Sàng (Contextual Care Quick-Links) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-006 (Bệnh nhân) | | |
| **Tính năng liên quan (Features)** | F-028 | | |
| **Mô tả tóm tắt (Brief Description)** | Menu tra cứu nhanh giúp người nhà tự xử lý đúng cách các tình huống giật mình thường ngày mà không hoảng sợ. | | |
| **Mục tiêu (Goal)** | Tra cứu nhanh chỉ dẫn chuẩn y khoa theo tình huống thường gặp tại nhà (dính nước, quên nhỏ thuốc, cộm xốn, vô tình dụi mắt). | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver chọn menu 'Hỏi đáp khẩn cấp & Tình huống thường gặp' trên ứng dụng. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Caregiver đã đăng nhập ứng dụng. | | |
| **Điều kiện sau (Post-conditions)** | Hiển thị câu trả lời y khoa chuẩn do Bác sĩ VISI kiểm duyệt sẵn. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver mở mục FAQ tình huống khẩn cấp | Hệ thống hiển thị danh sách các tình huống hay gặp nhất: 'Dính nước vào mắt', 'Quên nhỏ thuốc 1 cữ', 'Vô tình chạm tay vào mắt', 'Mắt chảy nước mắt liên tục'. |
| | 2 | Caregiver chọn một tình huống (ví dụ: 'Dính nước máy vào mắt') | Hệ thống hiển thị chỉ dẫn 3 bước: (1) Nhắm mắt nhẹ nhàng, không dụi; (2) Dùng gạc sạch thấm khô mi; (3) Nhỏ ngay 1 giọt kháng sinh chỉ định và theo dõi. |
| | 3 | Hiển thị nút 'Vẫn lo lắng? Gọi Hotline VISI 0395 151 151' | Hỗ trợ kết nối nhân viên y tế nếu người nhà vẫn chưa an tâm. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P1** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR7 (Chuẩn y khoa đã được kiểm duyệt) | | |

---

---

---

### UC-025: Kích Hoạt Chế Độ Trợ Năng Nhãn Khoa (Ophthalmic Accessibility Mode)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-025** | | |
| **Tên Use Case (Use Case Name)** | Kích Hoạt Chế Độ Trợ Năng Nhãn Khoa (Ophthalmic Accessibility Mode) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-006 (Bệnh nhân), ACT-001 (Caregiver) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Không có | | |
| **Tính năng liên quan (Features)** | F-026 | | |
| **Mô tả tóm tắt (Brief Description)** | Tối ưu trải nghiệm cho bệnh nhân lớn tuổi sau mổ mắt thị lực còn mờ, hỗ trợ tự nghe lịch thuốc qua giọng đọc tiếng Việt. | | |
| **Mục tiêu (Goal)** | Chuyển giao diện sang chữ lớn (≥18pt), tương phản cao High Contrast và bật tính năng Audio Guide đọc tiếng Việt. | | |
| **Tác nhân kích hoạt (Trigger)** | Bệnh nhân hoặc Caregiver nhấn biểu tượng 'Trợ Năng Mắt' trên thanh tiêu đề. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Ứng dụng đang mở trên trình duyệt di động. | | |
| **Điều kiện sau (Post-conditions)** | Giao diện chuyển đổi sang chế độ trợ năng nhãn khoa. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Người dùng nhấn bật công tắc 'Chế độ Trợ Năng Mắt' | Hệ thống áp dụng bộ CSS trợ năng: cỡ chữ tăng lên ≥18pt, tiêu đề ≥24pt, độ tương phản nền đen chữ vàng đạt chuẩn WCAG AAA, nút bấm mở rộng ≥48px. |
| | 2 | Người dùng chạm vào một cữ thuốc trong ngày | Hệ thống tự động phát âm thanh Text-to-Speech (TTS) đọc rõ: 'Cữ thuốc sáng: nhỏ 1 giọt Cravit vào mắt phải'. |
| | 3 | Người dùng có thể tắt chế độ trợ năng bất cứ lúc nào | Giao diện trở về chế độ tiêu chuẩn. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P1** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR6 (Phân quyền truy cập trợ năng) | | |

---

---

---

### NHÓM UC-026: QUẢN LÝ TÀI KHOẢN NHÂN VIÊN VÀ PHÂN QUYỀN CƠ SỞ (STAFF ACCOUNTS & MULTI-BRANCH RBAC CRUD)

> **Mô tả nhóm nghiệp vụ:** Quản trị vòng đời tài khoản người dùng nội bộ bệnh viện (Bác sĩ, Điều dưỡng, CSKH, GCMO, Admin). Phân rã thành 4 Use Case CRUD nhằm kiểm soát chặt chẽ quy trình tạo tài khoản, tra cứu danh sách, phân quyền vai trò (Role-Based Access Control) và khóa tài khoản thu hồi quyền hạn ngay lập tức khi nhân sự thay đổi hoặc nghỉ việc (BR26).

#### UC-026.1: Khởi Tạo Tài Khoản Nhân Viên Y Tế & Gán Chi Nhánh (Create Staff Account — POST /api/v1/staff)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-026.1** | | |
| **Tên Use Case (Use Case Name)** | Khởi Tạo Tài Khoản Nhân Viên Y Tế & Gán Chi Nhánh (Create Staff Account — POST /api/v1/staff) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Quản trị viên hệ thống - System Admin (ACT-006) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL, Dịch vụ SMS/Email OTP | | |
| **Tính năng liên quan (Features)** | F-026 (Quản lý Nhân viên & Phân quyền), F-002 (Xác thực 2FA Nhân viên) | | |
| **Mô tả tóm tắt (Brief Description)** | Admin nhập thông tin định danh nhân sự, chọn vai trò RBAC (Doctor, Nurse, CSKH, GCMO), chọn chi nhánh công tác và gửi đường dẫn thiết lập mật khẩu/2FA qua SMS/Email công vụ. | | |
| **Mục tiêu (Goal)** | Tạo mới tài khoản nhân viên y tế, gán vai trò chuyên môn và gán phạm vi cơ sở bệnh viện trực thuộc (thuộc 1 trong 5 cơ sở VISI — BR26). | | |
| **Tác nhân kích hoạt (Trigger)** | Admin bấm nút '+ Thêm Nhân Viên Mới' tại module Quản trị Người dùng (SCR-DOC-01). | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Admin đã đăng nhập với vai trò SYSTEM_ADMIN. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi mới được tạo trong `accounts` và `doctor_profiles` với `status = 'PENDING_ACTIVATION'`, gửi SMS/Email kích hoạt. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Admin chọn nút '+ Thêm Nhân Viên Mới' tại portal quản trị. | Hệ thống mở modal 'Tạo Tài Khoản Nhân Sự Y Tế'. |
| | 2 | Admin nhập Họ tên nhân viên, Email công vụ (đuôi `@visi.vn`), Số điện thoại di động (10 chữ số). | Hệ thống kiểm tra tính hợp lệ và duy nhất của Email và SĐT. |
| | 3 | Admin chọn Vai trò chuyên môn: `DOCTOR` (Bác sĩ điều trị), `NURSE` (Điều dưỡng lưu viện), `CSKH` (Chăm sóc khách hàng), `GCMO` (Giám đốc Lâm sàng), `ADMIN` (Quản trị viên). | Hệ thống gán ma trận quyền hạn tương ứng. |
| | 4 | Admin chọn Cơ sở công tác trực thuộc: Cơ sở 1 đến Cơ sở 5 của Tập đoàn VISI (hoặc 'Toàn Hệ Thống' nếu là GCMO/Ban Giám đốc). | Hệ thống gán `facility_id` để thiết lập ranh giới dữ liệu an toàn (BR26). |
| | 5 | Admin bấm 'Khởi Tạo Tài Khoản & Gửi Kích Hoạt'. | Frontend gửi `POST /api/v1/staff`, backend lưu dữ liệu, sinh token kích hoạt an toàn và gửi SMS/Email tới nhân viên. |
| | 6 | Hệ thống ghi nhận Audit Log `CREATE_STAFF_ACCOUNT`. | Trả về HTTP 201 Created và hiển thị thông báo thành công. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Email hoặc Số điện thoại đã tồn tại** | 1 | Thông tin định danh trùng với nhân viên khác trong hệ thống. | Hệ thống trả về HTTP 409 Conflict: 'Email hoặc Số điện thoại này đã được đăng ký cho một tài khoản nhân sự khác'. |
| **Không có quyền quản trị** | 1 | Người dùng không có vai trò SYSTEM_ADMIN cố gắng gọi API. | Hệ thống trả về HTTP 403 Forbidden và ghi log an ninh. |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR26 (Phân quyền RBAC và giới hạn dữ liệu đa cơ sở bệnh viện) | | |

---

#### UC-026.2: Xem Danh Sách & Lọc Nhân Viên Theo Cơ Sở (Read/List Staff — GET /api/v1/staff)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-026.2** | | |
| **Tên Use Case (Use Case Name)** | Xem Danh Sách & Lọc Nhân Viên Theo Cơ Sở (Read/List Staff — GET /api/v1/staff) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Quản trị viên hệ thống (ACT-006), Ban Giám đốc (ACT-005) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL | | |
| **Tính năng liên quan (Features)** | F-026 (Quản lý Nhân viên & Phân quyền) | | |
| **Mô tả tóm tắt (Brief Description)** | Cung cấp bảng dữ liệu danh bạ nhân sự phân trang, hỗ trợ tìm kiếm nhanh theo họ tên, email hoặc SĐT. | | |
| **Mục tiêu (Goal)** | Tra cứu, tìm kiếm và lọc danh sách toàn bộ nhân viên y tế theo chi nhánh cơ sở, vai trò và trạng thái hoạt động. | | |
| **Tác nhân kích hoạt (Trigger)** | Admin hoặc Ban Giám đốc truy cập menu 'Quản trị Nhân sự' tại SCR-DOC-01. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Người dùng đã đăng nhập với vai trò ADMIN hoặc HOSPITAL_DIRECTOR. | | |
| **Điều kiện sau (Post-conditions)** | Bảng danh sách nhân viên hiển thị đầy đủ thông tin phân trang. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Admin mở menu 'Quản trị Nhân sự'. | Frontend gửi request `GET /api/v1/staff?page=1&limit=20`. |
| | 2 | Backend truy vấn kết hợp bảng `accounts` và `doctor_profiles`. | Hệ thống trả về danh sách nhân viên kèm cơ sở và vai trò. |
| | 3 | Giao diện hiển thị bảng dữ liệu nhân sự: | Cột: Họ tên, Email, SĐT, Vai trò RBAC, Cơ sở công tác, Trạng thái (`ACTIVE` xanh, `PENDING` vàng, `LOCKED` đỏ), Nút thao tác. |
| | 4 | Admin chọn bộ lọc Chi nhánh (ví dụ: 'Cơ sở 2 - Hải Phòng') hoặc tìm theo tên bác sĩ. | Bảng dữ liệu cập nhật kết quả lọc tức thì. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR26 (Phân quyền đa cơ sở) | | |

---

#### UC-026.3: Chỉnh Sửa Thông Tin & Vai Trò RBAC Nhân Viên (Update Staff — PUT /api/v1/staff/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-026.3** | | |
| **Tên Use Case (Use Case Name)** | Chỉnh Sửa Thông Tin & Vai Trò RBAC Nhân Viên (Update Staff — PUT /api/v1/staff/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Quản trị viên hệ thống (ACT-006) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL, Hệ thống Audit Trail | | |
| **Tính năng liên quan (Features)** | F-026, F-027 | | |
| **Mô tả tóm tắt (Brief Description)** | Admin thay đổi thông tin hồ sơ nhân viên; khi thay đổi vai trò hoặc chi nhánh, hệ thống sẽ thu hồi ngay các phiên làm việc hiện tại để ép nhân viên đăng nhập lại với quyền mới. | | |
| **Mục tiêu (Goal)** | Cập nhật thông tin nhân sự, điều chuyển cơ sở làm việc hoặc nâng cấp/thay đổi quyền hạn vai trò chuyên môn. | | |
| **Tác nhân kích hoạt (Trigger)** | Admin bấm nút 'Chỉnh sửa' trên một dòng nhân viên. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Tài khoản nhân viên tồn tại trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Thông tin được cập nhật trong CSDL; các token JWT cũ bị thu hồi vào blacklist; ghi Audit Log nghiêm ngặt. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Admin bấm nút 'Sửa' trên nhân viên cần cập nhật. | Hệ thống mở modal chỉnh sửa với thông tin hiện tại. |
| | 2 | Admin thay đổi Chi nhánh công tác từ 'Cơ sở 1' sang 'Cơ sở 3' (Điều chuyển nhân sự). | Hệ thống ghi nhận sự thay đổi cơ sở dữ liệu. |
| | 3 | Admin bấm 'Lưu Thay Đổi'. | Frontend gửi `PUT /api/v1/staff/{id}`, backend cập nhật CSDL. |
| | 4 | Backend kích hoạt cơ chế thu hồi phiên làm việc (Session Revocation): | Vô hiệu hóa toàn bộ refresh token và access token hiện tại của nhân viên đó để ngăn rò rỉ quyền cũ. |
| | 5 | Hệ thống ghi nhật ký kiểm toán `UPDATE_STAFF_ROLE_FACILITY`. | Trả về HTTP 200 OK kèm thông báo thành công. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Tự hạ quyền tài khoản Admin duy nhất** | 1 | Admin duy nhất của hệ thống cố tình đổi vai trò của chính mình sang vai trò khác. | Hệ thống từ chối: 'Không thể hạ quyền của tài khoản Quản trị viên tối cao cuối cùng trong hệ thống'. |
| **Mức độ ưu tiên (Priority)** | **Cao (High)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR26 (Nguyên tắc thu hồi quyền hạn ngay lập tức) | | |

---

#### UC-026.4: Khóa / Vô Hiệu Hóa Tài Khoản Nhân Viên (Deactivate/Lock Staff — DELETE /api/v1/staff/{id})

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-026.4** | | |
| **Tên Use Case (Use Case Name)** | Khóa / Vô Hiệu Hóa Tài Khoản Nhân Viên (Deactivate/Lock Staff — DELETE /api/v1/staff/{id}) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Phân rã CRUD) |
| **Tác nhân chính (Primary Actor)** | Quản trị viên hệ thống (ACT-006) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Hệ thống CSDL PostgreSQL, Dịch vụ Token Blacklist, Audit Trail | | |
| **Tính năng liên quan (Features)** | F-026, F-027 | | |
| **Mô tả tóm tắt (Brief Description)** | Admin chuyển trạng thái tài khoản sang `LOCKED`, lập tức hủy toàn bộ phiên làm việc đang active trên mọi thiết bị và chặn đăng nhập mới. | | |
| **Mục tiêu (Goal)** | Khóa tức thì quyền truy cập của nhân viên nghỉ việc hoặc có hành vi vi phạm bảo mật y tế. | | |
| **Tác nhân kích hoạt (Trigger)** | Admin bấm nút 'Khóa Tài Khoản' trên dòng nhân viên tại SCR-DOC-01. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Tài khoản nhân viên đang ở trạng thái `ACTIVE` và khác tài khoản Admin đang đăng nhập. | | |
| **Điều kiện sau (Post-conditions)** | Trường `status` chuyển thành `'LOCKED'`, nhân viên bị văng ra khỏi hệ thống ngay lập tức; ghi log kiểm toán cấp độ bảo mật cao nhất. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Admin bấm nút 'Khóa Tài Khoản' trên nhân viên. | Hệ thống hiển thị modal cảnh báo an ninh màu đỏ: 'CẢNH BÁO: Thao tác này sẽ ngắt toàn bộ phiên làm việc của nhân viên trên mọi thiết bị ngay lập tức. Bạn có chắc chắn muốn khóa tài khoản này?'. |
| | 2 | Admin nhập lý do khóa tài khoản (ví dụ: 'Nhân viên chấm dứt hợp đồng lao động ngày 15/09/2026') và xác nhận. | Frontend gửi request `DELETE /api/v1/staff/{id}` kèm lý do. |
| | 3 | Backend cập nhật `status = 'LOCKED'`, `locked_at = CURRENT_TIMESTAMP`. | Đồng thời đẩy toàn bộ `user_id` vào Redis token blacklist để ngắt kết nối websocket/API tức thì. |
| | 4 | Ghi nhật ký kiểm toán nghiêm ngặt `LOCK_STAFF_ACCOUNT` kèm IP và danh tính Admin thực hiện. | Hệ thống trả về HTTP 200 OK. |
| | 5 | Giao diện cập nhật huy hiệu nhân viên sang màu Đỏ 'ĐÃ KHÓA'. | Hiển thị thông báo hoàn tất thao tác an ninh. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện chuẩn không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **Tự khóa tài khoản của chính mình** | 1 | Admin bấm khóa chính tài khoản mình đang đăng nhập. | Hệ thống chặn thao tác: 'Bạn không thể tự khóa tài khoản của chính mình'. |
| **Tài khoản đã bị khóa từ trước** | 1 | Nhân viên đã ở trạng thái LOCKED. | Hệ thống báo lỗi: 'Tài khoản nhân viên này đã bị vô hiệu hóa từ trước'. |
| **Mức độ ưu tiên (Priority)** | **Cao (High - An ninh dữ liệu tối khẩn)** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR26 (Nguyên tắc thu hồi quyền hạn ngay lập tức - Principle of Least Privilege) | | |

---


### UC-027: Tra Cứu Nhật Ký Kiểm Toán Hệ Thống (Audit Trail & Compliance Logging)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-027** | | |
| **Tên Use Case (Use Case Name)** | Tra Cứu Nhật Ký Kiểm Toán Hệ Thống (Audit Trail & Compliance Logging) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-007 (Quản trị viên hệ thống) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| Ban Giám Đốc (CEO, COO) | | |
| **Tính năng liên quan (Features)** | F-024 | | |
| **Mô tả tóm tắt (Brief Description)** | Hệ thống ghi vết bất biến mọi hành vi can thiệp vào Care Plan, in QR, sửa đơn thuốc, nộp khảo sát, tiếp nhận Red Flag. | | |
| **Mục tiêu (Goal)** | Truy vấn và kết xuất nhật ký thao tác lâm sàng phục vụ kiểm tra an toàn thông tin, bảo mật dữ liệu và pháp lý y khoa. | | |
| **Tác nhân kích hoạt (Trigger)** | Quản trị viên hoặc Ban Giám Đốc mở module Nhật ký kiểm toán. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Tài khoản có quyền kiểm toán cấp hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Báo cáo kiểm toán được kết xuất phục vụ thanh tra. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Quản trị viên mở màn hình Tra cứu Nhật ký kiểm toán | Hệ thống hiển thị bộ lọc thời gian, tác nhân, loại hành động. |
| | 2 | Quản trị viên nhập tiêu chí tìm kiếm (ví dụ: các thao tác sửa đơn thuốc trong tuần qua tại cơ sở Thủ Đức) | Hệ thống truy vấn bảng `system_audit_logs` và trả về danh sách bản ghi bất biến kèm IP, timestamp và nội dung thay đổi (Diff). |
| | 3 | Quản trị viên nhấn 'Xuất báo cáo PDF/Excel' | Hệ thống xuất file báo cáo có chữ ký số xác thực dữ liệu. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P1** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR5 (Tính toàn vẹn dữ liệu), BR21 (Tính bất biến của nhật ký kiểm toán) | | |

---

---

---

### UC-028: Kết Xuất Báo Cáo Vận Hành và Chỉ Số Tuân Thủ KPI (Compliance Analytics & KPIs)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-028** | | |
| **Tên Use Case (Use Case Name)** | Kết Xuất Báo Cáo Vận Hành và Chỉ Số Tuân Thủ KPI (Compliance Analytics & KPIs) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | Ban Giám Đốc (CEO, COO) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-007 (System Admin) | | |
| **Tính năng liên quan (Features)** | F-025 | | |
| **Mô tả tóm tắt (Brief Description)** | Cung cấp bảng phân tích số liệu vận hành phục vụ Ban Lãnh đạo VISI Medical Group đánh giá hiệu quả áp dụng nền tảng. | | |
| **Mục tiêu (Goal)** | Tổng hợp các chỉ số KPIs: tỷ lệ kích hoạt QR (mục tiêu ≥85%), tỷ lệ tuân thủ thuốc đúng giờ, tỷ lệ hoàn thành Recovery Check, tỷ lệ tái khám theo từng chi nhánh. | | |
| **Tác nhân kích hoạt (Trigger)** | Lãnh đạo truy cập phân hệ Báo cáo quản trị định kỳ. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Tài khoản lãnh đạo có quyền xem báo cáo toàn chuỗi. | | |
| **Điều kiện sau (Post-conditions)** | Báo cáo phân tích KPI thời gian thực được hiển thị trực quan dạng biểu đồ. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Lãnh đạo mở màn hình Báo cáo vận hành KPI | Hệ thống tổng hợp dữ liệu từ 5 chi nhánh bệnh viện. |
| | 2 | Xem các chỉ số trọng yếu: Tỷ lệ quét kích hoạt QR (thực tế vs mục tiêu 85%), Tỷ lệ uống thuốc đúng giờ theo loại phẫu thuật, Tỷ lệ phản ứng Red Flag <5 phút của CSKH | Hiển thị biểu đồ so sánh giữa các chi nhánh. |
| | 3 | Lãnh đạo chọn xuất báo cáo tổng kết tháng | Hệ thống tạo file báo cáo định dạng PDF phục vụ họp giao ban tập đoàn. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Luồng thực hiện tuần tự không rẽ nhánh | - |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P1** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR15 (Quyền xem dữ liệu tổng hợp liên chi nhánh cho Ban Giám Đốc) | | |

---

---

---

## 3. BẢNG TỔNG HỢP DANH MỤC QUY TẮC NGHIỆP VỤ TOÀN HỆ THỐNG (MASTER BUSINESS RULES CATALOG)

### 3.1 Bảng Ma Trận Quy Tắc Nghiệp Vụ (Master BR Matrix)

| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Chi tiết Quy tắc Nghiệp vụ | Use Cases áp dụng |
| :--- | :--- | :--- | :--- |
| **BR1** | **Kiểm tra tính hợp lệ của số điện thoại** | Hệ thống phải xác thực định dạng số điện thoại của Caregiver (chuẩn di động Việt Nam: 10 chữ số, đầu số hợp lệ) trước khi khởi tạo quy trình gửi mã OTP. | UC-001, UC-002, UC-004, UC-005, UC-006, UC-008, UC-010, UC-019, UC-020, UC-021, UC-022, UC-022b, UC-023, UC-026, UC-028 |
| **BR2** | **Xác thực mã OTP** | Mã OTP gồm 6 chữ số ngẫu nhiên, có hiệu lực trong 5 phút. Nếu nhập sai quá 5 lần liên tiếp, hệ thống khóa tạm thời yêu cầu gửi OTP trong 15 phút. | UC-001, UC-007, UC-009, UC-014, UC-015, UC-016, UC-018, UC-027 |
| **BR3** | **Phân quyền tài khoản Caregiver** | Chỉ những tài khoản Caregiver đang ở trạng thái hoạt động (Active) mới được phép xác thực thành công và truy cập các chức năng chăm sóc bệnh nhân. | UC-001 |
| **BR4** | **Tính hợp lệ và an toàn của mã QR** | Mã QR phải chứa token mã hóa ngẫu nhiên (UUIDv4 kết hợp ký số), không chứa dữ liệu bệnh án dạng thô. Mã QR phải thuộc Kế hoạch chăm sóc đang ở trạng thái ACTIVE mới được phép liên kết. | UC-003, UC-010b, UC-011, UC-012, UC-013 |
| **BR5** | **Giới hạn tối đa 3 Caregiver liên kết** | Một Caregiver có thể liên kết với nhiều bệnh nhân, nhưng mỗi hồ sơ bệnh nhân chỉ cho phép tối đa 3 Caregiver cùng liên kết để chia sẻ ca chăm sóc. Mọi thao tác đều được lưu vết kiểm toán. | UC-003, UC-010c, UC-027 |
| **BR6** | **Phạm vi nội dung được cấp quyền** | Caregiver chỉ được truy cập các nội dung Learning Path, lịch thuốc và thông tin chăm sóc của bệnh nhân đã được liên kết hợp lệ với mình. | UC-017, UC-025 |
| **BR7** | **Nguồn gốc nội dung y khoa** | Mọi hướng dẫn chuyên môn, kỹ thuật nhỏ thuốc, liều lượng và cữ dùng phải xuất phát từ cấu hình của Bác sĩ/Bệnh viện VISI. Hệ thống tuyệt đối không tự động suy diễn hoặc điều chỉnh khuyến nghị y tế. | UC-009, UC-010b, UC-014, UC-016, UC-017, UC-024 |
| **BR8** | **Chính sách bỏ quiz trong MVP** | Trong phiên bản MVP, Học viện Caregiver tập trung vào Infographics đồ họa tĩnh tinh gọn (F-014). Việc làm bài kiểm tra quiz không phải là điều kiện tiên quyết và được dời sang Phase 2. | Áp dụng toàn hệ thống |
| **BR9** | **Bắt buộc hoàn thành bảng kiểm Recovery Check** | Caregiver bắt buộc phải trả lời đầy đủ toàn bộ 3–5 câu hỏi khảo sát trước khi được phép nộp kết quả; không cho phép gửi bảng kiểm còn bỏ trống câu hỏi. | UC-008, UC-019 |
| **BR10** | **Giới hạn chẩn đoán và phân loại trạng thái 3 mức** | Hệ thống chỉ đối chiếu câu trả lời với ma trận tiêu chí lâm sàng đã được Bác sĩ cấu hình sẵn để phân loại 3 mức: Xanh (Bình thường), Vàng (Cần chú ý), Đỏ (Nguy hiểm Red Flag). Hệ thống không đưa ra chẩn đoán bệnh thay thế bác sĩ. | UC-008, UC-019, UC-020 |
| **BR11** | **Ưu tiên quy trình hành động cấp cứu Red Flag** | Khi xuất hiện dấu hiệu Red Flag, hệ thống lập tức chuyển sang giao diện cảnh báo đỏ toàn màn hình, ưu tiên hiển thị nút bấm gọi 1 chạm đến Hotline VISI 0395 151 151 và đẩy tín hiệu báo động khẩn cấp lên Dashboard viện. | UC-008, UC-020 |
| **BR12** | **Cam kết SLA phản ứng Red Flag & Leo thang** | Nhân viên CSKH/Điều dưỡng trực phải tiếp nhận và thực hiện cuộc gọi can thiệp cho ca Red Flag trong vòng dưới 5 phút. Nếu sau 15 phút chưa được xử lý, hệ thống tự động leo thang (Escalate) gửi tin nhắn SMS khẩn cấp tới Bác sĩ trực cơ sở. | UC-022, UC-022b, UC-023 |
| **BR13** | **Quy định lưu vết can thiệp lâm sàng** | Mọi cuộc gọi hỗ trợ của CSKH hoặc can thiệp của Bác sĩ đối với các ca Red Flag/Cờ vàng đều phải được ghi nhận chi tiết (thời gian, người tiếp nhận, hướng xử lý) vào hồ sơ điện tử. | Áp dụng toàn hệ thống |
| **BR14** | **Kiểm soát quyền truy cập của Nhân viên y tế** | Chỉ các tài khoản nội bộ được cấp phát chính thức có vai trò phù hợp (Admin, Bác sĩ, Điều dưỡng, CSKH, GCMO) mới được đăng nhập vào hệ thống quản trị bệnh trạm kết hợp xác thực 2 bước (2FA). | UC-002, UC-021, UC-026 |
| **BR15** | **Phân tách dữ liệu theo cơ sở bệnh viện (Multi-Branch Isolation)** | Nhân viên y tế thuộc chi nhánh bệnh viện nào chỉ được phép xem, chỉnh sửa hồ sơ và giám sát bệnh nhân thuộc chi nhánh đó. Chỉ Ban Giám Đốc và GCMO mới có quyền xem dữ liệu tổng hợp toàn chuỗi 5 bệnh viện. | UC-002, UC-021, UC-026, UC-028 |
| **BR16** | **Tính toàn vẹn dữ liệu lâm sàng khi xuất viện** | Hồ sơ bệnh nhân tạo tại phòng lưu viện phải có tối thiểu: Mã bệnh nhân, Năm sinh, Mắt phẫu thuật (MP/MT/2M), Loại phẫu thuật và Bác sĩ mổ trước khi có thể kích hoạt Care Plan. | UC-004 |
| **BR17** | **Bảo mật thông tin bệnh nhân theo Nghị định 13/2023/NĐ-CP** | Tên bệnh nhân hiển thị trên ứng dụng của Caregiver và phiếu in xuất viện phải được lưu trữ và hiển thị ở dạng viết tắt bảo mật (ví dụ: 'Trần V. B.') để chống lộ dữ liệu cá nhân nhạy cảm. | UC-004 |
| **BR18** | **Tính độc lập dữ liệu Care Plan cá nhân hóa (Data Independence)** | Khi Điều dưỡng nhân bản Master Template thành Care Plan bệnh nhân, dữ liệu được sao chép sang bảng thực thi độc lập. Mọi tùy biến liều lượng của Bác sĩ cho bệnh nhân không làm biến đổi Master Template gốc. | UC-005, UC-010 |
| **BR19** | **Quy trình vòng đời Master Template** | Master Template tuân thủ nghiêm ngặt 4 trạng thái vòng đời: DRAFT (Bản nháp) -> PENDING_APPROVAL (Chờ duyệt) -> ACTIVE (Đã ban hành) -> ARCHIVED (Lưu trữ lịch sử). Chỉ template ACTIVE mới được phép áp dụng cho bệnh nhân. | UC-005, UC-006 |
| **BR20** | **Nhận diện trực quan danh mục thuốc** | Mỗi loại thuốc nhỏ mắt trong lịch dùng thuốc phải hiển thị rõ tên biệt dược, nồng độ, số giọt chỉ định, mắt áp dụng và hình ảnh màu sắc nắp/thân lọ thuốc để người nhà không nhầm lẫn. | UC-007, UC-014 |
| **BR21** | **Tính bất biến của nhật ký dùng thuốc** | Lịch sử xác nhận uống/nhỏ thuốc (timestamp, Caregiver thực hiện, tình trạng cữ) được lưu trữ bất biến vào `patient_medication_logs`, không cho phép sửa đổi hoặc xóa sau khi đã ghi nhận. | UC-015, UC-027 |
| **BR22** | **Hiển thị trực quan danh mục Nên làm & Cần tránh** | Danh mục Do & Don't phải được phân định rõ ràng thành 2 cột màu: Cột Xanh (Nên làm) và Cột Đỏ (Cần tránh), kèm icon minh họa và giải thích lý do y khoa. | UC-009, UC-016 |
| **BR23** | **Ràng buộc bộ đếm thời gian giãn cách đệm 5–10 phút (Drop Interval Buffer Timer)** | Nếu trong cùng một cữ dùng có từ 2 loại thuốc nhỏ mắt trở lên, sau khi xác nhận lọ thứ nhất, hệ thống tự động khóa nút xác nhận lọ thứ hai và đếm lùi 5–10 phút để tránh hiện tượng rửa trôi thuốc (washout effect). | UC-007, UC-015 |
| **BR24** | **Thời gian kích hoạt nhắc hẹn tái khám** | Hệ thống phải tự động kích hoạt thông báo đẩy (Push) và tin nhắn SMS/Zalo nhắc hẹn tái khám cho Caregiver trước thời điểm hẹn đúng 24 giờ. | UC-018 |
| **BR25** | **Ràng buộc toàn vẹn khi lưu trữ hồ sơ bệnh nhân** | Không cho phép xóa vĩnh viễn hồ sơ bệnh nhân. Chỉ cho phép chuyển trạng thái sang `ARCHIVED` (xóa mềm). Nghiêm cấm lưu trữ hồ sơ nếu đang có Care Plan ở trạng thái `ACTIVE`. | Áp dụng toàn hệ thống |
| **BR26** | **Tự động kích hoạt luồng CSKH cho ca xuất viện** | Ngay khi Care Plan chuyển sang trạng thái ACTIVE và hoàn tất bàn giao tại phòng lưu viện, hồ sơ bệnh nhân tự động xuất hiện trên Dashboard CSKH của chi nhánh để theo dõi từ xa. | Áp dụng toàn hệ thống |

---

### 3.2 Phân Loại BR Theo 6 Trụ Cột Nghiệp Vụ Chính

1. **Trụ cột Xác thực & An ninh dữ liệu (Security & Compliance):** `BR1`, `BR2`, `BR3`, `BR4`, `BR14`, `BR15`, `BR17`, `BR21`. Tuân thủ nghiêm ngặt Nghị định 13/2023/NĐ-CP về bảo vệ dữ liệu cá nhân y tế.
2. **Trụ cột Bàn giao Lâm sàng & Vòng đời QR (Clinical Handoff & QR):** `BR4`, `BR5`, `BR16`, `BR18`. Tối ưu thời gian bàn giao <30 giây tại phòng lưu viện, giới hạn tối đa 3 Caregiver cùng liên kết.
3. **Trụ cột Dược lý Nhãn khoa & Chống rửa trôi thuốc (Ophthalmic Pharmacology):** `BR7`, `BR20`, `BR23`. Bắt buộc bộ đếm thời gian giãn cách 5–10 phút giữa 2 loại thuốc nhỏ mắt (Drop Interval Buffer Timer).
4. **Trụ cột Giám sát Hồi phục & Sàng lọc Biến chứng (Triage & Monitoring):** `BR9`, `BR10`, `BR22`, `BR26`. Bảng kiểm Recovery Check 3 mức Xanh/Vàng/Đỏ xuất hiện mỗi sáng trong 7 ngày đầu.
5. **Trụ cột Cấp cứu Red Flag & Cam kết SLA (Emergency Response & SLA):** `BR11`, `BR12`, `BR13`. Hotline 1 chạm VISI 0395 151 151, cam kết CSKH liên hệ <5 phút, tự động leo thang sau 15 phút.
6. **Trụ cột Quản trị Chuyên môn & Tái khám (Clinical Governance & Appointments):** `BR6`, `BR8`, `BR19`, `BR24`, `BR25`. Phê duyệt Master Template điện tử và theo dõi lộ trình 5 mốc tái khám chuẩn VISI.
