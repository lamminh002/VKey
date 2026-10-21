# VKey v4.3.1

Bản cập nhật sửa lỗi cho cả Sciter và Classic, thay thế v4.3.0 đã thu hồi.

### Sửa lỗi

- Sửa lỗi báo cập nhật thành công dù cài đặt thất bại.
- Sửa lỗi VKey tự mở lại ngay sau khi chọn **Thoát**.
- Sửa lỗi không xác minh được engine tải về khi bật **Kiểm tra chính tả nâng cao**.
- Bổ sung chữ ký gói cập nhật để Sciter và Classic tiếp tục cập nhật tự động.

### Lưu ý khi nâng cấp

**Từ v4.2 hoặc v4.3.0: cần cài đặt thủ công một lần.** Trình cập nhật cũ yêu cầu chữ ký SignPath nên không thể tự cài bản này. Xem [hướng dẫn nâng cấp](https://github.com/phatMT97/VKey#ký-số-và-cập-nhật-thủ-công).

Sau khi cài v4.3.1, các lần cập nhật tiếp theo sẽ được xác minh bằng chữ ký gói của dự án và cài đặt tự động.

### Chữ ký số

**Cả Sciter và Classic đều chưa được ký Authenticode.** SignPath không chấp thuận ký bản Sciter vì Sciter là thành phần mã nguồn đóng. Phương án ký riêng Classic đang chờ phê duyệt lại.

Windows SmartScreen hoặc phần mềm diệt virus có thể cảnh báo hoặc chặn ứng dụng. Chữ ký gói cập nhật không thay thế Authenticode.

### VirusTotal

Kết quả quét cho Sciter, Classic và DLL engine sẽ được bổ sung trước khi phát hành.

VKey sử dụng hook bàn phím và gửi phím tổng hợp để xử lý tiếng Việt; các hành vi này có thể bị bộ quét nhận nhầm. Đây là khả năng giải thích cảnh báo, chưa phải kết luận từ các hãng antivirus. Xem [hướng dẫn kiểm tra bản tải](https://github.com/phatMT97/VKey/blob/Main/docs/ANTIVIRUS.md).

---

[Xem các thay đổi của VKey v4.3](https://github.com/phatMT97/VKey/blob/Main/docs/releases/v4.3.0.md).

---

*Cảm ơn bạn đã tin tưởng và lựa chọn VKey! Mọi đóng góp của bạn đều là nguồn động lực lớn giúp bộ gõ ngày càng hoàn thiện hơn.* ❤️
