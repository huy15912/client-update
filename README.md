# Zenith Update — Android client

App Android Auto Update 2.0, bản **0.3.0-alpha**, package `com.hsn.autoupdate`, versionCode **3**, Android 10+ (`minSdk/targetSdk 29`). Cài song song được với SAF Instance Probe cũ.

## Tải và thử

- [Release v0.3.0-alpha](https://github.com/huy15912/client-update/releases/tag/v0.3.0-alpha)
- [Tải APK trực tiếp](https://github.com/huy15912/client-update/releases/download/v0.3.0-alpha/zenith-update-v0.3.0-alpha-debug.apk)
- [Hướng dẫn thử trên cloud phone](CLOUD_PHONE_TEST.md)
- SHA-256: `BDCF6DBE262B80E4D01DA40233522867125EA5CCE1E6A6D7E928A18D877523EF`

APK debug, chữ ký v2 đã xác minh. Không cần Google Play. Chỉ APK/tài liệu phát hành được đăng ở nhánh này; source Android và code server giữ trong project local. Không có token, secret, key ký hoặc cấu hình server trong payload.

## Chức năng

Hai tab Tổng quan/Log; UUID/token và liên kết Discord qua dashboard; nhiều instance SAF; foreground service nhận job; tải/kiểm dung lượng và SHA-256; cài/xóa mod theo Fabric ID, config lồng thư mục và options; backup/rollback và journal trước khi ghi; xuất log, quản lý dữ liệu job.

**Chưa có mod tắt game thật.** Để thử bản app đầu, bật **Thử thủ công khi chưa có mod cầu nối**, đóng mọi game rồi xác nhận trong app ở mỗi job. Mặc định backup bật. App có listener/protocol cầu nối thử nghiệm nhưng chưa xác nhận với Minecraft.

Server cần code mới hỗ trợ report tiến độ cùng trạng thái và được restart sau cập nhật. App gọi `https://zenithautoupdate.xyz`; dashboard dùng `https://dashboard.zenithautoupdate.xyz`.

## Đã kiểm tra

- Build APK/app-test và lint đạt, 0 lỗi lint, 2 cảnh báo.
- Server 27/27 test HTTP/SQLite.
- Android 10 emulator: 51 kiểm tra cho lượt SAF và 51 kiểm tra cho lượt HTTP (47 kiểm tra nền dùng chung).
- SAF dùng quyền chọn qua DocumentsUI và Downloads DocumentsProvider thật, ghi/đọc lại/khôi phục config trên thư mục giả.
- HTTP gọi server Python thật với SQLite tạm, tải JAR/config/options và cài vào hai instance fixture; server nhận `succeeded`, `3/3`.

**Chưa xác nhận updater này trên Minecraft/Amethyst cloud phone thật.** Chưa thử mod thoát game, Windows EXE, mọi tình huống force-stop/provider hoặc mất response reset. Test fixture không thay thế kết quả máy thật.

## Bản cũ

[v0.2-probe](https://github.com/huy15912/client-update/releases/tag/v0.2-probe) là phép thử quyền file SAF, không phải updater. Giữ Release/APK đó để đối chiếu. Mỗi version tiếp theo có versionCode/versionName tăng và Release riêng.
