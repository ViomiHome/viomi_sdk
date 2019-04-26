# 云米语音模块
  语音模块是内部集成了思必驰 DUI 解决方案，支持语音唤醒，语音识别和语音合成功能，内部已经集成对云米设备的语音控制，接入者无需关心云米设备的语音控制，但其他技能需要接入者自行在回调接口中处理。
### 最新版本：1.0.5
### 修改日期：2019.04.26
## 使用方法
  1. 在你项目根目录下的 build.gradle 文件配置 maven 仓库地址：
  ```
  repositories {
      maven { url 'https://raw.githubusercontent.com/MiEcosystem/mijiaSDK/stable3.0/repository' }
      maven { url "https://raw.githubusercontent.com/ViomiHome/viomi_sdk/master" }
  }
  ```
  2. 然后在 app/build.gradle 文件中添加依赖:
  ```
  dependencies {
      implementation 'com.viomi.speech:viomi-speech-lib:1.0.5'
  }
  ```
  3. 在 Application 或 Activity 中实现对语音服务的初始化：
  ```
  ViomiSpeechConfig config = ViomiSpeechConfig.newBuilder()
          .setBranch("prod") // 必须设置，指定语音技能分支，一般测试为 "test"，正式发版为 "prod"
          .setDeviceDebug(false) // 语音控制设备服务器环境，true 为测试环境，false 为正式环境，默认 false
          .setDebug(false) // 语音调试开关，打开后会保存唤醒，asr 和 tts 日志在 /sdcard/Android/data/<package-name>/cache/ 目录下，默认 false
          .setLog(true) // 调试日志打印开关，默认 false
          .setDid(did) // 非必需，设备 did，主要用于大数据统计
          .setMac(mac) // 必须设置，必须是正确的 MAC 格式，否者语音初始化不成功
          .setMiClientId("") // 必须设置，请求小米 RPC 接口的 AUTH_ID
          .setMiId("") // 非必须，小米账号 id，主要用于大数据统计
          .setViomiId("") // 云米账号 id，主要用于控制设备时判断登录状态
          .setViomiToken("") // 云米账号 token，用于控制设备接口传参
          .setModel("") // 非必需，设备对应 model，主要用于大数据统计
          .setType(ViomiSpeechConfig.Type.DEVICE_FRIDGE_21_FACE_4_MIC) // 必须设置，语音初始化会根据设备类型下载对应资源包
          .build();
   SpeechIml speechIml = new SpeechIml();
   ViomiSpeechManager.getInstance().init(this, config, speechIml);
   ```
   
   SpeechIml 继承语音回调接口 ViomiSpeechListener:
   ```
   public class SpeechIml implements ViomiSpeechListener {

    @Override
    public void wakeUp(Context context) {
        // TODO 语音唤醒的回调
    }

    @Override
    public void endDialog(Context context) {
        // TODO 语音对话结束的回调
    }

    @Override
    public void onNluResult(Context context, String data) {
        // TODO 自定义技能回调
    }

    @Override
    public void onNlpResult(Context context, String message, String data) {
        // TODO 快捷唤醒词和内置技能回调
    }
}
```
至此，语音服务已经集成到你的项目中了。

### ViomiSpeechConfig.Type
   设备类型，目前支持的设备有 DEVICE_FRIDGE_21_FACE_4_MIC（21FACE 冰箱 4麦方案），DEVICE_SPEAKER（云米小V音箱），
   DEVICE_RANGE_HOOD(X1烟机)，DEVICE_MAGIC_MIRROR(魔镜)，开发者需要根据设备类型设置对应的ViomiSpeechConfig.Type，
   否则会影响设备语音唤醒率和识别度。
   
## 部分接口说明
   ```
   ViomiSpeechManager.getInstance().speechContent("你要回复的内容");
   ```
   对话过程中，在语音回调接口处理技能中，如需要回复用户内容，可调用此接口，如果不唤醒对话，调用此方法无效。
   ```
   ViomiSpeechManager.getInstance().wakeupManual();
   ```
   用于手动唤醒，效果跟语音唤醒一样。
   ```
   ViomiSpeechManager.getInstance().speak("你要合成的内容");
   ```
   文字合成语音并播放，无论有没唤醒对话，调用此方法均有效，不过内容不会更新到对话列表。
   ```
   ViomiSpeechManager.getInstance().enableWakeup(true);
   ```
   使能语音唤醒功能，true 为打开语音唤醒功能，false 为关闭语音唤醒功能。
   ```
   ViomiSpeechManager.getInstance().saveLocation(latitude, longitude, city);
   ```
   上报设备位置到思必驰后台。
   ```
   ViomiSpeechManager.getInstance().saveUserInfo(miId, viomiId, miClientId, viomiToken);
   ```
   设备每当登录或注销登录时，都需要调用此接口保存用户信息，否则语音控制设备的登录状态无法同步。
   ```
   ViomiSpeechManager.getInstance().release(context);
   ```
   释放语音资源服务，调用此方法后语音服务关闭，开发者根据自己实际情况在合适位置调用。
   
## 注意事项
   1. 模块内部已经引用了云米设备SDK：com.viomi.devicelib:viomi-device-lib:2.0.6，引用了本SDK无需再引用云米设备SDK。
   2. 语音初始化必须通过 setMac() 传入格式正确的 mac 地址，否则语音服务初始化不成功。
   
## Demo 工程地址
   https://gitlab.viomi.com.cn/app/Android/Device/viomi_speech_demo
   
   
