---
title: Android进阶控件之ViewPager(2)
date: 2018-09-12
categories: 学习笔记
---

# Android之路—初章

继续记录ViewPaper.

## ViewPager + FragmentPagerAdapter + Fragment

ViewPaper还是可以承载Fragment,Fragment会在后面的博客中专门记录,这里只说ViewPaper

### 创建三个用于Fragment切换

Fragment的布局文件.背景是图,上面文本是图片名

```xml
<FrameLayout xmlns:android="http://schemas.android.com/apk/res/android"
    xmlns:tools="http://schemas.android.com/tools"
    android:layout_width="match_parent"
    android:layout_height="match_parent"
    tools:context="com.example.lenovo.viewpager_demo.Fragment_1">

    <ImageView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:background="@mipmap/pic_1" />

    <TextView
        android:layout_width="match_parent"
        android:layout_height="match_parent"
        android:gravity="center|top"
        android:text="《麦田群鸦》"
        android:textColor="#000"
        android:textSize="30dp" />

</FrameLayout>
```

Java代码

```java
import android.os.Bundle;
import android.support.v4.app.Fragment;
import android.view.LayoutInflater;
import android.view.View;
import android.view.ViewGroup;

public class Fragment_1 extends Fragment {

    /*构造函数*/
    public Fragment_1() {
        // Required empty public constructor
    }

    /*重写创建视图方法*/
    public View onCreateView(LayoutInflater inflater, ViewGroup container,
                             Bundle savedInstanceState) {
		/*返回由布局转换为的视图*/
        return inflater.inflate(R.layout.fragment_1, container, false);
    }
}

```

### 主布局改为PagerTitleStrip

```xml
<android.support.v4.view.PagerTitleStrip
            android:id="@+id/pivture_pts"
            android:layout_width="match_parent"
            android:layout_height="wrap_content">
</android.support.v4.view.PagerTitleStrip>
```

### 设置Fragment适配器

```java
import android.support.v4.app.Fragment;
import android.support.v4.app.FragmentManager;
import android.support.v4.app.FragmentPagerAdapter;
import android.view.ViewGroup;

import java.util.List;

public class FragmentAdapter extends FragmentPagerAdapter {

    private List<Fragment> fragments;
    private List<String> tiltle;
    private Fragment pagerFrament;

    /*构造方法*/
    public FragmentAdapter(FragmentManager fm, List<Fragment> fragments, List<String> title) {
        super(fm);
        this.fragments = fragments;
        this.tiltle = title;
    }

    /*标题栏数据位置*/
    public CharSequence getPageTitle(int position) {
        return tiltle.get(position);
    }

    /*获得fragment条目*/
    public Fragment getItem(int position) {
        return fragments.get(position);
    }

    /*获得总共页面数*/
    public int getCount() {
        return fragments.size();
    }

    /*初始化容器内容,将ViewGroup数据加载为适配器item*/
    public Object instantiateItem(ViewGroup container, int position) {
        return super.instantiateItem(container, position);
    }

    /*销毁页面*/
    public void destroyItem(ViewGroup container, int position, Object object) {
        super.destroyItem(container, position, object);
    }

    /*设置当前Fragment方法*/
    public void setPrimaryItem(ViewGroup container, int position, Object object) {
        pagerFrament = (Fragment) object;
        super.setPrimaryItem(container, position, object);
    }

    /*获取当前Fragment方法*/
    public Fragment getCurrentFragment() {
        return pagerFrament;
    }

    public int getItemPosition(Object object) {
        /*当主视图试图确定某个项目的位置是否已更改时调用。
        * 如果给定项目的位置未更改，则返回{@link #POSITION_UNCHANGED};
        * 如果该项目不再存在于适配器中，则返回{@link #POSITION_NONE}。
        * 这是PagerAdapter类中的成员
        * */
        return POSITION_NONE;
    }
}
```

相对视图适配器.增加一些主要的方法.

### 设置主Activity方法

把设置集合的几行代码抽象出去,成为方法,直接调用.

```java
/*设置标题集合方法*/
   public void setTitleList(){

       /*初始化集合*/
       title = new ArrayList<String>();

       /*向标题集合中添加元素*/
       title.add("《麦田群鸦》");
       title.add("《杏花》");
       title.add("《桃树花开》");
   }

   /*设置视图集合方法*/
   public void setViewsList(){

       /*创建集合对象*/
       viewList = new ArrayList<View>();

       /*将layout转为view对象，并将对象添加到List<View>中，作为适配器数据源*/
       View view1 = View.inflate(this,R.layout.view1_layout,null);
       View view2 = View.inflate(this,R.layout.view2_layout,null);
       View view3 = View.inflate(this,R.layout.view3_layout,null);

       /*向视图集合中添加元素*/
       viewList.add(view1);
       viewList.add(view2);
       viewList.add(view3);

       /*初始化视图适配器*/
       viewAdapter = new ViewAdapter(viewList, title);
   }

   /*设置Fragment集合的方法*/
   public void setFragmentList(){

       /*创建集合对象*/
       fragments = new ArrayList<Fragment>();

       /*向Fragment集合中添加元素*/
       fragments.add(new Fragment_1());
       fragments.add(new Fragment_2());
       fragments.add(new Fragment_3());

       /*创建Fragment适配器对象*/
       fragmentAdapter = new FragmentAdapter(getSupportFragmentManager()
               ,fragments,title);
   }
```

在onCreate方法中,修改适配器即可.

```java
viewPager.setAdapter(fragmentAdapter);//改为Fragment适配器
```

效果如下

[![img](https://i.loli.net/2018/09/12/5b9924dda129a.gif)](https://i.loli.net/2018/09/12/5b9924dda129a.gif)

## ViewPager的监听事件

ViewPager作为一个控件,其中也有一些监听事件,例如这个当pager发生改变时触发的函数

```java
/*移动数据方法
   * position是当前页卡数
   * positionOffset移动百分比
   * positionOffsetPixels移动像素点*/
   public void onPageScrolled(int position, float positionOffset, int positionOffsetPixels) {
       
   }

   /*当前页面锁定的方法*/
   public void onPageSelected(int position) {

   }

   /*滑动状态改变的方法*/
   public void onPageScrollStateChanged(int state) {

   }
```

在主函数以接口的方式,添加该监听事件,将会上述有三个方法.我们可以在里面设置自己想要的效果,下一篇的一些特效也需要这些方法.