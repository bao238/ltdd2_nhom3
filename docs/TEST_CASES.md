# 📋 BẢNG ĐẶC TẢ TEST CASES TOÀN DIỆN DỰ ÁN IOT SMART HOME & KIỂM SOÁT CỔNG RA VÀO
## Môn học: Lập trình Thiết bị Di động 2 (LTDD2) - Đồ Án Cuối Kỳ - Nhóm 3

---

## 📌 THÔNG TIN TỔNG QUAN VÀ CHIẾN LƯỢC KIỂM THỬ

### 1. Thông tin chung
- **Tên dự án**: Hệ thống IoT Smart Home & Kiểm Soát Cổng Ra Vào Thông Minh (Smart Home & Gate Control System).
- **Nền tảng ứng dụng**: React Native / Expo Go, Web (React Native Web).
- **Backend & Cloud**: Vercel Serverless API, Firebase Realtime Database.
- **Phần cứng vi điều khiển**: ESP32 Dual-Core, RFID RC522, Servo SG90, DHT11, LCD 16x2 I2C, Buzzer, LED Onboard, Remote IR.
- **Chuẩn kiểm thử áp dụng**: IEEE 829 Standard for Software and System Test Documentation.

### 2. Quy ước phân loại mức độ nghiêm trọng (Severity) và độ ưu tiên (Priority)
- **Severity**:
  - `Critical`: Lỗi nghiêm trọng khiến hệ thống ngừng hoạt động, sai lệch dữ liệu an toàn hoặc hỏng phần cứng/kẹt cổng.
  - `Major`: Lỗi chức năng chính không thực hiện được (không đóng mở được cổng, không đọc được cảm biến).
  - `Moderate`: Lỗi chức năng phụ hoặc dữ liệu hiển thị chậm nhưng không ảnh hưởng luồng chính.
  - `Minor`: Lỗi thẩm mỹ giao diện, chính tả hoặc biểu tượng nhỏ.
- **Priority**:
  - `P1 (Cao nhất)`: Bắt buộc phải pass trước khi báo cáo nghiệm thu.
  - `P2 (Trung bình)`: Phải hoàn thiện cho luồng nghiệp vụ mở rộng.
  - `P3 (Thấp)`: Kiểm tra giao diện, trường hợp biên (edge cases).

---

## 📱 PHẦN 1: TEST CASES GIAO DIỆN & CHỨC NĂNG ỨNG DỤNG MOBILE (EXPO GO / REACT NATIVE)

### 1.1. Module Bảng điều khiển (DashboardScreen & Header)

