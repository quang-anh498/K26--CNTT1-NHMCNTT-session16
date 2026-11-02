**Tên : Đặng Quang Anh** 

**Lớp : Ks26-CNTT1** 

**Session : 16**
# Vấn đề :  
Xung đột phiên đăng nhập (Session Conflict): Khi Bác sĩ trực đang mở hồ sơ kê đơn cho bệnh nhân A, bạn thực tập sinh mở thêm một tab đăng nhập tài khoản Cổng học tập PTIT / GitHub cá nhân, khiến hệ thống CarePulse bị văng đăng xuất (Logout) giữa chừng. • Lộ lọt riêng tư: Khi Bác sĩ trưởng khoa dùng máy chiếu để hội chẩn ca bệnh nặng với tuyến trên, thanh lịch sử duyệt web và các gợi ý tìm kiếm cá nhân (mua sắm Shopee, Facebook, video Youtube) hiện lên trước mặt toàn thể đồng nghiệp. • Rác thanh Bookmark: Thanh dấu trang chứa hơn 80 liên kết lộn xộn, không phân loại, khiến việc tìm đường link Tra cứu Dược thư Quốc gia mất tới 3 phút mỗi ca khám.

# Bài làm :
## Phần 1: Báo cáo phân tích kiến trúc đa Profile :
1. Phân tích nguyên nhân sâu xa dẫn đến hiện tượng "văng phiên đăng nhập".

- **Góc độ lưu trữ** **User** **Data:** Khi nhiều nhân sự (bác sĩ, điều dưỡng, thực tập sinh) sử dụng chung một Profile mặc định trên Google Chrome, toàn bộ dữ liệu phiên làm việc gồm **Cookies**, **Session** **Storage**, của các ứng dụng web đều được ghi chung vào một thư mục gốc duy nhất.

- **Cơ chế ghi đè** **Cookies** **phiên (Session** **Cookies):** Các hệ thống web enterprise xác thực người dùng dựa trên HTTP Cookies hoặc token định danh phiên.

2. Thiết kế Mô hình 3 Profile chuyên dụng cho máy tính trạm khám CarePulse

| Tiêu chí | Profile 1: Work | Profile 2: Personal | Profile 2: Personal |
| --- | --- | --- | --- |
| **Tên Profile** | CarePulse_Official | Clinic_Operations | Intern_Sandbox |
| **Mục đích sử dụng** | Dành cho các Bác sĩ thao tác hệ thống khám chữa bệnh kê khai đơn và tra cứu BHYT. | Dành cho điều dưỡng tra cứu email nội bộ. | Danh cho những người, sinh viên thực tập để kiểm tra tính năng ưeb mới. |
| **Màu nhận diện** | Xanh dương y tế.  | Xanh lá nhạt. |  Cam cảnh báo.  |
| **Dữ liệu lưu trữ độc lập** | **Cookies:** Chỉ chứa phiên đăng nhập CarePulse & BHYT.• **Bookmarks:** Thư mục chuyên ngành.• **Extensions:** Tiện ích y tế, chặn quảng cáo sạch. | • **Cookies:** Tài khoản email viện, cổng thông tin nội bộ.• **Bookmarks:** Link biểu mẫu, quy trình.• **Extensions:** Hỗ trợ văn phòng. | • **Cookies:** Tài khoản thực tập, GitHub, test lab.• **Bookmarks:** Tài liệu học tập.• **Extensions:** Công cụ dev |
# Phần 2: Thiết kế hệ thống Bookmark & Quy trình an toàn:
3. Cây thư mục Bookmark chuẩn (Ctrl + Shift + O)

Plaintext

` 📁 Thư mục Thanh dấu trang  `

`|--``  📁 01_CarePulse_Clinic ` ` │    `` |--  ``Hệ thống quản lý CarePulse```` │    ``|--``  Cổng tra cứu BHYT trực tuyến `

```|--`` 📁 02_Pharma_Porta` ` │    `` |--  ``Dược thư Quốc gia Việt Nam`

`│   ``|--`` Tra cứu danh mục thuốc BHYT`

`│   ``|--`` Cảnh giác dược & ADR Quốc gia`

` |--  ``📁 03_Medical_Tools` ` │    `` |--  ``Google``  Dịch Y Khoa (Hội chẩn) `

` │    `` |--  `` Tra cứu chỉ số XN Lâm sàng  ``Lab``   ``Tests``   ``Online`

` │    `` |--  `` Công cụ tính toán liều lượng thuốc  `

  

4. Hướng dẫn áp dụng mẹo Favicon-Only tiết kiệm diện tích gấp 3 lần:

1. **Thao tác thực hiện:** Nhấp chuột phải vào từng thư mục hoặc đường dẫn trang web đã lưu trên thanh Bookmark Bar.

2. Chọn **Chỉnh sửa** 

3. Tại ô **Tên** , hãy **xóa trắng hoàn toàn** văn bản tiêu đề  rồi bấm **Lưu** 

4. **Kết quả:** Trình duyệt Google Chrome sẽ tự động co gọn các thẻ chỉ hiển thị biểu tượng nhỏ gọn.

5. Xử lý Bẫy an ninh & Cảnh báo Cloud Sync

- **Lựa chọn chế độ duyệt** **web:** Khi có đoàn thanh tra hoặc khách mượn máy tính kiểm tra đột xuất trong 30 phút, nhân viên phòng khám **bắt buộc phải sử dụng** **Guest** **Profile** ****

- **Giải thích lý do:**

Chế độ **Incognito** vẫn cho phép người dùng truy cập các tệp đã tải xuống hoặc có thể bị lộ một số trạng thái  nếu không cấu hình kỹ.

**Guest** **Profile** tạo ra một không gian hoàn toàn cô lập, sạch sẽ tuyệt đối, không chia sẻ bất kỳ thông tin lịch sử.

- **Cảnh báo nguy cơ khi bật Cloud Sync tài khoản cá nhân:**

Tuyệt đối **không** bật tính năng đồng bộ hóa đám mây  bằng tài khoản Google cá nhân trên máy tính trạm y tế dùng chung.

  - **Nguy cơ:** Nếu bật Sync, toàn bộ mật khẩu đã lưu , lịch sử duyệt web cá nhân, thẻ tín dụng và dữ liệu cá nhân của nhân sự sẽ bị đồng bộ ngược về thiết bị cá nhân gây đến rò rỉ thông tin đánh mất dữ liệu cá nhân.


