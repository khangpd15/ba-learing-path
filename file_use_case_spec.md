# II. ĐẶC TẢ YÊU CẦU NGHIỆP VỤ (REQUIREMENT SPECIFICATIONS)
# DANH MỤC VÀ ĐẶC TẢ CHI TIẾT CÁC USE CASE (FILE_USE_CASE_SPEC.MD)

> **Dự án:** RemiCare Ophthalmic Post-Op Platform (Nền tảng Hướng dẫn và Giám sát Chăm sóc Hậu phẫu Nhãn khoa)  
> **Doanh nghiệp mục tiêu:** Công ty Cổ phần Tập đoàn Y khoa VISI (VISI Medical Group)  
> **Phiên bản:** V1 (Chuẩn hóa toàn diện theo US_US_V1 - Sắp xếp nghiêm ngặt theo Use Case ID từ UC-001 đến UC-028)  
> **Ngày phê duyệt:** 15/09/2026  
> **Tổng số Use Cases:** 31 Ca sử dụng (28 mã chính UC-001 đến UC-028 cùng 3 ca mở rộng UC-010b, UC-010c, UC-022b)  

---

## 1. TỔNG QUAN DANH MỤC USE CASE HỆ THỐNG

| Use Case ID | Tên Use Case | Actor chính | Actor hỗ trợ | Mục tiêu nghiệp vụ | Feature ID | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: |
| **UC-001** | Đăng ký và Đăng nhập Caregiver qua OTP | ACT-001 (Caregiver) | ACT-009 (SMS / ZNS Gateway) | Xác thực số điện thoại và định danh an toàn cho Caregiver không cần mậ... | F-001, F-003 | **P0** |
| **UC-002** | Đăng nhập Nhân viên Y tế và Xác thực 2FA | ACT-002 (Bác sĩ), ACT-003 (Điều dưỡng), ACT-004 (CSKH) | ACT-007 (System Admin) | Xác thực danh tính chuyên môn nhân viên y tế theo vai trò và cơ sở bện... | F-002, F-023 | **P0** |
| **UC-003** | Quét Mã QR Bàn Giao Liên Kết Hồ Sơ Bệnh Nhân | ACT-001 (Caregiver) | ACT-003 (Điều dưỡng), ACT-006 (Bệnh nhân) | Giải mã QR token từ phiếu xuất viện và thiết lập liên kết điện tử bảo ... | F-004, F-021 | **P0** |
| **UC-004** | Quản lý Hồ sơ Định danh Bệnh Nhân | ACT-003 (Điều dưỡng xuất viện), ACT-002 (Bác sĩ điều trị) | ACT-006 (Bệnh nhân), ACT-010 (HIS) | Tạo mới, tra cứu và quản lý thông tin lâm sàng tối thiểu của bệnh nhân... | F-005 | **P0** |
| **UC-005** | Quản lý Danh mục Mẫu Kế Hoạch Chăm Sóc | ACT-002 (Bác sĩ điều trị) | ACT-005 (Clinical Approver / GCMO) | Thiết lập, cập nhật và quản lý các gói phác đồ chuẩn hóa theo loại phẫ... | F-006 | **P0** |
| **UC-006** | Phê Duyệt Lâm Sàng Master Template | ACT-005 (Clinical Approver / GCMO) | ACT-002 (Bác sĩ điều trị) | Thẩm định chuyên môn y khoa và ban hành chính thức các phiên bản Maste... | F-007 | **P1** |
| **UC-007** | Cấu Hình Danh Mục Thuốc Mẫu Trong Template | ACT-002 (Bác sĩ điều trị) | ACT-005 (GCMO) | Cài đặt danh mục biệt dược mẫu, liều dùng, cữ dùng và khoảng cách giãn... | F-006, F-009, F-010 | **P0** |
| **UC-008** | Cấu Hình Bộ Câu Hỏi Recovery Check & Red Flag | ACT-002 (Bác sĩ điều trị) | ACT-005 (GCMO) | Thiết lập các câu hỏi khảo sát phục hồi định kỳ 3 mức (Xanh/Vàng/Đỏ) v... | F-006, F-016, F-017 | **P0** |
| **UC-009** | Cấu Hình Cẩm Nang Hướng Dẫn và Quy Tắc Sinh Hoạt trong Master Template | ACT-002 (Bác sĩ điều trị) | ACT-005 (Clinical Approver / GCMO) | Thiết lập cẩm nang 24h đầu, quy tắc nên làm/cần tránh (Do/Don't) và ng... | F-006, F-012, F-013, F-028 | **P0** |
| **UC-010** | Khởi Tạo và Cá Nhân Hóa Care Plan Bệnh Nhân | ACT-003 (Điều dưỡng xuất viện) | ACT-002 (Bác sĩ điều trị) | Nhân bản Master Template thành Care Plan thực tế cho bệnh nhân trong <... | F-008 | **P0** |
| **UC-010b** | Thực Hiện Bàn Giao Xuất Viện Tại Phòng Lưu Viện | ACT-003 (Điều dưỡng xuất viện) | ACT-001 (Caregiver), ACT-006 (Bệnh nhân) | Điều dưỡng trực tiếp kiểm tra mắt, dán khiên bảo hộ, trao phiếu xuất v... | F-008, F-022 | **P0** |
| **UC-010c** | Xác Nhận Hoàn Tất Bàn Giao Lâm Sàng | ACT-003 (Điều dưỡng xuất viện) | ACT-001 (Caregiver) | Hệ thống ghi nhận trạng thái Care Plan chuyển sang `ACTIVE` toàn diện ... | F-004, F-008 | **P0** |
| **UC-011** | Tạo và Phát Hành Mã QR Xuất Viện | ACT-003 (Điều dưỡng xuất viện) | ACT-001 (Caregiver), ACT-006 (Bệnh nhân) | Hệ thống sinh mã token mã hóa ngẫu nhiên an toàn (UUIDv4/JWT có chữ ký... | F-021 | **P0** |
| **UC-012** | In Phiếu Hướng Dẫn Xuất Viện Kèm Mã QR | ACT-003 (Điều dưỡng xuất viện) | ACT-001 (Caregiver) | Xuất lệnh in trực tiếp Phiếu xuất viện khổ chuẩn (A5/A4/decal) có chứa... | F-022 | **P0** |
| **UC-013** | Cấp Lại hoặc Thu Hồi Mã QR Bàn Giao | ACT-003 (Điều dưỡng), ACT-002 (Bác sĩ) | ACT-001 (Caregiver) | Tạo mã QR mới thay thế khi bị mất phiếu hoặc khi Bác sĩ đổi phác đồ th... | F-021, F-022 | **P0** |
| **UC-014** | Xem Lịch Dùng Thuốc và Hướng Dẫn Nhỏ Mắt | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Hiển thị cữ thuốc trong ngày theo dòng thời gian (Sáng, Trưa, Chiều, T... | F-009, F-011 | **P0** |
| **UC-015** | Xác Nhận Dùng Thuốc và Kích Hoạt Bộ Đếm Giãn Cách | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Ghi nhận cữ thuốc hoàn thành kèm timestamp đồng bộ; tự động đếm lùi 5–... | F-009, F-010 | **P0** |
| **UC-016** | Xem Cẩm Nang 24h Đầu và Bảng Nên Làm / Cần Tránh | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Tra cứu tức thì các hành động cấp thiết trong 24h đầu sau mổ và danh m... | F-012, F-013 | **P0** |
| **UC-017** | Xem Lộ Trình Học Viện Caregiver - Infographic Tĩnh | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Học tập kiến thức chăm sóc mắt qua các infographic trực quan tinh gọn ... | F-014 | **P1** |
| **UC-018** | Xem Lịch Tái Khám và Nhận Thông Báo Nhắc Hẹn | ACT-001 (Caregiver) | ACT-009 (SMS / ZNS Gateway), ACT-004 (CSKH) | Theo dõi 5 mốc tái khám chuẩn VISI (Day 1, 7, Month 1, 3, 6) và nhận t... | F-015, F-027 | **P0** |
| **UC-019** | Thực Hiện Khảo Sát Đánh Giá Phục Hồi Định Kỳ | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân), ACT-002 (Bác sĩ) | Trả lời 3–5 câu hỏi sàng lọc định kỳ mỗi sáng trong 7 ngày đầu để hệ t... | F-016 | **P0** |
| **UC-020** | Kích Hoạt Xử Lý Biến Chứng Báo Động Đỏ | ACT-001 (Caregiver) | ACT-004 (CSKH), ACT-008 (Cấp cứu ngoại viện) | Chuyển giao diện khẩn cấp toàn màn hình, cung cấp nút gọi 1 chạm đến H... | F-017 | **P0** |
| **UC-021** | Giám Sát Dashboard Phục Hồi Bệnh Nhân Tập Trung | ACT-004 (CSKH / Medical Monitor), ACT-003 (Điều dưỡng) | ACT-002 (Bác sĩ điều trị) | Theo dõi danh sách toàn bộ bệnh nhân của cơ sở theo trạng thái tuân th... | F-018 | **P0** |
| **UC-022** | Tiếp Nhận, Phân Loại và Điều Phối Cảnh Báo Red Flag | ACT-004 (CSKH / Medical Monitor) | ACT-002 (Bác sĩ trực), ACT-001 (Caregiver) | Tiếp nhận ca cảnh báo đỏ, chuyển trạng thái xử lý, gọi điện can thiệp ... | F-019 | **P0** |
| **UC-022b** | Tự Động Leo Thang Cảnh Báo Red Flag Chưa Xử Lý | ACT-007 (System / Admin) | ACT-004 (CSKH), ACT-002 (Bác sĩ trực) | Tự động phát chuông cấp độ 2 và gửi tin nhắn khẩn cấp lên Bác sĩ trực/... | F-019 | **P0** |
| **UC-023** | Ghi Nhận Nhật Ký Cuộc Gọi và Can Thiệp Lâm Sàng | ACT-004 (CSKH), ACT-003 (Điều dưỡng) | ACT-002 (Bác sĩ điều trị) | Ghi nhận chi tiết kết quả cuộc gọi tư vấn, lời dặn y tế và trạng thái ... | F-020 | **P0** |
| **UC-024** | Tra Cứu Tình Huống Chăm Sóc Khẩn Cấp - FAQ Lâm Sàng | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Tra cứu nhanh chỉ dẫn chuẩn y khoa theo tình huống thường gặp tại nhà ... | F-028 | **P1** |
| **UC-025** | Kích Hoạt Chế Độ Trợ Năng Nhãn Khoa | ACT-006 (Bệnh nhân), ACT-001 (Caregiver) | Không có | Chuyển giao diện sang chữ lớn (≥18pt), tương phản cao High Contrast và... | F-026 | **P1** |
| **UC-026** | Quản Lý Tài Khoản Nhân Viên và Phân Quyền Cơ Sở | ACT-007 (Quản trị viên hệ thống) | ACT-002, ACT-003, ACT-004 | Khởi tạo tài khoản và phân quyền truy cập nghiêm ngặt theo vai trò và ... | F-023 | **P0** |
| **UC-027** | Tra Cứu Nhật Ký Kiểm Toán Hệ Thống | ACT-007 (Quản trị viên hệ thống) | Ban Giám Đốc (CEO, COO) | Truy vấn và kết xuất nhật ký thao tác lâm sàng phục vụ kiểm tra an toà... | F-024 | **P1** |
| **UC-028** | Kết Xuất Báo Cáo Vận Hành và Chỉ Số Tuân Thủ KPI | Ban Giám Đốc (CEO, COO) | ACT-007 (System Admin) | Tổng hợp các chỉ số KPIs: tỷ lệ kích hoạt QR (mục tiêu ≥85%), tỷ lệ tu... | F-025 | **P1** |

---

## 2. ĐẶC TẢ CHI TIẾT TỪNG USE CASE (UC-001 ĐẾN UC-028)

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
| **A1: Đổi mật khẩu lần đầu** | 1 | Nhân viên đăng nhập bằng mật khẩu khởi tạo | Hệ thống bắt buộc nhân viên đổi mật khẩu mới có độ phức tạp cao trước khi truy cập chức năng. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Sai thông tin đăng nhập hoặc 2FA** | 1 | Nhập sai mật khẩu hoặc mã 2FA | Hệ thống báo lỗi và ghi nhận số lần thất bại; khóa tài khoản sau 5 lần sai liên tiếp. |
| **E2: Tài khoản bị khóa / hết hạn** | 1 | Nhân viên đã nghỉ việc hoặc bị đình chỉ | Hệ thống từ chối truy cập và yêu cầu liên hệ IT Bệnh viện. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR14 (Kiểm soát quyền truy cập nhân viên), BR15 (Phân tách dữ liệu theo cơ sở bệnh viện) | | |

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

### UC-004: Quản lý Hồ sơ Định danh Bệnh Nhân (Post-Op Patient Profile)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-004** | | |
| **Tên Use Case (Use Case Name)** | Quản lý Hồ sơ Định danh Bệnh Nhân (Post-Op Patient Profile) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-003 (Điều dưỡng xuất viện), ACT-002 (Bác sĩ điều trị) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-006 (Bệnh nhân), ACT-010 (HIS) | | |
| **Tính năng liên quan (Features)** | F-005 | | |
| **Mô tả tóm tắt (Brief Description)** | Điều dưỡng hoặc Bác sĩ nhập và quản lý thông tin bệnh nhân trước khi xuất viện: Mã BN, Họ tên viết tắt, Năm sinh, Mắt can thiệp (MP/MT), Loại phẫu thuật (Phaco/SILK), Bác sĩ mổ và Cơ sở điều trị. | | |
| **Mục tiêu (Goal)** | Tạo mới, tra cứu và quản lý thông tin lâm sàng tối thiểu của bệnh nhân mổ mắt trong <30 giây, tuân thủ bảo mật Nghị định 13/2023/NĐ-CP. | | |
| **Tác nhân kích hoạt (Trigger)** | Điều dưỡng tiếp nhận bệnh nhân tại phòng lưu viện sau khi hoàn thành ca phẫu thuật mắt. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Điều dưỡng đã đăng nhập vào hệ thống bệnh trạm.<br>2. Bệnh nhân đã hoàn thành ca mổ mắt tại phòng phẫu thuật. | | |
| **Điều kiện sau (Post-conditions)** | 1. Bản ghi bệnh nhân được lưu vào bảng `patients`.<br>2. Sẵn sàng cho bước áp dụng Master Template (UC-010). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Điều dưỡng mở form 'Tạo hồ sơ bệnh nhân mới' (SCR-DOC-04) | Hệ thống hiển thị form nhập liệu tinh gọn với các trường tối thiểu. |
| | 2 | Điều dưỡng nhập Mã bệnh nhân HIS, Họ tên, Năm sinh, chọn Mắt mổ (MP/MT/2M), Loại phẫu thuật (Phaco/SILK), Bác sĩ phẫu thuật | Hệ thống kiểm tra định dạng và tính hợp lệ dữ liệu (BR16). Tự động lưu họ tên dưới dạng viết tắt bảo mật theo NĐ 13 (ví dụ 'Trần V. B.'). |
| | 3 | Điều dưỡng nhấn 'Lưu hồ sơ' | Hệ thống lưu bản ghi bệnh nhân, tự động gắn mã cơ sở bệnh viện và chuyển sang màn hình Khởi tạo Care Plan. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Đồng bộ từ HIS (Phase 2 - ACT-010)** | 1 | Hệ thống nhận thông tin ca mổ qua API HIS | Tự động điền trước thông tin hành chính, Điều dưỡng chỉ cần kiểm tra xác nhận trong 5 giây. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Trùng mã bệnh nhân** | 1 | Nhập mã bệnh nhân đã tồn tại trong cơ sở | Hệ thống cảnh báo trùng lặp và cho phép chọn mở hồ sơ hiện hữu để cập nhật. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR16 (Tính toàn vẹn dữ liệu lâm sàng), BR17 (Quy tắc bảo mật định danh bệnh nhân NĐ 13) | | |

---

### UC-005: Quản lý Danh mục Mẫu Kế Hoạch Chăm Sóc (Care Plan Master Templates)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-005** | | |
| **Tên Use Case (Use Case Name)** | Quản lý Danh mục Mẫu Kế Hoạch Chăm Sóc (Care Plan Master Templates) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-002 (Bác sĩ điều trị) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-005 (Clinical Approver / GCMO) | | |
| **Tính năng liên quan (Features)** | F-006 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ chuyên khoa thiết lập khung phác đồ mẫu gồm các cấu phần chuyên môn chuẩn hóa dùng chung toàn chuỗi 5 bệnh viện VISI. | | |
| **Mục tiêu (Goal)** | Thiết lập, cập nhật và quản lý các gói phác đồ chuẩn hóa theo loại phẫu thuật (Phaco tiêu chuẩn, Laser SILK/ELITA), quản lý phiên bản (v1.0, v1.1). | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ mở menu Quản lý Care Plan Template trên portal. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Bác sĩ có thẩm quyền chuyên môn đã đăng nhập.<br>2. Có phác đồ chuẩn được Hội đồng Chuyên môn VISI phê chuẩn. | | |
| **Điều kiện sau (Post-conditions)** | 1. Bản ghi Master Template được tạo ở trạng thái `DRAFT` hoặc `PENDING_APPROVAL`.<br>2. Sẵn sàng cấu hình các tab con (UC-007, UC-008, UC-009). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn 'Tạo Master Template mới' (SCR-DOC-08) | Hệ thống mở giao diện không gian cấu hình 5 tab. |
| | 2 | Bác sĩ nhập Tên template, Loại phẫu thuật, Mô tả lâm sàng, Số phiên bản (ví dụ: v1.0) | Hệ thống khởi tạo thực thể `care_plan_templates` ở trạng thái `DRAFT` (BR19). |
| | 3 | Bác sĩ cấu hình các thành phần con và nhấn 'Lưu phác đồ mẫu' | Hệ thống lưu toàn diện gói template và hiển thị trạng thái hiện tại. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Nhân bản từ phiên bản cũ** | 1 | Bác sĩ chọn nhân bản Template v1.0 để nâng cấp v1.1 | Hệ thống sao chép toàn bộ cấu hình con sang bản ghi mới, cho phép chỉnh sửa nhanh. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thiếu cấu phần bắt buộc** | 1 | Template chưa có thuốc hoặc chưa có Do/Don't | Hệ thống cảnh báo không thể gửi phê duyệt cho đến khi hoàn tất đủ 5 cấu phần. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR18 (Tính độc lập dữ liệu phác đồ), BR19 (Quy trình vòng đời Master Template) | | |

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

### UC-007: Cấu Hình Danh Mục Thuốc Mẫu Trong Template (Template Medication Configuration)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-007** | | |
| **Tên Use Case (Use Case Name)** | Cấu Hình Danh Mục Thuốc Mẫu Trong Template (Template Medication Configuration) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-002 (Bác sĩ điều trị) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-005 (GCMO) | | |
| **Tính năng liên quan (Features)** | F-006, F-009, F-010 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ thiết lập danh mục thuốc trong Master Template (Tab 2) bao gồm tên biệt dược, số giọt, các cữ Sáng/Trưa/Chiều/Tối, hình ảnh vỏ lọ và thông số đệm buffer timer. | | |
| **Mục tiêu (Goal)** | Cài đặt danh mục biệt dược mẫu, liều dùng, cữ dùng và khoảng cách giãn cách đệm mặc định (5–10 phút) giữa các thuốc nhỏ mắt. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ chọn Tab 2: Danh mục thuốc trong không gian cấu hình template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Master Template đang ở trạng thái DRAFT.<br>2. Bác sĩ nắm rõ đơn thuốc chuẩn theo loại phẫu thuật. | | |
| **Điều kiện sau (Post-conditions)** | 1. Các bản ghi thuốc được lưu vào `template_medications`.<br>2. Sẵn sàng nhân bản khi bệnh nhân xuất viện. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ bấm 'Thêm thuốc mới vào mẫu' (SCR-DOC-08 Tab 2) | Hệ thống mở form nhập thông tin thuốc. |
| | 2 | Bác sĩ nhập: Tên biệt dược (ví dụ: Cravit 0.5%), Số giọt (1 giọt), Cữ dùng (Sáng, Chiều), Mắt áp dụng mặc định, Khoảng cách đếm ngược giãn cách (10 phút hoặc 5 phút), Tải ảnh vỏ lọ nhận diện | Hệ thống xác thực dữ liệu và thêm thuốc vào danh sách hiển thị của template (BR20, BR23). |
| | 3 | Bác sĩ nhấn 'Lưu danh mục thuốc' | Hệ thống cập nhật các bản ghi trong `template_medications`. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Cấu hình thuốc mỡ / gel tra mắt** | 1 | Bác sĩ cấu hình thuốc dạng gel (tra mắt cuối cùng) | Hệ thống gán thứ tự ưu tiên tra thuốc: Dung dịch nước -> Hỗn dịch -> Gel/Mỡ tra mắt. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thời gian giãn cách không hợp lệ** | 1 | Nhập thời gian đệm <5 phút hoặc >30 phút | Hệ thống cảnh báo tham số giãn cách chuẩn nhãn khoa phải từ 5 đến 10 phút. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR20 (Quy tắc định danh thuốc), BR23 (Ràng buộc thời gian giãn cách đệm 5-10 phút) | | |

---

### UC-008: Cấu Hình Bộ Câu Hỏi Recovery Check & Red Flag (Template Recovery & Alert Rules)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-008** | | |
| **Tên Use Case (Use Case Name)** | Cấu Hình Bộ Câu Hỏi Recovery Check & Red Flag (Template Recovery & Alert Rules) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-002 (Bác sĩ điều trị) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-005 (GCMO) | | |
| **Tính năng liên quan (Features)** | F-006, F-016, F-017 | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ cấu hình bộ câu hỏi ngắn (3-5 câu) xuất hiện vào các mốc thời gian (mỗi sáng trong 7 ngày đầu, Day 14, Day 30) và thiết lập ngưỡng báo động đỏ khẩn cấp. | | |
| **Mục tiêu (Goal)** | Thiết lập các câu hỏi khảo sát phục hồi định kỳ 3 mức (Xanh/Vàng/Đỏ) và điều kiện kích hoạt cảnh báo nguy cấp Red Flag kèm số hotline 0395 151 151. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ chọn Tab 3 (Recovery Check) và Tab 4 (Red Flag) trong không gian cấu hình template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Template đang mở ở chế độ chỉnh sửa.<br>2. Bộ tiêu chí lâm sàng đã được Hội đồng Y khoa phê duyệt. | | |
| **Điều kiện sau (Post-conditions)** | 1. Các mốc và câu hỏi được lưu vào `template_recovery_milestones`, `template_recovery_questions`, `template_red_flags`. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ thiết lập các mốc thời gian khảo sát (Day 1 đến Day 7, Day 14, Day 30) | Hệ thống tạo các mốc theo dõi trong template. |
| | 2 | Bác sĩ nhập 3–5 câu hỏi khảo sát kèm các lựa chọn đáp án và gắn cờ phân loại (Xanh: bình thường, Vàng: chú ý, Đỏ: Red Flag) | Hệ thống lưu cấu trúc câu hỏi và điều kiện phân loại tự động (BR9, BR10). |
| | 3 | Bác sĩ cấu hình thông điệp hướng dẫn khẩn cấp và đường dây nóng VISI 0395 151 151 | Hệ thống lưu cấu hình hành động cấp cứu Red Flag. |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Tùy biến câu hỏi theo loại phẫu thuật** | 1 | Ca mổ SILK/Laser cần khảo sát cộm xốn cọ xát vạt; ca Phaco khảo sát áp lực nhãn cầu | Hệ thống cho phép gắn bộ câu hỏi chuyên biệt theo từng loại mổ. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thiếu phương án cảnh báo Đỏ** | 1 | Bộ câu hỏi không có bất kỳ tiêu chí kích hoạt Red Flag nào | Hệ thống từ chối lưu và yêu cầu phải có ít nhất 1 tiêu chí phát hiện biến chứng nguy cấp. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR9 (Bắt buộc hoàn thành khảo sát), BR10 (Phân loại 3 mức Xanh/Vàng/Đỏ), BR11 (Kích hoạt cấp cứu khẩn cấp) | | |

---

### UC-009: Cấu Hình Cẩm Nang Hướng Dẫn và Quy Tắc Sinh Hoạt trong Master Template (Guidelines & FAQ Config)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-009** | | |
| **Tên Use Case (Use Case Name)** | Cấu Hình Cẩm Nang Hướng Dẫn và Quy Tắc Sinh Hoạt trong Master Template (Guidelines & FAQ Config) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-002 (Bác sĩ điều trị) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-005 (Clinical Approver / GCMO) | | |
| **Tính năng liên quan (Features)** | F-006, F-012, F-013, F-028 | | |
| **Mô tả tóm tắt (Brief Description)** | Ca sử dụng mới được bổ sung hoàn chỉnh trong US_US_V1 nhằm giúp Bác sĩ cấu hình toàn bộ nội dung giáo dục bệnh nhân, cẩm nang 24h sống còn, bảng 2 cột Nên làm & Cần tránh, và menu câu hỏi FAQ giải đáp thắc mắc thường gặp tại nhà. | | |
| **Mục tiêu (Goal)** | Thiết lập cẩm nang 24h đầu, quy tắc nên làm/cần tránh (Do/Don't) và ngân hàng tình huống FAQ lâm sàng gắn liền với từng loại phẫu thuật mắt. | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ chọn Tab 5: Cẩm nang & Quy tắc sinh hoạt trên giao diện cấu hình template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Master Template đang ở trạng thái DRAFT.<br>2. Nội dung hướng dẫn đã qua rà soát y khoa. | | |
| **Điều kiện sau (Post-conditions)** | 1. Các mục Do & Don't được lưu vào `template_do_dont_items`.<br>2. Ngân hàng cẩm nang 24h và FAQ được liên kết với template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ cấu hình Cẩm nang 24 giờ đầu: danh mục việc cấp thiết (đeo kính bảo hộ, tư thế nằm, dán khiên mắt khi ngủ, kiêng cúi đầu) | Hệ thống lưu các chỉ dẫn sống còn 24h đầu. |
| | 2 | Bác sĩ nhập danh mục Nên làm (Cột Xanh) và Cần tránh (Cột Đỏ): phân loại theo vệ sinh, vận động, ăn uống, giấc ngủ kèm lý do y khoa | Hệ thống lưu vào `template_do_dont_items` và hiển thị xem trước trực quan 2 cột (BR22). |
| | 3 | Bác sĩ cấu hình ngân hàng tình huống FAQ lâm sàng (Dính nước vào mắt, Quên nhỏ thuốc, Cộm xốn mắt, Ngứa mắt) kèm lời khuyên xử lý chuẩn VISI | Hệ thống lưu ngân hàng FAQ gắn với loại phẫu thuật (F-028). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Tải nội dung từ thư viện chuẩn VISI** | 1 | Bác sĩ chọn nạp sẵn bộ quy tắc Do/Don't chuẩn của Bệnh viện Mắt VISI | Hệ thống tự động điền đầy đủ danh mục, Bác sĩ chỉ cần kiểm tra nhanh. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Nội dung hướng dẫn chứa thuật ngữ cấm** | 1 | Nhập khuyến nghị tự mua thuốc ngoài | Hệ thống cảnh báo và yêu cầu sửa đổi tuân thủ phác đồ VISI. |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR7 (Nguồn gốc nội dung y khoa), BR22 (Hiển thị trực quan 2 cột màu Do & Don't) | | |

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
| **Mô tả tóm tắt (Brief Description)** | Điều dưỡng tại quầy lưu viện chọn hồ sơ bệnh nhân và áp dụng Master Template tương ứng với loại mổ để sinh ra Kế hoạch chăm sóc độc lập cho bệnh nhân. | | |
| **Mục tiêu (Goal)** | Nhân bản Master Template thành Care Plan thực tế cho bệnh nhân trong <30 giây (Clinical Setup 3 bước); cho phép Bác sĩ điều chỉnh liều nếu cần. | | |
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

### UC-026: Quản Lý Tài Khoản Nhân Viên và Phân Quyền Cơ Sở (Multi-Branch Staff RBAC)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | **UC-026** | | |
| **Tên Use Case (Use Case Name)** | Quản Lý Tài Khoản Nhân Viên và Phân Quyền Cơ Sở (Multi-Branch Staff RBAC) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA — VISI Medical Group |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 15/09/2026 (Phiên bản V1 Chuẩn hóa) |
| **Tác nhân chính (Primary Actor)** | ACT-007 (Quản trị viên hệ thống) | | |
| **Tác nhân hỗ trợ (Supporting Actors)**| ACT-002, ACT-003, ACT-004 | | |
| **Tính năng liên quan (Features)** | F-023 | | |
| **Mô tả tóm tắt (Brief Description)** | Quản trị viên quản lý danh mục tài khoản nhân viên y tế, gán quyền và đảm bảo phân tách dữ liệu an toàn giữa các bệnh viện trong chuỗi. | | |
| **Mục tiêu (Goal)** | Khởi tạo tài khoản và phân quyền truy cập nghiêm ngặt theo vai trò và cơ sở trực thuộc chuỗi 5 bệnh viện VISI. | | |
| **Tác nhân kích hoạt (Trigger)** | Quản trị viên truy cập module Phân quyền & Nhân sự. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Quản trị viên hệ thống đã xác thực tài khoản cấp cao. | | |
| **Điều kiện sau (Post-conditions)** | Tài khoản nhân viên được tạo mới, cập nhật hoặc vô hiệu hóa. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Quản trị viên chọn 'Thêm tài khoản nhân viên mới' | Mở form nhập thông tin định danh nhân sự. |
| | 2 | Quản trị viên nhập thông tin, chọn vai trò (`DOCTOR`, `NURSE`, `CSKH`, `GCMO`) và chọn Chi nhánh bệnh viện (VISI Thủ Đức, VISI Bình Dương, v.v.) | Hệ thống kiểm tra tính duy nhất của email/mã nhân viên. |
| | 3 | Quản trị viên nhấn 'Kích hoạt tài khoản' | Hệ thống cấp tài khoản, gửi thông tin mật khẩu tạm và kích hoạt chính sách phân quyền cơ sở (BR15). |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Khóa tài khoản nhân viên chuyển công tác** | 1 | Nhân viên nghỉ việc hoặc điều chuyển | Quản trị viên chuyển trạng thái tài khoản sang `DEACTIVATED`, thu hồi toàn bộ quyền truy cập tức thì. |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| Không có | - | Không phát sinh ngoại lệ đặc thù | - |
| **Mức độ ưu tiên (Priority)** | **P0** | | |
| **Quy tắc nghiệp vụ (Business Rules)** | BR14 (Phân quyền RBAC), BR15 (Phân tách phạm vi dữ liệu theo cơ sở bệnh viện) | | |

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