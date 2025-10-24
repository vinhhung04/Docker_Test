## Dự án: Eproject

Đây là một dự án microservices mẫu cho khóa học/đồ án, được triển khai thành nhiều dịch vụ nhỏ (microservices) gồm `api-gateway`, `auth`, `order` và `product`.

### Tổng quan

- Ngôn ngữ: Node.js (JavaScript)
- Mỗi dịch vụ có `package.json` và có thể chạy độc lập
- Hỗ trợ phát triển cục bộ bằng Docker Compose (có file `docker-compose.yml` ở thư mục gốc)

### Cấu trúc thư mục chính

- `api-gateway/` - Cổng API, chịu trách nhiệm định tuyến và có thể thực hiện các logic chung (proxy)
- `auth/` - Dịch vụ xác thực (đăng ký/đăng nhập, middleware xác thực)
- `order/` - Dịch vụ quản lý đơn hàng
- `product/` - Dịch vụ quản lý sản phẩm
- `docker-compose.yml` - Tập lệnh để khởi chạy toàn bộ hệ thống bằng Docker Compose

Mỗi dịch vụ có cấu trúc tương tự gồm `src/`, `routes/`, `controllers/`, `services/`, `repositories/` và `test/`.

### Yêu cầu

- Node.js v14+ (hoặc phiên bản tương thích)
- npm hoặc yarn
- Docker và Docker Compose (nếu muốn chạy bằng container)

### Cách chạy (cục bộ, không dùng Docker)

1. Mở terminal, vào từng thư mục dịch vụ và cài phụ thuộc:

```powershell
cd auth; npm install
cd ../product; npm install
cd ../order; npm install
cd ../api-gateway; npm install
```

2. Chạy từng dịch vụ (mỗi dịch vụ có thể có script `start` trong `package.json`):

```powershell
cd auth; npm start
cd ../product; npm start
cd ../order; npm start
cd ../api-gateway; npm start
```

Lưu ý: Một số dịch vụ có thể cần cấu hình môi trường (PORT, URL đến DB hoặc message broker). Kiểm tra các file `src/config` hoặc `config.js` tương ứng.

### Chạy bằng Docker Compose

1. Đảm bảo Docker đang chạy.
2. Từ thư mục gốc của dự án (nơi có `docker-compose.yml`), chạy:

```powershell
docker-compose up --build
```

Tùy cài đặt của `docker-compose.yml`, các service sẽ được khởi tạo và có thể truy cập qua các cổng được cấu hình.

### Test

Mỗi service có thư mục `test/`. Bạn có thể chạy lệnh test trong từng thư mục:

```powershell
cd product; npm test
cd ../auth; npm test
```

### API cơ bản (tóm tắt)

- Auth: đăng ký, đăng nhập, endpoints liên quan tới token và middleware xác thực
- Product: CRUD sản phẩm, danh sách sản phẩm
- Order: Tạo đơn hàng, xem đơn hàng
- Api-gateway: Forward request tới các service tương ứng

Để biết chi tiết endpoint, xem các file routes trong `src/routes` hoặc `routes/` của từng service.

### Gợi ý phát triển

- Thêm file `.env` cho mỗi service để cấu hình biến môi trường
- Viết test unit cho controllers/services
- Triển khai CI/CD (ví dụ: GitHub Actions) để tự động hoá test và build Docker images


### -- Register -- POST
http://localhost:3003/auth/register
{
  "username": "hung",
  "password": "123"
}
### -- Login -- POST
http://localhost:3003/auth/login
{
  "username": "hung",
  "password": "123"
}
### -- Create Product -- POST
http://localhost:3003/products/
{
  "name": "Laptop",
  "description": "New Apple Model",
  "price": 1500
}
### -- Buy Product -- POST
http://localhost:3003/products/buy
[
{
  "_id": "68faf0dde661d6124c3b44ef",
  "quantity": 1
}
]
### -- Get Product By Id -- GET
http://localhost:3003/products/68faf0dde661d6124c3b44ef