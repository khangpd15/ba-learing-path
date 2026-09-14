# 1. Epic: Caregiver Academy

# 2. User Stories chi tiết

**Acceptance Criteria:**

---

## US-02 — Đăng nhập tài khoản Caregiver

> **Là một Caregiver, tôi muốn đăng nhập vào hệ thống, để truy cập thông tin và các chức năng chăm sóc bệnh nhân mà tôi được liên kết.**
> 

**Acceptance Criteria:**

- Caregiver nhập thông tin đăng nhập.
- Hệ thống xác thực tài khoản.
- Đăng nhập thành công → truy cập hệ thống.
- Sai thông tin → hiển thị lỗi.
- Tài khoản chỉ truy cập được chức năng phù hợp với quyền Caregiver.
- Có thể đăng xuất.

## US-03 — Quét QR hồ sơ bệnh nhân

**Acceptance Criteria:**

```
Caregiver
   ↓
Đăng nhập
   ↓
Quét QR
   ↓
Xác định bệnh nhân
   ↓
Xác nhận
   ↓
Liên kết Caregiver ↔ Patien
```

**Acceptance Criteria:**

- Sau bài học có thể có Mini Quiz.

## US-05 — Caregiver 24h đầu cần làm gì

**Acceptance Criteria:**

- Có hình ảnh/video minh họa.

**Acceptance Criteria:**

- Mỗi thuốc hiển thị các thông tin được bác sĩ/bệnh viện cung cấp, bao gồm:
    - Tên thuốc.
    - Liều lượng.
    - Thời điểm sử dụng.
    - Cách sử dụng.
    - Mô tả loại thuốc
- Với thuốc nhỏ mắt, hệ thống có thể cung cấp hướng dẫn cách nhỏ thuốc đúng cách bằng video/hình ảnh(từ learning path).
- Nếu có nhiều loại thuốc, hệ thống hiển thị hướng dẫn giúp Caregiver phân biệt và sử dụng đúng theo Care Plan(từ learning path).
- Caregiver có thể xem lại hướng dẫn về thuốc bất cứ khi nào cần(từ learning path).

**Acceptance Criteria:**

**Acceptance Criteria:**

**Acceptance Criteria:**

**Lưu ý:** Nội dung y khoa cụ thể nên lấy từ bác sĩ/bệnh viện, không để hệ thống tự suy diễn.

---

## US-10 — Recovery Check

> **Là một Caregiver, tôi muốn được yêu cầu trả lời Recovery Check định kỳ về tình trạng của bệnh nhân, để bác sĩ/bệnh viện có thể theo dõi quá trình hồi phục và phát hiện sớm những dấu hiệu bất thường.**
> 

**Acceptance Criteria:**

- Hệ thống tạo Recovery Check theo lịch do bác sĩ/bệnh viện thiết lập.
- Caregiver nhận thông báo khi đến thời gian Recovery Check.
- Caregiver **bắt buộc trả lời** các câu hỏi trong Recovery Check.
    - Mỗi lần Recovery Check gồm khoảng **3–5 câu hỏi** phù hợp với loại phẫu thuật và giai đoạn hồi phục.
- Hệ thống lưu lại câu trả lời và thời gian thực hiện.
- Bác sĩ/bệnh viện có thể xem kết quả Recovery Check.
- Nếu câu trả lời xuất hiện dấu hiệu bất thường, hệ thống đánh dấu để bác sĩ/bệnh viện chú ý.
- Caregiver được hướng dẫn hành động phù hợp nếu phát hiện dấu hiệu cần xử lý.
- Hệ thống không tự đưa ra chẩn đoán.

### Flow

```
Care Plan
   │
   ▼
Thiết lập Recovery Check
   │
   ▼
Caregiver nhận thông báo
   │
   ▼
Trả lời 3–5 câu hỏi
   │
   ▼
Lưu kết quả
   │
   ├───────────────► Bác sĩ / Bệnh viện
   │                    │
   │                    ▼
   │              Theo dõi phục hồi
   │
   ▼
Câu trả lời Recovery Check
          ↓
Đối chiếu tiêu chí đã cấu hình
          ↓
┌─────────┼──────────┐
↓         ↓          ↓
Bình     Cần chú ý   Red Flag
thường
          │
          ▼
    Bác sĩ/Bệnh viện
       theo dõi
```

