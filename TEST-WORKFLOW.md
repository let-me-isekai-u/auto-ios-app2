# Hướng dẫn Test Workflow trên Máy Jailbreak

Hướng dẫn chi tiết cách test Auto Setup iOS App workflow trên máy jailbreak sau khi build IPA từ Codemagic.

## 🎯 Mục đích

Test workflow để đảm bảo:
- App hoạt động đúng trên jailbreak device
- UI/UX smooth và responsive
- Auto setup flow hoàn chỉnh
- Notifications hoạt động
- Không có crash hoặc memory leaks
- Chuẩn bị cho production build

## 📋 Yêu cầu

### Jailbreak Device:
- **iPhone/iPad** đã jailbreak (iOS 15.0+)
- **Filza** hoặc **Sileo** để install IPA
- **Storage space** ít nhất 100MB
- **Battery** đầy hoặc cắm sạc

### IPA File:
- File `AutoSetupApp.ipa` từ Codemagic build
- File size khoảng 5-15MB
- Build version mới nhất

## 🚀 Bước 1: Download IPA từ Codemagic

### 1.1 Vào Codemagic Dashboard:
1. Mở https://codemagic.io
2. Đăng nhập tài khoản
3. Chọn application `auto-ios-app2`
4. Vào tab **"Builds"**

### 1.2 Download IPA:
1. Chọn build thành công mới nhất
2. Vào tab **"Artifacts"**
3. Click download `AutoSetupApp.ipa`
4. Lưu vào thư mục dễ tìm

### 1.3 Verify IPA:
```bash
# Check file size (should be 5-15MB)
ls -lh AutoSetupApp.ipa

# Check file integrity
file AutoSetupApp.ipa
```

## 📱 Bước 2: Transfer IPA to Device

### Cách 1: AirDrop (Khuyến nghị)
1. **Mac**: AirDrop IPA sang iPhone
2. **iPhone**: Mở file → **Share** → **Save to Files**
3. **Lưu** vào thư mục Files app

### Cách 2: USB Transfer
1. **Kết nối** iPhone qua USB
2. **Mở** Finder (macOS) hoặc iTunes (Windows)
3. **Drag & drop** IPA vào device
4. **Sync** để transfer

### Cách 3: Cloud Storage
1. **Upload** IPA lên Google Drive/iCloud
2. **iPhone**: Download từ cloud
3. **Lưu** vào Files app

### Cách 4: SSH/SCP (Advanced)
```bash
# Copy via SSH (if SSH enabled)
scp AutoSetupApp.ipa root@[DEVICE_IP]:/var/mobile/Documents/
```

## 🔧 Bước 3: Install trên Jailbreak Device

### Sử dụng Filza (Khuyến nghị):
1. **Mở** Filza
2. **Navigate** đến thư mục chứa IPA
3. **Tap** `AutoSetupApp.ipa`
4. **Chọn** "Install"
5. **Chờ** install hoàn tất
6. **Verify** app xuất hiện trên home screen

### Sử dụng Sileo:
1. **Mở** Sileo
2. **Tap** "Sources" → "+"
3. **Add** IPA file path
4. **Install** từ source

### Sử dụng Cydia:
1. **Copy** IPA vào `/var/mobile/Documents/`
2. **Mở** Terminal trên device
3. **Chạy**: `dpkg -i AutoSetupApp.ipa`

## 🧪 Bước 4: Test Workflow

### 4.1 Launch App:
1. **Tap** app icon trên home screen
2. **Chờ** app launch (2-3 giây)
3. **Cho phép** notifications khi được hỏi
4. **Verify** main Settings screen hiển thị

### 4.2 Test Main Navigation:
- [ ] **Settings** → Main screen hiển thị đúng
- [ ] **General** → Navigate smooth
- [ ] **Language & Region** → Screen load nhanh
- [ ] **Region Selection** → List hiển thị đầy đủ
- [ ] **Back navigation** → Hoạt động đúng

### 4.3 Test Region Selection:
1. **Settings** → **General** → **Language & Region**
2. **Tap** "Region"
3. **Scroll** xuống tìm "Japan"
4. **Tap** "Japan"
5. **Verify** region được chọn
6. **Tap** "Back" để quay lại

### 4.4 Test App Restart Simulation:
1. **Force close** app (swipe up từ bottom)
2. **Reopen** app
3. **Verify** notification hiện "App restart required"
4. **Check** app state được lưu

### 4.5 Test Date & Time Settings:
1. **General** → **Date & Time**
2. **Toggle** "Set Automatically" OFF
3. **Tap** "Time Zone"
4. **Search** "Tokyo"
5. **Select** Tokyo timezone
6. **Verify** timezone được set

