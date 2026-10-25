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
## Giám sát (Prometheus + Grafana)
- Prometheus: `http://<IP-VM>:9090`, Grafana: `http://<IP-VM>:3000`
- Exporter: node-exporter (máy chủ), cAdvisor (container), mysqld-exporter (MySQL), blackbox (HTTP WordPress).
- mysqld-exporter dùng tài khoản MySQL riêng `exporter` với quyền tối thiểu (mật khẩu trong `.env`, biến `EXPORTER_PASSWORD`).
- Dashboard nạp tự động từ `grafana/dashboards/`.
- Mạng: `db` (internal, không ra ngoài), `web`, `monitoring`.

## Log tập trung (Loki + Promtail)
- Promtail đọc log container qua Docker socket (chỉ đọc) và đẩy sang Loki.
- Grafana có datasource Loki, truy vấn bằng LogQL (Explore, chọn Loki).
- Truy vấn mẫu 1: {container="nginx"}
- Truy vấn mẫu 2: {container="nginx"} |~ "\" (301|302|404) "
- Truy vấn mẫu 3: sum by (service) (count_over_time({container=~".+"}[1m]))
