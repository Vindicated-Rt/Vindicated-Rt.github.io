---
title: Android进阶控件之ToolBar
date: 2018-05-14
categories: 学习笔记
---

# Android之路—初章

在母亲节这个重要的节日里,祝天下所有的母亲身体健康,天天开心。

2014年Google的 I/O (Innovation in the Open) 大会上大力推崇Material Design设计,在Android 5.0之后,Google推荐使用ToolBar,使用ToolBar取代之前的ActionBar,那今天就记录ToolBar的一些使用方法。

## ToolBar是什么

打开Android 开发者文档:

**public class Toolbar extends ViewGroup**

java.lang.Object

    ↳ android.view.View

       ↳ android.view.ViewGroup

            ↳ android.support.v7.widget.Toolbar

ToolBar是android.support.v7.widget中的一个类,继承自ViewGroup,它用于程序内容中的标准工具栏。

## 使用Toolbar

### 前期准备

既然Google推荐是用ToolBar取代ActionBar,那就在配置文件中修改Activity的Theme,去掉原有的ActionBar

```xml
android:theme="@style/Theme.AppCompat.Light.NoActionBar"
```

如这个主题就是亮色无ActionBar;

ToolBar是android.support.v7.widget的一个类,保证向下兼容性,为项目添加Dependencies，来添加com.android.support:appcompat-v7,之后就会在build.gradle中自动添加下面的一句

```java
compile 'com.android.support:appcompat-v7:25.+'
```

保证v7:后面的版本与该项目的开发版本一致。

### 添加ToolBar控件

在布局文件中添加ToolBar控件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="horizontal">
    
    <android.support.v7.widget.Toolbar
        android:id="@+id/toolbar"
        android:layout_width="match_parent"
        android:layout_height="?attr/actionBarSize">
    </android.support.v7.widget.Toolbar>

</LinearLayout>
```

其中layout_height设置的是”?attr/actionBarSize”,意思是同原来ActionBar高度一致。

在Google开发者文档中,ToolBar有很多xml属性,可以自行查阅。

### 设置ToolBar布局

这时主布局文件中有了一个ToolBar控件,我们可以在java代码中,设置它的一些属性。

```java
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.support.v7.widget.Toolbar;

public class MainActivity extends AppCompatActivity{

    private Toolbar toolbar;
    
    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        toolbar = (Toolbar) findViewById(R.id.toolbar);
        toolbar.setNavigationIcon(R.mipmap.ic_launcher_round);//设置导航图标
        toolbar.setLogo(R.mipmap.ic_launcher);//设置主图标
        toolbar.setTitle("Title");//设置标题
        toolbar.setSubtitle("SubTitle");//设置子标题
        setSupportActionBar(toolbar);//为当前activity设置toolbar

    }
}
```

程序标题栏就会成这样

[![toolbar](https://i.loli.net/2018/05/14/5af9705bdfbba.png)](https://i.loli.net/2018/05/14/5af9705bdfbba.png)

### 自定义ToolBar的menu

在menu文件夹下创建一个toolbar_menu的xml文件

[![menu](https://i.loli.net/2018/05/14/5af971f5e6962.png)](https://i.loli.net/2018/05/14/5af971f5e6962.png)

添加如下代码

```xml
<?xml version="1.0" encoding="utf-8"?>
<menu xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:app="http://schemas.android.com/apk/res-auto">

    <item
        android:id="@+id/icon1"
        android:icon="@drawable/icon1"
        android:title="title1"
        app:showAsAction="always" />

    <item
        android:title="title2"
        android:id="@+id/icon2"
        android:icon="@drawable/icon2"
        app:showAsAction="always" />

    <item
        android:title="title3"
        android:id="@+id/icon3"
        android:icon="@drawable/icon2"
        app:showAsAction="always"/>

</menu>
```

添加了3个item,但这只是一个xml文件,并没有和toolbar关联。

### 加载menu布局

在Activity中添加方法

```java
/*布置toolbar布局*/
public boolean onCreateOptionsMenu(Menu menu) {
    getMenuInflater().inflate(R.menu.toolbar_menu, menu);
    return true;
}
```

### item点击事件

在Activity中添加方法

```java
/*item点击事件*/
public boolean onOptionsItemSelected(MenuItem item) {
    return super.onOptionsItemSelected(item);
}
```

在方法中,可以用switch-case来选择item的id来设置点击事件。