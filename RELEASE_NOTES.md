# VKey v4.3.1

Phát hành cho cả Sciter và Classic, thay thế v4.3.0 đã thu hồi. Nội dung dưới đây bao gồm toàn bộ thay đổi của v4.3 và các bản sửa lỗi trong v4.3.1.

### Tính năng và cải tiến từ v4.3

#### VKey Browser — chuyển chế độ theo website (thử nghiệm)

- Thêm extension [VKey-Browser](https://github.com/phatMT97/VKey-Browser) và native host để chọn **Theo VKey**, **English** hoặc **TSF tương thích** theo từng tên miền.
- Tự áp dụng chế độ khi chuyển tab hoặc cửa sổ. Ví dụ, dùng tiếng Việt trên `google.com` và tiếng Anh trên `facebook.com` mà không thay đổi trạng thái V/E gốc.
- Có công tắc tạm dừng toàn bộ quy tắc và giữ lại cấu hình để dùng tiếp khi bật lại.
- Popup hiển thị trạng thái kết nối với native host và sử dụng icon VKey. Chromium và Firefox có gói manifest riêng, hỗ trợ Manifest V3.
- Chế độ TSF theo tên miền xử lý tình trạng dính chữ sau emoji hoặc ảnh trong editor/forum trên Firefox; composition hiển thị không gạch chân.
- Extension chỉ gửi hostname, loại trình duyệt, trạng thái focus và chế độ xử lý cục bộ; không gửi URL đầy đủ, nội dung trang hoặc phím gõ.
- Hiện cài bằng Developer mode / Load unpacked, chưa có trên Chrome Web Store hoặc Firefox AMO. Quy tắc được áp dụng sau khi điều hướng, không dựa trên nội dung đang nhập trong thanh địa chỉ.
- Xem [hướng dẫn cài đặt](https://github.com/phatMT97/VKey/blob/Main/docs/BROWSER_EXTENSION.md).

#### Kiểm tra chính tả nâng cao (Sciter)

- Bổ sung engine Rust để nhận diện và sửa một số lỗi gõ nhanh, đảo ký tự hoặc nhầm ký tự, ví dụ `hcaof` → `chào`, `xywr` → `xử`.
- Cải thiện nhận diện từ tiếng Anh và tên thương hiệu, chẳng hạn gõ `asus` bằng đúng thứ tự `a-s-u-s`, không cần thêm phím hoặc phục hồi từ.
- Tính năng mặc định tắt và chỉ tải engine khi người dùng chủ động bật. Nếu engine không có hoặc bị gỡ bỏ, VKey tiếp tục dùng engine C++ đi kèm cho các chức năng gõ thông thường.
- Classic chính thức sử dụng engine C++, không nạp engine Rust.
- Xem [demo](https://www.vkey.qd.je/#demo), [kiến trúc engine](https://github.com/phatMT97/VKey/blob/Main/docs/ENGINE_ARCHITECTURE.md) và [FAQ về giấy phép, nguồn gốc](https://github.com/phatMT97/VKey/blob/Main/docs/ENGINE_FAQ.md).

#### Giao diện Sciter

- Gom các tùy chọn icon vào tab **Hệ thống**.
- Sắp xếp lại tab **Macro**, tách các nút Lưu, Test, Xóa và Nhập; thêm ô chọn để xóa nhiều mục gõ tắt cùng lúc.
- Thêm hộp thoại quản lý **Từ điển cá nhân & Ngoại lệ chính tả**, hỗ trợ thêm, sửa, xóa, nhập/xuất `.txt` và lưu áp dụng ngay. Từ điển cá nhân được lưu trong `user_dictionary.txt`.
- Cải thiện hiệu năng hiển thị và chuyển đổi giao diện sáng/tối.

#### Chế độ game

- Chuyển cơ chế giữ phím cho game từ mặc định toàn cục sang tùy chọn theo từng ứng dụng. Các ứng dụng thông thường dùng cách xóa rồi thay ký tự để giảm nháy chữ và lỗi đặt dấu.
- Bật **Chế độ game** trong mục **Cách gửi** ở **Cấu hình từng ứng dụng**, hoặc gán phím tắt **Bật / tắt chế độ game cho app đang mở**. Nhấn lại phím tắt để gỡ ứng dụng khỏi chế độ game.
- Phím tắt này không có tổ hợp mặc định; người dùng tự chọn để tránh trùng phím trong game.
- Các cấu hình Cách gửi đã đặt riêng, như Clipboard, Firefox hoặc Cloud/Remote, được giữ nguyên.

#### Website

- Ra mắt [vkey.qd.je](https://www.vkey.qd.je/) để tra cứu tính năng, hướng dẫn sử dụng và tải bản phát hành.

### Các lỗi đã sửa từ v4.3

- Sửa lỗi bỏ dấu khi gõ `ruouwj` để tạo thành `rượu`.
- Sửa lỗi từ `view` bị chuyển thành `vieư`.
- Sửa lỗi nạp `user_dictionary.txt` do khác cú pháp ghi chú (`;` và `#`), khiến những từ đã lưu như `soà`, `khưm` vẫn bị engine nâng cao sửa lại.
- Sửa tình trạng ứng dụng chuyển sang trạng thái chờ quá sớm.
- Sửa lỗi mất từ trước đó khi dùng Shift để viết hoa tiếng Việt trong Excel Online.
- Sửa macro không hoạt động với TSF.
- Sửa lỗi đôi lúc gõ `khoong` không chuyển thành `không`, kể cả sau khi xóa và gõ lại.

### Sửa lỗi và cập nhật trong v4.3.1

- Sửa lỗi báo cập nhật thành công dù cài đặt thất bại.
- Sửa lỗi VKey tự mở lại ngay sau khi chọn **Thoát**.
- Sửa lỗi không xác minh được engine tải về khi bật **Kiểm tra chính tả nâng cao**.
- Bổ sung chữ ký gói cập nhật để Sciter và Classic tiếp tục cập nhật tự động.

### Lưu ý khi nâng cấp

**Từ v4.2 hoặc v4.3.0: cần cài đặt thủ công một lần.** Trình cập nhật cũ yêu cầu chữ ký SignPath nên không thể tự cài bản này. Xem [hướng dẫn nâng cấp](https://github.com/phatMT97/VKey#ký-số-và-cập-nhật-thủ-công). Nếu bản cũ tự mở lại liên tục, kết thúc cả `VKeyWatchdog.exe` và `VKey.exe` trong Task Manager trước khi cài bản mới.

Sau khi cài v4.3.1, các lần cập nhật tiếp theo sẽ được xác minh bằng chữ ký gói của dự án và cài đặt tự động.

### Chữ ký số

**Cả Sciter và Classic đều chưa được ký Authenticode.** SignPath không chấp thuận ký bản Sciter vì Sciter là thành phần mã nguồn đóng. Phương án ký riêng Classic đang chờ phê duyệt lại.

Windows SmartScreen hoặc phần mềm diệt virus có thể cảnh báo hoặc chặn ứng dụng. Chữ ký gói cập nhật không thay thế Authenticode.

### Kết quả VirusTotal (v4.3.1)

| Tệp / báo cáo VirusTotal | Engine báo độc hại | Đáng ngờ | Quét lúc (UTC) |
|---|---|---|---|
| [VKey.zip](https://www.virustotal.com/gui/file/5be8efb1738adcb0a199f0ee64d758e946700de1cf750ca03fd83b817f113363/detection) | 0 | 0 | 2026-10-21 12:42:34 |
| [VKey.exe](https://www.virustotal.com/gui/file/2b09dcbc9f8f8787523bd6c88141c82c8d90ef5cc1f759ed10579fc5bf295808/detection) | 1 | 0 | 2026-10-21 12:43:03 |
| [VKeyClassic.zip](https://www.virustotal.com/gui/file/33f8729bc8c641622ca266d43941cc94dc24a43d10c7a048e56a75d275f239fa/detection) | 1 | 0 | 2026-10-21 12:42:47 |
| [VKeyClassic.exe](https://www.virustotal.com/gui/file/6a134b5216c0d7e4bd445f91eb57dbd81edc4d7e4587a7835e53a28cb44457c2/detection) | 2 | 0 | 2026-10-21 12:43:19 |
| [vkey_engine.dll](https://www.virustotal.com/gui/file/d63cbe59e2f4998d802a7e1a5f2cc5ba0a51a1048491a3511016b9bbb5cb6397/detection) | 0 | 0 | 2026-10-21 12:43:35 |

VKey sử dụng hook bàn phím và gửi phím tổng hợp để xử lý tiếng Việt; các hành vi này có thể bị bộ quét nhận nhầm. Đây là khả năng giải thích cảnh báo, chưa phải kết luận từ các hãng antivirus. Xem [hướng dẫn kiểm tra bản tải](https://github.com/phatMT97/VKey/blob/Main/docs/ANTIVIRUS.md).

---

[Xem các thay đổi của VKey v4.3](https://github.com/phatMT97/VKey/blob/Main/docs/releases/v4.3.0.md).

---

*Cảm ơn bạn đã tin tưởng và lựa chọn VKey! Mọi đóng góp của bạn đều là nguồn động lực lớn giúp bộ gõ ngày càng hoàn thiện hơn.* ❤️
