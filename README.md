# Bài Tập 4: Lưu Trữ Dữ Liệu Đơn Giản Với Docker Volume

## 1. Mục tiêu
- Tạo một Named Volume để lưu trữ dữ liệu độc lập với vòng đời của container.
- Chứng minh tính bền vững của dữ liệu: Container ghi dữ liệu rồi tự hủy (`--rm`), container mới gắn cùng volume vẫn đọc được dữ liệu nguyên vẹn.

---

## 2. Các bước thực hiện và Kết quả Terminal

### Bước 1: Tạo Named Volume `easy-volume`
Lệnh thực thi:
```bash
docker volume create easy-volume
```

Kết quả Terminal:
```text
easy-volume
```

Kiểm tra danh sách Volume:
```bash
docker volume ls
```
```text
DRIVER    VOLUME NAME
local     easy-volume
```

---

### Bước 2: Dùng container thứ nhất ghi dữ liệu vào volume
Container chạy image `alpine`, gắn volume `easy-volume` vào thư mục `/data`, ghi chuỗi `Luu tru du lieu Docker` vào tệp `/data/test.txt` và tự động hủy sau khi hoàn tất (`--rm`).

Lệnh thực thi:
```bash
docker run --rm -v easy-volume:/data alpine sh -c "echo 'Luu tru du lieu Docker' > /data/test.txt"
```

Kết quả Terminal:
```text
Unable to find image 'alpine:latest' locally
latest: Pulling from library/alpine
da9db072f522: Pull complete 
Digest: sha256:beefdbd8a1da6d5915ab2f63a0009b60ad097334791a11014110323396b27e77
Status: Downloaded newer image for alpine:latest
```
*(Container kết thúc tiến trình và tự giải phóng)*

---

### Bước 3: Dùng container thứ hai đọc dữ liệu từ volume (Kiểm tra)
Khởi chạy container `alpine` thứ hai, gắn cùng volume `easy-volume` vào `/data` để đọc tệp `/data/test.txt`.

Lệnh kiểm tra:
```bash
docker run --rm -v easy-volume:/data alpine cat /data/test.txt
```

Kết quả Terminal:
```text
Luu tru du lieu Docker
```

---

### Bước 4: Dọn dẹp volume (Tùy chọn)
Lệnh thực thi:
```bash
docker volume rm easy-volume
```

Kết quả Terminal:
```text
easy-volume
```

---

## 3. Kết luận
- Dữ liệu lưu trữ trong Docker Volume `easy-volume` tồn tại bền vững ngay cả khi container ghi dữ liệu đã dừng và bị xóa.
- Container thứ hai truy cập và đọc chính xác tệp `/data/test.txt`, xuất ra đúng dòng chữ mong đợi:
  > `Luu tru du lieu Docker`
