# VISI MEDICAL GROUP — REMICARE OPHTHALMIC POST-OP PLATFORM
# TÀI LIỆU ĐẶC TẢ YÊU CẦU ĐÃ SẮP XẾP THEO ID (US_US_V1)

> **Dự án:** RemiCare Ophthalmic Post-Op Platform (Nền tảng Hướng dẫn và Giám sát Chăm sóc Hậu phẫu Nhãn khoa)  
> **Doanh nghiệp mục tiêu:** Công ty Cổ phần Tập đoàn Y khoa VISI (VISI Medical Group)  
> **Phiên bản:** V1 (Chuẩn hóa toàn diện, đã sắp xếp tất cả Functional Requirements, User Stories, Use Cases và Actors theo thứ tự ID tăng dần)  
> **Vai trò:** Senior Business Analyst, Product Owner & System Analyst  
> **Ngày phê duyệt:** 15/09/2026  

---

## 1. TỔNG QUAN BỐI CẢNH VÀ NGUYÊN TẮC THIẾT KẾ

Tài liệu này là phiên bản nâng cấp chuẩn hóa (**US_US_V1**) từ phiên bản V0, trong đó toàn bộ danh mục **Functional Requirements (Features), Tác nhân (Actors), Câu chuyện người dùng (User Stories) và Ca sử dụng (Use Cases)** đã được:
1. **Sắp xếp tuần tự và nhất quán tuyệt đối theo mã định danh (ID)**, giúp các đội ngũ Lập trình (Dev), Kiểm thử (QA/QC), Phân tích nghiệp vụ (BA) và Quản lý dự án (PM/PO) dễ dàng tra cứu, kiểm tra chéo và đối chiếu với mã nguồn/test cases.
2. **Loại bỏ các dòng tiêu đề nhóm phân mảnh** trong lòng bảng để bảo đảm tính liên tục (continuous table data), hỗ trợ lọc (filtering) và sắp xếp dữ liệu tối ưu trong các công cụ quản lý yêu cầu (Jira, Confluence, Excel, Notion).
3. **Bổ sung ca sử dụng UC-009 hoàn chỉnh**, bảo đảm tính liền mạch 100% từ `UC-001` đến `UC-028` mà không có bất kỳ khoảng trống mã số nào.

### 1.1. Các vấn đề cốt lõi của chuỗi 5 bệnh viện VISI được giải quyết:
1. **Chống nhầm lẫn và rửa trôi thuốc mắt (F-009, F-010):** Bệnh nhân mổ mắt phải dùng 3–5 loại thuốc (kháng sinh fluoroquinolone, kháng viêm steroid, nước mắt nhân tạo không chất bảo quản). Hệ thống cung cấp lịch dùng thuốc trực quan và **Bộ đếm thời gian giãn cách 5–10 phút thông minh (Drop Interval Buffer Timer)** ngăn ngừa việc nhỏ dồn dập làm trôi thuốc.
2. **Sàng lọc biến chứng & kích hoạt cấp cứu khẩn cấp Red Flag (F-016, F-017, F-019):** Bệnh nhân không tự phân biệt được cộm xốn sinh lý với biến chứng nguy hiểm (tăng nhãn áp cấp, lệch vạt giác mạc, viêm mủ nội nhãn). Hệ thống cung cấp bài đánh giá **Recovery Check 3 mức** và nút gọi 1 chạm đến **Hotline VISI 0395 151 151** trong khung giờ vàng cấp cứu.
3. **Số hóa quy trình bàn giao xuất viện dưới 30 giây (F-004, F-008, F-021, F-022):** Thay thế việc dặn dò miệng lặp đi lặp lại và tờ rơi giấy dễ thất lạc bằng mã QR bảo mật in trực tiếp trên Phiếu xuất viện; Caregiver quét mã truy cập Web App (PWA) tức thì không cần cài đặt app.
4. **Phân định minh bạch trách nhiệm 3 tầng phía bệnh viện:**
   * **Bác sĩ (Doctor - ACT-002):** Cấu hình và phê duyệt phác đồ mẫu chuẩn (Master Template), xử lý ngoại lệ lâm sàng phức tạp.
   * **Điều dưỡng (Nurse - ACT-003):** Kích hoạt hồ sơ bệnh nhân từ template và bấm in phiếu QR (<30 giây) tại quầy lưu viện.
   * **Chăm sóc Khách hàng (CSKH / Medical Monitor - ACT-004):** Thường trực Dashboard cơ sở, tiếp nhận và điều phối xử lý ca Red Flag trong <5 phút.

---

## 2. DANH SÁCH TÍNH NĂNG CHỨC NĂNG (FUNCTIONAL REQUIREMENTS / FEATURES)
*Được sắp xếp nghiêm ngặt theo Feature ID từ F-001 đến F-028*

* **P0:** Bắt buộc cho phiên bản MVP (Thử nghiệm Pilot 60 ngày cho 100 ca mổ Phaco và SILK tại cơ sở Bệnh viện Mắt VISI Thủ Đức).
* **P1:** Quan trọng, triển khai trong giai đoạn MVP mở rộng hoặc Phase 2 trên toàn chuỗi 5 bệnh viện.

