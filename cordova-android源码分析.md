##### Cordova Android源码分析

> Cordova是一个专业的移动应用开发框架，是一个全面的WEB APP开发的框架，提供以WEB形式来访问终端设备的API功能。Cordova只是个原生外壳，APP的内核是一个完整的WEB APP。需要调用的原生功能将以原生插件的形式实现，以暴露js接口的方式调用。

Cordova Android项目是Cordova Android原生部分的Java代码实现，提供了Android原生代码和上层Web页面的javascript通讯接口。



- CordovaInterface接口分析：

  CordovaInterface是Cordova应用的底层接口，CordovaActivity需要实现这个接口。用来隔离Cordova插件开发，隔离插件对Cordova核心库的直接依赖。

- CordovaActivity核心库分析：

  CordovaActivity是Cordova应用的入口类，用户用来加载html页面Activity需要加载这个Activity。CordovaActivity会读取Cordova配置文件res/xml/config.xml中的配置。

  CordovaActivity继承了Android Activity，实现了CordovaInterface接口。

  比较重要的成员变量有CordovaWebView appView，CordovaWebViewClicnt webViewClient,线程池threadPool。CordovaActivity继承了Activity,因此它的生命周期和Activity一样。

- ​

  ​

  ​

