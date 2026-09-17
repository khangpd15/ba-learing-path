# ERD Description — Family Remote Care Platform
**Hệ thống Hỗ trợ Chăm sóc Bệnh nhân Phẫu thuật Mắt Từ xa**

> Tài liệu mô tả bản chất nghiệp vụ của toàn bộ các thực thể (entities) trên mô hình Conceptual/Logical ERD phục vụ đánh giá và nghiệm thu kiến trúc dữ liệu trước khi chuyển sang bước Thiết kế Vật lý (Physical Database Diagram).

---

## Bảng mô tả chi tiết thực thể (ERD Description Table)

| Table Name | Description |
| :--- | :--- |
| **User** | Thực thể quản lý tài khoản định danh và xác thực tập trung (Authentication & Authorization Master). Lưu trữ thông tin đăng nhập, trạng thái tài khoản dùng chung cho tất cả các đối tượng tương tác với hệ thống (Bác sĩ, Điều dưỡng, Người chăm sóc, Bệnh nhân). |
| **Caregiver** | Thực thể hồ sơ vai trò (Role Profile Master) dành cho Người chăm sóc, kế thừa (IS-A) từ User. Đại diện cho cá nhân chịu trách nhiệm trực tiếp đồng hành, giám sát và thực hiện quy trình chăm sóc bệnh nhân tại nhà sau phẫu thuật. |
| **PatientProfile** | Thực thể hồ sơ bệnh nhân (Patient Master Data), kế thừa/gắn liền với User của Bệnh nhân (Care Recipient). Quản lý thông tin định danh y tế, nhân khẩu học và bệnh sử liên quan đến quá trình điều trị mắt. |
| **PatientCaregiver** | Thực thể quan hệ trung gian (Associative Entity / Junction Data) quản lý liên kết nhiều-nhiều (N:M) giữa Bệnh nhân và Người chăm sóc; lưu trữ phân quyền ủy thác chăm sóc (ràng buộc nghiệp vụ tối đa 03 Caregiver chăm sóc một bệnh nhân). |
| **MedicalStaff** | Thực thể hồ sơ vai trò (Role Profile Master) dành cho nhân viên y tế, kế thừa (IS-A) từ User. Đại diện cho các chuyên gia y tế (Hospital Director, Trưởng khoa, Bác sĩ phẫu thuật/Surgeon, Điều dưỡng lâm sàng/Discharge Nurse, Chăm sóc khách hàng) thực hiện chỉ định chuyên môn và điều phối chăm sóc. |
| **AuditLog** | Dữ liệu lịch sử và giám sát hệ thống (Audit Trail / Historical Data). Ghi nhận bất biến các thao tác nghiệp vụ trọng yếu, thay đổi trạng thái dữ liệu, thời điểm và tác nhân thực hiện nhằm phục vụ an toàn thông tin và tuân thủ tiêu chuẩn y tế. |
| **CarePlanTemplate** | Mẫu kế hoạch chăm sóc chuẩn (Master Configuration / Blueprint). Tập hợp các gói chỉ định, hướng dẫn và lịch trình phục hồi sau mổ chuẩn hóa do Bác sĩ/Trưởng khoa xây dựng và phê duyệt, làm khung tham chiếu để áp dụng cho từng bệnh nhân. |
| **CarePlan** | Kế hoạch chăm sóc thực tế (Transactional Master Data). Đại diện cho một đợt chăm sóc phục hồi cụ thể được cá nhân hóa và kích hoạt cho một bệnh nhân trong một đợt điều trị, được khởi tạo từ CarePlanTemplate hoặc xây dựng trực tiếp bởi Bác sĩ. |
| **Prescription** | Đơn thuốc y tế (Unified Clinical Prescription - đảm nhận vai trò kép: Template mẫu hoặc Transactional thực tế). Đại diện cho phác đồ dùng thuốc chuẩn do nhân viên y tế soạn thảo (khi gắn vào Template) hoặc đơn thuốc điều trị cụ thể được kích hoạt cho bệnh nhân (khi gắn vào CarePlan). |
| **MedicationSchedule** | Lịch trình dùng thuốc chi tiết (Medication Regimen / Schedule Data). Xác định quy chuẩn liều lượng, thời điểm, tần suất và hướng dẫn dùng thuốc; tồn tại ở dạng khung lịch mẫu (theo Prescription mẫu) hoặc lịch dùng thuốc thực tế của bệnh nhân (theo Prescription trong CarePlan). |
| **MedicationIntake** | Dữ liệu nhật ký tuân thủ dùng thuốc (Transactional / Execution Log). Ghi nhận thực tế từng lần bệnh nhân hoặc người chăm sóc thực hiện uống/nhỏ thuốc theo lịch (ghi nhận trạng thái đã uống, bỏ lỡ, thời gian uống thực tế) để nhân viên y tế theo dõi độ tuân thủ. |
| **FollowUpAppointment** | Dữ liệu sự kiện tái khám (Clinical Event / Transactional Data). Quản lý lịch hẹn theo dõi và tái khám định kỳ sau phẫu thuật của bệnh nhân gắn liền với tiến trình của CarePlan và bác sĩ/nhân viên y tế phụ trách. |
| **LearningPath** | Lộ trình học tập kiến thức chăm sóc (Educational Structure - vai trò kép: Master Catalog hoặc Assigned Instance). Gom nhóm các bài học theo tiến trình hồi phục, có thể tạo độc lập bởi MedicalStaff, cấu hình sẵn trong CarePlanTemplate hoặc phân bổ cụ thể vào CarePlan cho Caregiver/Bệnh nhân học. |
| **MicroLearning** | Nội dung bài học ngắn (Educational Content Master). Chứa đựng kiến thức, video, hình ảnh chỉ dẫn cô đọng về chăm sóc mắt sau phẫu thuật theo từng chủ đề thuộc LearningPath. |
| **Quiz** | Bài đánh giá mức độ hiểu biết (Assessment Content). Đại diện cho bài kiểm tra gắn liền với một nội dung MicroLearning nhằm đảm bảo người chăm sóc nắm vững kiến thức, kỹ năng trước khi thực hành. |
| **QuizItem** | Câu hỏi kiểm tra (Assessment Item Content). Từng câu hỏi chi tiết và phương án kiểm tra thuộc về một bài Quiz để khảo sát kiến thức của Caregiver. |
| **RecoveryCheck** | Mốc đánh giá phục hồi (Milestone Assessment Configuration - vai trò kép: Master Template hoặc Instance). Định nghĩa các mốc thời gian kiểm tra tình trạng mắt sau phẫu thuật (như ngày 3, ngày 7, ngày 14...) do Bác sĩ tạo độc lập hoặc cấu hình trong Template/CarePlan. |
| **CheckQuestion** | Câu hỏi khảo sát triệu chứng phục hồi (Clinical Survey Item Content). Các câu hỏi lâm sàng chi tiết gắn cố định vào từng mốc RecoveryCheck cụ thể để thu thập tình trạng thực tế của mắt. |
| **RecoveryCheckResponse** | Kết quả đánh giá phục hồi thực tế (Transactional Clinical Outcome). Ghi nhận kết quả trả lời của bệnh nhân/caregiver tại một mốc RecoveryCheck trong khuôn khổ CarePlan; là bản ghi kết quả duy nhất (1:1 với RecoveryCheck instance), không cho phép nộp lại sau khi hoàn thành. |
| **RecoveryGuideline** | Bộ cẩm nang hướng dẫn phục hồi (Care Guidance Catalog - vai trò kép: Master Template hoặc Instance). Tập hợp các khuyến cáo chăm sóc chuyên môn (việc nên làm, cần tránh sau mổ) do MedicalStaff tạo độc lập, gắn vào CarePlanTemplate hoặc phân bổ vào CarePlan. |
| **GuidelineItem** | Điều khoản hướng dẫn cụ thể (Guideline Detail Content). Từng chỉ dẫn hoặc lưu ý y tế chi tiết nên làm và không nên làm nằm trong bộ RecoveryGuideline. |
| **RedFlags** | Bộ tiêu chuẩn cảnh báo dấu hiệu nguy hiểm (Safety Configuration - vai trò kép: Master Template hoặc Instance). Cấu hình tập hợp các triệu chứng bất thường cấp cứu sau phẫu thuật cần theo dõi sát sao, gắn theo Template hoặc CarePlan. |
| **RedFlagItem** | Dấu hiệu cảnh báo bất thường cụ thể (Safety Alert Item Content). Chi tiết từng triệu chứng nguy cơ cao (như đau nhức dữ dội, mờ mắt đột ngột, đỏ mắt tăng dần) thuộc bộ cảnh báo RedFlags để người chăm sóc nhận diện và xử lý khẩn cấp. |