Ví dụ:

```
Recovery Check – Day 3

1. Hôm nay bệnh nhân còn đau không?
   ○ Không
   ○ Có, nhẹ
   ○ Có, nhiều

2. Mắt có đỏ bất thường không?
   ○ Không
   ○ Có

3. Bệnh nhân đã sử dụng thuốc đúng lịch?
   ○ Có
   ○ Không
```

---

## US-11 — Emergency Action

**Là một Caregiver, tôi muốn được hướng dẫn phải làm gì khi bệnh nhân xuất hiện dấu hiệu bất thường, để tôi có thể nhanh chóng thực hiện hành động phù hợp.**

**Acceptance Criteria:**

Khi phát hiện Red Flag:

```
Red Flag
   ↓
Hiển thị hướng dẫn
   ↓
┌────────────────────────┐
│ Liên hệ VISI           │
│ Gọi số điện thoại      │
│ Đến bệnh viện          │
│ Xem hướng dẫn xử lý    │
└────────────────────────┘
```

- Hệ thống hiển thị hướng dẫn rõ ràng.
- Có thông tin liên hệ được bệnh viện cấu hình.
- Không để Caregiver tự chẩn đoán bệnh.
- Nếu cần cấp cứu → ưu tiên hướng dẫn liên hệ cơ sở y tế/cấp cứu.

---

## US-12 — Mini Quiz

**Là một Caregiver, tôi muốn làm bài kiểm tra ngắn sau mỗi nội dung học, để biết mình đã hiểu đúng cách chăm sóc bệnh nhân hay chưa.**

**Acceptance Criteria:**

- Mỗi bài có khoảng 3 câu hỏi.
- Có câu hỏi lựa chọn đáp án.
- Hiển thị kết quả.
- Cho biết câu trả lời đúng/sai.
- Có thể giải thích ngắn cho đáp án.
- Ghi nhận trạng thái hoàn thành.

Ví dụ:

> Bệnh nhân vừa phẫu thuật mắt. Caregiver nên làm gì?
> 

```
○ Dụi mắt khi ngứa
● Nhắc bệnh nhân không dụi mắt
○ Tự mua thêm thuốc
○ Ngừng thuốc khi thấy đỡ
```

---

## US-13 — Tra cứu nội dung chăm sóc

**Là một Caregiver, tôi muốn dễ dàng truy cập và xem lại các nội dung trong Learning Path, để tôi có thể tìm hiểu cách chăm sóc bệnh nhân bất cứ khi nào cần.**

**Acceptance Criteria:**

- Caregiver xem được các nội dung được đề xuất cho bệnh nhân.
- Có thể chọn từng chủ đề để xem.
- Có thể xem lại nội dung đã xem hoặc chưa xem.
- Nội dung được tổ chức theo từng chủ đề chăm sóc.
- Không bắt buộc caregiver phải học theo tiến độ cố định.

---

## US-14 — Tìm hiểu theo tình huống chăm sóc

> **Là một Caregiver, tôi muốn dễ dàng truy cập các nội dung liên quan trong Learning Path khi có nhu cầu chăm sóc cụ thể, để tôi có thể tìm hiểu cách xử lý phù hợp.**
> 

Không cần imply AI.

Ví dụ:

```
Tôi cần biết cách nhỏ thuốc
        ↓
Medication Coach

Tôi cần biết có được tắm không
        ↓
Do & Don't

Tôi lo bệnh nhân có dấu hiệu bất thường
        ↓
Red Flag
```

---

# 2. User Story phía Bác sĩ

## **US-15 — Đăng ký tài khoản Bác sĩ**

> **Là một Bác sĩ, tôi muốn đăng ký tài khoản trên hệ thống, để có thể quản lý hồ sơ bệnh nhân và thiết lập kế hoạch chăm sóc.**
> 

**Acceptance Criteria:**

