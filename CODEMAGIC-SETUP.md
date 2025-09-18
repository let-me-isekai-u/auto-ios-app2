# Hướng dẫn sử dụng Codemagic để Build IPA

Hướng dẫn chi tiết cách sử dụng Codemagic CI/CD để build file IPA cho Auto Setup iOS App từ Windows.

## 🎯 Tổng quan

Codemagic là một CI/CD service chuyên dụng cho mobile apps, cho phép bạn:
- Build iOS apps từ Windows/Linux/Mac
- Tự động hóa quá trình build và test
- Tạo IPA files cho jailbreak và production
- Upload lên App Store Connect hoặc TestFlight

## 📋 Yêu cầu

### Tài khoản Codemagic:
- **Free tier**: 500 build minutes/tháng
- **Paid plans**: Từ $95/tháng cho unlimited builds
- **Sign up**: https://codemagic.io/signup

### Repository:
- Code đã được push lên GitHub/GitLab/Bitbucket
- File `codemagic.yaml` đã được cấu hình

## 🚀 Bước 1: Thiết lập Codemagic

### 1.1 Đăng ký tài khoản
1. Vào https://codemagic.io/signup
2. Đăng nhập bằng GitHub/GitLab/Bitbucket
3. Chọn plan phù hợp (Free tier đủ cho testing)

### 1.2 Kết nối Repository
1. Trong Codemagic dashboard, click **"Add application"**
2. Chọn repository `auto-ios-app2`
3. Codemagic sẽ tự động detect file `codemagic.yaml`

### 1.3 Cấu hình Environment Variables
Trong Codemagic dashboard, vào **"Environment variables"** và thêm:

```
# App Configuration
APP_NAME = AutoSetupApp
BUNDLE_ID = com.autosetup.AutoSetupApp
XCODE_WORKSPACE = AutoSetupApp.xcodeproj
XCODE_SCHEME = AutoSetupApp

# Build Configuration  
BUILD_CONFIGURATION = Release
EXPORT_METHOD = development

# iOS Configuration
IOS_VERSION = 12.0
XCODE_VERSION = 14.0
```

## 🔧 Bước 2: Cấu hình Build

### 2.1 Jailbreak Build (Khuyến nghị cho testing)
File `codemagic.yaml` đã được cấu hình để build cho jailbreak:
- Không cần code signing
- Có thể install trên jailbreak device
- Phù hợp cho testing workflow

### 2.2 Production Build (Cho App Store)
Để build cho production, cần cấu hình thêm:

#### Apple Developer Account:
```
APP_STORE_CONNECT_ISSUER_ID = [Your Issuer ID]
APP_STORE_CONNECT_KEY_IDENTIFIER = [Your Key ID]  
APP_STORE_CONNECT_PRIVATE_KEY = [Your Private Key]
CERTIFICATE_PRIVATE_KEY = [Your Certificate]
CERTIFICATE_PASSPHRASE = [Your Passphrase]
```

#### Cách lấy Apple Developer credentials:
1. Vào https://developer.apple.com/account
2. **Certificates, Identifiers & Profiles**
3. Tạo **App Store Connect API Key**
4. Download certificate và private key

## 🏃‍♂️ Bước 3: Chạy Build

### 3.1 Manual Build
1. Vào Codemagic dashboard
2. Chọn application `auto-ios-app2`
3. Click **"Start new build"**
4. Chọn branch `main`
5. Click **"Start build"**

### 3.2 Automatic Build
Build sẽ tự động chạy khi:
- Push code lên repository
- Tạo pull request
- Merge vào main branch

### 3.3 Monitor Build Progress
- Xem real-time logs
- Monitor build status
- Download artifacts khi hoàn thành

## 📱 Bước 4: Download IPA

### 4.1 Sau khi build thành công:
1. Vào **"Artifacts"** tab
2. Download file `AutoSetupApp.ipa`
3. File size khoảng 5-15MB

