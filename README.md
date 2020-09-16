# lbopush-react-native  

## 集成了小米&华为厂商推送,（ios暂不支持），通过小米｜华为厂商提供的api集成的客户端rn-sdk,
# 暂时不支持oppo,vivo等推送,后续会慢慢迭代集成5大厂商推送，包括ios会接通apn推送,因为本人对oc还是一知半解，所以还是希望有oc大佬来参与,非常欢迎加入!
### issue多多提问题，多多指教，欢迎star,感谢🙏！

### --V1.2.0 华为正式接入 支持华为透传推送及普通消息推送（暂时华为推送点击回调 只支持*原生activity* 跳转）--


## 安装
    npm install lbopush-react-native
## 包配置
    settings.gradle
            include ':lbopush-react-native'
            project(':lbopush-react-native').projectDir = new File(rootProject.projectDir, '../node_modules/lbopush-react-native/android')

    app/build.gradle
        implementation project(':lbopush-react-native')
        
    src/main/MainApplication
        packages.add(new MiPushPackage());

## 小米厂商集成步骤
### 手动集成
      暂无需要手动添加任何配置
             
## 华为厂商集成步骤
### 手动集成
    1.首先确保华为开发者联盟正确申请 推送app 并且按照 华为推送文档 正确填写 SHA256指纹.
    2.在华为开发者联盟 正确下载agconnect-services.json 并且放置在 主模块工程目录下 且于src同一级目录
    3.添加HUAWEI agcp插件以及Maven代码库。
      在“allprojects > repositories”中配置HMS Core SDK的Maven仓地址。
      在“buildscript > repositories”中配置HMS Core SDK的Maven仓地址。
      如果App中添加了“agconnect-services.json”文件则需要在“buildscript > dependencies”中增加agcp配置。
      
      buildscript {
          repositories {
              google()
              jcenter()
              maven {url 'https://developer.huawei.com/repo/'}
          }
          dependencies {
              ...
              classpath 'com.huawei.agconnect:agcp:1.3.1.300'
          }
      }
       
      allprojects {
          repositories {
              google()
              jcenter()
              maven {url 'https://developer.huawei.com/repo/'}
          }
      }
    4.华为集成完毕

## 华为推送点击回调须知
    1.普通推送 
      主工程清单文件 android配置一个自定义activity 用于点击普通推送后跳转
      exp:
               <activity android:name=".HuaweiActivity">
                  <intent-filter>
      
                      <action android:name="android.intent.action.VIEW" />
      
                      <category android:name="android.intent.category.DEFAULT" />
                      <category android:name="android.intent.category.BROWSABLE" />
     
                      <data
                          android:host="com.huawei.codelabpush"
                          android:path="/deeplink"
                          android:scheme="pushscheme" />
      
                  </intent-filter>
              </activity>
      切记 必须要配置scheme 支持外部传参至activity
      服务端推送 时 intent地址 由客户端提供获取的intent字符串 开发者可以调用 NativeModules.MiPush.gethuaweiintentstr(); 修改后获取自己所需的string 提供后台填写
      
      activity获取参数
        exp：
              Intent intent = getIntent();
              if (null != intent) {
                  // 方法1设置的数据通过如下方式获取
                  String name1 = intent.getData().getQueryParameter("name");
                  int age1 = Integer.parseInt(intent.getData().getQueryParameter("age"));
                  // 方法2设置的数据通过如下方式获取
                  String name2 = intent.getStringExtra("name");
                  int age2 = intent.getIntExtra("age", -1);
                  Toast.makeText(this, "name " + name1 + ",age " + age1, Toast.LENGTH_SHORT).show();
              }
     2.透传消息
      还在编写中,暂不支持点击推送回调       


### 使用方法
   #### app唤醒必须首先调用 **registerPush** 方法,实例推送服务!!!

### 使用必须

    *必须首先在小米开放平台注册好推送账号.拿到appkey及appid
    *必须首先在华为开发者联盟注册好推送.拿到agconnect-services.json文件


## api:
```javascript
const conf={
    "xiaomi_appid":"你的小米appId",
    "xiaomi_appkey":"你的小米appKey",
};
static registerPush(String channelname,String channeldec,String channelid,Object conf)
注册推送
```
```javascript
static unregisterPush()
关闭MiPush推送服务
```

```javascript
static enablePush()
启用MiPush推送服务
```
```javascript
static disablePush()
禁用MiPush推送服务
```
```javascript
static setAlias(String alia)
设置alias
```
```javascript
static unsetAlias()
取消alias
```
```javascript
static pausePush()
暂停接收MiPush服务推送的消息
```

```javascript
static resumePush()
恢复接收MiPush服务推送的消息
```

```javascript
static getAllAlias():promise
获取设备所有别名
```
```javascript
static clearNotification()
清除米推送通知
```
```javascript
static getRegId()
获取注册的设备id
```
   
```javascript
static OnMessageArrived()
收到推送回调
```

```javascript
static OnMessageClicked()
点击推送回调
```
```javascript
static removeListener()
删除监听
```

```javascript
static getPhoneType()
获取手机厂商品牌
```

```javascript
static getHuaweitoken()
获取华为token 需要上传给服务器 服务器通过token 推送
```

## 我会陆续吧vivo,oppo,魅族等5大厂商 原厂推送集成进来 , 期待有能力的伙伴加入进来一起完善提交pr！！！
