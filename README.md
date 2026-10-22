# Zenith Update — Android 0.4.0-alpha

App cập nhật Android 10+, một UUID quản lý nhiều instance SAF. APK versionCode **6**, package **com.hsn.autoupdate**, minSdk/targetSdk 29, compileSdk 36. Cài đè bản cũ giữ danh tính và instance.

[Tải APK](https://github.com/huy15912/client-update/releases/download/v0.4.0-alpha/zenith-update-v0.4.0-alpha-debug.apk) · [Release](https://github.com/huy15912/client-update/releases/tag/v0.4.0-alpha) · [Thử cloud phone](CLOUD_PHONE_TEST.md)

APK local: build/zenith-update-v0.4.0-alpha-debug.apk. SHA-256:

    0e51494cfd579f7d58d4d3ebcf49b034cf8edb8dc3d6d5a12848e0b5d5855dd3

## Bản mới

- Icon app và mod dùng ảnh bánh răng/mũi tên đã tách nền trắng.
- Có mod **Zenith Game Bridge 0.1.0**, Minecraft 1.21.11 / Java 21 / Fabric Loader >=0.19.3, không cần Fabric API. JAR giao local ở client mod/build; owner tự đăng tab Mod phụ thuộc.
- App đã tải/cài helper từ server, kiểm hash/metadata và tạo config riêng từng instance. Cài helper thành công tự tắt chế độ thử thủ công.
- Luồng tự động đã nối: nhận job → tải/kiểm → yêu cầu game thoát → đợi khóa OS được nhả → backup/cài/đọc lại → báo kết quả. Không bắt xác nhận từng job. Mod thông báo trước 5 giây, gọi luồng thoát bình thường trên main thread.
- HMAC hai chiều, MAC từng shutdown ràng buộc nonce/session/job; app phải quan sát khóa thật qua SAF. ACK/socket ngắt chưa cấp quyền ghi. Giữ khóa qua cài/rollback. Job thường không được sửa helper, bridge config hoặc file khóa.

Sau cài helper, bật theo dõi và mở game mỗi instance một lần để app hiện **Đã kết nối**. Không lấy kết quả build làm bằng chứng đã chạy Minecraft thật. [Thiết lập chi tiết](CLOUD_PHONE_TEST.md), [protocol](BRIDGE_PROTOCOL.md).

## Các chức năng giữ lại

Mở app tự bật theo dõi, heartbeat mỗi 10 giây khi liên kết; lỗi cổng không tắt heartbeat. Enroll/liên kết/reset danh tính, nhiều instance SAF, JAR thêm/thay theo ID chính với alias/nested chuẩn Fabric, xóa mod theo ID, config lồng thư mục, options.txt. Cài từng file với journal trước ghi, backup/rollback, đọc lại hash; lỗi một instance thì cả job lỗi. Log lọc/sao chép/xuất, giới hạn dung lượng và ẩn bí mật. Thử thủ công vẫn là lựa chọn riêng để chẩn đoán, không phải luồng tự động.

## Kiểm thử 22/10/2026

- Build/lint: 0 lỗi, 2 cảnh báo; chữ ký APK v2 hợp lệ. Mod build/remap thành công.
- **93** kiểm tra Android nền; **100** khi thêm SAF thật. Có PID/UID khác giữ khóa bằng đường dẫn native, app mở qua SAF và chỉ lấy được sau khi PID chết.
- **109** với HTTP thủ công 3 tài nguyên, gồm cài hai instance, thay lại/rollback.
- **122** với HTTP tự động, snapshot 11 tài nguyên (4 config + 6 JAR + options) trên hai instance giả. ACK/socket đóng trước PID chết vẫn chưa ghi, không bấm xác nhận; server succeeded 11/11, hash đọc lại đúng.
- Mod: **29** kiểm tra socket JVM thật, HMAC vector, lệnh giả/sai instance/replay/mất kết nối và callback hợp lệ.
- Không chạy các JAR snapshot. Không sửa DB/job/instance sản xuất. Server không đổi code trong bản này.

Chưa chạy helper trên Minecraft/UG Phone thật. Cần xác nhận thông báo/thoát/lưu thế giới và khóa trên provider Amethyst. Provider thiếu khóa thì app lỗi rõ. Không tự mở lại launcher, không nhận job khi Android force-stop hoặc thiết bị mất mạng/nguồn. Không mở lại game trong lúc app cài; preLaunch chặn game khi khóa bận nhưng Fabric có thể đã đọc JAR trước đó.

## Build và kiểm tra

Dùng JAVA_HOME=E:\Android studio\jbr, ANDROID_HOME=E:\Android\Sdk, GRADLE_USER_HOME=E:\code mod java\auto update\verison 2.0\android-saf-probe\.gradle-user.

    .\gradlew.bat :app:assembleDebug :app:assembleDebugAndroidTest :app:lintDebug --offline --console=plain

Cài APK debug và androidTest vào emulator rồi chạy:

    adb shell am instrument -w com.hsn.autoupdate.test/com.hsn.autoupdate.CheckRunner
    python tests/run_android_http.py
    python tests/run_android_http.py --automatic --snapshot-job <UUID job local>

Snapshot chỉ đọc nguồn và sao chép vào server/SQLite/instance fixture riêng. SAF test chỉ nhận cây Download/zenith-fixture đã cấp quyền từ UI; APK test cần quyền READ/WRITE_EXTERNAL_STORAGE để PID thử mở native path. Quyền này chỉ nằm trong APK kiểm thử, không có trong APK phân phối. Không đưa test APK lên Release.

Chỉ đăng APK/checksum/tài liệu; source, token, config cá nhân, dữ liệu server và khóa ký giữ local. Mỗi bản có tag/Release riêng, giữ nguyên bản cũ.