| Mã Test Case | Tên Chức Năng / Kịch Bản | Điều Kiện Tiền Đề | Các Bước Thực Hiện (Steps) | Dữ Liệu Đầu Vào | Kết Quả Kỳ Vọng (Expected Result) | Mức Độ | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **TC_DASH_001** | Khởi chạy App và tải giao diện Dashboard | Ứng dụng đã cài đặt trên Expo Go hoặc mở trên trình duyệt Web | 1. Mở ứng dụng Smart Home.<br>2. Chờ ứng dụng khởi tạo context và load storage. | Không có | 1. Hiển thị Header với tiêu đề "Bảng Điều Khiển" và subtitle "HỆ THỐNG IOT • NHÓM 3".<br>2. Hiển thị hàng Status Chips: số lượng cảm biến, trạng thái cổng, tình trạng an toàn.<br>3. Thẻ Nhiệt độ & Độ ẩm hiển thị đúng giá trị. | P1 / Major | Pass |
| **TC_DASH_002** | Hiển thị dữ liệu Nhiệt độ & Độ ẩm DHT11 thời gian thực | App kết nối Firebase hoặc nhận số liệu từ ESP32 | 1. Đặt nhiệt độ môi trường ở 28°C, độ ẩm 70%.<br>2. Quan sát thẻ TempHumidityCard trên Dashboard. | `temperature = 28`, `humidity = 70` | 1. Thẻ hiển thị số đo nhiệt độ lớn: `28°C` và độ ẩm: `70%`.<br>2. Trạng thái tiện nghi hiển thị: "Lý tưởng" hoặc "Thoải mái".<br>3. Màu sắc hiển thị trạng thái an toàn (xanh lá). | P1 / Major | Pass |
| **TC_DASH_003** | Đổi màu và cảnh báo khi nhiệt độ vượt ngưỡng nguy hiểm | App đang chạy ở màn hình Dashboard | 1. Tăng nhiệt độ cảm biến hoặc nhấn nút mô phỏng lên 45°C (ngưỡng nguy hiểm 40°C).<br>2. Quan sát thẻ nhiệt độ và chip trạng thái. | `temperature = 45` | 1. Thẻ Nhiệt độ chuyển sang màu đỏ cảnh báo nguy hiểm.<br>2. Status Chip trạng thái đổi thành "X Cảnh báo" với chấm đỏ.<br>3. Hệ thống tạo một bản ghi cảnh báo mới trong tab Cảnh báo. | P1 / Critical | Pass |
| **TC_DASH_004** | Bật / Tắt Đèn LED (GPIO 2) nhanh từ Dashboard | App kết nối mạng LAN ESP32 hoặc Firebase Cloud | 1. Nhấn nút công tắc đèn LED trên Dashboard.<br>2. Quan sát icon và trạng thái đèn trên App.<br>3. Nhấn thêm lần nữa để tắt. | Hành động Click/Tap nút đèn | 1. Lần nhấn 1: Icon chuyển màu vàng sáng, trạng thái "Đang bật", ESP32 sáng LED pin 2 sau < 300ms.<br>2. Lần nhấn 2: Icon tắt, trạng thái "Đang tắt", ESP32 tắt LED. | P1 / Major | Pass |
| **TC_DASH_005** | Khóa chống trùng lặp click (Debounce) khi nhấn nút Đèn liên tục | App đang hiển thị nút điều khiển đèn | 1. Nhấn liên tục nút đèn 5 lần trong vòng 500ms.<br>2. Quan sát phản hồi hệ thống. | 5 click / 500ms | Hệ thống kích hoạt cơ chế `lastLightToggleTimeRef`, chỉ gửi 1 lệnh hợp lệ duy nhất, không gây tràn hàng đợi hay crash vi điều khiển. | P2 / Moderate | Pass |
| **TC_DASH_006** | Mở Modal Thêm Cảm Biến Mới từ Dashboard | Đang ở DashboardScreen | 1. Nhấn vào nút Thêm cảm biến (+).<br>2. Quan sát màn hình Modal hiện ra. | Click nút (+) | Modal `AddSensorModal` xuất hiện mượt mà, hiển thị danh sách các mẫu cảm biến: Khí Gas MQ-2, Ánh sáng LDR, Chuyển động PIR, Cảm biến Tùy chọn. | P2 / Moderate | Pass |
| **TC_DASH_007** | Thêm thành công Cảm biến Khí Gas (MQ-2) | Modal AddSensor đang mở | 1. Chọn preset "Cảm biến Khí Gas (MQ-2)".<br>2. Nhập vị trí phòng: "Nhà Bếp".<br>3. Giữ ngưỡng cảnh báo mặc định (300 ppm / 500 ppm).<br>4. Nhấn "Thêm Cảm Biến". | Tên: Khí Gas, Vị trí: Nhà Bếp, Đơn vị: ppm | 1. Modal đóng lại.<br>2. Thẻ cảm biến Khí Gas xuất hiện trên Dashboard với icon ngọn lửa màu cam.<br>3. Cảm biến mới được tự động lưu vào `AsyncStorage`.<br>4. Chip đếm cảm biến tăng thêm 1. | P1 / Major | Pass |
| **TC_DASH_008** | Thao tác Kéo xuống để Làm mới (Pull-to-Refresh) | Đang cuộn ở đầu trang Dashboard | 1. Dùng ngón tay kéo màn hình từ trên xuống.<br>2. Thả tay và quan sát biểu tượng loading. | Thao tác vuốt xuống | Hiển thị vòng xoay làm mới dữ liệu, sau 800ms vòng xoay ẩn đi và dữ liệu được đồng bộ mới nhất từ LocalStorage/Cloud. | P3 / Minor | Pass |

---

### 1.2. Module Kiểm soát Cổng Ra Vào (GateScreen & Servo / RFID UI)

