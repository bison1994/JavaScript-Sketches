### 混合开发

native => 体验好、效率差（多平台、受版本限制，无法自由更新）

h5 => 体验差、效率好（跨平台、不受版本限制，随时更新）

混合开发主要是回避 native 效率差、成本高的问题，同时为了保持 native 的同等功能，需要借助容器的能力。混合开发的核心就是容器（或者说桥的开发）

桥，显然是指双向通信

- h5 => native（h5 调用 native 的 API）
  + 容器内 js 调用某个方法，能够被 native 监听、捕获、拦截
  + 容器内 js 直接调用 native 注入的方法（给浏览器环境新增 API）
- native => h5（native 调用容器内方法，应用场景：h5 监听 native 的事件）

典型应用：ionic、wexin js sdk


### JS Bridge 实现原理

**iOS（h5 => native）**

方式一：通过协议调用，利用 UIWebview 可以通过 shouldStartLoadWithRequest 拦截网络请求的特点

```js
// 也可以用 location.href 发起请求，但问题是多个请求同时发起时，native 只能处理最后一个，而忽略前面的请求
var url = 'scheme://action?param=xxx'
var iframe = document.createElement('iframe')
iframe.style.width = '1px'
iframe.style.height = '1px'
iframe.style.display = 'none'
iframe.src = url
document.body.appendChild(iframe)
setTimeout(function() {
  iframe.remove()
}, 0)
```

```swift
func webView (webView: UIWebView, shouldStartLoadWithRequest request: NSURLRequest, navigationType: UIWebViewNavigationType) -> Bool {
  let url = request.URL
  let scheme = url?.scheme
  let method = url?.host
  let query = url?.query
}
```


方式二：通过 Native 注入 API，比如用 WebViewJavascriptBridge

**iOS（native => h5）**

```js
// 利用 UIWebview 的 API
webview.stringByEvaluatingJavaScriptByString('window.onPageShow()')
```


**Android（h5 => native）**

方式一，通过协议调用，利用 webview 可以通过 [shouldOverrideUrlLoading](https://developer.android.com/reference/android/webkit/WebViewClient.html#shouldOverrideUrlLoading(android.webkit.WebView,%2520android.webkit.WebResourceRequest)) 拦截网络请求的特点

```java
private class MyWebViewClient extends WebViewClient {
  @Override
  public boolean shouldOverrideUrlLoading (WebView view, WebResourceRequest request) {
    // do some stuff
  }
}
```

> 对比 [shouldInterceptRequest](https://developer.android.com/reference/android/webkit/WebViewClient.html#shouldInterceptRequest(android.webkit.WebView,%2520android.webkit.WebResourceRequest))


方式二，通过 native 注入 API，用 `addJavascriptInterface`

```java
class JSInterface {
  @JavascriptInterface
  public String getUserData () {
    return 'userData'
  }
}
webview.addJavascriptInterface(new JSInterface(), 'jsBridge')

window.jsBridge.getUserData() // 'userData'
```


方式三，通过覆写 prompt 方法（alert、console.log 也行，但实践中肯定不能这么搞）

```java
class CustomWebChromeClient extends WebChromeClient {
  @Override
  public boolean onJsPrompt(WebView view, String url, String message, String defaultValue, JsPromptResult result) {
    // do some stuff
  }
}
```


**Android（native => h5）**

方式一：利用 webview 的 loadUrl API

```js
webView.loadUrl('javascript:onPageShow()')
```


方式二：利用 [evaluateJavascript](https://developer.android.com/reference/android/webkit/WebView#evaluateJavascript(java.lang.String,%2520android.webkit.ValueCallback%3Cjava.lang.String%3E)) API


**Webview Customization**

可以通过 android.webkit.WebSettings 修改 webview


### 容器的其它关键能力和应用

**加载性能**

通过离线缓存、预加载提高页面首屏性能
