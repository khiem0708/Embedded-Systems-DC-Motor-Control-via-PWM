
# ⚙️ Điều Khiển Động Cơ DC Dùng PWM

## 📝 Giới Thiệu

Đề tài xây dựng một hệ thống điều khiển tốc độ và chiều quay của **động cơ DC** bằng phương pháp **PWM (Pulse Width Modulation)**, sử dụng vi điều khiển **STM32F103C8T6**, **driver L298**, và **cảm biến encoder**. Hệ thống cho phép điều khiển theo thời gian thực và giám sát qua UART với giao diện như **Hercules**.

## 🔧 Thành Phần Phần Cứng

| Thiết bị | Mô tả |
|---------|-------|
| STM32F103C8T6 (Blue Pill) | Vi điều khiển 32-bit, tạo xung PWM, đọc encoder, giao tiếp UART |
| L298 Motor Driver | Mạch cầu H điều khiển chiều quay và tốc độ động cơ |
| DC Motor + Encoder | Động cơ có tích hợp encoder 2 kênh AB |
| Biến trở / Giao diện UART | Điều chỉnh tốc độ theo thời gian thực |
| Breadboard, nguồn, dây cắm,… | Phụ kiện lắp ráp và thử nghiệm |

## 📐 Đặc Tả Hệ Thống

- **Tín hiệu vào**: PWM điều khiển tốc độ, tín hiệu logic đảo chiều, xung encoder phản hồi
- **Tín hiệu ra**: Tốc độ hiển thị qua UART (Hercules)
- **Tần số PWM**: 16kHz
- **Sai số tốc độ**: ±5% RPM
- **Giao tiếp UART**: Hiển thị tốc độ và trạng thái theo thời gian thực

## 🧠 Chức Năng

- Điều khiển chiều quay: Thuận/Nghịch
- Điều chỉnh tốc độ từ 0–100%
- Phản hồi từ encoder để hiệu chỉnh chính xác
- Giao tiếp UART để giám sát tốc độ động cơ
- Tích hợp điều khiển phản hồi PID

## 📊 Sơ Đồ Hệ Thống

```
[STM32 PWM] → [L298] → [DC Motor]
   ↑                            ↓
[Encoder] ← [STM32 Timer] ← UART Display (Hercules)
```

## 🧮 Tính Toán Cơ Bản

- **Công suất động cơ**: `P = 6V x 0.16A = 0.96W`
- **Số xung/vòng**: 270 xung/vòng (2 kênh encoder)
- **Tốc độ encoder**: 1350 xung/giây
- **Điện áp hoạt động**: STM32 (3.3V – 5V), Động cơ (6V – 12V)

## 💾 Cấu Trúc Phần Mềm

### Khởi tạo:
- Cấu hình GPIO, Timer PWM
- Cấu hình encoder interface và UART
- Khởi tạo các tham số PID

### Vòng lặp chính:
- Đọc xung từ encoder
- Tính vận tốc thực tế
- Tính sai số (error)
- Tính toán điều khiển PID
- Cập nhật giá trị PWM
- Gửi dữ liệu tốc độ qua UART

## 📌 Kết Quả Thử Nghiệm

- ✅ Điều khiển tốc độ động cơ mượt, ổn định
- ✅ Điều khiển chiều quay chính xác, không giật
- ✅ Giao tiếp UART phản hồi nhanh (<100ms)
- ⚠️ Một số vấn đề cần tối ưu thêm:
  - PID chưa hoàn chỉnh
  - LCD chưa hiển thị được dữ liệu

## 🧪 Kỹ Thuật & Công Nghệ

- Vi điều khiển: `STM32F103C8T6`
- Driver cầu H: `L298`
- Mạch in: thiết kế PCB + breadboard thử nghiệm
- Phần mềm: `STM32CubeMX`, `Keil uVision`, `Hercules`, `UART Serial Monitor`

## 📆 Thông Tin Dự Án

- **Lớp**: Thiết kế hệ thống nhúng – L05
- **Nhóm**: 13
- **Giáo viên hướng dẫn**: Thầy Trương Quang Vinh
- **Thành viên nhóm**:
  - Lê Hoàng An
  - Cù Quốc Cường
  - Lê Tấn Được
  - Tạ Duy Khiêm
  - Nguyễn Huỳnh Hoàng Thống
- **Thời gian thực hiện**: 30/09/2024 – 30/11/2024
- **Kinh phí**: ~400,000 VNĐ

## 📚 Tài Liệu Tham Khảo

1. [STM32F103C8T6 Datasheet](https://www.datasheet4u.com/datasheet-pdf/STMicroelectronics/STM32F103C8T6/pdf.php?id=1480335)  
2. [L298 H-Bridge Datasheet](https://www.sparkfun.com/datasheets/Robotics/L298_H_Bridge.pdf)  
3. Tài liệu bài giảng STM32 & HAL Library