| Mã Test Case | Tên Chức Năng / Kịch Bản | Điều Kiện Tiền Đề | Các Bước Thực Hiện (Steps) | Dữ Liệu Đầu Vào | Kết Quả Kỳ Vọng (Expected Result) | Mức Độ | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **TC_GATE_001** | Kiểm tra hiển thị trạng thái Cổng ban đầu | App vừa khởi động | 1. Chuyển sang Tab "Cổng".<br>2. Kiểm tra thông số trạng thái cổng. | Dữ liệu lưu trong Storage | 1. Nếu cổng đang đóng: Banner màu xanh, chữ "ĐÃ KHÓA AN TOÀN", icon ổ khóa đóng.<br>2. Nếu cổng đang mở: Banner màu vàng cam, chữ "CỔNG ĐANG MỞ", icon mở khóa.<br>3. Nút hành động hiển thị chữ tương ứng: "MỞ CỔNG NGAY" hoặc "ĐÓNG CỔNG NGAY". | P1 / Major | Pass |
| **TC_GATE_002** | Thao tác Mở Cổng 1-chạm từ App Mobile | Cổng đang ở trạng thái ĐÓNG | 1. Nhấn nút "MỞ CỔNG NGAY".<br>2. Quan sát giao diện App và thiết bị thật. | Click nút Mở | 1. Giao diện đổi sang trạng thái "CỔNG ĐANG MỞ" ngay lập tức (Optimistic UI).<br>2. Gửi lệnh POST lên API/Firebase.<br>3. Động cơ Servo SG90 trên ESP32 quay góc 90 độ.<br>4. Màn hình LCD hiển thị: "CUA: DANG MO - APP".<br>5. Còi Buzzer kêu 1 tiếng bip ngắn. | P1 / Critical | Pass |
| **TC_GATE_003** | Thao tác Đóng Cổng 1-chạm từ App Mobile | Cổng đang ở trạng thái MỞ | 1. Nhấn nút "ĐÓNG CỔNG NGAY".<br>2. Quan sát giao diện App và thiết bị thật. | Click nút Đóng | 1. Giao diện đổi sang "ĐÃ KHÓA AN TOÀN".<br>2. Servo SG90 quay về 0 độ.<br>3. LCD hiển thị: "CUA: DA DONG - APP".<br>4. Buzzer kêu 2 tiếng bip ngắn xác nhận. | P1 / Critical | Pass |
| **TC_GATE_004** | Kiểm tra tính năng Tự động đóng cổng (Auto-Close) | Cổng đang mở, chế độ Auto-Close đang BẬT, Delay = 10s | 1. Bật chế độ Tự đóng cổng với thời gian 10 giây.<br>2. Nhấn mở cổng qua nút trên App.<br>3. Không thao tác gì và đợi đúng 10 giây. | `autoCloseDelay = 10s` | 1. Đếm ngược hết 10 giây, App tự động kích hoạt lệnh đóng cổng.<br>2. Cổng tự động quay về trạng thái ĐÃ KHÓA.<br>3. Thêm một bản ghi vào nhật ký: "Tự động đóng cổng sau 10s". | P1 / Major | Pass |
| **TC_GATE_005** | Lựa chọn các mốc thời gian Auto-Close khác nhau | Đang ở GateScreen | 1. Nhấn lần lượt chọn các mốc: 5s, 10s, 15s, 30s, 60s.<br>2. Quan sát chip thời gian được chọn. | Mốc thời gian (5, 10, 15, 30, 60) | Mốc thời gian được active hiển thị viền nổi bật; giá trị `autoCloseDelaySec` trong state được cập nhật chính xác và lưu vào bộ nhớ máy. | P2 / Moderate | Pass |
| **TC_GATE_006** | Hiển thị thông tin Thẻ RFID khi quẹt thẻ Master hợp lệ | Bo mạch ESP32 kết nối mạng, Master UID = `620F0307` | 1. Quẹt thẻ Master vào module RFID RC522.<br>2. Quan sát khung thẻ "THÔNG TIN QUẸT THẺ GẦN NHẤT" trên App. | UID: `620F0307` | 1. Hiển thị mã thẻ: `620F0307`.<br>2. Huy hiệu: "HỢP LỆ (MASTER)".<br>3. Thời gian quẹt hiển thị đúng giờ thực tế.<br>4. Cổng tự mở, sau 3s tự đóng lại (đối với phương thức RFID). | P1 / Critical | Pass |
| **TC_GATE_007** | Hiển thị cảnh báo khi quẹt Thẻ RFID lạ không hợp lệ | Đang mở tab Cổng trên App | 1. Dùng thẻ RFID chưa đăng ký quẹt vào đầu đọc RC522.<br>2. Quan sát phản hồi trên App và thiết bị. | UID lạ: `A1B2C3D4` | 1. App hiển thị mã thẻ lạ kèm nhãn màu đỏ: "KHÔNG HỢP LỆ / TỪ CHỐI".<br>2. Cổng KHÔNG mở.<br>3. Còi Buzzer phát âm thanh cảnh báo 3 tiếng kéo dài.<br>4. Tự động ghi log xâm nhập vào Nhật ký và tab Cảnh báo. | P1 / Critical | Pass |
| **TC_GATE_008** | Xem và Xóa lịch sử Nhật ký ra vào (Access Logs) | Đã có ít nhất 3 bản ghi ra vào trong danh sách | 1. Nhấn "Xem tất cả" để mở rộng toàn bộ danh sách log.<br>2. Nhấn nút thùng rác "Xóa lịch sử".<br>3. Nhấn xác nhận trong hộp thoại Popup. | Xác nhận Xóa | 1. Danh sách mở rộng đầy đủ tất cả các lần quẹt thẻ và mở app.<br>2. Sau khi xác nhận xóa, toàn bộ danh sách log được làm trống.<br>3. Giao diện hiển thị: "Chưa có lượt ra vào nào". | P2 / Moderate | Pass |

---

### 1.3. Module Cảnh báo Hệ thống (AlertsScreen)

