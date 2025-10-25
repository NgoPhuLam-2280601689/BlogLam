---
title: "Các lệnh Git cơ bản sinh viên CNTT phải biết"
date: 2025-10-22T19:00:00+07:00
draft: false
description: "Đừng code chay nữa! Hiểu rõ Git là gì và học cách dùng các lệnh cơ bản (commit, push, pull, branch) để quản lý code như một lập trình viên chuyên nghiệp."
tags: ["Git", "Version Control", "GitHub", "Hướng dẫn"]
categories: ["Công cụ"]
---

Nếu bạn là lập trình viên, việc bạn không biết dùng Git cũng giống như một nhà văn không biết cách dùng "Undo" (Hoàn tác) hay "Save As..." (Lưu thành bản khác).

Git là một **Hệ thống Quản lý Phiên bản Phân tán** (Distributed Version Control System).

Nói đơn giản: Git là một cỗ máy thời gian cho code của bạn.

* Nó cho phép bạn lưu lại các "ảnh chụp" (snapshot) của dự án tại những thời điểm quan trọng.
* Nếu bạn làm hỏng thứ gì đó, bạn có thể quay trở lại "ảnh chụp" trước đó.
* Nó cho phép nhiều người cùng làm việc trên một dự án mà không "dẫm chân" lên code của nhau.

Bài viết này sẽ hướng dẫn bạn những lệnh Git cơ bản nhất mà bạn sẽ dùng HÀNG NGÀY.

### Bước 0: Cấu hình "Danh tính"

Trước khi làm bất cứ điều gì, bạn phải giới thiệu cho Git biết bạn là ai. Git sẽ dùng thông tin này để "đóng dấu" lên mỗi thay đổi (commit) mà bạn tạo ra.

Mở terminal (PowerShell, Git Bash...) và gõ:

```bash
# Thay bằng tên của bạn
git config --global user.name "Nguyen Van An"

# Thay bằng email bạn dùng trên GitHub
git config --global user.email "an.nguyen@email.com"
````

Bạn chỉ cần làm việc này **một lần duy nhất** trên máy của mình.

### Bước 1: Khởi tạo "Kho chứa" (Repository)

Đầu tiên, bạn cần có một dự án. Giả sử bạn có một thư mục tên `do-an-cuoi-ky`.

Để "biến" thư mục này thành một kho chứa Git, hãy `cd` vào thư mục đó và gõ:

```bash
git init
```

Lệnh này sẽ tạo ra một thư mục con ẩn tên là `.git`. Đây là "bộ não" của Git, chứa toàn bộ lịch sử và các "ảnh chụp" của bạn. Bạn không bao giờ nên tự ý chỉnh sửa file trong thư mục này.

### Bước 2: "Ba bước thần thánh" (Status, Add, Commit)

Đây là quy trình bạn sẽ lặp đi lặp lại nhiều nhất khi làm việc với Git.

#### 2.1. `git status`: Kiểm tra tình hình

Đây là lệnh "bác sĩ" của bạn. Bất cứ khi nào bạn không chắc mình đang làm gì, hãy gõ:

```bash
git status
```

Git sẽ cho bạn biết:

  * Những file nào mới được tạo (untracked).
  * Những file nào đã bị sửa (modified).
  * Những file nào đã sẵn sàng để "chụp ảnh" (staged).

#### 2.2. `git add`: Đưa vào "Phòng chờ" (Staging Area)

Giả sử bạn vừa tạo 2 file mới là `index.html` và `style.css`. Nếu bạn gõ `git status`, chúng sẽ xuất hiện màu đỏ (untracked).

Trước khi "chụp ảnh", bạn phải chọn những file nào bạn muốn đưa vào bức ảnh đó. Đây gọi là đưa file vào "phòng chờ" (Staging Area).

```bash
# Thêm một file cụ thể
git add index.html

