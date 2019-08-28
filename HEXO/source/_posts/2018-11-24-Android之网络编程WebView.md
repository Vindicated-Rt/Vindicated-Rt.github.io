---
title: Android之网络编程WebView
date: 2018-11-24
categories: 学习笔记
---

# Android之路—中章

智能手机的兴起, 我觉得很多原因都来自网络的普及, 尤其是 Android 和 ios 的崛起, 打败诺基亚,重新占领被塞班系统垄断的手机市场, 网络应用是Android开发者的必备知识.

## WebView

WebVIew是Android中一个显示视图类的控件,看名字就知道是用来显示网页的, 我们可以用这个控件来开发浏览器

java.lang.Object

  ↳ android.view.View

     ↳ android.widget.ViewGroup

        ↳ android.widget.AbsoluteLayout

           ↳ android.webkit.WebView

WebView继承一个布局AbsoluteLayout,绝对布局, 这跟网页可以放大,缩小,从而有不同的布局有很大关系, 开发浏览器时, 适应手机屏幕也是源自于此.同时, 它还属于WebKit这个工具类, 使用WebKit中的渲染引擎来显示网页, 从而可以适配Javascript, html5之类的语言, 写出的网页.

### 添加权限

这里需要说明一下权限的一些问题, 从WebView这个控件来说, 有的人会想, 显示网页的控件, 必须要添加网络权限.实际上并不全对, Android网络权限, 是APP访问网络的权限, 所以, 如果用WebView显示一个本地的静态网页, 是不用申请网络权限的, 自然不用添加网络权限, 只有根据某类协议, 打开远程的, 需要联网的网页时, 才需要添加网络权限 :

```xml
<uses-permission android:name="android.permission.INTERNET"/>

```

### 显示网页

**根据网址显示网页** :

```java
public void loadUrl(String url);
```

传入网址, 但是这样方式,系统会提示是否由系统默认浏览器打开,所以加上这个方法, 就可以用WebView打开网页

```java
public void setWebViewClient(WebViewClient client);
```

**根据HTML显示网页** :

```java
public void loadData(String data, String mimeType, String encoding)
```

data : String类型的数据, 一般为html字符串;

mimeType : data数据的MIME类型, HTML的类型为 “text/html”;

encoding : data数据的编码格式, 比如”utf-8”;

实际开发中, loadData()中的html data中尽量不要包含’#’, ‘%’, ‘\’, ‘?’四中特殊字符, 但不绝对, 因为会显示乱码, 从而使用 loadData 的升级版 loadDataWithBaseURL :

```java
public void loadDataWithBaseURL(String baseUrl, String data,
                                String mimeType, String encoding, String historyUrl)

```

baseUrl : HTML代码片段中相关资源的相对根路径;

historyUrl : 历史Url;

baseUrl指定了你的data参数中数据是以什么地址为基准的，因为data中的数据可能会有超链接或者是image元素，而很多网站的地址都是用的相对路径，如果没有baseUrl，webview将访问不到这些资源。这也是loadData方法在显示一些HTML代码的时候, 会显示乱码的原因.

在Assets资源文件夹下有一个html文件, 从文件输入流获取html文件,显示html代码网页.

```java
String data = "";
try {
    InputStream inputStream = getAssets().open("test.html");
    BufferedInputStream bis = new BufferedInputStream(inputStream);
    ByteArrayOutputStream bos = new ByteArrayOutputStream();
    byte[] databyte = new byte[500];
    int coint = 0;
    while((coint = bis.read(databyte,0,databyte.length)) != -1){
        bos.write(databyte,0,coint);
    }
    data = new String(bos.toByteArray(),"utf-8");
} catch (IOException e) {
    e.printStackTrace();
}
webView.loadDataWithBaseURL(null,data,"text/html","utf-8",null);
```

## WebView.getSettings()

WebView不像其他控件有直接的set属性的方法, 而是调用了一个设置 getSettings() 方法来完成的, getSettings() 方法返回的是一个 WebSettings 类, 这个类中有很多关于网页浏览的设置枚举项, 这其中某些设置可以由 WebView 进行更改, 所以在 WebView 中写入 getSettings() 方法获得一个 WebSettings 的对象, 对某些设置进行操作.我们可以直接使用getSettings()方法进行设置 :

```java
webView.getSettings().setSupportZoom(true);
```

也可由实例化的WebSettings对象进行操作 :

```java
WebSettings webSettings = webView.getSettings();
webSettings.setSupportZoom(true);
```

下面是一些常用的设置项 :

