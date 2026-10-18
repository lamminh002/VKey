# VKey và phần mềm diệt virus (Antivirus false positives)

> **TL;DR:** VKey là bộ gõ mã nguồn mở. Ở v4.3, SignPath Foundation chỉ ký
> `VKeyClassic.exe`, `VKeyTSF.dll` và `VKeyWatchdog.exe` trong gói Classic;
> bản tiêu chuẩn Sciter và `VKeyBrowserHost.exe` không thuộc phạm vi này — hãy
> xác minh bản tải bằng **Sigstore attestation**, xem mục *Kiểm chứng* bên dưới.
> Một số phần mềm diệt
> virus (Kaspersky, Bitdefender, Avast…) đôi khi
> cảnh báo VKey theo **hành vi** (behavior detection) chứ không phải chữ ký virus —
> vì bộ gõ nào cũng phải hook bàn phím + gửi phím, trông "giống keylogger". Đây là
> **cảnh báo nhầm (false positive)**. Dưới đây là cách khôi phục và loại trừ.

---

## Vì sao bị cảnh báo?

Ký số (code signing) giúp Windows SmartScreen và các bộ quét **theo chữ ký** tin
tưởng file. Bản tiêu chuẩn Sciter v4.3 không có chữ ký nên khả năng bị cảnh báo
cao hơn; bản Classic có chữ ký vẫn có thể gặp phát hiện theo hành vi. Ngay cả khi
đã ký số, phần mềm diệt virus vẫn có lớp
**phát hiện theo hành vi** —
chấm điểm những gì tiến trình *làm khi chạy*, không quan tâm ai ký. Bộ gõ tiếng
Việt về bản chất phải:

- Cài **global keyboard hook** (đọc phím để bỏ dấu)
- **Gửi phím tổng hợp** (`SendInput`) để thay chữ
- **Tự khởi động cùng Windows** (autorun)
- Chạy **watchdog** để bật lại nếu bị tắt

Cụm hành vi này trùng khớp hồ sơ "keylogger" trong heuristic của AV → cảnh báo
nhầm. VKey **không** gửi phím của bạn đi đâu cả — toàn bộ xử lý là cục bộ, mã nguồn
công khai để kiểm chứng.

Từ bản này, VKey đã giảm bớt tín hiệu gây nhầm (watchdog không còn bật lại vô hạn
sau khi bị AV tắt; không ghi log nội dung phím ở bản Release; không ghi autorun
lặp lại).

## Kiểm chứng VKey là bản thật (khuyên dùng trước khi loại trừ)

Bản tiêu chuẩn Sciter v4.3 không có chữ ký Authenticode, nên cách xác minh là
**Sigstore attestation** — nó chứng minh file được build từ đúng mã nguồn trong
repo này:

```bash
gh attestation verify VKey.zip --repo PhatMT97/VKey
```

Nếu lệnh trên báo lỗi → **không** dùng file đó, tải lại từ trang phát hành chính thức.

> Với các bản **v4.0–v4.2** và ba binary Foundation của **Classic v4.3**, cách kiểm tra là:
> ```powershell
> (Get-AuthenticodeSignature "C:\path\to\VKeyClassic.exe").Status # → Valid
> (Get-AuthenticodeSignature "C:\path\to\VKeyClassic.exe").SignerCertificate.Subject
> # → CN=SignPath Foundation, O=SignPath Foundation, L=Lewes, S=Delaware, C=US
> ```
> Chạy lệnh này trên `VKey.exe` của bản tiêu chuẩn v4.3 sẽ trả về `NotSigned` —
> đó là điều bình thường với bản Sciter, không phải dấu hiệu file giả.

## Khôi phục khi VKey bị cách ly (quarantine)

### Kaspersky
1. Mở Kaspersky → **Bảo mật** (Security) → **Báo cáo / Cách ly** (Reports / Quarantine).
2. Tìm mục VKey.exe, chọn **Khôi phục** (Restore).
3. Thêm loại trừ: **Cài đặt → Mối đe dọa & Loại trừ → Quản lý loại trừ → Thêm** →
   chọn thư mục cài VKey (ví dụ `D:\Softwares\VKey`).
4. Nếu Kaspersky đã xóa cả khóa autorun: mở lại VKey → **Cài đặt → Khởi động cùng
   Windows** để bật lại.

### Windows Defender
1. **Bảo mật Windows → Bảo vệ khỏi virus & mối đe dọa → Lịch sử bảo vệ**.
2. Tìm mục VKey → **Tác vụ → Khôi phục**.
3. Loại trừ: **Cài đặt bảo vệ khỏi virus & mối đe dọa → Loại trừ → Thêm** → thư mục VKey.

### Chung (Bitdefender / Avast / AVG…)
Vào phần **Quarantine / Cách ly**, khôi phục VKey, rồi thêm **Exclusion / Ngoại lệ**
cho thư mục cài đặt.

## <span id="eset-browser">ESET: không gõ được tiếng Việt trong trình duyệt</span>

