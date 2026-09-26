# BÀI TẬP BÁO CÁO LẬP TRÌNH WEB - TNUT

**Họ và tên:** Ngô Văn Nguyên  
**Lớp:** Kỹ thuật Máy tính  

---

## 1. Môi trường triển khai
- Môi trường giả lập OS: Linux Ubuntu 22.04 LTS (thông qua WSL2 trên Windows 11).
- Công cụ đóng gói: Docker & Docker Compose.

## 2. Danh sách các dịch vụ (Docker Containers)
Hệ thống bao gồm các dịch vụ khởi chạy qua `docker-compose.yml`:
1. **Nginx Web Server** (Cổng `80`): Phục vụ trang web tĩnh HTML/JS và cấu hình Reverse Proxy truyền dữ liệu từ Node-RED.
2. **Node-RED Service** (Cổng `1880`): Xây dựng RESTful API khởi tạo dữ liệu mẫu cho bài tập 2.
3. **MariaDB Database** (Cổng `3306`): Hệ quản trị CSDL quan hệ.
4. **phpMyAdmin** (Cổng `8081`): Giao diện quản lý CSDL trực quan.

---

## 3. Cấu hình chi tiết

### File `docker-compose.yml`
```yaml
version: '3.8'

services:
  web_server:
    image: nginx:latest
    container_name: nginx_web
    restart: always
    ports:
      - "80:80"
      - "8080:80"
    volumes:
      - ./nginx/default.conf:/etc/nginx/conf.d/default.conf
      - ./html:/usr/share/nginx/html

  nodered_service:
    image: nodered/node-red:latest
    container_name: nodered_app
    restart: always
    ports:
      - "1880:1880"
    volumes:
      - ./nodered:/data

  database:
    image: mariadb:10.6
    container_name: mariadb_db
    restart: always
    environment:
      MYSQL_ROOT_PASSWORD: rootpassword
      MYSQL_DATABASE: tnut_db
      MYSQL_USER: tnut_user
      MYSQL_PASSWORD: tnutpassword
    ports:
      - "3306:3306"

  db_manager:
    image: phpmyadmin/phpmyadmin:latest
    container_name: phpmyadmin_gui
    restart: always
    ports:
      - "8081:80"
    environment:
      PMA_HOST: database
