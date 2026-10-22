# Cầu nối app ↔ mod v2

Triển khai trong APK 0.4.0-alpha và Zenith Game Bridge 0.1.0 / Minecraft 1.21.11. Đã kiểm socket JVM và Android/SAF; thoát Minecraft cloud phone thật còn cần kiểm chứng.

## Kết nối

TCP chỉ dùng 127.0.0.1, mặc định 10110; cho đổi cổng 1024–65535. JSON UTF-8 mỗi dòng một gói, tối đa 8192 byte. App tối đa 8 kết nối, timeout bắt tay 15 giây và ping 45 giây. Mod ping khoảng 5 giây, timeout đọc 15 giây, reconnect backoff 1, 2, 4… tối đa 30 giây. Mất app/kết nối không tự tắt game.

App cài helper và ghi config/zenith-bridge.json: protocol=2, instance_id UUID ổn định, secret 64 hex ngẫu nhiên, host=127.0.0.1, port. Không dùng client token làm secret. Không upload/chia sẻ file cấu hình này.

HMAC-SHA256 dùng **chuỗi secret UTF-8 làm key**, không decode hex, kết quả hex chữ thường. Nonce 64 hex mới cho từng kết nối; session UUID mới mỗi lần khởi tạo mod.

## Khóa game

Fabric preLaunch giữ khóa độc quyền byte [0,1) trên config/zenith-update.lock. Channel/lock static tồn tại tới hết JVM; không nhả trong stopping/shutdown hook. Khóa bận/không hỗ trợ thì helper chặn khởi động và báo lỗi. Không xóa/thay file khóa.

Mod ghi marker bằng chính descriptor giữ khóa, gồm type=lease, protocol=2, instance_id, session và MAC của chuỗi dưới đây (\n là ký tự xuống dòng):

    lease\n<instance_id>\n<session>

Trong bắt tay, app đọc marker qua SAF, kiểm MAC và session trùng hello, rồi thử khóa cùng file. Chỉ chấp nhận khi **không lấy được khóa** do game giữ. Provider trả bản sao/inode không chia sẻ khóa bị từ chối. Session đã kiểm được lưu riêng theo instance. Sau cài/cài lại helper phải mở game và bật theo dõi để bắt tay lần đầu.

Job tự động chỉ lấy khóa khi socket đã ngắt, marker đúng session đã kiểm và OS cho lấy khóa độc quyền. Game còn sống nhưng mất cầu nối thì khóa vẫn bận: chờ tối đa 5 phút, sau đó lỗi mà chưa cài. Game đã đóng sẵn vẫn cập nhật được nếu session đã kiểm. App giữ mọi khóa qua backup/cài/đọc lại/rollback, nhả trong finally. Không hỗ trợ khóa thì lỗi, không tự đổi sang xác nhận thủ công.

Không mở/đóng descriptor khác tới cùng file khóa khi app đang giữ: POSIX có thể nhả khóa của tiến trình khi descriptor khác đóng. Không mở lại launcher trong lúc cài; preLaunch chặn game đi tiếp khi khóa bận, nhưng Fabric có thể đã đọc JAR trước bước này.

## Bắt tay và lệnh

1. App gửi challenge: type=challenge, protocol=2, nonce.
2. Mod gửi hello: type=hello, protocol=2, instance_id, mod_version, session, mac. MAC ký chuỗi nonce + "\n" + instance_id + "\n" + mod_version + "\n" + session.
3. App kiểm secret, phiên bản đúng bản helper đã cài, marker/khóa, từ chối kết nối trùng instance. Trả welcome: type=welcome, protocol=2, mac ký "server\n" + nonce + "\n" + instance_id.
4. Mod kiểm welcome trước khi nhận lệnh; gửi ping, app đáp pong.

App gửi shutdown: type=shutdown, protocol=2, command_id="<job UUID>:<instance UUID>", instance_id, delay_seconds=5, mac. MAC ký:

    shutdown\n<nonce>\n<session>\n<command_id>\n<instance_id>\n<delay_seconds dạng số nguyên>

Mod kiểm đủ trường/MAC, từ chối delay ngoài 3–30 giây và mã lệnh sai. Lệnh hợp lệ xử lý một lần; ACK kèm đúng command_id. App kiểm ACK thuộc lệnh đang chờ. MAC từng shutdown ngăn chương trình chiếm cổng/relay bắt tay chèn lệnh hoặc phát lại lệnh từ nonce cũ.

Mod hiện title/chat tiếng Việt, hoặc AlertScreen khi chưa vào thế giới, đợi 5 giây rồi gọi Minecraft.stop() trên main thread. Không kill/System.exit Minecraft. Socket đóng sau ACK nhưng khóa còn giữ tới hết JVM. **ACK/socket đóng không cấp quyền ghi.**

## Cài helper và thủ công

Cài helper là bước thiết lập khi mọi game đã đóng, theo dõi đã tắt. App kiểm hash/metadata, mod ID zenith-game-bridge và custom.zenith_bridge_protocol=2; backup/cài JAR và config. Nếu có file khóa cũ phải lấy được khóa trước khi thay helper. Cài xong tự tắt thử thủ công và xóa session cũ để bắt tay lại.

Thử thủ công là lựa chọn chẩn đoán riêng: từng job chờ người dùng đóng game/xác nhận và từ chối khi mod vẫn kết nối. Luồng tự động không dùng nút đó.
