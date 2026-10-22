# Thử Zenith Update 0.4.0-alpha trên cloud phone

Android 10+, package com.hsn.autoupdate, versionCode **6**. Cài đè bản cũ để giữ UUID/token/instance. Icon mới đã tách nền trắng. Helper: **Zenith Game Bridge 0.1.0**, Minecraft **1.21.11**, Java **21**, Fabric Loader **>=0.19.3**; không cần Fabric API.

## Thiết lập một lần

1. Owner đăng **zenith-game-bridge-0.1.0.jar** vào tab **Mod phụ thuộc** của dashboard. JAR được giao local trong client mod/build, người dùng tự đăng. GitHub chỉ có APK/tài liệu.
2. Cài đè APK mới, kiểm UUID/tài khoản/instance. Chọn gốc instance có mods/ và options.txt, giữ quyền SAF và bật backup.
3. **Đóng toàn bộ game**, tắt Theo dõi cập nhật, bấm **Tải và cài mod phụ thuộc từ server**. App kiểm hash/metadata, backup/cài JAR và cấu hình riêng từng instance. Cài xong tự tắt thử thủ công.
4. Bật theo dõi, mở Minecraft từng instance một lần. App phải hiện **Đã kết nối**, log có **Mod đã kết nối và xác thực khóa**. Lần bắt tay đầu chứng minh SAF nhìn thấy đúng file/khóa do game giữ. Không sao chép hoặc upload/chia sẻ zenith-bridge.json.
5. Mặc định cổng 10110. Cổng bận thì tắt theo dõi, chọn cổng cao khác, cài lại helper/config khi game đóng rồi bắt tay lại. Không cần liên kết lại UUID.

## Thử tự động

Tắt **Thử thủ công (bỏ qua tự động tắt game)**, giữ theo dõi ở nền rồi tạo job nhỏ trên dashboard lúc game đang chạy.

- App tải/kiểm hash, yêu cầu đúng instance thoát.
- Mod báo tiếng Việt, chờ 5 giây rồi yêu cầu Minecraft thoát bình thường.
- App chờ game nhả khóa, backup/cài/đọc lại file rồi báo succeeded. **Không cần mở UG để bấm xác nhận.**
- Không mở lại game khi đang cài. Bản này không tự mở lại launcher.
- Sau lần bắt tay đầu, game đóng sẵn vẫn cập nhật được nếu marker còn đúng session và app lấy được khóa.

Kiểm tiếp job hỗn hợp nhiều JAR/config/options, hai instance, xóa mod, cập nhật khi game đóng và thay helper rồi bắt tay lại. Một instance không đạt thì cả job lỗi, không tự bỏ qua. Bản vá alias/nested JAR của 0.3.2 vẫn giữ.

## Đọc lỗi

- Owner chưa đăng JAR: đăng helper trước.
- Mở game xác thực khóa lần đầu: bật theo dõi và mở game đến khi Đã kết nối.
- Không quan sát được khóa/provider không hỗ trợ: xuất log; app chưa được phép tự động cài trên provider đó.
- Game mất cầu nối nhưng còn giữ khóa: app chờ tối đa 5 phút rồi lỗi, không dùng xác nhận thủ công trong luồng tự động.
- Chưa cài đúng mod phụ thuộc: dùng nút riêng khi game đóng. Job thường không được sửa/xóa helper, cấu hình hay khóa.
- Journal gián đoạn: tắt theo dõi, đóng game, xem file có thể đã sửa rồi Khôi phục. Backup tắt/hỏng thì tự xử lý trước khi xác nhận kết thúc job lỗi.
- Android force-stop/khởi động lại có thể dừng app. Mở app để bật lại theo dõi. Thiết bị mất mạng/app bị dừng thì không nhận được job.

Gửi log kèm Android/launcher version và provider/URI. Không gửi token, state.json hoặc zenith-bridge.json.

## Đã kiểm

Build/lint 0 lỗi, 2 cảnh báo; APK v2 hợp lệ. 93 kiểm tra Android nền; 100 với SAF và khóa giữa UID khác; 109 với HTTP thủ công 3 tài nguyên; 122 với HTTP tự động 11 tài nguyên, hai instance giả. Mod có 29 kiểm tra socket JVM. Chưa xác nhận thoát/lưu thế giới trên Minecraft/UG Phone thật.