- Bác sĩ nhập thông tin đăng ký cần thiết.
- Xác thực thông tin đăng ký.
- Tạo tài khoản Bác sĩ thành công.
- Tài khoản có role **DOCTOR**.
- Không cho phép trùng thông tin định danh/tài khoản.
- Có thông báo rõ ràng khi đăng ký thất bại.

---

## **US-16 — Đăng nhập tài khoản Bác sĩ**

> **Là một Bác sĩ, tôi muốn đăng nhập vào hệ thống, để truy cập các chức năng quản lý bệnh nhân và Care Plan.**
> 

**Acceptance Criteria:**

- Bác sĩ nhập thông tin đăng nhập.
- Hệ thống xác thực tài khoản.
- Đăng nhập thành công → vào màn hình dành cho Bác sĩ.
- Sai thông tin → thông báo lỗi.
- Chỉ tài khoản có quyền phù hợp mới truy cập được chức năng Doctor.
- Có đăng xuất.

---

## US-17 — Nhập thông tin bệnh nhân

> **Là một Bác sĩ, tôi muốn nhập thông tin cơ bản và thông tin điều trị của bệnh nhân, để hệ thống có đầy đủ thông tin xác định bệnh nhân và vấn đề bệnh nhân đang điều trị.**
> 

**Acceptance Criteria:**

**AC1 — Nhập thông tin định danh bệnh nhân**

- Bác sĩ có thể nhập các thông tin cơ bản của bệnh nhân:
    - Họ và tên
    - Ngày sinh / tuổi
    - Giới tính
    - Thông tin cần thiết khác theo quy định của hệ thống.

**AC2 — Nhập thông tin điều trị**

- Bác sĩ có thể nhập thông tin về vấn đề bệnh nhân đang điều trị, ví dụ:
    - Bệnh lý / chẩn đoán
    - Vấn đề sức khỏe cần điều trị
    - Ghi chú y tế cần thiết.

**AC3 — Kiểm tra thông tin bắt buộc**

- Hệ thống xác định các trường bắt buộc.
- Không cho lưu nếu thiếu thông tin bắt buộc.
- Hiển thị thông báo để bác sĩ bổ sung hoặc chỉnh sửa.

**AC4 — Lưu hồ sơ bệnh nhân**

- Khi thông tin hợp lệ, bác sĩ có thể lưu hồ sơ.
- Hệ thống tạo/lưu **Patient Record** để sử dụng ở các bước tiếp theo.

---

## US-18 — Quản lý Care Plan Template

**Là một Bác sĩ, tôi muốn tạo và quản lý Care Plan Template theo từng loại phẫu thuật, để có thể tái sử dụng cấu hình chăm sóc cho nhiều bệnh nhân mà không phải thiết lập lại từ đầu.**

**Acceptance Criteria:**

- Có thể tạo Template cho từng loại phẫu thuật.
- Ví dụ:
    - Phaco
    - Lác
    - LASIK/ICL
    - Võng mạc
- Một Template có thể chứa:
    - Learning Path.
    - Caregiver 101 Template
    - Medication Template.
    - Recovery Check Template.
    - Red Flag Template.
    - Do & Don't Template.
    - Follow-up Template.
- Có thể chỉnh sửa Template.
- Có thể kích hoạt/vô hiệu hóa Template.
- Template có thể được sử dụng cho nhiều bệnh nhân.

---

## US-19 — Cấu hình Caregiver Nên Làm Gì Trong 24h đầu

**Là một Bác sĩ, tôi muốn cấu hình nội dung hướng dẫn Caregiver theo từng loại phẫu thuật, để caregiver biết những việc cần làm và cần tránh trong 24 giờ đầu sau khi bệnh nhân xuất viện.**

**Acceptance Criteria:**

- Có thể cấu hình Caregiver 101 cho từng loại phẫu thuật.
- Có thể cấu hình các nội dung caregiver cần thực hiện trong 24 giờ đầu.
- Có thể cấu hình các nội dung caregiver cần lưu ý hoặc tránh.
- Có thể sử dụng:
    - Văn bản.
    - Hình ảnh.
    - Video.
    - Infographic.
