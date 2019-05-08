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

```js
// 方式一：通过协议调用，利用 UIWebview 可以通过 shouldStartLoadWithRequest 拦截所有网络请求的特点

// 方式二：通过 Native 注入 API，比如用 WebViewJavascriptBridge

```

```swift
func webView (webView)
```

**iOS（native => h5）**

```js
// 利用 UIWebview 的 API
webview.stringByEvaluatingJavaScriptByString('window.onPageShow()')
```

**Android（h5 => native）**

```js
// 方式一，通过协议调用，利用 webview 可以通过 shouldOverrideUrlLoading 拦截所有网络请求的特点


// 方式二，通过 native 注入 API，用 addJavascriptInterface

class JSInterface {
  @JavascriptInterface
  public String getUserData () {
    return 'userData'
  }
}
webview.addJavascriptInterface(new JSInterface(), 'jsBridge')

window.jsBridge.getUserData() // 'userData'

// 方式三，通过覆写 prompt 方法（alert、console.log 也行，但肯定不能覆写）
onJsPrompt
```

**Android（native => h5）**

```js
// 利用 webview 的 loadUrl API
webView.loadUrl('javascript:onPageShow()')

// 利用 evaluateJavascript API
```

### 容器的其它关键能力和应用

**加载性能**

通过离线缓存、预加载提高页面首屏性能
