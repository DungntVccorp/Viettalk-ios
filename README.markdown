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

# Kiến trúc chung

![Alt text](https://raw.githubusercontent.com/DungntVccorp/Viettalk-ios/master/05.png)


# SDK gồm có!
- **CoreDM.framework** ==> chứa kiến trúc database và giao tiếp kết nối tới  server
- **ProtoModel.framework** ==> chứa định nghĩa model
- **VietTalkCore.framework** ==> Chứa api chat và báo hiệu
- **VTCallAPI.framework** ==> chứa những api Liên quan VOIP
- **ViettalkUI.framework** ==> chứa giao diện chat
- **ViettalkCallUI.framework** ==> chứa giao diện gọi điện

### ProtoModel.framework Dependency
| Framework Name| Link | Version |
| ------ | ------ | ------ |
| protobuf-swift | https://github.com/alexeyxo/protobuf-swift | v4.0.6
### VietTalkCore.framework Dependency
| Framework Name| Link | Version |
| ------ | ------ | ------ |
| CoreDM | cung cấp cùng thư viện | 0.1
| PhoneNumberKit | https://github.com/marmelroy/PhoneNumberKit | 2.6.0
### VTCallAPI.framework Dependency
| Framework Name | Link | Version
| ------ | ------ | ------ |
| VietTalkCore | cung cấp cùng thư viện | 0.1
| AudioToolBox | System framework |
| AVFoundation |  System framework | 
### ViettalkUI.framework Dependency
| Framework Name | Link | Version
| ------ | ------ | ------ |
| SwiftLinkPreview | https://github.com/LeonardoCardoso/SwiftLinkPreview | 3.0.1
| SnapKit | https://github.com/SnapKit/SnapKit | 5.0.0
| Kingfisher | https://github.com/onevcat/Kingfisher | 4.10.0 
| SwipeCellKit | https://github.com/SwipeCellKit/SwipeCellKit | master
| CLImageEditorCarthage | https://github.com/DungntVccorp/CLImageEditorCarthage | master
| Pulsator | https://github.com/shu223/Pulsator | 0.5.3 
| SKPhotoBrowser | https://github.com/suzuki-0000/SKPhotoBrowser | 6.1.0
| MBProgressHUD | https://github.com/jdg/MBProgressHUD | 1.1.0 
| TOCropViewController | https://github.com/TimOliver/TOCropViewController | master
| THContactPickerCart | https://github.com/DungntVccorp/THContactPickerCart | master 
| Floaty | https://github.com/kciter/Floaty | 4.2.0
| DZNEmptyDataSet | https://github.com/dzenbot/DZNEmptyDataSet | v1.8.1
| Google Map SDK | https://developers.google.com/maps/documentation/ios-sdk/intro | master
| Google Places SDK | https://developers.google.com/places/ios-sdk/intro | master
### ViettalkUI.framework Dependency
| Framework Name | Link | Version
| ------ | ------ | ------ |
| ViettalkUI | cung cấp cùng thư viện | 0.1

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
3rd Framework
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
![Alt text](https://raw.githubusercontent.com/DungntVccorp/Viettalk-ios/master/01.png)
- Nếu sử dụng cho **Objective C** Project 
```sh
Always Embed Swift Standard Libraries = "YES"
```
![Alt text](https://raw.githubusercontent.com/DungntVccorp/Viettalk-ios/master/02.png)
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
![Alt text](https://raw.githubusercontent.com/DungntVccorp/Viettalk-ios/master/03.png)
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
