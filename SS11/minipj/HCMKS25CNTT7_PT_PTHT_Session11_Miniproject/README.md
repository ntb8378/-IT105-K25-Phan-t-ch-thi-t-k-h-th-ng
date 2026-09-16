# [Mini Project] Báo Cáo Phân Tích & Thiết Kế Hệ Thống Quản Lý Phòng Khám Nha Khoa DentCare

---

## MỤC LỤC
1. [Phần I – Phân Tích Hệ Thống và Thu Thập Yêu Cầu](#phần-i--phân-tích-hệ-thống-và-thu-thập-yêu-cầu)
2. [Phần II – Mô Hình Hóa Quy Trình (Activity & Use Case Diagram)](#phần-ii--mô-hình-hóa-quy-trình-activity--use-case-diagram)
3. [Phần III – Thiết Kế Cấu Trúc Tĩnh (Class Diagram)](#phần-iii--thiết-kế-cấu-trúc-tĩnh-class-diagram)
4. [Phần IV – Mô Hình Hóa Tương Tác (Sequence Diagram)](#phần-iv--mô-hình-hóa-tương-tác-sequence-diagram)
5. [Phần V – Ràng Buộc Nghiệp Vụ & Phân Quyền (RBAC)](#phần-v--ràng-buộc-nghiệp-vụ--phân-quyền-rbac)
6. [Phần VI – Thiết Kế Hiển Thị & Báo Cáo](#phần-vi--thiết-kế-hiển-thị--báo-cáo)

---

## PHẦN I – PHÂN TÍCH HỆ THỐNG VÀ THU THẬP YÊU CẦU

### 1. Nhận Diện 5 Thành Phần Của Hệ Thống Thông Tin (HTTT)
* **Phần cứng (Hardware):**
  * Máy tính để bàn (PC) tại quầy Lễ tân để tiếp đón, đặt lịch và thanh toán.
  * Máy trạm/Tablet tại ghế nha khoa cho Nha sĩ tra cứu và nhập hồ sơ bệnh án.
  * Máy chủ cơ sở dữ liệu (Database Server) và máy in hóa đơn/phiếu hẹn.
* **Phần mềm (Software):**
  * Ứng dụng Quản lý Nha khoa DentCare (Frontend & Backend API).
  * Hệ quản trị CSDL quan hệ (RDBMS: PostgreSQL hoặc MySQL).
  * Cổng tích hợp SMS Gateway (Twilio/eSMS) gửi tin nhắn nhắc hẹn tự động.
* **Dữ liệu (Data):**
  * Danh mục bệnh nhân, lịch sử điều trị (Medical Records), lịch làm việc nha sĩ.
  * Danh mục dịch vụ, đơn giá, hóa đơn thanh toán và báo cáo doanh thu.
* **Quy trình (Process):**
  * Quy trình tiếp nhận, kiểm tra xung đột slot và đặt lịch hẹn khám.
  * Quy trình khám, chẩn đoán, ghi nhận dịch vụ điều trị.
  * Quy trình tạo hóa đơn, thu tiền (tiền mặt/chuyển khoản) và kết ca thống kê.
* **Con người (People):**
  * Bệnh nhân, Nhân viên Lễ tân, Nha sĩ, Quản lý phòng khám, Quản trị viên (Admin).

### 2. Phân Loại Hệ Thống Thông Tin
* **Phân loại:** **Hệ thống Xử lý Giao dịch (TPS - Transaction Processing System)**.
* **Giải thích:** DentCare trực tiếp ghi nhận và xử lý các giao dịch phát sinh hàng ngày ở tầng vận hành: lập lịch hẹn, ghi bệnh án, tính viện phí và xuất hóa đơn tức thời. Ngoài ra, hệ thống tích hợp phân hệ **MIS (Hệ thống Thông tin Quản lý)** phục vụ cấp Quản lý thông qua các báo cáo tổng hợp doanh thu và hiệu suất khám theo định kỳ.

### 3. Quy Trình SDLC & Lựa Chọn Mô Hình Phát Triển
* **Các bước SDLC áp dụng:**
  1. *Khảo sát & Lập kế hoạch (Planning):* Khảo sát hiện trạng dùng sổ tay, phân tích tính khả thi và ngân sách.
  2. *Phân tích yêu cầu (Analysis):* Thu thập yêu cầu từ 4 nhóm Stakeholder, xây dựng SRS, Use Case, Activity Diagram.
  3. *Thiết kế hệ thống (Design):* Thiết kế Class Diagram, Sequence Diagram, kiến trúc CSDL, UI/UX Mockup.
  4. *Hiện thực hóa / Lập trình (Implementation):* Phát triển mã nguồn, API và kết nối CSDL.
  5. *Kiểm thử (Testing):* Viết Unit Test, Integration Test, nghiệm thu quy trình nghiệp vụ (UAT).
  6. *Triển khai & Bảo trì (Deployment & Maintenance):* Chuyển đổi dữ liệu cũ, đào tạo nhân viên, sao lưu dữ liệu.
* **Mô hình lựa chọn:** **Mô hình Agile / Scrum**.
  * *Lý do:* Phòng khám chuyển từ vận hành 100% thủ công sang tự động hóa, quy trình thực tế thường xuyên thay đổi khi tiếp xúc phần mềm. Chia dự án thành các Sprint ngắn 2 tuần giúp Lễ tân và Nha sĩ phản hồi sớm để tinh chỉnh giao diện nhập liệu.

### 4. Bảng Phân Tích Stakeholders & Kỹ Thuật Thu Thập Yêu Cầu

| Stakeholder | Nguồn yêu cầu | Kỹ thuật thu thập | Mục tiêu thu thập |
| :--- | :--- | :--- | :--- |
| **Bệnh nhân** | Thói quen đặt hẹn, phản hồi về thời gian chờ | Khảo sát trực tuyến (Surveys), Phân tích phản hồi khách hàng | Đặt hẹn trực quan, nhận SMS nhắc lịch, tra cứu hóa đơn rõ ràng |
| **Lễ tân** | Sổ đặt hẹn tay, biên lai thu tiền giấy | Phỏng vấn trực tiếp (Interviews), Quan sát thực tế thao tác | Thao tác tìm kiếm nhanh, kiểm tra trùng lịch tức thì, in hóa đơn nhanh |
| **Nha sĩ** | Hồ sơ bệnh án giấy, quy trình điều trị | Phỏng vấn bán cấu trúc, Bản mẫu giao diện (Prototyping) | Nhập liệu nhanh gọn khi đeo găng tay, chọn dịch vụ có sẵn |
| **Quản lý** | Sổ tổng kết thu chi, nhu cầu kiểm soát | Phỏng vấn 1-1, Phân tích biểu mẫu báo cáo cũ | Thống kê doanh thu theo kỳ, đánh giá lượt khám, quản lý bảng giá |

### 5. Yêu Cầu Kỹ Thuật (SRS)

#### A. Yêu Cầu Chức Năng (Functional Requirements)
* **FR1 (Đặt & Quản lý lịch hẹn):** Cho phép đặt lịch khám, kiểm tra xung đột khung giờ nha sĩ, đổi giờ hẹn, hủy hẹn với lý do.
* **FR2 (Khám & Ghi bệnh án):** Nha sĩ chẩn đoán, chọn dịch vụ thực hiện từ danh mục, lưu đơn thuốc và hướng dẫn tái khám.
* **FR3 (Thanh toán & Xuất hóa đơn):** Tự động tính tiền dựa trên dịch vụ đã làm, hỗ trợ tiền mặt/chuyển khoản, in hóa đơn tài chính.
* **FR4 (Báo cáo & Thống kê):** Thống kê doanh thu theo ngày/tháng/năm, đếm tổng lượt khám, xuất danh mục bảng giá dịch vụ.
* **FR5 (Quản lý Hồ sơ & Danh mục):** CRUD thông tin bệnh nhân, quản lý tài khoản nha sĩ, quản lý bảng giá dịch vụ.

#### B. Yêu Cầu Phi Chức Năng (Non-Functional Requirements)
* **NFR1 (Hiệu năng):** Thời gian phản hồi kiểm tra slot trống và tạo hóa đơn không quá **1.5 giây**.
* **NFR2 (Bảo mật):** Toàn bộ dữ liệu truyền qua HTTPS/TLS 1.3, phân quyền theo vai trò (RBAC) nghiêm ngặt.
* **NFR3 (Toàn vẹn dữ liệu):** Dữ liệu lịch sử bệnh án và hóa đơn không thể bị xóa vật lý (chỉ Soft Delete hoặc khóa chỉnh sửa sau khi thanh toán).
* **NFR4 (Khả dụng):** Hệ thống sẵn sàng hoạt động tối thiểu **99.5%** trong khung giờ khám từ 07:30 đến 21:00 hàng ngày.

### 6. User Stories
* **US01:** Là **Lễ tân**, tôi muốn **kiểm tra slot trống theo từng Nha sĩ và ngày cụ thể**, để **sắp xếp lịch khám cho bệnh nhân không bị trùng**.
* **US02:** Là **Bệnh nhân**, tôi muốn **tự hủy lịch hẹn trước giờ khám**, để **phòng khám giải phóng thời gian cho bệnh nhân khác**.
* **US03:** Là **Nha sĩ**, tôi muốn **chọn các dịch vụ đã thực hiện từ danh mục có sẵn**, để **hồ sơ bệnh án chính xác và hệ thống tự động tính tiền viện phí**.
* **US04:** Là **Lễ tân**, tôi muốn **chọn phương thức thanh toán (Tiền mặt hoặc Chuyển khoản QR)**, để **hoàn tất giao dịch và in biên lai nhanh chóng**.
* **US05:** Là **Quản lý**, tôi muốn **xem báo cáo tổng hợp doanh thu và số lượt khám theo tháng**, để **đánh giá hiệu quả kinh doanh của phòng khám**.

---

## PHẦN II – MÔ HÌNH HÓA QUY TRÌNH (ACTIVITY & USE CASE DIAGRAM)

### 1. Activity Diagram: Luồng Đặt Lịch Hẹn

<img src="./img/Activity Diagram(datlich).drawio.png">

### 2. Activity Diagram: Luồng Khám Bệnh và Thanh Toán (Swimlane)

<img src="./img/Activity Diagram(kham).drawio.png">

### 3. Use Case Diagram Tổng Thể

<img src="./img/Use Case Diagram.drawio.png">

### 4. Đặc Tả Use Case: "Đặt Lịch Hẹn"
* **Tên Use Case:** Đặt lịch hẹn (Book Appointment).
* **Actor chính:** Bệnh nhân (hoặc Lễ tân đặt thay).
* **Mô tả:** Cho phép chọn Nha sĩ, ngày hẹn và khung giờ để đăng ký khám trước.
* **Tiền điều kiện:** 
  1. Bệnh nhân đã có hồ sơ trong hệ thống.
  2. Lịch trực của Nha sĩ đã được thiết lập.
* **Luồng chính (Main Flow):**
  1. Actor truy cập chức năng "Đặt lịch hẹn".
  2. Hệ thống tải danh sách Nha sĩ và lịch trực theo ngày.
  3. Actor chọn Nha sĩ, Ngày hẹn, Khung giờ (timeSlot) và nhập triệu chứng.
  4. Actor nhấn nút "Xác nhận đặt lịch".
  5. Hệ thống kiểm tra: khung giờ đã chọn của Nha sĩ chưa có lịch nào ở trạng thái "Đã xác nhận".
  6. Hệ thống lưu bản ghi Appointment mới với trạng thái "Đã xác nhận".
  7. Kích hoạt Use Case `<<extend>>`: Hệ thống gửi SMS/Email xác nhận đến số điện thoại bệnh nhân.
  8. Hệ thống thông báo thành công và hiển thị mã cuộc hẹn.
* **Luồng thay thế (Alternative Flows):**
  * *4a. Khung giờ đã kín:* 
    * 4a1. Hệ thống báo lỗi: "Khung giờ này vừa có người đặt, vui lòng chọn giờ khác".
    * 4a2. Hệ thống tải lại danh sách các slot còn trống kề cận.
    * 4a3. Actor chọn slot mới hoặc hủy bỏ thao tác.
  * *4b. Ngày hẹn không hợp lệ (nhỏ hơn ngày hiện tại):*
    * 4b1. Hệ thống hiển thị lỗi cảnh báo: "Ngày hẹn khám phải từ hôm nay trở đi".
* **Hậu điều kiện:** Bản ghi lịch hẹn được tạo thành công trong CSDL; slot thời gian của Nha sĩ bị khóa lại, ngăn đặt trùng.

---

## PHẦN III – THIẾT KẾ CẤU TRÚC TĨNH (CLASS DIAGRAM)

### 1. Class Diagram Tổng Thể 

<img src="./img/Class Diagram.drawio.png">

### 2. Giải Thích Quan Hệ & Ràng Buộc Kiến Trúc
* **Composition:** `Invoice ◆── (1..*) InvoiceDetail`. Hóa đơn sở hữu chặt chẽ các dòng chi tiết. Nếu xóa `Invoice`, toàn bộ `InvoiceDetail` trực thuộc sẽ bị xóa theo (Cascade Delete).
* **Generalization:** Lớp cha trừu tượng `PaymentMethod` được kế thừa bởi `CashPayment` (Tiền mặt) và `TransferPayment` (Chuyển khoản QR/Ngân hàng).
* **Bảo toàn dữ liệu lịch sử:** Áp dụng khóa ngoại `ON DELETE RESTRICT` hoặc kỹ thuật `Soft Delete` (`isDeleted = true`). Khi xóa `Patient`, các bản ghi `MedicalRecord` và `Invoice` đã tạo vẫn được giữ nguyên vẹn trong hệ thống.

---

## PHẦN IV – MÔ HÌNH HÓA TƯƠNG TÁC (SEQUENCE DIAGRAM)

### 1. Sequence Diagram: Đặt Lịch Hẹn

<img src="./img/Sequence Diagram(datlich).drawio.png">

### 2. Sequence Diagram: Khám Bệnh & Tạo Hóa Đơn

<img src="./img/Sequence Diagram(khamvataohoadon).drawio.png">

### 3. Sequence Diagram: Hủy Lịch Hẹn

<img src="./img/Sequence Diagram(huylich).drawio.png">

---

## PHẦN V – RÀNG BUỘC NGHIỆP VỤ & PHÂN QUYỀN (RBAC)

### 1. Ma Trận Xác Thực Dữ Liệu (Validation Rules)

| Trường dữ liệu | Biểu thức kiểm tra (Validation Rule) | Thông báo lỗi hiển thị |
| :--- | :--- | :--- |
| **Số điện thoại** | Regex: `^0[0-9]{9}$` (đúng 10 số, bắt đầu bằng 0) | "Số điện thoại không hợp lệ, phải gồm 10 số và bắt đầu bằng số 0." |
| **Ngày sinh** | `dob <= CURRENT_DATE()` | "Ngày sinh không được lớn hơn ngày hiện tại." |
| **Ngày hẹn** | `appointmentDate >= CURRENT_DATE()` | "Ngày hẹn khám phải từ hôm nay trở đi." |
| **Đơn giá dịch vụ** | `price > 0` | "Đơn giá dịch vụ phải lớn hơn 0 VNĐ." |
| **Số lượng dịch vụ** | `quantity >= 1` (Số nguyên) | "Số lượng chỉ định dịch vụ phải tối thiểu là 1." |

### 2. Ma Trận Phân Quyền Người Dùng (RBAC Matrix)

*Ký hiệu: C (Create), R (Read), U (Update), D (Delete), X (Không có quyền)*

| Phân hệ / Chức năng | Bệnh nhân | Lễ tân | Nha sĩ | Quản lý |
| :--- | :---: | :---: | :---: | :---: |
| **Hồ sơ bệnh nhân** | R (của mình) | C, R, U | R | R |
| **Đặt / Hủy lịch hẹn** | C, R, U (của mình) | C, R, U, D | R (xem ca) | R |
| **Khám & Ghi bệnh án (MedicalRecord)** | X | X | C, R, U | R |
| **Tạo Hóa đơn & Thu tiền (Invoice)** | X | C, R, U | X | R |
| **Quản lý Danh mục Dịch vụ** | R | R | R | C, R, U, D |
| **Báo cáo Thống kê Doanh thu** | X | X | X | C, R, U, D |

---

## PHẦN VI – THIẾT KẾ HIỂN THỊ & BÁO CÁO

### 1. Báo Cáo Tồn Kho Dịch Vụ & Bảng Giá Niêm Yết

```text
========================================================================================
                      PHÒNG KHÁM NHA KHOA DENTCARE
                   DANH MỤC DỊCH VỤ VÀ BẢNG GIÁ NIÊM YẾT
========================================================================================
Mã DV    Tên Dịch Vụ Nha Khoa       Đơn Vị Tính   Đơn Giá (VNĐ)    Trạng Thái Áp Dụng
----------------------------------------------------------------------------------------
DV001    Khám & Tư vấn tổng quát     Lượt                 0        Đang cung cấp
DV002    Cạo vôi răng & Đánh bóng    2 hàm          250,000        Đang cung cấp
DV003    Trám răng Composite         1 răng         350,000        Đang cung cấp
DV004    Nhổ răng khôn (hàm dưới)    1 răng       1,500,000        Đang cung cấp
DV005    Tẩy trắng răng tại phòng    Liệu trình   1,800,000        Đang cung cấp
DV006    Bọc răng sứ Zirconia        1 răng       3,200,000        Đang cung cấp
========================================================================================
Tổng số dịch vụ hoạt động: 06 dịch vụ
```

### 2. Mẫu Báo Cáo Doanh Thu Theo Kỳ (Tháng)

```text
========================================================================================
                      BÁO CÁO DOANH THU & HOẠT ĐỘNG THÁNG
Kỳ báo cáo: Tháng 09/2026                               Người lập: Quản lý phòng khám
Ngày xuất báo cáo: 16/09/2026
----------------------------------------------------------------------------------------
Ngày       Số lượt khám    Tiền mặt (VNĐ)   Chuyển khoản (VNĐ)     Tổng cộng (VNĐ)
----------------------------------------------------------------------------------------
14/09/2026      18           3,500,000          8,200,000            11,700,000
15/09/2026      22           4,800,000         12,500,000            17,300,000
16/09/2026      15           2,100,000          7,900,000            10,000,000
----------------------------------------------------------------------------------------
TỔNG KỲ:        55 Lượt     10,400,000         28,600,000            39,000,000 VNĐ
========================================================================================
```

### 3. Giao Diện Tra Cứu Hồ Sơ Bệnh Nhân

```text
+--------------------------------------------------------------------------------------+
| [Tra cứu Bệnh nhân] Nhập Mã/SĐT: [ 0912345678 ]            [ TÌM KIẾM ]              |
+--------------------------------------------------------------------------------------+
| THÔNG TIN BỆNH NHÂN:                                                                 |
| Họ tên: NGUYỄN VĂN AN      | Ngày sinh: 15/05/1995       | Giới tính: Nam            |
| Mã BN: BN1029              | Điện thoại: 0912345678      | Số lần đến khám: 02       |
+--------------------------------------------------------------------------------------+
| LỊCH SỬ KHÁM BỆNH & HÓA ĐƠN LIÊN QUAN:                                               |
|                                                                                      |
| [Lần 1] Ngày: 10/08/2026 - Nha sĩ điều trị: BS. Lê Hoàng                             |
|  - Chẩn đoán: Viêm lợi nhẹ, nhiều vôi răng mặt trong.                                |
|  - Dịch vụ: Cạo vôi răng (x1) - 250,000 VNĐ                                          |
|  - Hóa đơn: HD00842 | Số tiền: 250,000 VNĐ | Trạng thái: Đã thanh toán (Tiền mặt)    |
| ------------------------------------------------------------------------------------ |
| [Lần 2] Ngày: 16/09/2026 - Nha sĩ điều trị: BS. Trần Minh                            |
|  - Chẩn đoán: Sâu men răng số 46.                                                    |
|  - Dịch vụ: Trám răng Composite (x1) - 350,000 VNĐ                                   |
|  - Hóa đơn: HD00915 | Số tiền: 350,000 VNĐ | Trạng thái: Đã thanh toán (Chuyển khoản)|
+--------------------------------------------------------------------------------------+
```