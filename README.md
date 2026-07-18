# ỨNG DỤNG IC 74HC595 VÀ IC 555 TẠO HIỆU ỨNG TRONG HỆ THỐNG CHASING LED (PIF)

Báo cáo đề tài cuối khóa môn **Điện tử cơ bản (ĐTCB)** thực hiện tại **Trường Đại học Bách khoa - ĐHQG TP.HCM** (Khoa Điện – Điện tử, Bộ môn Điện tử). Dự án tập trung vào việc thiết kế một hệ thống điều khiển chuỗi LED đuổi (Chasing LED) trong thực tế thông qua việc kết hợp tần số tạo xung từ mạch dao động IC 555 và chức năng dịch/chốt dữ liệu của IC 74HC595.

---

## 👥 Thành viên thực hiện
* **Nguyễn Duy Tuyên**
* **Võ Thái Toàn**
* **Thời gian báo cáo**: 21 tháng 11 năm 2024

---

## ⚙️ Nguyên lý hoạt động hệ thống
Hệ thống tận dụng khả năng cung cấp xung CLOCK dao động liên tục của IC NE555 nhằm kích thích quá trình dịch và chốt dữ liệu liên tục của IC 74HC595. 
* Ban đầu, tín hiệu mức cao (mức 1) được giữ ổn định tại chân 14 (DS) của IC 74HC595 khiến chuỗi LED sáng dần lên.
* Sau khi LED cuối cùng trong chuỗi được làm sáng, mạch tự động đảo trạng thái dữ liệu đầu vào để tiến hành tắt dần các LED.
* Quá trình nạp xả luân phiên của tụ điện $C$ giữa hai ngưỡng điện áp chuẩn là $1/3 V_{CC}$ và $2/3 V_{CC}$ sẽ kích hoạt các bộ so sánh bên trong IC 555, từ đó tự động đảo trạng thái điện áp ngõ ra (chân 3) tạo thành chuỗi xung vuông liên tục.

---

## 📊 Thông số kỹ thuật mạch dao động IC 555
Thời gian BẬT ($T_{HIGH}$) và TẮT ($T_{LOW}$) của xung vuông ngõ ra hoàn toàn phụ thuộc vào các giá trị linh kiện ngoại vi được lựa chọn thích hợp ($R_A$, $R_B$ và $C$) theo các công thức sau:

* **Thời gian ngõ ra ở mức cao ($T_{HIGH}$)** (thời gian tụ sạc từ $1/3 V_{CC}$ lên $2/3 V_{CC}$):
  $$T_{HIGH} = 0.693 \cdot (R_A + R_B) \cdot C$$
* **Thời gian ngõ ra ở mức thấp ($T_{LOW}$)** (thời gian tụ xả từ $2/3 V_{CC}$ xuống $1/3 V_{CC}$):
  $$T_{LOW} = 0.693 \cdot R_B \cdot C$$
* **Tổng chu kỳ dao động ($T$)**:
  $$T = T_{HIGH} + T_{LOW} = 0.693 \cdot (R_A + 2R_B) \cdot C$$
* **Tần số dao động ($f$)**:
  $$f = \frac{1}{T} = \frac{1.44}{(R_A + 2R_B) \cdot C}$$
* **Phần trăm chu kỳ nhiệm vụ (Duty Cycle $D$)**:
  $$D = \frac{T_{HIGH}}{T} \cdot 100\% = \frac{R_A + R_B}{R_A + 2R_B} \cdot 100\%$$

> *Lưu ý*: Phương trình chỉ ra rằng tần số dao động hoàn toàn không phụ thuộc vào nguồn áp nguồn $V_{CC}$. Không nên rút ngắn giá trị $R_A = 0$ ohm vì có nguy cơ làm hỏng transistor xả (chân 7) bên trong IC.

---

## 🛠️ Cấu trúc các khối chức năng

### 1. Khối nguồn (Mạch ổn áp 5V)
* Sử dụng IC ổn áp chính **LM7805** nhận nguồn vào từ Pin 12V (BAT1) hoặc nguồn 9VDC qua giắc nối J2, hạ áp ổn định xuống mức 5V ở ngõ ra J1 để nuôi toàn bộ mạch logic.
* Tụ hóa C1/C3 (1000uF) đóng vai trò bù áp tạm thời cho mạch khi tải tiêu thụ hoặc nguồn vào sụt áp đột ngột.
* Tụ gốm C2/C4 (104) với trở kháng lớn hỗ trợ ngăn chặn tăng áp đột ngột và lọc bỏ các nhiễu răng cưa không mong muốn.

### 2. Khối hiển thị LED (Gồm 2 chế độ)
Hệ thống được thiết kế linh hoạt phân tách làm hai phân vùng hoạt động hiệu ứng:
* **MODE 1**: Điều khiển các cụm LED phối hợp định hình hiệu ứng phức tạp thông qua việc liên kết các IC dịch.
* **MODE 2**: Chạy chuỗi LED đuổi nối tiếp mở rộng thông qua kết nối tầng các chân dịch dữ liệu nối tiếp.

---

## 🔌 Đặc tính linh kiện chính

| Linh kiện | Loại đóng gói / Đặc tính chính | Điện áp hoạt động | Dòng tải / Tiêu thụ |
| :--- | :--- | :--- | :--- |
| **IC NE555DTR** | SOIC-8 (Gắn bề mặt), tạo dao động/mạch đa hài | 4.5V đến 16V | Tiêu thụ tối ưu, hoạt động từ -40°C đến +85°C |
| **IC 74HC595D** | SOIC-16/TSSOP-16, thanh ghi dịch 8-bit chốt | 2V đến 6V | Tải đầu ra ±6 mA mỗi cổng, tần số tối đa 100 MHz |
| **IC LM7805** | IC ổn áp tuyến tính tích hợp | Ngõ vào > 7V | Ngõ ra cố định chuẩn 5V cấp cho tải |

---

## ⚡ Tính toán công suất hệ thống
Bảng tổng hợp công suất tiêu thụ định mức tối đa của các linh kiện trên bo mạch:

* **Khối IC NE555**: $600\text{ mW} \times 2 = 1.2\text{ W}$
* **Khối IC 74HC595**: $500\text{ mW} \times 6 = 3.0\text{ W}$
* **Hệ thống LED (24 bóng)**: $0.066\text{ W} \times 24 = 1.6\text{ W}$
* **Tổng công suất tiêu thụ toàn mạch**: **5.8 W**