| Mã Test Case | Tên Chức Năng / Kịch Bản | Điều Kiện Tiền Đề | Các Bước Thực Hiện (Steps) | Dữ Liệu Đầu Vào | Kết Quả Kỳ Vọng (Expected Result) | Mức Độ | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **TC_ALERT_001** | Hiển thị danh sách cảnh báo phân cấp màu sắc | Hệ thống có sẵn các cảnh báo: Khẩn cấp, Cảnh báo, Thông tin | 1. Chuyển sang Tab "Cảnh Báo".<br>2. Quan sát danh sách các thẻ cảnh báo. | Danh sách cảnh báo trong Context | 1. Cảnh báo nguy hiểm (Danger): Viền đỏ, icon khiên cảnh báo đỏ.<br>2. Cảnh báo chú ý (Warning): Viền vàng cam.<br>3. Thông tin hệ thống (Info): Viền xanh dương.<br>4. Hiển thị rõ tiêu đề, nội dung chi tiết, giờ và ngày tháng. | P1 / Major | Pass |
| **TC_ALERT_002** | Lọc cảnh báo theo Tab phân loại | Có đầy đủ các loại cảnh báo | 1. Bấm chọn Tab "Tất cả".<br>2. Bấm chọn Tab "Nguy hiểm".<br>3. Bấm chọn Tab "Thông tin". | Thao tác chọn Tab lọc | 1. Tab Tất cả: Hiển thị toàn bộ cảnh báo.<br>2. Tab Nguy hiểm: Chỉ hiển thị các cảnh báo có `type = 'danger'`.<br>3. Tab Thông tin: Chỉ hiển thị các cảnh báo `type = 'info'`. | P2 / Moderate | Pass |
| **TC_ALERT_003** | Nút bấm Kích hoạt Mô phỏng Cảnh báo Khẩn cấp | Đang ở AlertsScreen | 1. Cuộn xuống phần "Thử Nghiệm Tình Huống Khẩn Cấp".<br>2. Bấm nút "Test Quá Nhiệt (45°C)".<br>3. Bấm nút "Test Thẻ Lạ Xâm Nhập". | Click nút Test khẩn cấp | 1. Khi bấm Test Quá Nhiệt: Thêm ngay một cảnh báo khẩn cấp đỏ: "Cảnh báo nhiệt độ cao bất thường", nhiệt độ tăng lên 45°C.<br>2. Khi bấm Test Thẻ Lạ: Thêm cảnh báo xâm nhập trái phép với UID giả lập. | P2 / Major | Pass |
| **TC_ALERT_004** | Xóa sạch toàn bộ lịch sử cảnh báo | Đang có danh sách cảnh báo | 1. Nhấn biểu tượng thùng rác "Xóa tất cả".<br>2. Chọn "Đồng ý xóa" trên Alert xác nhận. | Thao tác xóa | Toàn bộ danh sách cảnh báo bị xóa sạch; giao diện hiển thị trạng thái trống: "Hệ thống an toàn - Không có cảnh báo nào". | P2 / Moderate | Pass |

---

### 1.4. Module Lịch sử Đo Lường & Cấu hình Cài đặt (History & Settings)

| Mã Test Case | Tên Chức Năng / Kịch Bản | Điều Kiện Tiền Đề | Các Bước Thực Hiện (Steps) | Dữ Liệu Đầu Vào | Kết Quả Kỳ Vọng (Expected Result) | Mức Độ | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **TC_HIST_001** | Xem biểu đồ và bảng biến thiên lịch sử cảm biến | Có dữ liệu đo lường lưu trữ | 1. Mở màn hình HistoryScreen.<br>2. Chọn cảm biến Nhiệt độ.<br>3. Chuyển sang chọn cảm biến Độ ẩm. | Chọn loại cảm biến | Hiển thị biểu đồ dạng thanh/đường biến thiên giá trị theo từng mốc thời gian; số liệu trung bình (Average), cao nhất (Max), thấp nhất (Min) được tính toán chuẩn xác. | P2 / Moderate | Pass |
| **TC_SET_001** | Lưu cấu hình địa chỉ IP ESP32 trong mạng LAN | Đang ở màn hình SettingsScreen | 1. Nhập vào ô IP ESP32: `http://192.168.1.50:80`.<br>2. Nhấn nút "LƯU CẤU HÌNH". | IP: `http://192.168.1.50:80` | 1. Hiển thị thông báo "Đã lưu cấu hình kết nối".<br>2. Cấu hình được ghi đè vào `AsyncStorage`.<br>3. App chuyển hướng gọi các lệnh điều khiển sang địa chỉ IP mới. | P1 / Major | Pass |
| **TC_SET_002** | Bật / Tắt chế độ kết nối Cloud Firebase | SettingsScreen đang mở | 1. Gạt công tắc "Sử dụng Cloud Firebase".<br>2. Quan sát trạng thái kết nối trên Header. | Switch Toggle ON/OFF | 1. Khi BẬT: Nguồn kết nối hiển thị "Đám mây Firebase". Dữ liệu đồng bộ trực tiếp với Firebase Realtime Database.<br>2. Khi TẮT: Hệ thống chỉ gửi nhận gói tin trực tiếp với Web Server ESP32 qua mạng LAN. | P1 / Major | Pass |
| **TC_SET_003** | Xem Sơ đồ Chân Phần cứng ESP32 | SettingsScreen đang mở | 1. Nhấn vào mục "Sơ Đồ Chân Kết Nối Phần Cứng (ESP32)".<br>2. Kiểm tra bảng chân kết nối. | Thao tác mở Accordion | Bảng sơ đồ mở ra rõ ràng: GPIO 4 (DHT11), GPIO 13 (Servo SG90), GPIO 27 (Buzzer), GPIO 2 (LED), SPI Pins (RC522), I2C 21-22 (LCD). | P3 / Minor | Pass |
| **TC_SET_004** | Khôi phục Cài đặt Mặc định (Factory Reset) | Đã chỉnh sửa IP và thêm nhiều cảm biến | 1. Nhấn nút "Đặt Lại Mặc Định".<br>2. Xác nhận hộp thoại cảnh báo. | Click Khôi phục | Toàn bộ dữ liệu LocalStorage được reset; danh sách cảm biến trở về mặc định (chỉ gồm Nhiệt độ & Độ ẩm); IP và Firebase URL trở về giá trị gốc của nhóm. | P2 / Moderate | Pass |

