# Zenith Update — Android 0.3.1-alpha

[Tải APK](https://github.com/huy15912/client-update/releases/download/v0.3.1-alpha/zenith-update-v0.3.1-alpha-debug.apk) · [Release/checksum](https://github.com/huy15912/client-update/releases/tag/v0.3.1-alpha) · [Hướng dẫn cloud phone](CLOUD_PHONE_TEST.md)

Android 10+, package `com.hsn.autoupdate`, versionCode **4**. Cài đè 0.3.0; giữ UUID/token, liên kết và instance. Không gỡ app hoặc reset danh tính để nâng cấp.

## Sửa trong bản này

- Cầu nối đổi từ cổng 1010 sang **10110**, cho chọn cổng cao khác khi bị chiếm. Cổng thực tế được ghi vào cấu hình bridge của mod.
- Bind cổng lỗi không còn tắt heartbeat/nhận job. App hiển thị riêng lỗi cầu nối và heartbeat gần nhất.
- **Mở app tự bật theo dõi**, sau liên kết heartbeat mỗi 10 giây và duy trì foreground service khi ở nền. Công tắc tạm dừng đến lần mở app tiếp theo. Force-stop/mất mạng vẫn có thể làm gián đoạn.
- Nút xác nhận chỉ bật khi job thực sự chờ đóng game; thêm retry report mất response/HTTP stream ngắt.

App có hai tab Tổng quan/Log, quản lý nhiều instance SAF, tải/kiểm hash, cài/xóa mod theo Fabric ID, config/options, backup/rollback và nhật ký gián đoạn.

**Bản hoàn thiện phải tự xử lý job mà không mở UG để xác nhận.** Bản alpha hiện vẫn cần **Thử thủ công khi chưa có mod cầu nối** và xác nhận game đã đóng; mod tắt game/xác minh tiến trình chưa hoàn thiện. Không tự bỏ qua kiểm tra an toàn rồi ghi vào game đang chạy.

## Kiểm tra

Build/lint đạt (0 lỗi, 2 cảnh báo), chữ ký APK v2 hợp lệ. Android 10 emulator: **61 kiểm tra nền**, **65 với SAF**, **69 với HTTP**. Đã thử service tự khởi động từ Activity, bind cổng cao, cổng bận/EACCES, nhận và cài job khi bridge lỗi, retry response thất lạc và hai instance fixture. HTTP dùng server Python/SQLite tạm, server nhận `succeeded`, `3/3`. Server code không đổi trong bản vá; 27 bài server đã đạt ở bản trước.

**Chưa xác nhận bản 0.3.1 trên cloud phone/Amethyst thật.** Lỗi 0.3.0 đã được ghi nhận trên UG Phone. Chưa có mod tắt game thật, Windows EXE hoặc kiểm chứng mọi tình huống force-stop. Sau nâng cấp, đợi heartbeat thành công rồi tạo lại job nếu job trước đã hết cửa sổ 60 giây.

SHA-256 APK:

```
694faf9ac3cdd68d97d306c9c1ce5540c970b3a857651aa11338d2d0333f28d3
```

Chỉ đăng APK/checksum/tài liệu. Source Android/server giữ trong project local; không có token, key ký hoặc config server trong payload. [Release 0.3.0](https://github.com/huy15912/client-update/releases/tag/v0.3.0-alpha) và [SAF probe 0.2](https://github.com/huy15912/client-update/releases/tag/v0.2-probe) được giữ nguyên. Mỗi version có Release riêng.