# Thêm tất cả các file đã thay đổi trong thư mục hiện tại
git add .
```

Tại sao lại cần "phòng chờ"? Nó cho phép bạn nhóm các thay đổi lại. Ví dụ, bạn sửa 10 file, nhưng 5 file đầu là để sửa lỗi (bug), 5 file sau là để thêm tính năng (feature). Bạn có thể `add` 5 file đầu và `commit` (chụp ảnh) với tin nhắn "Sửa lỗi A", sau đó `add` 5 file sau và `commit` với tin nhắn "Thêm tính năng B".

#### 2.3. `git commit`: "Chụp ảnh" (Lưu lại)

Khi các file đã ở trong "phòng chờ", bạn đã sẵn sàng để lưu chúng vào lịch sử. Đây là bước "chụp ảnh".

```bash
git commit -m "Initial commit: Thêm file HTML và CSS cơ bản"
```

  * `commit` là hành động "chụp ảnh".
  * `-m` là viết tắt của "message" (tin nhắn).
  * Nội dung trong dấu ngoặc kép là **bắt buộc**. Đây là lời ghi chú để bạn (và đồng đội) biết "bức ảnh" này đã thay đổi những gì. **Hãy tập viết commit message rõ ràng\!**

### Bước 3: Đẩy code lên "Đám mây" (GitHub)

Hiện tại, toàn bộ lịch sử code (các commit) mới chỉ nằm trên máy tính của bạn. Nếu máy tính bị hỏng, bạn sẽ mất tất cả.

Chúng ta cần "sao lưu" nó lên một nơi gọi là "remote repository" (kho chứa từ xa), ví dụ như GitHub.

Giả sử bạn đã lên GitHub.com và tạo một kho chứa (repository) rỗng tên là `do-an-cuoi-ky`.

#### 3.1. `git remote add`: Kết nối với "Nhà"

Bạn cần cho Git ở máy bạn biết "địa chỉ nhà" trên GitHub.

```bash
# Copy-paste đường link .git từ trang repo GitHub của bạn
git remote add origin [https://github.com/TenCuaBan/do-an-cuoi-ky.git](https://github.com/TenCuaBan/do-an-cuoi-ky.git)
```

  * `remote add`: Thêm một địa chỉ từ xa.
  * `origin`: Đây là **tên gọi tắt** (biệt danh) mặc định cho "nhà" của bạn. Bạn có thể đặt tên khác, nhưng 99% mọi người đều dùng `origin`.

#### 3.2. `git push`: Đẩy "Ảnh chụp" lên nhà

Bây giờ, hãy đẩy toàn bộ "album ảnh" (các commit) của bạn từ máy tính lên `origin` (GitHub).

```bash
git push -u origin main
```

  * `push`: Đẩy code đi.
  * `origin`: Đẩy đi đâu? Đẩy lên "nhà" (địa chỉ chúng ta vừa thêm).
  * `main`: Đẩy nhánh nào? Đẩy nhánh tên là `main` (trước đây thường gọi là `master`).
  * `-u`: Là một lệnh "cài đặt" để lần sau bạn chỉ cần gõ `git push` là Git tự hiểu bạn muốn đẩy lên `origin main`.

### Bước 4: Lấy code từ "Nhà" về (Làm việc nhóm)

Giả sử đồng đội của bạn cũng `push` một thay đổi lên GitHub. Làm sao để bạn cập nhật code mới nhất đó về máy?

```bash
git pull origin main
```

  * `pull`: Kéo code về.
  * `origin main`: Kéo từ nhánh `main` trên `origin` (GitHub).

Lệnh này sẽ tự động tải code mới về và "trộn" (merge) vào code hiện tại của bạn.

### Tóm tắt (Những lệnh bạn sẽ dùng 90% thời gian)

1.  **`git status`**: Xem có gì mới.
2.  **`git add .`**: Thêm tất cả thay đổi vào phòng chờ.
3.  **`git commit -m "Tin nhắn"`**: Lưu lại thay đổi với một tin nhắn.
4.  **`git pull`**: Lấy code mới nhất từ đồng đội về.
5.  **`git push`**: Đẩy code của mình lên cho đồng đội.

Đây chỉ là những bước đầu tiên, nhưng cũng là nền tảng quan trọng nhất để làm việc với Git. Chúc bạn code vui\!

```
```