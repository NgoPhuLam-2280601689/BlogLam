---
title: "SQL JOINs: Nghệ thuật 'Ghép' dữ liệu từ nhiều bảng"
date: 2025-10-20T10:30:00+07:00
draft: false
tags: ["SQL", "Database", "Cơ sở dữ liệu", "Data"]
categories: ["Cơ sở dữ liệu"]
description: "SQL JOIN là gì? Giải thích đơn giản và trực quan về INNER JOIN, LEFT JOIN, và tại sao nó là kỹ năng bắt buộc khi làm việc với cơ sở dữ liệu."
---

Một trong những sức mạnh lớn nhất của Cơ sở dữ liệu Quan hệ (Relational Database) là khả năng "bình thường hóa" (normalize) dữ liệu. Tức là, thay vì nhồi nhét mọi thứ vào một bảng khổng lồ, chúng ta chia nhỏ dữ liệu ra nhiều bảng logic để tránh lặp lại thông tin.

Ví dụ, chúng ta không lưu tên người dùng trong bảng "Đơn hàng". Chúng ta chỉ lưu `UserID` (một con số). Sau đó, chúng ta có một bảng "Người dùng" riêng chỉ để lưu thông tin người dùng.

Nhưng khi cần xem báo cáo, chúng ta muốn biết "Tên người dùng" đã mua "Tên sản phẩm" nào. Làm thế nào để kết hợp dữ liệu từ hai bảng này?

Câu trả lời chính là `JOIN`. `JOIN` là lệnh cho phép bạn "ghép" các hàng từ hai hoặc nhiều bảng lại với nhau, dựa trên một cột chung (gọi là "khóa ngoại" - foreign key).

### Chuẩn bị "Dữ liệu mẫu"

Hãy tưởng tượng chúng ta có 2 bảng:

**Bảng 1: `Users`** (Lưu danh sách người dùng)
| UserID | Name |
| :--- | :--- |
| 1 | An Nguyễn |
| 2 | Bình Trần |
| 3 | Chi Lê |

**Bảng 2: `Orders`** (Lưu danh sách đơn hàng đã đặt)
| OrderID | Product | UserID |
| :--- | :--- | :--- |
| 101 | Laptop | 2 |
| 102 | Chuột | 1 |
| 103 | Bàn phím | 2 |
| 104 | Màn hình | 4 |

Lưu ý: "An" (ID 1) và "Bình" (ID 2) đã mua hàng. "Chi" (ID 3) chưa mua gì. Và có một đơn hàng "Màn hình" (ID 104) được đặt bởi `UserID` 4, người không có trong bảng `Users` của chúng ta (có thể là khách vãng lai).

Cột chung để liên kết 2 bảng này là `Users.UserID` và `Orders.UserID`.

### 1. INNER JOIN (Phần giao)

`INNER JOIN` (hoặc chỉ cần gõ `JOIN`) là loại JOIN phổ biến nhất. Nó chỉ trả về những hàng **có sự trùng khớp ở cả hai bảng**.

Nói cách khác: "Hãy cho tôi danh sách những **người dùng đã đặt hàng** VÀ **đơn hàng đó thuộc về ai đó**."



```sql
SELECT Users.Name, Orders.Product
FROM Users
INNER JOIN Orders ON Users.UserID = Orders.UserID;
````

**Kết quả sẽ là:**

| Name | Product |
| :--- | :--- |
| Bình Trần | Laptop |
| An Nguyễn | Chuột |
| Bình Trần | Bàn phím |

**Tại sao?**

  * "Chi Lê" (ID 3) bị loại, vì cô ấy không có đơn hàng nào trong bảng `Orders`.
  * Đơn hàng "Màn hình" (UserID 4) bị loại, vì `UserID` 4 không tồn tại trong bảng `Users`.
  * `INNER JOIN` chỉ lấy những gì "khớp" hoàn hảo: An (1) khớp với Chuột (1), Bình (2) khớp với Laptop (2) và Bàn phím (2).

### 2\. LEFT JOIN (Lấy tất cả bên Trái)

`LEFT JOIN` (hoặc `LEFT OUTER JOIN`) sẽ: Lấy **TẤT CẢ** các hàng từ bảng bên trái (bảng `Users`), và chỉ lấy những hàng **khớp** từ bảng bên phải (bảng `Orders`).

Nói cách khác: "Hãy cho tôi danh sách **tất cả người dùng**, kèm theo đơn hàng của họ **nếu có**. Nếu họ không có đơn hàng nào, cứ hiển thị tên họ và để trống phần đơn hàng."

```sql
SELECT Users.Name, Orders.Product
FROM Users
LEFT JOIN Orders ON Users.UserID = Orders.UserID;
```

**Kết quả sẽ là:**

| Name | Product |
| :--- | :--- |
| An Nguyễn | Chuột |
| Bình Trần | Laptop |
| Bình Trần | Bàn phím |
| **Chi Lê** | **NULL** |

**Tại sao?**

  * Bảng bên trái là `Users`, vì vậy nó lấy tất cả 3 người: An, Bình, Chi.
  * An và Bình có đơn hàng, nên nó ghép vào như `INNER JOIN`.
  * "Chi Lê" (ID 3) không có bản ghi nào khớp trong bảng `Orders`, vì vậy `LEFT JOIN` vẫn giữ lại "Chi Lê" và điền `NULL` (trống) vào cột `Product`.
  * Đơn hàng "Màn hình" (UserID 4) vẫn bị loại, vì nó không khớp với bất kỳ ai trong bảng `Users` (bảng bên trái).

### Các loại JOIN khác (Nâng cao)

Ngoài 2 loại chính trên, còn có:

  * **RIGHT JOIN:** Ngược lại với `LEFT JOIN`. Lấy **tất cả** từ bảng bên phải (`Orders`) và chỉ lấy những gì khớp từ bảng bên trái (`Users`). (Nếu dùng ví dụ trên, nó sẽ lấy cả đơn hàng "Màn hình" và ghép với `NULL` ở cột `Name`).
  * **FULL OUTER JOIN:** Lấy **TẤT CẢ** từ cả hai bảng. Bất cứ hàng nào không khớp ở bảng đối diện sẽ được điền `NULL`. (Kết quả sẽ bao gồm cả "Chi Lê - NULL" và "NULL - Màn hình").

### Kết luận

Hiểu rõ các loại `JOIN` là kỹ năng cực kỳ quan trọng. `INNER JOIN` dùng để tìm các cặp dữ liệu khớp nhau ("Ai đã mua hàng?"), trong khi `LEFT JOIN` thường được dùng để tìm dữ liệu "bị thiếu" ("Ai chưa mua hàng?").

```
```