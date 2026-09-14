# 05. Ma Trận Ánh Xạ Nghiệp Vụ (Screen - Entity - Database Mapping)

> **Dự án:** Hệ Thống Hướng Dẫn & Theo Dõi Chăm Sóc Bệnh Nhân Hậu Phẫu Mắt (Post-Op Eye Care Platform)  
> **Tài liệu tham chiếu:** [file_use_case_spec.md](file:///d:/DOC_BA/docs_of_projects/file_use_case_spec.md) | [01_Database_Analysis.md](file:///d:/DOC_BA/docs_of_projects/01_Database_Analysis.md) | [02_ERD.md](file:///d:/DOC_BA/docs_of_projects/02_ERD.md) | [03_Database_Design.md](file:///d:/DOC_BA/docs_of_projects/03_Database_Design.md) | [04_Screen_Flow.md](file:///d:/DOC_BA/docs_of_projects/04_Screen_Flow.md)  
> **Người thực hiện:** Đội ngũ BA  
> **Ngày lập:** 11/09/2026 | **Trạng thái:** Bản chuẩn hóa ma trận truy vết (Baseline)

---

## 1. Mục Tiêu & Phương Pháp Luận Truy Vết

Tài liệu này thiết lập cầu nối truy vết hoàn chỉnh (End-to-End Traceability Matrix) theo mô hình 4 tầng chuẩn mực của kỹ nghệ yêu cầu phần mềm:

$$\text{Use Case (Nghiệp vụ)} \longrightarrow \text{Screen (Giao diện UI)} \longrightarrow \text{Entity (Thực thể Logic)} \longrightarrow \text{Database Table (Bảng Vật lý)}$$

### Mục tiêu kiểm tra chất lượng (Quality Gates):
1. **100% Bao phủ Nghiệp vụ:** Đảm bảo toàn bộ các bước trong Use Case Specification đều có màn hình tương tác thực thi.
2. **100% Hỗ trợ Dữ liệu:** Đảm bảo mọi trường thông tin nhập vào hoặc kết xuất trên màn hình đều được lưu trữ và truy xuất từ các bảng cơ sở dữ liệu tương ứng.
3. **Không trùng lặp và Không over-design:** Tránh tình trạng mỗi Use Case tạo một thực thể độc lập gây phân mảnh cơ sở dữ liệu; thống nhất sử dụng chung mô hình Master Template và Patient Care Plan Instance.

---

## 2. Ma Trận Ánh Xạ 4 Tầng Toàn Diện (Traceability Matrix)

| Mã UC | Tên Use Case Nghiệp Vụ | Màn Hình Tương Tác (Screen) | Thực Thể Nghiệp Vụ (Entity) | Bảng Cơ Sở Dữ Liệu Vật Lý | Thao Tác Dữ Liệu (CRUD & Ràng Buộc) |
|---|---|---|---|---|---|
| **UC-01** | Đăng nhập Caregiver | `SCR-CG-01`: Đăng nhập OTP Caregiver | `UserAccount`, `CaregiverProfile`, `OtpVerification` | `accounts`<br>`caregiver_profiles`<br>`otp_verifications` | **C, R, U**: Tạo/kiểm tra mã OTP; chứng thực tài khoản; cấp token phiên đăng nhập. |
| **UC-02** | Quét QR liên kết bệnh nhân | `SCR-CG-03`: Quét mã QR liên kết | `PatientQRCode`, `CaregiverPatientLink`, `PatientProfile`, `PatientCarePlan` | `patient_qr_codes`<br>`caregiver_patient_links`<br>`patients`<br>`patient_care_plans` | **R, C**: Đọc giải mã token QR; xác thực trạng thái Active (BR4); ghi nhận quan hệ liên kết ủy quyền mới (BR5). |
| **UC-03** | Learning Path & Mini Quiz | `SCR-CG-05`: Lộ trình bài học<br>`SCR-CG-06`: Chi tiết bài học & Video<br>`SCR-CG-07`: Bộ 3 câu Mini Quiz | `TemplateLearningModule`, `TemplateQuizQuestion`, `CaregiverQuizSubmission` | `template_learning_modules`<br>`template_quiz_questions`<br>`caregiver_quiz_submissions` | **R, C**: Đọc danh sách bài học và 3 câu hỏi trắc nghiệm; lưu kết quả làm bài trắc nghiệm của Caregiver (BR8). |
| **UC-05** | Xem hướng dẫn Do & Don't | `SCR-CG-08`: Hướng dẫn Nên làm & Cần tránh | `TemplateDoDontItem` | `template_do_dont_items` | **R**: Đọc danh mục hành vi sinh hoạt theo 2 nhóm màu Xanh/Đỏ và lọc theo nhóm (BR22). |
| **UC-06** | Lịch dùng thuốc & Nhắc thuốc | `SCR-CG-09`: Lịch dùng thuốc & Xác nhận cữ | `PatientMedication`, `MedicationLog` | `patient_medications`<br>`medication_logs` | **R, C**: Đọc đơn thuốc bệnh nhân, nhận diện và lưu ý; kích hoạt đếm lùi 5 phút (BR23); tạo bản ghi xác nhận cữ thuốc. |
| **UC-07** | Xem lịch tái khám & Nhắc khám | `SCR-CG-10`: Chi tiết lịch hẹn tái khám | `PatientFollowupAppointment`, `DoctorProfile` | `patient_followup_appointments`<br>`doctor_profiles` | **R, U**: Đọc thông tin lịch hẹn tái khám; tự động cập nhật cờ đã gửi thông báo nhắc 24h & 2h (BR24). |
| **UC-08** | Nộp bảng kiểm Recovery Check | `SCR-CG-11`: Bảng kiểm phục hồi định kỳ | `TemplateRecoveryMilestone`, `TemplateRecoveryQuestion`, `RecoveryCheckSubmission`, `RecoveryCheckAnswer`, `RedFlagIncident` | `template_recovery_milestones`<br>`template_recovery_questions`<br>`recovery_check_submissions`<br>`recovery_check_answers`<br>`red_flag_incidents` | **R, C**: Đọc 3–5 câu hỏi khảo sát; lưu lượt nộp và từng câu trả lời (BR9); tự động phân loại lâm sàng (BR10); kích hoạt cơ chế leo thang quá hạn đa kênh và cảnh báo Bác sĩ nếu quên nộp (BR12); nếu có Red Flag thì tự động sinh bản ghi khẩn cấp (BR11). |
| **UC-09** | Xử lý khẩn cấp khi gặp Red Flag | `SCR-CG-12`: Màn hình Xử lý Khẩn cấp Red Flag | `TemplateRedFlag`, `RedFlagIncident`, `PatientCarePlan` | `template_red_flags`<br>`red_flag_incidents`<br>`patient_care_plans` | **R, C, U**: Đọc hướng dẫn sơ cứu tức thì và hotline cấp cứu 24/7; cập nhật cờ xác nhận đã bấm gọi điện thoại cấp cứu. |
| **UC-11** | Đăng nhập Bác sĩ | `SCR-DOC-01`: Đăng nhập Bác sĩ | `UserAccount`, `DoctorProfile` | `accounts`<br>`doctor_profiles` | **R**: Xác thực tài khoản Bác sĩ qua username/password băm; kiểm tra vai trò `DOCTOR` (BR14). |
| **UC-12.1** | Tạo mới hồ sơ bệnh nhân | `SCR-DOC-04`: Tạo mới hồ sơ BN (Form động ca mổ) | `PatientProfile` | `patients` | **C**: Lưu bản ghi bệnh nhân mới; lưu trữ cấu trúc động các trường lâm sàng tùy biến theo ca mổ trong `clinical_custom_data (JSONB)`. |
| **UC-12.2** | Xem danh sách & Tìm kiếm BN | `SCR-DOC-03`: Danh sách & Tìm kiếm BN | `PatientProfile`, `PatientCarePlan`, `CaregiverPatientLink` | `patients`<br>`patient_care_plans`<br>`caregiver_patient_links` | **R**: Truy vấn danh sách bệnh nhân kèm trạng thái Care Plan và thông tin người chăm sóc được lọc theo thời gian thực. |
| **UC-12.3** | Xem chi tiết hồ sơ bệnh nhân | `SCR-DOC-05`: Chi tiết hồ sơ bệnh nhân | `PatientProfile`, `PatientCarePlan`, `CaregiverProfile`, `CaregiverPatientLink`, `RecoveryCheckSubmission` | `patients`<br>`patient_care_plans`<br>`caregiver_profiles`<br>`caregiver_patient_links`<br>`recovery_check_submissions` | **R**: Đọc toàn diện lý lịch bệnh nhân, Kế hoạch chăm sóc hiện tại, danh sách Caregiver liên kết, lịch sử phục hồi và sự kiện Red Flag. |
| **UC-12.4** | Chỉnh sửa hồ sơ bệnh nhân | `SCR-DOC-06`: Chỉnh sửa hồ sơ BN | `PatientProfile` | `patients` | **U**: Cập nhật thông tin bệnh nhân, số điện thoại, ghi chú y khoa và các chỉ số lâm sàng tùy biến. |
| **UC-12.5** | Xóa/Lưu trữ hồ sơ bệnh nhân | `SCR-DOC-05`: Chi tiết hồ sơ BN / `SCR-DOC-06` | `PatientProfile`, `PatientCarePlan` | `patients`<br>`patient_care_plans` | **U**: Chuyển trạng thái sang `ARCHIVED` (xóa mềm); kiểm tra chặn xóa nếu đang có Care Plan Active (BR25). |
| **UC-13.1** | Tạo mới Care Plan Template | `SCR-DOC-08`: Không gian cấu hình Master Template | `CarePlanTemplate` và toàn bộ 5 thực thể `Template*` | `care_plan_templates`<br>`template_learning_modules`<br>`template_medications`<br>`template_recovery_milestones`<br>`template_red_flags`<br>`template_do_dont_items` | **C**: Khởi tạo thông tin chung và cấu hình trực tiếp 5 tab con; chỉ thực hiện lưu toàn bộ gói template ở trạng thái `DRAFT` khi hoàn tất bước cuối cùng. |
| **UC-13.2** | Xem danh sách Care Plan Template | `SCR-DOC-07`: Danh mục Care Plan Template | `CarePlanTemplate` | `care_plan_templates` | **R**: Kết xuất bảng danh sách template theo loại phẫu thuật, trạng thái hoạt động và ngày cập nhật. |
| **UC-13.3** | Xem chi tiết Care Plan Template | `SCR-DOC-09`: Chi tiết Care Plan Template | `CarePlanTemplate` và toàn bộ 5 thực thể `Template*` | `care_plan_templates`<br>Các bảng `template_*` | **R**: Kết xuất toàn bộ cấu hình chuyên môn chi tiết trên cả 5 tab thành phần con của gói mẫu. |
| **UC-13.4** | Chỉnh sửa Care Plan Template | `SCR-DOC-08`: Không gian cấu hình Master Template | `CarePlanTemplate` và toàn bộ 5 thực thể `Template*` | `care_plan_templates`<br>Các bảng `template_*` | **U, C, D**: Cho phép chỉnh sửa thông tin chung VÀ thêm/sửa/xóa trực tiếp trên 5 tab con; lưu đồng bộ khi nhấn "Lưu thay đổi". |
| **UC-13.5** | Kích hoạt / Vô hiệu hóa Template| `SCR-DOC-09`: Chi tiết Care Plan Template | `CarePlanTemplate` | `care_plan_templates` | **U**: Cập nhật trạng thái sang `ACTIVE` (nếu đủ cấu hình) hoặc `INACTIVE` (vô hiệu hóa phác đồ). |
| **UC-14.1** | Thêm bài học & Mini Quiz | `SCR-DOC-08` (Tab 1: Learning Path) | `TemplateLearningModule`, `TemplateQuizQuestion` | `template_learning_modules`<br>`template_quiz_questions` | **C**: Thêm bài học mới (tiêu đề, media) VÀ nhập trực tiếp bộ 3 câu hỏi trắc nghiệm kèm đáp án và giải thích y khoa. |
| **UC-14.2** | Xem danh sách bài học | `SCR-DOC-08` (Tab 1: Learning Path) | `TemplateLearningModule`, `TemplateQuizQuestion` | `template_learning_modules`<br>`template_quiz_questions` | **R**: Đọc danh mục bài học, phương tiện minh họa và trạng thái bộ câu hỏi Mini Quiz kèm nút xem trước. |
| **UC-14.3** | Sửa bài học & Mini Quiz | `SCR-DOC-08` (Tab 1: Learning Path) | `TemplateLearningModule`, `TemplateQuizQuestion` | `template_learning_modules`<br>`template_quiz_questions` | **U**: Cập nhật văn bản, media bài học HOẶC chỉnh sửa nội dung/đáp án/giải thích của 3 câu hỏi trắc nghiệm. |
| **UC-14.4** | Xóa bài học khỏi lộ trình | `SCR-DOC-08` (Tab 1: Learning Path) | `TemplateLearningModule`, `TemplateQuizQuestion` | `template_learning_modules`<br>`template_quiz_questions` | **D**: Xóa bài học; CSDL tự động cascade xóa 3 câu hỏi trắc nghiệm đi kèm. |
| **UC-14.5** | Sắp xếp thứ tự bài học | `SCR-DOC-08` (Tab 1: Learning Path) | `TemplateLearningModule` | `template_learning_modules` | **U**: Cập nhật chỉ số `display_order` của danh mục bài học. |
| **UC-15.1** | Thêm thuốc vào Medication Template | `SCR-DOC-08` (Tab 2: Medication Template) | `TemplateMedication` | `template_medications` | **C**: Lưu loại thuốc mới kèm chọn Loại phẫu thuật áp dụng, Mô tả nhận diện trực quan và Lưu ý lâm sàng đặc thù; hỗ trợ A1 thêm liên tiếp nhiều thuốc. |
| **UC-15.2** | Xem danh sách thuốc mẫu | `SCR-DOC-08` (Tab 2: Medication Template) | `TemplateMedication` | `template_medications` | **R**: Kết xuất danh mục thuốc mẫu theo loại phẫu thuật kèm mô tả nhận diện và lưu ý an toàn. |
| **UC-15.3** | Sửa thông tin thuốc mẫu | `SCR-DOC-08` (Tab 2: Medication Template) | `TemplateMedication` | `template_medications` | **U**: Cập nhật liều dùng, cữ uống, mô tả nhận diện thuốc hoặc lưu ý về loại thuốc. |
| **UC-15.4** | Xóa thuốc khỏi template | `SCR-DOC-08` (Tab 2: Medication Template) | `TemplateMedication` | `template_medications` | **D**: Xóa bản ghi thuốc khỏi cấu hình template. |
| **UC-16.1** | Tạo mốc & câu hỏi Recovery Check | `SCR-DOC-08` (Tab 3: Recovery Check) | `TemplateRecoveryMilestone`, `TemplateRecoveryQuestion` | `template_recovery_milestones`<br>`template_recovery_questions` | **C**: Chọn Loại phẫu thuật áp dụng, nhập mốc ngày theo dõi và thiết lập 3–5 câu hỏi khảo sát kèm điều kiện cờ cảnh báo. |
| **UC-16.2** | Xem mốc & câu hỏi Recovery Check | `SCR-DOC-08` (Tab 3: Recovery Check) | `TemplateRecoveryMilestone`, `TemplateRecoveryQuestion` | `template_recovery_milestones`<br>`template_recovery_questions` | **R**: Đọc danh sách mốc thời gian và chi tiết câu hỏi khảo sát phục hồi. |
| **UC-16.3** | Sửa mốc & câu hỏi Recovery Check | `SCR-DOC-08` (Tab 3: Recovery Check) | `TemplateRecoveryMilestone`, `TemplateRecoveryQuestion` | `template_recovery_milestones`<br>`template_recovery_questions` | **U**: Điều chỉnh mốc ngày, nội dung câu hỏi hoặc tiêu chí kích hoạt cảnh báo Red Flag. |
| **UC-16.4** | Xóa mốc / câu hỏi Recovery Check | `SCR-DOC-08` (Tab 3: Recovery Check) | `TemplateRecoveryMilestone`, `TemplateRecoveryQuestion` | `template_recovery_milestones`<br>`template_recovery_questions` | **D**: Xóa mốc theo dõi hoặc xóa câu hỏi khảo sát đơn lẻ. |
| **UC-17.1** | Thêm dấu hiệu Red Flag | `SCR-DOC-08` (Tab 4: Red Flag) | `TemplateRedFlag` | `template_red_flags` | **C**: Chọn Loại phẫu thuật áp dụng, nhập tên triệu chứng nguy hiểm, hướng dẫn sơ cứu khẩn cấp và hotline cấp cứu 24/7. |
| **UC-17.2** | Xem danh sách Red Flag | `SCR-DOC-08` (Tab 4: Red Flag) | `TemplateRedFlag` | `template_red_flags` | **R**: Đọc bảng tổng hợp các dấu hiệu cảnh báo khẩn cấp và đường dây nóng hỗ trợ. |
| **UC-17.3** | Sửa dấu hiệu Red Flag | `SCR-DOC-08` (Tab 4: Red Flag) | `TemplateRedFlag` | `template_red_flags` | **U**: Chỉnh sửa chỉ dẫn sơ cứu hoặc cập nhật số điện thoại hotline cấp cứu của bệnh viện. |
| **UC-17.4** | Xóa dấu hiệu Red Flag | `SCR-DOC-08` (Tab 4: Red Flag) | `TemplateRedFlag` | `template_red_flags` | **D**: Xóa tiêu chí cảnh báo nguy hiểm khỏi template. |
| **UC-18.1** | Thêm mục Do & Don't | `SCR-DOC-08` (Tab 5: Do & Don't) | `TemplateDoDontItem` | `template_do_dont_items` | **C**: Chọn Loại phẫu thuật áp dụng, phân loại Nên làm (Do) / Cần tránh (Don't), nhóm sinh hoạt, lý do y tế và thời gian áp dụng. |
| **UC-18.2** | Xem danh sách Do & Don't | `SCR-DOC-08` (Tab 5: Do & Don't) | `TemplateDoDontItem` | `template_do_dont_items` | **R**: Đọc danh mục chỉ dẫn chia 2 cột màu Xanh / Đỏ kèm mốc thời gian kiêng cữ. |
| **UC-18.3** | Sửa mục Do & Don't | `SCR-DOC-08` (Tab 5: Do & Don't) | `TemplateDoDontItem` | `template_do_dont_items` | **U**: Cập nhật câu từ hướng dẫn, giải thích lâm sàng hoặc thời gian áp dụng. |
| **UC-18.4** | Xóa mục Do & Don't | `SCR-DOC-08` (Tab 5: Do & Don't) | `TemplateDoDontItem` | `template_do_dont_items` | **D**: Loại bỏ mục chỉ dẫn sinh hoạt khỏi cấu hình template. |
| **UC-19** | Tạo & tùy biến Care Plan BN | `SCR-DOC-10`: Tùy biến Care Plan bệnh nhân | `PatientCarePlan`, `PatientMedication`, `PatientFollowupAppointment` | `patient_care_plans`<br>`patient_medications`<br>`patient_followup_appointments` | **C, U**: Nhân bản template sang Care Plan bệnh nhân; tùy biến đơn thuốc thực tế; lập lịch hẹn tái khám; kích hoạt trạng thái Active (BR18, BR19). |
| **UC-20** | Tạo mã QR cho bệnh nhân | `SCR-DOC-11`: Phiếu xuất viện & In mã QR | `PatientQRCode`, `PatientCarePlan` | `patient_qr_codes`<br>`patient_care_plans` | **C, U**: Tạo chuỗi token ngẫu nhiên mã hóa an toàn gắn với Care Plan Active (BR20); xuất lệnh in phiếu và đếm số lượt in. |
| **UC-21** | Theo dõi Recovery Check của BN | `SCR-DOC-12`: Bảng giám sát phục hồi & Red Flag | `RecoveryCheckSubmission`, `RecoveryCheckAnswer`, `RedFlagIncident`, `DoctorProfile` | `recovery_check_submissions`<br>`recovery_check_answers`<br>`red_flag_incidents`<br>`doctor_profiles` | **R, U**: Truy vấn các lượt nộp bảng kiểm; phân loại ưu tiên Red Flag, Cần chú ý, Quá hạn 🟠 (BR12, Default to Unsafe); cập nhật `doctor_viewed = TRUE` và ghi chú y khoa; hỗ trợ nút gọi điện thoại trực tiếp cho Caregiver khi có bất thường hoặc quá hạn. |