Nếu VKey vẫn gõ được trong Notepad/Word nhưng không đặt dấu trên Edge/Chrome, **Browser protection / Safe Banking & Browsing** của ESET có thể đang chặn bộ gõ. Trong [issue #229](https://github.com/phatMT97/VKey/issues/229), người dùng đã xác nhận thêm VKey vào **Browser protection allowlist** rồi khởi động lại trình duyệt giúp gõ tiếng Việt trở lại.

### 1. Kiểm tra tiện ích ESET và bảo vệ trình duyệt

- **Edge:** mở `edge://extensions`.
- **Chrome:** mở `chrome://extensions`.
- **Firefox:** mở `about:addons` → **Extensions** (Tiện ích mở rộng).

Tìm tiện ích có tên **ESET**, chẳng hạn **ESET Browser Privacy & Security** hoặc **ESET Browser Security**. Nếu có, mở ứng dụng ESET trên máy để kiểm tra **Browser protection / Safe Banking & Browsing**, đặc biệt là **Secure all browsers** (Bảo vệ tất cả trình duyệt) và **Keyboard protection** (Bảo vệ bàn phím). Tên và vị trí mục có thể khác tùy phiên bản ESET.

Không thấy tiện ích chưa có nghĩa là ESET không bảo vệ trình duyệt. Cửa sổ ẩn danh và việc chạy VKey bằng quyền Administrator cũng không bảo đảm tránh được lớp bảo vệ này.

### 2. Thêm VKey vào Browser protection allowlist

1. Xác định đúng thư mục VKey đang sử dụng. Có thể mở **Task Manager**, tìm tiến trình `VKey.exe` hoặc `VKeyClassic.exe`, nhấp chuột phải → **Open file location** (Mở vị trí tệp).
2. Trong ESET, mở **Advanced setup** (`F5`) → **Protections → Browser protection → Browser protection allowlist → Edit**.
3. Chọn **Add** và thêm từng file theo chế độ đang dùng:

   | Chế độ VKey | File cần thêm bằng đường dẫn đầy đủ |
   |---|---|
   | Hook (mặc định) | `VKey.exe`, hoặc `VKeyClassic.exe` nếu dùng Classic |
   | TSF | File `.exe` tương ứng ở trên và `VKeyTSF.dll` của bản đang dùng |

   Chỉ thêm `VKeyTSF.dll` chưa đủ cho chế độ Hook. Nếu ESET ghi nhận DLL bị chặn ở đường dẫn khác, hãy dùng đúng đường dẫn trong log.
4. Nhấn **OK / Apply** để lưu tất cả hộp thoại. **Thoát hoàn toàn trình duyệt**, kể cả tiến trình chạy nền, rồi mở lại và thử gõ, ví dụ `tieengs Vieetj` → `tiếng Việt` với Telex.

Đây là ngoại lệ trong **Browser protection allowlist** của ESET; danh sách loại trừ quét virus thông thường hoặc mục **Loại trừ ứng dụng** của VKey không thay thế thiết lập này. Bạn có thể giữ **Secure all browsers** bật sau khi thêm ngoại lệ.

### 3. Nếu vẫn chưa gõ được

- Kiểm tra lại đường dẫn, nhất là khi vừa cập nhật hoặc di chuyển VKey, và chắc chắn đã đóng hết tiến trình trình duyệt trước khi mở lại.
- Mở **Log files → Browser protection** trong ESET để tìm file VKey bị chặn. Với file VKey đã xác minh, nhấp chuột phải vào bản ghi để thêm vào allowlist nếu phiên bản ESET hỗ trợ.
- Để kiểm tra nguyên nhân, có thể **tạm tắt Secure all browsers** trong **Safe Banking & Browsing**, khởi động lại trình duyệt rồi thử gõ. Thao tác này giảm bảo vệ trình duyệt; hãy bật lại sau khi kiểm tra và hoàn tất ngoại lệ.

Tham khảo tài liệu ESET: [Browser protection allowlist](https://help.eset.com/essp/18/en-US/idh_config_sb_allowlist.html) và [Safe Banking & Browsing](https://help.eset.com/essp/18/en-US/idh_config_sb.html).

## Báo cáo cảnh báo nhầm (giúp mọi người dùng)

Gửi mẫu cho hãng AV để họ gỡ cảnh báo — vài ngày là có hiệu lực cho tất cả người dùng:

- **Kaspersky:** https://opentip.kaspersky.com/ (chọn *False detection* / gửi file).
- **Microsoft:** https://www.microsoft.com/wdsi/filesubmission
- **Bitdefender:** https://www.bitdefender.com/submit/
- **Avast/AVG:** https://www.avast.com/false-positive-file-form.php

Hoặc mở issue tại kho GitHub của VKey kèm **tên phát hiện** (ví dụ `PDM:Trojan.Win32…`)
và tên phần mềm AV — tác giả sẽ nộp allowlist giúp.

---

## English (short)

VKey is an open-source Vietnamese IME. In **v4.3 SignPath Foundation signing is
limited to `VKeyClassic.exe`, `VKeyTSF.dll`, and `VKeyWatchdog.exe`**; the
standard Sciter edition and `VKeyBrowserHost.exe` are outside that scope.
Some antivirus products flag it by **behavior detection**
(not signature) because every IME must hook the keyboard, synthesize keystrokes,
autostart, and self-supervise — a cluster that looks keylogger-shaped. It is a
**false positive**; VKey processes everything locally and never sends your
keystrokes anywhere. An unsigned build is more likely to trip these heuristics.

- **Verify authenticity:** `gh attestation verify VKey.zip --repo PhatMT97/VKey`
  (Sigstore build attestation). On v4.0–v4.2 and the three approved Classic v4.3
  binaries you can additionally check
  `(Get-AuthenticodeSignature VKeyClassic.exe).SignerCertificate.Subject` →
  `CN=SignPath Foundation, …`.
- **Restore + exclude:** open your AV's Quarantine, restore VKey, add the install
  folder to Exclusions. Re-enable "Run at startup" in VKey settings if the autorun
  key was removed.
- **Report the false positive:** Kaspersky OpenTIP, Microsoft WDSI, Bitdefender,
  Avast/AVG submission forms (links above), or file a GitHub issue with the exact
  detection name.