| 方法名                                               | 方法解释                                                     |
| :--------------------------------------------------- | :----------------------------------------------------------- |
| setSupportZoom(boolean support)                      | WebView是否支持使用屏幕控件或手势进行缩放, 默认是true,支持缩放 |
| setMediaPlaybackRequiresUserGesture(boolean require) | WebView是否通过手势触发播放媒体, 默认是true,通过手势触发     |
| setAllowFileAccess(boolean allow)                    | WebView内部是否允许访问文件,默认是true, 允许访问             |
| setLoadWithOverviewMode(boolean overview)            | WebView是否使用预览模式加载界面                              |
| setSaveFormData(boolean save)                        | WebView是否保存表单数据,默认true,保存数据                    |
| setTextZoom(int textZoom)                            | WebView中加载页面字体变焦百分比,默认100,100%                 |
| setStandardFontFamily(String font)                   | WebView标准字体库字体,默认字体“sans-serif”                   |
| setMinimumFontSize(int size)                         | WebView字体最小值,默认值8,取值1到72                          |
| setLoadsImagesAutomatically(boolean flag)            | WebView是否加载图片资源,默认true,自动加载图片                |
| setJavaScriptEnabled(boolean flag)                   | WebView是否允许执行JavaScript脚本,默认false,不允许执行       |
| setDefaultTextEncodingName(String encoding)          | WebView加载页面文本内容的编码,默认“UTF-8”                    |

**特别注意** 在设置 **setJavaScriptEnabled(boolean flag)** 方法时, 会Android studio 会报出如下警告 :

[![Warning](https://i.loli.net/2018/11/23/5bf797c86f700.png)](https://i.loli.net/2018/11/23/5bf797c86f700.png)

当你的WebView允许支持 JavaScript 时, 就会有一个网络安全隐患, XSS跨站脚本攻击, 有的网站内部会有一些, 使用JavaScript代码嵌入的钓鱼信息, 或则恶意广告, 可能会获取用户的一些隐私, 所以默认是不允许支持 JavaScript 脚本语言的. JavaScript 语言同时可以写出很多网页前端酷炫的特效, 如果不是想要体现这些特效的或者实现某些必要弹窗, Google是不建议WebView支持JavaScript语言的.

## 用户交互

**返回键**

在使用WebView时使用系统返回键按钮, 只能实现Activity的返回, 不能实现网页的返回, 所以提高用户的浏览体验, 通常会重写系统返回键按钮的

```java
/*重写系统返回按键的方法*/
@Override
public boolean onKeyDown(int keyCode, KeyEvent event) {
    if (keyCode == KeyEvent.KEYCODE_BACK) {
        if (webView.canGoBack()) {
            webView.goBack();
            return true;
        } else {
            System.exit(0);
        }
    }
    return super.onKeyDown(keyCode, event);
}
```

这里使用 **canGoBack()** 方法做了判断, 设置系统返回键是否进行网页返回.

**JavaScript调用Android方法**

在多数时候浏览的网页是带有JavaScript脚本代码的, 在这些代码必要存在时, 就需要让JavaScript代码调用一些Android层面的代码了.JavaScript代码中是name对象, 而Android是object对象, 所以需要允许支持JavaScript代码之后, 还要调用WebView的addJavascriptInterface方法将object对象体现在网页上作为JavaScript的name对象, 方便JavaScript层代码调用.

```java
public void addJavascriptInterface(Object object, String name)
```

第一个参数是object可以为自己定义的类, 第二个参数是字符串, 就是在JavaScript层代码实现的相对object对象的name对象.自定义一个类JavaScript_object

```java
import android.content.Context;
import android.webkit.JavascriptInterface;
import android.widget.Toast;

public class JavaScript_object {
    Context mContext;
    JavaScript_object(Context context){
        mContext = context;
    }
    @JavascriptInterface
    public void showToast(String str){
        Toast.makeText(mContext,str,Toast.LENGTH_LONG).show();
    }
}
```

根据网页JavaScript代码可以自行为这个类添加相关方法, 这里只写了一个打印Toast的方法,用@JavascriptInterface标识,就可以在JavaScript层代码实现了.

在HTML层代码中就可以添加

```java
<input type="button" value="打印土司"
       onclick="myObject.showToast('想打印的土司');" />
```

这里的myobject就是方法 addJavascriptInterface 的第二个参数.

**WebViewClient用户操作**

之前使用 setWebViewClient 方法让WebView打开Uri传入的网址, 那么调用此方法之后, 就可获得它的一个用户操作对象, 从而可以进行一些浏览器一样的方法, 这些方法也是开发浏览器的必须

```java
webView.setWebViewClient(new WebViewClient());
WebViewClient webViewClient = webView.getWebViewClient();
```

以下是一些用户操作的相关方法, 如果写一个浏览器, 也是会常用到这些方法的.

```java
//更新网页浏览的历史记录
doUpdateVisitedHistory(WebView view, String url, boolean isReload)

//WebView重新请求网页数据
onFormResubmission(WebView view, Message dontResend, Message resend)

//用户加载页面资源时调用
onLoadResource(WebView view, String url) 

//可以通过重写该方法, 加载一个loading页面或进度条的页面来提示用户正在加载
onPageStarted(WebView view, String url, Bitmap favicon) 

//在页面加载结束时, 关闭loading页面或进度条,实现跳转
onPageFinished(WebView view, String url)

//重写此方法可以让webview处理https请求
onReceivedSslError(WebView view, SslErrorHandler handler, SslError error)

//重写此方法才能够处理在浏览器中的按键事件 
shouldOverrideKeyEvent(WebView view, KeyEvent event)
    
//浏览网页的时候, 有的点击请求是打来新的网址, 这时又会询问,是调用系统浏览器打开还是直接WebView打开, 这是就需要重写这个方法进行显示
shouldOverrideUrlLoading(WebView view, String url)
```