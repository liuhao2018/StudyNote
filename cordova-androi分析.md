### 1.Cordova Android源码概要：

#####   Cordova是一个专业的移动应用开发框架，是一个全面的WEB APP开发的框架，提供以WEB形式来访问终端设备的API功能。Cordova只是个原生外壳，APP的内核是一个完整的WEB APP。需要调用的原生功能将以原生插件的形式实现，以暴露js接口的方式调用。

#####    Cordova Android项目是Cordova Android原生部分的Java代码实现，提供了Android原生代码和上层Web页面的javascript通讯接口。从Cordova3.0版本以后，所有的设备能力API都从Cordova核心框架抽离出去，变成了插件，各平台分别进行原生实现。

- CordovaInterface接口分析：

  CordovaInterface是Cordova应用的底层接口，CordovaActivity需要实现这个接口。用来隔离Cordova插件开发，隔离插件对Cordova核心库的直接依赖。主要方法有startActivityForResult，setActivityResultCallback，getActivity，onMessage，getThreadPool这几个接口方法。具体的实现在CordovaActivity中。

  ​

- CordovaActivity核心分析：

  CordovaActivity是Cordova应用的入口类，用户用来加载html页面Activity需要加载这个Activity。CordovaActivity会读取Cordova配置文件res/xml/config.xml中的配置。CordovaActivity继承了Android Activity，实现了CordovaInterface接口。比较重要的成员变量有CordovaWebView appView，CordovaWebViewClicnt webViewClient,线程池threadPool。CordovaActivity继承了Activity,因此它的生命周期和Activity一样。

  ​

- CordovaWebView类分析：

  CordovaWebView类继承了Android WebView类，包含了PluginManager pluginManager,BroadcastReceiver receiver,CordovaResourceApi resourceApi等重要的成员变量，与其他核心类关联起来。接着，按情况分别调用自身的setWebChromeClient,initWebViewClient,loadConfiguration,setup方法。

  ​

- CordovaWebViewClient类分析：

  CordovaWebViewClient类继承了android WebViewClient，实现了CordovaWebView的回调函数，这些回调函数在渲染文档的过程中会被触发，例如onPageStarted(),shouldOverrideUrlLoading()等方法。

  ​

- CordovaResourceApi类分析：

  Cordova Android使用okhttp框架作为网络访问框架，其封装了okhttp。  

  ​

- CordovaPlugin类：

  在自定义插件时必须继承这个类，重写excute(String action,JsonArray args,CallbackContext callbackContext)方法。

  ​

- CordovaManager类:

  插件管理器。


### 2.自定义插件开发：

##### 在使用cordova的过程中，虽然官方提供的插件以及第三方开源的插件较多，但是，有时候为了实现某种需求，还是需要自己来编写插件。这里介绍官方提供的plugman来创建插件。

##### plugman的使用：

- 首先，安装plugman

  ```
  npm install -g plugman
  ```

- 创建plugin

  ```
  plugman create --name HelloPlugin --plugin_id helloPlugin --plugin_version 0.0.1
  ```

  此命令会在当前目录创建一个helloPlugin插件。

- 在插件开发完成之后，将插件添加到项目中去，运行如下命令：

  ```
  cordova plugin add /Users/liuhao/Desktop/cordova-plugin/HelloPlugin
  ```

##### 下面以Android中Toast的例子完整演示插件开发过程：

- 配置plugin.xml:

  ```xml
  <?xml version="1.0" encoding="UTF-8"?>
  <plugin xmlns="http://apache.org/cordova/ns/plugins/1.0"
          xmlns:android="http://schemas.android.com/apk/res/android"
          id="cordova-plugin-toaster"
          version="0.0.1">
      <name>Toaster Plugin</name>
      <description>Cordova Toaster Plugin</description>
      <license>Apache 2.0</license>
      <keywords>cordova,toaster, toast, toasts, android, notification, ionic</keywords>
      <repo>https://github.com/yaroslav0507/cordova-plugin-toaster.git</repo>
      <issue>https://github.com/yaroslav0507/cordova-plugin-toaster/issues</issue>

      <js-module src="www/toaster.js" name="notification">
          <merges target="navigator.notification" />
          <merges target="navigator" />
      </js-module>

      <!-- android -->
      <platform name="android">
          <config-file target="res/xml/config.xml" parent="/*">
              <feature name="Toaster" >
                  <param name="android-package" value="Toaster"/>
              </feature>
          </config-file>
          <source-file src="src/android/Toaster.java" target-dir="src/" />
      </platform>
  </plugin>
  ```

