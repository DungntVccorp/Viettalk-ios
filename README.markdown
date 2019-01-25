# Hướng dẫn tích hợp  Viettalk vào ứng dụng 
# Version 0.1

[![N|Solid](http://viettalk.vn/images/zoota-plus-logo.png)](https://nodesource.com/products/nsolid)

[![Build Status](https://travis-ci.org/joemccann/dillinger.svg?branch=master)](https://travis-ci.org/joemccann/dillinger)

Tài liệu hướng dẫn tích hợp chức năng chat và gọi điện  của  **VIETTALK** vào  ứng dụng.

# Chức năng chính!

- API kết nối đến hệ thống chat
- API kết nối tới hệ thống voip 
- Giao diện danh sách cuộc hội thoại
- Giao diện hội thoại chat đơn và nhóm
- giao diện nhận và trả lời cuộc gọi đến
- giao diện gọi đi

# SDK gồm có!
- **CoreDM.framework** ==> chứa kiến trúc và giao tiếp kết nối tới  server
- **ProtoModel.framework** ==> chứa định nghĩa model
- **VietTalkCore.framework** ==> Chứa api chat và báo hiệu
- **VTCallAPI.framework** ==> chứa những api Liên quan VOIP
- **ViettalkUI.framework** ==> chứa giao diện chat
- **ViettalkCallUI.framework** ==> chứa giao diện gọi điện

### Thư viện phụ thuộc bên thứ 3 kèm theo

| Third party | Link |
| ------ | ------ |
| SwiftLinkPreview | https://github.com/LeonardoCardoso/SwiftLinkPreview
| SnapKit | https://github.com/SnapKit/SnapKit
| Kingfisher | https://github.com/onevcat/Kingfisher
| SwipeCellKit | https://github.com/SwipeCellKit/SwipeCellKit
| CLImageEditorCarthage | https://github.com/DungntVccorp/CLImageEditorCarthage
| Pulsator | https://github.com/shu223/Pulsator
| SKPhotoBrowser | https://github.com/suzuki-0000/SKPhotoBrowser
| MBProgressHUD | https://github.com/jdg/MBProgressHUD
| TOCropViewController | https://github.com/TimOliver/TOCropViewController
| THContactPickerCart | https://github.com/DungntVccorp/THContactPickerCart
| Floaty | https://github.com/kciter/Floaty
| Google Map SDK | https://developers.google.com/maps/documentation/ios-sdk/intro
| Google Places SDK | https://developers.google.com/places/ios-sdk/intro

### Yêu cầu hệ thống

- iOS 9.0+ / Mac OS X 10.14+
- Xcode 10.1+
- Swift 4.2+
- C++11 
- armv7s arm64 arm64e
- iPhone 5s trở lên ( không hỗ trợ giao diện  cho Ipad)
- không hỗ trợ IOS đã bị **Jailbreak**
### Cài Đặt

- Kéo thả những framework sau vào **Linked Framworks And Libraries** trong **TARGETS** project
```sh
CoreDM.framework
ProtocolBuffers.framework
ProtoModel.framework
ViettalkCallUI.framework
VietTalkCore.framework
ViettalkUI.framework
VTCallAPI.framework
```
3nd Framework
```sh
CLImage.framework
ContactPickerFramework.framework
CropViewController.framework
Kingfisher.framework
MBProgressHUD.framework
Pulsator.framework
SKPhotoBrowser.framework
SnapKit.framework
SwiftLinkPreview.framework
SwipeCellKit.framework
TOCropViewController.framework
Floaty.framework
```
- Nếu sử dụng cho **Objective C** Project 
```sh
Always Embed Swift Standard Libraries = "YES"
```
ở trong **Build Settings**
- Cấu hình File Info.plist
```sh
viettalk_is_dev_mode = "YES / NO"
viettalk_token = "token_duoc_cung_cap_theo_Bundle_identifier"
NSCameraUsageDescription = "str_description"
NSLocationWhenInUseUsageDescription = "str_description"
NSMicrophoneUsageDescription = "str_description"
NSPhotoLibraryUsageDescription = "str_description"
```
chú ý token được cung cấp theo ID của ứng dụng 

- Cài đặt Google Map và Google Map Place theo hướng dẫn 

[GOOGLE MAP](https://developers.google.com/maps/documentation/ios-sdk/start)

[GOOGLE MAP PLACES](https://developers.google.com/places/ios-sdk/start#step-2-install-the-api)

### Sử dụng trên Project Objective C

### Sử dụng trên Project Swift Project


### Todos
- [x] Demo trên objective C Project
- [ ] Demo trên swift project
- [ ] Hướng dẫn sử dụng và tài liệu API
- [ ] Hỗ trợ giao diện iPad
- [ ] Hỗ trợ push notification
- [ ] Hỗ trợ wake up call

License
----

**VIETTALK COPYRIGHT 2019 @ VIVAS - VNPT TECHNOLOGY**

