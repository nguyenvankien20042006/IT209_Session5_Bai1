# Báo cáo BÀI 1: Khôi phục commit đã mất bằng Git Reflog

**Đường dẫn thư mục:** `homework/session_05/ex1/`

## 1. Bối cảnh sự cố

Trong quá trình phát triển dự án, sau khi thực hiện commit tính năng quan trọng, học viên đã vô tình chạy lệnh reset lùi lịch sử:

```
git reset --hard HEAD~1

```

Lệnh này khiến commit chứa mã nguồn vừa chỉnh sửa bị biến mất hoàn toàn khỏi lịch sử làm việc thông thường (`git log`) và làm sạch Working Directory.

## 2. Các bước thực hiện khôi phục

### Bước 1: Mô phỏng sự cố

1. Tạo file tính năng `feature.txt` và lưu vào Git:

   ```
   echo "Day la tinh nang quan trong" > feature.txt
   git add .
   git commit -m "Them tinh nang quan trong"
   
   ```

2. Giả lập thao tác lỡ tay xóa commit:

   ```
   git reset --hard HEAD~1
   
   ```

   *Kết quả:* Git thông báo `HEAD is now at c866422 Initial commit`. Commit tính năng đã biến mất khỏi `git log`.

### Bước 2: Tra cứu nhật ký Reflog

Sử dụng lệnh `git reflog` để truy vết lịch sử con trỏ `HEAD`:

```
git reflog

```

**Kết quả ghi nhận thực tế từ Terminal:**

```
c866422 (HEAD -> master) HEAD@{0}: reset: moving to HEAD~1
606b11c HEAD@{1}: commit: Them tinh nang quan trong
c866422 (HEAD -> master) HEAD@{2}: commit (initial): Initial commit

```

> **Phân tích:** Commit tính năng quan trọng đã bị mất có mã hash là **`606b11c`** (tại vị trí `HEAD@{1}`).

### Bước 3: Khôi phục Commit

Thực hiện kéo con trỏ `HEAD` quay lại vị trí commit đã bị lỡ tay xóa:

```
git reset --hard 606b11c

```

**Kết quả từ Terminal:**
`HEAD is now at 606b11c Them tinh nang quan trong`

## 3. Kiểm tra và Xác nhận kết quả

Chạy lệnh kiểm tra lại lịch sử commit:

```
git log --oneline

```

**Kết quả hiển thị:**

```
606b11c (HEAD -> master) Them tinh nang quan trong
c866422 Initial commit

```

**Kết luận:**

* Lịch sử `git log` đã được khôi phục đầy đủ.

* Commit `"Them tinh nang quan trong"` và file `feature.txt` đã quay trở lại làm việc bình thường trên nhánh `master`.