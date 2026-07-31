# 🔐 Trao đổi khóa an toàn giữa Thiết bị IoT và Server
---

## 📖 Tổng quan

Đồ án này mô phỏng quá trình trao đổi khóa an toàn giữa thiết bị IoT và máy chủ sử dụng **Elliptic Curve Diffie-Hellman (ECDH)** và cơ chế **TLS Handshake**. Sau khi trao đổi khóa, cả hai bên cùng tạo ra Session Key và sử dụng **AES-GCM** để mã hóa dữ liệu cảm biến trước khi truyền qua mạng.

Đồ án được thực hiện trong khuôn khổ học phần **Bảo mật IoT** tại Trường Đại học Văn Hiến.

---

## 🎯 Mục tiêu

- ✅ Tìm hiểu và mô phỏng TLS Handshake
- ✅ Triển khai trao đổi khóa ECDH trên đường cong SECP256R1
- ✅ Tạo Shared Secret và dẫn xuất Session Key bằng HKDF
- ✅ Mã hóa và giải mã dữ liệu cảm biến bằng AES-GCM
- ✅ Xác thực Server bằng fingerprint SHA-256
- ✅ Phân tích rủi ro bảo mật (MITM, Static Keys)

---

## 🏗️ Kiến trúc hệ thống

![Kiến trúc hệ thống](results/images/system_architecture.png)

*Hình 1: Kiến trúc tổng thể hệ thống trao đổi khóa an toàn*

### Các thành phần chính:

| **Thành phần** | **Chức năng** |
|----------------|---------------|
| **Thiết bị IoT (Client)** | Thu thập dữ liệu cảm biến, tạo cặp khóa ECC, trao đổi khóa, mã hóa dữ liệu |
| **Máy chủ (Server)** | Xác thực kết nối, trao đổi khóa, giải mã và xử lý dữ liệu |
| **TLS Handshake** | Thiết lập kết nối bảo mật giữa hai bên |
| **ECDH** | Tạo Shared Secret từ cặp khóa công khai và khóa riêng |
| **HKDF** | Dẫn xuất Session Key từ Shared Secret |
| **AES-GCM** | Mã hóa/giải mã dữ liệu và xác thực toàn vẹn |

---

## 🔄 Quy trình TLS Handshake

![Quy trình TLS Handshake](results/images/tls_handshake.png)

*Hình 2: Quy trình TLS Handshake mô phỏng*

### Các bước thực hiện:

1. **Client Hello** → IoT gửi yêu cầu kết nối + Public Key
2. **Server Hello** → Server phản hồi với Public Key + Fingerprint
3. **Xác thực Server** → Client kiểm tra fingerprint
4. **Tính Shared Secret** → Cả hai bên tính ECDH
5. **Sinh Session Key** → HKDF tạo Session Key 256-bit
6. **Mã hóa dữ liệu** → Client mã hóa dữ liệu bằng AES-GCM
7. **Giải mã & xác thực** → Server giải mã và kiểm tra toàn vẹn

---

## 🔑 Quá trình ECDH

![Quá trình ECDH](results/images/ecdh_process.png)

*Hình 3: Quá trình trao đổi khóa ECDH*

### Công thức toán học:
Client: Server:
Private Key (a) Private Key (b)
Public Key (A = a × G) Public Key (B = b × G)

↓ Trao đổi Public Key ↓

Shared Secret = a × B Shared Secret = b × A
↓ ↓
Shared Secret Giống Nhau! ←─────────→
↓ ↓
HKDF Derivation HKDF Derivation
↓ ↓
Session Key ←────── Giống ──────→ Session Key

text

---

## 🖥️ Giao diện chương trình

![Giao diện chính](results/images/gui/gui_main.png)

*Hình 4: Giao diện chương trình mô phỏng*

### Tính năng:

| **Khu vực** | **Chức năng** |
|-------------|---------------|
| **Checkbox điều khiển** | Bật/tắt Xác thực Server, Static Keys, MITM |
| **Nút bấm** | Khởi động Server, Client, Demo toàn bộ, Xóa log |
| **Log Server** | Hiển thị nhật ký hoạt động của Server |
| **Log Client** | Hiển thị nhật ký hoạt động của Client |

---

## 📁 Cấu trúc dự án
secure-key-exchange-iot-server/
│
├── 📁 src/ # Mã nguồn Python
│ ├── client.py # Thiết bị IoT (Client)
│ ├── server.py # Máy chủ (Server)
│ ├── crypto_utils.py # Tiện ích mật mã
│ ├── config.py # Cấu hình hệ thống
│ ├── gui.py # Giao diện Tkinter
│ └── iot_tls_demo.py # Entry point
│
├── 📁 results/ # Kết quả và hình ảnh
│ ├── images/ # Sơ đồ hệ thống
│ │ ├── system_architecture.png
│ │ ├── tls_handshake.png
│ │ ├── ecdh_process.png
│ │ └── gui/ # Ảnh chụp giao diện
│ ├── demo_output.txt # Log demo
│ └── session_keys.log # Log sinh khóa
│
├── 📁 reports/ # Tài liệu báo cáo
│ ├── report.docx # Báo cáo Word
│ └── report.pdf # Báo cáo PDF
│
├── 📁 references/ # Tài liệu tham khảo
│ └── references.md # Danh mục tham khảo
│
├── 📄 README.md # File này
├── 📄 LICENSE # Giấy phép MIT
└── 📄 .gitignore # Quy tắc bỏ qua file

