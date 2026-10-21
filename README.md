# SAF Instance Probe

Prototype Android kiểm tra quyền truy cập thư mục instance bằng Storage Access Framework (SAF). Đây **chưa phải** ứng dụng cập nhật mod.

## Tải APK

- Bản hiện tại: **0.2-probe** (`versionCode` **2**)
- File: [`build/saf-instance-probe-v0.2-probe-debug.apk`](build/saf-instance-probe-v0.2-probe-debug.apk)
- SHA-256: `A6B301B4926ED1B1599F00EA0FD948D04C21CF7C3C91B6743F1F52C1BBF75A0F`

Mở link APK trên cloud phone, chọn **Download raw file** nếu GitHub hiện trang xem file, rồi mở APK để cài. Đây là bản **debug**; hệ điều hành có thể yêu cầu cho phép trình duyệt hoặc trình quản lý file cài ứng dụng từ nguồn này. Không cần Google Play.

## Kiểm tra trên cloud phone

1. Mở Amethyst và tạo/chạy một instance trước. Xác định thư mục gốc của instance có `options.txt` và `mods/`.
2. Mở SAF Instance Probe, chọn thư mục instance bằng trình chọn thư mục của Android và cấp quyền cho thư mục đó.
3. Đọc các dấu hiệu app hiển thị. Chỉ khi chắc chắn đó là instance cần thử, bấm phép thử trong `mods/`.
4. App tạo một file `.txt` riêng có tiền tố `hsn-saf-probe-`, đọc nó, đổi tên rồi xóa. Kiểm tra kết quả trên màn hình và xác nhận không còn file thử. Ghi lại phiên bản Android, phiên bản Amethyst, đường dẫn/URI được chọn và thông báo lỗi nếu có.

App không đụng tới file mod có sẵn. Dấu hiệu `options.txt` và `mods/` chỉ giúp nhận diện, chưa chứng minh đây là instance Amethyst. Bản này đã thử luồng SAF trên Android 10 emulator với thư mục giả; **chưa kiểm chứng trên Amethyst hoặc cloud phone**.

## Giới hạn quyền lưu trữ

SAF chỉ cấp quyền cho cây thư mục được người dùng chọn. Trình chọn thư mục hoặc storage provider có thể từ chối truy cập, đặc biệt ở vùng riêng của ứng dụng khác như `Android/data`; app không vượt qua giới hạn đó. `minSdk` và `targetSdk` đều là 29 (Android 10). Hành vi trên Android mới hơn có thể khác.

## Cấu trúc và phát hành bản tiếp theo

- `build/`: APK đã build để tải.
- `README.md`: phiên bản, checksum, cách cài và phạm vi kiểm chứng.

Mã nguồn Android đang nằm trong project riêng trên máy phát triển. Trước **mỗi bản phát hành mới**, tăng `versionCode` và `versionName` trong `app/build.gradle.kts` của project đó, chạy `gradlew.bat :app:assembleDebug`, kiểm tra metadata/chữ ký APK, rồi chép APK mới vào `build/` với version trong tên file. Cập nhật link và SHA-256 ở đây. Giữ APK cũ nếu cần cho người dùng so sánh hoặc tải lại.

APK debug được ký bằng debug key của máy build. Nếu sau này đổi máy hoặc đổi key, Android có thể không cho cài đè bản cũ; khi đó cần dùng cùng key hoặc gỡ bản cũ trước. Không đưa khóa ký riêng lên repo.
