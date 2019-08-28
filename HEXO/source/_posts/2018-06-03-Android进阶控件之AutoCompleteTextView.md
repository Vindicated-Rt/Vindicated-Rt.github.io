---
title: Android进阶控件之AutoCompleteTextView
date: 2018-06-03
categories: 学习笔记
---

# Android之路—初章

前两个星期没有更新博客,是与小伙伴合作了一个小实战,在最后几天为了参加学校的比赛,所以有些小忙,不过获得了二等奖,还是很开心的,这里做下一篇博客的预告,放出这个实战项目的GitHub链接 [东篱一隅](https://github.com/Vindicated-Rt/AViewOfTori)

今天要说的是自动完成文本显示框。（AutoCompleteTextView）

## AutoCompleteTextView是什么

打开谷歌官方文档：

**public class AutoCompleteTextView extends EditText implements Filter.FilterListener**

java.lang.Object

  ↳ android.view.View

     ↳ android.widget.TextView

        ↳ android.widget.EditText

           ↳ android.widget.AutoCompleteTextView

AutoCompleteTextView是继承自EditText,说明它也是可以编辑的文本显示控件,只是添加了一个功能:可以根据输入的部分文本,匹配对应的内容,如果点击匹配的内容,可以补全想要的完整文本。

就如同浏览器的搜索框

[![谷歌浏览器](https://i.loli.net/2018/06/03/5b13f39dd001e.png)](https://i.loli.net/2018/06/03/5b13f39dd001e.png)

当然,Google的搜索框还有自动匹配搜索内容的功能。

## 控件常用属性

| 属性                             | 描述                                 |
| :------------------------------- | :----------------------------------- |
| android:completionHint           | 设置出现在下拉列表底部的提示信息     |
| android:completionThreshold      | 设置触发补全提示信息的最小字符个数   |
| android:dropDownHorizontalOffset | 设置下拉菜单于文本框之间的水平偏移量 |
| android:dropDownHeight           | 设置下拉菜单的高度                   |
| android:dropDownWidth            | 设置下拉菜单的宽度                   |
| android:singleLine               | 设置是否单行显示文本内容             |
| android:dropDownVerticalOffse    | 设置下拉菜单于文本框之间的垂直偏移量 |

## 简单使用AutoCompleteTextView

按理说使用AutoCompleteTextView应该是配合相关数据进行搜索的使用,那在之后的搜索引擎篇可以再次深入使用,在次使用简单的数组适配器来存放简单的匹配数据。

### 添加控件(设置相关属性)

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <AutoCompleteTextView
        android:id="@+id/at"
        android:layout_width="match_parent"
        android:layout_height="wrap_content"
        android:completionHint="无更多匹配内容"
        android:completionThreshold="1"
        android:hint="请输入关键字" />

</LinearLayout>
```

在下拉匹配列表底部设置内容:无更多匹配内容,最小触发字符个数:1。

### 设置适配器(初始化数据)

```java
package com.example.lenovo.autocompletetextview_demo;

import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.AutoCompleteTextView;

public class MainActivity extends AppCompatActivity {

    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        AutoCompleteTextView autoCompleteTextView = (AutoCompleteTextView)findViewById(R.id.at);
        String str[] = {"a","ab","abc","abcd","abcde","abcdef","abcdefg","abcdefgh"};
        ArrayAdapter<String> arrayAdapter = new ArrayAdapter<String>														(this,android.R.layout.simple_list_item_1,str);
        autoCompleteTextView.setAdapter(arrayAdapter);
    }
}
```

简单介绍ArrayAdapter,数组适配器,T表示是数组类型,它有很多重载,这里用到的是

```java
public ArrayAdapter(Context context,int resource, T[] objects) {
    this(context, resource, 0, Arrays.asList(objects));
}
```

三个参数:上下文,适配布局id,适配数据数组。其中上下文是this当前Activity,数据是初始化的字符串数组。

## 效果图

[![demo](https://i.loli.net/2018/06/03/5b1402bc24eea.gif)](https://i.loli.net/2018/06/03/5b1402bc24eea.gif)