- Có thể sắp xếp thứ tự nội dung.
- Có thể cập nhật nội dung khi hướng dẫn của bệnh viện thay đổi.
- Nội dung được sử dụng lại cho nhiều bệnh nhân cùng loại phẫu thuật.

### Ví dụ

```
Phaco Caregiver 101
│
├── Sau khi về nhà cần làm gì?
│
├── Hướng dẫn bệnh nhân nghỉ ngơi
│
├── Cách bảo vệ mắt
│
├── Những hoạt động cần tránh
│
├── Lưu ý khi sử dụng thuốc
│
└── Khi nào cần liên hệ bệnh viện?
```

---

## US-20 — Cấu hình Learning Path

> **Là một Bác sĩ, tôi muốn tạo Learning Path gồm các nội dung hướng dẫn chăm sóc phù hợp với từng loại phẫu thuật, để Caregiver có thể dễ dàng tìm hiểu và thực hiện chăm sóc bệnh nhân đúng cách.**
> 

**Acceptance Criteria:**

**AC1 — Tạo Learning Path**

- Bác sĩ có thể tạo Learning Path.
- Nhập tên Learning Path.
- Chọn loại phẫu thuật áp dụng.
- Nhập mô tả/mục đích của Learning Path.

**AC2 — Nội dung đa phương tiện**

Mỗi nội dung hướng dẫn có thể sử dụng:

- Video
- Hình ảnh
- Text
- Infographic
- Hoặc kết hợp nhiều hình thức.

**AC3 — Sắp xếp nội dung**

Bác sĩ có thể:

- Thêm nội dung
- Xóa nội dung
- Thay đổi thứ tự
- Chỉnh sửa nội dung

**AC4 — Caregiver xem Learning Path**

- Caregiver có thể mở Learning Path được cung cấp cho bệnh nhân.
- Có thể chọn nội dung muốn xem.
- Có thể xem lại nội dung bất cứ lúc nào.
- Không bắt buộc phải hoàn thành toàn bộ Learning Path.

---

## US-21 — Cấu hình Medication Template

**Là một Bác sĩ, tôi muốn cấu hình mẫu hướng dẫn và lịch sử dụng thuốc cho từng loại phẫu thuật, để có thể áp dụng nhanh cho bệnh nhân mà không phải nhập lại các thông tin giống nhau.**

**Acceptance Criteria:**

Có thể cấu hình:

- Tên thuốc.
- Liều lượng.
- Số lần sử dụng.
- Thời điểm sử dụng.
- Thời gian sử dụng.
- Trước/sau ăn nếu có.
- Cách sử dụng.
- Thứ tự sử dụng giữa các thuốc nếu có.

Ví dụ:

```
Phaco Medication Template

Thuốc A
├── 1 giọt
├── 4 lần/ngày
└── 7 ngày

Thuốc B
├── 1 giọt
├── 3 lần/ngày
└── 14 ngày
```

**Lưu ý:** Template là cấu hình mặc định. Khi áp dụng cho bệnh nhân, bác sĩ vẫn có thể điều chỉnh theo đơn thực tế.

---

## US-22 — Cấu hình Recovery Check

**Là một Bác sĩ, tôi muốn cấu hình các câu hỏi và lịch Recovery Check theo loại phẫu thuật, để hệ thống có thể tự động thu thập thông tin phù hợp trong quá trình bệnh nhân hồi phục.**

**Acceptance Criteria:**

Có thể cấu hình:

- Thời điểm thực hiện Recovery Check.
- Tần suất thực hiện.
- Số lượng câu hỏi.
- Nội dung câu hỏi.
- Loại câu trả lời.
- Ngưỡng/điều kiện cần chú ý.
- Dấu hiệu Red Flag liên quan.

Ví dụ:

```
Phaco Recovery Check

Day 1
→ 3 câu hỏi

Day 3
→ 5 câu hỏi

Day 7
→ 5 câu hỏi

Day 14
→ 3 câu hỏi
```

---

## US-23 — Cấu hình Red Flag

**Là một Bác sĩ, tôi muốn cấu hình các dấu hiệu cảnh báo theo từng loại phẫu thuật, để hệ thống có thể nhận biết những trường hợp cần được nhân viên y tế chú ý.**