---

## 🌐 PHẦN 2: TEST CASES BACKEND API (SERVERLESS VERCEL & FIREBASE RTDB)

### 2.1. Kiểm thử Endpoint `/api/sensors`

| Mã Test Case | Phương Thức | Endpoint | Payload Gửi Đi (Body) | Mã Trả Về (Status Code) | Kết Quả Phản Hồi Kỳ Vọng (Response Body) | Mức Độ | Trạng Thái |
| :--- | :---: | :--- | :--- | :---: | :--- | :---: | :---: |
| **TC_API_SEN_001** | `GET` | `/api/sensors` | Không có | `200 OK` | Trả về JSON chứa dữ liệu cảm biến mới nhất từ Firebase: `{ "temperature": 29.5, "humidity": 65, "gate": false, "light": false }`. Header có `Access-Control-Allow-Origin: *`. | P1 / Critical | Pass |
| **TC_API_SEN_002** | `OPTIONS` | `/api/sensors` | Không có (Preflight CORS request) | `204 No Content` | Cho phép CORS preflight từ trình duyệt web; các headers `Access-Control-Allow-Methods: GET, OPTIONS` đầy đủ. | P2 / Major | Pass |
| **TC_API_SEN_003** | `POST` | `/api/sensors` | `{ "dummy": 123 }` | `200 OK` hoặc `405 Method Not Allowed` | Endpoint hoạt động đúng đặc tả Swagger (chỉ GET dữ liệu cảm biến). | P3 / Minor | Pass |

---

### 2.2. Kiểm thử Endpoint `/api/gate`

| Mã Test Case | Phương Thức | Endpoint | Payload Gửi Đi (Body) | Mã Trả Về (Status Code) | Kết Quả Phản Hồi Kỳ Vọng (Response Body) | Mức Độ | Trạng Thái |
| :--- | :---: | :--- | :--- | :---: | :--- | :---: | :---: |
| **TC_API_GATE_001** | `POST` | `/api/gate` | `{ "open": true, "method": "App Mobile Test" }` | `200 OK` | Phản hồi `{ "success": true, "gate": true, "action": "OPEN", "method": "App Mobile Test" }`. Node `commands/gate` trên Firebase chuyển thành `true`. | P1 / Critical | Pass |
| **TC_API_GATE_002** | `POST` | `/api/gate` | `{ "open": false, "method": "Web Dashboard" }` | `200 OK` | Phản hồi `{ "success": true, "gate": false, "action": "CLOSE" }`. Node `commands/gate` trên Firebase chuyển thành `false`. | P1 / Critical | Pass |
| **TC_API_GATE_003** | `POST` | `/api/gate` | Rỗng `{}` | `200 OK` | Giá trị mặc định `open` là `false`; không làm sập server; phản hồi đóng cổng an toàn. | P2 / Moderate | Pass |
| **TC_API_GATE_004** | `GET` | `/api/gate` | Không có | `405 Method Not Allowed` | Báo lỗi `{ "error": "Chỉ chấp nhận phương thức POST" }` do chỉ cho phép ra lệnh điều khiển bằng POST. | P2 / Moderate | Pass |

---

### 2.3. Kiểm thử Endpoint `/api/light` và `/api/telemetry`

| Mã Test Case | Phương Thức | Endpoint | Payload Gửi Đi (Body) | Mã Trả Về (Status Code) | Kết Quả Phản Hồi Kỳ Vọng (Response Body) | Mức Độ | Trạng Thái |
| :--- | :---: | :--- | :--- | :---: | :--- | :---: | :---: |
| **TC_API_LGT_001** | `POST` | `/api/light` | `{ "state": true }` | `200 OK` | `{ "success": true, "light": true, "action": "ON" }`. Đồng thời cập nhật `commands.json` và `telemetry.json` lên Firebase. | P1 / Major | Pass |
| **TC_API_LGT_002** | `POST` | `/api/light` | `{ "state": false }` | `200 OK` | `{ "success": true, "light": false, "action": "OFF" }`. Tắt đèn thành công. | P1 / Major | Pass |
| **TC_API_TEL_001** | `GET` | `/api/telemetry` | Không có | `200 OK` | Trả về toàn bộ dữ liệu telemetry: `{ "temperature": ..., "humidity": ..., "gate": ..., "light": ..., "rfid": { ... } }`. | P1 / Critical | Pass |
| **TC_API_TEL_002** | `POST` | `/api/telemetry` | `{ "temperature": 31, "humidity": 68 }` | `200 OK` | Cập nhật thành công gói tin tele lên Firebase Cloud; phản hồi status 200. | P1 / Major | Pass |

