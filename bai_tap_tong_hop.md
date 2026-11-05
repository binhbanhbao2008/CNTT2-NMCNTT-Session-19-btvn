


# 1. Mục tiêu
- Nhận diện Phishing và mã độc Crack.
- Sử dụng Bitwarden và xác thực 2FA.
- Bảo vệ Secrets bằng `.env` và `.gitignore`.
- Kiểm tra an toàn trước khi Push GitHub.

# PHẦN 1: NHẬN DIỆN RỦI RO

### Câu 1: Email giả mạo GitHub
- **Loại tấn công:** Phishing kết hợp Typosquatting.
- **Dấu hiệu:** Tên miền `githuh-security.com` giả mạo GitHub, không phải `github.com`.
- **Cách xử lý:** Không nhấn link, không nhập mật khẩu, kiểm tra trực tiếp trên GitHub.

## Câu 2: Phần mềm Crack
- **Cạm bẫy:** Mã độc ngụy trang phần mềm Crack.
- **Nguy hiểm:** Tắt Windows Defender làm mất lớp bảo vệ, tạo điều kiện cho Trojan và Infostealer hoạt động.
- **Cách xử lý:** Không tải Crack, sử dụng phần mềm chính hãng.

## Câu 3: Quy tắc 5 KHÔNG
1. Không mở liên kết đáng ngờ.
2. Không tải phần mềm không rõ nguồn gốc.
3. Không cung cấp mật khẩu hoặc OTP.
4. Không tắt phần mềm bảo mật.
5. Không bỏ qua cảnh báo an ninh.

**Áp dụng:**
- Tình huống 1: Không mở link, không cung cấp mật khẩu.
- Tình huống 2: Không tải Crack, không tắt Defender.

## Câu 4: Quy trình cấp cứu 4 bước
1. **Cô lập:** Ngắt Internet, ngừng sử dụng máy nhiễm mã độc.
2. **Báo cáo:** Thông báo ngay cho trưởng nhóm hoặc bộ phận IT.
3. **Xử lý:** Quét, loại bỏ mã độc hoặc cài lại hệ điều hành.
4. **Khôi phục:** Dùng thiết bị sạch để đổi mật khẩu, thu hồi phiên đăng nhập, bật 2FA và kiểm tra tài khoản.

# PHẦN 2: PASSWORD MANAGER

## Câu 1: Nguy cơ dùng chung mật khẩu
- Email bị lộ mật khẩu có thể khiến GitHub và Vercel bị chiếm quyền.
- **Kiểu tấn công:** Credential Stuffing.
- **Giải pháp:** Mỗi tài khoản dùng mật khẩu riêng biệt.

## Câu 2: Zero-Knowledge
- Bitwarden mã hóa két sắt bằng AES-256.
- Dữ liệu được giải mã tại thiết bị người dùng.
- Máy chủ bị hack không thể trực tiếp đọc mật khẩu nếu khóa giải mã vẫn an toàn.

## Câu 3: Password Generator

| Thông số | Giá trị |
|---|---|
| Độ dài | 24 ký tự |
| Chữ hoa A–Z | Bật |
| Chữ thường a–z | Bật |
| Chữ số 0–9 | Bật |
| Ký tự đặc biệt | Bật |

**Cách làm:** Bitwarden → Generator → Thiết lập → Generate.

Tạo 3 mật khẩu khác nhau cho GitHub, Vercel và Email nhóm.

# PHẦN 3: KÍCH HOẠT 2FA GITHUB

## Câu 1: Vì sao chọn TOTP?
**Hai nhược điểm của SMS OTP:**
1. Dễ bị SIM Swapping chiếm đoạt số điện thoại.
2. Phụ thuộc sóng di động, có nguy cơ bị nghe lén SS7.

**Giải pháp:** Dùng Google Authenticator sinh mã TOTP, hoạt động Offline.

## Câu 2: Các bước kích hoạt
1. GitHub → Settings.
2. Chọn Password and authentication.
3. Chọn Enable two-factor authentication.
4. Mở Google Authenticator → Quét QR.
5. Lấy mã OTP 6 số.
6. Nhập mã vào GitHub để xác nhận.
7. Lưu Recovery Codes và hoàn tất.

**Lưu ý:** Mỗi thành viên bật 2FA cho tài khoản cá nhân; chủ GitHub Organization yêu cầu thành viên sử dụng 2FA.

## Câu 3: Backup Codes
- **Mục đích:** Khôi phục tài khoản khi mất điện thoại.
- **Nên lưu:** Bitwarden được bảo vệ an toàn hoặc bản in trong két sắt.
- **Không nên lưu:** GitHub công khai, ảnh chụp không mã hóa hoặc tin nhắn nhóm.

*Đề giả định 10 mã; số lượng thực tế phụ thuộc dịch vụ.*

# PHẦN 4: BẢO VỆ SECRETS

## Câu 1: Hai Secrets bị Hardcode
- `API_KEY`: Khóa API xuất hiện trực tiếp trong code.
- `DB_PASSWORD`: Mật khẩu cơ sở dữ liệu xuất hiện trực tiếp trong code.

**Giải pháp:** Chuyển sang biến môi trường và thay thế các Secrets đã lộ.

## Câu 2: Sửa file config.js

```javascript
const API_KEY = process.env.API_KEY;
const DB_PASSWORD = process.env.DB_PASSWORD;

function connectDatabase() {
  return connect(DB_PASSWORD);
}
```

## Câu 3: Nội dung 3 file

**File `.env` (mẫu minh họa, không chứa Secret thật):**

```dotenv
API_KEY=THAY_BANG_API_KEY_MOI
DB_PASSWORD=THAY_BANG_MAT_KHAU_MOI
```

**File `.gitignore`:**

```gitignore
.env
.env.*
!.env.example
node_modules/
```

**File `.env.example`:**

```dotenv
API_KEY=
DB_PASSWORD=
```

**Cách làm:**
1. Tạo file `.env` và điền Secrets mới trên máy cá nhân.
2. Tạo `.gitignore` để Git bỏ qua `.env`.
3. Tạo `.env.example` chỉ chứa tên biến.
4. Chạy Node.js với `node --env-file=.env config.js` để nạp biến môi trường

## Câu 4: Quy tắc 4 bước trước khi Push

1. **Kiểm tra code:** Không còn API Key hoặc Password hardcode.
2. **Kiểm tra `.gitignore`:** Đảm bảo `.env` không được theo dõi.
3. **Kiểm tra Git:** Dùng `git status` và `git diff --cached` kiểm tra dữ liệu chuẩn bị Commit.
4. **Kiểm tra lịch sử:** Đảm bảo Secrets cũ không nằm trong Commit; nếu từng bị lộ, phải thu hồi hoặc đổi ngay trước khi Push.

**Lệnh kiểm tra:**

```bash
git status
git check-ignore .env
git diff --cached
```



