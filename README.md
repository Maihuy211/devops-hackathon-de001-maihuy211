# DevOps Hackathon - Đề 001: Quản lý phòng Lab

## 1. Thông tin sinh viên

| Họ và tên | Mã sinh viên | Lớp | Tài khoản Linux | GitHub | Cổng Nginx |
|---|---|---|---|---|---|
| Mai Văn Huy | PTIT-HN-091 | CNTT1 | `maihuy-cntt1` | [Maihuy211] | `8088` |

## 2. Môi trường triển khai

- Máy chủ: `devops-vps`
- Hệ điều hành: Ubuntu 24.04
- IP công khai: `221.121.4.78`
- Web server: Nginx
- Nơi chạy: VPS WiserVPS

## 3. Cấu trúc dự án

```text
devops-hackathon-de001-maihuy211/
├── src/index.html
├── nginx/maihuy-cntt1.conf
├── screenshots/
├── .gitignore
└── README.md
```


## 4. Cấu hình Nginx

| Tham số | Giá trị | Giải thích |
|---|---|---|
| `listen` | `8088` và `[::]:8088` | Cổng riêng cho website, hỗ trợ IPv4 và IPv6 |
| `server_name` | `221.121.4.78` | Địa chỉ IP máy chủ |
| `root` | `/var/www/devops-hackathon-de001-maihuy211/src` | Web root tuyệt đối |
| `index` | `index.html` | Trang mặc định |
| `allow` | `all` | Cho phép request từ bên ngoài |
| access log | `/var/log/nginx/maihuy-cntt1.access.log` | Log truy cập riêng |
| error log | `/var/log/nginx/maihuy-cntt1.error.log` | Log lỗi riêng |

Kích hoạt cấu hình:

```bash
sudo cp /var/www/devops-hackathon-de001-maihuy211/nginx/maihuy-cntt1.conf /etc/nginx/sites-available/maihuy-cntt1.conf
sudo ln -s /etc/nginx/sites-available/maihuy-cntt1.conf /etc/nginx/sites-enabled/maihuy-cntt1.conf
sudo unlink /etc/nginx/sites-enabled/default
sudo nginx -t
sudo systemctl reload nginx
```

## 5. Tường lửa UFW

```bash
sudo ufw allow 22/tcp
sudo ufw allow 8088/tcp
sudo ufw --force enable
sudo ufw status verbose
```

## 6. Quy trình triển khai

```bash
sudo apt update
sudo apt install -y nginx git ufw curl
sudo git clone https://github.com/Maihuy211/devops-hackathon-de001-maihuy211.git /var/www/devops-hackathon-de001-maihuy211
sudo chown -R maihuy-cntt1:maihuy-cntt1 /var/www/devops-hackathon-de001-maihuy211
sudo find /var/www/devops-hackathon-de001-maihuy211 -type d -exec chmod 755 {} \;
sudo find /var/www/devops-hackathon-de001-maihuy211 -type f -exec chmod 644 {} \;
```

Kiểm tra website:

```bash
curl -I http://127.0.0.1:8088
```

Truy cập từ trình duyệt:

```text
http://221.121.4.78:8088
```

## 7. Ảnh minh chứng

1. `01-user.png`: `id` và `whoami` của tài khoản `maihuy-cntt1`.
2. `02-nginx.png`: `nginx -t` thành công và Nginx active/enabled.
3. `03-ufw.png`: `sudo ufw status verbose` có port 22 và 8088.
4. `04-website.png`: trình duyệt mở `http://221.121.4.78:8088`.
5. `05-git-log.png`: `git log --oneline` có ít nhất 4 commit.
6. `06-update.png`: website sau khi cập nhật bằng `git pull`.

## 8. Quy trình cập nhật

```text
Sửa src/index.html -> git add -> git commit -> git push
-> git pull trên VPS -> tải lại trình duyệt -> kiểm tra nội dung mới
```

## 9. Sự cố và cách khắc phục

- Nếu không truy cập được port 8088, kiểm tra Nginx, UFW và firewall mạng của VPS.
- Nếu `nginx -t` lỗi, kiểm tra lại port, đường dẫn `root` và dấu chấm phẩy trong server block.
- Nếu `git pull` không chạy, kiểm tra repository GitHub đang ở chế độ public.