---

## 3. Phân Tích Độ Bao Phủ & Đánh Giá Chất Lượng (Coverage Analysis)

### 3.1 Độ bao phủ màn hình giao diện (UI Coverage)
* **Tổng số Use Case hiện hành:** 42 Use Case (bao gồm cả các Use Case con CRUD).
* **Tổng số Màn hình thiết kế:** 24 Màn hình chính thức (`SCR-CG-01` → `SCR-CG-12` cho Caregiver, `SCR-DOC-01` → `SCR-DOC-12` cho Bác sĩ).
* **Tỷ lệ bao phủ màn hình:** **100%**. Mọi Use Case đều có giao diện tương ứng với các trạng thái thành công và ngoại lệ rõ ràng.
* Không phát sinh màn hình mồ côi (mọi màn hình đều có điểm vào và điểm ra kết nối mạch lạc).

### 3.2 Độ bao phủ cơ sở dữ liệu (Database Coverage)
* **Tổng số Thực thể nghiệp vụ:** 23 Thực thể cốt lõi + 1 Thực thể mở rộng quản trị `[FUTURE / ADMIN]`.
* **Tổng số Bảng vật lý:** 24 Bảng dữ liệu quan hệ chuẩn hóa.
* **Tỷ lệ bao phủ dữ liệu:** **100%**. Mọi thao tác nhập liệu, tra cứu, chỉnh sửa và xóa của các Use Case đều được ánh xạ tương ứng vào bảng CSDL vật lý.
* Đáp ứng đầy đủ 100% các Quy tắc Nghiệp vụ Dữ liệu từ BR1 đến BR25.

