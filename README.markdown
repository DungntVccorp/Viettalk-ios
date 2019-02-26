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
| DZNEmptyDataSet | https://github.com/dzenbot/DZNEmptyDataSet
| protobuf-swift | https://github.com/alexeyxo/protobuf-swift
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
DZNEmptyDataSet.framework
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

### Cấu Hình Sử dụng trên Project Objective C
- ở class AppDelegate import
```sh
@import GoogleMaps;
@import GooglePlaces;
@import VietTalkCore;
@import ViettalkUI;
@import ViettalkCallUI;
#import <VTCallAPI/ViettalkCallAPI.h>
```
- Trong file AppDelegate.m thêm code ở trong hàm **didFinishLaunchingWithOptions**
```sh
    [GMSServices provideAPIKey:@"map_key"];
    [GMSPlacesClient provideAPIKey:@"map_key"];
    [[ViettalkAPI shared] setViettalkAPIDelegateWithDelegate:self];
```
- Trong file AppDelegate.m thực thi hàm delegate của **ViettalkCallAPIDelegate** và **ViettalkAPIDelegate**
```sh
-(void)onReceiveMessageWithForm:(NSData *)ChatID :(NSData *)messageID{
    
}
-(void)onReceiveState:(NSInteger)state error:(NSString *)error{
    if(state == ViettalkApiStateAuthen_error){
        /// RETRY AUTHEN
        if([error isEqualToString:@""] == NO){
            UIAlertController *alr = [UIAlertController alertControllerWithTitle:nil message:error preferredStyle:UIAlertControllerStyleAlert];
            [alr addAction:[UIAlertAction actionWithTitle:@"Đóng" style:UIAlertActionStyleDefault handler:nil]];
            [alr addAction:[UIAlertAction actionWithTitle:@"Thử lại" style:UIAlertActionStyleDefault handler:^(UIAlertAction * _Nonnull action) {
                
            }]];
            [self.window.rootViewController presentViewController:alr animated:YES completion:nil];
        }
    }else if(state == ViettalkApiStateAuthen_success){
        
        /// CẤU HÌNH CALL NẾU SỬ DỤNG CALL TRONG ỨNG DỤNG
        NSDictionary *data_login = [[ViettalkAPI shared] get_user_login];
        NSString *sipDomain = data_login[@"sipDomain"];
        NSString *sipPassword = data_login[@"sipPassword"];
        NSString *sipProxy = data_login[@"sipProxy"];
        NSString *sipUserName = data_login[@"sipUserName"];
        [[ViettalkCallAPI sharedInstance] setDelegate:self];
        [[ViettalkCallAPI sharedInstance] setupAccont:sipUserName password:sipPassword domain:sipDomain proxy:sipProxy];
        
        UIAlertController *alr = [UIAlertController alertControllerWithTitle:nil message:@"Đăng nhập hệ thống chat thành công" preferredStyle:UIAlertControllerStyleAlert];
        [alr addAction:[UIAlertAction actionWithTitle:@"Đóng" style:UIAlertActionStyleDefault handler:nil]];
        [self.window.rootViewController presentViewController:alr animated:YES completion:nil];
        
    }else if(state == ViettalkApiStateAuthen_failure){
        
    }
    
}

-(void)onReceiveNewIncomingCall:(NSDictionary *)newCall{
    
    if ([[ViettalkUI shared] canMakeCall] == YES) {
        [[ViettalkUI shared] showIncommingCall:self.window.rootViewController call_info:newCall];
    }
}
```
- Trong file AppDelegate.m bổ sung sử dụng trạng thái của ứng dụng 
```sh
- (void)applicationWillResignActive:(UIApplication *)application {
    [[ViettalkAPI shared] applicationWillResignActive:application];
}
- (void)applicationDidEnterBackground:(UIApplication *)application {
    [[ViettalkAPI shared] applicationDidEnterBackground:application];
}
- (void)applicationWillEnterForeground:(UIApplication *)application {
    [[ViettalkAPI shared] applicationWillEnterForeground:application];
}
- (void)applicationDidBecomeActive:(UIApplication *)application {
    [[ViettalkAPI shared] applicationDidBecomeActive:application];
}
- (void)applicationWillTerminate:(UIApplication *)application {
    [[ViettalkAPI shared] applicationWillTerminate:application];
}
```
### Sử dụng trên Objective C
- Dể Hiển thị danh sách cuộc hội thoại cũ 
```sh
if([[ViettalkUI shared] canShowChatHistory] == YES){
        [[ViettalkUI shared] showChatHistory:view_controller isPush:NO];
}
```
- Để hiển thị chi tiết một cuộc hội thoại  hoặc  tạo mới một cuộc  hôị  thoại 
```sh
if([[ViettalkUI shared] canShowMessage] == YES){
    
    [[ViettalkUI shared] showChatWithTel:self isPush:FALSE tel:phone_number_string_84xxxxx nameDisplay:name_display_string];
    }
```

- Để tạo 1 group chat mới
```sh
[[ViettalkAPI shared] createChatWorkSpaceWithProfiles:new_profile_list isGroup:isG onSuccess:^(Chat *result_create_chat) {
            [[ViettalkUI shared] showChat:self isPush:FALSE chat:result_create_chat];
        } onError:^(id er) {
            NSLog(@"%@",er);
        } onTimeout:^(id timeout) {
            NSLog(@"TIME OUT");
        }];
```


- Để thực hiện call đến 1 số 
```sh
if([[ViettalkUI shared] canMakeCall] == YES){
        [[ViettalkUI shared] showMakeCall:self tel:phone_number_string_84xxxxx name:name_display_string];
    }
```



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