### 4.6 Test Location Services:
1. **Settings** → **Privacy**
2. **Tap** "Location Services"
3. **Toggle** Location Services OFF
4. **Verify** toggle hoạt động
5. **Check** app state được lưu

### 4.7 Test Notifications:
- [ ] **First launch**: Setup notification hiện
- [ ] **After region**: Restart notification hiện
- [ ] **Second launch**: Continue notification hiện
- [ ] **Completion**: Success notification hiện

## 📊 Bước 5: Performance Testing

### 5.1 Memory Usage:
1. **Mở** Settings → **General** → **iPhone Storage**
2. **Check** Auto Setup app size
3. **Monitor** memory usage trong app
4. **Verify** không có memory leaks

### 5.2 Battery Usage:
1. **Settings** → **Battery**
2. **Check** Auto Setup app battery usage
3. **Verify** usage bình thường
4. **Monitor** trong 30 phút sử dụng

### 5.3 Performance Metrics:
- **Launch time**: < 3 giây
- **Navigation**: Smooth, no lag
- **Memory usage**: < 50MB
- **Battery impact**: Low
- **Storage usage**: < 20MB

## 🐛 Bước 6: Bug Testing

### 6.1 Edge Cases:
- [ ] **Force close** app trong quá trình setup
- [ ] **Restart** device trong quá trình setup
- [ ] **Low memory** warning
- [ ] **Network** interruption
- [ ] **Background** app switching

### 6.2 Error Handling:
- [ ] **Invalid** region selection
- [ ] **Missing** timezone data
- [ ] **Permission** denied
- [ ] **Storage** full
- [ ] **Battery** low

### 6.3 Compatibility:
- [ ] **iOS version** compatibility
- [ ] **Device** compatibility
- [ ] **Jailbreak** compatibility
- [ ] **Storage** compatibility

## 📝 Bước 7: Test Results Documentation

### 7.1 Test Report Template:
```
Test Date: ___________
Device: iPhone ___ (iOS ___)
Jailbreak: _____________
IPA Version: ___________

✅ Working Features:
- [ ] App launch
- [ ] Main UI
- [ ] Navigation
- [ ] Region selection
- [ ] Date/Time settings
- [ ] Location services
- [ ] Notifications
- [ ] Auto setup flow
- [ ] Performance
- [ ] Memory usage

❌ Issues Found:
- [ ] Issue 1: Description
- [ ] Issue 2: Description

📊 Performance Metrics:
- Launch time: ___ seconds
- Memory usage: ___ MB
- Battery impact: Low/Medium/High
- Storage usage: ___ MB

📝 Notes:
- Overall experience: Good/Fair/Poor
- UI/UX: Good/Fair/Poor
- Stability: Good/Fair/Poor
- Recommendations: ___________
```

### 7.2 Screenshots:
- **Main screen** hiển thị
- **Region selection** screen
- **Date/Time** settings
- **Location services** toggle
- **Notifications** hiển thị
- **Completion** screen

## 🔄 Bước 8: Iterate và Improve

### 8.1 Nếu có lỗi:
1. **Document** lỗi chi tiết
2. **Take** screenshots
3. **Check** device logs
4. **Fix** code issues
5. **Push** changes lên repository
6. **Re-run** Codemagic build
7. **Re-test** trên device

### 8.2 Nếu test thành công:
1. **Document** test results
2. **Prepare** cho production build
3. **Share** results với team
4. **Proceed** với next steps

## 🚀 Bước 9: Next Steps

### 9.1 Nếu test thành công:
- **Proceed** với production build
- **Configure** Apple Developer Account
- **Setup** proper code signing
- **Build** cho App Store

### 9.2 Nếu cần improvements:
- **Fix** identified issues
- **Optimize** performance
- **Improve** UI/UX
- **Re-test** workflow

## ⚠️ Lưu ý quan trọng

### Security:
- **Không** share IPA file publicly
- **Không** install trên device không trust
- **Verify** IPA integrity trước khi install

### Legal:
- **Jailbreak** có thể void warranty
- **Test** chỉ cho mục đích development
- **Không** distribute IPA publicly

### Performance:
- **Monitor** device performance
- **Check** battery usage
- **Verify** storage space
- **Test** trên nhiều device nếu có thể

## 📞 Support

Nếu gặp vấn đề:
1. **Check** device logs
2. **Verify** iOS version compatibility
3. **Check** jailbreak compatibility
4. **Contact** development team
5. **Check** Codemagic build logs

---

**Happy Testing! 🧪**
