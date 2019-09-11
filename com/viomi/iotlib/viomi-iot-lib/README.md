# 云米V-IOT设备SDK
    语音模块是内部集成了思必驰 DUI 解决方案，支持语音唤醒，语音识别和语音合成功能，内部已经集成对云米设备的语音控制，接入者无需关心云米设备的语音控制，但其他技能需要接入者自行在回调接口中处理。
### 最新版本：1.0.4
### 修改日期：2019.09.11
## 使用方法
### 一、基础配置使用
  1. 在你项目根目录下的 build.gradle 文件配置 maven 仓库地址：
  ```
  repositories {
      maven { url "https://raw.githubusercontent.com/ViomiHome/viomi_sdk/master" }
  }
  ```
  2. 然后在 app/build.gradle 文件中添加依赖:
  ```
  dependencies {
      implementation "com.viomi.iotlib:viomi-iot-lib:1.0.4"
  }
  ```
  3. 在 Application 或 Activity 中实现对SDK的初始化：
  ```
  ViotSDK.getInstance().init(getApplicationContext(), APP_KEY);//目前APP_KEY由V-IOT后台提供
  ```
  4. 在用户扫码登录成功之后需要对sdk进行登录，sdk会把用户的信息缓存在本地：
  ```
  ViotSDK.getInstance().saveViomiUser(new ViomiUser(token, userId));  //云米的token和userId
  ```
  5. 在用户退出登录，同时也对sdk的用户信息进行删除：
  ```
  ViotSDK.getInstance().deleteViomiUser();
  ```
### 二、配网模块
  1. 设置监听器
  ```
  ViotSDK.getInstance().addListener(ViotSDKListener listener);
  ```
  2. 移除监听器（activity ondestory时记得移除，防止内存泄漏）
  ```
  ViotSDK.getInstance().removeListener(ViotSDKListener listener);
  ```
  3. 扫描附近V-IOT设备
  （1）开始扫描设备
  ```
   //开始扫描设备
  ViotSDK.getInstance().startScan();
  
  //回调接口
  public void didScanDevices(ViotSDKErrorCode code, List<MeshBleDevice> devices) {
    
  }
  ```
  （2）停止扫描设备
  ```
  ViotSDK.getInstance().stopScan();
  ```
  4. 扫描当前网络下V-IOT网关设备
  ```
  ViotSDK.getInstance().scanMesh(meshs -> { //返回网关列表
        });
  ```
  5. 配网
  （1）当前网络有网关设备，进行配网（需要输入WiFi密码）
  ```
  
  ```
  （2）当前网络无网关设备，进行配网
  ```
  ```
  6. 获取设备列表
  ```
  ```
  7. 解绑设备
  ```
  ```
  
    
