# 云米V-IOT设备SDK
    语音模块是内部集成了思必驰 DUI 解决方案，支持语音唤醒，语音识别和语音合成功能，内部已经集成对云米设备的语音控制，接入者无需关心云米设备的语音控制，但其他技能需要接入者自行在回调接口中处理。
### 最新版本：1.0.4
### 修改日期：2019.09.11
## 使用方法
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