- toast.js-中间件：

  ```javascript
  var exec = require('cordova/exec');

  module.exports = {

      showToast: function(param) {
      	return new Promise(function(resolve, reject) {
      		exec(function(result) {
      			resolve(result);
      		},
      		null,
      		"Toaster",
  		param,
      		[]);
      	});   
      }
  };
  ```

- Toaster.java-原生java类：

  ```java
  import org.apache.cordova.CordovaWebView;
  import org.apache.cordova.CallbackContext;
  import org.apache.cordova.CordovaPlugin;
  import org.apache.cordova.CordovaInterface;
  import android.provider.Settings;
  import android.widget.Toast;
  import org.json.JSONArray;
  import org.json.JSONException;
  import org.json.JSONObject;

  public class Toaster extends CordovaPlugin {

    public static final String TAG = "Toaster";

    public Toaster() {}

    public void initialize(CordovaInterface cordova, CordovaWebView webView) {
      super.initialize(cordova, webView);
    }

    public boolean execute(final String action, JSONArray args, CallbackContext callbackContext) throws JSONException {

      final int duration = Toast.LENGTH_SHORT;

      cordova.getActivity().runOnUiThread(new Runnable() {
        public void run() {
          Toast toast = Toast.makeText(cordova.getActivity().getApplicationContext(), action, duration);
          toast.show();
        }
      });

      return true;
    }
  }
  ```

- Test：

  ```javascript
  <script type="text/javascript">
    doucument.addEventListener("deviceready",onDeviceReady,false);
    function onDeviceReady(){
      navigator.showToast('Your toast test here');
    }
  </script>
  ```

  ​


### 3.事件（deviceready）：

##### 采用Cordova开发的应用在运行的时候，Cordova提供的HTML5调用Native的功能并不是立即就能使用的，Cordova框架在读入HTML5代码之后，要进行HTML5和Native建立桥接，在未能完成这个桥接的初始的情况下，是不能调用Native功能的。在Cordova框架中，当这个桥接的初始化完成后，会调用他自身特有的事件，即`devaceready`事件。

code：

```javascript
document.addEventListener('deviceready',function(){
  	console.log('Device is Ready!');
},false);
```

`需要注意的是，decviceready事件是每次读入HTML的时候都会被调用，而不只是应用启动时调用。`

除了deviceready事件以外，Cordova应用在内部读取HTML代码的时候还会调用一些其他的事件，但这些并不是Cordova框架提供的事件，而是嵌入的Webview的浏览器Render引擎提供的。

- DOMContentLoaded事件

  页面的DOM内容加载完成后触发，而无需等待其他资源（CSS，JS)的加载。

- load事件

  在DOMContentLoaded事件之后，其他资源加载完成后触发。

所以，其实调用的顺序是DOMContentLoaded->load->deviceready。deviceready事件一定是在load事件之后，所以load事件的执行速度会影响到deviceready事件的调用。把一些不必要的资源可以放在deviceready事件之后调用从而提高执行速度。

code：

```javascript
document.addEventListener('DOMContentLoaded', function () {  
  console.log('DOMContentLoaded OK!');  
}, false);  
  
window.addEventListener('load', function () {  
  console.log('load OK!');  
}, false);  
  
document.addEventListener('deviceready', function () {  
  console.log('deviceready OK!');  
}, false); 
```



### 4.开发中需要注意的地方：

##### 因为Cordova的开源性质和产品定位，使得每个人都可以开发自己的Cordova插件，这也就导致了目前Cordova插件质量残次不齐。所以需要注意一下几个方面：

- 首先，和硬件交互相关的场景，最好使用Cordova的官方插件，这些插件都是经过了实践的检验的，基本上不会有什么太大的问题。具体的插件列表查看[官方发布日志](https://cordova.apache.org/news/2016/04/20/plugins-release.html)
- 目前，国内的推送厂商只有极光推送官方推出了Cordova插件，github:[https://github.com/jpush/jpush-phonegap-plugin](https://github.com/jpush/jpush-phonegap-plugin)。
- github上有一位国外开发者开发了一些Cordova插件，star数不错。github:[https://github.com/katzer](https://github.com/katzer)。

