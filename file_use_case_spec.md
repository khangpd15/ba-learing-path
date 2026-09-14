# II. Đặc Tả Yêu Cầu Nghiệp Vụ (Requirement Specifications)

## 1. Danh Mục và Đặc Tả Chi Tiết Các Use Case

---

## PHÂN HỆ 1: NGƯỜI CHĂM SÓC (CAREGIVER)

---

### 1.1 UC-01: Đăng nhập hệ thống (Caregiver Login System)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-01 | | |
| **Tên Use Case (Use Case Name)** | Đăng nhập hệ thống (Caregiver Login System) | | |
| **Người tạo (Created by)** | Phùng Đình Khang | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 24/01/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Người chăm sóc (Caregiver) | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả cách Caregiver đăng nhập vào hệ thống bằng số điện thoại đã đăng ký và xác thực mã OTP để truy cập các chức năng Kế hoạch chăm sóc (Care Plan), Học viện Caregiver (Caregiver Academy) và theo dõi bệnh nhân. | | |
| **Mục tiêu (Goal)** | Xác thực danh tính Caregiver an toàn và cấp quyền truy cập các tính năng chăm sóc bệnh nhân | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver nhấn nút “Đăng nhập” trên màn hình đăng nhập. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Caregiver có tài khoản đã đăng ký hợp lệ trong hệ thống.<br>2. Tài khoản không bị khóa, vô hiệu hóa hoặc xóa.<br>3. Số điện thoại đăng ký sẵn sàng nhận tin nhắn SMS chứa OTP. | | |
| **Điều kiện sau (Post-conditions)** | 1. Caregiver đăng nhập thành công vào hệ thống.<br>2. Hệ thống thiết lập phiên làm việc bảo mật.<br>3. Caregiver được chuyển hướng đến Trang chủ hiển thị các bệnh nhân và Care Plan được phân quyền. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver nhấn nút “Đăng nhập” | Hệ thống hiển thị giao diện đăng nhập với ô nhập số điện thoại |
| | 2 | Caregiver nhập số điện thoại đã đăng ký và nhấn “Tiếp tục” | Hệ thống kiểm tra định dạng số điện thoại, kiểm tra tài khoản đang hoạt động, tạo mã OTP ngẫu nhiên, gửi mã OTP qua SMS và hiển thị màn hình xác thực OTP |
| | 3 | Caregiver nhập mã OTP nhận được từ tin nhắn | Hệ thống kiểm tra tính chính xác và thời hạn hiệu lực của OTP, chứng thực tài khoản, tạo phiên làm việc bảo mật và chuyển hướng Caregiver đến Trang chủ |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Đăng nhập sau khi quét mã QR** | 1 | Caregiver quét mã QR do bệnh viện cấp khi chưa đăng nhập ứng dụng | Hệ thống mở trang Đăng nhập và lưu tạm mã định danh QR trong phiên làm việc |
| | 2 | Caregiver thực hiện đăng nhập bằng số điện thoại và OTP | Hệ thống xác thực đăng nhập, kiểm tra mã QR, tự động tạo liên kết Caregiver với bệnh nhân và mở thẳng Kế hoạch chăm sóc của bệnh nhân |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Số điện thoại không hợp lệ** | 1 | Caregiver nhập số điện thoại sai định dạng | Hệ thống phát hiện định dạng sai, chặn xử lý và hiển thị thông báo lỗi: “Số điện thoại không hợp lệ.” |
| **E2: Tài khoản chưa đăng ký** | 1 | Caregiver nhập số điện thoại chưa tồn tại trong hệ thống | Hệ thống phát hiện không tìm thấy tài khoản, hiển thị thông báo: “Số điện thoại chưa được đăng ký trong hệ thống.” kèm nút chuyển sang Đăng ký |
| **E3: Sai mã OTP** | 1 | Caregiver nhập không đúng mã OTP | Hệ thống từ chối xác thực và hiển thị: “Mã OTP không chính xác.” |
| **E4: Mã OTP hết hạn** | 1 | Caregiver nhập mã OTP sau thời gian hiệu lực | Hệ thống từ chối xác thực, hiển thị: “Mã OTP đã hết hạn.” và cung cấp nút gửi lại mã mới |
| **E5: Tài khoản bị vô hiệu hóa** | 1 | Caregiver cố gắng đăng nhập vào tài khoản bị khóa | Hệ thống từ chối đăng nhập và thông báo: “Tài khoản đã bị vô hiệu hóa.” |
| **E6: Vượt giới hạn yêu cầu OTP** | 1 | Caregiver nhấn gửi lại OTP quá nhiều lần trong thời gian ngắn | Hệ thống tạm thời chặn gửi OTP và thông báo thời gian chờ cần thiết |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR1, BR2, BR3 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR1 | Kiểm tra tính hợp lệ của số điện thoại | Hệ thống phải xác thực định dạng số điện thoại của Caregiver trước khi khởi tạo quy trình gửi mã OTP. |
| BR2 | Xác thực mã OTP | Mã OTP phải chính xác, khớp với mã hệ thống đã sinh ra và được sử dụng trong khoảng thời gian hiệu lực quy định. |
| BR3 | Phân quyền tài khoản | Chỉ những tài khoản Caregiver đang ở trạng thái hoạt động (Active) mới được phép xác thực thành công và truy cập các chức năng của Caregiver. |

---

### 1.2 UC-02: Quét mã QR liên kết hồ sơ bệnh nhân (Scan QR Code)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-02 | | |
| **Tên Use Case (Use Case Name)** | Quét mã QR liên kết hồ sơ bệnh nhân (Scan QR Code to Link Patient Profile) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Tác nhân chính: Caregiver<br>Tác nhân phụ: Bác sĩ (Doctor), Bệnh nhân (Patient) | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả cách Caregiver quét mã QR do bệnh viện phát hành gắn với Kế hoạch chăm sóc của bệnh nhân, xác thực thông tin bệnh nhân và thiết lập liên kết bảo mật giữa Caregiver và Kế hoạch chăm sóc. | | |
| **Mục tiêu (Goal)** | Liên kết tài khoản Caregiver với hồ sơ bệnh nhân và Kế hoạch chăm sóc tương ứng bằng mã QR | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver nhấn nút “Quét mã QR” trên trang chủ ứng dụng. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Caregiver đã đăng nhập vào hệ thống.<br>2. Ứng dụng được cấp quyền sử dụng camera [ASSUMPTION].<br>3. Mã QR liên kết với Kế hoạch chăm sóc đang Hoạt động (Active) đã được Bác sĩ phát hành. | | |
| **Điều kiện sau (Post-conditions)** | 1. Caregiver liên kết thành công với Kế hoạch chăm sóc của bệnh nhân.<br>2. Caregiver được phân quyền truy cập thông tin chăm sóc của bệnh nhân này.<br>3. Hệ thống ghi nhận lịch sử liên kết vào nhật ký kiểm toán. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver nhấn nút “Quét mã QR” | Hệ thống mở camera và hiển thị khung ngắm quét mã |
| | 2 | Caregiver hướng camera vào mã QR trên phiếu xuất viện của bệnh nhân | Hệ thống quét và giải mã dữ liệu, kiểm tra tính hợp lệ và hiển thị tóm tắt thông tin bệnh nhân: Họ và tên, Năm sinh/Tuổi, Loại phẫu thuật, Bác sĩ điều trị |
| | 3 | Caregiver kiểm tra thông tin và nhấn “Xác nhận liên kết” | Hệ thống lưu thông tin liên kết, hiển thị thông báo: “Liên kết hồ sơ bệnh nhân thành công.” và chuyển đến Bảng điều khiển Care Plan của bệnh nhân |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Hủy liên kết** | 1 | Caregiver đối chiếu thông tin phát hiện sai bệnh nhân và nhấn “Hủy bỏ” | Hệ thống hủy yêu cầu liên kết, không lưu dữ liệu và đưa Caregiver về Trang chủ |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Không có quyền camera** | 1 | Caregiver từ chối cấp quyền camera | Hệ thống hiển thị: “Vui lòng cấp quyền truy cập camera để quét mã QR.” kèm nút mở Cài đặt thiết bị |
| **E2: Mã QR không hợp lệ** | 1 | Caregiver quét mã QR không đúng chuẩn hệ thống | Hệ thống báo lỗi: “Mã QR không hợp lệ hoặc không thuộc hệ thống.” và tiếp tục mở khung quét |
| **E3: Kế hoạch chăm sóc đã đóng** | 1 | Quét mã QR của Kế hoạch chăm sóc đã hoàn tất hoặc bị vô hiệu hóa | Hệ thống từ chối và hiển thị: “Kế hoạch chăm sóc gắn với mã QR này đã kết thúc hoặc bị vô hiệu hóa.” |
| **E4: Đã liên kết trước đó** | 1 | Quét mã QR của bệnh nhân đã có trong danh sách | Hệ thống thông báo: “Bạn đã được liên kết với hồ sơ bệnh nhân này.” và chuyển đến màn hình chi tiết bệnh nhân |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR4, BR5 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR4 | Tính hợp lệ của mã QR | Hệ thống phải xác thực mã QR được tạo từ hệ thống bệnh viện và liên kết với Kế hoạch chăm sóc đang ở trạng thái Hoạt động (Active) trước khi cho phép liên kết. |
| BR5 | Tính toàn vẹn liên kết Caregiver - Bệnh nhân | Một Caregiver có thể liên kết với nhiều bệnh nhân, nhưng mỗi liên kết phải có sự xác nhận rõ ràng từ Caregiver và phải được lưu vết kèm thời gian và thông tin kiểm toán. |

---

### 1.3 UC-03: Xem hướng dẫn chăm sóc, Learning Path và kiểm tra kiến thức (View Care Instructions, Learning Path & Mini Quiz)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-03 | | |
| **Tên Use Case (Use Case Name)** | Xem hướng dẫn chăm sóc, Learning Path và kiểm tra kiến thức (View Care Instructions, Learning Path & Mini Quiz) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Người chăm sóc (Caregiver) | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả cách Caregiver truy cập và học các hướng dẫn chăm sóc theo lộ trình (Learning Path) gồm các giai đoạn (phase) bằng video hoặc hình ảnh minh họa được Bác sĩ cấu hình riêng cho loại phẫu thuật. Khi Caregiver xem hết các phase video hoặc hình ảnh của bài học, ở cuối sẽ xuất hiện 3 câu hỏi trắc nghiệm kiểm tra nhanh (Mini Quiz) để củng cố kiến thức và đánh giá sự thấu hiểu của Caregiver. | | |
| **Mục tiêu (Goal)** | Giúp Caregiver nắm vững kiến thức, thao tác chăm sóc bệnh nhân chuẩn y khoa và kiểm tra nhanh mức độ hiểu bài qua 3 câu hỏi trắc nghiệm ở cuối bài học | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver chọn “Learning Path” trên bảng điều khiển bệnh nhân. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Caregiver đã đăng nhập.<br>2. Caregiver đã liên kết với Kế hoạch chăm sóc đang hoạt động của bệnh nhân. | | |
| **Điều kiện sau (Post-conditions)** | 1. Nội dung hướng dẫn được hiển thị đầy đủ.<br>2. Tiến trình học tập và kết quả trả lời 3 câu hỏi kiểm tra được ghi nhận vào hệ thống. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver nhấn vào mục “Learning Path” | Hệ thống hiển thị danh sách các chủ đề/giai đoạn (phase) bài học theo cấu hình phẫu thuật của bệnh nhân |
| | 2 | Caregiver chọn một chủ đề cần xem (ví dụ: “Nên làm gì 24h đầu”) | Hệ thống hiển thị chi tiết bài học bao gồm các phase video, hình ảnh minh họa, infographic và văn bản hướng dẫn do Bác sĩ thiết lập |
| | 3 | Caregiver xem tuần tự hết các phase video hoặc hình ảnh của bài học | Hệ thống chuyển đến phần cuối bài học và tự động hiển thị 3 câu hỏi trắc nghiệm kiểm tra nhanh phù hợp với bài học |
| | 4 | Caregiver chọn đáp án cho 3 câu hỏi và nhấn “Nộp bài” | Hệ thống chấm điểm tự động, hiển thị số câu đúng/sai, chỉ ra đáp án đúng kèm lời giải thích y khoa ngắn gọn, và ghi nhận trạng thái đã hoàn thành bài học cho Caregiver |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Làm lại câu hỏi kiểm tra kiến thức** | 1 | Caregiver nhấn nút “Làm lại” tại màn hình kết quả bài kiểm tra | Hệ thống làm mới các câu trả lời và cho phép Caregiver chọn lại đáp án từ đầu |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Lỗi tải video / hình ảnh** | 1 | Mạng yếu làm gián đoạn tải video hướng dẫn | Hệ thống tự động chuyển sang hiển thị văn bản mô tả dự phòng và hiển thị nút “Thử tải lại” |
| **E2: Chưa chọn hết 3 câu hỏi kiểm tra** | 1 | Caregiver nhấn “Nộp bài” khi chưa trả lời đủ 3 câu hỏi | Hệ thống chặn nộp bài, làm nổi bật các câu hỏi còn thiếu và hiển thị: “Vui lòng trả lời đầy đủ cả 3 câu hỏi kiểm tra trước khi hoàn tất bài học.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR6, BR7, BR8 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR6 | Phạm vi nội dung được cấp quyền | Caregiver chỉ được truy cập các nội dung Learning Path và hướng dẫn thuốc thuộc Kế hoạch chăm sóc của bệnh nhân đã được liên kết với mình. |
| BR7 | Nguồn gốc nội dung y khoa | Mọi hướng dẫn chuyên môn, liều lượng thuốc và thời gian sử dụng phải xuất phát từ cấu hình của Bác sĩ/Bệnh viện. Hệ thống tuyệt đối không tự động suy diễn hoặc điều chỉnh khuyến nghị y tế. |
| BR8 | Hoàn thành bài kiểm tra không gây chặn chức năng | Kết quả trả lời 3 câu hỏi kiểm tra được ghi nhận nhằm mục đích theo dõi tiến độ học tập; kết quả kiểm tra không làm khóa hay hạn chế quyền truy cập của Caregiver đối với các tính năng chăm sóc bệnh nhân khác. |

---

