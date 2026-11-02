**Tên : Đặng Quang Anh** 

**Lớp : Ks26-CNTT1** 

**Session : 16**
# Vấn đề :
1. Thiết lập môi trường trình duyệt làm việc chuẩn Dev.

2. Hỗ trợ debug một lỗi hiển thị đang xảy ra trên trang nội bộ test.internal-tool.com.

Bài làm :
## Phần A – Thiết lập Profile và Bookmark chuẩn Dev:
1. Giải thích hiện tượng dùng chung 1 Profile cho GitHub cá nhân và GitHub công ty:

Xảy ra hiện tượng **Session** **Conflict** ****và ghi đè Cookies và Session Storage. Trình duyệt chỉ duy trì một phiên xác thực active cho tên miền `github.com` trong cùng một thư mục User Data.

1. Mô hình 3 Profile chuẩn Developer:

| STT  | Tên | Mục đích sử dụng |
| --- | --- | --- |
| 1 | Personal | Dành cho các hoạt động cá nhân, mạng xã hội, giải trí và tài khoản GitHub cá nhân. |
| 2 | Company_Work | Dành riêng cho môi trường công việc chính thức, lưu tài khoản GitHub công ty. |
| 3 | `Sandbox/Test` | Dành cho việc thử nghiệm các tiện ích mở rộng. |
1. Thiết kế cấu trúc Bookmark gồm 3 thư mục:

📁 `01_Work_Management`

[Jira Software](https://www.atlassian.com/software/jira)

[Confluence Documentation](https://www.atlassian.com/software/confluence)

📁 `02_Source_Control`

[GitHub Enterprise](https://github.com/)

[GitLab Company](https://gitlab.com/)

📁 `03``_Dev_unity`

[MDN Web Docs](https://developer.mozilla.org/)

[Stack Overflow](https://stackoverflow.com/)

1. Cho đồng nghiệp mượn máy 5 phút:

- Giải pháp: Cho họ dùng Guest Profile

- Lý do: Guest Profile tạo ra một không gian duyệt web hoàn toàn cô lập, không lưu lịch sử, không chia sẻ cookies hay mật khẩu với các profile chính.

## Phần B – Chọn đúng Extension hỗ trợ:
1. **Chọn** **Extension** **xử lý JSON thô:**

- Nhóm: Thuộc nhóm Dev Suite.

- Tên tiện ích: ` JSON  ``Formatter` 

- Vì sao 2 nhóm còn lại không phù hợp:

*Security* *Suite:* Chuyên dùng để quét bảo mật, quản lý mật khẩu hoặc chặn mã độc.

*Productivity* *Suite:* Tập trung vào tối ưu hóa công việc văn phòng.

1. **Cấu hình quyền** **Site** **Access** 

- Lựa chọn: Cấu hình là `On``   ``click`.

- Giải thích theo quy tắc quản trị Extensions:Thao tắc bảo mật và tối ưu hiệu năng, chỉ nên cấp quyền chạy extension khi người dùng chủ động kích hoạt trên các trang cần thiết.
## 
## Phần C – Phân tích luồng HTTP Request:
1. 4 giai đoạn xử lý khi gõ `test.internal-tool.com` và nhấn Enter:

- Giai đoạn 1: DNS Resolution 

- Giai đoạn 2: TCP Handshake và TLS Connection 

- Giai đoạn 3: HTTP Request và Response 

- Giai đoạn 4: Rendering 

1. So sánh mã trạng thái và hướng debug:

| **Mã trạng thái** | Ý nghĩa | Lỗi nằm ở đâu? | Hướng debug ban đầu |
| --- | --- | --- | --- |
| 404 Not Found | Không tìm thấy tài nguyên trên server. | Phía Client hoặc Routing phía Server cấu hình sai endpoint. | Kiểm tra lại đường dẫn URL, kiểm tra cấu hình Routing của Server. |
| ` 500  ``Internal``  Server  ``Error`Lỗi  | Lỗi máy chủ nội bộ | Phía Server. | Mở log của Server, kiểm tra mã nguồn backend. |
1. **Chọn Tab DevTools để xem mã trạng thái HTTP:**

- Tab cần mở: Network.

- Vì sao không chọn các tab còn lại:

*+*  *Elements:* Dùng để kiểm tra và chỉnh sửa cấu trúc CSS, không hiển thị các request mạng.

*+*  *Console:* Dùng để xem log lỗi JavaScript hoặc chạy lệnh script.

*+*  *Application:* Dùng để quản lý Storage , không chuyên trách theo dõi trạng thái truyền tải mạng thời gian thực

## 
## Phần D – Chẩn đoán Cache và Cookies:
1. Dev vừa sửa CSS nhưng F5 vẫn thấy giao diện cũ:

Nguyên nhân: Trình duyệt đang tải file CSS cũ được lưu trong bộ nhớ Browser Cache để tăng tốc độ tải trang mà không gửi request mới lên server.

Thao tác khắc phục: Nhấn tổ hợp phím `Ctrl``  + F5. `

1. Cookie `session_id` không có dấu tích ở cột `HttpOnly`:

- Rủi ro bảo mật: Mã độc JavaScript  có thể dễ dàng đọc được giá trị của cookie này thông qua lệnh `document.cookie`.

- Tên loại tấn công: Tấn công XSS nhằm đánh cắp phiên đăng nhập.

1. Đăng xuất nhưng vẫn giữ nguyên Cache ảnh/CSS:

- Thao tác thực hiện: Chỉ cần xóa  riêng các mục Cookies and other site data.

## Phần E – Debug bằng Inspect Element và Network Tab
1. Thao tác sửa màu nút "Đăng nhập" trực tiếp trên trình duyệt:

- Nhấp chuột phải vào nút "Đăng nhập", chọn Inspect  để mở tab Elements.

- Tại cửa sổ *Styles*, tìm đến dòng định nghĩa thuộc tính màu sắc

- Nhấp vào ô màu nhỏ bên cạnh, chỉnh sửa trực tiếp mã màu hoặc dùng công cụ Color Picker.

1. Quy trình 4 Bước Debugging khi request API hiện dòng màu đỏ trên tab Network:

- Bước 1 : Tái hiện lại lỗi bằng cách thao tác lại trên trang để request API đó tiếp tục xuất hiện màu đỏ.

- Bước 2 : Nhấp vào dòng request màu đỏ đó, kiểm tra chi tiết các tab con.

- Bước 3 : Khoanh vùng nguyên nhân  và thử nghiệm chỉnh sửa.

- Bước 4: Gửi lại request  và kiểm tra xem dòng request đã chuyển sang màu xanh hay chưa.

1. 2 thao tác cần bật trên tab Network để lọc API và chống cache:

- Thao tác 1: Chọn bộ lọc Fetch hay XHR ở thanh menu ngang phía trên tab Network để chỉ hiển thị các lời gọi API.

- Thao tác 2: Tích chọn ô Disable cache ở góc trên cùng của tab Network để đảm bảo mọi dữ liệu tải về luôn là bản mới nhất từ server.


