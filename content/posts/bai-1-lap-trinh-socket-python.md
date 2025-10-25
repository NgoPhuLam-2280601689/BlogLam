---
title: "Lập trình Socket trong Python [Phần 1]: Xây dựng TCP Server đầu tiên"
date: 2025-10-25T19:00:00+07:00
draft: false
description: "Tìm hiểu kiến thức nền tảng về Lập trình Mạng bằng cách xây dựng một máy chủ TCP đơn giản với module 'socket' của Python."
tags: ["Python", "Socket", "Lập trình Mạng", "TCP", "Server"]
---

Chào các bạn!

Nếu bạn đang học môn Lập trình Mạng, "Socket" có lẽ là từ bạn nghe nhiều nhất. Vậy socket là gì?

Nói một cách đơn giản, **socket là một điểm cuối (endpoint) của một kênh giao tiếp hai chiều**.

Bạn có thể tưởng tượng nó giống như một "cuộc gọi điện thoại" 📞. Để hai người nói chuyện được, cả hai đều phải nhấc máy. "Cái điện thoại" ở mỗi đầu chính là một socket.

Trong lập trình mạng, socket cho phép một chương trình (ví dụ: trình duyệt của bạn) gửi và nhận dữ liệu từ một chương trình khác (ví dụ: máy chủ web của Google) qua mạng, bất kể chúng đang chạy trên cùng một máy hay ở hai đầu thế giới.

Trong loạt bài viết này, chúng ta sẽ tìm hiểu cách sử dụng module `socket` có sẵn của Python để tạo ra các ứng dụng mạng. Bài đầu tiên sẽ tập trung vào việc xây dựng một **TCP Server** đơn giản.

## 1. TCP là gì? Tại sao lại là TCP?

Trước khi viết code, chúng ta cần biết về "luật chơi". Khi giao tiếp qua mạng, có hai giao thức (protocol) chính:

1.  **TCP (Transmission Control Protocol - Giao thức điều khiển truyền vận):**
    * Giống như một **cuộc gọi điện thoại**.
    * **Kết nối (Connection-oriented):** Phải thiết lập kết nối (bắt tay) trước khi truyền dữ liệu.
    * **Đáng tin cậy (Reliable):** Đảm bảo dữ liệu đến đúng nơi, đúng thứ tự, và không bị mất. Nếu mất, nó sẽ tự động gửi lại.
    * Hoàn hảo cho: Truy cập web (HTTP), email (SMTP), truyền file (FTP).

2.  **UDP (User Datagram Protocol - Giao thức Dữ liệu Người dùng):**
    * Giống như gửi một **bưu thiếp (postcard)**.
    * **Không kết nối (Connectionless):** Cứ thế gửi, không cần biết người nhận có sẵn sàng hay không.
    * **Không đáng tin cậy:** Dữ liệu có thể đến sai thứ tự, bị mất, hoặc lặp lại.
    * Hoàn hảo cho: Game online, stream video, gọi VoIP (những ứng dụng ưu tiên tốc độ hơn là sự chính xác tuyệt đối).

Trong bài này, chúng ta sẽ xây dựng một Server **TCP**, vì nó là nền tảng của hầu hết các ứng dụng web mà bạn dùng hàng ngày.

## 2. Server làm những gì?

Một TCP Server luôn thực hiện 5 bước tuần tự:

1.  **Create (Tạo):** Tạo ra một socket.
2.  **Bind (Gắn):** Gắn socket đó với một địa chỉ IP và một cổng (port) cụ thể trên máy tính. Giống như "đăng ký" một số điện thoại cho cái điện thoại của bạn.
3.  **Listen (Lắng nghe):** Bật máy và chờ kết nối đến.
4.  **Accept (Chấp nhận):** Khi có client kết nối (gọi đến), server chấp nhận cuộc gọi.
5.  **Communicate (Giao tiếp):** Nhận và gửi dữ liệu với client.

Hãy biến 5 bước này thành code!

## 3. Viết code cho TCP Server

Chúng ta sẽ tạo một "Echo Server". Đây là một server rất cơ bản: bất cứ thứ gì client gửi đến, server sẽ gửi trả lại (echo) y hệt.

Tạo một file mới tên là `server.py` và gõ vào nội dung sau:

```python
import socket

# 1. Định nghĩa địa chỉ và cổng
# HOST là địa chỉ IP của máy chủ. 
# '127.0.0.1' là địa chỉ đặc biệt (localhost), nghĩa là "chính máy này".
HOST = '127.0.0.1'  
# PORT là số cổng mà server sẽ lắng nghe.
# Chúng ta chọn một số lớn hơn 1023 (vì các cổng nhỏ hơn thường được
# dành riêng cho hệ thống).
PORT = 65432        

print("--- Server đang khởi động ---")

# 2. Tạo và cấu hình Socket
# 'with' statement sẽ tự động đóng socket khi khối lệnh kết thúc
with socket.socket(socket.AF_INET, socket.SOCK_STREAM) as s:
    # socket.AF_INET:     Chỉ định rằng chúng ta đang dùng IPv4.
    # socket.SOCK_STREAM: Chỉ định rằng chúng ta đang dùng TCP.

    # 3. Bind (Gắn) socket vào địa chỉ và cổng
    s.bind((HOST, PORT))

    # 4. Listen (Lắng nghe) kết nối
    # Server bắt đầu lắng nghe kết nối đến
    s.listen()
    print(f"Server đang lắng nghe tại {HOST}:{PORT}")

    # 5. Accept (Chấp nhận) kết nối
    # Lệnh accept() sẽ "dừng" (block) chương trình
    # cho đến khi có một client kết nối.
    # Khi có client kết nối, nó trả về 2 thứ:
    # - conn: Một đối tượng socket MỚI, dùng để giao tiếp với client.
    # - addr: Thông tin (IP, Port) của client vừa kết nối.
    conn, addr = s.accept()

    # Chúng ta lại dùng 'with' cho socket mới (conn)
    with conn:
        print(f"Đã kết nối bởi {addr}")
        
        # 6. Giao tiếp (Nhận và Gửi dữ liệu)
        while True:
            # Nhận dữ liệu từ client
            # 1024 là kích thước bộ đệm (buffer size)
            data = conn.recv(1024)
            
            # Nếu không nhận được dữ liệu (data rỗng)
            # nghĩa là client đã ngắt kết nối.
            if not data:
                print(f"Client {addr} đã ngắt kết nối.")
                break # Thoát khỏi vòng lặp
            
            print(f"Nhận từ Client: {data.decode('utf-8')}")

            # Gửi trả lại (echo) dữ liệu cho client
            # Chúng ta gửi lại chính xác những gì đã nhận
            conn.sendall(data)