### 4.2 Transfer to Device:
#### Cách 1: AirDrop (Khuyến nghị)
1. AirDrop IPA từ Mac sang iPhone
2. Mở file trên iPhone
3. Install bằng Filza/Sileo

#### Cách 2: USB Transfer
1. Copy IPA vào iPhone qua iTunes/Finder
2. Mở Filza → On My iPhone
3. Tap IPA file → Install

#### Cách 3: Cloud Storage
1. Upload IPA lên Google Drive/iCloud
2. Download trên iPhone
3. Install bằng Filza

## 🧪 Bước 5: Test trên Jailbreak Device

### 5.1 Install App:
1. Mở **Filza** trên jailbreak device
2. Navigate đến thư mục chứa IPA
3. Tap `AutoSetupApp.ipa`
4. Chọn **Install**
5. Chờ install hoàn tất

### 5.2 Test Workflow:
1. **Launch app** → Cho phép notifications
2. **Settings → General → Language & Region → Region → Japan**
3. **Restart app** → Tiếp tục setup
4. **General → Date & Time → Tắt auto timezone → Tokyo**
5. **Privacy → Location Services → Tắt location**
6. **Verify** setup hoàn thành

### 5.3 Test Checklist:
- [ ] App launch successfully
- [ ] Main Settings screen hiển thị đúng
- [ ] Navigation smooth giữa các màn hình
- [ ] Region selection hoạt động
- [ ] Date/Time settings hoạt động
- [ ] Location Services toggle hoạt động
- [ ] Notifications hiển thị đúng
- [ ] Auto setup flow hoàn thành
- [ ] App không crash
- [ ] Performance ổn định

## 🔄 Bước 6: Iterate và Improve

### 6.1 Nếu có lỗi:
1. Check build logs trong Codemagic
2. Fix code issues
3. Push changes lên repository
4. Re-run build

### 6.2 Nếu test thành công:
1. Ghi lại test results
2. Có thể proceed với production build
3. Hoặc gửi source cho Mac user

## 📊 Build Configuration Options

### Jailbreak Build (Current):
```yaml
CODE_SIGN_IDENTITY: ""
CODE_SIGNING_REQUIRED: NO
CODE_SIGNING_ALLOWED: NO
EXPORT_METHOD: development
```

### Production Build:
```yaml
CODE_SIGN_IDENTITY: "Apple Development"
CODE_SIGNING_REQUIRED: YES
CODE_SIGNING_ALLOWED: YES
EXPORT_METHOD: app-store
```

### Ad Hoc Build:
```yaml
EXPORT_METHOD: ad-hoc
```

## 💰 Pricing và Limits

### Free Tier:
- 500 build minutes/tháng
- 1 concurrent build
- Public repositories only
- Community support

### Paid Plans:
- **Starter**: $95/tháng - 1,000 minutes
- **Professional**: $195/tháng - 2,500 minutes  
- **Business**: $395/tháng - 5,000 minutes

## 🐛 Troubleshooting

### Build Fails:
1. **Check logs** trong Codemagic dashboard
2. **Verify** file `codemagic.yaml` syntax
3. **Check** environment variables
4. **Ensure** repository có đầy đủ files

### IPA không install được:
1. **Check** iOS version compatibility
2. **Verify** jailbreak device compatibility
3. **Try** re-download IPA file
4. **Check** available storage space

### App crash:
1. **Check** device logs
2. **Verify** iOS version >= 15.0
3. **Test** trên device khác
4. **Check** memory usage

## 📞 Support

### Codemagic Support:
- **Documentation**: https://docs.codemagic.io/
- **Community**: https://discord.gg/codemagic
- **Email**: support@codemagic.io

### Project Support:
- **GitHub Issues**: Tạo issue trong repository
- **Documentation**: Đọc README.md và các file hướng dẫn

## 🎉 Kết luận

Với Codemagic, bạn có thể:
- ✅ Build iOS apps từ Windows
- ✅ Tự động hóa build process
- ✅ Test trên jailbreak device
- ✅ Iterate nhanh chóng
- ✅ Chuẩn bị cho production build

**Happy Building! 🚀**
