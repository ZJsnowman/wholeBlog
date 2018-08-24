---
title: Android 四大组件之广播
date: 2017-09-26 14:24:03
tags: [Android]
---

关于广播你所应该知道的一切<!-- more -->

# 广播类型
广播分为标准广播和有序广播

# 接受广播

## 动态注册

## 静态注册
通过 AS 创建广播,自动注册和生成代码
```xml
<receiver
               android:name=".broadcast.BootCompleteReceiver"
               android:enabled="true"
               android:exported="true">
           <intent-filter >
               <action android:name="android.intent.action.BOOT_COMPLETED"/>
           </intent-filter>
       </receiver>
```
- enable 表示启用该广播
- export 表示允许这个广播接收器接受程序以外的广播
- intent-filter 配置的是具体广播内容,这里是接受系统开机广播

```java
public class BootCompleteReceiver extends BroadcastReceiver {

    @Override
    public void onReceive(Context context, Intent intent) {
        Toast.makeText(context, "开机了", Toast.LENGTH_SHORT).show();
    }
}
```
当接受到广播,就会执行`onReceive`的内容.这里不要做太多耗时的内容和添加过多逻辑.一般
创建通知和启动其他服务即可.
