# VISI MEDICAL GROUP — REMICARE OPHTHALMIC POST-OP PLATFORM
# TÀI LIỆU ĐẶC TẢ CHI TIẾT CÂU CHUYỆN NGƯỜI DÙNG (USER STORIES SPECIFICATION)

> **Dự án:** RemiCare Ophthalmic Post-Op Platform (Nền tảng Hướng dẫn và Giám sát Chăm sóc Hậu phẫu Nhãn khoa)  
> **Doanh nghiệp mục tiêu:** Công ty Cổ phần Tập đoàn Y khoa VISI (VISI Medical Group)  
> **Phiên bản:** V1 (Đồng bộ toàn diện theo tài liệu chuẩn US_US_V1 - Đã sắp xếp theo ID tăng dần từ US-001 đến US-035)  
> **Ngày phê duyệt:** 15/09/2026  
> **Tiêu chuẩn kỹ thuật:** INVEST Criteria, Gherkin Acceptance Criteria, Nghị định 13/2023/NĐ-CP  

---

## 1. TỔNG QUAN VÀ MA TRẬN CÂU CHUYỆN NGƯỜI DÙNG (MASTER USER STORY MATRIX)

Tài liệu này đặc tả chi tiết toàn bộ **35 User Stories** của nền tảng RemiCare, được liên kết chặt chẽ với danh mục **28 Features (F-001 đến F-028)**, **10 Tác nhân (ACT-001 đến ACT-010)** và **31 Use Cases (UC-001 đến UC-028)** đã được chuẩn hóa trong [US_US_V1.md](file:///d:/EXE101/DOC_UC_DB_PROJECT_EXE101/US_US_V1.md).

### 1.1 Nguyên Tắc Thiết Kế Nghiệp Vụ & Phân Định Ranh Giới 7 Vai Trò:

Hệ thống xác lập ma trận chức năng, thẩm quyền và giới hạn nhiệm vụ cụ thể cho 7 nhóm người dùng:
1. **Giám đốc Bệnh viện (Hospital Director / GCMO):** Xem báo cáo vận hành, xử lý trường hợp cần đánh giá chuyên môn cấp cao (Escalated Review), theo dõi cảnh báo y tế nghiêm trọng, xem hồ sơ bệnh nhân theo quyền được cấp, quản lý/giám sát Bác sĩ & Điều dưỡng, phê duyệt Care Plan quan trọng, theo dõi chất lượng chăm sóc, xem lịch sử hoạt động và báo cáo tổng hợp. *(Lưu ý: Không mặc định xem mọi hồ sơ; chỉ xem theo quyền được cấp).*
2. **Bác sĩ (Ophthalmic Doctor / Surgeon):** CRUD Medical Record, sao chép (clone) Master Template để tạo và cập nhật Care Plan cho từng bệnh nhân cụ thể tùy theo thể trạng của họ, tùy biến liều lượng thuốc, tạo/quản lý Learning Path, thiết lập Recovery Check, Red Flags, Do/Don't, lịch Follow-up/Tái khám, xuất bản hoặc gửi Care Plan phê duyệt, theo dõi tiến trình chăm sóc, xem kết quả khảo sát & cảnh báo, xử lý ca chuyển cấp.
3. **Điều dưỡng (Discharge / Clinical Nurse):** Xem Medical Record theo quyền được cấp, xem Care Plan, chỉ nhập thông tin cơ bản của bệnh nhân (<30s), tạo Care Plan ban đầu theo mẫu Bác sĩ duyệt (tuyệt đối không được chỉnh sửa liều lượng thuốc), xuất QR liên kết, theo dõi nhiệm vụ chăm sóc, cập nhật tình trạng, ghi nhận Recovery Check, theo dõi dấu hiệu bất thường, gửi cảnh báo đến Bác sĩ, cấp lại/vô hiệu hóa QR, hỗ trợ hướng dẫn Caregiver, theo dõi danh sách Caregiver liên kết. *(Lưu ý: Bác sĩ quyết định chuyên môn y tế; Bác sĩ có quyền sao chép template để tùy biến liều lượng theo thể trạng bệnh nhân; Điều dưỡng chỉ nhập thông tin cơ bản của bệnh nhân và áp dụng mẫu đã duyệt, tuyệt đối không được chỉnh sửa liều lượng thuốc, Red Flags hoặc hướng dẫn điều trị).*
4. **Chăm sóc khách hàng (Customer Care - CSKH):** Tra cứu thông tin tài khoản, hỗ trợ đăng ký/đăng nhập, hỗ trợ liên kết QR, ghi nhận phản hồi/khiếu nại, theo dõi trạng thái yêu cầu, hướng dẫn sử dụng, chuyển vấn đề kỹ thuật cho IT/Admin, chuyển vấn đề y tế cho Điều dưỡng/Bác sĩ. *(Lưu ý: CSKH chỉ được xem thông tin tài khoản và trạng thái hỗ trợ cần thiết; không được tự ý truy cập toàn bộ hồ sơ y tế chuyên sâu).*
5. **Caregiver (Người Chăm Sóc / Thân Nhân):** Đăng ký/đăng nhập OTP, quét QR liên kết, xem danh sách bệnh nhân đang chăm sóc, xem Care Plan được cấp quyền, xem Learning Path, học cẩm nang, làm Recovery Check, xác nhận đã cho uống thuốc, xem lịch thuốc, nhận thông báo giờ uống thuốc, xem bộ đếm ngược 5-10p, xem lịch tái khám, nhận cảnh báo Red Flag, gửi phản hồi/ghi chú, cập nhật tình trạng chăm sóc, theo dõi lịch sử chăm sóc, quản lý tối đa 03 Caregiver cho mỗi bệnh nhân, xem lịch sử các bệnh nhân đã từng chăm sóc.
6. **Admin (Quản Trị Viên Hệ Thống):** CRUD tài khoản người dùng, quản lý vai trò và quyền truy cập RBAC, khóa/mở khóa tài khoản, CRUD Audit Log, xem lịch sử hoạt động hệ thống, quản lý cấu hình hệ thống an toàn dữ liệu NĐ 13/2023.
7. **Care Recipient (Bệnh Nhân / Người Thụ Hưởng Chăm Sóc):** Đăng ký/đăng nhập OTP, xem thông tin cá nhân & hồ sơ y tế của bản thân, xem Care Plan được cấp quyền, xem danh sách thuốc & hướng dẫn, nhận thông báo giờ uống thuốc, xác nhận bản thân đã uống thuốc/nhỏ mắt, xem bộ đếm ngược thời gian dùng thuốc nhỏ mắt (5-10 phút chống rửa trôi thuốc), xem lịch tái khám & nhận nhắc hẹn, xem Learning Path, học hướng dẫn chăm sóc, làm Recovery Check, cập nhật sức khỏe hằng ngày, gửi ghi chú triệu chứng bất thường, nhận cảnh báo Red Flag, gửi phản hồi/hỗ trợ, xem lịch sử chăm sóc của bản thân, xem tiến độ hoàn thành, cập nhật thông tin cá nhân trong phạm vi cho phép, tải ảnh/tài liệu y tế của bản thân nếu được cấp quyền.

---

### 1.2 Nguyên Tắc Kỹ Thuật & Lâm Sàng Cốt Lõi:
1. **Loại bỏ Mini Quiz trong MVP (F-014, US-022):** Chuyển dịch toàn bộ kiến thức sang dạng Infographic tĩnh tinh gọn nhằm tránh gây phiền toái, quá tải thông tin cho người nhà bệnh nhân. Các bảng dữ liệu liên quan đến Quiz được bảo lưu ở trạng thái mở rộng Phase 2.
2. **Chống nhầm lẫn và rửa trôi thuốc mắt (F-009, F-010, US-015 đến US-018):** Tự động kích hoạt bộ đếm thời gian giãn cách 5–10 phút (Drop Interval Buffer Timer) ngay sau khi nhỏ lọ thuốc thứ nhất, tạm khóa nút xác nhận lọ thứ hai.
3. **Cấp cứu Red Flag 1 chạm (F-017, US-024):** Chuyển giao diện cảnh báo đỏ toàn màn hình, cung cấp nút gọi tức thì tới Hotline VISI `0395 151 151` và tự động leo thang (Escalate) sau 15 phút nếu chưa được can thiệp.

---

### 1.3 Bảng Tổng Hợp 35 User Stories

| Story ID | Feature ID | Actor | Tóm tắt mục tiêu User Story | Priority | Ca sử dụng liên quan |
| :--- | :--- | :--- | :--- | :---: | :--- |
| **US-001** | F-001 | Caregiver | Đăng ký & Đăng nhập Người Chăm Sóc qua OTP | **P0** | UC-001 |
| **US-002** | F-001 | Caregiver | Duy trì phiên đăng nhập an toàn (Keep-Alive Session) | **P0** | UC-001 |
| **US-003** | F-002 | Điều dưỡng xuất viện | Đăng nhập Nhân viên Y tế Tập trung (Điều dưỡng) | **P0** | UC-002, UC-026 |
| **US-004** | F-002 | Bác sĩ điều trị | Đăng nhập Bác sĩ Điều trị kết hợp Xác thực 2 lớp (2FA) | **P0** | UC-002, UC-026 |
| **US-005** | F-004 | Caregiver | Quét mã QR liên kết hồ sơ bệnh nhân | **P0** | UC-003 |
| **US-006** | F-004 | Caregiver | Hỗ trợ tối đa 3 Caregiver cùng liên kết 1 bệnh nhân | **P0** | UC-003 |
| **US-007** | F-005 | Điều dưỡng xuất viện | Nhập thông tin tóm tắt bệnh nhân trước xuất viện | **P0** | UC-004 |
| **US-008** | F-021 | Điều dưỡng xuất viện | Tự động sinh mã QR token mã hóa an toàn khi kích hoạt Care Plan | **P0** | UC-011 |
| **US-009** | F-022 | Điều dưỡng xuất viện | In trực tiếp Phiếu xuất viện kèm mã QR tại quầy lưu viện | **P0** | UC-012 |
| **US-010** | F-022 | Điều dưỡng xuất viện | Cấp lại mã QR mới và vô hiệu hóa mã cũ | **P0** | UC-013 |
| **US-011** | F-006 | Bác sĩ điều trị | Thiết lập và quản trị Master Template chuẩn hóa | **P0** | UC-005 |
| **US-012** | F-007 | Người phê duyệt lâm sàng / GCMO | Thẩm định và ký duyệt điện tử Master Template | **P1** | UC-006 |
| **US-013** | F-008 | Điều dưỡng xuất viện | Áp dụng nhanh Master Template cho bệnh nhân trong 3 bước (<30s) | **P0** | UC-010, UC-010b |
| **US-014** | F-008 | Bác sĩ điều trị | Tùy biến liều lượng Care Plan bệnh nhân mà không đổi Master Template gốc | **P0** | UC-010 |
| **US-015** | F-009 | Caregiver | Xem danh sách cữ thuốc theo khung giờ kèm ảnh nhận diện | **P0** | UC-014 |
| **US-016** | F-009 | Caregiver | Bấm nút xác nhận đã dùng thuốc lưu timestamp đồng bộ | **P0** | UC-015 |
| **US-017** | F-010 | Caregiver | Tự động kích hoạt bộ đếm lùi giãn cách 5–10 phút chống rửa trôi thuốc | **P0** | UC-015 |
| **US-018** | F-010 | Caregiver | Phát âm thanh và rung báo khi hết thời gian đếm lùi giãn cách | **P0** | UC-015 |
| **US-019** | F-011 | Caregiver | Xem video / ảnh minh họa kỹ thuật kéo mi dưới nhỏ thuốc chuẩn | **P0** | UC-014 |
| **US-020** | F-012 | Caregiver | Xem cẩm nang hành động đặc biệt cho 24 giờ đầu sau mổ mắt | **P0** | UC-016 |
| **US-021** | F-013 | Caregiver | Xem bảng Những điều Nên làm (Xanh) & Cần tránh (Đỏ) trực quan | **P0** | UC-016 |
| **US-022** | F-014 | Caregiver | Xem các Infographic kiến thức hồi phục mắt tinh gọn trong Học viện Caregiver | **P1** | UC-017 |
| **US-023** | F-016 | Caregiver | Trả lời khảo sát Recovery Check 3–5 câu mỗi sáng trong 7 ngày đầu | **P0** | UC-019 |
| **US-024** | F-017 | Caregiver | Màn hình Cảnh báo Đỏ & Nút gọi 1 chạm Hotline VISI 0395 151 151 | **P0** | UC-020 |
| **US-025** | F-016 | Caregiver | Nhận thông báo phản hồi an tâm khi kết quả Recovery Check bình thường | **P0** | UC-019 |
| **US-026** | F-018 | Nhân viên CSKH / Điều dưỡng trực | Theo dõi Dashboard hiển thị trạng thái toàn bộ bệnh nhân chi nhánh | **P0** | UC-021 |
| **US-027** | F-019 | Nhân viên CSKH / Medical Monitor | Bật chuông cảnh báo và đưa ca Red Flag lên đầu, can thiệp <5 phút | **P0** | UC-022 |
| **US-028** | F-019 | Nhân viên CSKH / Hệ thống | Tự động leo thang (Escalate) ca Red Flag lên Bác sĩ trực sau 15 phút | **P0** | UC-022b |
| **US-029** | F-020 | Nhân viên CSKH | Ghi nhận nhật ký cuộc gọi và hướng can thiệp trực tiếp trên Dashboard | **P0** | UC-023 |
| **US-030** | F-018 | Bác sĩ điều trị | Xem lịch sử Recovery Check và biểu đồ triệu chứng khi bệnh nhân tái khám | **P0** | UC-021 |
| **US-031** | F-015 | Caregiver | Xem lộ trình 5 mốc tái khám chuẩn VISI và nhận thông báo nhắc trước 24h | **P0** | UC-018 |
| **US-032** | F-015 | Nhân viên CSKH | Xem danh sách bệnh nhân có lịch tái khám ngày mai chưa xác nhận | **P1** | UC-018 |
| **US-033** | F-023 | Quản trị viên hệ thống | Khởi tạo tài khoản nhân viên và phân quyền theo chi nhánh bệnh viện | **P0** | UC-026 |
| **US-034** | F-024 | Quản trị viên hệ thống | Tra cứu nhật ký kiểm toán (Audit Trail) phục vụ pháp lý và thanh tra | **P1** | UC-027 |
| **US-035** | F-026 | Bệnh nhân hậu phẫu | Bật chế độ Trợ năng nhãn khoa (Chữ to, tương phản cao, Audio Guide) | **P1** | UC-025 |

---

## 2. ĐẶC TẢ CHI TIẾT TỪNG USER STORY (US-001 ĐẾN US-035)

### US-001 — Đăng ký & Đăng nhập Người Chăm Sóc qua OTP

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-001** |
| **Tính năng liên quan (Feature ID)** | **F-001** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-001** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR1, BR2, BR3** |
| **Giá trị nghiệp vụ (Business Value)** | Định danh người chăm sóc hợp pháp; loại bỏ rào cản ghi nhớ mật khẩu cho người lớn tuổi. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn đăng ký và đăng nhập vào ứng dụng bằng Số điện thoại cá nhân và nhận mã xác thực OTP qua SMS/Zalo, để có thể nhanh chóng truy cập hệ thống chăm sóc mà không cần ghi nhớ mật khẩu phức tạp."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-001.1:** Given Caregiver mở ứng dụng RemiCare lần đầu hoặc chưa đăng nhập, When họ nhập số điện thoại di động (định dạng 10 chữ số chuẩn Việt Nam), Then hệ thống kiểm tra tính hợp lệ của số điện thoại (BR1).
- **AC-US-001.2:** When số điện thoại hợp lệ và nhấn 'Tiếp tục', Then hệ thống gửi mã OTP gồm 6 chữ số ngẫu nhiên qua tin nhắn SMS Brandname 'VISI GROUP' hoặc Zalo ZNS trong vòng dưới 10 giây; mã có hiệu lực 5 phút (BR2).
- **AC-US-001.3:** When Caregiver nhập đúng mã OTP 6 chữ số, Then hệ thống xác thực thành công, tạo phiên làm việc bảo mật và chuyển tiếp vào màn hình chính hoặc màn hình Quét QR nếu phiên được mở từ liên kết QR (BR3).
- **AC-US-001.4:** When Caregiver nhập sai mã OTP quá 5 lần liên tiếp, Then hệ thống khóa tạm thời yêu cầu gửi OTP trong 15 phút và hiển thị thông báo hỗ trợ.
- **AC-US-001.5:** Tuân thủ Nghị định 13/2023/NĐ-CP về bảo vệ dữ liệu cá nhân: chỉ thu thập số điện thoại tối thiểu, không yêu cầu CCCD hay mật khẩu.

---

### US-002 — Duy trì phiên đăng nhập an toàn (Keep-Alive Session)

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-002** |
| **Tính năng liên quan (Feature ID)** | **F-001** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-001** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR3** |
| **Giá trị nghiệp vụ (Business Value)** | Tối ưu hóa trải nghiệm người dùng; đảm bảo mở hướng dẫn nhỏ thuốc tức thì trong tình huống khẩn cấp mà không bị cản trở bởi màn hình đăng nhập. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn duy trì phiên đăng nhập an toàn trên thiết bị di động cá nhân, để không phải nhập lại OTP nhiều lần trong ngày mỗi khi đến cữ tra thuốc cho người bệnh."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-002.1:** Given Caregiver đã xác thực OTP thành công trên trình duyệt/PWA của thiết bị di động cá nhân, When họ đóng và mở lại ứng dụng trong vòng 30 ngày, Then hệ thống tự động nhận diện phiên làm việc hợp lệ qua JWT/Refresh Token được lưu trữ an toàn mà không yêu cầu nhập lại OTP.
- **AC-US-002.2:** When Caregiver chủ động nhấn 'Đăng xuất' trong menu Cài đặt, Then hệ thống hủy phiên đăng nhập, thu hồi token trên máy chủ và đưa về màn hình Đăng nhập.
- **AC-US-002.3:** When phát hiện phiên làm việc có dấu hiệu bất thường từ địa chỉ IP lạ hoặc token hết hạn sau 30 ngày, Then hệ thống yêu cầu xác thực lại OTP một lần nữa để bảo đảm an toàn.

---

### US-003 — Đăng nhập Nhân viên Y tế Tập trung (Điều dưỡng)

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-003** |
| **Tính năng liên quan (Feature ID)** | **F-002** |
| **Tác nhân thực hiện (Actor)** | **ACT-003 (Điều dưỡng xuất viện)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-002, UC-026** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR14, BR15** |
| **Giá trị nghiệp vụ (Business Value)** | Bảo đảm tính pháp lý và truy vết trách nhiệm lâm sàng; ngăn ngừa truy cập trái phép; tăng tốc quy trình xuất viện. |

> **Tuyên bố User Story:**  
> *"Là một Điều dưỡng xuất viện, tôi muốn đăng nhập vào hệ thống bệnh trạm bằng tài khoản định danh do bệnh viện cấp phát tập trung, để thao tác kích hoạt Kế hoạch chăm sóc và in phiếu xuất viện theo đúng thẩm quyền cơ sở."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-003.1:** Given Điều dưỡng truy cập cổng web nội bộ `portal.remicare.visi.vn`, When nhập Tên đăng nhập/Mã nhân viên và Mật khẩu do IT Bệnh viện cấp, Then hệ thống kiểm tra thông tin xác thực.
- **AC-US-003.2:** When đăng nhập thành công, Then hệ thống xác định cơ sở làm việc trực thuộc (ví dụ: Bệnh viện Mắt VISI Thủ Đức) và hiển thị giao diện trạm điều dưỡng (SCR-DOC-03, SCR-DOC-10, SCR-DOC-11).
- **AC-US-003.3:** Hệ thống chặn hoàn toàn chức năng tự đăng ký tự do từ ngoài internet; mọi tài khoản phải do System Admin (ACT-007) khởi tạo và phân quyền (F-023).

---

### US-004 — Đăng nhập Bác sĩ Điều trị kết hợp Xác thực 2 lớp (2FA)

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-004** |
| **Tính năng liên quan (Feature ID)** | **F-002** |
| **Tác nhân thực hiện (Actor)** | **ACT-002 (Bác sĩ điều trị)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-002, UC-026** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR14, BR15** |
| **Giá trị nghiệp vụ (Business Value)** | Bảo vệ dữ liệu bệnh án nhạy cảm theo Luật Khám bệnh, chữa bệnh và Nghị định 13/2023/NĐ-CP; bảo đảm chuẩn mực an toàn thông tin y tế. |

> **Tuyên bố User Story:**  
> *"Là một Bác sĩ điều trị, tôi muốn đăng nhập bảo mật vào hệ thống bằng tài khoản chuyên môn kết hợp xác thực 2 lớp (2FA), để truy cập danh sách bệnh nhân mổ của mình và phê duyệt các điều chỉnh phác đồ đặc biệt."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-004.1:** Given Bác sĩ truy cập cổng web nội bộ, When nhập chính xác Tên đăng nhập và Mật khẩu, Then hệ thống yêu cầu bước xác thực thứ hai (2FA) qua ứng dụng Authenticator hoặc mã OTP gửi về số điện thoại Bác sĩ.
- **AC-US-004.2:** When mã 2FA chính xác, Then hệ thống cấp phiên làm việc với vai trò Bác sĩ Chuyên khoa (`DOCTOR`), cho phép truy cập danh sách bệnh nhân phẫu thuật, cấu hình Master Template và xem chi tiết hồ sơ bệnh án.
- **AC-US-004.3:** When nhập sai mã 2FA quá 3 lần, Then hệ thống khóa tài khoản 30 phút và ghi nhật ký cảnh báo an ninh (Audit Trail).

---

### US-005 — Quét mã QR liên kết hồ sơ bệnh nhân

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-005** |
| **Tính năng liên quan (Feature ID)** | **F-004** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-003** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR4, BR5** |
| **Giá trị nghiệp vụ (Business Value)** | Số hóa bàn giao 100%; loại bỏ hoàn toàn việc nhập liệu thủ công mã hồ sơ dễ sai sót; gắn kết Caregiver với bệnh nhân trong <5 giây. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn sử dụng camera điện thoại quét mã QR in trên Phiếu xuất viện của bệnh nhân, để tự động liên kết tài khoản của mình với Kế hoạch chăm sóc mắt của người thân chỉ trong vài giây."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-005.1:** Given Caregiver đã đăng nhập ứng dụng RemiCare, When nhấn nút 'Quét mã QR' và hướng camera vào mã QR in trên Phiếu xuất viện, Then hệ thống quét và giải mã token bảo mật trong <2 giây.
- **AC-US-005.2:** When token hợp lệ và thuộc Kế hoạch chăm sóc đang hoạt động (`ACTIVE`), Then hệ thống hiển thị màn hình tóm tắt thông tin bệnh nhân: Họ tên viết tắt (bảo mật), Năm sinh, Mắt phẫu thuật (MP/MT), Bác sĩ mổ, Cơ sở điều trị.
- **AC-US-005.3:** When Caregiver nhấn 'Xác nhận liên kết', Then hệ thống tạo bản ghi liên kết `caregiver_patient_links` và tự động chuyển hướng vào Trang chủ Care Plan của bệnh nhân đó.
- **AC-US-005.4:** When quét mã QR đã hết hạn, bị thu hồi hoặc không đúng chuẩn hệ thống, Then ứng dụng thông báo rõ: 'Mã QR không hợp lệ hoặc đã bị vô hiệu hóa. Vui lòng liên hệ điều dưỡng để được cấp lại.' (BR4).

---

### US-006 — Hỗ trợ tối đa 3 Caregiver cùng liên kết 1 bệnh nhân

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-006** |
| **Tính năng liên quan (Feature ID)** | **F-004** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-003** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR5** |
| **Giá trị nghiệp vụ (Business Value)** | Phản ánh đúng thực tế chăm sóc gia đình Việt Nam; tránh tình trạng gián đoạn dùng thuốc khi người nhà chính bận việc. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn hệ thống cho phép tối đa 3 thành viên trong gia đình cùng quét mã QR để liên kết vào hồ sơ một bệnh nhân, để cả nhà có thể thay phiên nhau nhỏ thuốc và theo dõi hồi phục."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-006.1:** Given một bệnh nhân đã có Caregiver thứ nhất liên kết, When thành viên gia đình thứ hai hoặc thứ ba dùng tài khoản của mình quét cùng mã QR trên phiếu xuất viện, Then hệ thống cho phép liên kết thành công và ghi nhận vai trò người chăm sóc đồng hành.
- **AC-US-006.2:** When đã đủ 3 Caregiver liên kết vào hồ sơ bệnh nhân này, nếu có người thứ tư quét mã, Then hệ thống thông báo: 'Hồ sơ bệnh nhân đã đạt giới hạn tối đa 3 người chăm sóc liên kết. Vui lòng liên hệ người chăm sóc chính để quản lý danh sách.' (BR5).
- **AC-US-006.3:** Mọi thao tác xác nhận đã nhỏ thuốc hoặc nộp bảng kiểm phục hồi từ bất kỳ Caregiver nào trong nhóm 3 người đều được đồng bộ tức thì theo thời gian thực tới 2 người còn lại.

---

### US-007 — Nhập thông tin tóm tắt bệnh nhân trước xuất viện

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-007** |
| **Tính năng liên quan (Feature ID)** | **F-005** |
| **Tác nhân thực hiện (Actor)** | **ACT-003 (Điều dưỡng xuất viện)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-004** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR16, BR17** |
| **Giá trị nghiệp vụ (Business Value)** | Tạo lập hồ sơ theo dõi lâm sàng độc lập; chuẩn bị dữ liệu xuất bản mã QR bàn giao trong thời gian <30 giây. |

> **Tuyên bố User Story:**  
> *"Là một Điều dưỡng xuất viện, tôi muốn nhập thông tin tóm tắt của bệnh nhân (Mã bệnh nhân, Họ tên viết tắt, Năm sinh, Mắt phẫu thuật: MP/MT) trước khi xuất viện, để thiết lập thực thể phục hồi hậu phẫu cá nhân hóa."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-007.1:** Given Điều dưỡng tại phòng lưu viện mở màn hình Tạo hồ sơ bệnh nhân (SCR-DOC-04), When nhập các trường thông tin tối thiểu: Mã bệnh nhân HIS (ví dụ: `BN-2026-0891`), Họ tên bệnh nhân (hệ thống tự động lưu dạng viết tắt bảo mật theo NĐ 13 như 'Nguyễn V. A.'), Năm sinh, Mắt phẫu thuật (`MP` - Mắt phải, `MT` - Mắt trái, `2M` - Cả hai mắt), Loại phẫu thuật (`Phaco tiêu chuẩn` hoặc `Laser xóa cận SILK/ELITA`), Bác sĩ phẫu thuật chính, Then form xác thực tính đầy đủ.
- **AC-US-007.2:** Hệ thống kiểm tra trùng lặp mã bệnh nhân tại cơ sở điều trị (BR16).
- **AC-US-007.3:** Thời gian nhập liệu trung bình được tối ưu <30 giây với các trường chọn nhanh và gợi ý thông minh.

---

### US-008 — Tự động sinh mã QR token mã hóa an toàn khi kích hoạt Care Plan

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-008** |
| **Tính năng liên quan (Feature ID)** | **F-021** |
| **Tác nhân thực hiện (Actor)** | **ACT-003 (Điều dưỡng xuất viện)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-011** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR4** |
| **Giá trị nghiệp vụ (Business Value)** | Tự động hóa quy trình cấp mã lâm sàng; bảo mật dữ liệu bệnh án trong chuỗi token ngẫu nhiên. |

> **Tuyên bố User Story:**  
> *"Là một Điều dưỡng xuất viện, tôi muốn hệ thống tự động sinh mã QR mã hóa an toàn ngay khi Kế hoạch chăm sóc được kích hoạt, để sẵn sàng in ra phiếu hướng dẫn xuất viện cho người nhà."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-008.1:** Given Kế hoạch chăm sóc (Care Plan) của bệnh nhân được Điều dưỡng bấm nút 'Kích hoạt', When hệ thống xử lý giao dịch, Then tự động sinh chuỗi mã hóa token ngẫu nhiên (UUIDv4 kết hợp chữ ký số HMAC-SHA256) gắn với `plan_id`.
- **AC-US-008.2:** Token QR không chứa thông tin y tế nhạy cảm dạng văn bản thô; chỉ có thể được giải mã thông qua API RemiCare có xác thực phiên.
- **AC-US-008.3:** Mã QR được lưu trữ với trạng thái ban đầu là `ISSUED` (Đã cấp) kèm timestamp và định danh điều dưỡng thực hiện.

---

### US-009 — In trực tiếp Phiếu xuất viện kèm mã QR tại quầy lưu viện

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-009** |
| **Tính năng liên quan (Feature ID)** | **F-022** |
| **Tác nhân thực hiện (Actor)** | **ACT-003 (Điều dưỡng xuất viện)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-012** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR4** |
| **Giá trị nghiệp vụ (Business Value)** | Tích hợp nhịp nhàng vào luồng vận hành phòng lưu viện hiện tại; giảm thời gian dặn dò miệng từ 15 phút xuống dưới 2 phút. |

> **Tuyên bố User Story:**  
> *"Là một Điều dưỡng xuất viện, tôi muốn bấm in trực tiếp Phiếu xuất viện tích hợp mã QR và thông tin tóm tắt bằng máy in tại quầy lưu viện, để trao tận tay người nhà kèm lời dặn dò trước khi rời viện."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-009.1:** Given Điều dưỡng đã kích hoạt Care Plan cho bệnh nhân, When nhấn nút 'In Phiếu Xuất Viện' trên màn hình SCR-DOC-11, Then hệ thống mở lệnh in trực tiếp ra máy in laser/nhiệt tại quầy trong <3 giây.
- **AC-US-009.2:** Bản in xuất viện theo mẫu chuẩn VISI (khổ A5 ngang hoặc A4): chứa logo VISI Medical Group, Tên bệnh nhân viết tắt, Năm sinh, Mắt phẫu thuật, Bác sĩ mổ, Mã QR độ phân giải cao (≥300 DPI, kích thước ≥3x3 cm), Hướng dẫn quét 3 bước bằng camera Zalo/Điện thoại, và Số Hotline cấp cứu 0395 151 151.
- **AC-US-009.3:** Điều dưỡng trao phiếu cho người nhà, chứng kiến người nhà quét thử thành công trước khi hoàn tất thủ tục rời viện.

---

### US-010 — Cấp lại mã QR mới và vô hiệu hóa mã cũ

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-010** |
| **Tính năng liên quan (Feature ID)** | **F-022** |
| **Tác nhân thực hiện (Actor)** | **ACT-003 (Điều dưỡng xuất viện)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-013** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR4, BR18** |
| **Giá trị nghiệp vụ (Business Value)** | Kiểm soát tính toàn vẹn và duy nhất của phác đồ điều trị; ngăn ngừa việc nhỏ thuốc theo đơn cũ đã bị hủy. |

> **Tuyên bố User Story:**  
> *"Là một Điều dưỡng xuất viện, tôi muốn cấp lại mã QR mới và vô hiệu hóa mã cũ khi người nhà làm mất phiếu xuất viện hoặc khi bác sĩ thay đổi đơn thuốc, để đảm bảo người nhà luôn tiếp cận đúng phác đồ mới nhất."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-010.1:** Given một bệnh nhân đã có mã QR phát hành trước đó, When người nhà báo mất phiếu hoặc Bác sĩ chỉnh sửa đơn thuốc hậu phẫu, Then Điều dưỡng chọn hồ sơ bệnh nhân và bấm nút 'Cấp lại mã QR' (SCR-DOC-11).
- **AC-US-010.2:** When Điều dưỡng nhập lý do cấp lại và nhấn xác nhận, Then hệ thống ngay lập tức chuyển trạng thái mã QR cũ thành `REVOKED` (Đã thu hồi), sinh chuỗi token QR mới, cập nhật Care Plan và in phiếu mới.
- **AC-US-010.3:** Khi mã QR cũ bị quét lại sau đó, hệ thống từ chối truy cập và báo lỗi: 'Mã QR này đã bị thu hồi do được cấp mới. Vui lòng quét mã QR trên phiếu mới nhất.' (BR4).
- **AC-US-010.4:** Các Caregiver đã liên kết trước đó nhận được thông báo cập nhật tự động trong ứng dụng mà không cần liên kết lại từ đầu.

---

### US-011 — Thiết lập và quản trị Master Template chuẩn hóa

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-011** |
| **Tính năng liên quan (Feature ID)** | **F-006** |
| **Tác nhân thực hiện (Actor)** | **ACT-002 (Bác sĩ điều trị)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-005** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR18, BR19** |
| **Giá trị nghiệp vụ (Business Value)** | Chuẩn hóa chất lượng chuyên môn chuỗi; bảo vệ thương hiệu và kết quả kỹ thuật của máy laser ELITA và máy Phaco. |

> **Tuyên bố User Story:**  
> *"Là một Bác sĩ điều trị, tôi muốn thiết lập và duy trì các Mẫu kế hoạch chăm sóc (Care Plan Master Template) chuẩn hóa theo từng loại phẫu thuật (Phaco, SILK...), để tái sử dụng thống nhất trên toàn chuỗi 5 bệnh viện VISI."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-011.1:** Given Bác sĩ có thẩm quyền truy cập màn hình Cấu hình Master Template (SCR-DOC-08), When tạo mới một template, Then hệ thống yêu cầu nhập: Tên mẫu (ví dụ: 'Phác đồ Hậu Phẫu Phaco Tiêu Chuẩn v1.0'), Loại phẫu thuật áp dụng (`Phaco` hoặc `Laser SILK`), Mô tả lâm sàng, Phiên bản.
- **AC-US-011.2:** Bác sĩ có thể cấu hình trọn gói 5 tab thành phần: (1) Cẩm nang & Infographic Learning Path, (2) Danh mục thuốc mẫu & thời gian giãn cách đệm, (3) Mốc khảo sát Recovery Check, (4) Tiêu chí cảnh báo Red Flag & Hotline, (5) Danh mục Do & Don't.
- **AC-US-011.3:** Template sau khi soạn thảo được lưu ở trạng thái `DRAFT` (Bản nháp) hoặc gửi phê duyệt `PENDING_APPROVAL` (BR19).

---

### US-012 — Thẩm định và ký duyệt điện tử Master Template

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-012** |
| **Tính năng liên quan (Feature ID)** | **F-007** |
| **Tác nhân thực hiện (Actor)** | **ACT-005 (Người phê duyệt lâm sàng / GCMO)** |
| **Mức độ ưu tiên (Priority)** | **P1** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-006** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR19** |
| **Giá trị nghiệp vụ (Business Value)** | Thiết lập chốt chặn an toàn lâm sàng cao nhất; kiểm soát rủi ro tai biến y khoa trên toàn hệ thống tập đoàn. |

> **Tuyên bố User Story:**  
> *"Là một Giám đốc Chuyên môn (GCMO), tôi muốn thẩm định và ký duyệt điện tử các phiên bản Master Template trước khi ban hành, để đảm bảo mọi nội dung y khoa tuân thủ phác đồ của Hội đồng Chuyên môn VISI."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-012.1:** Given Master Template ở trạng thái `PENDING_APPROVAL`, When Giám đốc Chuyên môn (BS.CKII Trần Bá Kiền) đăng nhập hệ thống, Then danh sách các template chờ duyệt được hiển thị ưu tiên tại mục Thẩm định lâm sàng.
- **AC-US-012.2:** When GCMO xem xét toàn bộ các thông số thuốc, liều lượng, khoảng cách đệm 5-10 phút và tiêu chí Red Flag, Then có thể chọn 'Phê duyệt' (Approve) hoặc 'Yêu cầu chỉnh sửa' (Reject with comments).
- **AC-US-012.3:** When bấm 'Phê duyệt', hệ thống yêu cầu ký duyệt điện tử, chuyển trạng thái template sang `ACTIVE` và phát hành áp dụng đồng loạt cho 5 chi nhánh bệnh viện VISI (BR19).

---

### US-013 — Áp dụng nhanh Master Template cho bệnh nhân trong 3 bước (<30s)

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-013** |
| **Tính năng liên quan (Feature ID)** | **F-008** |
| **Tác nhân thực hiện (Actor)** | **ACT-003 (Điều dưỡng xuất viện)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-010, UC-010b** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR16, BR18** |
| **Giá trị nghiệp vụ (Business Value)** | Tối ưu hóa năng suất lao động của điều dưỡng; biến phần mềm thành công cụ hỗ trợ tiện lợi thay vì gánh nặng nhập liệu. |

> **Tuyên bố User Story:**  
> *"Là một Điều dưỡng xuất viện, tôi muốn chọn Master Template và áp dụng nhanh cho bệnh nhân chỉ trong 3 bước (<30 giây), để hoàn tất thủ tục bàn giao xuất viện mà không làm ùn tắc phòng lưu viện."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-013.1:** Given Điều dưỡng mở màn hình Khởi tạo Care Plan bệnh nhân (SCR-DOC-10), When thực hiện quy trình 3 bước Clinical Setup: Bước 1: Chọn bệnh nhân từ danh sách mổ trong ngày; Bước 2: Chọn Master Template tương ứng với loại phẫu thuật đã mổ (hệ thống tự động đổ đầy danh mục thuốc, lịch tái khám); Bước 3: Nhấn nút 'Kích hoạt & Sinh QR', Then hệ thống hoàn tất quá trình nhân bản phác đồ trong <1 giây.
- **AC-US-013.2:** Toàn bộ dữ liệu thuốc, giờ nhỏ, mốc tái khám được nhân bản sang các bảng thực thi độc lập của bệnh nhân.
- **AC-US-013.3:** Tổng thời gian thao tác của điều dưỡng từ lúc mở màn hình đến khi xuất bản in không quá 30 giây.

---

### US-014 — Tùy biến liều lượng Care Plan bệnh nhân mà không đổi Master Template gốc

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-014** |
| **Tính năng liên quan (Feature ID)** | **F-008** |
| **Tác nhân thực hiện (Actor)** | **ACT-002 (Bác sĩ điều trị)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-010** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR18** |
| **Giá trị nghiệp vụ (Business Value)** | Đảm bảo tính linh hoạt y khoa (Clinical Flexibility) song song với tính chuẩn hóa hệ thống (BR18 Data Independence). |

> **Tuyên bố User Story:**  
> *"Là một Bác sĩ điều trị, tôi muốn tùy chỉnh liều lượng hoặc thêm bớt loại thuốc trong Care Plan của bệnh nhân mà không làm biến đổi Master Template gốc, để đáp ứng đặc điểm bệnh lý riêng của từng ca mổ phức tạp."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-014.1:** Given bệnh nhân có tiền sử glôcôm hoặc viêm màng bồ đào cần phác đồ thuốc riêng, When Bác sĩ mở Care Plan cá nhân của bệnh nhân (SCR-DOC-10), Then Bác sĩ có thể thêm thuốc mới, tăng/giảm số giọt, hoặc thay đổi số cữ nhỏ trong ngày.
- **AC-US-014.2:** When Bác sĩ lưu thay đổi, hệ thống chỉ cập nhật vào bản ghi `patient_medication_schedules` của bệnh nhân đó.
- **AC-US-014.3:** Master Template gốc (`care_plan_templates`) được bảo toàn tuyệt đối, không bị biến đổi bởi bất kỳ tùy biến cá nhân hóa nào (Quy tắc tính độc lập dữ liệu BR18).

---

### US-015 — Xem danh sách cữ thuốc theo khung giờ kèm ảnh nhận diện

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-015** |
| **Tính năng liên quan (Feature ID)** | **F-009** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-014** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR20, BR23** |
| **Giá trị nghiệp vụ (Business Value)** | Giải quyết triệt để khoảng trống nhầm lẫn thuốc (BN-001); bảo đảm bệnh nhân nhận đúng thuốc, đúng liều lượng chỉ định. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn xem danh sách các cữ thuốc trong ngày được phân chia rõ ràng theo khung giờ (Sáng, Trưa, Chiều, Tối) kèm hình ảnh lọ thuốc và số giọt cần nhỏ, để không bị nhầm lẫn giữa 3–5 loại thuốc mắt khác nhau."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-015.1:** Given Caregiver mở tab 'Lịch Thuốc' (SCR-CG-09), When xem giao diện, Then các cữ thuốc trong ngày được gom nhóm trực quan theo 4 khung giờ: Sáng (07:00), Trưa (11:30), Chiều (16:30), Tối (20:00).
- **AC-US-015.2:** Mỗi thẻ thuốc hiển thị đầy đủ thông tin: Tên biệt dược, nồng độ (ví dụ: Cravit 0.5%, Vigamox 0.5%, Sanlein 0.1%), Hình ảnh thực tế của vỏ hộp/lọ thuốc để nhận diện màu sắc nắp lọ, Số giọt cần nhỏ (ví dụ: 'Nhỏ 1 giọt'), Mắt áp dụng (Mắt Phải / Mắt Trái / Cả 2 mắt), và Lưu ý đặc biệt (ví dụ: 'Lắc đều trước khi dùng').
- **AC-US-015.3:** Cữ thuốc hiện tại gần nhất được làm nổi bật với viền màu xanh dương và đếm lùi thời gian đến cữ.

---

### US-016 — Bấm nút xác nhận đã dùng thuốc lưu timestamp đồng bộ

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-016** |
| **Tính năng liên quan (Feature ID)** | **F-009** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-015** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR20, BR21** |
| **Giá trị nghiệp vụ (Business Value)** | Ngăn ngừa việc nhỏ trùng liều hoặc người nhà khác tưởng chưa nhỏ lại nhỏ tiếp gây quá liều độc cho giác mạc. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn bấm nút 'Xác nhận đã nhỏ thuốc' sau mỗi lần cho bệnh nhân dùng thuốc, để hệ thống lưu vết và đánh dấu cữ thuốc đã hoàn thành trong ngày."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-016.1:** Given Caregiver vừa nhỏ thuốc xong cho bệnh nhân, When nhấn nút 'Xác nhận đã nhỏ' trên thẻ thuốc tương ứng, Then hệ thống ghi nhận thời điểm thao tác (timestamp chính xác đến giây), chuyển trạng thái thuốc sang `TAKEN` và hiển thị dấu tích xanh hoàn thành.
- **AC-US-016.2:** Lịch sử dùng thuốc được lưu vào `patient_medication_logs` (BR21).
- **AC-US-016.3:** Trạng thái hoàn thành được đồng bộ tức thì lên thiết bị của các Caregiver khác trong gia đình và đẩy về Dashboard theo dõi của Bệnh viện VISI.

---

### US-017 — Tự động kích hoạt bộ đếm lùi giãn cách 5–10 phút chống rửa trôi thuốc

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-017** |
| **Tính năng liên quan (Feature ID)** | **F-010** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-015** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR23** |
| **Giá trị nghiệp vụ (Business Value)** | Triệt tiêu hoàn toàn hiện tượng rửa trôi thuốc (washout effect); bảo toàn sinh khả dụng dược lý nhãn khoa tối đa. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn hệ thống tự động kích hoạt đồng hồ đếm ngược 10 phút (hoặc 5 phút theo chỉ định) sau khi nhỏ lọ thứ nhất, để tôi biết chính xác khi nào mắt đã hấp thu xong và an toàn để nhỏ lọ thứ hai."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-017.1:** Given một cữ dùng có từ 2 loại thuốc nhỏ mắt trở lên (ví dụ: Kháng sinh + Nước mắt nhân tạo), When Caregiver bấm xác nhận đã nhỏ lọ thứ nhất, Then hệ thống tự động kích hoạt Bộ đếm thời gian giãn cách đệm (Drop Interval Buffer Timer) với thời gian mặc định là 10 phút (hoặc 5 phút tùy phác đồ cấu hình) (BR23).
- **AC-US-017.2:** Đồng hồ hiển thị đếm lùi từng giây dạng vòng tròn hoạt ảnh trực quan.
- **AC-US-017.3:** Trong thời gian đếm ngược, nút xác nhận của lọ thuốc thứ hai bị tạm khóa (disable / xám mờ), kèm thông báo cảnh báo y khoa: 'Vui lòng chờ mắt hấp thu thuốc thứ nhất để tránh rửa trôi thuốc.' (BR23).

---

### US-018 — Phát âm thanh và rung báo khi hết thời gian đếm lùi giãn cách

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-018** |
| **Tính năng liên quan (Feature ID)** | **F-010** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-015** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR23** |
| **Giá trị nghiệp vụ (Business Value)** | Giảm căng thẳng và tiện lợi hóa việc chăm sóc tại nhà cho người thân bận rộn làm việc gia đình. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn hệ thống phát âm thanh thông báo và rung khi đồng hồ đếm ngược giãn cách kết thúc, để tôi không phải đứng canh điện thoại liên tục mà vẫn nhỏ thuốc tiếp theo đúng lúc."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-018.1:** Given đồng hồ đếm lùi giãn cách đang chạy, When thời gian đếm ngược về đúng `00:00`, Then thiết bị phát chuông âm thanh thông báo dịu nhẹ và rung chuông 3 nhịp.
- **AC-US-018.2:** Nút xác nhận của lọ thuốc tiếp theo lập tức mở khóa và chuyển sang trạng thái sẵn sàng nhỏ.
- **AC-US-018.3:** Ứng dụng hiển thị thông báo biểu ngữ: 'Đã hết thời gian chờ! Bây giờ bạn có thể nhỏ tiếp lọ [Tên lọ thuốc 2].'

---

### US-019 — Xem video / ảnh minh họa kỹ thuật kéo mi dưới nhỏ thuốc chuẩn

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-019** |
| **Tính năng liên quan (Feature ID)** | **F-011** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-014** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR7** |
| **Giá trị nghiệp vụ (Business Value)** | Loại bỏ nguy cơ nhiễm khuẩn chéo và chấn thương cơ học mép rạch gây viêm mủ nội nhãn (Endophthalmitis). |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn xem video ngắn hoặc hình ảnh minh họa cách kéo mi dưới và khoảng cách giữ đầu lọ thuốc, để tôi thực hiện thao tác nhỏ thuốc chuẩn xác mà không để đầu lọ chạm vào mắt bệnh nhân."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-019.1:** Given Caregiver đang xem thẻ thuốc hoặc mục Hướng dẫn nhỏ mắt (SCR-CG-06), When bấm nút 'Xem kỹ thuật nhỏ mắt chuẩn', Then hệ thống hiển thị video ngắn (15–30 giây) hoặc ảnh minh họa trực quan từng bước.
- **AC-US-019.2:** Nội dung nhấn mạnh 3 nguyên tắc vô trùng tối thượng của VISI: (1) Rửa sạch tay bằng xà phòng trước khi nhỏ; (2) Dùng ngón tay kéo nhẹ mi dưới tạo túi cùng kết mạc và nhỏ 1 giọt vào túi cùng; (3) Giữ đầu lọ cách mắt 2–3 cm, TUYỆT ĐỐI KHÔNG chạm đầu lọ vào lông mi, mi mắt hay giác mạc; (4) Nhắm mắt nhẹ nhàng trong 1–2 phút, không chớp dồn dập (BR7).
- **AC-US-019.3:** Có nút bấm phát lại video hoặc xem chế độ hình ảnh tĩnh nếu đường truyền mạng chậm.

---

### US-020 — Xem cẩm nang hành động đặc biệt cho 24 giờ đầu sau mổ mắt

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-020** |
| **Tính năng liên quan (Feature ID)** | **F-012** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-016** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR7, BR22** |
| **Giá trị nghiệp vụ (Business Value)** | Bảo vệ mép mổ trong 'thời điểm vàng' 24h đầu; chống bung vạt giác mạc hoặc xuất huyết tiền phòng. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn xem cẩm nang hành động đặc biệt cho 24 giờ đầu sau mổ, để biết cách bảo vệ mắt mổ ngay khi vừa từ viện về nhà trong khoảng thời gian nhạy cảm nhất."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-020.1:** Given bệnh nhân mới xuất viện về nhà trong ngày đầu tiên (Day 0 - Day 1), When Caregiver truy cập ứng dụng, Then mục 'Cẩm Nang 24 Giờ Đầu Sau Mổ' xuất hiện nổi bật tại đầu Trang chủ (SCR-CG-04).
- **AC-US-020.2:** Nội dung hướng dẫn chi tiết các hành động sống còn: (1) Luôn đeo kính bảo hộ mắt kể cả lúc nghỉ ngơi; (2) Dán khiên bảo vệ mắt bằng băng keo y tế trước khi đi ngủ để chống vô thức dụi mắt trong đêm; (3) Tư thế nằm ngủ an toàn: nằm ngửa hoặc nằm nghiêng về phía mắt lành (không đè lên mắt mổ); (4) Giải thích hiện tượng cộm xốn nhẹ, chảy nước mắt sinh lý là bình thường, không hoảng sợ; (5) Tuyệt đối không cúi gập đầu hoặc nâng vật nặng quá 5kg (BR22).
- **AC-US-020.3:** Có nút đánh dấu 'Đã đọc và nắm vững' để ghi nhận sự hiểu biết của người nhà.

---

### US-021 — Xem bảng Những điều Nên làm (Xanh) & Cần tránh (Đỏ) trực quan

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-021** |
| **Tính năng liên quan (Feature ID)** | **F-013** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-016** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR22** |
| **Giá trị nghiệp vụ (Business Value)** | Ngăn ngừa các hành vi nguy hiểm phổ biến: dụi mắt, để nước sinh hoạt bắn vào mắt, cúi gập xách nặng trong tuần đầu. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn xem bảng danh mục Những điều Nên làm và Tuyệt đối Cần tránh trực quan bằng màu sắc, để dễ dàng nhắc nhở người bệnh kiêng cữ đúng cách trong sinh hoạt hàng ngày."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-021.1:** Given Caregiver mở màn hình 'Nên làm & Cần tránh' (SCR-CG-08), When quan sát giao diện, Then nội dung được phân chia rõ thành 2 cột màu tương phản mạnh: Cột Xanh lục (Nên làm - DO) và Cột Đỏ cờ (Cần tránh - DON'T).
- **AC-US-021.2:** Mỗi mục có biểu tượng (icon) trực quan, tiêu đề ngắn gọn và lời giải thích y khoa: ví dụ Cần tránh: 'Không để nước sinh hoạt bắn vào mắt trong 7 ngày đầu' - Lý do: 'Nước máy chứa vi khuẩn tiềm ẩn nguy cơ nhiễm trùng vết rạch mổ nhãn cầu'.
- **AC-US-021.3:** Hỗ trợ bộ lọc nhanh theo 4 chủ đề sinh hoạt: Vệ sinh cá nhân, Vận động & Lao động, Dinh dưỡng & Ăn uống, Giấc ngủ & Thư giãn (BR22).

---

### US-022 — Xem các Infographic kiến thức hồi phục mắt tinh gọn trong Học viện Caregiver

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-022** |
| **Tính năng liên quan (Feature ID)** | **F-014** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P1** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-017** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR6, BR7** |
| **Giá trị nghiệp vụ (Business Value)** | Nâng cao nhận thức y tế cộng đồng; tạo sự an tâm và gắn kết thương hiệu sâu sắc với VISI Medical Group mà không tạo gánh nặng thi cử. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn xem các infographic kiến thức tinh gọn trong Học viện Caregiver, để hiểu rõ tiến trình hồi phục tự nhiên của mắt và cảm thấy an tâm hơn khi đồng hành cùng người bệnh."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-022.1:** Given Caregiver chọn mục 'Học Viện Caregiver' (SCR-CG-05, SCR-CG-06), When xem danh mục bài học, Then toàn bộ nội dung được trình bày dưới dạng các Infographic đồ họa tĩnh tinh gọn, giải thích tiến trình hồi phục sinh lý mắt từ Day 1 đến Month 1.
- **AC-US-022.2:** MVP loại bỏ hoàn toàn các bài kiểm tra trắc nghiệm (Mini Quiz) nhằm tránh gây áp lực, phiền hà hoặc tâm lý thi cử cho người nhà lớn tuổi (F-014).
- **AC-US-022.3:** Hệ thống tự động lưu vết các bài học đã xem để người dùng dễ theo dõi tiến độ học tập của mình.

---

### US-023 — Trả lời khảo sát Recovery Check 3–5 câu mỗi sáng trong 7 ngày đầu

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-023** |
| **Tính năng liên quan (Feature ID)** | **F-016** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-019** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR9, BR10** |
| **Giá trị nghiệp vụ (Business Value)** | Thiết lập kênh giám sát y tế chủ động từ xa; phát hiện sớm dấu hiệu bất thường trước khi trở thành biến chứng nặng. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn trả lời bảng khảo sát Recovery Check 3–5 câu hỏi mỗi sáng trong 7 ngày đầu, để giúp bệnh viện nắm bắt tình trạng hồi phục hàng ngày của người thân tôi."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-023.1:** Given bệnh nhân đang trong 7 ngày đầu sau mổ (hoặc các mốc Day 14, Day 30), When đồng hồ điểm 08:00 sáng, Then ứng dụng gửi thông báo nhắc Caregiver nộp bảng kiểm phục hồi (Recovery Check) (SCR-CG-11).
- **AC-US-023.2:** Bảng kiểm gồm 3–5 câu hỏi trắc nghiệm ngắn gọn được chuẩn hóa theo loại mổ: Mức độ đau mắt, Tình trạng thị lực (rõ / mờ đột ngột), Tình trạng chảy dịch/mủ, Mắt đỏ/sưng, Hiện tượng chớp sáng/ruồi bay (BR9).
- **AC-US-023.3:** Caregiver bắt buộc chọn đủ câu trả lời trước khi nhấn nút 'Gửi đánh giá' (BR9).
- **AC-US-023.4:** Hệ thống ngay lập tức đối chiếu với ma trận tiêu chí lâm sàng và phân loại kết quả thành 3 mức: Xanh (Bình thường), Vàng (Cần chú ý), Đỏ (Nguy hiểm Red Flag) (BR10).

---

### US-024 — Màn hình Cảnh báo Đỏ & Nút gọi 1 chạm Hotline VISI 0395 151 151

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-024** |
| **Tính năng liên quan (Feature ID)** | **F-017** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-020** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR10, BR11** |
| **Giá trị nghiệp vụ (Business Value)** | Cứu vãn thị giác người bệnh trong khung giờ vàng cấp cứu nhãn khoa; cung cấp phao cứu sinh đáng tin cậy cho gia đình. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn màn hình lập tức chuyển sang chế độ Cảnh báo Đỏ và cung cấp nút bấm gọi khẩn cấp 1 chạm đến Hotline VISI (0395 151 151) khi phát hiện dấu hiệu nguy hiểm, để tôi có thể liên hệ cấp cứu kịp thời."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-024.1:** Given câu trả lời Recovery Check hoặc lựa chọn khẩn cấp phát hiện dấu hiệu Red Flag (Đau nhức dữ dội lan lên nửa đầu, Thị lực mờ đột ngột như màn đen che, Chảy mủ vàng đặc, Mắt đỏ rực kèm sưng mi, Va đập cơ học mép mổ), When Caregiver gửi khảo sát, Then giao diện ứng dụng lập tức chuyển sang chế độ Cảnh báo Đỏ toàn màn hình (SCR-CG-12) (BR11).
- **AC-US-024.2:** Màn hình hiển thị nút gọi lớn nổi bật 1 chạm: 'GỌI NGAY HOTLINE CẤP CỨU VISI: 0395 151 151'. Khi bấm nút, điện thoại tự động quay số khẩn cấp mà không cần bấm phím số.
- **AC-US-024.3:** Hướng dẫn sơ cứu khẩn cấp hiển thị song song: 'Dán ngay khiên bảo vệ mắt, không nhỏ thêm bất kỳ thuốc gì, không dụi mắt và đưa bệnh nhân đến cơ sở VISI gần nhất'.
- **AC-US-024.4:** Hệ thống đồng thời bắn tín hiệu báo động đỏ thời gian thực lên Dashboard của Bệnh viện (F-019).

---

### US-025 — Nhận thông báo phản hồi an tâm khi kết quả Recovery Check bình thường

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-025** |
| **Tính năng liên quan (Feature ID)** | **F-016** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-019** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR10** |
| **Giá trị nghiệp vụ (Business Value)** | Giảm áp lực tâm lý hoang mang; giảm các cuộc gọi thắc mắc thông thường về tổng đài CSKH bệnh viện. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn nhận được thông báo phản hồi an tâm khi kết quả khảo sát hoàn toàn bình thường, để người nhà giải tỏa tâm lý lo lắng thường gặp sau mổ mắt."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-025.1:** Given Caregiver hoàn tất nộp bảng kiểm Recovery Check, When toàn bộ câu trả lời đều nằm trong giới hạn phục hồi sinh lý chuẩn (Phân loại Xanh - Bình thường), Then màn hình hiển thị biểu tượng tích xanh cùng thông điệp động viên y khoa ấm áp.
- **AC-US-025.2:** Nội dung thông báo: 'Chúc mừng! Diễn tiến hồi phục mắt của bệnh nhân đang rất tốt theo đúng phác đồ. Bạn hãy tiếp tục duy trì lịch nhỏ thuốc và kiêng cữ theo hướng dẫn nhé!'.
- **AC-US-025.3:** Ứng dụng chuyển về Trang chủ và ghi nhận trạng thái đã hoàn thành khảo sát cho ngày hôm đó.

---

### US-026 — Theo dõi Dashboard hiển thị trạng thái toàn bộ bệnh nhân chi nhánh

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-026** |
| **Tính năng liên quan (Feature ID)** | **F-018** |
| **Tác nhân thực hiện (Actor)** | **ACT-004 (Nhân viên CSKH / Điều dưỡng trực)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-021** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR14, BR15** |
| **Giá trị nghiệp vụ (Business Value)** | Nâng cao năng suất theo dõi bệnh nhân sau mổ; tập trung nguồn lực vào nhóm bệnh nhân có nguy cơ thay vì gọi dàn trải. |

> **Tuyên bố User Story:**  
> *"Là một Nhân viên CSKH/Điều dưỡng trực ban, tôi muốn theo dõi Bảng điều khiển (Dashboard) hiển thị trạng thái của toàn bộ bệnh nhân xuất viện tại chi nhánh mình, để phân loại và ưu tiên chăm sóc các trường hợp có nguy cơ cao."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-026.1:** Given Nhân viên CSKH / Điều dưỡng trực đăng nhập vào Dashboard quản lý cơ sở (SCR-DOC-12), When truy cập trang Giám sát phục hồi, Then bảng hiển thị danh sách toàn bộ bệnh nhân xuất viện của chi nhánh bệnh viện đang công tác.
- **AC-US-026.2:** Danh sách được tự động phân loại theo 3 nhóm chỉ báo trạng thái: (1) Nhóm Đỏ (Red Flag cần can thiệp gấp) ghim trên cùng; (2) Nhóm Vàng (Cần chú ý theo dõi); (3) Nhóm Xanh (Phục hồi ổn định).
- **AC-US-026.3:** Hỗ trợ bộ lọc linh hoạt: theo Ngày phẫu thuật, Loại phẫu thuật (Phaco/SILK), Bác sĩ phẫu thuật chính, và Tỷ lệ tuân thủ thuốc.

---

### US-027 — Bật chuông cảnh báo và đưa ca Red Flag lên đầu, can thiệp <5 phút

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-027** |
| **Tính năng liên quan (Feature ID)** | **F-019** |
| **Tác nhân thực hiện (Actor)** | **ACT-004 (Nhân viên CSKH / Medical Monitor)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-022** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR12** |
| **Giá trị nghiệp vụ (Business Value)** | Đảm bảo SLA phản ứng cấp cứu y tế; giảm thiểu tối đa biến cố y khoa ngoài viện cho tập đoàn. |

> **Tuyên bố User Story:**  
> *"Là một Nhân viên CSKH, tôi muốn hệ thống lập tức bật chuông cảnh báo và đưa ca bệnh lên đầu danh sách xử lý khi có tín hiệu Red Flag phát sinh, để tôi có thể gọi điện can thiệp trong vòng dưới 5 phút."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-027.1:** Given Dashboard CSKH đang mở trên màn hình trực, When có bất kỳ bệnh nhân nào trong cơ sở kích hoạt Red Flag, Then hệ thống phát chuông âm thanh cảnh báo to và liên tục, đồng thời nhấp nháy viền đỏ toàn bảng.
- **AC-US-027.2:** Thẻ ca bệnh Red Flag được tự động ghim lên vị trí hàng đầu danh sách, hiển thị đồng hồ đếm thời gian từ lúc phát sinh cảnh báo.
- **AC-US-027.3:** Nhân viên CSKH bấm nút 'Tiếp nhận ca' (chuyển trạng thái sang `IN_PROGRESS`), dừng chuông báo và thực hiện cuộc gọi điện thoại hỗ trợ bệnh nhân trong cam kết SLA dưới 5 phút (BR12).

---

### US-028 — Tự động leo thang (Escalate) ca Red Flag lên Bác sĩ trực sau 15 phút

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-028** |
| **Tính năng liên quan (Feature ID)** | **F-019** |
| **Tác nhân thực hiện (Actor)** | **ACT-004 (Nhân viên CSKH / Hệ thống)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-022b** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR12** |
| **Giá trị nghiệp vụ (Business Value)** | Ngăn ngừa tình trạng trễ nải trong xử lý sự cố y khoa; bảo đảm an toàn sinh mạng người bệnh tuyệt đối. |

> **Tuyên bố User Story:**  
> *"Là một Nhân viên CSKH, tôi muốn hệ thống tự động leo thang (Escalate) cảnh báo lên Bác sĩ trực hoặc Lãnh đạo cơ sở nếu ca Red Flag chưa được phản hồi sau 15 phút, để đảm bảo không một bệnh nhân nguy cấp nào bị bỏ quên."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-028.1:** Given một ca Red Flag đã phát sinh trên Dashboard, When sau 15 phút mà ca bệnh vẫn chưa được CSKH bấm tiếp nhận hoặc chưa ghi nhận cuộc gọi xử lý thành công, Then hệ thống tự động kích hoạt quy trình Leo thang cảnh báo cấp độ 2 (Escalation Level 2) (UC-022b, BR12).
- **AC-US-028.2:** Hệ thống tự động gửi tin nhắn SMS cảnh báo khẩn cấp và gọi tự động tới số điện thoại của Bác sĩ trực cơ sở và Trưởng cơ sở.
- **AC-US-028.3:** Trên Dashboard, ca bệnh chuyển trạng thái `ESCALATED_OVERDUE` với màu tím đậm cảnh báo vi phạm SLA.

---

### US-029 — Ghi nhận nhật ký cuộc gọi và hướng can thiệp trực tiếp trên Dashboard

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-029** |
| **Tính năng liên quan (Feature ID)** | **F-020** |
| **Tác nhân thực hiện (Actor)** | **ACT-004 (Nhân viên CSKH)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-023** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR12** |
| **Giá trị nghiệp vụ (Business Value)** | Đảm bảo tính liên tục của dữ liệu chăm sóc lâm sàng (Continuity of Care); phục vụ công tác kiểm thảo y khoa. |

> **Tuyên bố User Story:**  
> *"Là một Nhân viên CSKH, tôi muốn ghi nhận kết quả cuộc gọi liên hệ bệnh nhân (tình trạng thực tế, hướng xử lý, hẹn tái khám gấp) trực tiếp trên Dashboard, để bác sĩ điều trị và ban quản lý cùng nắm bắt diễn tiến ca bệnh."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-029.1:** Given CSKH vừa kết thúc cuộc gọi can thiệp với người nhà bệnh nhân, When mở form 'Ghi nhận cuộc gọi' trên ca bệnh tương ứng, Then nhập các thông tin: Người nghe máy (Caregiver chính / Bệnh nhân), Tình trạng thực tế ghi nhận qua điện thoại, Hướng xử lý can thiệp (Đã trấn an & hướng dẫn tiếp tục dùng thuốc / Hẹn tái khám cấp cứu tại cơ sở trong 2 giờ tới / Chuyển Bác sĩ mổ trực tiếp gọi lại).
- **AC-US-029.2:** When bấm 'Lưu nhật ký', dữ liệu được ghi vào `call_intervention_logs`.
- **AC-US-029.3:** Ca bệnh được chuyển trạng thái sang `RESOLVED` (Đã xử lý an toàn) hoặc `TRANSFERRED_TO_DOCTOR` (Đã chuyển Bác sĩ).

---

### US-030 — Xem lịch sử Recovery Check và biểu đồ triệu chứng khi bệnh nhân tái khám

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-030** |
| **Tính năng liên quan (Feature ID)** | **F-018** |
| **Tác nhân thực hiện (Actor)** | **ACT-002 (Bác sĩ điều trị)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-021** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR14** |
| **Giá trị nghiệp vụ (Business Value)** | Hỗ trợ ra quyết định lâm sàng chính xác dựa trên bằng chứng dữ liệu thực tế tại nhà (Real-World Evidence). |

> **Tuyên bố User Story:**  
> *"Là một Bác sĩ điều trị, tôi muốn xem lịch sử trả lời Recovery Check và biểu đồ triệu chứng của bệnh nhân khi họ đến tái khám, để có dữ liệu khách quan đánh giá đáp ứng lâm sàng của phác đồ mổ."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-030.1:** Given bệnh nhân đến phòng khám mắt tái khám định kỳ (Day 1, Day 7, Month 1), When Bác sĩ mở hồ sơ bệnh nhân trên hệ thống RemiCare, Then hệ thống hiển thị biểu đồ dòng thời gian theo dõi phục hồi.
- **AC-US-030.2:** Bác sĩ có thể xem chi tiết: Tỷ lệ tuân thủ nhỏ thuốc đúng giờ theo từng ngày, Lịch sử câu trả lời bảng kiểm Recovery Check hàng ngày, Các cảnh báo Red Flag đã từng phát sinh kèm ghi chú cuộc gọi can thiệp của CSKH.
- **AC-US-030.3:** Dữ liệu giúp Bác sĩ đánh giá chính xác mức độ hồi phục của giác mạc/thể thủy tinh và điều chỉnh đơn thuốc tái khám phù hợp.

---

### US-031 — Xem lộ trình 5 mốc tái khám chuẩn VISI và nhận thông báo nhắc trước 24h

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-031** |
| **Tính năng liên quan (Feature ID)** | **F-015** |
| **Tác nhân thực hiện (Actor)** | **ACT-001 (Caregiver)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-018** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR24** |
| **Giá trị nghiệp vụ (Business Value)** | Nâng cao tỷ lệ tuân thủ tái khám từ 60% lên ≥90%; phát hiện sớm biến chứng muộn như đục bao sau hay glôcôm thứ phát. |

> **Tuyên bố User Story:**  
> *"Là một Người chăm sóc, tôi muốn xem danh sách các mốc tái khám sắp tới (Day 1, Day 7, Month 1...) và nhận thông báo nhắc hẹn trước 24 giờ, để tôi chủ động sắp xếp công việc và đưa đón bệnh nhân đi khám đúng hẹn."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-031.1:** Given Caregiver truy cập tab 'Lịch Tái Khám' (SCR-CG-10), When xem màn hình, Then hệ thống hiển thị lộ trình 5 mốc tái khám bắt buộc chuẩn VISI: Ngày 1 (Day 1), Ngày 7 (Day 7), Tháng 1 (Month 1), Tháng 3 (Month 3), Tháng 6 (Month 6).
- **AC-US-031.2:** Mỗi mốc hiển thị rõ: Ngày dương lịch hẹn khám, Địa chỉ chi nhánh Bệnh viện Mắt VISI nơi đã mổ, Bác sĩ hẹn khám, và Tình trạng (Sắp tới / Đã khám / Quá hạn).
- **AC-US-031.3:** Hệ thống tự động kích hoạt thông báo đẩy (Push Notification) và tin nhắn SMS/Zalo nhắc hẹn trước thời điểm khám 24 giờ (BR24).
- **AC-US-031.4:** Caregiver có thể bấm nút 'Xác nhận sẽ đến' hoặc 'Yêu cầu đổi giờ khám' ngay trên ứng dụng.

---

### US-032 — Xem danh sách bệnh nhân có lịch tái khám ngày mai chưa xác nhận

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-032** |
| **Tính năng liên quan (Feature ID)** | **F-015** |
| **Tác nhân thực hiện (Actor)** | **ACT-004 (Nhân viên CSKH)** |
| **Mức độ ưu tiên (Priority)** | **P1** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-018** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR24** |
| **Giá trị nghiệp vụ (Business Value)** | Chủ động điều tiết lưu lượng bệnh nhân tái khám tại các chi nhánh VISI; tránh quá tải cục bộ tại phòng khám. |

> **Tuyên bố User Story:**  
> *"Là một Nhân viên CSKH, tôi muốn xem danh sách các bệnh nhân có lịch tái khám vào ngày mai nhưng chưa xác nhận, để chủ động gửi tin nhắn nhắc nhở hoặc gọi điện hỗ trợ đặt lịch."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-032.1:** Given CSKH trực màn hình Quản lý lịch hẹn trên Dashboard, When lọc danh sách theo mốc ngày mai (N+1) và trạng thái `CHƯA_XÁC_NHẬN`, Then hệ thống hiển thị danh sách các bệnh nhân cần liên hệ.
- **AC-US-032.2:** CSKH có thể chọn 'Gửi tin nhắn ZNS nhắc hẹn hàng loạt' hoặc bấm nút gọi điện trực tiếp từ giao diện để hỏi thăm và hỗ trợ bệnh nhân đặt khung giờ khám thuận tiện.
- **AC-US-032.3:** Khi bệnh nhân xác nhận qua điện thoại, CSKH chuyển trạng thái cuộc hẹn sang `CONFIRMED`.

---

### US-033 — Khởi tạo tài khoản nhân viên và phân quyền theo chi nhánh bệnh viện

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-033** |
| **Tính năng liên quan (Feature ID)** | **F-023** |
| **Tác nhân thực hiện (Actor)** | **ACT-007 (Quản trị viên hệ thống)** |
| **Mức độ ưu tiên (Priority)** | **P0** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-026** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR14, BR15** |
| **Giá trị nghiệp vụ (Business Value)** | Kiểm soát an toàn thông tin phân tán theo chuỗi bệnh viện; tuân thủ quy chế bảo mật bệnh viện. |

> **Tuyên bố User Story:**  
> *"Là một Quản trị viên hệ thống, tôi muốn khởi tạo tài khoản nhân viên y tế và gán quyền theo vai trò cùng cơ sở trực thuộc (5 bệnh viện VISI), để đảm bảo nhân viên chỉ truy cập đúng dữ liệu bệnh nhân của chi nhánh mình."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-033.1:** Given Quản trị viên hệ thống (IT Admin) truy cập module Quản lý người dùng, When tạo mới tài khoản nhân viên y tế, Then nhập thông tin định danh: Mã nhân viên, Họ tên, Email nội bộ, Số điện thoại, Vai trò RBAC (`ADMIN`, `DOCTOR`, `NURSE`, `CSKH`, `GCMO`), và Chi nhánh trực thuộc (ví dụ: `Bệnh viện Mắt VISI Thủ Đức`, `Bệnh viện Mắt VISI Bình Dương`).
- **AC-US-033.2:** Khi nhân viên đăng nhập, hệ thống áp dụng bộ lọc dữ liệu nghiêm ngặt: Điều dưỡng/CSKH/Bác sĩ cơ sở nào chỉ được xem, chỉnh sửa hồ sơ và Dashboard của bệnh nhân thuộc cơ sở đó (BR15).
- **AC-US-033.3:** Chỉ có Ban Giám Đốc và GCMO mới có quyền xem dữ liệu tổng hợp liên chi nhánh.

---

### US-034 — Tra cứu nhật ký kiểm toán (Audit Trail) phục vụ pháp lý và thanh tra

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-034** |
| **Tính năng liên quan (Feature ID)** | **F-024** |
| **Tác nhân thực hiện (Actor)** | **ACT-007 (Quản trị viên hệ thống)** |
| **Mức độ ưu tiên (Priority)** | **P1** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-027** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR5, BR21** |
| **Giá trị nghiệp vụ (Business Value)** | Cung cấp bằng chứng pháp lý minh bạch tuyệt đối; đáp ứng tiêu chuẩn an toàn an ninh mạng y tế. |

> **Tuyên bố User Story:**  
> *"Là một Quản trị viên hệ thống, tôi muốn tra cứu nhật ký kiểm toán (Audit Trail) về mọi thao tác tạo phác đồ, sửa đơn thuốc, kích hoạt QR và xử lý cảnh báo, để phục vụ công tác thanh tra chất lượng và bảo mật dữ liệu."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-034.1:** Given Quản trị viên hệ thống mở màn hình Nhật ký kiểm toán (Audit Trail), When thực hiện tìm kiếm, Then hệ thống cung cấp bộ lọc theo: Khoảng thời gian, Tác nhân thực hiện (Mã nhân viên / Caregiver), Loại hành vi (Tạo Care Plan, Sửa liều thuốc, In QR, Thu hồi QR, Xác nhận uống thuốc, Kích hoạt Red Flag, Đóng cảnh báo).
- **AC-US-034.2:** Mỗi bản ghi kiểm toán ghi nhận bất biến (immutable log): Timestamp chính xác, Địa chỉ IP, Tác nhân, Hành động, Dữ liệu cũ và Dữ liệu mới (Diff).
- **AC-US-034.3:** Hỗ trợ xuất báo cáo định dạng Excel / PDF có ký số phục vụ đoàn thanh tra y tế hoặc điều tra sự cố lâm sàng.

---

### US-035 — Bật chế độ Trợ năng nhãn khoa (Chữ to, tương phản cao, Audio Guide)

| Thuộc tính | Giá trị chi tiết |
| :--- | :--- |
| **Mã Story (Story ID)** | **US-035** |
| **Tính năng liên quan (Feature ID)** | **F-026** |
| **Tác nhân thực hiện (Actor)** | **ACT-006 (Bệnh nhân hậu phẫu)** |
| **Mức độ ưu tiên (Priority)** | **P1** |
| **Ca sử dụng liên kết (Use Cases)** | **UC-025** |
| **Quy tắc nghiệp vụ áp dụng (BRs)** | **BR6** |
| **Giá trị nghiệp vụ (Business Value)** | Tăng cường trải nghiệm nhân văn; trao quyền tự chủ một phần cho người bệnh khi thị lực bắt đầu hồi phục. |

> **Tuyên bố User Story:**  
> *"Là một Bệnh nhân mổ mắt đã qua giai đoạn đầu, tôi muốn kích hoạt chế độ giao diện chữ to tương phản cao và nghe thuyết minh bằng giọng nói tiếng Việt, để tôi có thể tự kiểm tra lịch nhỏ thuốc mà không làm mỏi mắt."*

#### Tiêu chí chấp nhận (Acceptance Criteria - AC):
- **AC-US-035.1:** Given Bệnh nhân hoặc Caregiver nhấn biểu tượng 'Trợ năng nhãn khoa' (Accessibility Icon) ở góc trên màn hình, When kích hoạt chế độ này, Then toàn bộ giao diện tự động chuyển đổi sang cấu hình tối ưu thị giác nhãn khoa: Cỡ chữ phóng to toàn diện (≥18pt cho văn bản thường, ≥24pt cho tiêu đề); Bảng màu tương phản cao (High Contrast: Nền đen chữ vàng sáng hoặc Nền trắng chữ đen tuyền, đạt chuẩn WCAG AAA); Kích thước các nút bấm mở rộng (≥48x48px).
- **AC-US-035.2:** Tích hợp tính năng Đọc bằng giọng nói tiếng Việt (Audio Guide / Text-to-Speech): Khi người bệnh chạm vào thẻ thuốc hoặc lịch tái khám, hệ thống tự động phát âm thanh đọc to, rõ ràng: 'Cữ thuốc sáng nay lúc 7 giờ gồm có lọ Cravit nhỏ 1 giọt vào mắt phải'.
- **AC-US-035.3:** Chế độ có thể bật/tắt dễ dàng bất cứ lúc nào.

---
