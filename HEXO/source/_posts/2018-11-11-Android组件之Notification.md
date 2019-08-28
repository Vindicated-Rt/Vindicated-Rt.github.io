---
title: Android组件之Notification
date: 2018-11-11
categories: 学习笔记
---

# Android之路—初章

Android的版本已经更新到Pie了,今日记录一个在Android O之后更新的需要重写的通知Notification

## 手机通知

手机最上一栏,就是状态栏,很多应用的通知就显示在这里,当用户下拉状态栏后,就能显示这些通知.

Notification,就是构建这种通知的一个类.

java.lang.Object

  ↳ android.app.Notification

### 用Notification + NotificationManager发送通知

写一个demo,主页面两个按钮:一个发送通知,一个取消通知

一个Notification 需要 NotificationManager 来管理,所以需要创建这样的对象

```java
private NotificationManager manager;
manager = (NotificationManager) getSystemService(Context.NOTIFICATION_SERVICE);
```

这个管理者对象取得系统的服务中的通知管理服务.

在点击通知时,通常是能跳转到一些界面的,这就需要创建跳转的Intent

```java
Intent intent = new Intent(this, MainActivity.class);
PendingIntent pendingIntent = PendingIntent.getActivity(this, 0, intent, 0);
```

这里的Intent是跳转到主页面的一个连接,PendingIntent是一个比intent更先进的一个类,它必须满足一些条件,才能触发作为它的参数的Intent.

其中PendingIntent的getActivity方法中参数分别是

1. Context context 上下文
2. int requestCode 请求码,每次requestcode不同,就能产生多个Pendingintent.
3. Intent intent 触发的Intent
4. int flags 对不同操作作标识

然后创建通知的对象,其中 **一定要设置图标**

```java
Notification notification = new Notification.Builder(getApplicationContext())
        .setSmallIcon(R.mipmap.ic_launcher)     //必须设置图标,通知才能显示
        .setTicker("have a good day")           //设置通知栏通知语句
        .setWhen(System.currentTimeMillis())    //设置通知栏时间
        .setContentTitle("美好的一天")           //设置通知栏显示标题
        .setContentText("来之app的问候")         //设置通知栏显示文本
        .setContentIntent(pendingIntent)        //设置通知栏跳转页面
    	.setAutoCancel(true)                    //通知自动的消失
        .build();                               //创建通知栏
```

最后用管理对象来通知系统发送通知即可

```java
manager.notify(1, notification);
```

其中,第一个参数是通知的ID,开发者自己设定为全局标识即可.

## Android O 更新特新

上面demo在Android O 之前的版本手机上可以运行,但是在Android O 以上的手机却无法显示通知.

这是因为在Android O 更新之后, Notification的发送,需要选择发送的渠道,才可以通过NotificationManager发送.

所以就要创建渠道类NotificationChannel

### NotificationChannel

创建NotificationChannel对象

```java
if (android.os.Build.VERSION.SDK_INT >= android.os.Build.VERSION_CODES.O) {
            NotificationChannel channel = new NotificationChannel(id,name,NotificationManager.IMPORTANCE_MIN);
}
```

三个参数:

1. channelId 通知渠道的ID 可以是任意的字符串，开发者设置的全局变量
2. channelName 通知渠道的名称，这个是用户可见的，开发者需要认真规划的命名
3. importance 通知渠道的重要等级，有以下几个等级，不过这个用户都是可以手动修改的

| 用户可见级别             | 重要性             |
| :----------------------- | :----------------- |
| 发出声音且显示为提醒通知 | IMPORTANCE_HIGH    |
| 发出声音                 | IMPORTANCE_DEFAULT |
| 没有声音                 | IMPORTANCE_LOW     |
| 无声且不显示在通知栏     | IMPORTANCE_MIN     |

同时,channel有很多设置

```java
setLightColor();//设置通知灯使用的颜色
setSound();//提供一个Uri，用于在通知发布到此频道时播放声音
setGroup();//设置通知分配到的组
setBypassDnd();//设置通知是否应绕过“请勿打扰”模式(中断_筛选器_优先级值)
setLockScreenVisibility();//设置是否应在锁定屏幕上显示来自此通道的通知
```

设置好渠道参数之后,通知管理对象调用方法,创造这个渠道

```java
manager.createNotificationChannel(channel);
Notification notification = new Notification.Builder(getApplicationContext(), id)
             .setSmallIcon(R.mipmap.ic_launcher)     //必须设置图标,通知才能显示
             .setTicker("have a good day")           //设置通知栏通知语句
             .setWhen(System.currentTimeMillis())    //设置通知栏时间
             .setContentTitle("美好的一天")           //设置通知栏显示标题
             .setContentText("来之app的问候")         //设置通知栏显示文本
             .setContentIntent(pendingIntent)        //设置通知栏跳转页面
             .setAutoCancel(true)                    //设置通知栏渠道
             .build();                               //创建通知栏
```

不同于之前的方法,添加参数,对应相应的渠道.