**Acceptance Criteria:**

Có thể cấu hình:

- Dấu hiệu.
- Mức độ cảnh báo.
- Điều kiện kích hoạt.
- Hướng dẫn xử lý.
- Thông tin liên hệ tương ứng.

Ví dụ:

```
Recovery Check
      ↓
Câu trả lời
      ↓
Đối chiếu điều kiện đã cấu hình
      ↓
┌──────────────┬──────────────┐
│ Bình thường  │   Red Flag   │
└──────────────┴──────────────┘
                       ↓
                Bác sĩ chú ý
```

---

## US-24 — Tạo Care Plan cho bệnh nhân

Đây mới là **Care Plan thực tế của từng bệnh nhân**.

**Là một Bác sĩ, tôi muốn tạo Care Plan cho từng bệnh nhân dựa trên Template của loại phẫu thuật, để nhanh chóng thiết lập kế hoạch chăm sóc phù hợp mà không phải cấu hình lại từ đầu.**

**Acceptance Criteria:**

- Bác sĩ chọn bệnh nhân.
- Bác sĩ chọn loại phẫu thuật.
- Hệ thống đề xuất Template tương ứng.
- Hệ thống tạo Care Plan từ Template.
- Bác sĩ có thể xem lại toàn bộ cấu hình.
- Bác sĩ có thể điều chỉnh những thông tin khác với Template.
- Care Plan được lưu riêng cho bệnh nhân.
- Thay đổi Care Plan của một bệnh nhân **không làm thay đổi Template hoặc Care Plan của bệnh nhân khác**.

### Flow

```
Bệnh nhân
    ↓
Chọn loại phẫu thuật
    ↓
Phaco
    ↓
Phaco Care Plan Template
    │
    ├── Learning Path
    ├── Medication
    ├── Recovery Check
    ├── Red Flag
    ├── Do & Don't
    └── Follow-up
    ↓
Tạo Patient Care Plan
    ↓
Bác sĩ review
    ↓
Điều chỉnh nếu cần
    ↓
Publish / Activate
```

---

## US-25 — Tạo QR cho bệnh nhân

Sau khi Care Plan đã được tạo:

**Là một Bác sĩ, tôi muốn tạo mã QR gắn với Care Plan của bệnh nhân, để caregiver có thể truy cập đúng kế hoạch chăm sóc sau khi bệnh nhân xuất viện.**

### Flow

```
Patient
   ↓
Patient Care Plan
   ↓
Generate QR
   ↓
QR được cấp cho bệnh nhân
   ↓
Caregiver quét QR
   ↓
Xác nhận liên kết
   ↓
Learning Path + Care Plan
```

---

## US-26 — Theo dõi Recovery Check của bệnh nhân

> **Là một Bác sĩ, tôi muốn xem kết quả Recovery Check của bệnh nhân sau xuất viện, để theo dõi tình trạng hồi phục và kịp thời phát hiện những trường hợp cần được hỗ trợ.**
> 

**Acceptance Criteria:**

- Xem được danh sách Recovery Check của bệnh nhân.
- Xem câu trả lời và thời gian trả lời.
- Theo dõi được tình trạng qua các ngày.
- Các câu trả lời có dấu hiệu bất thường được đánh dấu.
- Có thể xem lại lịch sử Recovery Check.

Như vậy kiến trúc nghiệp vụ sẽ **sạch hơn rất nhiều**:

```
                 CARE PLAN
                    │
       ┌────────────┴────────────┐
       ▼                         ▼
 LEARNING PATH              RECOVERY CHECK
       │                         │
       │                    BẮT BUỘC
       │                         │
       ▼                         ▼
Caregiver tự xem          Caregiver trả lời
khi cần                         │
       │                         ▼
       │                  Dữ liệu sức khỏe
       │                         │
       │                         ▼
       │                Bác sĩ / Bệnh viện
       │                         │
       ▼                         ▼
Biết cách chăm sóc       Theo dõi hồi phục
đúng cách                & phát hiện bất thường
```

---