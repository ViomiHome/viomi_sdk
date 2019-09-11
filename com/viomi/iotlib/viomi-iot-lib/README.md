# 云米V-IOT设备SDK
### 最新版本：1.0.4
### 修改日期：2019.09.11
# 使用方法
## 一、基础配置使用
### 1.在你项目根目录下的 build.gradle 文件配置 maven 仓库地址：
  
  ```
  repositories {
      maven { url "https://raw.githubusercontent.com/ViomiHome/viomi_sdk/master" }
  }
  ```
  
### 2.然后在 app/build.gradle 文件中添加依赖:
  
  ```
  dependencies {
      implementation "com.viomi.iotlib:viomi-iot-lib:1.0.4"
  }
  ```
### 3.在 Application 或 Activity 中实现对SDK的初始化：
  
  ```
  ViotSDK.getInstance().init(getApplicationContext(), APP_KEY);//目前APP_KEY由V-IOT后台提供
  ```
  
### 4.在用户扫码登录成功之后需要对sdk进行登录，sdk会把用户的信息缓存在本地：
  
  ```
  ViotSDK.getInstance().saveViomiUser(new ViomiUser(token, userId));  //云米的token和userId
  ```
  
### 5.在用户退出登录，同时也对sdk的用户信息进行删除：
  
  ```
  ViotSDK.getInstance().deleteViomiUser();
  ```
  
## 二、配网模块
### 1.设置监听器
  
  ```
  ViotSDK.getInstance().addListener(ViotSDKListener listener);
  ```
  
### 2.移除监听器（activity ondestory时记得移除，防止内存泄漏）
  
  ```
  ViotSDK.getInstance().removeListener(ViotSDKListener listener);
  ```
  
### 3.扫描附近V-IOT设备
  
  ```
   //(1)开始扫描设备
  ViotSDK.getInstance().startScan();
  
  //回调接口
  public void didScanDevices(ViotSDKErrorCode code, List<MeshBleDevice> devices) {
    
  }
  //（2）停止扫描设备
  ViotSDK.getInstance().stopScan();
  ```
  
### 4.扫描当前网络下V-IOT网关设备
 
  ```
  ViotSDK.getInstance().scanMesh(meshs -> { //返回网关列表
        });
  ```
  
### 5.配网
  
  ```
  //（1）当前网络有网关设备，进行配网（需要输入WiFi密码）
  ViotSDK.getInstance().startConfigure(meshBleDevices, wifiInfo, password); 
  
  //（2）当前网络无网关设备，进行配网
  ViotSDK.getInstance().startConfigureChildren(meshBleDevices);
  
  //（3）配网的回调
  //code 错误码， prorgess 进度，  meshBleDevices配置成功的设备列表
  public void didConfigDevices(ViotSDKErrorCode code, int prorgess, List<MeshBleDevice> meshBleDevices) {
      
  }
  ```
  
### 6.获取设备列表
  
  ```
  //获取设备列表
  ViotSDK.getInstance().getRemoteDeviceList(); 
  
  //回调接口
  public void didRemoteDevices(ViotSDKErrorCode code, List<VDevice> devices){
  
  }
  ```
  
### 7.解绑设备
  
  ```
  //解绑设备
  ViotSDK.getInstance().unbind(did); 
  
  //回调接口
  public void didUnbindDevice(ViotSDKErrorCode code) {
  
  }
  ```
  
### 8.重命名设备
  
  ```
  //重命名设备
  ViotSDK.getInstance().rename(did, name); 
    
  //回调接口
  public void didRenameDevice(ViotSDKErrorCode code, String did) {
  
  }
  ...
  
  
  
    

  
  
    