text

---

## 🚀 Hướng dẫn cài đặt và chạy demo

### Yêu cầu hệ thống

- **Python** 3.11 trở lên
- **pip** (Python package manager)
- **Hệ điều hành:** Windows / Linux / macOS

### Bước 1: Cài đặt Python

1. Tải Python từ [python.org](https://www.python.org/downloads/)
2. Trong quá trình cài đặt, chọn **"Add Python to PATH"**
3. Kiểm tra cài đặt:
   ```bash
   python --version
Bước 2: Tải mã nguồn
bash
# Clone repository từ GitHub
git clone https://github.com/thach-05/40-secure-key-exchange-iot-server.git

# Di chuyển vào thư mục dự án
cd 40-secure-key-exchange-iot-server
Bước 3: Cài đặt thư viện
bash
# Cài đặt thư viện cryptography
pip install cryptography
Bước 4: Chạy chương trình
bash
# Di chuyển vào thư mục src
cd src

# Chạy chương trình
python iot_tls_demo.py
Bước 5: Sử dụng giao diện
5.1 Chọn chế độ kiểm thử (tích checkbox):
Checkbox	Mô tả
☑ Xác thực Server	Bật/tắt kiểm tra fingerprint server
☑ Dùng khóa tĩnh	Bật/tắt chế độ khóa cố định (Static Keys)
☑ Giả lập MITM	Bật/tắt chế độ server giả mạo
5.2 Chạy demo:
"Khởi động Server" → Server bắt đầu lắng nghe kết nối

"Khởi động Client" → Client kết nối và thực hiện handshake

"Chạy Demo Toàn Bộ" → Tự động hóa toàn bộ quy trình

"Xóa Log" → Xóa log trên cả hai khung

5.3 Quan sát kết quả:
Log Server (khung trái): Hiển thị hoạt động của máy chủ

Log Client (khung phải): Hiển thị hoạt động của thiết bị IoT

🧪 Các kịch bản demo
ID	Kịch bản	Checkbox	Kết quả mong đợi
TC-01	Xác thực Server thành công	☑ Xác thực Server	Log "Xác thực THÀNH CÔNG"
TC-02	Xác thực Server thất bại (MITM)	☑ Xác thực Server + ☑ MITM	Log "Xác thực THẤT BẠI"
TC-03	Bỏ qua xác thực Server	☐ Xác thực Server + ☑ MITM	Log "Bỏ qua xác thực"
TC-04	Sử dụng khóa tĩnh (Static Keys)	☑ Xác thực Server + ☑ Khóa tĩnh	Log "Sử dụng khóa tĩnh"
TC-05	End-to-End với dữ liệu cảm biến	☑ Xác thực Server	JSON được mã hóa/giải mã
Ảnh minh chứng:
Kịch bản	Hình ảnh
Xác thực thành công	https://results/images/gui/handshake_success.png
Xác thực thất bại (MITM)	https://results/images/gui/handshake_fail.png
End-to-End dữ liệu	https://results/images/gui/data_decrypted.png
🛠️ Công nghệ sử dụng
Công nghệ	Phiên bản	Mục đích
Python	3.11	Ngôn ngữ lập trình
Cryptography	Latest	ECDH, HKDF, AES-GCM
Tkinter	Built-in	Giao diện đồ họa
Socket	Built-in	Kết nối mạng
Git	Latest	Quản lý phiên bản
Draw.io	Online	Vẽ sơ đồ hệ thống
📚 Tài liệu tham khảo
STT	Nguồn	Mô tả
1	Mbed TLS	Tham khảo TLS Handshake và ECDH
2	OWASP ISVS	Tiêu chuẩn bảo mật IoT
3	RFC 8446 - TLS 1.3	Giao thức TLS 1.3
4	RFC 5869 - HKDF	Hàm dẫn xuất khóa HKDF
5	Cryptography.io	Thư viện mật mã Python
6	Stallings (2020)	Sách Cryptography and Network Security
👨‍💻 Tác giả
Thông tin	Chi tiết
Họ tên	Huỳnh Hồng Ngọc Thạch
MSSV	231A010845
Học phần	Bảo mật IoT (INT4410)
Lớp	INT441001
Giảng viên	Thầy Hồ Nhựt Minh
Trường	Đại học Văn Hiến
Năm học	2025-2026
📄 Giấy phép
Dự án này được cấp phép theo MIT License - xem file LICENSE để biết thêm chi tiết.

🙏 Lời cảm ơn
Xin chân thành cảm ơn:

Thầy Hồ Nhựt Minh đã hướng dẫn và hỗ trợ trong suốt quá trình thực hiện đồ án

Khoa Công Nghệ - Thông Tin đã tạo điều kiện học tập và nghiên cứu

Các nguồn tham khảo đã cung cấp kiến thức quý báu

📞 Liên hệ
GitHub: thach-05

Email: [Email của bạn]

Trường: Đại học Văn Hiến