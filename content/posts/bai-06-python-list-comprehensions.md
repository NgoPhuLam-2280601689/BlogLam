---
title: "Viết code Python ngắn gọn hơn với List Comprehensions"
date: 2025-10-21T13:00:00+07:00
draft: false
tags: ["Python", "Tips", "Code", "Pythonic"]
categories: ["Lập trình"]
description: "List Comprehension là gì? Hướng dẫn cách viết code Python 'đẹp' và hiệu quả hơn bằng cú pháp đặc biệt này."
---

Một trong những điều khiến Python được yêu thích là "tính biểu cảm" (expressiveness) của nó. Thường có nhiều cách để giải quyết một vấn đề, nhưng luôn có một cách được coi là "Pythonic" — một cách viết sạch sẽ, rõ ràng, và hiệu quả.

**List Comprehension** là một trong những ví dụ điển hình nhất của "Pythonic". Nó là một cú pháp đặc biệt, cho phép bạn tạo ra các danh sách (list) mới từ các danh sách có sẵn chỉ trong **một dòng code**.

Nó giống như việc bạn ra lệnh bằng ngôn ngữ tự nhiên: "Hãy tạo cho tôi một danh sách MỚI, bằng cách lấy TỪNG PHẦN TỬ trong danh sách CŨ, NẾU nó thỏa mãn điều kiện A, và biến đổi nó theo cách B."

Hãy xem qua một ví dụ.

### Cách viết Thông thường (Vòng lặp `for`)

Giả sử chúng ta muốn tạo một danh sách chứa bình phương của các số từ 0 đến 9. Cách viết truyền thống (và hoàn toàn không sai) là dùng vòng lặp `for`:

```python
# 1. Khởi tạo một danh sách rỗng
squares = []

# 2. Lặp qua các số từ 0 đến 9
for i in range(10):
    # 3. Tính bình phương và thêm vào danh sách
    squares.append(i * i)

# 4. In kết quả
print(squares) 
# Output: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
````

Cách này rõ ràng, nhưng hơi dài dòng. Nó tốn 3 dòng code chỉ để thực hiện một thao tác đơn giản.

### Cách viết "Pythonic" (List Comprehension)

Bây giờ, hãy làm điều y hệt chỉ với **một dòng code** sử dụng List Comprehension:

```python
squares = [i * i for i in range(10)]

print(squares) 
# Output: [0, 1, 4, 9, 16, 25, 36, 49, 64, 81]
```

Hãy "giải phẫu" cú pháp này. Nó có 3 phần chính, đọc từ **trái sang phải** nhưng hãy hiểu theo thứ tự **từ phải sang trái**:

`[ (biểu_thức_biến_đổi) | (vòng_lặp_for) ]`

1.  **(Vòng lặp `for`) `for i in range(10)`**: "Hãy lặp qua các số từ 0 đến 9, mỗi số gọi là `i`..."
2.  **(Biểu thức biến đổi) `i * i`**: "...với mỗi `i` đó, hãy tính bình phương (`i * i`) của nó."
3.  **` (Dấu ngoặc vuông)  `[...]\`**: "...và 'gom' tất cả kết quả đó vào một danh sách mới."

Đọc xuôi: "[Hãy lấy `i * i`] [cho mỗi `i`] [trong `range(10)`]".

### Thêm "phép thuật": List Comprehension với Điều kiện `if`

Đây là lúc List Comprehension thực sự tỏa sáng. Giả sử chúng ta chỉ muốn lấy bình phương của các **số chẵn**?

Cách viết thông thường:

```python
even_squares = []
for i in range(10):
    # Thêm một điều kiện 'if' ở đây
    if i % 2 == 0: 
        even_squares.append(i * i)
        
print(even_squares) 
# Output: [0, 4, 16, 36, 64]
```

Cách viết "Pythonic":

```python
even_squares = [i * i for i in range(10) if i % 2 == 0]

print(even_squares) 
# Output: [0, 4, 16, 36, 64]
```

Cú pháp bây giờ là:

`[ (biểu_thức_biến_đổi) | (vòng_lặp_for) | (điều_kiện_if) ]`

1.  **(Vòng lặp `for`) `for i in range(10)`**: "Lặp qua các số từ 0 đến 9..."
2.  **(Điều kiện `if`) `if i % 2 == 0`**: "...CHỈ LẤY những số `i` nào là số chẵn..."
3.  **(Biểu thức biến đổi) `i * i`**: "...và tính bình phương của chúng, rồi gom vào danh sách."

Đọc xuôi: "[Hãy lấy `i * i`] [cho mỗi `i`] [trong `range(10)`] [NẾU `i` là số chẵn\`]".

### Tại sao nên dùng?

  * **Ngắn gọn và Dễ đọc (hơn):** Khi đã quen, bạn sẽ thấy cú pháp này mô tả "ý định" (intent) rõ ràng hơn là một vòng lặp `for` dài dòng.
  * **Hiệu suất:** Trong hầu hết các trường hợp, List Comprehension chạy nhanh hơn một chút so với việc dùng vòng lặp `for` và `append()`, vì Python tối ưu hóa nó ở mức C.

Tuy nhiên, hãy cẩn thận: Đừng lạm dụng nó. Nếu logic của bạn quá phức tạp (ví dụ: lồng 2-3 vòng `for` hoặc có `if-else` phức tạp), một vòng lặp `for` thông thường sẽ dễ đọc và dễ bảo trì hơn\!

```
```