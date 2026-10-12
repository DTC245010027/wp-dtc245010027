# WordPress Product Website (Đề 3) - dtc245010027

Website quảng bá sản phẩm bằng WordPress, triển khai bằng Docker Compose.

## Thành phần
- WordPress (php8.2-apache), MySQL 8.0, phpMyAdmin
- Nginx reverse proxy (HTTPS tự ký, security headers)

## Chạy hệ thống
1. Sao chép `.env.example` thành `.env` và đặt mật khẩu mạnh.
2. Tạo chứng chỉ tự ký vào `nginx/certs/` (server.crt, server.key).
3. `docker compose up -d`
4. Truy cập `https://<IP-VM>` (WordPress) và `https://<IP-VM>/phpmyadmin/`.