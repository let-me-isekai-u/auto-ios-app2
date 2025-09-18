# Environment Variables cho Codemagic

Danh sách các biến môi trường cần thiết để cấu hình Codemagic build cho Auto Setup iOS App.

## 🔧 Biến môi trường cơ bản

### App Configuration
```bash
# Tên ứng dụng
APP_NAME=AutoSetupApp

# Bundle identifier (phải unique)
BUNDLE_ID=com.autosetup.AutoSetupApp

# Xcode project file
XCODE_WORKSPACE=AutoSetupApp.xcodeproj

# Xcode scheme name
XCODE_SCHEME=AutoSetupApp
```

### Build Configuration
```bash
# Build configuration (Debug/Release)
BUILD_CONFIGURATION=Release

# Export method (development/ad-hoc/app-store/enterprise)
EXPORT_METHOD=development

# iOS deployment target
IOS_VERSION=17.0

# Xcode version
XCODE_VERSION=15.0
```

## 🔐 Biến môi trường cho Production Build

### Apple Developer Account
```bash
# App Store Connect API Key
APP_STORE_CONNECT_ISSUER_ID=your-issuer-id
APP_STORE_CONNECT_KEY_IDENTIFIER=your-key-id
APP_STORE_CONNECT_PRIVATE_KEY=your-private-key

# Code Signing Certificate
CERTIFICATE_PRIVATE_KEY=your-certificate-private-key
CERTIFICATE_PASSPHRASE=your-certificate-passphrase
```

### Team Configuration
```bash
# Apple Developer Team ID
TEAM_ID=your-team-id

# Provisioning Profile
PROVISIONING_PROFILE=your-provisioning-profile-name
```

## 📱 Biến môi trường cho Jailbreak Build

### Không cần Code Signing
```bash
# Disable code signing for jailbreak builds
CODE_SIGN_IDENTITY=""
CODE_SIGNING_REQUIRED=NO
CODE_SIGNING_ALLOWED=NO
```

## 🚀 Cách thiết lập trong Codemagic

### Bước 1: Vào Environment Variables
1. Mở Codemagic dashboard
2. Chọn application `auto-ios-app2`
3. Vào tab **"Environment variables"**

### Bước 2: Thêm biến môi trường
1. Click **"Add variable"**
2. Điền **Variable name** và **Value**
3. Chọn **Variable group** (nếu có)
4. Click **"Add"**

### Bước 3: Cấu hình theo loại build

#### Jailbreak Build (Khuyến nghị cho testing):
```bash
APP_NAME=AutoSetupApp
BUNDLE_ID=com.autosetup.AutoSetupApp
XCODE_WORKSPACE=AutoSetupApp.xcodeproj
XCODE_SCHEME=AutoSetupApp
BUILD_CONFIGURATION=Release
EXPORT_METHOD=development
IOS_VERSION=17.0
XCODE_VERSION=15.0
CODE_SIGN_IDENTITY=""
CODE_SIGNING_REQUIRED=NO
CODE_SIGNING_ALLOWED=NO
```

#### Production Build (Cho App Store):
```bash
APP_NAME=AutoSetupApp
BUNDLE_ID=com.yourcompany.AutoSetupApp
XCODE_WORKSPACE=AutoSetupApp.xcodeproj
XCODE_SCHEME=AutoSetupApp
BUILD_CONFIGURATION=Release
EXPORT_METHOD=app-store
IOS_VERSION=17.0
XCODE_VERSION=15.0
APP_STORE_CONNECT_ISSUER_ID=your-issuer-id
APP_STORE_CONNECT_KEY_IDENTIFIER=your-key-id
APP_STORE_CONNECT_PRIVATE_KEY=your-private-key
CERTIFICATE_PRIVATE_KEY=your-certificate-private-key
CERTIFICATE_PASSPHRASE=your-certificate-passphrase
TEAM_ID=your-team-id
```

## 🔑 Cách lấy Apple Developer Credentials

### App Store Connect API Key:
1. Vào https://developer.apple.com/account
2. **Certificates, Identifiers & Profiles**
3. **Keys** → **App Store Connect API**
4. Click **"+"** để tạo key mới
5. Chọn **Access** (App Manager hoặc Admin)
6. Download `.p8` file
7. Lưu **Key ID** và **Issuer ID**

### Code Signing Certificate:
1. **Certificates** → **Development** hoặc **Distribution**
2. Click **"+"** để tạo certificate mới
3. Upload **Certificate Signing Request** (.csr file)
4. Download certificate (.cer file)
5. Convert sang .p12 format với private key

### Provisioning Profile:
1. **Profiles** → **Development** hoặc **Distribution**
2. Click **"+"** để tạo profile mới
3. Chọn **App ID** và **Certificates**
4. Chọn **Devices** (cho Development)
5. Download profile (.mobileprovision file)

## 📋 Checklist thiết lập

### Jailbreak Build:
- [ ] APP_NAME
- [ ] BUNDLE_ID
- [ ] XCODE_WORKSPACE
- [ ] XCODE_SCHEME
- [ ] BUILD_CONFIGURATION
- [ ] EXPORT_METHOD
- [ ] IOS_VERSION
- [ ] XCODE_VERSION
- [ ] CODE_SIGN_IDENTITY (empty)
- [ ] CODE_SIGNING_REQUIRED (NO)
- [ ] CODE_SIGNING_ALLOWED (NO)

### Production Build:
- [ ] Tất cả biến jailbreak build
- [ ] APP_STORE_CONNECT_ISSUER_ID
- [ ] APP_STORE_CONNECT_KEY_IDENTIFIER
- [ ] APP_STORE_CONNECT_PRIVATE_KEY
- [ ] CERTIFICATE_PRIVATE_KEY
- [ ] CERTIFICATE_PASSPHRASE
- [ ] TEAM_ID
- [ ] PROVISIONING_PROFILE

## ⚠️ Lưu ý quan trọng

### Security:
- **Không commit** credentials vào repository
- **Sử dụng** Codemagic environment variables
- **Encrypt** sensitive data
- **Rotate** keys định kỳ

### Bundle ID:
- **Phải unique** trên App Store
- **Format**: com.company.appname
- **Không được** trùng với app khác
- **Thay đổi** nếu cần thiết

### Xcode Version:
- **Compatible** với iOS version
- **Latest stable** version khuyến nghị
- **Check** compatibility matrix

## 🐛 Troubleshooting

### Build fails với "Code signing error":
1. Check **CERTIFICATE_PRIVATE_KEY** format
2. Verify **CERTIFICATE_PASSPHRASE** đúng
3. Check **TEAM_ID** valid
4. Verify **PROVISIONING_PROFILE** match

### Build fails với "Bundle ID error":
1. Check **BUNDLE_ID** format
2. Verify **BUNDLE_ID** unique
3. Check **PROVISIONING_PROFILE** match bundle ID

### Build fails với "Xcode version error":
1. Check **XCODE_VERSION** available
2. Verify **IOS_VERSION** compatible
3. Update **XCODE_VERSION** nếu cần

## 📞 Support

Nếu gặp vấn đề với environment variables:
1. Check **Codemagic documentation**
2. Verify **Apple Developer Account** status
3. Check **Build logs** trong Codemagic
4. Contact **Codemagic support**

---

**Happy Configuring! 🔧**
