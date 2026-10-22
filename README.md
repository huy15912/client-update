# Zenith Update — Android 0.4.1-alpha

[Tải APK](https://github.com/huy15912/client-update/releases/download/v0.4.1-alpha/zenith-update-v0.4.1-alpha-debug.apk) · [Release](https://github.com/huy15912/client-update/releases/tag/v0.4.1-alpha) · [Hướng dẫn](CLOUD_PHONE_TEST.md)

Android 10+, package com.hsn.autoupdate, versionCode **7**. Cài đè bản cũ giữ UUID/token/instance. APK local: build/zenith-update-v0.4.1-alpha-debug.apk.

## Giao diện mới

Thanh ngang cố định phía trên có **6 tab**, vuốt ngang để tới các mục phía sau:

| Tab | Nội dung |
| --- | --- |
| Tổng quan | Trạng thái, heartbeat/cầu nối, số instance, job gián đoạn và xác nhận thử thủ công |
| Tài khoản | UUID, liên kết, đổi tên và quản lý danh tính |
| Instance | Thêm/kiểm tra/bỏ thư mục game, nhãn và trạng thái mod |
| Mod phụ thuộc | Tải/cài helper từ server và hướng dẫn thiết lập |
| Cài đặt | Theo dõi, cổng cầu nối, backup, chế độ thử thủ công, dọn dữ liệu cũ |
| Log | Lọc, làm mới, sao chép/xuất nhật ký |

Nội dung cuộn dọc riêng, tab đang chọn tự được đưa vào vùng nhìn thấy. Giữ vị trí cuộn từng tab khi chuyển/refresh, giữ tab/bộ lọc/vị trí khi Activity được tạo lại.

Bản này sắp xếp lại giao diện. Helper vẫn **Zenith Game Bridge 0.1.0**, giao thức 2, Minecraft 1.21.11 / Java 21 / Fabric Loader >=0.19.3. JAR vẫn giao local để owner tự đăng. Không cần cài lại helper chỉ vì nâng APK 0.4.0 lên 0.4.1.

## Cập nhật tự động

Thiết lập lần đầu theo CLOUD_PHONE_TEST.md: đóng game, tắt theo dõi trong Cài đặt, sang Mod phụ thuộc để tải/cài helper. Bật theo dõi và mở game mỗi instance một lần đến khi tab Instance hiện Đã kết nối.

Tạo job → app tải/kiểm hash → mod báo trước 5 giây và yêu cầu thoát bình thường → app chờ khóa OS được nhả → backup/cài/đọc lại/báo kết quả. Không mở UG xác nhận từng job. [Giao thức](BRIDGE_PROTOCOL.md).

## Kiểm chứng

**0.4.1:** build/lint 0 lỗi, 2 cảnh báo; chữ ký APK v2 hợp lệ; **106 kiểm tra Android** gồm chuyển đủ 6 tab, đúng nhóm nội dung, tab được chọn/nhìn thấy và các kiểm thử nền hiện có. Đã xem ảnh thật, thử vuốt ngang trên emulator Android 10.

**Đã kiểm ở 0.4.0:** 100 kiểm tra với SAF/khóa giữa PID-UID khác; 109 với HTTP thủ công 3 tài nguyên; 122 với HTTP tự động 11 tài nguyên trên hai instance giả; mod có 29 kiểm tra socket JVM. Các ca HTTP/SAF mở rộng này không chạy lại cho thay đổi giao diện 0.4.1. Không chạy JAR snapshot hay chạm DB/instance sản xuất.

Helper mới vẫn chưa được xác nhận thoát/lưu thế giới trên Minecraft/UG Phone thật. Provider không hỗ trợ khóa thì app báo lỗi. Không tự mở launcher; không mở game lại khi app đang cài.

APK SHA-256:

    8910c0f96b5adfec7235993b253db233dd9a6d4303e8e230c598ab4409aba174

## Build local

JAVA_HOME=E:\Android studio\jbr, ANDROID_HOME=E:\Android\Sdk, GRADLE_USER_HOME=E:\code mod java\auto update\verison 2.0\android-saf-probe\.gradle-user.

    .\gradlew.bat :app:assembleDebug :app:assembleDebugAndroidTest :app:lintDebug --offline --console=plain
    adb shell am instrument -w com.hsn.autoupdate.test/com.hsn.autoupdate.CheckRunner

Chỉ đăng APK/checksum/tài liệu. Source, token/config cá nhân, dữ liệu server và khóa ký giữ local; không đăng test APK. Mỗi version có Release riêng, giữ nguyên bản cũ.
