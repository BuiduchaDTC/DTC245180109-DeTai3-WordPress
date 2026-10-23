# Đề tài 3: Website Quảng bá Sản phẩm (WordPress) - Docker

- **Họ và tên:** Bùi Đức Hà
- **Mã sinh viên:** DTC245180109
- **Môn học:** Triển khai và Quản trị Hệ thống Phần mềm

---

## 🏗️ Tổng quan hệ thống
Hệ thống được triển khai hoàn toàn bằng **Docker Compose** với **9 container** hoạt động đồng bộ:
1. **`wp_mysql_db`**: Cơ sở dữ liệu MySQL 8.0 (đã phân quyền bảo mật qua script khởi tạo).
2. **`wp_app_web`**: Website giới thiệu sản phẩm sử dụng WordPress (LOHA SALES).
3. **`wp_phpmyadmin`**: Công cụ quản lý cơ sở dữ liệu qua giao diện web.
4. **`wp_nginx_proxy`**: Nginx làm Reverse Proxy, tích hợp chứng chỉ SSL/TLS tự ký (HTTPS) và HTTP Security Headers.
5. **`wp_cadvisor`**: Giám sát thông số tài nguyên của container.
6. **`wp_prometheus`**: Thu thập và lưu trữ metrics hệ thống.
7. **`wp_grafana`**: Giao diện trực quan hóa dữ liệu giám sát.
8. **`wp_loki`**: Hệ thống lưu trữ log tập trung.
9. **`wp_promtail`**: Tác nhân gom log từ các container đẩy về Loki.

---

## 🚀 Hướng dẫn chạy hệ thống nhanh chóng

### Bước 1: Clone Repository về máy chủ
```bash
git clone [https://github.com/BuiduchaDTC/DTC245180109-DeTai3-WordPress.git](https://github.com/BuiduchaDTC/DTC245180109-DeTai3-WordPress.git)
cd DTC245180109-DeTai3-WordPress

Bước 2: Thiết lập tệp biến môi trường
Tạo tệp .env từ tệp mẫu có sẵn:

Bash
cp .env.example .env
Bước 3: Khởi chạy toàn bộ hệ thống
Chạy lệnh Docker Compose ở chế độ nền:

Bash
docker compose up -d
Bước 4: Kiểm tra trạng thái
Gõ lệnh để kiểm tra các container đang chạy:

Bash
docker ps
(Đảm bảo hiển thị đủ 9 container ở trạng thái Up).

🔒 Các biện pháp Bảo mật (Hardening) đã áp dụng
Database Privilege Hardening: Tạo tài khoản riêng wp_app_user, chỉ cấp quyền trên database wordpress_product_db.

Network Isolation: Phân tách thành 2 lớp mạng (frontend_net và backend_net), cô lập database khỏi internet trực tiếp.

Transport Security (HTTPS): Bắt buộc chuyển hướng lưu lượng từ cổng 80 (HTTP) sang cổng 443 (HTTPS) thông qua Nginx.

Security Headers: Cấu hình các tiêu chuẩn bảo mật phòng chống tấn công XSS, Clickjacking (X-Frame-Options, HSTS,...).

Self-healing: Thiết lập chính sách restart: unless-stopped cho toàn bộ container.