### 1.5 UC-05: Xem hướng dẫn Nên làm & Cần tránh (Do & Don't)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-05 | | |
| **Tên Use Case (Use Case Name)** | Xem hướng dẫn Nên làm & Cần tránh (View Do & Don't Guidance) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Người chăm sóc (Caregiver) | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả cách Caregiver tra cứu danh mục các việc ĐƯỢC PHÉP LÀM (Do) và các việc BỊ CẤM / CẦN TRÁNH (Don't) trong sinh hoạt hàng ngày được Bác sĩ thiết lập riêng cho loại phẫu thuật của bệnh nhân. | | |
| **Mục tiêu (Goal)** | Giúp Caregiver nhận biết rõ các hành vi an toàn và các hành vi gây nguy cơ tổn thương vết mổ của bệnh nhân | | |
| **Tác nhân kích hoạt (Trigger)** | Caregiver nhấn vào mục “Nên làm & Cần tránh” (Do & Don't) trên bảng điều khiển bệnh nhân. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Caregiver đã đăng nhập.<br>2. Bệnh nhân đã được liên kết Kế hoạch chăm sóc có cấu hình Do & Don't. | | |
| **Điều kiện sau (Post-conditions)** | Danh mục các việc Nên làm và Cần tránh được hiển thị trực quan, phân loại rõ ràng cho Caregiver. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver nhấn chọn mục “Nên làm & Cần tránh” | Hệ thống tải cấu hình Do & Don't từ Kế hoạch chăm sóc của bệnh nhân |
| | 2 | Caregiver xem màn hình chia 2 cột rõ rệt: Cột xanh (Việc NÊN LÀM - Do) và Cột đỏ (Việc CẦN TRÁNH - Don't) | Hệ thống hiển thị chi tiết từng mục gồm: Tiêu đề, Mô tả giải thích lý do y khoa, và thời gian áp dụng (ví dụ: kiêng gội đầu úp mặt trong 7 ngày đầu) |
| | 3 | Caregiver nhấn vào một mục để xem chi tiết hướng dẫn minh họa | Hệ thống hiển thị hình ảnh/lưu ý chi tiết của hành vi đó |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Lọc theo nhóm sinh hoạt** | 1 | Caregiver chọn bộ lọc nhóm sinh hoạt (Vệ sinh cá nhân, Vận động / Thể thao, Ăn uống, Giấc ngủ) | Hệ thống cập nhật danh sách hiển thị tương ứng với nhóm sinh hoạt được chọn |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Chưa có cấu hình Do & Don't** | 1 | Kế hoạch chăm sóc chưa có nội dung Do & Don't được duyệt | Hệ thống hiển thị thông báo: “Danh mục hướng dẫn đang được Bác sĩ cập nhật. Vui lòng liên hệ Bác sĩ nếu cần hỗ trợ.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7, BR22 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR22 | Tính hiển thị trực quan danh mục Do & Don't | Danh mục Do & Don't phải được phân định rõ ràng bằng màu sắc quy chuẩn (Xanh lá cho việc Nên làm, Đỏ cảnh báo cho việc Cần tránh) và gắn liền với mốc thời gian kiêng cữ cụ thể sau phẫu thuật. |

---

### 1.6 UC-06: Xem lịch dùng thuốc và nhận thông báo nhắc thuốc

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-06 | | |
| **Tên Use Case (Use Case Name)** | Xem lịch dùng thuốc và nhận thông báo nhắc thuốc (View Medication Schedule & Reminders) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Tác nhân chính: Caregiver<br>Tác nhân phụ: Hệ thống thông báo (Notification Service) | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả cách Caregiver theo dõi lịch dùng thuốc (uống thuốc và nhỏ mắt) trong ngày của bệnh nhân, đánh dấu đã uống, và nhận thông báo nhắc nhở tự động theo từng cữ dùng thuốc. | | |
| **Mục tiêu (Goal)** | Đảm bảo bệnh nhân dùng đúng loại thuốc, đúng liều, đúng cữ thời gian và đúng khoảng cách an toàn | | |
| **Tác nhân kích hoạt (Trigger)** | 1. Đến giờ cữ thuốc theo lịch (Hệ thống tự động kích hoạt thông báo).<br>2. Caregiver mở mục “Lịch dùng thuốc” trên ứng dụng. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Kế hoạch chăm sóc của bệnh nhân đã có đơn thuốc được Bác sĩ kích hoạt.<br>2. Thiết bị Caregiver đã bật quyền nhận thông báo ứng dụng [ASSUMPTION]. | | |
| **Điều kiện sau (Post-conditions)** | Lịch sử uống/nhỏ thuốc được ghi nhận; thông báo nhắc nhở được gửi đúng giờ. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Đến giờ dùng thuốc đã định (ví dụ: 08:00 Sáng) | Hệ thống tự động gửi thông báo đẩy (Push Notification) đến máy Caregiver: “Đến giờ nhỏ thuốc [Tên thuốc] cho bệnh nhân [Tên bệnh nhân]” |
| | 2 | Caregiver nhấn vào thông báo hoặc truy cập mục “Lịch dùng thuốc” | Hệ thống hiển thị danh sách các cữ thuốc trong ngày: Sáng, Trưa, Chiều, Tối kèm chỉ dẫn (uống trước/sau ăn, số giọt nhỏ mắt, thứ tự nhỏ) |
| | 3 | Caregiver thực hiện cho bệnh nhân dùng thuốc và nhấn “Đánh dấu đã dùng thuốc” | Hệ thống ghi nhận trạng thái đã hoàn thành cữ thuốc kèm mốc thời gian thực tế, chuyển trạng thái hiển thị sang màu xanh |
| **Luồng thay thế (Alternative Flow)** | Không áp dụng | Không có luồng thay thế | |
| **Luồng ngoại lệ (Exception Flow)** | Không áp dụng | Không có luồng ngoại lệ | |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7, BR23 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR23 | Ràng buộc khoảng cách thời gian nhỏ mắt | Nếu bệnh nhân được kê đơn từ 2 loại thuốc nhỏ mắt trở lên trong cùng một cữ, hệ thống phải kích hoạt bộ đếm thời gian giãn cách bắt buộc tối thiểu 5 phút giữa các lần nhỏ để tránh rửa trôi thuốc. |

---

### 1.7 UC-07: Xem lịch tái khám và nhận thông báo tái khám

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-07 | | |
| **Tên Use Case (Use Case Name)** | Xem lịch tái khám và nhận thông báo tái khám (View Follow-up Schedule & Reminders) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Tác nhân chính: Caregiver<br>Tác nhân phụ: Hệ thống thông báo (Notification Service), Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả cách Caregiver xem thông tin các buổi hẹn tái khám (ngày giờ, địa điểm, bác sĩ phụ trách, lưu ý trước khám) và tự động nhận thông báo nhắc nhở trước ngày tái khám. | | |
| **Mục tiêu (Goal)** | Nhắc nhở và đảm bảo bệnh nhân đến tái khám đúng hẹn theo chỉ định của Bác sĩ | | |
| **Tác nhân kích hoạt (Trigger)** | 1. Hệ thống tự động gửi thông báo trước ngày hẹn tái khám (trước 1 ngày hoặc 2 ngày).<br>2. Caregiver truy cập mục “Lịch tái khám” trên ứng dụng. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đã thiết lập lịch hẹn tái khám trong Kế hoạch chăm sóc của bệnh nhân. | | |
| **Điều kiện sau (Post-conditions)** | Thông tin lịch hẹn tái khám được hiển thị chi tiết cho Caregiver; thông báo nhắc hẹn được gửi thành công đến thiết bị. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Đến mốc thời gian quy định trước ngày tái khám (ví dụ: trước 24 giờ) | Hệ thống gửi thông báo đẩy và tin nhắn nhắc hẹn tới Caregiver: “Nhắc hẹn: Bệnh nhân [Tên] có lịch tái khám vào ngày [Ngày] lúc [Giờ] tại [Địa điểm]” |
| | 2 | Caregiver nhấn vào thông báo hoặc chọn mục “Lịch tái khám” trên ứng dụng | Hệ thống hiển thị chi tiết phiếu hẹn tái khám: Ngày khám, Khung giờ, Phòng khám, Bác sĩ phụ trách, Các giấy tờ và thuốc cần mang theo, Các lưu ý nhịn ăn/nhỏ mắt trước khám |
| **Luồng thay thế (Alternative Flow)** | Không áp dụng | Không có luồng thay thế | |
| **Luồng ngoại lệ (Exception Flow)** | Không áp dụng | Không có luồng ngoại lệ | |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR24 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR24 | Thời gian kích hoạt nhắc hẹn tái khám | Hệ thống phải tự động kích hoạt thông báo nhắc hẹn tái khám tối thiểu trước 24 giờ và trước 2 giờ tính đến thời điểm hẹn của buổi khám. |

---

### 1.8 UC-08: Nộp bảng kiểm phục hồi Recovery Check (Submit Recovery Check)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-08 | | |
| **Tên Use Case (Use Case Name)** | Nộp bảng kiểm phục hồi Recovery Check (Submit Recovery Check) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Tác nhân chính: Caregiver<br>Tác nhân phụ: Bác sĩ (Doctor), Hệ thống Bệnh viện | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả cách Caregiver nhận thông báo định kỳ và bắt buộc trả lời 3–5 câu hỏi khảo sát về tình trạng phục hồi của bệnh nhân, để hệ thống đối chiếu tiêu chí (Bình thường / Cần chú ý / Red Flag) và gửi kết quả cho Bác sĩ theo dõi. | | |
| **Mục tiêu (Goal)** | Thu thập dữ liệu phục hồi hậu phẫu của bệnh nhân từ người chăm sóc một cách có cấu trúc nhằm sớm phát hiện các biến chứng bất thường | | |
| **Tác nhân kích hoạt (Trigger)** | Hệ thống gửi thông báo đến hạn Recovery Check hoặc Caregiver bấm vào banner nhắc nhở trên trang chủ. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Caregiver đã đăng nhập và liên kết với Kế hoạch chăm sóc đang hoạt động.<br>2. Đã đến mốc thời gian làm Recovery Check (ví dụ: Ngày 1, Ngày 3, Ngày 7...) do Bác sĩ thiết lập. | | |
| **Điều kiện sau (Post-conditions)** | 1. Toàn bộ câu trả lời, thời gian nộp và phân loại trạng thái được lưu vào hệ thống.<br>2. Bảng theo dõi hồi phục của Bác sĩ được cập nhật.<br>3. Nếu có dấu hiệu bất thường/Red Flag, hệ thống gắn cờ cảnh báo và kích hoạt luồng khẩn cấp (UC-09). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Caregiver mở thông báo hoặc nhấn vào banner nhắc nhở Recovery Check | Hệ thống hiển thị bảng câu hỏi bắt buộc (3–5 câu hỏi về mức độ đau, đỏ mắt, tuân thủ dùng thuốc) của mốc phục hồi hiện tại (ví dụ: Ngày 3) |
| | 2 | Caregiver trả lời đầy đủ các câu hỏi dựa trên tình trạng thực tế của bệnh nhân và nhấn “Gửi kết quả” | Hệ thống kiểm tra tính đầy đủ, đối chiếu câu trả lời với tiêu chí đã cấu hình, và phân loại trạng thái là “Bình thường” (Normal) |
| | 3 | Hệ thống lưu kết quả với thời gian và ID Caregiver | Hệ thống hiển thị thông báo xác nhận: “Đã ghi nhận thông tin phục hồi. Bệnh nhân đang hồi phục tốt theo tiến độ.” và cập nhật Bảng điều khiển của Bác sĩ |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Trạng thái kích hoạt “Cần chú ý”** | 1 | Tại Bước 2 của Luồng chính, hệ thống phát hiện câu trả lời vượt ngưỡng lưu ý nhẹ (ví dụ: đau nhẹ kéo dài, khô mắt) | Hệ thống phân loại trạng thái “Cần chú ý”, đánh dấu màu vàng trên Bảng điều khiển Bác sĩ, và hiển thị lời khuyên trấn an, nhắc Caregiver theo dõi sát và nhỏ thuốc đúng giờ |
| **A2: Trạng thái kích hoạt “Red Flag”** | 1 | Tại Bước 2 của Luồng chính, câu trả lời thỏa mãn điều kiện dấu hiệu khẩn cấp Red Flag (ví dụ: đau dữ dội đột ngột, giảm thị lực, chảy máu/chảy dịch nhiều) | Hệ thống lập tức phân loại “Red Flag”, đánh dấu màu đỏ khẩn cấp trên màn hình Bác sĩ và tự động chuyển tiếp Caregiver sang màn hình Hành động Khẩn cấp (UC-09) |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Bỏ sót câu hỏi bắt buộc** | 1 | Caregiver nhấn “Gửi kết quả” khi còn câu hỏi để trống | Hệ thống chặn gửi, viền đỏ các câu hỏi còn thiếu và hiển thị: “Đây là bảng kiểm bắt buộc. Vui lòng trả lời toàn bộ câu hỏi trước khi gửi.” (BR9) |
| **E2: Lỗi kết nối mạng khi gửi dữ liệu** | 1 | Mất mạng khi đang tải câu trả lời lên máy chủ | Hệ thống lưu tạm các câu trả lời vào bộ nhớ cục bộ, hiển thị: “Không thể gửi dữ liệu do mất kết nối mạng. Dữ liệu đã được lưu tạm. Vui lòng kiểm tra mạng và nhấn Thử lại.” |
| **E3: Quá hạn nộp bảng kiểm (Overdue Recovery Check)** | 1 | Caregiver quên nộp hoặc bận không mở ứng dụng, quá hạn mốc kiểm tra quy định | Hệ thống kích hoạt cơ chế leo thang cảnh báo theo 3 tầng bảo vệ (BR12):<br>- **Sau 2 giờ:** Gửi lại Push Notification lần 2 với mức ưu tiên cao (âm báo khẩn).<br>- **Sau 4–6 giờ:** Kích hoạt kênh dự phòng gửi tin nhắn SMS / Zalo ZNS đến Caregiver; đồng thời tự động kích hoạt nguyên lý an toàn y khoa "Default to Unsafe", gắn cờ cảnh báo quá hạn màu cam 🟠 lên Dashboard Bác sĩ ("Bệnh nhân [Tên] đã quá hạn kiểm tra Day X 4 tiếng chưa phản hồi").<br>- **Tại Bác sĩ:** Hiển thị nút gọi nhanh "Gọi nhắc Caregiver" trên màn hình giám sát để Bác sĩ/Điều dưỡng can thiệp chủ động. |
| **Mức độ ưu tiên (Priority)** | Cao (High - Trọng yếu về an toàn điều trị) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR9, BR10, BR12 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR9 | Bắt buộc hoàn thành bảng kiểm Recovery Check | Caregiver bắt buộc phải trả lời đầy đủ toàn bộ 3–5 câu hỏi trong bảng kiểm Recovery Check mới có thể nộp kết quả lên hệ thống. |
| BR10 | Giới hạn chẩn đoán và phân loại trạng thái | Hệ thống chỉ đối chiếu câu trả lời với tiêu chí Bác sĩ đã cấu hình để phân nhóm (Bình thường, Cần chú ý, Red Flag) nhằm hỗ trợ nhân viên y tế theo dõi. Hệ thống tuyệt đối không tự đưa ra kết luận chẩn đoán y khoa. |
| BR12 | Cơ chế xử lý quá hạn và cảnh báo leo thang Recovery Check (Default to Unsafe) | Khi Caregiver không nộp bảng kiểm trong khung giờ quy định, hệ thống tự động kích hoạt cơ chế cảnh báo đa kênh (Push lần 2, SMS/ZNS) và áp dụng nguyên tắc an toàn y khoa "Default to Unsafe" (tiềm ẩn nguy cơ biến chứng mất kiểm soát), đẩy cờ cảnh báo quá hạn 🟠 lên Dashboard Bác sĩ và mở nút gọi điện can thiệp trực tiếp. |

---

### 1.9 UC-09: Xem hướng dẫn xử lý khẩn cấp khi gặp Red Flag

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-09 | | |
| **Tên Use Case (Use Case Name)** | Xem hướng dẫn xử lý khẩn cấp khi gặp Red Flag (View Emergency Action for Red Flag) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Tác nhân chính: Caregiver<br>Tác nhân phụ: Khoa Cấp cứu Bệnh viện / Bác sĩ | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả cách hệ thống lập tức hiển thị quy trình xử lý khẩn cấp, số điện thoại hotline cấp cứu của bệnh viện và hướng dẫn sơ cứu tức thì khi phát hiện dấu hiệu Red Flag từ Recovery Check hoặc khi Caregiver chủ động bấm yêu cầu hỗ trợ. | | |
| **Mục tiêu (Goal)** | Cung cấp tức thời các chỉ dẫn hành động khẩn cấp và kênh kết nối trực tiếp với bệnh viện khi xuất hiện triệu chứng nguy hiểm | | |
| **Tác nhân kích hoạt (Trigger)** | Recovery Check kích hoạt dấu hiệu Red Flag (UC-08 Luồng A2) HOẶC Caregiver bấm nút “Báo cáo dấu hiệu bất thường / Khẩn cấp” trên màn hình chính. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Kế hoạch chăm sóc của bệnh nhân đã được cấu hình số điện thoại khẩn cấp và quy trình xử lý của bệnh viện. | | |
| **Điều kiện sau (Post-conditions)** | 1. Hướng dẫn khẩn cấp và nút gọi trực tiếp được hiển thị cho Caregiver.<br>2. Sự kiện khẩn cấp được ghi lại trong nhật ký sự kiện y tế của bệnh nhân kèm thời gian. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Hệ thống phát hiện sự kiện Red Flag | Hệ thống hiển thị ngay màn hình Xử lý Khẩn cấp với banner cảnh báo nổi bật màu đỏ chỉ rõ triệu chứng nguy hiểm cần chú ý |
| | 2 | Hệ thống hiển thị các hành động khẩn cấp ưu tiên: Nút gọi hotline cấp cứu bệnh viện / Bác sĩ điều trị, Địa chỉ bệnh viện kèm chỉ đường, Các việc cần làm ngay (không dụi mắt, đeo khiên bảo vệ, không tự nhỏ thêm thuốc), Kênh hỗ trợ VISI [TBD] | Caregiver đọc các chỉ dẫn khẩn cấp |
| | 3 | Caregiver nhấn nút “Gọi cấp cứu / Gọi bệnh viện” | Thiết bị khởi chạy ứng dụng gọi điện với số hotline đã được cấu hình; hệ thống ghi nhận thời điểm gọi và sự kiện Red Flag vào hồ sơ bệnh nhân |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Xác nhận đang di chuyển đến bệnh viện** | 1 | Caregiver đọc hướng dẫn và nhấn “Đã hiểu và đang di chuyển đến bệnh viện” | Hệ thống ghi nhận mốc thời gian xác nhận và duy trì một banner cảnh báo khẩn cấp màu đỏ trên đầu màn hình ứng dụng cho đến khi nhân viên y tế xử lý [ASSUMPTION] |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thiết bị không hỗ trợ gọi điện** | 1 | Caregiver sử dụng máy tính bảng hoặc máy tính không có chức năng gọi điện thoại trực tiếp | Hệ thống hiển thị số điện thoại cỡ lớn in đậm kèm nút “Sao chép số điện thoại” và các kênh liên hệ thay thế (nhắn tin/chat) |
| **Mức độ ưu tiên (Priority)** | Cao (High - Đặc biệt khẩn cấp) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR11 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR11 | Ưu tiên quy trình hành động khẩn cấp | Khi xuất hiện dấu hiệu Red Flag, hệ thống phải ưu tiên hiển thị thông tin liên hệ khẩn cấp và hướng dẫn xử lý tức thì của bệnh viện. Việc liên hệ y tế khẩn cấp được ưu tiên tuyệt đối so với các tính năng tự chăm sóc trên ứng dụng. |

---

## PHÂN HỆ 2: BÁC SĨ (DOCTOR)

> **Ghi chú quản trị:** Tài khoản Bác sĩ (Doctor Account) được Quản trị viên Bệnh viện (Admin) khởi tạo và phân quyền sẵn trong hệ thống. Bác sĩ sử dụng tài khoản được cấp để đăng nhập trực tiếp tại UC-11.

---

### 1.11 UC-11: Đăng nhập hệ thống phía Bác sĩ (Doctor Login System)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-11 | | |
| **Tên Use Case (Use Case Name)** | Đăng nhập hệ thống phía Bác sĩ (Doctor Login System) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Use case này mô tả cách Bác sĩ đăng nhập vào hệ thống bằng tài khoản do Quản trị viên cấp và mật khẩu để truy cập Bảng điều khiển Bác sĩ, quản lý hồ sơ bệnh nhân, cấu hình Care Plan Template và theo dõi tiến trình hồi phục của bệnh nhân. | | |
| **Mục tiêu (Goal)** | Xác thực danh tính Bác sĩ và cấp quyền truy cập các tính năng chuyên môn y tế | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhập thông tin và nhấn “Đăng nhập” trên Cổng thông tin Bác sĩ. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Bác sĩ đã được Quản trị viên (Admin) tạo sẵn tài khoản công vụ với vai trò DOCTOR.<br>2. Tài khoản đang hoạt động (không bị khóa hay vô hiệu hóa). | | |
| **Điều kiện sau (Post-conditions)** | 1. Bác sĩ được chứng thực và phiên làm việc bảo mật được thiết lập.<br>2. Bác sĩ được chuyển hướng đến Bảng điều khiển Bác sĩ (Doctor Dashboard). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ truy cập trang Đăng nhập Bác sĩ | Hệ thống hiển thị biểu mẫu đăng nhập (Email/SĐT/Tên đăng nhập và Mật khẩu) |
| | 2 | Bác sĩ nhập thông tin và nhấn “Đăng nhập” | Hệ thống kiểm tra định dạng, đối chiếu thông tin trong cơ sở dữ liệu, kiểm tra trạng thái hoạt động và xác nhận vai trò DOCTOR |
| | 3 | Hệ thống thiết lập phiên làm việc bảo mật | Hệ thống tạo token phiên đăng nhập và chuyển hướng Bác sĩ đến Bảng điều khiển Bác sĩ (hiển thị Danh sách bệnh nhân, Care Plan Templates và Cảnh báo Recovery Check) |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Bác sĩ Đăng xuất** | 1 | Bác sĩ nhấn “Đăng xuất” trên thanh điều hướng | Hệ thống hủy phiên làm việc hiện tại, vô hiệu hóa token và đưa người dùng về trang Đăng nhập |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Sai thông tin đăng nhập** | 1 | Bác sĩ nhập sai tên đăng nhập hoặc mật khẩu | Hệ thống ghi nhận số lần thử sai và hiển thị thông báo lỗi: “Thông tin đăng nhập không chính xác.” |
| **E2: Tài khoản bị khóa hoặc vô hiệu hóa** | 1 | Bác sĩ cố gắng đăng nhập vào tài khoản đã bị khóa | Hệ thống từ chối đăng nhập và hiển thị: “Tài khoản của bạn đã bị vô hiệu hóa hoặc tạm khóa. Vui lòng liên hệ Quản trị viên.” |
| **E3: Không đúng quyền hạn vai trò** | 1 | Người dùng không có vai trò DOCTOR (ví dụ Caregiver) cố đăng nhập qua Cổng Bác sĩ | Hệ thống chặn truy cập và thông báo: “Tài khoản không có quyền truy cập vào cổng thông tin Bác sĩ.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR14 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR14 | Kiểm soát quyền truy cập của Bác sĩ | Chỉ các tài khoản đã được xác thực có vai trò DOCTOR đang hoạt động mới được quyền truy cập hồ sơ bệnh án, mẫu kế hoạch chăm sóc và bảng theo dõi y khoa. |

---

### 1.12 NHÓM UC-12: QUẢN LÝ HỒ SƠ BỆNH NHÂN (CRUD PATIENT RECORDS)

---

#### 1.12.1 UC-12.1: Tạo mới hồ sơ bệnh nhân (Create Patient Record)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-12.1 | | |
| **Tên Use Case (Use Case Name)** | Tạo mới hồ sơ bệnh nhân (Create Patient Record) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ nhập thông tin định danh và thông tin điều trị ban đầu để tạo mới một hồ sơ bệnh nhân vào hệ thống. | | |
| **Mục tiêu (Goal)** | Tiếp nhận và khởi tạo hồ sơ bệnh nhân mới phục vụ việc thiết lập Kế hoạch chăm sóc | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Thêm bệnh nhân mới” trên trang danh sách bệnh nhân. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đã đăng nhập với vai trò DOCTOR. | | |
| **Điều kiện sau (Post-conditions)** | Hồ sơ bệnh nhân mới được lưu với mã Patient ID duy nhất; sẵn sàng tạo Care Plan. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn nút “Thêm bệnh nhân mới” | Hệ thống hiển thị biểu mẫu nhập thông tin bệnh nhân |
| | 2 | Bác sĩ nhập thông tin cơ bản (Họ tên, Ngày sinh/Tuổi, Giới tính, Số điện thoại) và chọn Loại phẫu thuật (Phaco, Lác, LASIK/ICL, Cắt dịch kính...) | Hệ thống tự động kích hoạt và hiển thị động các trường dữ liệu lâm sàng tùy chỉnh phù hợp theo loại ca phẫu thuật đã chọn (ví dụ: Mắt phẫu thuật, Công suất IOL, Độ lác trước mổ, Độ khúc xạ, Vết mổ...). Bác sĩ nhập các trường thông tin chuyên môn tùy biến này và ghi chú điều trị. Hệ thống kiểm tra tính hợp lệ dữ liệu và các trường bắt buộc |
| | 3 | Bác sĩ nhấn “Lưu hồ sơ” | Hệ thống cấp mã Patient ID tự động, lưu bản ghi vào CSDL, hiển thị: “Tạo hồ sơ bệnh nhân thành công.” và gợi ý chuyển sang tạo Care Plan |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Hủy tạo hồ sơ** | 1 | Bác sĩ nhấn “Hủy bỏ” khi đang nhập dữ liệu | Hệ thống xác nhận và đóng biểu mẫu, đưa về danh sách bệnh nhân |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thiếu trường bắt buộc** | 1 | Bác sĩ để trống các trường: Họ tên, Ngày sinh, Giới tính, Chẩn đoán, Loại phẫu thuật | Hệ thống chặn lưu và viền đỏ các trường thiếu: “Vui lòng nhập đầy đủ các trường bắt buộc (*)” |
| **E2: Ngày sinh không hợp lệ** | 1 | Bác sĩ nhập ngày sinh trong tương lai | Hệ thống báo lỗi: “Ngày sinh không thể lớn hơn ngày hiện tại.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR15 | | |

---

#### 1.12.2 UC-12.2: Xem danh sách và tìm kiếm hồ sơ bệnh nhân (View & Search Patient List)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-12.2 | | |
| **Tên Use Case (Use Case Name)** | Xem danh sách và tìm kiếm hồ sơ bệnh nhân (View & Search Patient List) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem danh sách các bệnh nhân đang quản lý, tìm kiếm theo tên/mã và lọc theo loại phẫu thuật hoặc trạng thái. | | |
| **Mục tiêu (Goal)** | Cung cấp cái nhìn tổng quan và tra cứu nhanh hồ sơ bệnh nhân của Bác sĩ | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ chọn mục “Danh sách bệnh nhân” trên menu điều hướng. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đã đăng nhập với vai trò DOCTOR. | | |
| **Điều kiện sau (Post-conditions)** | Danh sách bệnh nhân được kết xuất kèm thông tin tóm tắt và trạng thái Care Plan. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn mục “Danh sách bệnh nhân” | Hệ thống truy xuất và hiển thị bảng danh sách bệnh nhân: Mã BN, Họ tên, Tuổi, Loại phẫu thuật, Ngày mổ, Trạng thái Care Plan, Tên Caregiver đã liên kết |
| | 2 | Bác sĩ nhập từ khóa (Tên/Mã BN) vào ô tìm kiếm hoặc chọn lọc theo Loại phẫu thuật | Hệ thống lọc danh sách theo thời gian thực và hiển thị các bản ghi khớp với điều kiện lọc |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Phân trang danh sách** | 1 | Bác sĩ chuyển sang trang tiếp theo khi danh sách vượt quá 20 bệnh nhân | Hệ thống hiển thị nhóm 20 bệnh nhân tiếp theo |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Không tìm thấy kết quả** | 1 | Từ khóa tìm kiếm không khớp với bất kỳ bệnh nhân nào | Hệ thống hiển thị thông báo: “Không tìm thấy hồ sơ bệnh nhân phù hợp.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR14 | | |

---

#### 1.12.3 UC-12.3: Xem chi tiết hồ sơ bệnh nhân (View Patient Record Detail)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-12.3 | | |
| **Tên Use Case (Use Case Name)** | Xem chi tiết hồ sơ bệnh nhân (View Patient Record Detail) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem toàn bộ thông tin chi tiết của một bệnh nhân, bao gồm lý lịch y tế, Kế hoạch chăm sóc hiện tại, danh sách người chăm sóc đã liên kết và lịch sử phục hồi. | | |
| **Mục tiêu (Goal)** | Xem xét toàn diện tình trạng lâm sàng và tiến trình điều trị của bệnh nhân | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn vào một dòng bệnh nhân trong Danh sách bệnh nhân. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bệnh nhân được chọn có tồn tại trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Toàn bộ dữ liệu chi tiết hồ sơ bệnh nhân được hiển thị. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn vào tên bệnh nhân trong danh sách | Hệ thống mở trang Chi tiết hồ sơ bệnh nhân với các phần: Thông tin cá nhân, Chi tiết phẫu thuật, Thông tin Kế hoạch chăm sóc, Thông tin Caregiver liên kết, và Lịch sử Recovery Check |
| | 2 | Bác sĩ chuyển giữa các tab để xem dữ liệu chi tiết tương ứng | Hệ thống hiển thị đầy đủ thông tin của từng tab chức năng |
| **Luồng thay thế (Alternative Flow)** | Không áp dụng | Không có luồng thay thế | |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Hồ sơ đã bị xóa/vô hiệu hóa** | 1 | Bác sĩ truy cập bằng liên kết cũ của hồ sơ đã vô hiệu hóa | Hệ thống thông báo: “Hồ sơ bệnh nhân không khả dụng hoặc đã bị lưu trữ.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR14 | | |

---

#### 1.12.4 UC-12.4: Chỉnh sửa hồ sơ bệnh nhân (Edit Patient Record)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-12.4 | | |
| **Tên Use Case (Use Case Name)** | Chỉnh sửa hồ sơ bệnh nhân (Edit Patient Record) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ cập nhật thông tin cá nhân, số điện thoại liên hệ, thông tin chẩn đoán hoặc ghi chú y tế của bệnh nhân. | | |
| **Mục tiêu (Goal)** | Cập nhật chính xác các thông tin thay đổi của bệnh nhân trong quá trình theo dõi | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Chỉnh sửa hồ sơ” trên trang chi tiết bệnh nhân. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ có quyền chỉnh sửa hồ sơ của bệnh nhân này. | | |
| **Điều kiện sau (Post-conditions)** | Dữ liệu cập nhật được lưu vào hệ thống kèm lịch sử chỉnh sửa (audit log). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Chỉnh sửa hồ sơ” | Hệ thống hiển thị biểu mẫu chỉnh sửa với các trường dữ liệu hiện tại của bệnh nhân |
| | 2 | Bác sĩ thay đổi các thông tin cần thiết (Số điện thoại, địa chỉ, ghi chú y tế, mắt điều trị...) | Hệ thống kiểm tra tính hợp lệ dữ liệu nhập |
| | 3 | Bác sĩ nhấn “Cập nhật” | Hệ thống lưu thay đổi, ghi nhận người sửa và mốc thời gian, hiển thị thông báo: “Cập nhật hồ sơ bệnh nhân thành công.” |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Hủy bỏ thay đổi** | 1 | Bác sĩ nhấn “Hủy bỏ” khi chưa lưu | Hệ thống hủy các thay đổi và giữ nguyên dữ liệu cũ |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Xóa trắng trường bắt buộc** | 1 | Bác sĩ xóa trắng trường Họ tên hoặc Ngày sinh và nhấn Cập nhật | Hệ thống chặn lưu và hiển thị thông báo: “Không thể để trống các thông tin bắt buộc.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR15 | | |

---

#### 1.12.5 UC-12.5: Xóa / Vô hiệu hóa hồ sơ bệnh nhân (Delete / Deactivate Patient Record)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-12.5 | | |
| **Tên Use Case (Use Case Name)** | Xóa / Vô hiệu hóa hồ sơ bệnh nhân (Delete / Deactivate Patient Record) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor), Quản trị viên | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xóa (nếu tạo nhầm và chưa phát sinh Care Plan) hoặc vô hiệu hóa/lưu trữ hồ sơ bệnh nhân khi đã kết thúc đợt điều trị. | | |
| **Mục tiêu (Goal)** | Quản lý vòng đời hồ sơ bệnh nhân, đảm bảo dữ liệu sạch và an toàn thông tin y tế | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ chọn “Xóa hồ sơ” hoặc “Lưu trữ hồ sơ” trên màn hình quản lý. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ có thẩm quyền đối với hồ sơ bệnh nhân này. | | |
| **Điều kiện sau (Post-conditions)** | Hồ sơ được chuyển trạng thái Lưu trữ (Archived) hoặc xóa mềm khỏi danh sách hiển thị hoạt động. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn chức năng “Lưu trữ / Vô hiệu hóa hồ sơ” | Hệ thống hiển thị hộp thoại xác nhận cảnh báo: “Hồ sơ này sẽ được lưu trữ và ngừng nhận các cập nhật mới. Bạn có chắc chắn muốn tiếp tục?” |
| | 2 | Bác sĩ chọn lý do (Kết thúc điều trị / Chuyển viện / Tạo nhầm) và nhấn “Xác nhận” | Hệ thống cập nhật trạng thái hồ sơ sang `Archived`, vô hiệu hóa mã QR liên kết, hiển thị thông báo: “Đã lưu trữ hồ sơ bệnh nhân thành công.” |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Hồ sơ đang có Care Plan hoạt động** | 1 | Bác sĩ cố xóa hẳn hồ sơ đang có Care Plan Active và có Caregiver đang theo dõi | Hệ thống chặn thao tác xóa cứng và thông báo: “Không thể xóa hồ sơ đang có Kế hoạch chăm sóc hoạt động. Vui lòng kết thúc Kế hoạch chăm sóc trước khi lưu trữ.” |
| **Mức độ ưu tiên (Priority)** | Trung bình (Medium) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR25 | | |

#### Quy tắc nghiệp vụ (Business Rules - Áp dụng cho nhóm UC-12)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR15 | Thông tin bệnh nhân bắt buộc | Hồ sơ bệnh nhân bắt buộc phải có đầy đủ Họ và tên, Ngày sinh hoặc Tuổi, Giới tính, Chẩn đoán bệnh và Loại phẫu thuật trước khi được lưu vào cơ sở dữ liệu. |
| BR25 | Ràng buộc xóa và lưu trữ hồ sơ bệnh nhân | Hệ thống áp dụng cơ chế xóa mềm (Soft Delete / Archive) đối với hồ sơ bệnh nhân đã phát sinh dữ liệu y tế; chỉ cho phép xóa cứng nếu hồ sơ vừa tạo nhầm và chưa phát sinh Care Plan hay liên kết QR nào. |

---

### 1.13 NHÓM UC-13: QUẢN LÝ CARE PLAN TEMPLATE TỔNG THỂ (MASTER TEMPLATE CRUD)

---

#### 1.13.1 UC-13.1: Tạo mới Care Plan Template (Create Care Plan Template)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-13.1 | | |
| **Tên Use Case (Use Case Name)** | Tạo mới Care Plan Template (Create Care Plan Template) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor), Quản trị viên chuyên môn | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ tạo mới một Care Plan Template master gắn với loại phẫu thuật cụ thể (Phaco, Lác, LASIK...), nhập thông tin chung, mở không gian làm việc để cấu hình lần lượt 5 thành phần con (Learning Path, Medication, Recovery Check, Red Flag, Do & Don't) và chỉ lưu toàn bộ template khi đã hoàn tất cấu hình. | | |
| **Mục tiêu (Goal)** | Khởi tạo gói mẫu chăm sóc chuẩn hóa hoàn chỉnh để tái sử dụng cho nhiều bệnh nhân | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn “Tạo Template mới” trong trang Quản lý Template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ có vai trò DOCTOR được cấp quyền quản lý chuyên môn. | | |
| **Điều kiện sau (Post-conditions)** | Bản ghi Care Plan Template mới cùng cấu hình 5 thành phần con được lưu vào hệ thống ở trạng thái Bản nháp (Draft). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Tạo Template mới” | Hệ thống hiển thị giao diện khởi tạo template gồm khu vực nhập Thông tin chung và Không gian làm việc với 5 tab thành phần con (Learning Path, Medication, Recovery Check, Red Flag, Do & Don't) |
| | 2 | Bác sĩ nhập Thông tin chung: Tên Template, Chọn Loại phẫu thuật áp dụng (Phaco, Lác, LASIK...), Nhập Mô tả mục tiêu lâm sàng | Hệ thống kiểm tra tính duy nhất của tên template theo loại phẫu thuật |
| | 3 | Bác sĩ chuyển qua lại giữa 5 tab chức năng để cấu hình chi tiết các thành phần con:<br>- **Tab Learning Path**: Thiết lập lộ trình bài học và câu hỏi Mini Quiz.<br>- **Tab Medication**: Thiết lập danh mục đơn thuốc mẫu.<br>- **Tab Recovery Check**: Thiết lập các mốc và câu hỏi kiểm tra phục hồi.<br>- **Tab Red Flag**: Thiết lập các dấu hiệu cảnh báo khẩn cấp.<br>- **Tab Do & Don't**: Thiết lập danh mục Nên làm & Cần tránh | Hệ thống kiểm tra và hiển thị trực quan dữ liệu cấu hình đã nhập trên từng tab |
| | 4 | Sau khi hoàn tất thiết lập thông tin chung và 5 thành phần con, Bác sĩ nhấn nút “Lưu Template” | Hệ thống kiểm tra tính toàn vẹn của dữ liệu, lưu gói Care Plan Template vào CSDL ở trạng thái `Draft` và hiển thị thông báo: “Tạo mới Care Plan Template thành công.” |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Trùng tên Template cho loại phẫu thuật** | 1 | Bác sĩ đặt tên trùng với template đã có của cùng loại phẫu thuật | Hệ thống báo lỗi: “Tên Template đã tồn tại cho loại phẫu thuật này.” |
| **E2: Rời khỏi trang khi chưa bấm lưu** | 1 | Bác sĩ đóng trình duyệt hoặc điều hướng sang trang khác khi chưa nhấn “Lưu Template” | Hệ thống hiển thị hộp thoại cảnh báo: “Dữ liệu cấu hình chưa được lưu. Bạn có chắc chắn muốn rời khỏi không?” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR16 | | |

---

#### 1.13.2 UC-13.2: Xem danh sách Care Plan Template (View Care Plan Template List)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-13.2 | | |
| **Tên Use Case (Use Case Name)** | Xem danh sách Care Plan Template (View Care Plan Template List) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem danh mục các Care Plan Template trong hệ thống kèm trạng thái (Draft / Active / Inactive) và số bệnh nhân đang áp dụng. | | |
| **Mục tiêu (Goal)** | Cung cấp danh sách các mẫu quy trình chăm sóc hiện có trong bệnh viện | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ chọn mục “Quản lý Care Plan Template” trên thanh menu. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đã đăng nhập vào hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Toàn bộ danh sách template được tải và hiển thị. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ vào mục “Quản lý Care Plan Template” | Hệ thống hiển thị bảng danh sách gồm: Tên Template, Loại phẫu thuật, Trạng thái (Hoạt động / Bản nháp / Vô hiệu hóa), Ngày cập nhật, Bác sĩ tạo |
| | 2 | Bác sĩ lọc theo Loại phẫu thuật (Phaco, Lác, LASIK...) hoặc trạng thái | Hệ thống lọc danh sách theo bộ lọc được chọn |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR14 | | |

---

#### 1.13.3 UC-13.3: Xem chi tiết Care Plan Template (View Care Plan Template Detail)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-13.3 | | |
| **Tên Use Case (Use Case Name)** | Xem chi tiết Care Plan Template (View Care Plan Template Detail) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem chi tiết toàn bộ các thành phần của một Template (Learning Path, Đơn thuốc mẫu, Bộ câu hỏi Recovery Check, Dấu hiệu Red Flag, Danh mục Do & Don't). | | |
| **Mục tiêu (Goal)** | Kiểm tra toàn diện nội dung chuyên môn của một gói template trước khi quyết định kích hoạt hoặc áp dụng | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn vào một template trong danh sách. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template tồn tại trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Toàn bộ nội dung của các cấu phần được kết xuất chi tiết. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn vào một template trong danh sách | Hệ thống mở màn hình chi tiết template với 5 tab thành phần con |
| | 2 | Bác sĩ xem xét nội dung chi tiết qua các tab | Hệ thống hiển thị cấu hình cụ thể của từng tab (danh mục bài học, danh mục thuốc, lịch recovery check, red flag, do/don't) |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR14 | | |

---

#### 1.13.4 UC-13.4: Chỉnh sửa Care Plan Template (Edit Care Plan Template)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-13.4 | | |
| **Tên Use Case (Use Case Name)** | Chỉnh sửa Care Plan Template (Edit Care Plan Template) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ chỉnh sửa thông tin chung (tên, mô tả) VÀ trực tiếp điều chỉnh nội dung trên 5 tab thành phần con (Learning Path, Medication, Recovery Check, Red Flag, Do & Don't) của Care Plan Template, sau khi hoàn tất chỉnh sửa toàn bộ mới nhấn lưu thay đổi. | | |
| **Mục tiêu (Goal)** | Cho phép Bác sĩ cập nhật đồng bộ thông tin chung và cấu hình chuyên môn trên 5 tab thành phần con của Care Plan Template | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Chỉnh sửa Template” trên giao diện chi tiết template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang ở trạng thái cho phép chỉnh sửa (Draft hoặc Active). | | |
| **Điều kiện sau (Post-conditions)** | Toàn bộ các thay đổi ở thông tin chung và 5 tab thành phần con được lưu vào hệ thống. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Chỉnh sửa Template” | Hệ thống mở giao diện chỉnh sửa toàn diện template với Thông tin chung và 5 tab thành phần con tải sẵn dữ liệu hiện tại |
| | 2 | Bác sĩ điều chỉnh Thông tin chung (Tên template, Mô tả) nếu cần | Hệ thống kiểm tra tính hợp lệ dữ liệu |
| | 3 | Bác sĩ chuyển qua lại giữa 5 tab thành phần con (Learning Path, Medication, Recovery Check, Red Flag, Do & Don't) để thêm, sửa, xóa hoặc sắp xếp lại các nội dung chuyên môn tương ứng | Hệ thống hiển thị trực quan các thay đổi trên từng tab tương ứng |
| | 4 | Sau khi hoàn tất việc chỉnh sửa toàn bộ các cấu phần, Bác sĩ nhấn “Lưu thay đổi” | Hệ thống kiểm tra tính toàn vẹn của dữ liệu, lưu toàn bộ cập nhật vào CSDL, ghi nhật ký kiểm toán và hiển thị thông báo: “Cập nhật Care Plan Template thành công.” |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Hủy bỏ chỉnh sửa** | 1 | Bác sĩ nhấn nút “Hủy bỏ” khi chưa lưu | Hệ thống hiển thị hộp thoại xác nhận hủy, khôi phục lại dữ liệu ban đầu và đóng giao diện chỉnh sửa |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thiếu thông tin bắt buộc tại tab con** | 1 | Bác sĩ để trống trường bắt buộc tại một trong 5 tab con khi nhấn lưu | Hệ thống chặn lưu, đánh dấu đỏ tab bị lỗi và hiển thị: “Vui lòng kiểm tra và nhập đủ thông tin bắt buộc tại tab [Tên tab].” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR16, BR17 | | |

---

#### 1.13.5 UC-13.5: Kích hoạt / Vô hiệu hóa Care Plan Template (Activate / Deactivate Template)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-13.5 | | |
| **Tên Use Case (Use Case Name)** | Kích hoạt / Vô hiệu hóa Care Plan Template (Activate / Deactivate Template) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor), Quản trị viên | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ kích hoạt (Active) template để đưa vào áp dụng cho bệnh nhân, hoặc vô hiệu hóa (Inactive) khi phác đồ không còn sử dụng. | | |
| **Mục tiêu (Goal)** | Kiểm soát vòng đời và tính khả dụng của các mẫu chăm sóc trong bệnh viện | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Kích hoạt” hoặc “Vô hiệu hóa” trên giao diện template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Để kích hoạt, template phải được cấu hình đầy đủ cả 5 thành phần con. | | |
| **Điều kiện sau (Post-conditions)** | Trạng thái template chuyển sang Active hoặc Inactive tương ứng. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Kích hoạt Template” | Hệ thống kiểm tra điều kiện tiên quyết (xác thực có đủ thuốc, recovery check, red flag...) |
| | 2 | Bác sĩ xác nhận kích hoạt | Hệ thống cập nhật trạng thái sang `Active`, hiển thị thông báo: “Template đã được kích hoạt và sẵn sàng áp dụng cho bệnh nhân.” |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Vô hiệu hóa Template** | 1 | Bác sĩ nhấn “Vô hiệu hóa” trên template Active | Hệ thống cảnh báo: “Template này sẽ không thể chọn cho bệnh nhân mới. Các bệnh nhân đang dùng không bị ảnh hưởng. Tiếp tục?” |
| | 2 | Bác sĩ xác nhận | Hệ thống đổi trạng thái sang `Inactive` |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thiếu cấu hình thành phần khi kích hoạt** | 1 | Template thiếu danh mục thuốc hoặc câu hỏi Recovery Check | Hệ thống chặn kích hoạt và cảnh báo: “Template phải có đầy đủ cấu hình Recovery Check và Thuốc trước khi kích hoạt.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR16, BR17 | | |

#### Quy tắc nghiệp vụ (Business Rules - Áp dụng cho nhóm UC-13)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR16 | Ràng buộc loại phẫu thuật của Template | Mỗi Care Plan Template phải được liên kết rõ ràng với duy nhất một loại phẫu thuật chuyên khoa (ví dụ: Phaco, Lác, LASIK/ICL, Võng mạc). |
| BR17 | Tính độc lập và nguyên khối của Template | Care Plan Template là một gói cấu hình chuẩn gồm Caregiver 101, Learning Path, Thuốc, Recovery Check và Red Flag. Việc chỉnh sửa master template không làm thay đổi các Care Plan của bệnh nhân đã được tạo trước đó. |

---

### 1.14 NHÓM UC-14: QUẢN LÝ CẤU HÌNH LEARNING PATH TEMPLATE (US-20 CRUD)

---

#### 1.14.1 UC-14.1: Thêm bài học và câu hỏi trắc nghiệm kiểm tra (Create Learning Module & Mini Quiz)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-14.1 | | |
| **Tên Use Case (Use Case Name)** | Thêm bài học và câu hỏi trắc nghiệm kiểm tra (Create Learning Module & Mini Quiz) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ thêm một bài học mới vào Learning Path của Template (gồm tiêu đề, phân loại, văn bản mô tả, tệp video/ảnh) VÀ tạo bộ 3 câu hỏi trắc nghiệm (Mini Quiz) kiểm tra kiến thức phù hợp cho loại bệnh, tự nhập câu hỏi, các phương án lựa chọn, chỉ định đáp án đúng kèm lời giải thích y khoa. | | |
| **Mục tiêu (Goal)** | Xây dựng bài học chuẩn y khoa và đính kèm bộ câu hỏi trắc nghiệm phù hợp với bệnh lý để kiểm tra kiến thức của người chăm sóc ở cuối bài | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Thêm bài học mới” trong tab Learning Path của Template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đang trong giao diện chỉnh sửa Care Plan Template. | | |
| **Điều kiện sau (Post-conditions)** | Bài học mới cùng bộ câu hỏi trắc nghiệm Mini Quiz được lưu vào danh mục Learning Path của Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Thêm bài học mới” | Hệ thống hiển thị biểu mẫu tạo bài học gồm phần thông tin bài học và phần cấu hình bộ câu hỏi trắc nghiệm Mini Quiz |
| | 2 | Bác sĩ nhập thông tin bài học: Tiêu đề bài học, Phân loại chủ đề, Nội dung văn bản hướng dẫn; Đính kèm video/hình ảnh/infographic | Hệ thống tải lên và kiểm tra dung lượng/định dạng tệp đính kèm |
| | 3 | Bác sĩ thiết lập câu hỏi kiểm tra: Nhập nội dung 3 câu hỏi trắc nghiệm phù hợp cho loại bệnh, nhập các phương án trả lời (A, B, C, D), tích chọn đáp án đúng và nhập lời giải thích y khoa cho từng câu | Hệ thống kiểm tra tính đầy đủ của bộ câu hỏi trắc nghiệm (phải có đủ câu hỏi, các lựa chọn và chỉ định ít nhất 1 đáp án đúng) |
| | 4 | Bác sĩ nhấn “Lưu bài học” | Hệ thống ghi nhận bài học kèm bộ 3 câu hỏi trắc nghiệm Mini Quiz vào danh mục Learning Path của Template và cấp số thứ tự hiển thị |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Tệp đính kèm sai định dạng** | 1 | Bác sĩ tải lên tệp không đúng chuẩn (không phải mp4, png, jpg, pdf) | Hệ thống báo lỗi định dạng và từ chối tải tệp |
| **E2: Chưa hoàn thiện câu hỏi trắc nghiệm** | 1 | Bác sĩ nhập câu hỏi nhưng chưa nhập đủ các phương án hoặc chưa tích chọn đáp án đúng | Hệ thống cảnh báo: “Vui lòng nhập đầy đủ nội dung câu hỏi, các phương án lựa chọn và tích chọn đáp án đúng cho từng câu hỏi Mini Quiz.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7 | | |

---

#### 1.14.2 UC-14.2: Xem danh sách bài học Learning Path (View Learning Modules)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-14.2 | | |
| **Tên Use Case (Use Case Name)** | Xem danh sách bài học Learning Path (View Learning Modules) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem toàn bộ danh mục bài học của Learning Path trong Template theo đúng thứ tự hiển thị, kiểm tra trạng thái bộ câu hỏi Mini Quiz và xem trước nội dung hiển thị. | | |
| **Mục tiêu (Goal)** | Kiểm tra kết cấu giáo trình chăm sóc và bộ câu hỏi trắc nghiệm của gói template phẫu thuật | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ mở tab “Learning Path” trong template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang mở trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Danh sách các bài học, hình thức media, trạng thái câu hỏi trắc nghiệm và thứ tự hiển thị được kết xuất. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ mở tab “Learning Path” | Hệ thống hiển thị danh sách bài học theo dạng thẻ hoặc bảng gồm: Thứ tự, Tiêu đề bài học, Loại media (Video/Hình ảnh/Text), Số lượng câu hỏi trắc nghiệm Mini Quiz (3 câu), và nút Xem trước |
| | 2 | Bác sĩ nhấn nút “Xem trước” tại một bài học | Hệ thống hiển thị giao diện xem trước bài học bao gồm các video/hình ảnh minh họa và bộ câu hỏi trắc nghiệm như hiển thị cho người chăm sóc |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7 | | |

---

#### 1.14.3 UC-14.3: Chỉnh sửa nội dung bài học và câu hỏi trắc nghiệm (Edit Learning Module & Mini Quiz)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-14.3 | | |
| **Tên Use Case (Use Case Name)** | Chỉnh sửa nội dung bài học và câu hỏi trắc nghiệm (Edit Learning Module & Mini Quiz) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ cập nhật tiêu đề, nội dung hướng dẫn, thay đổi video/hình ảnh minh họa HOẶC chỉnh sửa nội dung/đáp án của 3 câu hỏi trắc nghiệm Mini Quiz của bài học. | | |
| **Mục tiêu (Goal)** | Đảm bảo kiến thức đào tạo và câu hỏi kiểm tra luôn chuẩn xác theo hướng dẫn y khoa cập nhật | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Sửa” tại một bài học trong tab Learning Path. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bài học tồn tại trong Learning Path của template. | | |
| **Điều kiện sau (Post-conditions)** | Nội dung cập nhật của bài học và bộ câu hỏi trắc nghiệm được lưu vào hệ thống. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Sửa bài học” | Hệ thống mở biểu mẫu chỉnh sửa bài học với nội dung bài học và bộ câu hỏi trắc nghiệm hiện có |
| | 2 | Bác sĩ cập nhật nội dung văn bản, tệp video/ảnh HOẶC sửa đổi nội dung câu hỏi trắc nghiệm, các phương án lựa chọn, đáp án đúng và lời giải thích y khoa | Hệ thống kiểm tra tính hợp lệ dữ liệu |
| | 3 | Bác sĩ nhấn “Lưu thay đổi” | Hệ thống ghi nhận nội dung và câu hỏi cập nhật vào Template và thông báo thành công |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7 | | |

---

#### 1.14.4 UC-14.4: Xóa nội dung bài học khỏi Learning Path (Delete Learning Module)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-14.4 | | |
| **Tên Use Case (Use Case Name)** | Xóa nội dung bài học khỏi Learning Path (Delete Learning Module) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xóa bỏ một bài học không còn phù hợp khỏi danh mục Learning Path của Template (bao gồm cả bộ câu hỏi trắc nghiệm đi kèm). | | |
| **Mục tiêu (Goal)** | Tinh gọn và loại bỏ các nội dung hướng dẫn thừa hoặc lỗi thời | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn biểu tượng “Xóa” tại một bài học. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bài học có trong danh sách Learning Path. | | |
| **Điều kiện sau (Post-conditions)** | Bài học và toàn bộ câu hỏi trắc nghiệm đi kèm bị loại bỏ khỏi cấu hình Learning Path của Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Xóa” tại bài học | Hệ thống hiển thị thông báo xác nhận: “Bạn có chắc chắn muốn xóa bài học này khỏi Learning Path? Thao tác này sẽ xóa đồng thời toàn bộ câu hỏi trắc nghiệm Mini Quiz gắn liền với bài học.” |
| | 2 | Bác sĩ nhấn “Xác nhận xóa” | Hệ thống xóa bài học cùng bộ câu hỏi trắc nghiệm, tự động cập nhật lại thứ tự hiển thị của các bài học còn lại và thông báo thành công |
| **Mức độ ưu tiên (Priority)** | Trung bình (Medium) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7 | | |

---

#### 1.14.5 UC-14.5: Thay đổi thứ tự bài học trong Learning Path (Reorder Learning Modules)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-14.5 | | |
| **Tên Use Case (Use Case Name)** | Thay đổi thứ tự bài học trong Learning Path (Reorder Learning Modules) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ kéo thả hoặc sắp xếp lại trình tự xuất hiện của các bài học trong lộ trình học tập của người chăm sóc. | | |
| **Mục tiêu (Goal)** | Sắp xếp lộ trình học tập theo trình tự diễn tiến hồi phục hợp lý (ví dụ: 24h đầu -> Chăm sóc tuần 1 -> Dinh dưỡng...) | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ thao tác kéo thả hoặc nhấn nút mũi tên lên/xuống trên danh sách bài học. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Learning Path có từ 2 bài học trở lên. | | |
| **Điều kiện sau (Post-conditions)** | Thứ tự bài học mới được lưu trữ và áp dụng ngay lập tức (bài học và câu hỏi trắc nghiệm đi kèm di chuyển đồng bộ). | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ kéo thả một bài học đến vị trí mới trong danh sách | Hệ thống cập nhật vị trí trực quan trên giao diện (bộ câu hỏi trắc nghiệm tương ứng đi liền với bài học) |
| | 2 | Bác sĩ nhấn “Lưu thứ tự” | Hệ thống cập nhật chỉ số thứ tự (index) của toàn bộ danh mục bài học và lưu vào CSDL |
| **Mức độ ưu tiên (Priority)** | Trung bình (Medium) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7 | | |

---

### 1.15 NHÓM UC-15: QUẢN LÝ CẤU HÌNH MEDICATION TEMPLATE (US-21 CRUD)

---

#### 1.15.1 UC-15.1: Thêm thuốc vào Medication Template (Create Medication Item)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-15.1 | | |
| **Tên Use Case (Use Case Name)** | Thêm thuốc vào Medication Template (Create Medication Item) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ thêm một loại thuốc chuẩn vào mẫu đơn thuốc của loại phẫu thuật, thiết lập liều dùng, số lần, thời điểm, cách dùng mặc định, mô tả nhận diện trực quan và các lưu ý lâm sàng đặc thù của thuốc. Bác sĩ có thể lưu và tiếp tục thêm nhiều loại thuốc liên tiếp vào template. | | |
| **Mục tiêu (Goal)** | Xây dựng danh mục thuốc chuẩn hóa đầy đủ nhận diện và lưu ý an toàn cho từng loại phẫu thuật để áp dụng nhanh khi kê đơn | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn “Thêm thuốc” trong tab Medication Template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đang cấu hình Care Plan Template. | | |
| **Điều kiện sau (Post-conditions)** | Loại thuốc mới được bổ sung vào danh mục thuốc của Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Thêm thuốc” | Hệ thống hiển thị biểu mẫu cấu hình thuốc |
| | 2 | Bác sĩ chọn/nhập các trường thông tin chi tiết:<br>- **Loại phẫu thuật**: Chọn loại phẫu thuật áp dụng (Phaco, Lác, LASIK/ICL, Cắt dịch kính...)<br>- **Tên thuốc**: Tên biệt dược/hoạt chất, hàm lượng<br>- **Dạng bào chế**: Thuốc nhỏ mắt / Thuốc uống<br>- **Liều lượng**: Số giọt hoặc số viên; **Tần suất**: Số lần/ngày<br>- **Thời điểm dùng trong ngày**: Sáng, Trưa, Chiều, Tối<br>- **Quan hệ bữa ăn**: Trước/Sau ăn; **Số ngày sử dụng**; **Thứ tự dùng**<br>- **Mô tả nhận diện về thuốc**: Mô tả màu sắc, hình dáng bao bì, màu nắp lọ/vỏ hộp (ví dụ: Lọ nắp trắng dung dịch đục, viên nang màu vàng-đỏ...) để người chăm sóc dễ phân biệt trực quan<br>- **Lưu ý về loại thuốc**: Hướng dẫn thao tác đặc thù (ví dụ: Lắc kỹ trước khi nhỏ, bảo quản ngăn mát tủ lạnh, cách xa cữ thuốc khác tối thiểu 5 phút...) | Hệ thống kiểm tra tính đầy đủ và hợp lệ của thông tin thuốc |
| | 3 | Bác sĩ nhấn “Lưu thuốc” | Hệ thống lưu thuốc vào danh mục Medication Template, cập nhật bảng hiển thị và đóng biểu mẫu thêm thuốc |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Thêm liên tiếp nhiều loại thuốc vào Medication Template** | 1 | Sau khi nhập đầy đủ thông tin một loại thuốc tại Bước 2, Bác sĩ nhấn nút “Lưu và tiếp tục thêm” thay vì đóng form | Hệ thống lưu loại thuốc vừa nhập vào danh mục Medication Template, tự động tăng chỉ số thứ tự dùng, làm mới biểu mẫu để Bác sĩ tiếp tục nhập loại thuốc tiếp theo vào template mà không phải thoát ra ngoài |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thiếu thông tin liều hoặc tên thuốc** | 1 | Bác sĩ để trống Tên thuốc hoặc Liều lượng | Hệ thống cảnh báo: “Tên thuốc và Liều lượng không được để trống.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7, BR23 | | |

---

#### 1.15.2 UC-15.2: Xem danh sách thuốc trong Medication Template (View Medication List)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-15.2 | | |
| **Tên Use Case (Use Case Name)** | Xem danh sách thuốc trong Medication Template (View Medication List) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem toàn bộ danh mục thuốc mẫu được kê cho loại phẫu thuật này, bao gồm cả thuốc uống và thuốc nhỏ mắt kèm thứ tự sử dụng. | | |
| **Mục tiêu (Goal)** | Kiểm tra tính chính xác của phác đồ thuốc mẫu | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ chọn tab “Medication Template”. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang mở trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Bảng danh sách thuốc và thông số chi tiết được hiển thị. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ chọn tab “Medication Template” | Hệ thống hiển thị bảng danh mục thuốc gồm: STT dùng, Tên thuốc, Dạng thuốc, Liều dùng, Tần suất, Số ngày dùng, Mô tả nhận diện thuốc, Lưu ý sử dụng, Ghi chú cách dùng |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7 | | |

---

#### 1.15.3 UC-15.3: Chỉnh sửa thông tin thuốc trong Medication Template (Edit Medication Item)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-15.3 | | |
| **Tên Use Case (Use Case Name)** | Chỉnh sửa thông tin thuốc trong Medication Template (Edit Medication Item) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ điều chỉnh liều lượng, số lần dùng, thời điểm uống, mô tả nhận diện trực quan hoặc lưu ý đặc thù của một loại thuốc mẫu. | | |
| **Mục tiêu (Goal)** | Tinh chỉnh phác đồ thuốc mẫu khi có thay đổi trong quy chuẩn y khoa | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Sửa” tại dòng thuốc trong danh sách. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Thuốc tồn tại trong danh mục Medication Template. | | |
| **Điều kiện sau (Post-conditions)** | Dữ liệu thuốc mẫu được cập nhật mới. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Sửa” tại một loại thuốc | Hệ thống mở biểu mẫu chỉnh sửa thông số thuốc với đầy đủ dữ liệu hiện có |
| | 2 | Bác sĩ điều chỉnh: Liều dùng, Tần suất, Thời gian dùng, Thứ tự dùng, Mô tả nhận diện thuốc hoặc Lưu ý về loại thuốc | Hệ thống kiểm tra tính hợp lệ dữ liệu |
| | 3 | Bác sĩ nhấn “Cập nhật” | Hệ thống lưu thông số mới và hiển thị thông báo thành công |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7 | | |

---

#### 1.15.4 UC-15.4: Xóa thuốc khỏi Medication Template (Delete Medication Item)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-15.4 | | |
| **Tên Use Case (Use Case Name)** | Xóa thuốc khỏi Medication Template (Delete Medication Item) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ loại bỏ một loại thuốc khỏi mẫu đơn thuốc của loại phẫu thuật. | | |
| **Mục tiêu (Goal)** | Loại bỏ các loại thuốc không còn được khuyến nghị trong phác đồ chuẩn | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn biểu tượng “Xóa” tại dòng thuốc. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Thuốc có trong danh mục Medication Template. | | |
| **Điều kiện sau (Post-conditions)** | Thuốc được loại bỏ khỏi danh mục của Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Xóa” tại dòng thuốc | Hệ thống hiển thị cảnh báo: “Bạn có chắc muốn xóa loại thuốc này khỏi danh mục mẫu?” |
| | 2 | Bác sĩ nhấn “Xác nhận xóa” | Hệ thống xóa loại thuốc khỏi cấu hình template và sắp xếp lại thứ tự các thuốc còn lại |
| **Mức độ ưu tiên (Priority)** | Trung bình (Medium) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR7 | | |

---

### 1.16 NHÓM UC-16: QUẢN LÝ CẤU HÌNH RECOVERY CHECK TEMPLATE (US-22 CRUD)

---

#### 1.16.1 UC-16.1: Tạo mốc thời gian và câu hỏi Recovery Check (Create Milestone & Questions)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-16.1 | | |
| **Tên Use Case (Use Case Name)** | Tạo mốc thời gian và câu hỏi Recovery Check (Create Milestone & Questions) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ tạo mới một mốc khảo sát phục hồi (ví dụ: Day 1, Day 3, Day 7) và thiết lập từ 3–5 câu hỏi khảo sát kèm đáp án và điều kiện cảnh báo. | | |
| **Mục tiêu (Goal)** | Thiết lập lịch trình và nội dung theo dõi triệu chứng phục hồi chuẩn hóa theo tiến trình lâm sàng | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn “Thêm mốc Recovery Check” trong tab Recovery Check của Template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đang cấu hình Care Plan Template. | | |
| **Điều kiện sau (Post-conditions)** | Mốc kiểm tra và bộ câu hỏi được lưu vào cấu hình Recovery Check của Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Thêm mốc Recovery Check” | Hệ thống hiển thị biểu mẫu tạo mốc kiểm tra |
| | 2 | Bác sĩ chọn Loại phẫu thuật áp dụng (Phaco, Lác, LASIK/ICL, Cắt dịch kính...) và nhập Mốc thời gian theo dõi (ví dụ: Ngày 1, Ngày 3, Ngày 7, Ngày 14 sau phẫu thuật) | Hệ thống xác nhận loại phẫu thuật và mốc thời gian |
| | 3 | Bác sĩ thêm 3–5 câu hỏi: Nhập nội dung câu hỏi (ví dụ: “Bệnh nhân có cảm thấy đau buốt không?”), Chọn loại đáp án (Có/Không, Nhiều lựa chọn), Thiết lập đáp án nào là “Bình thường”, đáp án nào là “Cần chú ý”, và đáp án nào kích hoạt “Red Flag” | Hệ thống liên kết câu hỏi với tiêu chí đánh giá cảnh báo |
| | 4 | Bác sĩ nhấn “Lưu mốc Recovery Check” | Hệ thống lưu mốc kiểm tra cùng bộ câu hỏi vào Template |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Trùng lặp mốc thời gian** | 1 | Bác sĩ tạo mốc Ngày 3 khi đã có mốc Ngày 3 | Hệ thống báo lỗi: “Mốc thời gian Ngày 3 đã tồn tại trong template.” |
| **E2: Số lượng câu hỏi không nằm trong khoảng 3–5** | 1 | Bác sĩ nhập dưới 3 câu hỏi | Hệ thống nhắc nhở: “Mỗi lần Recovery Check nên có từ 3 đến 5 câu hỏi để đảm bảo hiệu quả theo dõi.” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR9, BR10 | | |

---

#### 1.16.2 UC-16.2: Xem danh sách mốc và câu hỏi Recovery Check (View Milestones & Questions)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-16.2 | | |
| **Tên Use Case (Use Case Name)** | Xem danh sách mốc và câu hỏi Recovery Check (View Milestones & Questions) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem toàn bộ lịch trình các mốc Recovery Check và nội dung câu hỏi của từng mốc trong Template. | | |
| **Mục tiêu (Goal)** | Kiểm tra tính logic và liên tục của lộ trình theo dõi phục hồi | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ mở tab “Recovery Check Template”. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang mở trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Bảng tổng hợp các mốc và câu hỏi được hiển thị trực quan. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ mở tab “Recovery Check Template” | Hệ thống hiển thị danh sách các mốc: Day 1 (3 câu), Day 3 (5 câu), Day 7 (5 câu)... |
| | 2 | Bác sĩ nhấn mở rộng một mốc để xem chi tiết câu hỏi và tiêu chí cảnh báo | Hệ thống mở rộng danh sách chi tiết các câu hỏi và quy tắc gắn cờ cảnh báo tương ứng |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR10 | | |

---

#### 1.16.3 UC-16.3: Chỉnh sửa mốc thời gian và câu hỏi Recovery Check (Edit Milestone & Questions)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-16.3 | | |
| **Tên Use Case (Use Case Name)** | Chỉnh sửa mốc thời gian và câu hỏi Recovery Check (Edit Milestone & Questions) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ điều chỉnh nội dung câu hỏi, đổi mốc ngày hoặc cấu hình lại ngưỡng cảnh báo của một mốc Recovery Check. | | |
| **Mục tiêu (Goal)** | Tinh chỉnh bộ câu hỏi khảo sát phục hồi chính xác theo thực tế lâm sàng | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Sửa” tại một mốc kiểm tra. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Mốc kiểm tra tồn tại trong Recovery Check Template. | | |
| **Điều kiện sau (Post-conditions)** | Các câu hỏi và tiêu chí cảnh báo mới được cập nhật. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Sửa” tại một mốc kiểm tra | Hệ thống hiển thị biểu mẫu chỉnh sửa của mốc đó |
| | 2 | Bác sĩ chỉnh sửa câu từ câu hỏi, thay đổi đáp án hoặc đổi điều kiện kích hoạt cảnh báo Red Flag | Hệ thống kiểm tra tính hợp lệ |
| | 3 | Bác sĩ nhấn “Lưu cập nhật” | Hệ thống lưu thông tin mới và thông báo thành công |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR10 | | |

---

#### 1.16.4 UC-16.4: Xóa mốc thời gian hoặc câu hỏi Recovery Check (Delete Milestone / Question)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-16.4 | | |
| **Tên Use Case (Use Case Name)** | Xóa mốc thời gian hoặc câu hỏi Recovery Check (Delete Milestone / Question) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xóa bỏ một mốc kiểm tra không cần thiết hoặc xóa một câu hỏi đơn lẻ trong mốc. | | |
| **Mục tiêu (Goal)** | Tinh gọn quy trình theo dõi phục hồi, tránh gây phiền hà cho người chăm sóc | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Xóa” tại mốc hoặc câu hỏi. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Mốc hoặc câu hỏi có tồn tại trong cấu hình. | | |
| **Điều kiện sau (Post-conditions)** | Mốc kiểm tra hoặc câu hỏi bị loại bỏ khỏi Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Xóa” tại mốc hoặc câu hỏi | Hệ thống hỏi xác nhận xóa |
| | 2 | Bác sĩ xác nhận | Hệ thống xóa mục được chọn và cập nhật lại giao diện hiển thị |
| **Mức độ ưu tiên (Priority)** | Trung bình (Medium) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR10 | | |

---

### 1.17 NHÓM UC-17: QUẢN LÝ CẤU HÌNH RED FLAG TEMPLATE (US-23 CRUD)

---

#### 1.17.1 UC-17.1: Thêm dấu hiệu cảnh báo Red Flag (Create Red Flag Sign)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-17.1 | | |
| **Tên Use Case (Use Case Name)** | Thêm dấu hiệu cảnh báo Red Flag (Create Red Flag Sign) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ thêm một dấu hiệu nguy hiểm (Red Flag) cho loại phẫu thuật, thiết lập mức độ cảnh báo, điều kiện kích hoạt, chỉ dẫn xử lý khẩn cấp và số điện thoại hotline. | | |
| **Mục tiêu (Goal)** | Xác lập các tiêu chí nhận diện biến chứng nguy hiểm để bảo vệ an toàn cho bệnh nhân | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn “Thêm dấu hiệu Red Flag” trong tab Red Flag của Template. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đang cấu hình Care Plan Template. | | |
| **Điều kiện sau (Post-conditions)** | Dấu hiệu cảnh báo Red Flag mới được lưu vào cấu hình Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Thêm dấu hiệu Red Flag” | Hệ thống hiển thị biểu mẫu cấu hình dấu hiệu nguy hiểm |
| | 2 | Bác sĩ chọn/nhập các thông tin cảnh báo:<br>- **Loại phẫu thuật**: Chọn loại phẫu thuật áp dụng (Phaco, Lác, LASIK/ICL, Cắt dịch kính...)<br>- **Tên dấu hiệu nguy hiểm**: Đau nhức dữ dội, Đột ngột mờ mắt, Chảy máu vết mổ...<br>- **Mức độ cảnh báo**: Cấp cứu khẩn cấp / Khám trong ngày<br>- **Điều kiện kích hoạt**: Từ câu trả lời Recovery Check bất thường hoặc do Caregiver tự báo cáo<br>- **Hướng dẫn xử lý ban đầu**: Các bước sơ cứu/hành động khẩn cấp cần thực hiện ngay<br>- **Số điện thoại hotline cấp cứu**: Đường dây nóng trực 24/7 của bệnh viện | Hệ thống kiểm tra tính đầy đủ và hợp lệ của thông tin cảnh báo |
| | 3 | Bác sĩ nhấn “Lưu dấu hiệu Red Flag” | Hệ thống lưu cấu hình vào Template và hiển thị thông báo thành công |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thiếu số hotline cấp cứu** | 1 | Bác sĩ để trống số điện thoại khẩn cấp | Hệ thống báo lỗi: “Bắt buộc phải cấu hình số điện thoại khẩn cấp cho dấu hiệu Red Flag.” |
| **Mức độ ưu tiên (Priority)** | Cao (High - Đặc biệt quan trọng) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR11 | | |

---

#### 1.17.2 UC-17.2: Xem danh sách dấu hiệu cảnh báo Red Flag (View Red Flag Signs)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-17.2 | | |
| **Tên Use Case (Use Case Name)** | Xem danh sách dấu hiệu cảnh báo Red Flag (View Red Flag Signs) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem danh mục toàn bộ các dấu hiệu cảnh báo Red Flag đã thiết lập cho loại phẫu thuật. | | |
| **Mục tiêu (Goal)** | Kiểm tra và rà soát các dấu hiệu biến chứng được giám sát trong gói template | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ mở tab “Red Flag Template”. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang mở trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Bảng danh sách dấu hiệu Red Flag và hành động khẩn cấp được hiển thị. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ mở tab “Red Flag Template” | Hệ thống hiển thị bảng danh sách gồm: Tên dấu hiệu nguy hiểm, Mức độ cảnh báo, Điều kiện kích hoạt, Chỉ dẫn xử lý, Số hotline liên hệ |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR11 | | |

---

#### 1.17.3 UC-17.3: Chỉnh sửa dấu hiệu cảnh báo Red Flag (Edit Red Flag Sign)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-17.3 | | |
| **Tên Use Case (Use Case Name)** | Chỉnh sửa dấu hiệu cảnh báo Red Flag (Edit Red Flag Sign) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ cập nhật mô tả dấu hiệu, điều chỉnh hướng dẫn xử lý hoặc cập nhật số hotline cấp cứu của bệnh viện. | | |
| **Mục tiêu (Goal)** | Đảm bảo thông tin cấp cứu và hướng dẫn xử lý luôn chính xác và cập nhật | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Sửa” tại dòng dấu hiệu Red Flag. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Dấu hiệu Red Flag tồn tại trong Template. | | |
| **Điều kiện sau (Post-conditions)** | Thông số cập nhật của Red Flag được lưu vào hệ thống. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Sửa” tại dấu hiệu Red Flag | Hệ thống mở biểu mẫu chỉnh sửa thông số Red Flag |
| | 2 | Bác sĩ cập nhật nội dung chỉ dẫn, số điện thoại hotline hoặc mức độ cảnh báo | Hệ thống kiểm tra tính hợp lệ dữ liệu |
| | 3 | Bác sĩ nhấn “Lưu thay đổi” | Hệ thống lưu thông tin mới và thông báo cập nhật thành công |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR11 | | |

---

#### 1.17.4 UC-17.4: Xóa dấu hiệu cảnh báo Red Flag (Delete Red Flag Sign)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-17.4 | | |
| **Tên Use Case (Use Case Name)** | Xóa dấu hiệu cảnh báo Red Flag (Delete Red Flag Sign) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xóa bỏ một tiêu chí Red Flag khỏi cấu hình của Template. | | |
| **Mục tiêu (Goal)** | Điều chỉnh danh mục cảnh báo phù hợp với quy định chuyên môn | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn biểu tượng “Xóa” tại dòng dấu hiệu Red Flag. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Dấu hiệu Red Flag có trong danh sách. | | |
| **Điều kiện sau (Post-conditions)** | Dấu hiệu bị loại bỏ khỏi cấu hình Red Flag của Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Xóa” tại dòng Red Flag | Hệ thống hiển thị hộp thoại xác nhận xóa |
| | 2 | Bác sĩ nhấn “Xác nhận xóa” | Hệ thống xóa tiêu chí cảnh báo khỏi Template và thông báo thành công |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR11 | | |

---

### 1.18 NHÓM UC-18: QUẢN LÝ CẤU HÌNH DO & DON'T TEMPLATE (CRUD)

---

#### 1.18.1 UC-18.1: Thêm nội dung Nên làm / Cần tránh (Create Do & Don't Item)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-18.1 | | |
| **Tên Use Case (Use Case Name)** | Thêm nội dung Nên làm / Cần tránh (Create Do & Don't Item) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ thêm một chỉ dẫn sinh hoạt vào danh mục NÊN LÀM (Do) hoặc CẦN TRÁNH (Don't) cho loại phẫu thuật, thiết lập mốc thời gian áp dụng và giải thích y khoa. | | |
| **Mục tiêu (Goal)** | Thiết lập danh mục hướng dẫn sinh hoạt an toàn chuẩn hóa cho người chăm sóc | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn “Thêm việc Nên làm / Cần tránh” trong tab Do & Don't. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đang cấu hình Care Plan Template. | | |
| **Điều kiện sau (Post-conditions)** | Mục chỉ dẫn mới được bổ sung vào danh mục Do & Don't của Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Thêm chỉ dẫn Do/Don't” | Hệ thống hiển thị biểu mẫu tạo nội dung |
| | 2 | Bác sĩ chọn/nhập các thông tin chỉ dẫn:<br>- **Loại phẫu thuật**: Chọn loại phẫu thuật áp dụng (Phaco, Lác, LASIK/ICL, Cắt dịch kính...)<br>- **Phân loại**: NÊN LÀM (Do) hoặc CẦN TRÁNH (Don't)<br>- **Nhóm sinh hoạt**: Vệ sinh, Vận động, Ăn uống, Giấc ngủ<br>- **Tiêu đề hành vi**: Ví dụ: “Đeo kính bảo vệ mắt khi ngủ”, “Không để nước dính vào mắt”...<br>- **Giải thích y tế**: Lý do và cơ sở lâm sàng<br>- **Thời gian áp dụng**: Ví dụ: 7 ngày đầu, 1 tháng đầu sau phẫu thuật | Hệ thống kiểm tra tính đầy đủ và hợp lệ của dữ liệu chỉ dẫn |
| | 3 | Bác sĩ nhấn “Lưu chỉ dẫn” | Hệ thống lưu chỉ dẫn vào danh mục Do & Don't của Template |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Thiếu phân loại Do hoặc Don't** | 1 | Bác sĩ không chọn loại hành vi là Do hay Don't | Hệ thống nhắc nhở chọn phân loại rõ ràng |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR22 | | |

---

#### 1.18.2 UC-18.2: Xem danh sách nội dung Nên làm / Cần tránh (View Do & Don't Items)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-18.2 | | |
| **Tên Use Case (Use Case Name)** | Xem danh sách nội dung Nên làm / Cần tránh (View Do & Don't Items) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ xem toàn bộ danh mục các việc Nên làm và Cần tránh đã thiết lập cho Template, phân loại theo nhóm sinh hoạt. | | |
| **Mục tiêu (Goal)** | Kiểm tra tính đầy đủ của các khuyến cáo sinh hoạt hậu phẫu | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ mở tab “Do & Don't Template”. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Template đang mở trong hệ thống. | | |
| **Điều kiện sau (Post-conditions)** | Danh sách các chỉ dẫn Do & Don't được kết xuất trực quan theo 2 nhóm màu Xanh / Đỏ. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ mở tab “Do & Don't Template” | Hệ thống hiển thị bảng danh sách phân thành 2 cột: Cột Xanh (Do - Việc nên làm) và Cột Đỏ (Don't - Việc cần tránh) kèm nhóm sinh hoạt và thời gian áp dụng |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR22 | | |

---

#### 1.18.3 UC-18.3: Chỉnh sửa nội dung Nên làm / Cần tránh (Edit Do & Don't Item)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-18.3 | | |
| **Tên Use Case (Use Case Name)** | Chỉnh sửa nội dung Nên làm / Cần tránh (Edit Do & Don't Item) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ chỉnh sửa câu từ, giải thích y khoa hoặc thời gian áp dụng của một việc Nên làm hoặc Cần tránh. | | |
| **Mục tiêu (Goal)** | Cập nhật hướng dẫn sinh hoạt chính xác theo tiêu chuẩn điều trị mới | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Sửa” tại một mục Do hoặc Don't. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Mục chỉ dẫn tồn tại trong cấu hình. | | |
| **Điều kiện sau (Post-conditions)** | Nội dung cập nhật được lưu vào Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Sửa” tại một mục Do/Don't | Hệ thống mở biểu mẫu chỉnh sửa nội dung |
| | 2 | Bác sĩ cập nhật mô tả hướng dẫn hoặc thời gian áp dụng | Hệ thống kiểm tra dữ liệu |
| | 3 | Bác sĩ nhấn “Lưu cập nhật” | Hệ thống lưu thay đổi và thông báo thành công |
| **Mức độ ưu tiên (Priority)** | Trung bình (Medium) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR22 | | |

---

#### 1.18.4 UC-18.4: Xóa nội dung Nên làm / Cần tránh (Delete Do & Don't Item)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-18.4 | | |
| **Tên Use Case (Use Case Name)** | Xóa nội dung Nên làm / Cần tránh (Delete Do & Don't Item) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ loại bỏ một mục chỉ dẫn Do hoặc Don't khỏi danh mục của Template. | | |
| **Mục tiêu (Goal)** | Tinh giản các chỉ dẫn không còn phù hợp với phác đồ điều trị | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Xóa” tại một mục Do hoặc Don't. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Mục chỉ dẫn có trong danh sách. | | |
| **Điều kiện sau (Post-conditions)** | Mục chỉ dẫn bị loại bỏ khỏi Template. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Xóa” tại mục Do/Don't | Hệ thống hỏi xác nhận xóa |
| | 2 | Bác sĩ xác nhận | Hệ thống xóa mục và cập nhật lại giao diện |
| **Mức độ ưu tiên (Priority)** | Trung bình (Medium) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR22 | | |

---

### 1.19 UC-19: Tạo và tùy biến Care Plan cho bệnh nhân (Create & Tailor Patient Care Plan)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-19 | | |
| **Tên Use Case (Use Case Name)** | Tạo và tùy biến Care Plan cho bệnh nhân (Create & Tailor Patient Care Plan) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ khởi tạo Kế hoạch chăm sóc thực tế cho bệnh nhân bằng cách nhân bản từ Care Plan Template tương ứng với loại phẫu thuật, tùy chỉnh liều thuốc, lịch tái khám, và kích hoạt độc lập cho bệnh nhân. | | |
| **Mục tiêu (Goal)** | Thiết lập kế hoạch chăm sóc cá nhân hóa, chính xác theo đơn thuốc thực tế của bệnh nhân mà không ảnh hưởng đến template gốc | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn “Tạo Care Plan” từ hồ sơ bệnh nhân. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | 1. Bác sĩ đã đăng nhập với vai trò DOCTOR.<br>2. Hồ sơ bệnh nhân đã tồn tại.<br>3. Có ít nhất một Care Plan Template đang Hoạt động (Active) cho loại phẫu thuật của bệnh nhân. | | |
| **Điều kiện sau (Post-conditions)** | 1. Kế hoạch chăm sóc riêng của bệnh nhân được lưu với trạng thái `Active`.<br>2. Sẵn sàng tạo mã QR (UC-20).<br>3. Template gốc hoàn toàn không bị biến đổi. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Tạo Care Plan” trên hồ sơ bệnh nhân | Hệ thống lấy loại phẫu thuật của bệnh nhân (ví dụ: Phaco) và gợi ý Template đang hoạt động tương ứng |
| | 2 | Bác sĩ chọn template được gợi ý | Hệ thống sao chép toàn bộ 5 thành phần con của template (Learning Path, Đơn thuốc, Recovery Check, Red Flag, Do & Don't) sang giao diện Kế hoạch chăm sóc riêng của bệnh nhân |
| | 3 | Bác sĩ tùy chỉnh đơn thuốc theo đơn thực tế (thêm/bớt thuốc, chỉnh liều, thời điểm), thiết lập Lịch hẹn tái khám cụ thể (Ngày, giờ, bác sĩ phụ trách) | Hệ thống kiểm tra tính hợp lệ của các thông số tùy chỉnh |
| | 4 | Bác sĩ nhấn “Kích hoạt Care Plan” | Hệ thống lưu Kế hoạch chăm sóc với trạng thái `Active`, thông báo thành công và hiển thị nút “Tạo mã QR cho bệnh nhân” |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Lưu bản nháp (Save as Draft)** | 1 | Bác sĩ chọn “Lưu bản nháp” thay vì Kích hoạt | Hệ thống lưu Care Plan với trạng thái `Draft` để hoàn thiện sau |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Bệnh nhân đã có Care Plan Active** | 1 | Bệnh nhân đã có một Care Plan đang hoạt động cho đợt điều trị này | Hệ thống cảnh báo: “Bệnh nhân đã có một Care Plan đang hoạt động. Bạn có muốn lưu trữ kế hoạch cũ để kích hoạt kế hoạch mới không?” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR18, BR19 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR18 | Tính cô lập của Care Plan bệnh nhân | Mọi thay đổi và tùy chỉnh trên Care Plan của một bệnh nhân chỉ áp dụng riêng cho bệnh nhân đó, tuyệt đối không làm thay đổi Template gốc hoặc Care Plan của bệnh nhân khác. |
| BR19 | Quy tắc duy nhất một Care Plan hoạt động | Trong một đợt điều trị, một bệnh nhân chỉ có tối đa một Kế hoạch chăm sóc ở trạng thái Hoạt động (Active) tại một thời điểm. |

---

### 1.20 UC-20: Tạo mã QR cho bệnh nhân (Generate QR Code for Patient Care Plan)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-20 | | |
| **Tên Use Case (Use Case Name)** | Tạo mã QR cho bệnh nhân (Generate QR Code for Patient Care Plan) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Tác nhân chính: Bác sĩ (Doctor)<br>Tác nhân phụ: Caregiver, Bệnh nhân (Patient) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ sinh mã QR định danh bảo mật gắn với Kế hoạch chăm sóc đã kích hoạt của bệnh nhân để in hoặc tải file bàn giao cho người chăm sóc khi xuất viện. | | |
| **Mục tiêu (Goal)** | Cung cấp phương thức liên kết vật lý - kỹ thuật số an toàn giúp Caregiver truy cập đúng Care Plan của bệnh nhân | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ nhấn nút “Tạo mã QR” trên Kế hoạch chăm sóc đã kích hoạt. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Kế hoạch chăm sóc của bệnh nhân đang ở trạng thái Hoạt động (Active). | | |
| **Điều kiện sau (Post-conditions)** | Mã QR được tạo, lưu vào CSDL và kết xuất thành Phiếu hướng dẫn xuất viện có thể in ấn. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ nhấn “Tạo mã QR” trên Care Plan Active | Hệ thống kiểm tra trạng thái Active, tạo chuỗi token mã hóa bảo mật gắn liền với ID Care Plan và lưu vào CSDL |
| | 2 | Hệ thống hiển thị mẫu Phiếu hướng dẫn xuất viện có mã QR: Hình ảnh mã QR, Thông tin bệnh nhân, Bác sĩ điều trị, và 3 bước hướng dẫn Caregiver quét mã | Bác sĩ kiểm tra thông tin trên phiếu |
| | 3 | Bác sĩ nhấn “In phiếu QR” hoặc “Tải file PDF/ảnh” | Hệ thống xuất lệnh in ra máy in hoặc tải file về máy; Bác sĩ bàn giao cho bệnh nhân/người chăm sóc |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Tạo lại mã QR khi bị mất** | 1 | Phiếu QR bị mất, Bác sĩ nhấn “Tạo lại mã QR” | Hệ thống thu hồi mã token QR cũ, cấp token mới và in lại phiếu mới |
| **Luồng ngoại lệ (Exception Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **E1: Care Plan chưa kích hoạt** | 1 | Cố tạo mã QR khi Care Plan còn ở trạng thái Draft | Hệ thống chặn tạo mã và thông báo: “Chỉ có thể tạo mã QR cho Kế hoạch chăm sóc đã được Kích hoạt (Active).” |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR20 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR20 | Bảo mật và gắn kết mã QR | Mã QR chỉ được sinh cho các Kế hoạch chăm sóc ở trạng thái Active, phải chứa token mã hóa an toàn chống làm giả và duy trì nhật ký kiểm toán về bác sĩ phát hành cùng thời gian tạo. |

---

### 1.21 UC-21: Theo dõi Recovery Check của bệnh nhân (Monitor Patient Recovery Check)

| Mục / Trường | Bước / Mục con | Hành động của Tác nhân / Giá trị | Phản hồi của Hệ thống / Ghi chú |
|---|---|---|---|
| **Mã Use Case (Use Case ID)** | UC-21 | | |
| **Tên Use Case (Use Case Name)** | Theo dõi Recovery Check của bệnh nhân (Monitor Patient Recovery Check) | | |
| **Người tạo (Created by)** | Đội ngũ BA | **Người cập nhật (Last updated by)** | Đội ngũ BA |
| **Ngày tạo (Date Created)** | 11/09/2026 | **Ngày cập nhật (Date last updated)** | 11/09/2026 |
| **Tác nhân (Actors)** | Bác sĩ (Doctor) | | |
| **Mô tả tóm tắt (Brief Description)** | Bác sĩ theo dõi tiến trình hồi phục của các bệnh nhân qua các mốc Recovery Check do Caregiver nộp, xem chi tiết câu trả lời, nhận diện các câu trả lời bị đánh dấu bất thường hoặc Red Flag để can thiệp kịp thời. | | |
| **Mục tiêu (Goal)** | Giám sát liên tục quá trình hồi phục tại nhà của bệnh nhân và chủ động xử lý biến chứng sớm | | |
| **Tác nhân kích hoạt (Trigger)** | Bác sĩ truy cập mục “Theo dõi hồi phục” hoặc nhấn vào thông báo cảnh báo Recovery Check. | | |
| **Điều kiện tiên quyết (Pre-conditions)** | Bác sĩ đã đăng nhập với vai trò DOCTOR; có bệnh nhân đã nộp kết quả Recovery Check. | | |
| **Điều kiện sau (Post-conditions)** | Bác sĩ đã nắm bắt và xem xét chi tiết kết quả kiểm tra phục hồi của bệnh nhân. | | |
| **Luồng chính (Main Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| | 1 | Bác sĩ vào màn hình Theo dõi phục hồi | Hệ thống hiển thị danh sách bệnh nhân được phân nhóm ưu tiên theo thứ tự an toàn lâm sàng: Cảnh báo Red Flag 🔴, Cần chú ý 🟡, Quá hạn kiểm tra 🟠 (Default to Unsafe - BR12), Bình thường 🟢, và Chưa đến hạn ⚪ |
| | 2 | Bác sĩ chọn một bệnh nhân và bấm vào mốc kiểm tra (ví dụ: “Ngày 3”) | Hệ thống hiển thị phiếu kết quả gồm: Thời gian nộp, Danh sách câu hỏi và câu trả lời thực tế của Caregiver, và màu sắc đánh dấu các câu trả lời bất thường |
| **Luồng thay thế (Alternative Flow)** | **Bước** | **Hành động của Tác nhân** | **Phản hồi của Hệ thống** |
| **A1: Bác sĩ liên hệ Caregiver** | 1 | Khi phát hiện dấu hiệu bất thường hoặc Red Flag, Bác sĩ nhấn “Liên hệ Caregiver” | Hệ thống hiển thị số điện thoại của Caregiver kèm nút bấm gọi điện trực tiếp |
| **A2: Lọc danh sách bệnh nhân** | 1 | Bác sĩ lọc theo Loại phẫu thuật, Khoảng thời gian hoặc Mức độ cảnh báo (kể cả lọc nhóm Quá hạn 🟠) | Hệ thống cập nhật danh sách hiển thị tương ứng |
| **A3: Xử lý bệnh nhân quá hạn nộp bảng kiểm (Overdue Action)** | 1 | Với các ca bệnh nhân nằm trong nhóm Quá hạn 🟠 (> 4–6 giờ chưa nộp Recovery Check theo BR12), Bác sĩ bấm nút “Gọi nhắc Caregiver” trực tiếp cạnh tên bệnh nhân | Hệ thống khởi chạy cuộc gọi nhanh tới số điện thoại người chăm sóc, đồng thời ghi nhận mốc thời gian Bác sĩ đã gọi nhắc vào nhật ký theo dõi |
| **Luồng ngoại lệ (Exception Flow)** | Không áp dụng | Không có luồng ngoại lệ | |
| **Mức độ ưu tiên (Priority)** | Cao (High) | | |
| **Quy tắc nghiệp vụ (Business Rule)** | BR12, BR21 | | |

#### Quy tắc nghiệp vụ (Business Rules)
| Mã BR | Tên Quy tắc Nghiệp vụ | Mô tả Quy tắc Nghiệp vụ |
|---|---|---|
| BR12 | Cơ chế xử lý quá hạn và cảnh báo leo thang Recovery Check (Default to Unsafe) | Tự động phân loại các ca quá hạn kiểm tra vào nhóm cảnh báo 🟠 trên Dashboard Bác sĩ, hỗ trợ nút gọi nhanh liên hệ Caregiver để ngăn ngừa tình trạng biến chứng không được phát hiện do quên nộp bảng kiểm. |
| BR21 | Tự động phát hiện bất thường và phân loại lâm sàng | Hệ thống phải tự động gắn cờ cảnh báo đối với mọi lượt nộp Recovery Check có dấu hiệu bất thường hoặc thỏa mãn tiêu chí Red Flag, ưu tiên hiển thị ở vị trí trên cùng của Bảng điều khiển Bác sĩ. |

---

## 2. Bảng Tổng Hợp Danh Mục Quy Tắc Nghiệp Vụ Toàn Hệ Thống (Master Business Rules Catalog)

Bảng tổng hợp toàn bộ các Quy tắc Nghiệp vụ (Business Rules - BR) được thiết kế và thực thi xuyên suốt hai phân hệ Caregiver (Mobile App) và Bác sĩ (Doctor Web Portal), đảm bảo tính an toàn y khoa tuyệt đối, bảo mật dữ liệu và chuẩn hóa quy trình điều trị hậu phẫu nhãn khoa.

### 2.1 Bảng Ma Trận Quy Tắc Nghiệp Vụ (Master BR Matrix)

| Mã BR | Tên Quy Tắc Nghiệp Vụ | Phân Nhóm Nghiệp Vụ | Mô Tả Chi Tiết & Ràng Buộc Lâm Sàng / Hệ Thống | Phạm Vi Áp Dụng (Use Cases) | Màn Hình Liên Quan |
|---|---|---|---|---|---|
| **BR1** | Kiểm tra tính hợp lệ của số điện thoại | Xác thực & Định danh (Auth) | Xác thực định dạng số điện thoại chuẩn (10 chữ số, đầu số hợp lệ của các nhà mạng Việt Nam) trước khi kích hoạt quy trình sinh và gửi mã xác thực OTP qua SMS. | UC-01 | `SCR-CG-01` |
| **BR2** | Xác thực mã OTP | Xác thực & Định danh (Auth) | Mã OTP bao gồm 6 chữ số ngẫu nhiên, chỉ có hiệu lực trong khoảng thời gian quy định (60–120 giây). Sau 3 lần nhập sai liên tiếp hoặc quá thời gian, mã OTP tự động vô hiệu hóa. | UC-01 | `SCR-CG-01` |
| **BR3** | Phân quyền tài khoản Caregiver | Quản lý Người dùng & Phân quyền | Chỉ tài khoản người chăm sóc ở trạng thái Hoạt động (`status = 'ACTIVE'`) mới được xác thực thành công và phân quyền truy cập Kế hoạch chăm sóc (Care Plan) của bệnh nhân. | UC-01 | `SCR-CG-01`, `SCR-CG-02` |
| **BR4** | Tính hợp lệ của mã QR liên kết | Liên kết Bệnh nhân & Care Plan | Mã QR in trên phiếu xuất viện phải chứa token mã hóa hợp lệ gắn với Kế hoạch chăm sóc đang ở trạng thái Hoạt động (`ACTIVE`). Nếu Care Plan đã đóng (`COMPLETED` hoặc `ARCHIVED`), hệ thống từ chối liên kết. | UC-02 | `SCR-CG-03` |
| **BR5** | Tính toàn vẹn liên kết Caregiver - Bệnh nhân | Liên kết Bệnh nhân & Care Plan | Một Caregiver có thể chăm sóc nhiều bệnh nhân (ví dụ cả bố và mẹ), nhưng mỗi lượt quét QR phải hiển thị rõ thông tin xác nhận bệnh nhân và lưu nhật ký kiểm toán (Caregiver ID, Patient ID, thời gian liên kết). | UC-02 | `SCR-CG-03`, `SCR-CG-04` |
| **BR6** | Phạm vi nội dung được cấp quyền | Quản lý Truy cập Nội dung | Caregiver chỉ được xem nội dung đào tạo (Learning Path), đơn thuốc, lịch tái khám và bảng kiểm phục hồi thuộc Kế hoạch chăm sóc của bệnh nhân đã được liên kết với mình; tuyệt đối không rò rỉ dữ liệu chéo giữa các bệnh nhân. | UC-03 | `SCR-CG-04`, `SCR-CG-05`, `SCR-CG-06` |
| **BR7** | Nguồn gốc nội dung y khoa | Tiêu Chuẩn & An Toàn Y Khoa | Toàn bộ nội dung giáo dục, liều lượng thuốc, khoảng cách nhỏ thuốc và mốc theo dõi đều phải do Bác sĩ/Bệnh viện cấu hình hoặc phê duyệt. Hệ thống công nghệ tuyệt đối không tự động suy diễn, điều chỉnh liều hoặc sửa đổi khuyến nghị chuyên môn. | UC-03, UC-05, UC-06, Nhóm UC-14, Nhóm UC-15 | `SCR-CG-05`, `SCR-CG-08`, `SCR-CG-09`, `SCR-DOC-08` |
| **BR8** | Hoàn thành bài kiểm tra không gây chặn chức năng | Đào tạo Caregiver (Academy) | Bộ 3 câu hỏi trắc nghiệm Mini Quiz ở cuối bài học Learning Path nhằm mục đích củng cố hiểu biết và đo lường mức độ tiếp thu của người chăm sóc; kết quả làm bài không làm khóa hay giới hạn bất kỳ quyền truy cập nào vào các tính năng chăm sóc bệnh nhân. | UC-03 | `SCR-CG-07` |
| **BR9** | Bắt buộc hoàn thành bảng kiểm Recovery Check | Giám Sát Phục Hồi Hậu Phẫu | Caregiver bắt buộc phải trả lời đầy đủ 100% số câu hỏi (từ 3–5 câu) trong mốc khảo sát Recovery Check mới được phép bấm nộp kết quả lên hệ thống, đảm bảo không bỏ sót bất kỳ triệu chứng nhạy cảm nào. | UC-08, UC-16.1 | `SCR-CG-11` |
| **BR10** | Giới hạn chẩn đoán và phân loại trạng thái lâm sàng | Giám Sát Phục Hồi Hậu Phẫu | Hệ thống chỉ đóng vai trò đối chiếu câu trả lời với các tiêu chí do Bác sĩ thiết lập sẵn để phân nhóm trạng thái (Bình thường 🟢, Cần chú ý 🟡, Red Flag 🔴); hệ thống tuyệt đối không tự đưa ra kết luận chẩn đoán bệnh lý thay Bác sĩ. | UC-08, Nhóm UC-16 | `SCR-CG-11`, `SCR-DOC-12` |
| **BR11** | Ưu tiên quy trình hành động khẩn cấp (Emergency First) | Xử Lý Khẩn Cấp & Red Flag | Khi kích hoạt dấu hiệu Red Flag (từ bảng kiểm hoặc nút khẩn cấp), hệ thống lập tức chuyển thẳng sang màn hình Cấp cứu, hiển thị số hotline trực 24/7 và hướng dẫn xử trí tức thời; ưu tiên kết nối y tế khẩn cấp lên trên mọi tính năng tự chăm sóc khác. | UC-09, Nhóm UC-17 | `SCR-CG-12`, `SCR-DOC-12` |
| **BR12** | Cơ chế xử lý quá hạn và cảnh báo leo thang Recovery Check (Default to Unsafe) | An Toàn Vận Hành & Fallback | Khi Caregiver không nộp bảng kiểm đúng hạn: Sau 2h gửi Push Notification lần 2 (âm báo ưu tiên cao); Sau 4–6h gửi tin nhắn dự phòng SMS/Zalo ZNS, tự động chuyển ca sang nhóm cảnh báo Quá hạn 🟠 (Default to Unsafe) trên Bảng giám sát Bác sĩ và mở nút "Gọi nhắc Caregiver" để chủ động can thiệp bằng điện thoại. | UC-08, UC-21 | `SCR-CG-11`, `SCR-DOC-12` |
| **BR14** | Kiểm soát quyền truy cập của Bác sĩ (Doctor RBAC) | Quản trị & Phân quyền Bác sĩ | Chỉ các tài khoản đã được Quản trị viên bệnh viện chứng thực có vai trò `DOCTOR` và ở trạng thái Hoạt động (`status = 'ACTIVE'`) mới có quyền truy cập hồ sơ bệnh án, quản lý Master Template và bảng điều khiển theo dõi lâm sàng. | UC-11, Nhóm UC-12, Nhóm UC-13 | `SCR-DOC-01`, `SCR-DOC-02`, `SCR-DOC-07` |
| **BR15** | Thông tin bệnh nhân bắt buộc | Quản Lý Hồ Sơ Bệnh Nhân | Hồ sơ bệnh nhân khởi tạo bắt buộc phải có đầy đủ: Họ và tên, Ngày sinh hoặc Tuổi, Giới tính, Số điện thoại, Chẩn đoán y khoa ban đầu và Loại ca phẫu thuật mắt áp dụng. | Nhóm UC-12 (UC-12.1, 12.4) | `SCR-DOC-04`, `SCR-DOC-06` |
| **BR16** | Ràng buộc loại phẫu thuật của Care Plan Template | Cấu Hình Care Plan Template | Mỗi Care Plan Template chuẩn bắt buộc phải được gắn với duy nhất một loại phẫu thuật mắt chuyên khoa (Phaco, Lác, LASIK/ICL, Cắt dịch kính...), không cho phép tạo template chung chung không định danh phẫu thuật. | Nhóm UC-13 (UC-13.1, 13.4, 13.5) | `SCR-DOC-07`, `SCR-DOC-08` |
| **BR17** | Tính độc lập và nguyên khối của Template | Cấu Hình Care Plan Template | Care Plan Template là một gói cấu hình chuẩn hóa nguyên khối. Mọi thao tác chỉnh sửa nội dung trên master template chỉ áp dụng cho các Kế hoạch chăm sóc tạo mới sau này; tuyệt đối không làm biến động hay thay đổi các Care Plan đang được áp dụng cho bệnh nhân trước đó. | Nhóm UC-13 (UC-13.4, 13.5) | `SCR-DOC-07`, `SCR-DOC-08` |
| **BR18** | Tính cô lập của Care Plan bệnh nhân | Cá Nhân Hóa Điều Trị | Khi Bác sĩ tùy chỉnh đơn thuốc, lịch tái khám hay hướng dẫn riêng cho một bệnh nhân cụ thể, các thay đổi này chỉ lưu trong phạm vi Care Plan của bệnh nhân đó, hoàn toàn cô lập và không ảnh hưởng đến template mẫu hoặc bệnh nhân khác. | UC-19 | `SCR-DOC-10` |
| **BR19** | Quy tắc duy nhất một Care Plan hoạt động | Cá Nhân Hóa Điều Trị | Trong một đợt phẫu thuật điều trị, mỗi bệnh nhân chỉ được phép có duy nhất một Kế hoạch chăm sóc ở trạng thái Hoạt động (`status = 'ACTIVE'`) tại cùng một thời điểm. | UC-19 | `SCR-DOC-10` |
| **BR20** | Bảo mật và gắn kết mã QR | Liên Kết Kỹ Thuật Số | Mã QR chỉ được hệ thống sinh ra khi Kế hoạch chăm sóc đã được Bác sĩ kích hoạt (`ACTIVE`). Mã QR chứa token ngẫu nhiên mã hóa an toàn, liên kết chặt chẽ với ID Care Plan và được lưu vết kiểm toán đầy đủ. | UC-20 | `SCR-DOC-11` |
| **BR21** | Tự động phát hiện bất thường và phân loại lâm sàng | Giám Sát Phục Hồi Hậu Phẫu | Mọi lượt nộp Recovery Check có cờ Cảnh báo Red Flag 🔴 hoặc Cần chú ý 🟡 phải được hệ thống tự động đẩy lên vị trí ưu tiên cao nhất trên Bảng giám sát Bác sĩ để Bác sĩ xử lý tức thời. | UC-21 | `SCR-DOC-12` |
| **BR22** | Tính hiển thị trực quan danh mục Do & Don't | Hướng Dẫn Chăm Sóc | Danh mục hướng dẫn sinh hoạt bắt buộc phải phân tách bằng hai khối màu chuẩn trực quan: Màu Xanh lá cho việc NÊN LÀM (Do) và Màu Đỏ cảnh báo cho việc CẦN TRÁNH (Don't), kèm mốc thời gian kiêng cữ cụ thể. | UC-05, Nhóm UC-18 | `SCR-CG-08`, `SCR-DOC-08` |
| **BR23** | Ràng buộc khoảng cách thời gian nhỏ mắt | Quản Lý Dùng Thuốc | Khi bệnh nhân có từ 2 loại thuốc nhỏ mắt trở lên trong cùng một cữ dùng, ứng dụng phải tự động kích hoạt bộ đếm thời gian giãn cách tối thiểu 5 phút giữa các lần nhỏ để tránh hiện tượng rửa trôi dược chất. | UC-06, UC-15.1 | `SCR-CG-09`, `SCR-DOC-08` |
| **BR24** | Thời gian kích hoạt nhắc hẹn tái khám | Quản Lý Tái Khám | Hệ thống tự động gửi thông báo nhắc lịch tái khám đến thiết bị Caregiver theo 2 mốc tiêu chuẩn: Mốc 1 trước thời điểm hẹn 24 giờ và Mốc 2 trước thời điểm hẹn 2 giờ. | UC-07 | `SCR-CG-10` |
| **BR25** | Ràng buộc xóa và lưu trữ hồ sơ bệnh nhân | Quản Lý Vòng Đời Dữ Liệu | Hồ sơ bệnh nhân đã phát sinh dữ liệu lâm sàng, liên kết Care Plan hoặc lịch sử nộp bảng kiểm chỉ được phép áp dụng cơ chế Xóa mềm / Lưu trữ (`status = 'ARCHIVED'`); nghiêm cấm xóa vĩnh viễn (Hard Delete) khỏi cơ sở dữ liệu y tế. | UC-12.5 | `SCR-DOC-03`, `SCR-DOC-05` |

### 2.2 Phân Loại BR Theo 6 Trụ Cột Nghiệp Vụ Chính

1. **Nhóm Xác thực & Định danh Người dùng:** `BR1`, `BR2`, `BR3`, `BR14`
2. **Nhóm Liên kết Bệnh nhân & Bảo mật QR:** `BR4`, `BR5`, `BR20`
3. **Nhóm An toàn Y khoa & Dùng thuốc Chuẩn hóa:** `BR6`, `BR7`, `BR8`, `BR22`, `BR23`, `BR24`
4. **Nhóm Giám sát Phục hồi, Quá hạn & Khẩn cấp (Recovery & Emergency Core):** `BR9`, `BR10`, `BR11`, `BR12`, `BR21`
5. **Nhóm Quản lý Master Template & Chuẩn hóa Phẫu thuật:** `BR16`, `BR17`
6. **Nhóm Hồ sơ Bệnh nhân & Cá nhân hóa Care Plan:** `BR15`, `BR18`, `BR19`, `BR25`
