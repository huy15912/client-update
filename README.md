# Zenith Update — Android 0.3.2-alpha

[Tải APK](https://github.com/huy15912/client-update/releases/download/v0.3.2-alpha/zenith-update-v0.3.2-alpha-debug.apk) · [Release/checksum](https://github.com/huy15912/client-update/releases/tag/v0.3.2-alpha) · [Hướng dẫn](CLOUD_PHONE_TEST.md)

Android 10+, `com.hsn.autoupdate`, versionCode **5**. Cài đè 0.3.0/0.3.1, giữ UUID/token, liên kết và instance.

## Nguyên nhân

Job 11 tài nguyên trong video dừng ở `preflight`, `0/11`: app 0.3.1 gộp ID chính, alias và mod nhúng thành một nhóm rồi từ chối nếu có nhiều ID. Cloth Config 21.11.153 có ID chính `cloth-config`, alias `cloth-config2` và thư viện nhúng `cloth-basic-math`; đây là cấu trúc Fabric hợp lệ. Lỗi xảy ra trước bước cài, chưa thay/xóa file game của job này.

## Sửa

- Cho thêm/thay toàn bộ JAR ngoài theo ID chính, giữ nguyên alias và các thư viện/mod nhúng.
- Vẫn chặn ID/alias tranh chấp giữa nhiều JAR, đổi ID chính chỉ dựa vào alias và việc sửa/xóa riêng mod con bằng cách đụng cả JAR ngoài. Bảo vệ mod cầu nối vẫn giữ.
- Lỗi nêu rõ JAR/ID. Chỉ quét kho mod một lần trên mỗi instance trong mỗi lượt kiểm trước. Không hiện `null (null)` cho uploader chưa có dấu vết.
- Giữ sửa 0.3.1: cổng mặc định 10110, mở app tự bật heartbeat, lỗi bridge không tắt theo dõi.

## Kiểm tra

Build/lint đạt (0 lỗi, 2 cảnh báo), chữ ký v2 hợp lệ. Android 10 emulator: **78 kiểm tra nền**, **82 với SAF**, **110 với HTTP và bản sao đúng 11 tài nguyên của job lỗi**.

Nguồn snapshot được đọc ở chế độ chỉ đọc. Bản sao 4 config + 6 JAR + options chạy trên server Python/SQLite tạm và hai instance fixture: tải/cài/đọc lại SHA-256, server xác nhận **11/11, succeeded**. Kiểm thêm lượt thay các file đã có và rollback; có hồi quy alias/nested JAR, xung đột thật và bảo vệ mod con. Không chạy JAR hay chạm instance Minecraft thật. Tài nguyên/config cá nhân không nằm trong Release.

Chưa xác nhận bản mới trên UG Phone/Minecraft thật; kiểm hash không chứng minh tương thích giữa mọi mod. Server code không đổi; 27 bài server đã đạt ở bản trước.

## Thử

Cài đè, mở app, đợi heartbeat rồi tạo **job mới** với cùng 11 tài nguyên. Job cũ đã failed không tự chạy lại. Không cần bỏ Cloth Config. Đọc `CLOUD_PHONE_TEST.md`; backup mặc định bật.

Bản alpha vẫn dùng xác nhận game đã đóng khi thử thủ công. Tự động hoàn toàn không cần mở UG xác nhận là yêu cầu bản hoàn thiện, chưa được tuyên bố hoàn tất vì mod/kiểm chứng game dừng chưa xong.

APK debug, minSdk/targetSdk 29. SHA-256:

```
8b1b3cf248b489b41dd6337acfbe679a1e3505e1fe3db651bc6d430455f99699
```

[Fabric metadata: provides và JARs](https://docs.fabricmc.net/develop/loader/fabric-mod-json). Chỉ đăng APK/checksum/tài liệu; source, token, config cá nhân và khóa ký không được đăng. Các Release cũ được giữ nguyên.
