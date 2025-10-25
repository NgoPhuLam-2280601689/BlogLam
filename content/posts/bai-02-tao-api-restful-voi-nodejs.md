---
title: "Tạo API RESTful đơn giản với Node.js và Express"
date: 2025-10-25T14:30:00+07:00
draft: false
tags: ["Node.js", "Express", "API", "Back-end"]
categories: ["Lập trình Back-end"]
description: "Hướng dẫn chi tiết cách xây dựng một API CRUD (Tạo, Đọc) cơ bản bằng Node.js và Express.js cho người mới bắt đầu."
---

Khi nói đến việc xây dựng back-end cho ứng dụng web, Node.js và Express.js là một "cặp đôi" cực kỳ phổ biến.

* **Node.js** là môi trường cho phép bạn chạy code JavaScript ở phía máy chủ (thay vì chỉ trên trình duyệt).
* **Express.js** là một framework (bộ khung) tối giản được xây dựng trên nền Node.js. Nó cung cấp các công cụ mạnh mẽ giúp việc định tuyến (routing) và xử lý các yêu cầu HTTP (như `GET`, `POST`) trở nên nhanh chóng và dễ dàng.

Trong bài viết này, chúng ta sẽ cùng nhau tạo một API "CRUD" siêu đơn giản. (CRUD là viết tắt của Create, Read, Update, Delete - nhưng trong bài này, chúng ta sẽ tập trung vào **Create** và **Read** trước).

### Bước 1: Khởi tạo dự án và Cài đặt

Đầu tiên, hãy mở terminal, tạo một thư mục mới cho dự án (ví dụ `my-api`) và di chuyển vào đó.

Sau đó, gõ hai lệnh sau:

```bash
# Lệnh 1: Khởi tạo một dự án Node.js
# '-y' có nghĩa là đồng ý với tất cả các câu hỏi mặc định
# Nó sẽ tạo ra file package.json để quản lý dự án
npm init -y

# Lệnh 2: Cài đặt thư viện Express
# Node.js sẽ tải Express về và lưu vào thư mục node_modules
npm install express
````

### Bước 2: Tạo file `server.js` và Viết code

Bây giờ, hãy tạo một file tên là `server.js` trong thư mục dự án của bạn. Đây sẽ là "trái tim" của API.

Hãy dán toàn bộ đoạn code sau vào file `server.js`:

```javascript
// 1. "Nhập khẩu" thư viện Express
const express = require('express');

// 2. Tạo một ứng dụng (app) Express
const app = express();

// 3. Chọn một cổng (port) cho server
// 3000 là cổng truyền thống cho các server phát triển
const port = 3000;

// 4. Middleware: Xử lý JSON
// Dòng này cực kỳ quan trọng!
// Nó bảo Express tự động "đọc" và "phân tích"
// bất kỳ dữ liệu JSON nào được gửi đến trong body của request.
app.use(express.json());

// 5. Tạo một cơ sở dữ liệu "giả" (in-memory)
// Để đơn giản, chúng ta lưu dữ liệu trong một mảng.
// Khi server tắt, dữ liệu này sẽ mất.
let users = [
    { id: 1, name: "Alice" },
    { id: 2, name: "Bob" }
];

// 6. ĐỊNH TUYẾN (Routing)

// --- GET (Đọc) tất cả users ---
// Khi ai đó truy cập (GET) vào đường dẫn '/users'
app.get('/users', (req, res) => {
  // 'req' (Request): Chứa thông tin về yêu cầu
  // 'res' (Response): Dùng để gửi phản hồi về cho client

  // Trả về toàn bộ mảng 'users' dưới dạng JSON
  res.json(users);
});

// --- POST (Tạo) một user mới ---
// Khi ai đó gửi dữ liệu (POST) đến đường dẫn '/users'
app.post('/users', (req, res) => {
  // Lấy dữ liệu user mới từ body của request
  // (Nhờ có 'app.use(express.json())' nên ta mới có req.body)
  const newUser = {
    id: users.length + 1, // Tạo ID đơn giản
    name: req.body.name   // Lấy tên từ dữ liệu gửi lên
  };

  // Thêm user mới vào mảng "cơ sở dữ liệu"
  users.push(newUser);

  // Phản hồi cho client
  // 'status(201)' có nghĩa là "Created" (Đã tạo thành công)
  // và gửi trả lại thông tin user vừa tạo
  res.status(201).json(newUser);
});

// 7. Khởi chạy Server
// Bảo server "lắng nghe" các yêu cầu ở cổng 3000
app.listen(port, () => {
  // In ra thông báo khi server đã sẵn sàng
  console.log(`Server đang lắng nghe tại http://localhost:${port}`);
});
```

### Bước 3: Chạy Server\!

Mọi thứ đã sẵn sàng. Quay lại terminal của bạn và gõ:

```bash
node server.js
```

Nếu bạn thấy dòng chữ `Server đang lắng nghe tại http://localhost:3000`, chúc mừng, API của bạn đã hoạt động\!

Bạn có thể dùng các công cụ như **Postman** hoặc **curl** để:

1.  Gửi yêu cầu **GET** đến `http://localhost:3000/users` để xem danh sách "Alice" và "Bob".
2.  Gửi yêu cầu **POST** đến `http://localhost:3000/users` (kèm theo một body JSON là `{"name": "Charlie"}`) để thêm user mới.

<!-- end list -->

```
```