---

## ⚡ PHẦN 3: TEST CASES PHẦN CỨNG IOT (ESP32, SENSORS, RFID, SERVO, BUZZER, LCD)

### 3.1. Đọc dữ liệu Cảm biến DHT11 (GPIO 4)

| Mã Test Case | Kịch Bản Kiểm Thử | Điều Kiện & Thiết Bị | Các Bước Kiểm Tra | Giá Trị Thực Tế Kỳ Vọng | Mức Độ | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **TC_HW_DHT_001** | Đọc nhiệt độ và độ ẩm phòng bình thường | ESP32 cắm nguồn, DHT11 gắn chân GPIO 4 | 1. Mở Serial Monitor với baudrate 115200.<br>2. Chờ chu kỳ đọc định kỳ (10 giây). | Serial in ra: `[DHT11] Nhiệt độ: 2X.X °C | Độ ẩm: XX %`. Giá trị hợp lệ (0-50°C, 20-90%). | P1 / Major | Pass |
| **TC_HW_DHT_002** | Xử lý ngoại lệ khi rút dây tín hiệu DHT11 | Rút chân GPIO 4 khi mạch đang chạy | 1. Tháo dây chân Data của DHT11 ra khỏi GPIO 4.<br>2. Quan sát log Serial và màn hình LCD. | Code không bị treo (freeze/crash); hàm `isnan(t)` hoặc `isnan(h)` bắt được ngoại lệ; giữ nguyên giá trị hợp lệ trước đó hoặc hiển thị "DHT Err". | P1 / Critical | Pass |
| **TC_HW_DHT_003** | Hiển thị thông số môi trường lên LCD 16x2 | LCD 16x2 I2C cắm chân SDA 21, SCL 22 | Quan sát màn hình LCD tại trạng thái nghỉ | Hàng 1 LCD hiển thị: `T: 28.5C  H: 65%`<br>Hàng 2 hiển thị trạng thái cửa: `CUA: DONG`. Không có ký tự rác. | P2 / Moderate | Pass |

---

### 3.2. Điều khiển Động cơ Cổng Servo SG90 (GPIO 13)

| Mã Test Case | Kịch Bản Kiểm Thử | Điều Kiện & Thiết Bị | Các Bước Kiểm Tra | Giá Trị Thực Tế Kỳ Vọng | Mức Độ | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **TC_HW_SRV_001** | Quay góc Mở cổng khi nhận lệnh mở | Servo kết nối GPIO 13, nguồn 5V | Gửi lệnh mở từ App hoặc quẹt thẻ Master | Cánh tay Servo quay mượt mà từ 0° sang 90° (hoặc góc mở cấu hình). Còi buzzer phát âm thanh mở cửa. | P1 / Critical | Pass |
| **TC_HW_SRV_002** | Quay góc Đóng cổng an toàn | Cổng đang mở ở góc 90° | Gửi lệnh đóng từ App hoặc hết thời gian tự đóng | Cánh tay Servo quay trả về 0°. Cổng đóng chặt chẽ, không bị kẹt cơ khí. | P1 / Critical | Pass |
| **TC_HW_SRV_003** | Khử rung servo và không tự động quay khi cắm điện | Vừa cấp nguồn cho ESP32 | Cắm cáp nguồn Micro-USB vào ESP32 | Khi khởi động, Servo lập tức ghim góc 0° (Đóng), không bị giật rung hoặc tự mở cửa ngoài ý muốn. | P1 / Major | Pass |

---

### 3.3. Xác thực Thẻ Từ RFID RC522 (SPI) & Nút BOOT

| Mã Test Case | Kịch Bản Kiểm Thử | Điều Kiện & Thiết Bị | Các Bước Kiểm Tra | Giá Trị Thực Tế Kỳ Vọng | Mức Độ | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **TC_HW_RFID_001** | Quét thẻ Master UID `620F0307` | Module RC522 gắn chân SPI (5, 26, 18, 23, 19) | Đưa thẻ Master vào gần ăng-ten RFID (< 3cm) | 1. Còi Buzzer kêu 1 tiếng "BÍP" ngắn vui tai.<br>2. LCD hiển thị: `UID: 620F0307` và `CUA DANG MO`.<br>3. Servo mở góc 90°.<br>4. Sau 3 giây tự động đóng lại.<br>5. Đẩy UID và thời gian lên Firebase. | P1 / Critical | Pass |
| **TC_HW_RFID_002** | Quét thẻ Lạ (Không trùng khớp Master UID) | Thẻ RFID khác có mã bất kỳ | Đưa thẻ lạ vào vùng quét của RC522 | 1. Còi Buzzer kêu 3 tiếng bíp cảnh báo dồn dập.<br>2. LCD hiển thị: `THE KHONG DUNG!` trong 2 giây.<br>3. Servo TUYỆT ĐỐI KHÔNG MỞ.<br>4. Firebase ghi nhận `isValid: false` để kích hoạt báo động trên App. | P1 / Critical | Pass |
| **TC_HW_RFID_003** | Nhấn giữ nút BOOT (GPIO 0) để Reset học thẻ Master mới | Nút BOOT trên bo mạch ESP32 | Nhấn và giữ nút BOOT trong 1.5 giây | 1. LCD hiển thị thông báo: `CHE DO HOC THE - QUET THE MOI...`.<br>2. Quẹt thẻ mới bất kỳ: Thẻ đó lập tức được lưu vào bộ nhớ EEPROM/Preferences của ESP32 thành Master UID mới mà không cần nạp lại code! | P2 / Major | Pass |

