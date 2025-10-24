# 🧩 Microservices E-Commerce Platform

### 🐳 Node.js | 🐇 RabbitMQ | 🔐 JWT | 🚪 API Gateway | 🧱 MongoDB

---

## 🚀 Giới thiệu

Dự án minh họa cách xây dựng **hệ thống thương mại điện tử (E-commerce)** theo **kiến trúc Microservices**, trong đó các chức năng chính được tách thành các service độc lập:

- 👤 **Auth Service:** Quản lý người dùng, đăng ký/đăng nhập & JWT
- 🛍️ **Product Service:** Quản lý sản phẩm & tồn kho
- 🧾 **Order Service:** Quản lý đơn hàng & cập nhật tồn kho qua RabbitMQ
- 🚪 **API Gateway:** Cổng duy nhất nhận và định tuyến request

Hệ thống vận hành trên Docker, sử dụng RabbitMQ để giao tiếp bất đồng bộ giữa các service.

---

## ⚙️ 1. Cài đặt RabbitMQ trên Docker

Chạy lệnh sau để khởi tạo RabbitMQ:

```bash
docker run -it --rm --name rabbitmq -p 5672:5672 -p 15672:15672 rabbitmq:4-management
```

🔗 Truy cập: [http://localhost:15672](http://localhost:15672)
👤 Tài khoản: `guest` / `guest`

📸 **Giao diện quản lý RabbitMQ**
![RabbitMQ Setup](public/asset/rabbitmq.png)

---

## 🧱 2. Khởi tạo các Container khác

📸
![Containers Setup](public/asset/dockersetup.png)

---

## 🌐 3. Cấu hình API Gateway

Cập nhật file cấu hình để định tuyến đến đúng các service (Auth, Product, Order).

📸
![Config API Gateway](public/asset/setupapigateway.png)

---

## 🔑 4. Tạo JWT khi đăng nhập

Thêm logic ký **JWT Token** trong Auth Service để xác thực người dùng.

📸
![JWT Logic](public/asset/logicloginjwt.png)

---

## 🧪 5. Kiểm thử API với Postman

### 🧍‍♂️ Đăng ký tài khoản

`POST /auth/api/v1/register`
![Register](public/asset/register.png)
📦 **Kết quả trong MongoDB:**
![MongoDB Register](public/asset/databaseuser.png)

---

### 🔐 Đăng nhập tài khoản

`POST /auth/api/v1/login`
![Login](public/asset/login.png)
✅ Nhận **JWT Token**

---

### 🛒 Thêm sản phẩm

`POST /products/api/v1/products/add`
Truyền token vào header:

```
Authorization: Bearer <JWT_TOKEN>
```

📸
![Add Product](public/asset/apikey.png)
📦 **Kết quả trong MongoDB:**
![MongoDB Product](public/asset/databaseproduct.png)

---


### 🧾 Tạo đơn hàng

`POST /products/api/v1/buy`
Truyền `ids` và `quantity` trong body.
📸
![Order](public/asset/buy.png)

✅ Nếu **đặt hàng thành công:**
![Order Success](public/asset/ketquadathang.png)

📊 Kiểm tra trong MongoDB:
![Check Order DB](public/asset/databaseorder.png)

---

## 🔁 6. CI/CD với GitHub Actions

Tự động build & deploy dự án thông qua workflow GitHub Actions.
📸
![CI/CD Workflow](public/asset/build-and-rundocker.png)
![CI/CD Workflow](public/asset/deploy.png)

---

## 🧠 Tổng kết hệ thống

### 💡 1. Hệ thống giải quyết vấn đề gì?

Giải quyết bài toán **E-commerce**, với 3 module chính: **Auth**, **Product**, **Order**.

---

### 🧩 2. Gồm bao nhiêu dịch vụ?

Tổng cộng **6 dịch vụ**:

- **Ứng dụng:** `api_gateway`, `auth_service`, `product_service`, `order_service`
- **Hạ tầng:** `mongodb`, `rabbitmq`

---

### 🧰 3. Ý nghĩa từng dịch vụ

| Dịch vụ                     | Vai trò                               |
| --------------------------- | ------------------------------------- |
| 🗄️ `nghiem_mongodb`         | Lưu dữ liệu (User, Product, Order)    |
| 🐇 `nghiem_rabbitmq`        | Hàng đợi tin nhắn bất đồng bộ         |
| 🚪 `nghiem_api_gateway`     | Cổng định tuyến request               |
| 👤 `nghiem_auth_service`    | Xử lý đăng ký, đăng nhập, JWT         |
| 🛍️ `nghiem_product_service` | Quản lý sản phẩm & tồn kho            |
| 🧾 `nghiem_order_service`   | Xử lý đơn hàng & sự kiện qua RabbitMQ |

---

### 🧠 4. Mẫu thiết kế sử dụng

- 🧩 **Microservices Architecture**
- 🚪 **API Gateway Pattern**
- 🗃️ **Database per Service**
- 📬 **Event-Driven / Pub-Sub (RabbitMQ)**

---

### 🔄 5. Giao tiếp giữa các service

- **Đồng bộ (HTTP):** `Client → API Gateway → Service`
- **Bất đồng bộ (RabbitMQ):** `Order Service → RabbitMQ → Product Service`

---

## 🌟 Kết luận

✅ Hệ thống microservices hoạt động ổn định với giao tiếp đồng bộ & bất đồng bộ.
✅ RabbitMQ giúp tách biệt service, tăng khả năng mở rộng và chịu lỗi.
✅ API Gateway & JWT bảo mật luồng giao tiếp giữa client và backend.

---

> 💻 **Tác giả:** _NguyenDuy_
> 🗓️ **Phiên bản:** 1.0
> 📦 **Chạy dự án:**
>
> ```bash
> docker-compose up# 🧩 **E-Commerce Microservices Platform**

### 🐳 Node.js | 🐇 RabbitMQ | 🔐 JWT | 🚪 API Gateway | 🧱 MongoDB

---

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