| Feature ID | Mô tả feature | Priority |
| :--- | :--- | :---: |
| **F-001** | **Xác thực Người Chăm Sóc qua OTP (Caregiver Authentication):** Đăng ký và đăng nhập không cần mật khẩu cho Caregiver bằng Số điện thoại và mã OTP qua SMS/ZNS; tự động liên kết phiên với bệnh nhân khi quét QR; kiểm soát phiên an toàn theo Nghị định 13/2023/NĐ-CP. | **P0** |
| **F-002** | **Xác thực Nhân Viên Y Tế Tập Trung (Staff Authentication & RBAC):** Xác thực đăng nhập cho Bác sĩ, Điều dưỡng, CSKH bằng tài khoản cấp phát nội bộ kết hợp 2FA; chặn hoàn toàn luồng tự đăng ký tự do; phân quyền chặt chẽ theo cơ sở bệnh viện (5 bệnh viện chuỗi VISI). | **P0** |
| **F-003** | **Quản lý Hồ sơ Người Chăm Sóc (Caregiver Profile Management):** Lưu trữ thông tin định danh tối thiểu của Caregiver (Họ tên, SĐT, Mối quan hệ); hỗ trợ 1 Caregiver quản lý nhiều bệnh nhân (ví dụ: người con chăm sóc cả bố và mẹ cùng mổ mắt). | **P0** |
| **F-004** | **Liên kết Caregiver - Bệnh nhân qua QR (Caregiver-Patient Linking):** Quét mã QR trên Phiếu xuất viện để thiết lập liên kết điện tử bảo mật với Care Plan bệnh nhân; hỗ trợ tối đa 3 Caregiver cùng liên kết vào 1 bệnh nhân để chia ca chăm sóc trong gia đình. | **P0** |
| **F-005** | **Quản lý Hồ sơ Bệnh Nhân Hậu Phẫu (Post-Op Patient Profile):** Quản lý thông tin lâm sàng tối thiểu: Mã bệnh nhân, Họ tên (viết tắt bảo mật theo NĐ 13), Năm sinh, Mắt phẫu thuật (MP/MT), Loại phẫu thuật (Phaco/SILK), Bác sĩ mổ và Cơ sở điều trị. | **P0** |
| **F-006** | **Quản lý Mẫu Kế Hoạch Chăm Sóc (Care Plan Master Templates):** Tạo lập, cấu hình và quản trị các gói phác đồ chuẩn hóa theo nhóm mổ (MVP tập trung 2 mẫu: Phaco tiêu chuẩn và Laser xóa cận SILK/ELITA); quản lý phiên bản (v1.0, v1.1); cài đặt danh mục thuốc và hướng dẫn mẫu. | **P0** |
| **F-007** | **Phê Duyệt Lâm Sàng Master Template (Clinical Approval Workflow):** Quy trình thẩm định chuyên môn: Bác sĩ soạn thảo -> Giám đốc Chuyên môn (BS.CKII Trần Bá Kiền) ký duyệt điện tử trước khi ban hành áp dụng đồng bộ cho toàn chuỗi 5 cơ sở VISI. | **P1** |
| **F-008** | **Khởi Tạo và Kích Hoạt Care Plan Cá Nhân Hóa (Patient Care Plan Instantiation):** Điều dưỡng nhân bản Master Template thành Care Plan thực tế cho bệnh nhân trong <30 giây (Clinical Setup 3 bước); cho phép Bác sĩ điều chỉnh liều lượng cá nhân hóa mà không làm đổi template gốc. | **P0** |
| **F-009** | **Lịch Uống và Nhỏ Thuốc Hậu Phẫu (Medication Regimen & Schedule):** Hiển thị danh sách cữ thuốc hàng ngày theo khung giờ (Sáng, Trưa, Chiều, Tối); hiển thị rõ tên biệt dược, số giọt, mắt chỉ định (MP/MT); nút xác nhận đã dùng kèm ghi nhận timestamp đồng bộ. | **P0** |
| **F-010** | **Bộ Đếm Thời Gian Giãn Cách Giữa Các Thuốc Nhỏ Mắt (Drop Interval Buffer Timer):** Tự động kích hoạt đồng hồ đếm lùi 5–10 phút thông minh ngay sau khi nhỏ lọ thuốc thứ nhất; tạm khóa nút xác nhận lọ thứ hai để chống rửa trôi thuốc; phát âm thanh và rung báo khi hết giờ. | **P0** |
| **F-011** | **Cẩm Nang Hướng Dẫn Thao Tác Nhỏ Thuốc / Vệ Sinh Mắt (Medication Guide):** Thư viện hình ảnh/video hướng dẫn kỹ thuật kéo mi dưới tạo túi cùng kết mạc, tuyệt đối không chạm đầu lọ vào mắt để ngăn ngừa viêm mủ nội nhãn; hướng dẫn lắc đều hỗn dịch thuốc. | **P0** |
| **F-012** | **Cẩm Nang Chăm Sóc Đặc Biệt 24 Giờ Đầu Sau Mổ (First 24h Critical Guide):** Danh mục hướng dẫn hành động khẩn thiết cho 24h đầu: đeo kính bảo hộ/dán khiên bảo vệ mắt khi ngủ, xử lý cộm xốn sinh lý, tư thế nằm ngửa/nghiêng mắt lành, kiêng cúi đầu xách nặng. | **P0** |
| **F-013** | **Quy Tắc Sinh Hoạt Nên Làm & Cần Tránh (Visual Do & Don't Guidelines):** Bảng trực quan 2 cột màu (Xanh: Nên làm / Đỏ: Cần tránh): kiêng nước sinh hoạt/xà phòng vào mắt trong tuần đầu, không dụi mắt, kiêng khói bụi, không trang điểm mắt trong 30 ngày. | **P0** |
| **F-014** | **Học Viện Người Chăm Sóc Tinh Gọn (Caregiver Micro-Academy Infographics):** Cung cấp tài liệu giáo dục trực quan dạng Infographic tĩnh tinh gọn, giải thích tiến trình hồi phục sinh lý mắt; loại bỏ bài kiểm tra quiz trong MVP để tránh phiền toái cho người nhà. | **P1** |
| **F-015** | **Quản Lý và Nhắc Lịch Tái Khám Bắt Buộc (Follow-Up Appointment Tracker):** Theo dõi lộ trình 5 mốc tái khám chuẩn VISI: Day 1, Day 7, Month 1, Month 3, Month 6; tự động gửi thông báo nhắc hẹn trước 24 giờ; hiển thị địa chỉ cơ sở VISI đã mổ và hotline đặt lịch. | **P0** |
| **F-016** | **Đánh Giá Phục Hồi Định Kỳ (Recovery Check 3 Mức):** Bộ câu hỏi sàng lọc 3–5 câu xuất hiện định kỳ (mỗi sáng trong 7 ngày đầu, Day 14, Day 30) khảo sát: đau nhức, thị lực mờ đột ngột, chảy mủ, mắt đỏ, chớp sáng; tự động phân loại 3 mức: Xanh (Bình thường) -> Vàng (Chú ý) -> Đỏ (Nguy hiểm). | **P0** |
| **F-017** | **Cảnh Báo Biến Chứng Nguy Hiểm (Red Flag Triage & Emergency Action):** Khi phát hiện dấu hiệu nguy cấp, hệ thống chuyển giao diện cảnh báo đỏ toàn màn hình, cung cấp nút gọi 1 chạm đến Hotline VISI (0395 151 151) và đẩy tín hiệu báo động khẩn lên Dashboard viện. | **P0** |
| **F-018** | **Bảng Điều Khiển Theo Dõi Bệnh Nhân Tập Trung (Clinical Monitoring Dashboard):** Giao diện web dành riêng cho Điều dưỡng và CSKH tại từng chi nhánh; hiển thị toàn bộ bệnh nhân xuất viện theo trạng thái tuân thủ; hỗ trợ lọc theo ngày mổ, loại mổ, bác sĩ điều trị. | **P0** |
| **F-019** | **Tiếp Nhận, Phân Loại và Xử Lý Cảnh Báo (Alert Triaging & Status Handling):** Module tiếp nhận cảnh báo trên Dashboard: phát chuông báo, đưa ca bệnh lên đầu danh sách; cho phép CSKH/Điều dưỡng nhận ca, chuyển trạng thái xử lý (Chờ -> Đang liên hệ -> Đã xử lý -> Chuyển bác sĩ). | **P0** |
| **F-020** | **Ghi Nhận Nhật Ký Cuộc Gọi và Can Thiệp (Call & Clinical Action Logging):** Cho phép CSKH/Điều dưỡng ghi nhận chi tiết nội dung cuộc gọi hỗ trợ: thời gian, người nghe máy, tình trạng ghi nhận, lời dặn y tế và phân loại mức độ rủi ro vào hồ sơ bệnh nhân. | **P0** |
| **F-021** | **Sinh Mã QR và Quản lý Vòng Đời QR (QR Lifecycle Management):** Sinh chuỗi token mã hóa ngẫu nhiên an toàn (UUIDv4/JWT có ký số) gắn với Care Plan; quản lý trạng thái mã QR: Tạo mới -> Đã cấp -> Đã kích hoạt -> Hết hạn -> Thu hồi. | **P0** |
| **F-022** | **In Phiếu Xuất Viện Kèm QR và Cấp Lại/Thu Hồi QR (Slip Print & QR Reissue):** In trực tiếp Phiếu xuất viện khổ chuẩn (A5/A4/decal) có in mã QR sắc nét; cho phép cấp lại mã QR mới khi người nhà làm mất phiếu hoặc khi bác sĩ đổi đơn thuốc, tự động vô hiệu hóa mã cũ. | **P0** |
| **F-023** | **Quản Lý Tài Khoản Nhân Viên và Phân Quyền Cơ Sở (Multi-Branch Staff RBAC):** Phân quyền vai trò (Admin, Bác sĩ, Điều dưỡng, CSKH, Giám đốc); kiểm soát phạm vi dữ liệu nghiêm ngặt theo cơ sở bệnh viện (nhân viên cơ sở nào chỉ xem bệnh nhân cơ sở đó). | **P0** |
| **F-024** | **Nhật Ký Kiểm Toán và Truy Vết Hoạt Động (Audit Trail & Compliance Logging):** Ghi vết bất biến (immutable log) mọi hành vi: ai kích hoạt Care Plan, ai sửa đơn thuốc, ai in QR, thời điểm xác nhận thuốc, lịch sử bấm Red Flag và nhật ký gọi của CSKH. | **P1** |
| **F-025** | **Báo Cáo Vận Hành và Chỉ Số Tuân Thủ (Compliance Analytics & Operational KPIs):** Tổng hợp báo cáo thời gian thực: Tỷ lệ quét kích hoạt QR (mục tiêu ≥85%), Tỷ lệ tuân thủ nhỏ thuốc đúng giờ, Tỷ lệ hoàn thành Recovery Check, Tỷ lệ tái khám Day 1, 7, 30. | **P1** |
| **F-026** | **Chế Độ Trợ Năng Giao Diện Nhãn Khoa (Ophthalmic Accessibility Mode):** Giao diện PWA tối ưu cho người mổ mắt và người già: Phông chữ to bản (≥18pt), tương phản cao (High Contrast: nền trắng chữ đen hoặc nền đen chữ vàng), nút bấm lớn (≥48px) và Audio Guide đọc tiếng Việt. | **P1** |
| **F-027** | **Hệ Thống Đa Kênh Thông Báo và Nhắc Việc (Multi-Channel Notification Gateway):** Điều phối thông báo tự động đa kênh đến Caregiver: Web Push Notification trên trình duyệt PWA, tin nhắn SMS Brandname 'VISI GROUP' và Zalo ZNS nhắc cữ thuốc và lịch tái khám. | **P1** |
| **F-028** | **Tra Cứu Tình Huống Khẩn Cấp & FAQ Lâm Sàng (Contextual Care Quick-Links):** Menu tra cứu nhanh tình huống thường gặp tại nhà: 'Dính nước vào mắt', 'Quên nhỏ thuốc', 'Mắt ngứa', 'Lỡ dụi mắt'; cung cấp câu trả lời y khoa chuẩn do VISI kiểm duyệt sẵn. | **P1** |

---

## 3. DANH SÁCH TÁC NHÂN HỆ THỐNG (ACTOR LIST) & PHÂN ĐỊNH TRÁCH NHIỆM

### 3.1 Bảng Phân Loại Tác Nhân Chuẩn Hóa

| Actor ID | Tên Actor | Mô tả vai trò và chức năng cốt lõi | Bản chất trong hệ thống |
| :--- | :--- | :--- | :--- |
| **ACT-001** | **Giám Đốc Bệnh Viện (Hospital Director / GCMO)** | Lãnh đạo bệnh viện / Giám đốc Chuyên môn: xem báo cáo vận hành, xử lý trường hợp cần đánh giá chuyên môn cấp cao (Escalated Review), theo dõi cảnh báo nghiêm trọng, xem hồ sơ bệnh nhân theo quyền được cấp, giám sát Bác sĩ/Điều dưỡng, phê duyệt Care Plan quan trọng. | **Clinical Executive Actor** *(Internal Leader)* |
| **ACT-002** | **Bác Sĩ Điều Trị / Phẫu Thuật (Ophthalmic Doctor / Surgeon)** | Bác sĩ chuyên khoa nhãn khoa: CRUD Medical Record, tạo/cập nhật Care Plan, cấu hình Medication, tạo/quản lý Learning Path, thiết lập Recovery Check, Red Flags, Do/Don't, lịch tái khám, theo dõi tiến trình chăm sóc và xử lý cảnh báo. | **Primary Clinical Actor** *(Internal Staff)* |
| **ACT-003** | **Điều Dưỡng Lưu Viện / Xuất Viện (Discharge / Clinical Nurse)** | Điều dưỡng phụ trách lưu viện: xem Medical Record theo quyền được cấp, nhập thông tin bệnh nhân (<30s), tạo Care Plan ban đầu theo hướng dẫn/mẫu được Bác sĩ phê duyệt, xuất QR bàn giao, theo dõi cữ thuốc, gửi cảnh báo đến Bác sĩ. | **Primary Nursing Actor** *(Internal Staff)* |
| **ACT-004** | **Chăm Sóc Khách Hàng (Customer Care - CSKH)** | Nhân viên CSKH: tra cứu thông tin tài khoản, hỗ trợ đăng ký/đăng nhập/liên kết QR, ghi nhận phản hồi/khiếu nại, hướng dẫn sử dụng, chuyển vấn đề kỹ thuật cho IT và chuyển vấn đề y tế cho Bác sĩ/Điều dưỡng. *Chỉ xem thông tin tài khoản, không tự ý xem toàn bộ hồ sơ y tế.* | **Supporting Service Actor** *(Internal Staff)* |
| **ACT-005** | **Người Chăm Sóc (Caregiver)** | Thân nhân trực tiếp chăm sóc: đăng ký/đăng nhập OTP, quét QR liên kết, xem Care Plan được cấp quyền, xem danh sách bệnh nhân đang chăm sóc, học cẩm nang, làm Recovery Check, xác nhận cho uống thuốc, canh bộ đếm ngược 5-10p, nhận thông báo tái khám và cảnh báo Red Flag (quản lý tối đa 03 Caregiver). | **Primary End-User Actor** *(External User)* |
| **ACT-006** | **Quản Trị Viên Hệ Thống (System Administrator - Admin)** | Kỹ sư CNTT: CRUD tài khoản người dùng, quản lý vai trò và quyền truy cập RBAC, khóa/mở khóa tài khoản, CRUD Audit Log, xem lịch sử hoạt động hệ thống, quản lý cấu hình hệ thống an toàn thông tin theo NĐ 13/2023. | **Administrative Actor** *(Internal Staff)* |
| **ACT-007** | **Bệnh Nhân Hậu Phẫu (Care Recipient / Patient)** | Người trực tiếp thụ hưởng điều trị: đăng ký/đăng nhập OTP, xem thông tin cá nhân & hồ sơ y tế của bản thân, xem Care Plan được cấp quyền, xem danh sách thuốc & đếm ngược, tự xác nhận đã dùng thuốc, làm Recovery Check, cập nhật sức khỏe, nhận cảnh báo Red Flag, tải ảnh mắt lên nếu được cấp quyền. | **Primary Beneficiary Actor** *(External User)* |
| **ACT-008** | **Đội Cấp Cứu Y Tế Ngoại Viện (External Emergency Support)** | Đội cấp cứu 115 hoặc bệnh viện đa khoa địa phương tiếp nhận bệnh nhân trong trường hợp biến chứng cấp cứu ngoài giờ làm việc hoặc bệnh nhân ở xa cơ sở VISI. | **External Supporting Actor** *(Emergency)* |
| **ACT-009** | **Cổng Dịch Vụ Viễn Thông (SMS / OTP / ZNS Gateway)** | Dịch vụ viễn thông bên thứ ba gửi mã OTP xác thực, tin nhắn SMS Brandname và Zalo ZNS nhắc cữ thuốc và lịch tái khám tự động. | **External Service Actor** *(System)* |
| **ACT-010** | **Hệ Thống Thông Tin Bệnh Viện (HIS Core Gateway)** | Hệ thống HIS nội bộ của VISI; trong Phase 2 RemiCare sẽ tích hợp qua API để tự động đồng bộ danh sách ca mổ và đơn thuốc xuất viện. | **External System Actor** *(Phase 2)* |

---

### 3.2 Ma Trận Trách Nhiệm Chi Tiết Của 7 Nhóm Tác Nhân Cốt Lõi

#### 1. Giám đốc Bệnh viện
* Xem báo cáo vận hành.
* Xử lý các trường hợp cần đánh giá chuyên môn cấp cao (Escalated Review).
* Theo dõi các cảnh báo y tế nghiêm trọng.
* Xem hồ sơ bệnh nhân theo phạm vi được phân quyền. *(Lưu ý: Không mặc định cho Giám đốc xem mọi hồ sơ nếu chưa có quy định phân quyền. Hệ thống chỉ cho phép xem hồ sơ bệnh nhân theo đúng quyền được cấp).*
* Quản lý và giám sát Bác sĩ, Điều dưỡng.
* Phê duyệt các Care Plan quan trọng.
* Theo dõi chất lượng và hiệu quả chăm sóc.
* Xem lịch sử hoạt động chuyên môn và báo cáo tổng hợp.

#### 2. Bác sĩ
* CRUD Medical Record.
* Tạo và cập nhật Care Plan: Có quyền **sao chép (clone) Master Template** để tạo và cá nhân hóa Care Plan cho 1 bệnh nhân cụ thể tùy theo thể trạng, cơ địa và đáp ứng lâm sàng của họ.
* Cấu hình và điều chỉnh liều lượng Medication trong Care Plan theo thể trạng bệnh nhân.
* Cấu hình Medication trong Care Plan.
* Tạo và quản lý Learning Path.
* Thiết lập Recovery Check.
* Thiết lập Red Flags.
* Thiết lập hướng dẫn Do & Don’t.
* Thiết lập lịch Follow-up / Tái khám.
* Xuất bản hoặc gửi Care Plan để phê duyệt.
* Theo dõi tiến trình chăm sóc của bệnh nhân.
* Xem kết quả Recovery Check và các cảnh báo từ Điều dưỡng/Caregiver.
* Xử lý các trường hợp được Điều dưỡng hoặc hệ thống chuyển cấp.

#### 3. Điều dưỡng
* Xem Medical Record theo quyền được cấp.
* Xem Care Plan.
* Nhập thông tin bệnh nhân vào hệ thống: **Chỉ được nhập thông tin cơ bản của bệnh nhân** (Họ tên, năm sinh, giới tính, SĐT, mắt mổ, ngày mổ trong <30 giây).
* Tạo Care Plan ban đầu theo hướng dẫn hoặc mẫu được Bác sĩ phê duyệt (**Tuyệt đối không được chỉnh sửa liều lượng thuốc**).
* Xuất QR để Caregiver liên kết với bệnh nhân.
* Theo dõi việc thực hiện nhiệm vụ chăm sóc.
* Cập nhật tình trạng bệnh nhân.
* Ghi nhận Recovery Check.
* Theo dõi các dấu hiệu bất thường.
* Gửi cảnh báo đến Bác sĩ.
* Cấp lại hoặc vô hiệu hóa mã QR khi QR bị mất hoặc có nguy cơ bị lộ.
* Hỗ trợ hướng dẫn Caregiver.
* Theo dõi danh sách Caregiver đã liên kết với bệnh nhân.
* > [!IMPORTANT]
  > **Lưu ý về quyền tạo Care Plan:** Bác sĩ là người có quyền chuyên môn tối cao tạo, chỉnh sửa và phê duyệt nội dung y tế, có quyền sao chép Master Template để tùy biến liều lượng thuốc theo thể trạng bệnh nhân. Điều dưỡng **chỉ được nhập thông tin cơ bản của bệnh nhân** và thiết lập Care Plan theo mẫu Bác sĩ cho phép, **tuyệt đối không được chỉnh sửa liều lượng thuốc**, danh mục thuốc, tiêu chí Red Flags hoặc hướng dẫn điều trị.

#### 4. Chăm sóc khách hàng (CSKH)
* Tra cứu thông tin tài khoản.
* Hỗ trợ đăng ký và đăng nhập.
* Hỗ trợ vấn đề liên kết QR.
* Ghi nhận phản hồi và khiếu nại.
* Theo dõi trạng thái yêu cầu hỗ trợ.
* Hướng dẫn người dùng sử dụng hệ thống.
* Chuyển các vấn đề kỹ thuật đến Admin hoặc bộ phận kỹ thuật.
* Chuyển các vấn đề liên quan đến y tế cho Điều dưỡng/Bác sĩ.
* > [!CAUTION]
  > **Giới hạn quyền bảo mật:** CSKH chỉ được xem thông tin tài khoản và trạng thái hỗ trợ cần thiết; không được tự ý truy cập toàn bộ hồ sơ y tế chuyên sâu của người bệnh.

#### 5. Caregiver (Người Chăm Sóc / Thân Nhân)
* Đăng ký và đăng nhập bằng số điện thoại/OTP.
* Quét QR để liên kết với bệnh nhân.
* Xem danh sách bệnh nhân đang chăm sóc.
* Xem Care Plan được cấp quyền.
* Xem Learning Path.
* Học các bài hướng dẫn chăm sóc.
* Thực hiện Recovery Check.
* Xác nhận đã cho bệnh nhân uống thuốc.
* Xem lịch dùng thuốc.
* Nhận thông báo khi đến giờ uống thuốc.
* Xem bộ đếm ngược thời gian dùng thuốc nhỏ mắt (Buffer Timer 5–10 phút).
* Xem lịch tái khám.
* Nhận cảnh báo Red Flag.
* Gửi phản hồi hoặc ghi chú chăm sóc.
* Cập nhật tình trạng thực hiện nhiệm vụ chăm sóc.
* Theo dõi lịch sử chăm sóc của bệnh nhân.
* Quản lý tối đa 03 Caregiver cho mỗi bệnh nhân nếu được cấp quyền.
* Xem lịch sử các bệnh nhân đã từng chăm sóc.

#### 6. Admin (Quản Trị Viên Hệ Thống)
* CRUD tài khoản người dùng.
* Quản lý vai trò và quyền truy cập RBAC.
* Khóa / mở khóa tài khoản.
* CRUD Audit Log.
* Xem lịch sử hoạt động hệ thống.
* Quản lý cấu hình hệ thống.

#### 7. Care Recipient (Bệnh Nhân / Người Thụ Hưởng Chăm Sóc)
* Đăng ký và đăng nhập bằng số điện thoại/OTP.
* Xem thông tin cá nhân và hồ sơ y tế của bản thân.
* Xem Care Plan được Bác sĩ hoặc Điều dưỡng cấp quyền.
* Xem danh sách thuốc và hướng dẫn sử dụng thuốc.
* Nhận thông báo khi đến giờ uống thuốc.
* Xác nhận bản thân đã uống thuốc hoặc sử dụng thuốc nhỏ mắt.
* Xem bộ đếm ngược thời gian dùng thuốc nhỏ mắt (Buffer Timer 5–10 phút chống rửa trôi thuốc).
* Xem lịch tái khám.
* Nhận thông báo và nhắc lịch tái khám.
* Xem Learning Path.
* Học các bài hướng dẫn chăm sóc.
* Thực hiện Recovery Check.
* Cập nhật tình trạng sức khỏe hằng ngày.
* Gửi ghi chú về triệu chứng hoặc tình trạng bất thường.
* Nhận cảnh báo Red Flag.
* Gửi phản hồi hoặc yêu cầu hỗ trợ.
* Xem lịch sử chăm sóc và quá trình điều trị của bản thân.
* Xem tiến độ hoàn thành nhiệm vụ chăm sóc.
* Cập nhật thông tin cá nhân trong phạm vi được phép.
* Tải lên hình ảnh hoặc tài liệu y tế của bản thân nếu được cấp quyền.

---

## 4. DANH SÁCH CÂU CHUYỆN NGƯỜI DÙNG (USER STORY LIST)
*Được sắp xếp nghiêm ngặt theo Story ID từ US-001 đến US-035*

Cấu trúc chuẩn hóa: `Là một [Actor], tôi muốn [hành động/nhu cầu], để [giá trị nghiệp vụ]`

| Story ID | Feature ID | Actor | User Story (Tiếng Việt) | Business Value | Priority |
| :--- | :--- | :--- | :--- | :--- | :---: |
| **US-001** | F-001 | Caregiver | Là một Người chăm sóc, tôi muốn đăng ký và đăng nhập vào ứng dụng bằng Số điện thoại cá nhân và nhận mã xác thực OTP qua SMS/Zalo, để có thể nhanh chóng truy cập hệ thống chăm sóc mà không cần ghi nhớ mật khẩu phức tạp. | Định danh người chăm sóc hợp pháp; loại bỏ rào cản thao tác cho người nhà lớn tuổi. | **P0** |
| **US-002** | F-001 | Caregiver | Là một Người chăm sóc, tôi muốn duy trì phiên đăng nhập an toàn trên thiết bị di động cá nhân, để không phải nhập lại OTP nhiều lần trong ngày mỗi khi đến cữ tra thuốc cho người bệnh. | Tối ưu hóa trải nghiệm người dùng; đảm bảo mở hướng dẫn nhỏ thuốc tức thì trong tình huống khẩn cấp. | **P0** |
| **US-003** | F-002 | Điều dưỡng xuất viện | Là một Điều dưỡng xuất viện, tôi muốn đăng nhập vào hệ thống bệnh trạm bằng tài khoản định danh do bệnh viện cấp phát tập trung, để thao tác kích hoạt Kế hoạch chăm sóc và in phiếu xuất viện theo đúng thẩm quyền cơ sở. | Bảo đảm tính pháp lý và truy vết trách nhiệm lâm sàng; ngăn ngừa truy cập trái phép. | **P0** |
| **US-004** | F-002 | Bác sĩ điều trị | Là một Bác sĩ điều trị, tôi muốn đăng nhập bảo mật vào hệ thống bằng tài khoản chuyên môn kết hợp xác thực 2 lớp (2FA), để truy cập danh sách bệnh nhân mổ của mình và phê duyệt các điều chỉnh phác đồ đặc biệt. | Bảo vệ dữ liệu bệnh án nhạy cảm theo Luật Khám bệnh, chữa bệnh và Nghị định 13/2023/NĐ-CP. | **P0** |
| **US-005** | F-004 | Caregiver | Là một Người chăm sóc, tôi muốn sử dụng camera điện thoại quét mã QR in trên Phiếu xuất viện của bệnh nhân, để tự động liên kết tài khoản của mình với Kế hoạch chăm sóc mắt của người thân chỉ trong vài giây. | Số hóa bàn giao 100%; loại bỏ hoàn toàn việc nhập liệu thủ công mã hồ sơ dễ sai sót. | **P0** |
| **US-006** | F-004 | Caregiver | Là một Người chăm sóc, tôi muốn hệ thống cho phép tối đa 3 thành viên trong gia đình cùng quét mã QR để liên kết vào hồ sơ một bệnh nhân, để cả nhà có thể thay phiên nhau nhỏ thuốc và theo dõi hồi phục. | Phản ánh đúng thực tế chăm sóc gia đình Việt Nam; tránh tình trạng gián đoạn dùng thuốc khi người nhà chính bận việc. | **P0** |
| **US-007** | F-005 | Điều dưỡng xuất viện | Là một Điều dưỡng xuất viện, tôi muốn nhập thông tin tóm tắt của bệnh nhân (Mã bệnh nhân, Họ tên viết tắt, Năm sinh, Mắt phẫu thuật: MP/MT) trước khi xuất viện, để thiết lập thực thể phục hồi hậu phẫu cá nhân hóa. | Tạo lập hồ sơ theo dõi lâm sàng độc lập; chuẩn bị dữ liệu xuất bản mã QR bàn giao. | **P0** |
| **US-008** | F-021 | Điều dưỡng xuất viện | Là một Điều dưỡng xuất viện, tôi muốn hệ thống tự động sinh mã QR mã hóa an toàn ngay khi Kế hoạch chăm sóc được kích hoạt, để sẵn sàng in ra phiếu hướng dẫn xuất viện cho người nhà. | Tự động hóa quy trình cấp mã lâm sàng; bảo mật dữ liệu bệnh án trong chuỗi token ngẫu nhiên. | **P0** |
| **US-009** | F-022 | Điều dưỡng xuất viện | Là một Điều dưỡng xuất viện, tôi muốn bấm in trực tiếp Phiếu xuất viện tích hợp mã QR và thông tin tóm tắt bằng máy in tại quầy lưu viện, để trao tận tay người nhà kèm lời dặn dò trước khi rời viện. | Tích hợp nhịp nhàng vào luồng vận hành phòng lưu viện hiện tại; giảm thời gian dặn dò miệng từ 15 phút xuống dưới 2 phút. | **P0** |
| **US-010** | F-022 | Điều dưỡng xuất viện | Là một Điều dưỡng xuất viện, tôi muốn cấp lại mã QR mới và vô hiệu hóa mã cũ khi người nhà làm mất phiếu xuất viện hoặc khi bác sĩ thay đổi đơn thuốc, để đảm bảo người nhà luôn tiếp cận đúng phác đồ mới nhất. | Kiểm soát tính toàn vẹn và duy nhất của phác đồ điều trị; ngăn ngừa việc nhỏ thuốc theo đơn cũ đã bị hủy. | **P0** |
| **US-011** | F-006 | Bác sĩ điều trị | Là một Bác sĩ điều trị, tôi muốn thiết lập và duy trì các Mẫu kế hoạch chăm sóc (Care Plan Master Template) chuẩn hóa theo từng loại phẫu thuật (Phaco, SILK...), để tái sử dụng thống nhất trên toàn chuỗi 5 bệnh viện VISI. | Chuẩn hóa chất lượng chuyên môn chuỗi; bảo vệ thương hiệu và kết quả kỹ thuật của máy laser ELITA và máy Phaco. | **P0** |
| **US-012** | F-007 | Người phê duyệt lâm sàng | Là một Giám đốc Chuyên môn (GCMO), tôi muốn thẩm định và ký duyệt điện tử các phiên bản Master Template trước khi ban hành, để đảm bảo mọi nội dung y khoa tuân thủ phác đồ của Hội đồng Chuyên môn VISI. | Thiết lập chốt chặn an toàn lâm sàng cao nhất; kiểm soát rủi ro tai biến y khoa trên toàn hệ thống tập đoàn. | **P1** |
| **US-013** | F-008 | Điều dưỡng xuất viện | Là một Điều dưỡng xuất viện, tôi muốn chọn Master Template và áp dụng nhanh cho bệnh nhân chỉ trong 3 bước (<30 giây), để hoàn tất thủ tục bàn giao xuất viện mà không làm ùn tắc phòng lưu viện. | Tối ưu hóa năng suất lao động của điều dưỡng; biến phần mềm thành công cụ hỗ trợ tiện lợi thay vì gánh nặng nhập liệu. | **P0** |
| **US-014** | F-008 | Bác sĩ điều trị | Là một Bác sĩ điều trị, tôi muốn tùy chỉnh liều lượng hoặc thêm bớt loại thuốc trong Care Plan của bệnh nhân mà không làm biến đổi Master Template gốc, để đáp ứng đặc điểm bệnh lý riêng của từng ca mổ phức tạp. | Đảm bảo tính linh hoạt y khoa (Clinical Flexibility) song song với tính chuẩn hóa hệ thống (BR18 Data Independence). | **P0** |
| **US-015** | F-009 | Caregiver | Là một Người chăm sóc, tôi muốn xem danh sách các cữ thuốc trong ngày được phân chia rõ ràng theo khung giờ (Sáng, Trưa, Chiều, Tối) kèm hình ảnh lọ thuốc và số giọt cần nhỏ, để không bị nhầm lẫn giữa 3–5 loại thuốc mắt khác nhau. | Giải quyết triệt để khoảng trống nhầm lẫn thuốc (BN-001); bảo đảm bệnh nhân nhận đúng thuốc, đúng liều lượng chỉ định. | **P0** |
| **US-016** | F-009 | Caregiver | Là một Người chăm sóc, tôi muốn bấm nút 'Xác nhận đã nhỏ thuốc' sau mỗi lần cho bệnh nhân dùng thuốc, để hệ thống lưu vết và đánh dấu cữ thuốc đã hoàn thành trong ngày. | Ngăn ngừa việc nhỏ trùng liều hoặc người nhà khác tưởng chưa nhỏ lại nhỏ tiếp gây quá liều độc cho giác mạc. | **P0** |
| **US-017** | F-010 | Caregiver | Là một Người chăm sóc, tôi muốn hệ thống tự động kích hoạt đồng hồ đếm ngược 10 phút (hoặc 5 phút theo chỉ định) sau khi nhỏ lọ thứ nhất, để tôi biết chính xác khi nào mắt đã hấp thu xong và an toàn để nhỏ lọ thứ hai. | Triệt tiêu hoàn toàn hiện tượng rửa trôi thuốc (washout effect); bảo toàn sinh khả dụng dược lý nhãn khoa tối đa. | **P0** |
| **US-018** | F-010 | Caregiver | Là một Người chăm sóc, tôi muốn hệ thống phát âm thanh thông báo và rung khi đồng hồ đếm ngược giãn cách kết thúc, để tôi không phải đứng canh điện thoại liên tục mà vẫn nhỏ thuốc tiếp theo đúng lúc. | Giảm căng thẳng và tiện lợi hóa việc chăm sóc tại nhà cho người thân bận rộn làm việc gia đình. | **P0** |
| **US-019** | F-011 | Caregiver | Là một Người chăm sóc, tôi muốn xem video ngắn hoặc hình ảnh minh họa cách kéo mi dưới và khoảng cách giữ đầu lọ thuốc, để tôi thực hiện thao tác nhỏ thuốc chuẩn xác mà không để đầu lọ chạm vào mắt bệnh nhân. | Loại bỏ nguy cơ nhiễm khuẩn chéo và chấn thương cơ học mép rạch gây viêm mủ nội nhãn (Endophthalmitis). | **P0** |
| **US-020** | F-012 | Caregiver | Là một Người chăm sóc, tôi muốn xem cẩm nang hành động đặc biệt cho 24 giờ đầu sau mổ, để biết cách bảo vệ mắt mổ ngay khi vừa từ viện về nhà trong khoảng thời gian nhạy cảm nhất. | Bảo vệ mép mổ trong 'thời điểm vàng' 24h đầu; chống bung vạt giác mạc hoặc xuất huyết tiền phòng. | **P0** |
| **US-021** | F-013 | Caregiver | Là một Người chăm sóc, tôi muốn xem bảng danh mục Những điều Nên làm và Tuyệt đối Cần tránh trực quan bằng màu sắc, để dễ dàng nhắc nhở người bệnh kiêng cữ đúng cách trong sinh hoạt hàng ngày. | Ngăn ngừa các hành vi nguy hiểm phổ biến: dụi mắt, để nước sinh hoạt bắn vào mắt, cúi gập xách nặng trong tuần đầu. | **P0** |
| **US-022** | F-014 | Caregiver | Là một Người chăm sóc, tôi muốn xem các infographic kiến thức tinh gọn trong Học viện Caregiver, để hiểu rõ tiến trình hồi phục tự nhiên của mắt và cảm thấy an tâm hơn khi đồng hành cùng người bệnh. | Nâng cao nhận thức y tế cộng đồng; tạo sự an tâm và gắn kết thương hiệu sâu sắc với VISI Medical Group. | **P1** |
| **US-023** | F-016 | Caregiver | Là một Người chăm sóc, tôi muốn trả lời bảng khảo sát Recovery Check 3–5 câu hỏi mỗi sáng trong 7 ngày đầu, để giúp bệnh viện nắm bắt tình trạng hồi phục hàng ngày của người thân tôi. | Thiết lập kênh giám sát y tế chủ động từ xa; phát hiện sớm dấu hiệu bất thường trước khi trở thành biến chứng nặng. | **P0** |
| **US-024** | F-017 | Caregiver | Là một Người chăm sóc, tôi muốn màn hình lập tức chuyển sang chế độ Cảnh báo Đỏ và cung cấp nút bấm gọi khẩn cấp 1 chạm đến Hotline VISI (0395 151 151) khi phát hiện dấu hiệu nguy hiểm, để tôi có thể liên hệ cấp cứu kịp thời. | Cứu vãn thị giác người bệnh trong khung giờ vàng cấp cứu nhãn khoa; cung cấp phao cứu sinh đáng tin cậy cho gia đình. | **P0** |
| **US-025** | F-016 | Caregiver | Là một Người chăm sóc, tôi muốn nhận được thông báo phản hồi an tâm khi kết quả khảo sát hoàn toàn bình thường, để người nhà giải tỏa tâm lý lo lắng thường gặp sau mổ mắt. | Giảm áp lực tâm lý hoang mang; giảm các cuộc gọi thắc mắc thông thường về tổng đài CSKH bệnh viện. | **P0** |
| **US-026** | F-018 | Nhân viên CSKH | Là một Nhân viên CSKH/Điều dưỡng trực ban, tôi muốn theo dõi Bảng điều khiển (Dashboard) hiển thị trạng thái của toàn bộ bệnh nhân xuất viện tại chi nhánh mình, để phân loại và ưu tiên chăm sóc các trường hợp có nguy cơ cao. | Nâng cao năng suất theo dõi bệnh nhân sau mổ; tập trung nguồn lực vào nhóm bệnh nhân có nguy cơ thay vì gọi dàn trải. | **P0** |
| **US-027** | F-019 | Nhân viên CSKH | Là một Nhân viên CSKH, tôi muốn hệ thống lập tức bật chuông cảnh báo và đưa ca bệnh lên đầu danh sách xử lý khi có tín hiệu Red Flag phát sinh, để tôi có thể gọi điện can thiệp trong vòng dưới 5 phút. | Đảm bảo SLA phản ứng cấp cứu y tế; giảm thiểu tối đa biến cố y khoa ngoài viện cho tập đoàn. | **P0** |
| **US-028** | F-019 | Nhân viên CSKH | Là một Nhân viên CSKH, tôi muốn hệ thống tự động leo thang (Escalate) cảnh báo lên Bác sĩ trực hoặc Lãnh đạo cơ sở nếu ca Red Flag chưa được phản hồi sau 15 phút, để đảm bảo không một bệnh nhân nguy cấp nào bị bỏ quên. | Ngăn ngừa tình trạng trễ nải trong xử lý sự cố y khoa; bảo đảm an toàn sinh mạng người bệnh tuyệt đối. | **P0** |
| **US-029** | F-020 | Nhân viên CSKH | Là một Nhân viên CSKH, tôi muốn ghi nhận kết quả cuộc gọi liên hệ bệnh nhân (tình trạng thực tế, hướng xử lý, hẹn tái khám gấp) trực tiếp trên Dashboard, để bác sĩ điều trị và ban quản lý cùng nắm bắt diễn tiến ca bệnh. | Đảm bảo tính liên tục của dữ liệu chăm sóc lâm sàng (Continuity of Care); phục vụ công tác kiểm thảo y khoa. | **P0** |
| **US-030** | F-018 | Bác sĩ điều trị | Là một Bác sĩ điều trị, tôi muốn xem lịch sử trả lời Recovery Check và biểu đồ triệu chứng của bệnh nhân khi họ đến tái khám, để có dữ liệu khách quan đánh giá đáp ứng lâm sàng của phác đồ mổ. | Hỗ trợ ra quyết định lâm sàng chính xác dựa trên bằng chứng dữ liệu thực tế tại nhà (Real-World Evidence). | **P0** |
| **US-031** | F-015 | Caregiver | Là một Người chăm sóc, tôi muốn xem danh sách các mốc tái khám sắp tới (Day 1, Day 7, Month 1...) và nhận thông báo nhắc hẹn trước 24 giờ, để tôi chủ động sắp xếp công việc và đưa đón bệnh nhân đi khám đúng hẹn. | Nâng cao tỷ lệ tuân thủ tái khám từ 60% lên ≥90%; phát hiện sớm biến chứng muộn như đục bao sau hay glôcôm thứ phát. | **P0** |
| **US-032** | F-015 | Nhân viên CSKH | Là một Nhân viên CSKH, tôi muốn xem danh sách các bệnh nhân có lịch tái khám vào ngày mai nhưng chưa xác nhận, để chủ động gửi tin nhắn nhắc nhở hoặc gọi điện hỗ trợ đặt lịch. | Chủ động điều tiết lưu lượng bệnh nhân tái khám tại các chi nhánh VISI; tránh quá tải cục bộ tại phòng khám. | **P1** |
| **US-033** | F-023 | Quản trị viên hệ thống | Là một Quản trị viên hệ thống, tôi muốn khởi tạo tài khoản nhân viên y tế và gán quyền theo vai trò cùng cơ sở trực thuộc (5 bệnh viện VISI), để đảm bảo nhân viên chỉ truy cập đúng dữ liệu bệnh nhân của chi nhánh mình. | Kiểm soát an toàn thông tin phân tán theo chuỗi bệnh viện; tuân thủ quy chế bảo mật bệnh viện. | **P0** |
| **US-034** | F-024 | Quản trị viên hệ thống | Là một Quản trị viên hệ thống, tôi muốn tra cứu nhật ký kiểm toán (Audit Trail) về mọi thao tác tạo phác đồ, sửa đơn thuốc, kích hoạt QR và xử lý cảnh báo, để phục vụ công tác thanh tra chất lượng và bảo mật dữ liệu. | Cung cấp bằng chứng pháp lý minh bạch tuyệt đối; đáp ứng tiêu chuẩn an toàn an ninh mạng y tế. | **P1** |
| **US-035** | F-026 | Bệnh nhân hậu phẫu | Là một Bệnh nhân mổ mắt đã qua giai đoạn đầu, tôi muốn kích hoạt chế độ giao diện chữ to tương phản cao và nghe thuyết minh bằng giọng nói tiếng Việt, để tôi có thể tự kiểm tra lịch nhỏ thuốc mà không làm mỏi mắt. | Tăng cường trải nghiệm nhân văn; trao quyền tự chủ một phần cho người bệnh khi thị lực bắt đầu hồi phục. | **P1** |

---

## 5. DANH SÁCH CA SỬ DỤNG HỆ THỐNG (USE CASE LIST)
*Được sắp xếp nghiêm ngặt theo Use Case ID từ UC-001 đến UC-028*

| Use Case ID | Tên Use Case | Actor chính | Actor hỗ trợ | Mục tiêu nghiệp vụ | Feature liên quan | Priority |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: |
| **UC-001** | Đăng ký và Đăng nhập Caregiver qua OTP | ACT-001 (Caregiver) | ACT-009 (SMS Gateway) | Xác thực số điện thoại và định danh an toàn cho Caregiver không cần mật khẩu. | F-001, F-003 | **P0** |
| **UC-002** | Đăng nhập Nhân viên Y tế và Xác thực 2FA | ACT-002 (Bác sĩ), ACT-003 (Điều dưỡng), ACT-004 (CSKH) | ACT-007 (System Admin) | Xác thực danh tính chuyên môn nhân viên y tế theo vai trò và cơ sở bệnh viện. | F-002, F-023 | **P0** |
| **UC-003** | Quét Mã QR Bàn Giao Liên Kết Hồ Sơ Bệnh Nhân | ACT-001 (Caregiver) | ACT-003 (Điều dưỡng), ACT-006 (Bệnh nhân) | Giải mã QR token từ phiếu xuất viện và thiết lập liên kết điện tử bảo mật với Care Plan. | F-004, F-021 | **P0** |
| **UC-004** | Quản lý Hồ sơ Định danh Bệnh Nhân | ACT-003 (Điều dưỡng), ACT-002 (Bác sĩ) | ACT-006 (Bệnh nhân), ACT-010 (HIS) | Tạo mới hoặc tra cứu thông tin hành chính, chẩn đoán mổ mắt và mắt can thiệp. | F-005 | **P0** |
| **UC-005** | Quản lý Danh mục Mẫu Kế Hoạch Chăm Sóc | ACT-002 (Bác sĩ điều trị) | ACT-005 (Clinical Approver) | Thiết lập và bảo trì các mẫu phác đồ chuẩn hóa theo loại phẫu thuật (Phaco, SILK). | F-006 | **P0** |
| **UC-006** | Phê Duyệt Lâm Sàng Master Template | ACT-005 (Clinical Approver / GCMO) | ACT-002 (Bác sĩ điều trị) | Thẩm định chuyên môn và ban hành chính thức các phiên bản Master Template toàn chuỗi. | F-007 | **P1** |
| **UC-007** | Cấu Hình Danh Mục Thuốc Mẫu Trong Template | ACT-002 (Bác sĩ điều trị) | ACT-005 (GCMO) | Cài đặt danh mục biệt dược, liều dùng, cữ dùng và khoảng cách giãn cách đệm mặc định. | F-006, F-009, F-010 | **P0** |
| **UC-008** | Cấu Hình Bộ Câu Hỏi Recovery Check & Red Flag | ACT-002 (Bác sĩ điều trị) | ACT-005 (GCMO) | Thiết lập câu hỏi sàng lọc định kỳ và các điều kiện kích hoạt cảnh báo nguy cấp. | F-006, F-016, F-017 | **P0** |
| **UC-009** | Cấu Hình Cẩm Nang Hướng Dẫn và Quy Tắc Sinh Hoạt trong Master Template | ACT-002 (Bác sĩ điều trị) | ACT-005 (Clinical Approver / GCMO) | Thiết lập cẩm nang 24h đầu, quy tắc nên làm/cần tránh (Do/Don't) và ngân hàng tình huống FAQ lâm sàng gắn liền với từng loại phẫu thuật mắt. | F-006, F-012, F-013, F-028 | **P0** |
| **UC-010** | Khởi Tạo và Cá Nhân Hóa Care Plan Bệnh Nhân | ACT-003 (Điều dưỡng xuất viện) | ACT-002 (Bác sĩ điều trị) | Nhân bản Master Template thành Care Plan thực tế trong <30 giây; điều chỉnh liều nếu cần. | F-008 | **P0** |
| **UC-010b** | Thực Hiện Bàn Giao Xuất Viện Tại Phòng Lưu Viện | ACT-003 (Điều dưỡng xuất viện) | ACT-001 (Caregiver), ACT-006 (Bệnh nhân) | Điều dưỡng dán khiên mắt, trao phiếu QR, hướng dẫn người nhà quét mã và dặn dò ra viện. | F-008, F-022 | **P0** |
| **UC-010c** | Xác Nhận Hoàn Tất Bàn Giao Lâm Sàng | ACT-003 (Điều dưỡng xuất viện) | ACT-001 (Caregiver) | Hệ thống ghi nhận trạng thái Care Plan chuyển sang ACTIVE và Caregiver đã liên kết. | F-004, F-008 | **P0** |
| **UC-011** | Tạo và Phát Hành Mã QR Xuất Viện | ACT-003 (Điều dưỡng xuất viện) | ACT-001 (Caregiver), ACT-006 (Bệnh nhân) | Hệ thống sinh mã token mã hóa ngẫu nhiên an toàn gắn với Care Plan đã ban hành. | F-021 | **P0** |
| **UC-012** | In Phiếu Hướng Dẫn Xuất Viện Kèm Mã QR | ACT-003 (Điều dưỡng xuất viện) | ACT-001 (Caregiver) | Xuất lệnh in trực tiếp Phiếu xuất viện khổ chuẩn có chứa mã QR sắc nét để bàn giao. | F-022 | **P0** |
| **UC-013** | Cấp Lại hoặc Thu Hồi Mã QR Bàn Giao | ACT-003 (Điều dưỡng), ACT-002 (Bác sĩ) | ACT-001 (Caregiver) | Tạo mã QR mới thay thế khi bị mất phiếu hoặc đổi phác đồ; vô hiệu hóa mã cũ. | F-021, F-022 | **P0** |
| **UC-014** | Xem Lịch Dùng Thuốc và Hướng Dẫn Nhỏ Mắt | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Hiển thị cữ thuốc trong ngày, mắt áp dụng, số giọt và hình ảnh hướng dẫn tra thuốc. | F-009, F-011 | **P0** |
| **UC-015** | Xác Nhận Dùng Thuốc và Kích Hoạt Bộ Đếm Giãn Cách | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Ghi nhận cữ thuốc hoàn thành và tự động đếm lùi 5–10 phút giữa 2 loại thuốc nhỏ mắt. | F-009, F-010 | **P0** |
| **UC-016** | Xem Cẩm Nang 24h Đầu và Bảng Nên Làm / Cần Tránh | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Tra cứu tức thì các hành động cấp thiết trong 24h đầu và danh mục sinh hoạt được phép/kiêng cữ. | F-012, F-013 | **P0** |
| **UC-017** | Xem Lộ Trình Học Viện Caregiver (Infographic Tĩnh) | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Học tập kiến thức chăm sóc mắt qua các infographic trực quan tinh gọn theo tiến trình hồi phục. | F-014 | **P1** |
| **UC-018** | Xem Lịch Tái Khám và Nhận Thông Báo Nhắc Hẹn | ACT-001 (Caregiver) | ACT-009 (SMS Gateway), ACT-004 (CSKH) | Theo dõi 5 mốc tái khám chuẩn VISI và nhận thông báo nhắc lịch tự động trước 24 giờ. | F-015, F-027 | **P0** |
| **UC-019** | Thực Hiện Khảo Sát Đánh Giá Phục Hồi Định Kỳ | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân), ACT-002 (Bác sĩ) | Trả lời 3–5 câu hỏi sàng lọc định kỳ để hệ thống tự động phân loại Xanh/Vàng/Đỏ. | F-016 | **P0** |
| **UC-020** | Kích Hoạt Xử Lý Biến Chứng Báo Động Đỏ | ACT-001 (Caregiver) | ACT-004 (CSKH), ACT-008 (Cấp cứu) | Chuyển giao diện khẩn cấp, cung cấp nút gọi 1 chạm đến Hotline VISI và đẩy báo động viện. | F-017 | **P0** |
| **UC-021** | Giám Sát Dashboard Phục Hồi Bệnh Nhân Tập Trung | ACT-004 (CSKH), ACT-003 (Điều dưỡng) | ACT-002 (Bác sĩ điều trị) | Theo dõi danh sách toàn bộ bệnh nhân của cơ sở theo trạng thái tuân thủ và hồi phục. | F-018 | **P0** |
| **UC-022** | Tiếp Nhận, Phân Loại và Điều Phối Cảnh Báo Red Flag | ACT-004 (CSKH / Medical Monitor) | ACT-002 (Bác sĩ), ACT-001 (Caregiver) | Tiếp nhận ca cảnh báo đỏ, chuyển trạng thái xử lý và gọi điện can thiệp khẩn cấp <5 phút. | F-019 | **P0** |
| **UC-022b** | Tự Động Leo Thang Cảnh Báo Red Flag Chưa Xử Lý | ACT-007 (System) | ACT-004 (CSKH), ACT-002 (Bác sĩ trực) | Tự động phát chuông và gửi tin nhắn cảnh báo lên Bác sĩ trực nếu ca đỏ bị trễ quá 15 phút. | F-019 | **P0** |
| **UC-023** | Ghi Nhận Nhật Ký Cuộc Gọi và Can Thiệp Lâm Sàng | ACT-004 (CSKH), ACT-003 (Điều dưỡng) | ACT-002 (Bác sĩ điều trị) | Ghi nhận chi tiết kết quả cuộc gọi tư vấn, lời dặn y tế và trạng thái bệnh nhân vào hồ sơ. | F-020 | **P0** |
| **UC-024** | Tra Cứu Tình Huống Chăm Sóc Khẩn Cấp (FAQ Lâm Sàng) | ACT-001 (Caregiver) | ACT-006 (Bệnh nhân) | Tra cứu nhanh chỉ dẫn chuẩn y khoa theo tình huống thường gặp tại nhà (dính nước, quên nhỏ thuốc). | F-028 | **P1** |
| **UC-025** | Kích Hoạt Chế Độ Trợ Năng Nhãn Khoa (Accessibility) | ACT-006 (Bệnh nhân), ACT-001 (Caregiver) | Không có | Chuyển giao diện sang chữ lớn, tương phản cao và bật tính năng đọc âm thanh tiếng Việt. | F-026 | **P1** |
| **UC-026** | Quản Lý Tài Khoản Nhân Viên và Phân Quyền Cơ Sở | ACT-007 (Quản trị viên hệ thống) | ACT-002, ACT-003, ACT-004 | Khởi tạo tài khoản và phân quyền truy cập nghiêm ngặt theo cơ sở trực thuộc chuỗi VISI. | F-023 | **P0** |
| **UC-027** | Tra Cứu Nhật Ký Kiểm Toán Hệ Thống (Audit Trail) | ACT-007 (Quản trị viên hệ thống) | Ban Giám Đốc | Truy vấn và kết xuất nhật ký thao tác lâm sàng phục vụ kiểm tra an toàn thông tin và pháp lý. | F-024 | **P1** |
| **UC-028** | Kết Xuất Báo Cáo Vận Hành và Chỉ Số Tuân Thủ KPI | Ban Giám Đốc (CEO, COO) | ACT-007 (System Admin) | Tổng hợp các chỉ số KPIs: tỷ lệ kích hoạt QR, tỷ lệ tuân thủ thuốc, tỷ lệ tái khám theo chi nhánh. | F-025 | **P1** |

---

## 6. MA TRẬN TRUY XUẤT YÊU CẦU TÓM TẮT (TRACEABILITY SUMMARY)

| Thành phần kiểm tra | Tổng số lượng | Trạng thái sắp xếp | Tỷ lệ phủ (Coverage) | Đánh giá tính toàn vẹn |
| :--- | :---: | :---: | :---: | :--- |
| **Features (Chức năng)** | **28** (`F-001` .. `F-028`) | 100% Sorted by ID | 100% được map với US và UC | **Đạt chuẩn (No Orphan Features)** |
| **Actors (Tác nhân)** | **10** (`ACT-001` .. `ACT-010`) | 100% Sorted by ID | 100% có quyền hạn & nhiệm vụ rõ ràng | **Đạt chuẩn (Separation of Duties)** |
| **User Stories (Story)** | **35** (`US-001` .. `US-035`) | 100% Sorted by ID | 100% thuộc Feature hợp lệ | **Đạt chuẩn (Full INVEST Criteria)** |
| **Use Cases (Ca sử dụng)**| **31** (`UC-001` .. `UC-028`)* | 100% Sorted by ID | 100% có Actor chính & hỗ trợ | **Đạt chuẩn (End-to-End Operational Flow)** |

*\*Ghi chú:* Danh mục Use Case bao gồm 28 mã số chính (`UC-001` đến `UC-028`) cùng 3 ca mở rộng vận hành chuyên sâu (`UC-010b` Bàn giao phòng lưu viện, `UC-010c` Xác nhận hoàn tất bàn giao và `UC-022b` Tự động leo thang cảnh báo đỏ).

---

## 7. CÁC NỘI DUNG CẦN XÁC NHẬN VỚI LÃNH ĐẠO VISI MEDICAL GROUP (VALIDATION CHECKLIST)

1. **Thời gian đếm ngược giãn cách giữa 2 loại thuốc nhỏ mắt (F-010, US-017, UC-015):** 
   * *Nội dung:* Xác nhận thông số mặc định là **10 phút** (theo nghiên cứu lâm sàng của BS.CKII Trần Bá Kiền) hay **5 phút** (theo tiêu chuẩn dược lý nhãn khoa thông thường). 
   * *Đề xuất kỹ thuật:* Hệ thống đã thiết lập tham số động (Dynamic Buffer Interval) trong Master Template, cho phép Bác sĩ điều chỉnh linh hoạt theo từng loại thuốc (hỗn dịch, dung dịch nước hay gel/mỡ).
2. **Quy chế thường trực Hotline cấp cứu 0395 151 151 (F-017, US-024, UC-020, UC-022):** 
   * *Nội dung:* Xác nhận cam kết nhân sự trực tổng đài 24/7 với Giám đốc Vận hành ThS. Nguyễn Tiến Đức để đảm bảo SLA tiếp nhận cuộc gọi khẩn cấp trong vòng dưới 5 phút.
   * *Phương án dự phòng:* Tự động kích hoạt cơ chế leo thang cảnh báo (UC-022b) gửi tin nhắn SMS khẩn cấp tới Bác sĩ trực cơ sở nếu ca đỏ chưa được xử lý sau 15 phút.
3. **Khổ in và chuẩn máy in Phiếu xuất viện tại phòng lưu viện (F-022, US-009, UC-012):** 
   * *Nội dung:* Khảo sát chủng loại máy in nhiệt/laser sẵn có tại quầy điều dưỡng Bệnh viện Mắt VISI Thủ Đức để chốt quy cách in (Khổ giấy A5 chuẩn, decal dán sổ khám bệnh, hay tích hợp trực tiếp vào Giấy ra viện A4 hiện hữu).