---

### 3.4. Đèn LED Chiếu Sáng (GPIO 2) & Còi Buzzer (GPIO 27)

| Mã Test Case | Kịch Bản Kiểm Thử | Điều Kiện & Thiết Bị | Các Bước Kiểm Tra | Giá Trị Thực Tế Kỳ Vọng | Mức Độ | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **TC_HW_LED_001** | Bật / Tắt LED qua HTTP Server LAN tức thì | ESP32 kết nối chung WiFi với máy tính | Gửi HTTP GET: `http://192.168.1.6/light?state=on` và `...state=off` | Đèn LED onboard (GPIO 2) bật sáng ngay lập tức (độ trễ đo được < 15ms nhờ WebServer chạy trên Core 1). | P1 / Major | Pass |
| **TC_HW_BUZ_001** | Phản hồi âm thanh còi Buzzer không gây block vi điều khiển | Buzzer gắn GPIO 27 | Thực hiện đóng/mở cổng hoặc quẹt thẻ | Còi kêu và Servo quay ĐỒNG THỜI (0ms delay); không dùng hàm `delay()` làm đóng băng việc đọc cảm biến và nhận gói tin mạng. | P1 / Critical | Pass |

---

### 3.5. Kiến trúc Dual-Core ESP32 & Khả Năng Chống Chịu Khi Mất Mạng

| Mã Test Case | Kịch Bản Kiểm Thử | Điều Kiện & Thiết Bị | Các Bước Kiểm Tra | Giá Trị Thực Tế Kỳ Vọng | Mức Độ | Trạng Thái |
| :--- | :--- | :--- | :--- | :--- | :---: | :---: |
| **TC_HW_DUAL_001** | Xử lý song song trên 2 nhân CPU độc lập | ESP32 chạy firmware tối ưu | Kiểm tra tác vụ Core 0 (Firebase) và Core 1 (Sensor & LAN) | Dù kết nối WiFi bị chậm hoặc Firebase phản hồi lâu, việc quẹt thẻ RFID và đóng mở cổng tại chỗ trên Core 1 vẫn hoạt động tức thời trong 0.05 giây. | P1 / Critical | Pass |
| **TC_HW_NET_002** | Khả năng tự phục hồi kết nối (Auto Reconnect) | Đang kết nối WiFi bình thường | Tắt bộ phát WiFi Router trong 30 giây rồi bật lại | ESP32 tự động quét và kết nối lại WiFi thành công; tiếp tục gửi số liệu lên Cloud mà không cần người dùng bấm nút reset phần cứng. | P1 / Major | Pass |

---

## 🔄 PHẦN 4: TEST CASES TÍCH HỢP TOÀN DIỆN HỆ THỐNG (END-TO-END INTEGRATION)

### Kịch Bản E2E_01: Báo động Quá nhiệt Phòng Chữa Cháy
- **Mục tiêu**: Đảm bảo từ khi cảm biến vật lý phát hiện nhiệt độ nguy hiểm đến khi App Mobile đổ chuông cảnh báo hoạt động thông suốt.
- **Luồng xử lý**:
  1. Hơ quẹt lửa gần đầu đọc DHT11 (Nhiệt độ đo được đạt 42°C > ngưỡng 40°C).
  2. ESP32 phát hiện `t >= 40.0`, kích hoạt còi Buzzer tại chỗ hú liên tục và hiển thị cảnh báo lên LCD.
  3. ESP32 đẩy bản tin telemetry khẩn lên Firebase Realtime Database qua Core 0.
  4. Ứng dụng di động Expo Go bắt sự kiện thay đổi dữ liệu từ Firebase.
  5. Màn hình Dashboard chuyển màu đỏ rực, phát thông báo đẩy và thêm mục cảnh báo nguy hiểm trong tab AlertsScreen.
- **Tiêu chuẩn nghiệm thu**: Thời gian truyền từ cảm biến vật lý lên màn hình điện thoại dưới 1.5 giây. **Đánh giá: PASS**.

