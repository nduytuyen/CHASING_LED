# ỨNG DỤNG IC 74HC595 VÀ IC 555 TẠO HIỆU ỨNG TRONG HỆ THỐNG CHASING LED (PIF)[cite: 2]

Báo cáo đề tài cuối khóa môn **Điện tử cơ bản (ĐTCB)** thực hiện tại **Trường Đại học Bách khoa - ĐHQG TP.HCM** (Khoa Điện – Điện tử, Bộ môn Điện tử)[cite: 2]. Dự án tập trung vào việc thiết kế một hệ thống điều khiển chuỗi LED đuổi (Chasing LED) trong thực tế thông qua việc kết hợp tần số tạo xung từ mạch dao động IC 555 và chức năng dịch/chốt dữ liệu của IC 74HC595[cite: 2].

---

## 👥 Thành viên thực hiện
* **Nguyễn Duy Tuyên** – MSSV: 2213821[cite: 2]
* **Võ Thái Toàn** – MSSV: 2213543[cite: 2]
* **Thời gian báo cáo**: 21 tháng 11 năm 2024[cite: 2]

---

## ⚙️ Nguyên lý hoạt động hệ thống
Hệ thống tận dụng khả năng cung cấp xung CLOCK dao động liên tục của IC NE555 nhằm kích thích quá trình dịch và chốt dữ liệu liên tục của IC 74HC595[cite: 2]. 
* Ban đầu, tín hiệu mức cao (mức 1) được giữ ổn định tại chân 14 (DS) của IC 74HC595 khiến chuỗi LED sáng dần lên[cite: 2].
* Sau khi LED cuối cùng trong chuỗi được làm sáng, mạch tự động đảo trạng thái dữ liệu đầu vào để tiến hành tắt dần các LED[cite: 2].
* Quá trình nạp xả luân phiên của tụ điện $C$ giữa hai ngưỡng điện áp chuẩn là $1/3 V_{CC}$ và $2/3 V_{CC}$ sẽ kích hoạt các bộ so sánh bên trong IC 555, từ đó tự động đảo trạng thái điện áp ngõ ra (chân 3) tạo thành chuỗi xung vuông liên tục[cite: 1].

---

## 📊 Thông số kỹ thuật mạch dao động IC 555
Thời gian BẬT ($T_{HIGH}$) và TẮT ($T_{LOW}$) của xung vuông ngõ ra hoàn toàn phụ thuộc vào các giá trị linh kiện ngoại vi được lựa chọn thích hợp ($R_A$, $R_B$ và $C$) theo các công thức sau[cite: 1]:

* **Thời gian ngõ ra ở mức cao ($T_{HIGH}$)** (thời gian tụ sạc từ $1/3 V_{CC}$ lên $2/3 V_{CC}$):
  $$T_{HIGH} = 0.693 \cdot (R_A + R_B) \cdot C$$[cite: 1]
* **Thời gian ngõ ra ở mức thấp ($T_{LOW}$)** (thời gian tụ xả từ $2/3 V_{CC}$ xuống $1/3 V_{CC}$):
  $$T_{LOW} = 0.693 \cdot R_B \cdot C$$[cite: 1]
* **Tổng chu kỳ dao động ($T$)**:
  $$T = T_{HIGH} + T_{LOW} = 0.693 \cdot (R_A + 2R_B) \cdot C$$[cite: 1]
* **Tần số dao động ($f$)**:
  $$f = \frac{1}{T} = \frac{1.44}{(R_A + 2R_B) \cdot C}$$[cite: 1]
* **Phần trăm chu kỳ nhiệm vụ (Duty Cycle $D$)**:
  $$D = \frac{T_{HIGH}}{T} \cdot 100\% = \frac{R_A + R_B}{R_A + 2R_B} \cdot 100\%$$[cite: 1]

> *Lưu ý*: Phương trình chỉ ra rằng tần số dao động hoàn toàn không phụ thuộc vào nguồn áp nguồn $V_{CC}$[cite: 1]. Không nên rút ngắn giá trị $R_A = 0$ ohm vì có nguy cơ làm hỏng transistor xả (chân 7) bên trong IC[cite: 1].

---

## 🛠️ Cấu trúc các khối chức năng

### 1. Khối nguồn (Mạch ổn áp 5V)
* Sử dụng IC ổn áp chính **LM7805** nhận nguồn vào từ Pin 12V (BAT1) hoặc nguồn 9VDC qua giắc nối J2, hạ áp ổn định xuống mức 5V ở ngõ ra J1 để nuôi toàn bộ mạch logic[cite: 1, 2].
* Tụ hóa C1/C3 (1000uF) đóng vai trò bù áp tạm thời cho mạch khi tải tiêu thụ hoặc nguồn vào sụt áp đột ngột[cite: 1, 2].
* Tụ gốm C2/C4 (104) với trở kháng lớn hỗ trợ ngăn chặn tăng áp đột ngột và lọc bỏ các nhiễu răng cưa không mong muốn[cite: 1, 2].

### 2. Khối hiển thị LED (Gồm 2 chế độ)
Hệ thống được thiết kế linh hoạt phân tách làm hai phân vùng hoạt động hiệu ứng[cite: 2]:
* **MODE 1**: Điều khiển các cụm LED phối hợp định hình hiệu ứng phức tạp thông qua việc liên kết các IC dịch[cite: 2].
* **MODE 2**: Chạy chuỗi LED đuổi nối tiếp mở rộng thông qua kết nối tầng các chân dịch dữ liệu nối tiếp[cite: 2].

---

## 🔌 Đặc tính linh kiện chính

| Linh kiện | Loại đóng gói / Đặc tính chính | Điện áp hoạt động | Dòng tải / Tiêu thụ |
| :--- | :--- | :--- | :--- |
| **IC NE555DTR**[cite: 2] | SOIC-8 (Gắn bề mặt), tạo dao động/mạch đa hài[cite: 2] | 4.5V đến 16V[cite: 2] | Tiêu thụ tối ưu, hoạt động từ -40°C đến +85°C[cite: 2] |
| **IC 74HC595D**[cite: 2] | SOIC-16/TSSOP-16, thanh ghi dịch 8-bit chốt[cite: 2] | 2V đến 6V[cite: 2] | Tải đầu ra ±6 mA mỗi cổng, tần số tối đa 100 MHz[cite: 2] |
| **IC LM7805**[cite: 1] | IC ổn áp tuyến tính tích hợp[cite: 1] | Ngõ vào > 7V | Ngõ ra cố định chuẩn 5V cấp cho tải[cite: 1] |

---

## ⚡ Tính toán công suất hệ thống
Bảng tổng hợp công suất tiêu thụ định mức tối đa của các linh kiện trên bo mạch[cite: 2]:

* **Khối IC NE555**: $600\text{ mW} \times 2 = 1.2\text{ W}$[cite: 2]
* **Khối IC 74HC595**: $500\text{ mW} \times 6 = 3.0\text{ W}$[cite: 2]
* **Hệ thống LED (24 bóng)**: $0.066\text{ W} \times 24 = 1.6\text{ W}$[cite: 2]
* **Tổng công suất tiêu thụ toàn mạch**: **5.8 W**[cite: 2]

---

## 📂 Tài nguyên dự án
* **Sơ đồ nguyên lý chi tiết & Video Demo sản phẩm**: Bạn có thể truy cập toàn bộ dữ liệu thiết kế mạch và video chạy thực tế tại [Đường dẫn Google Drive Dự Án](https://drive.google.com/drive/folders/1eTwyBuF4C0idJWcl-e5601-npBkVAwrY?usp=drive_link)[cite: 2].
