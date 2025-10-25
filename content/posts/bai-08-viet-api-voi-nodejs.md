---
title: "REST API là gì? Viết API đầu tiên với Node.js và Express"
date: 2025-10-18T19:00:00+07:00
draft: false
tags: ["Node.js", "Express", "API", "Back-end", "JavaScript"]
categories: ["Lập trình Back-end"]
---

Nếu bạn là sinh viên CNTT, chắc chắn bạn đã nghe cụm từ "API" ở khắp mọi nơi. Nhưng nó chính xác là gì? Và tại sao Node.js cùng Express lại là bộ đôi hoàn hảo để tạo ra nó?

### API là gì? (Giải thích đơn giản)

Hãy tưởng tượng bạn đang ở trong một nhà hàng.

* Bạn (Client) là **người dùng**.
* Nhà bếp (Server) là **máy chủ** chứa dữ liệu.
* Cuốn menu (API) là **API**.

Bạn không thể tự ý xông vào bếp (server) để lấy đồ ăn (dữ liệu). Thay vào đó, bạn dùng **cuốn menu (API)** để đưa ra một yêu cầu rõ ràng (ví dụ: "Cho tôi món Phở"). Người phục vụ sẽ nhận yêu cầu, đưa vào bếp, và mang món ăn (dữ liệu) ra cho bạn.

**API (Application Programming Interface)** chính là "cuốn menu" đó. Nó là một bộ quy tắc và giao thức cho phép các ứng dụng khác nhau "nói chuyện" và trao đổi dữ liệu với nhau.

**REST (Representational State Transfer)** là một trong những kiểu kiến trúc phổ biến nhất để thiết kế API. Nó sử dụng các phương thức HTTP (như `GET`, `POST`, `PUT`, `DELETE`) mà chúng ta sẽ tìm hiểu ngay sau đây.

### Tại sao lại là Node.js và Express?

* **Node.js:** Là môi trường giúp bạn chạy code JavaScript ở phía máy chủ (trong "nhà bếp").
* **Express.js:** Là một framework (giống như bộ khung sườn) tối giản cho Node.js. Nó giúp chúng ta tổ chức "nhà bếp" của mình một cách gọn gàng, nhanh chóng và hiệu quả. Nó lo các việc vặt, để chúng ta tập trung vào việc "chế biến món ăn" (xử lý logic).

### Bước 1: Chuẩn bị "Nhà bếp" (Cài đặt)

Trước tiên, chúng ta cần một thư mục dự án và cài đặt Express.

1.  Mở terminal và tạo một thư mục mới: `mkdir my-api && cd my-api`
2.  Khởi tạo dự án Node.js (tạo file `package.json`):
    ```bash
    npm init -y
    ```
3.  Cài đặt Express:
    ```bash
    npm install express
    ```

### Bước 2: Viết code Server (Tệp `server.js`)

Bây giờ, hãy tạo một file tên là `server.js`. Đây sẽ là nơi chúng ta định nghĩa "menu" của mình.

#### 2.1 Khởi tạo Server

Đầu tiên, chúng ta cần "nhập khẩu" Express, tạo ra ứng dụng (server) và chọn một "cổng" (port) để chạy. Giống như việc mở cửa nhà hàng ở một địa chỉ cụ thể.

```javascript
const express = require('express');
const app = express();
const port = 3000;

#### 2.2 Middleware: "Người phiên dịch"

Đây là một bước cực kỳ quan trọng. Hầu hết dữ liệu ngày nay được gửi ở dạng JSON. Dòng code dưới đây là một **middleware**, nó giống như một "người phiên dịch" đứng ở cửa.

Khi client gửi dữ liệu (ví dụ: tạo user mới) ở dạng JSON, dòng này sẽ tự động "dịch" nó sang một đối tượng JavaScript để chúng ta có thể sử dụng dễ dàng trong `req.body`.

```javascript
app.use(express.json()); 
````

#### 2.3 Tạo một cơ sở dữ liệu "giả"

Trong thực tế, bạn sẽ dùng MongoDB hoặc MySQL. Nhưng để cho đơn giản, chúng ta sẽ lưu dữ liệu ngay trong một mảng (array) tạm thời.

```javascript
let users = [
  { id: 1, name: 'An Nguyen' },
  { id: 2, name: 'Binh Tran' }
];
```

#### 2.4 Tạo "Món ăn" đầu tiên: GET (Đọc dữ liệu)

Chúng ta sẽ tạo "món" đầu tiên trong menu: Lấy danh sách tất cả người dùng.

  * Chúng ta dùng `app.get()` vì chúng ta muốn **LẤY (GET)** dữ liệu.
  * `/users` là "đường dẫn" (endpoint) mà client sẽ truy cập.
  * `(req, res)` là hàm xử lý, với `req` (Request - yêu cầu) là những gì client gửi lên, và `res` (Response - phản hồi) là những gì chúng ta trả về.

<!-- end list -->

```javascript
// GET (Đọc) tất cả users
app.get('/users', (req, res) => {
  res.json(users); // Trả về mảng users dưới dạng JSON
});
```

#### 2.5 "Món ăn" thứ hai: POST (Tạo mới dữ liệu)

Tiếp theo, chúng ta tạo "món" cho phép client **TẠO (POST)** một user mới.

  * Chúng ta dùng `app.post()` vì chúng ta muốn **TẠO (POST)** dữ liệu mới.
  * `req.body` chính là dữ liệu JSON mà client gửi lên (đã được "phiên dịch" bởi `app.use(express.json())`).
  * Chúng ta thêm user mới vào mảng và trả về `status 201` (nghĩa là "Đã tạo thành công").

<!-- end list -->

```javascript
// POST (Tạo) một user mới
app.post('/users', (req, res) => {
  const newUser = {
    id: users.length + 1, // Tạo ID đơn giản
    name: req.body.name 
  };
  
  users.push(newUser);
  res.status(201).json(newUser);
});
```

### Bước 3: Mở cửa Nhà hàng (Chạy Server)

Cuối cùng, chúng ta cần ra lệnh cho server "mở cửa" và bắt đầu lắng nghe ở cổng 3000.

```javascript
app.listen(port, () => {
  console.log(`Server đang lắng nghe tại http://localhost:${port}`);
});
```

### Chạy thử thôi\!

Bây giờ, hãy quay lại terminal và chạy:

```bash
node server.js
```

Bạn sẽ thấy dòng chữ `Server đang lắng nghe tại http://localhost:3000`.

Máy chủ của bạn đã chạy\! Bạn đã chính thức tạo ra một API. Bạn có thể dùng các công cụ như **Postman** hoặc **curl** để thử truy cập `GET http://localhost:3000/users` và `POST http://localhost:3000/users` (với một body JSON là `{"name": "Ten Cua Ban"}`) để xem kết quả\!

```
```