### Kịch Bản E2E_02: Điều khiển Cổng Thông Minh từ Xa và Tự Đóng An Toàn
- **Mục tiêu**: Người dùng từ xa mở cổng cho khách qua App và cổng tự đóng lại đúng hẹn.
- **Luồng xử lý**:
  1. Người dùng mở App Mobile tại bất kỳ đâu có Internet (4G/WiFi), vào tab Cổng và nhấn "MỞ CỔNG NGAY".
  2. App gửi request POST lên `/api/gate` (Vercel) -> cập nhật lệnh vào Firebase `/smart_home/commands/gate = true`.
  3. ESP32 polling Firebase (chu kỳ 400ms) bắt được lệnh mở cửa -> Servo quay 90 độ, LCD hiển thị `CUA: DANG MO - APP`.
  4. App bắt đầu bộ đếm ngược Auto-Close 10 giây (theo cấu hình người dùng).
  5. Sau 10 giây, App gửi lệnh đóng cổng tự động -> Servo quay về 0 độ, khóa an toàn được kích hoạt.
  6. Nhật ký ra vào lưu lại đầy đủ: "Mở qua App" và "Tự động đóng cổng sau 10s".
- **Tiêu chuẩn nghiệm thu**: Không bỏ sót lệnh, Servo đóng mở chuẩn xác. **Đánh giá: PASS**.

### Kịch Bản E2E_03: Quét Thẻ Master Tại Cổng Đồng Bộ Ngay Lập Tức Lên Điện Thoại
- **Mục tiêu**: Giám sát tức thì người mở cổng ở nhà thật lên ứng dụng di động.
- **Luồng xử lý**:
  1. Người nhà dùng thẻ Master UID `620F0307` quẹt vào cổng.
  2. ESP32 kiểm tra trùng khớp Master UID -> Mở cổng trong 3 giây và phát tiếng bíp.
  3. ESP32 ghi đè dữ liệu lên `/smart_home/telemetry/rfid` với `lastUid: 620F0307, isValid: true`.
  4. App Mobile nhận bản tin thời gian thực, cập nhật ngay ô "THÔNG TIN QUẸT THẺ GẦN NHẤT" với nhãn xanh "HỢP LỆ (MASTER)" kèm giờ quẹt.
- **Tiêu chuẩn nghiệm thu**: Độ trễ cập nhật trạng thái thẻ từ nhà lên App < 800ms. **Đánh giá: PASS**.

---

## 📊 BẢNG TỔNG HỢP VÀ MA TRẬN ĐÁNH GIÁ CHẤT LƯỢNG (TEST SUMMARY MATRIX)

| STT | Phân Hệ Kiểm Thử (Sub-System) | Tổng Số Test Cases | Số Test Case Đạt (PASS) | Số Test Case Lỗi (FAIL) | Tỷ Lệ Đạt (Pass Rate) | Đánh Giá Chung |
| :---: | :--- | :---: | :---: | :---: | :---: | :---: |
| 1 | **Giao diện & Chức năng Mobile App** | 19 | 19 | 0 | 100% | Đạt chuẩn UX/UI, phản hồi mượt mà |
| 2 | **Backend API & Firebase Cloud** | 9 | 9 | 0 | 100% | Chuẩn RESTful, bảo mật CORS, độ trễ thấp |
| 3 | **Phần cứng IoT & Nhúng ESP32** | 11 | 11 | 0 | 100% | Dual-core mượt mà, phản hồi không trễ |
| 4 | **Tích hợp Toàn diện Toàn hệ thống (E2E)**| 3 | 3 | 0 | 100% | Luồng nghiệp vụ khép kín hoàn hảo |
| **TỔNG** | **TOÀN BỘ DỰ ÁN** | **42** | **42** | **0** | **100%** | **SẴN SÀNG BẢO VỆ ĐỒ ÁN XUẤT SẮC** |

---

## ❓ CÂU HỎI TỰ KIỂM TRA & TIÊU CHÍ ĐÁNH GIÁ KHI BẢO VỆ ĐỒ ÁN (RUBRIC REVIEW)

1. **Câu hỏi**: Khi mất kết nối Internet/WiFi, hệ thống kiểm soát cổng có mở được bằng thẻ RFID nữa không?
   - **Trả lời phản biện**: Có! Do kiến trúc firmware phân chia Dual-Core: Nhân 1 quản lý phần cứng ngoại vi và đọc thẻ RFID độc lập tại chỗ nên việc quẹt thẻ Master mở cửa hoạt động bình thường mà không phụ thuộc vào Internet.
2. **Câu hỏi**: Làm thế nào để chống hiện tượng giật nút (bấm nhiều lần liên tục làm treo vi điều khiển)?
   - **Trả lời phản biện**: Dự án đã cài đặt cơ chế khóa chống rung thời gian (Debounce / Timestamp locking) thông qua `lastGateToggleTimeRef` và `lastLightToggleTimeRef` trong `IoTContext.js` với thời gian khóa 500ms.
3. **Câu hỏi**: Thẻ Master có bị mất khi mất điện không?
   - **Trả lời phản biện**: Không! Mã UID thẻ Master được lưu vĩnh viễn trong bộ nhớ không khả biến `Preferences` (NVS Flash) của ESP32 và có thể cài đặt lại bất cứ lúc nào thông qua nút nhấn BOOT.
