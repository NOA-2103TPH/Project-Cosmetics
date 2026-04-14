# Cosmetics Shop (Laravel 10)

Project web bán mỹ phẩm, gồm:
- Frontend cho khách hàng mua sắm
- Admin panel quản lý sản phẩm/đơn hàng/người dùng
- Thanh toán COD và VNPay sandbox

## 1) Công nghệ sử dụng
- PHP `^8.1`
- Laravel `^10.10`
- MySQL `8.0`
- Node.js (khuyến nghị `>=18`, Dockerfile dùng `20.x`)
- Vite (build assets frontend)

## 2) Chuẩn bị môi trường
Bạn có thể chạy theo 1 trong 2 cách:
- Chạy local (PHP + MySQL cài sẵn trên máy)
- Chạy Docker Compose

## 3) Cài đặt và chạy local
### Bước 1: Cài dependencies
```bash
composer install
npm install
```

### Bước 2: Tạo file môi trường
```bash
cp .env.example .env
```

Mở `.env` và chỉnh DB theo máy local, ví dụ:
```env
APP_NAME=Cosmetics
APP_ENV=local
APP_DEBUG=true
APP_URL=http://127.0.0.1:8000

DB_CONNECTION=mysql
DB_HOST=127.0.0.1
DB_PORT=3306
DB_DATABASE=cosmetics
DB_USERNAME=root
DB_PASSWORD=
```

### Bước 3: Khởi tạo app
```bash
php artisan key:generate
php artisan migrate --seed
php artisan storage:link
```

### Bước 4: Chạy project
Mở 2 terminal:

Terminal 1:
```bash
php artisan serve
```

Terminal 2:
```bash
npm run dev
```

Truy cập:
- Website: `http://127.0.0.1:8000`
- Trang admin: `http://127.0.0.1:8000/admin/login`

## 4) Cài đặt và chạy bằng Docker
### Bước 1: Tạo `.env`
```bash
cp .env.example .env
```

Đổi thông số DB trong `.env` để khớp `docker-compose.yml`:
```env
APP_URL=http://localhost:8001

DB_CONNECTION=mysql
DB_HOST=mysql
DB_PORT=3306
DB_DATABASE=laravel
DB_USERNAME=laravel
DB_PASSWORD=secret
```

### Bước 2: Build và chạy container
```bash
docker compose up -d --build
```

### Bước 3: Cài dependencies + migrate trong container PHP
```bash
docker compose exec php composer install
docker compose exec php php artisan key:generate
docker compose exec php php artisan migrate --seed
docker compose exec php php artisan storage:link
```

Nếu cần compile assets:
```bash
docker compose exec php npm install
docker compose exec php npm run build
```

Truy cập:
- Website: `http://localhost:8001`
- phpMyAdmin: `http://localhost:8080`
- Admin login: `http://localhost:8001/admin/login`

## 5) Tài khoản mặc định sau khi seed
Seeder tạo sẵn admin:
- Email: `admin@yomail.com`
- Password: `123456`

## 6) Các lệnh thường dùng
```bash
# chạy test
php artisan test

# clear cache config/route/view
php artisan optimize:clear
```

Docker:
```bash
docker compose ps
docker compose logs -f
docker compose down
```

## 7) Lưu ý quan trọng
- File `docker/mysql/init.sql` và script `docker-setup.sh` đang chứa nội dung cũ liên quan `fuelphp_*`; khi chạy Laravel nên ưu tiên dùng migration (`php artisan migrate --seed`) như hướng dẫn trên.
- VNPay trong project đang dùng cấu hình sandbox hard-code trong `CheckoutController`. Khi deploy production cần thay bằng biến môi trường để bảo mật tốt hơn.
