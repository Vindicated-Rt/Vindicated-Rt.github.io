---
title: Android进阶控件之Spinner
date: 2018-06-17
categories: 学习笔记
---

# Android之路—初章

今日记录Android中的一个下拉选择的控件,Spinner。

## Spinner是什么

java.lang.Object

  ↳ android.view.View

   ↳ android.view.ViewGroup

     ↳ android.widget.AdapterView〈T extents android.widget.Adapter〉

       ↳ android.view.ViewGroup

         ↳ android.widget.AbsSpinner

           ↳ android.widget.Spinner

             ↳ android.widget.Spinner

Spinner是一个下拉列表,列表项目来自adapter。

### 相关XML属性

| 属性                     | 描述                                                         |
| :----------------------- | :----------------------------------------------------------- |
| android:spinnerModes     | 其中有两种参数:1.dropdown是以下拉显示选项;2.dialog是以弹窗显示选项 |
| android:dropDownSelector | 当选择dropdown时,设置下拉列表的主题或者颜色                  |
| android:dropDownWidth    | 当选择dropdown时,设置下拉宽度                                |
| android:popupBackground  | 当选择dropdown时,设置下拉背景                                |
| android:prompt           | 当选泽dialog时,设置提示内容,要用string下资源,不支持字符直接输入 |

## 简单使用Spinner

现在实现两种模式的Spinner.

### 1.dropdown

添加布局文件:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Spinner
        android:id="@+id/choose_spinner"
        android:dropDownWidth="200dp"
        android:popupBackground="@color/colorPrimaryDark"
        android:spinnerMode="dialog"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

</LinearLayout>
```

设置选项:

```java
import android.support.v7.app.AppCompatActivity;
import android.os.Bundle;
import android.widget.ArrayAdapter;
import android.widget.Spinner;

public class MainActivity extends AppCompatActivity {

    private Spinner spinner;//初始化spinner对象

    protected void onCreate(Bundle savedInstanceState) {
        super.onCreate(savedInstanceState);
        setContentView(R.layout.activity_main);

        spinner = (Spinner) findViewById(R.id.choose_spinner);//绑定spinner对象
        String[] options = {"c","c++","java","python"};//初始化选项源
        ArrayAdapter<String> adapter = new ArrayAdapter<String>(this,
                android.R.layout.simple_list_item_1,options);//初始化适配器
        spinner.setAdapter(adapter);//设置适配器

    }
}
```

有如下效果:

[![dialog](https://i.loli.net/2018/06/17/5b266dda09c62.png)](https://i.loli.net/2018/06/17/5b266dda09c62.png)

### 2.dropdown

修改布局文件:

```xml
<?xml version="1.0" encoding="utf-8"?>
<LinearLayout xmlns:android="http://schemas.android.com/apk/res/android"
    android:layout_width="match_parent"
    android:layout_height="match_parent">

    <Spinner
        android:id="@+id/choose_spinner"
        android:spinnerMode="dialog"
        android:prompt="@string/choose"
        android:layout_width="wrap_content"
        android:layout_height="wrap_content"/>

</LinearLayout>
```

适配器不变,有如下效果

[![dialog](https://i.loli.net/2018/09/12/5b9904f3cb1fe.png)](https://i.loli.net/2018/09/12/5b9904f3cb1fe.png)

## Spinner的选项监听事件

```java
spinner.setOnItemSelectedListener(new AdapterView.OnItemSelectedListener() {
    @Override
    public void onItemSelected(AdapterView<?> parent,View view,int position,long id) {
        //选择某一选项后所执行的操作
    }

    @Override
    public void onNothingSelected(AdapterView<?> parent) {
        //没有选择时执行的操作
    }
});
```

onItemSelected是选择某一选项后所执行的操作,onNothingSelected是没有选择时执行的操作。