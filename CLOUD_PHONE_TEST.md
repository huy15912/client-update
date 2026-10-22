# Thử Zenith Update 0.3.1-alpha trên cloud phone

## Chuẩn bị

1. Server cần hỗ trợ cập nhật tiến độ trong cùng trạng thái (code đã sửa ở bản 0.3.0). Nếu đang dùng bản đó thì không cần đổi server cho bản vá 0.3.1; nếu chưa cập nhật thì cập nhật/restart, tránh `409` ở report. Giữ dashboard ở `dashboard.zenithautoupdate.xyz`, API app ở `zenithautoupdate.xyz`.
2. Tải APK `zenith-update-v0.3.1-alpha-debug.apk` từ Assets của [Release v0.3.1-alpha](https://github.com/huy15912/client-update/releases/tag/v0.3.1-alpha). Cài đè bản 0.3.0 rồi mở **Zenith Update**; giữ nguyên UUID/token, liên kết và instance. Không cần gỡ app hay reset danh tính. Package khác SAF probe nên có thể cài song song.
3. Dùng một instance thử hoặc bản sao instance trước. Bản này thực sự thay/xóa file được dashboard yêu cầu; backup mặc định bật.

## Liên kết

1. Bấm **Tạo danh tính lần đầu**, nhập tên máy. App hiện UUID và mã 8 ký tự có hạn 10 phút.
2. Vào dashboard bằng tài khoản còn hiệu lực, liên kết bằng mã đó. Trong app bấm **Làm mới tài khoản từ server**; phải thấy tên tài khoản và Discord ID đúng.
3. Mã hết hạn khi chưa liên kết: **Tạo mã liên kết mới**. Không reset danh tính chỉ để xin mã khác. Xin mới giới hạn 3 lần/giờ và cách nhau 60 giây.
4. Nếu token bị thu hồi hoặc reset mất response, app báo lỗi. Chỉ dùng nút chủ động tạo lại sau khi hiểu rằng UUID mới cần liên kết lại; app không lấy lại được token đã mất.

## Chọn thư mục

1. **Thêm instance bằng trình chọn thư mục** → chọn gốc instance có `mods/` và `options.txt` → cấp đọc/ghi → đặt nhãn.
2. App thử file tạm riêng trong `mods/` rồi xóa. Kiểm tra lại nếu quyền bị thu hồi. Có thể thêm instance thứ hai; tất cả instance đã thêm đều nhận cùng một job.
3. Bật **Sao lưu trước khi cập nhật**. Bật **Thử thủ công khi chưa có mod cầu nối** vì chưa có mod tắt game thật.
4. Mở app sẽ tự bật **Theo dõi cập nhật**. Android phải có thông báo tác vụ; app hiện heartbeat gần nhất và dashboard thấy client online. Giữ tác vụ chạy và mạng ổn định. Công tắc cho tạm dừng trong phiên; mở app lại sẽ tự bật. Job cũ đã quá cửa sổ 60 giây cần tạo lại sau khi client online.

## Job đầu tiên

1. Upload một config thử, ví dụ `zenith-test/settings.json`, rồi tạo job cho đúng client.
2. App tải/kiểm hash và chờ **đóng mọi game**. Tự thoát các game đang mở, trở về app, bấm **Tôi đã đóng tất cả game — tiếp tục job** và xác nhận. Chờ tối đa 5 phút; không mở lại game trong lúc cài.
3. App chụp backup cuối, cài/đọc lại file rồi báo thành công. Dashboard phải hiện `succeeded`, đủ tiến độ. Kiểm tra file ở `<instance>/config/zenith-test/settings.json`.
4. Thử tiếp `options.txt`, thêm/thay JAR có một Fabric mod ID, rồi xóa theo ID. Mod được thay theo ID dù tên JAR khác. Job xóa không tìm thấy mod phải thành công như tác vụ đã vắng mặt; nhiều JAR cùng ID phải báo lỗi.
5. Với hai instance, chỉ báo thành công nếu cả hai xong. Log phải ghi instance/file nào đã làm. Nếu instance thứ hai lỗi, kiểm tra log rollback instance trước đó.

## Khi có lỗi

- Mở tab **Log**, chọn nhóm Lỗi hoặc Tất cả, sao chép/xuất log. Gửi kèm Android version, launcher/version, provider/URI được chọn và bước lỗi. Không gửi `state.json`, `zenith-bridge.json` hoặc token.
- Có nhật ký gián đoạn: tắt theo dõi, đóng game, xem danh sách file có thể đã sửa rồi dùng **Khôi phục job gián đoạn**. Backup hỏng/tắt thì tự xử lý file; chỉ khi xong mới bấm **Đã sửa thủ công — kết thúc job lỗi**.
- `403`: kiểm tra tài khoản chưa liên kết/hết hạn. `409` ở heartbeat: hết slot. `409` ở report: kiểm tra code server đã cập nhật hoặc job đã bị hủy.
- Cầu nối mặc định dùng cổng 10110 (không dùng 1010). Nếu bị chiếm, tạm dừng theo dõi, chọn cổng khác từ 1024–65535 rồi bật lại; nếu đã cài mod thì cần ghi lại cấu hình qua nút cài mod khi game đã đóng. Lỗi cầu nối vẫn được hiển thị nhưng không tắt heartbeat; chế độ thử thủ công vẫn nhận job.
- Owner chưa đăng mod phụ thuộc: API trả `mod:null`; không phải lỗi mạng. Bản này vẫn thử cập nhật được bằng chế độ thủ công. Nút cài mod phụ thuộc không tự chứng minh mod đã chạy.

## Mục cần xác nhận từ máy thật

- Thêm/giữ quyền SAF sau khi mở lại app; nhiều instance có đúng thư mục.
- Liên kết/đổi tên/lấy tài khoản; app giữ UUID/token qua khởi động lại.
- Foreground service sống trong lúc chơi, nhận job trong 60 giây.
- JAR thêm/thay/xóa, config lồng thư mục, options, backup và tình huống lỗi instance thứ hai.
- Ghi log rõ khi mất mạng/app bị dừng; không báo thành công giả hoặc tự chạy lại thao tác cài.

Các bài test local hiện dùng emulator Android 10 và fixture, chưa xác nhận các mục trên với Minecraft thật. Cầu nối tắt game sẽ thử ở bước làm mod tiếp theo.
