---
title: Android进阶控件之ViewPager(1)
date: 2018-09-09
categories: 学习笔记
---

# Android之路—初章

开学了，从美国回来，又要开始学习了。

我将用三篇博客记录Android中的一个重要显示和滑动视图的控件,ViewPager。

## ViewPager是什么

从字面意思来看,这个控件是一个视图浏览者,打开从谷歌开发者文档中来看

java.lang.Object

  ↳ android.view.View

     ↳ android.view.ViewGroup

        ↳ android.support.v4.view.ViewPager

Layout manager that allows the user to flip left and right through pages of data.

它是继承自ViewGroup的一个类,那它也是一个视图的容器。而它承载的视图，可以左右滑动，达到用户浏览的效果。

简单使用ViewPager

使用ViewPager不单单是一个控件,它还需要适配器来提供所承载的视图。这篇简单记录承载图片的方式。

## ViewPager的图片浏览器

### 创建三个布局用于切换

用一个ImageView设置工程中的图片,三段代码一致,更改图片即可.

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <ImageView
        android:background="@mipmap/pic_1"
    <!--在此设置图片-->
        android:layout_width="match_parent"
        android:layout_height="match_parent" />

</LinearLayout>
```

### 添加适配器

添加一个适配器类，继承自PagerAdapter,用于存放数据源,视图集合.

```java
import android.support.v4.view.PagerAdapter;
import android.view.View;
import android.view.ViewGroup;

import java.util.List;

public class ViewAdpter extends PagerAdapter {

    private List<View> viewList;
    public ViewAdpter(List<View> viewList){
        this.viewList = viewList;
    }

    /*返回集合数目*/
    public int getCount() {
        return viewList.size();
    }

    /*检测视图是否由对象产生*/
    public boolean isViewFromObject(View view, Object object) {
        return view == object;
    }

    /*实例化*/
    public Object instantiateItem(ViewGroup container, int position) {
        container.addView(viewList.get(position));//将view添加到ViewGroup中
        return viewList.get(position);

    }

    /*销毁页卡*/
    public void destroyItem(ViewGroup container, int position, Object object) {
        container.removeView(viewList.get(position));

    }
}
```

### 为布局添加ViewPage控件

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    android:orientation="vertical">

    <android.support.v4.view.ViewPager
        android:id="@+id/picture_vp"
        android:layout_width="match_parent"
        android:layout_height="match_parent" />
            
</LinearLayout>
```

### 在主Activity中初始化控件,设置适配器

```java
package com.example.lenovo.viewpager_demo;

import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private List<View> viewList;
    private ViewPager viewPager;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);
        
        viewList = new ArrayList<View>();

        /*将layout转为view对象，并将对象添加到List<View>中，作为适配器数据源*/
        View view1 = View.inflate(this,R.layout.view1_layout,null);
        View view2 = View.inflate(this,R.layout.view2_layout,null);
        View view3 = View.inflate(this,R.layout.view3_layout,null);

        viewList.add(view1);
        viewList.add(view2);
        viewList.add(view3);

        ViewAdpter myadpter = new ViewAdpter(viewList);
        viewPager = (ViewPager) findViewById(R.id.pager);
        viewPager.setAdapter(myadpter);
    }
}
```

这样就能得到如下效果

[![img](https://i.loli.net/2018/09/10/5b966aa1a6b8c.gif)](https://i.loli.net/2018/09/10/5b966aa1a6b8c.gif)

其中《罗纳河的星月夜》是页卡1,《星空》是页卡2，《向日葵》是页卡3.

## PagerTabStrip和PagerTitleStrip

顾名思义,一个标签栏,一个是标题栏,不同的是PagerTabStrip多一条下划线随着页面滑动而滑动,PagerTabStrip的Tab是可以点击的，当用户点击某一个Tab时，当前页面就会跳转到这个页面.

### 在XML文件中配置PagerTabStrip

```xml
<android.support.v4.view.PagerTabStrip
            android:id="@+id/pivture_pts"
            android:layout_width="match_parent"
            android:layout_height="wrap_content"
            android:layout_gravity="bottom">
</android.support.v4.view.PagerTabStrip>
```

### 在适配器中添加几句code

```java
import android.support.v4.view.PagerAdapter;
import android.view.View;
import android.view.ViewGroup;

import java.util.List;

public class ViewAdpter extends PagerAdapter {

    private List<View> viewList;
    private List<String> tiltle;//新增title集合
   
    public ViewAdpter(List<View> viewList, List<String> tiltle){
        this.viewList = viewList;
        this.tiltle = tiltle;//新增构造方法
    }

    /*返回集合数目*/
    public int getCount() {
        return viewList.size();
    }

    /*标题栏数据位置*/
    public CharSequence getPageTitle(int position) {
        return tiltle.get(position);
    }

    /*检测视图是否由对象产生*/
    public boolean isViewFromObject(View view, Object object) {
        return view == object;
    }

    /*实例化*/
    public Object instantiateItem(ViewGroup container, int position) {
        container.addView(viewList.get(position));//将view添加到ViewGroup中
        return viewList.get(position);
    }

     /*销毁页卡*/
    public void destroyItem(ViewGroup container, int position, Object object) {
        container.removeView(viewList.get(position));
    }
}
```

### 在主Activity设置相关属性

```java
import android.support.v4.view.ViewPager;
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.view.View;

import java.util.ArrayList;
import java.util.List;

public class MainActivity extends AppCompatActivity {

    private List<View> viewList;
    private ViewPager viewPager;
    private List<String> title;

    @Override
    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        viewList = new ArrayList<View>();
        title = new ArrayList<String>();

        /*将layout转为view对象，并将对象添加到List<View>中，作为适配器数据源*/
        View view1 = View.inflate(this,R.layout.view1_layout,null);
        View view2 = View.inflate(this,R.layout.view2_layout,null);
        View view3 = View.inflate(this,R.layout.view3_layout,null);

        /*向集合中添加元素*/
        viewList.add(view1);
        viewList.add(view2);
        viewList.add(view3);

        title.add("《塞纳河的星月夜》");
        title.add("《星空》");
        title.add("《向日葵》");

        ViewAdpter myadpter = new ViewAdpter(viewList, title);
        viewPager = (ViewPager) findViewById(R.id.picture_vp);
        viewPager.setAdapter(myadpter);
    }
}
```

显示如下

[![img](https://i.loli.net/2018/09/10/5b96886b8eead.gif)](https://i.loli.net/2018/09/10/5b96886b8eead.gif)