### 3.3 Đánh giá sẵn sàng cho Phân hệ Quản trị viên `[FUTURE / ADMIN]`
* **Phân hệ CRUD Account của Admin:** Bảng `accounts` và `doctor_profiles` đã sẵn sàng các thuộc tính vai trò (`role = 'ADMIN'`), trạng thái (`status`), mật khẩu băm (`password_hash`), cho phép Quản trị viên tương lai thực thi phân quyền mà không làm thay đổi lược đồ dữ liệu.
* **Phân hệ CRUD Audit Log của Admin:** Bảng `audit_logs` đã được thiết kế sẵn sàng để lưu vết toàn bộ các sự kiện quan trọng (đăng nhập, tạo hồ sơ bệnh nhân, kích hoạt Care Plan, phát hành QR, sự kiện Red Flag). Khi Admin được bổ sung, phân hệ chỉ cần kết nối trực tiếp vào bảng này.

### 3.4 Bảng Tổng Hợp Ràng Buộc Tính Toàn Vẹn Dữ Liệu (Integrity Constraints Summary)

| Mã Ràng Buộc | Bảng Dữ Liệu | Loại Ràng Buộc | Diễn Giải |
|---|---|---|---|
| `idx_patient_single_active_plan` | `patient_care_plans` | Partial Unique Index | Tối đa 1 Care Plan Active/bệnh nhân (BR19) |
| `uq_template_name_surgery` | `care_plan_templates` | Unique Constraint | Không trùng tên template trong cùng loại phẫu thuật (UC-13.1 E1) |
| `uq_milestone_template_day` | `template_recovery_milestones` | Unique Constraint | Không trùng mốc ngày trong cùng template |
| `chk_days_post_op_positive` | `template_recovery_milestones` | Check Constraint | Số ngày sau mổ > 0 |
| `idx_links_caregiver_patient` | `caregiver_patient_links` | Unique Constraint | Chống liên kết trùng lặp |
| `qr_token (Unique)` | `patient_qr_codes` | Unique Constraint | Token QR duy nhất toàn hệ thống (BR20) |
| `care_plan_id (Unique)` | `patient_qr_codes` | Unique Constraint | Mỗi Care Plan chỉ có 1 mã QR (1:1) |
