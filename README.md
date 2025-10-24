
## 🚀 **Tổng quan dự án**

Dự án minh họa cách triển khai **hệ thống thương mại điện tử (E-Commerce)** theo **kiến trúc Microservices**, trong đó mỗi chức năng được tách thành một service độc lập, giúp dễ dàng mở rộng, bảo trì và triển khai song song.

**Các service chính gồm:**

- 👤 **Auth Service:** Đăng ký, đăng nhập và xác thực người dùng qua JWT  
- 🛍️ **Product Service:** Quản lý sản phẩm và tồn kho  
- 🧾 **Order Service:** Xử lý đơn hàng, đồng bộ tồn kho thông qua RabbitMQ  
- 🚪 **API Gateway:** Cổng trung gian định tuyến toàn bộ request

Toàn bộ hệ thống được container hóa bằng Docker, sử dụng **RabbitMQ** để truyền thông điệp bất đồng bộ giữa các service.

---

## ⚙️ **1. Cài đặt RabbitMQ**

Khởi chạy RabbitMQ trên Docker bằng lệnh:

```bash
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:4-management
```

- 🌐 Truy cập giao diện: [http://localhost:15672](http://localhost:15672)  
- 👤 Tài khoản mặc định: `guest` / `guest`

📸 **Giao diện quản lý RabbitMQ**
![RabbitMQ Setup](public/asset/rabbitmq.png)

---

## 🧱 **2. Khởi tạo các Container dịch vụ**

Chạy các container tương ứng cho từng service trong hệ thống:

📸  
![Containers Setup](public/asset/dockersetup.png)

---

## 🌐 **3. Cấu hình API Gateway**

Cập nhật file cấu hình định tuyến để kết nối chính xác đến các service: **Auth**, **Product**, và **Order**.

📸  
![Config API Gateway](public/asset/setupapigateway.png)

---

## 🔑 **4. Tích hợp JWT trong Auth Service**

Thêm logic ký và xác thực **JWT Token** giúp bảo mật luồng đăng nhập và truy cập API.

📸  
![JWT Logic](public/asset/logicloginjwt.png)

---

## 🧪 **5. Kiểm thử API bằng Postman**

### 🧍‍♂️ **Đăng ký tài khoản mới**

`POST /auth/api/v1/register`  
📸  
![Register](public/asset/register.png)

📦 **Kết quả trong MongoDB:**  
![MongoDB Register](public/asset/databaseuser.png)

---

### 🔐 **Đăng nhập và nhận JWT**

`POST /auth/api/v1/login`  
📸  
![Login](public/asset/login.png)  
✅ Sau khi đăng nhập thành công, hệ thống trả về **JWT Token**.

---

### 🛒 **Thêm sản phẩm mới**

`POST /products/api/v1/products/add`

Thêm header xác thực:
```
Authorization: Bearer <JWT_TOKEN>
```

📸  
![Add Product](public/asset/apikey.png)

📦 **Dữ liệu trong MongoDB:**  
![MongoDB Product](public/asset/databaseproduct.png)

---

### 🧾 **Tạo đơn hàng**

`POST /products/api/v1/buy`

Truyền danh sách `ids` và `quantity` trong body.  
📸  
![Order](public/asset/buy.png)

✅ **Kết quả khi đặt hàng thành công:**  
![Order Success](public/asset/ketquadathang.png)

📊 **Kiểm tra trong MongoDB:**  
![Check Order DB](public/asset/databaseorder.png)

---

## 🔁 **6. CI/CD với GitHub Actions**

Tự động hóa quy trình **build – deploy** thông qua **GitHub Actions**.  
📸  
![CI/CD Workflow](public/asset/build-and-rundocker.png)  
![CI/CD Workflow](public/asset/deploy.png)

---

## 🧠 **Tổng quan kiến trúc**

### 💡 1. Vấn đề hệ thống giải quyết
Xây dựng hệ thống thương mại điện tử với 3 module trọng tâm: **Auth**, **Product**, và **Order**, cho phép giao tiếp cả **đồng bộ (HTTP)** và **bất đồng bộ (RabbitMQ)**.

---

### 🧩 2. Thành phần chính

Hệ thống bao gồm **6 dịch vụ**:

| Dịch vụ                     | Vai trò chính                            |
| --------------------------- | ---------------------------------------- |
| 🗄️ `duy_mongodb`         | Lưu trữ dữ liệu người dùng, sản phẩm, đơn hàng |
| 🐇 `duy_rabbitmq`        | Trung gian truyền tin nhắn bất đồng bộ   |
| 🚪 `duy_api_gateway`     | Điều phối và định tuyến request          |
| 👤 `duy_auth_service`    | Đăng ký, đăng nhập, quản lý JWT          |
| 🛍️ `duy_product_service` | Quản lý sản phẩm & kho hàng              |
| 🧾 `duy_order_service`   | Xử lý đơn hàng và phản hồi tồn kho qua RabbitMQ |

---

### 🧰 3. Các mẫu thiết kế áp dụng

- 🧩 **Microservices Architecture**  
- 🚪 **API Gateway Pattern**  
- 🗃️ **Database per Service**  
- 📬 **Event-Driven Communication (RabbitMQ Pub/Sub)**

---

### 🔄 4. Giao tiếp giữa các service

| Kiểu giao tiếp | Luồng hoạt động |
|----------------|-----------------|
| 🔗 **Đồng bộ (HTTP)** | Client → API Gateway → Service |
| 📡 **Bất đồng bộ (RabbitMQ)** | Order Service → RabbitMQ → Product Service |

---

## 🌟 **Kết luận**

✅ Hệ thống Microservices vận hành ổn định với khả năng mở rộng cao.  
✅ RabbitMQ giúp tách biệt luồng xử lý, tăng hiệu năng và tính chịu lỗi.  
✅ API Gateway và JWT tăng cường bảo mật cho toàn bộ luồng request.

---

> 💻 **Tác giả:** _NguyenDuy_  
> 🗓️ **Phiên bản:** 1.0  
> 📦 **Khởi chạy dự án:**  
> ```bash
> docker-compose up
> ```

> ```

---
