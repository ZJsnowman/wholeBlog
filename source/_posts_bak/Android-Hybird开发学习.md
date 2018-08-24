---
date: 2017-07-17 15:53:55
title: Android Hybrid 开发学习笔记
tags: [Android]
---
学习 Hybrid过程中的一些笔记<!--more-->

-  WebView学习
- h5部分学习
- 框架学习(google 搜索JsBridge)

- 注意事项  安全问题
- web与native通信
同步调用
  addJavascriptInterface
  对话框机制
异步调用
  先同步再LoadUrl(javascript:xxxxx);
  js定时器轮询
  socket通信


## WebView学习
> 主要参考[Android官方](https://developer.android.com/guide/webapps/index.html)
一来权威，二者全面。通过学习这个可以从整体上对webview有个了解，知道webview基本用法，具体能做
什么事以及一些注意事项。
####  错误处理
```java
webview.setWebViewClient(new WebViewClient() {
  public void onReceivedError(WebView view, int errorCode, String description, String failingUrl) {
    Toast.makeText(activity, "Oh no! " + description, Toast.LENGTH_SHORT).show();
  }
});
```
#### 导航
```java
@Override
public boolean onKeyDown(int keyCode, KeyEvent event) {
    // Check if the key event was the Back button and if there's history
    if ((keyCode == KeyEvent.KEYCODE_BACK) && myWebView.canGoBack()) {
        myWebView.goBack();
        return true;
    }
    // If it wasn't the Back key or there's no web page history, bubble up to the default
    // system behavior (probably exit the activity)
    return super.onKeyDown(keyCode, event);
}
```
通过canGoBack(),cangoForward()方法来判断是否有个一访问历史，如果可以通过goBack和goForWard()来前进后退。
#### 加载JS

1. Android 定义给h5的方法类
```java
public class WebAppInterface {
    Context mContext;

    /** Instantiate the interface and set the context */
    WebAppInterface(Context c) {
        mContext = c;
    }

    /** Show a toast from the web page */
    @JavascriptInterface
    public void showToast(String toast) {
        Toast.makeText(mContext, toast, Toast.LENGTH_SHORT).show();
    }
}
```
> Caution: If you've set your ** targetSdkVersion ** to 17 or higher, you must add the @JavascriptInterface annotation to any method that you want available to your JavaScript (the method must also be public). If you do not provide the annotation, the method is not accessible by your web page when running on Android 4.2 or higher.

2. 将Java类绑定给JS
```Java
WebView webView = (WebView) findViewById(R.id.webview);
webView.addJavascriptInterface(new WebAppInterface(this), "AndroidToast");
```

3. h5调用
``` html
<!DOCTYPE html>
<html>
  <head>
    <meta charset="utf-8">
    <title></title>
  </head>
  <body>
    <input type="button" value="showAndroidToast" onClick="showAndroidToast('Hello Android!')" />

    <script type="text/javascript">
        function showAndroidToast(toast) {
            AndroidToast.showToast(toast);
        }
    </script>
  </body>
</html>
```


#### 安全问题
#### 运行线程  如何更新native UI
 JS执行时不在UI线程的（在JavaBridge线程中）。Android这边提供给h5调用方法也就不在主线程中执行。所以最好不要直接更新UI,虽然事实上可行，但是会有警告。

#### 网页内链接处理

```java
WebView myWebView = (WebView) findViewById(R.id.webview);
myWebView.setWebViewClient(new MyWebViewClient());
```

```java
private class MyWebViewClient extends WebViewClient {
    @Override
    public boolean shouldOverrideUrlLoading(WebView view, String url) {
        if (Uri.parse(url).getHost().equals("www.example.com")) {
            // This is my web site, so do not override; let my WebView load the page
            return false;
        }
        // Otherwise, the link is not for a page on my site, so launch another Activity that handles URLs
        Intent intent = new Intent(Intent.ACTION_VIEW, Uri.parse(url));
        startActivity(intent);
        return true;
    }
}
```

通过重写shouldOverrideUrlLoading() 方法，来控制点击网页内链接的行为。false为自己的webview处理，即在webview打开。true则用系统浏览器打开。通过判断可是实现不同的url地址有不同的处理行为。


#### 处理私有协议

#### Debug
客户端这边可以通过在Android本地打印出前端JS输出的日志。可以快速定位问题。
```java
WebView myWebView = (WebView) findViewById(R.id.webview);
myWebView.setWebChromeClient(new WebChromeClient() {
  public boolean onConsoleMessage(ConsoleMessage cm) {
    Log.d("MyApplication", cm.message() + " -- From line "
                         + cm.lineNumber() + " of "
                         + cm.sourceId() );
    return true;
  }
});
```
