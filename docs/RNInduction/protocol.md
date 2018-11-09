# 前言
React Native与传统的HybirdApp最大区别就是抛开WebView，使用JSC+原生组件的方式进行渲染，那么整个App启动/渲染流程又是怎样的呢？
# 一、整体框架
RN 这套框架让 JS开发者可以大部分使用JS代码就可以构建一个跨平台APP；
<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/send.png" width="375"/>

React 与 React native 的原理是相同的,都是由 javascript 实现的虚拟DOM 来驱动界面 View 的渲染,只不过 React.js 驱动 HTML DOM 的渲染, RN 是驱动 ios/android 原生组件的渲染


# 二、前端页面开发
## 1、js入口

```
import { AppRegistry } from "react-native";
import App from "./src/index";

AppRegistry.registerComponent("Wubacst", () => App);
```
把当前APP的对象注册到AppRegistry组件中， AppRegistry组件是js module

## 2、RN AppRegistry

- AppRegistry 是运行所有 React Native 应用程序的 JS 入口点
- 应用程序跟组件需要通过 AppRegistry.registerComponent 来注册它们自身
- 然后本地系统就可以加载应用程序的包，再然后当 AppRegistry.runApplication准备就绪后就可以真正的运行该应用程序了
- AppRegistry 在 require 序列里是 required，确保在其他模块被需求之前 JS 执行环境已经被 required


#### AppRegistry常用方法

```
//static静态方法,用来注册配置信息
static **registerConfig**(config: Array<AppConfig>) 

//static静态方法,注册组件
static **registerComponent**(appKey: string, getComponentFunc: Function) 

//static静态方法,注册线程
static **registerRunnable**(appKey: string, func: Function) 

//static静态方法,进行运行应用 
static **runApplication**(appKey: string, appParameters: any)

```

```
runApplication: function runApplication(appKey, appParameters) {
      var msg = 'Running application "' + appKey + '" with appParams: ' + JSON.stringify(appParameters) + '. ' + '__DEV__ === ' + String(__DEV__) + ', development-level warning are ' + (__DEV__ ? 'ON' : 'OFF') + ', performance optimizations are ' + (__DEV__ ? 'OFF' : 'ON');
      infoLog(msg);
      BugReporting.addSource('AppRegistry.runApplication' + runCount++, function () {
        return msg;
      });
      invariant(runnables[appKey] && runnables[appKey].run, 'Application ' + appKey + ' has not been registered.\n\n' + "Hint: This error often happens when you're running the packager " + '(local dev server) from a wrong folder. For example you have ' + 'multiple apps and the packager is still running for the app you ' + 'were working on before.\nIf this is the case, simply kill the old ' + 'packager instance (e.g. close the packager terminal window) ' + 'and start the packager in the correct app folder (e.g. cd into app ' + "folder and run 'npm start').\n\n" + 'This error can also happen due to a require() error during ' + 'initialization or failure to call AppReg
      istry.registerComponent.\n\n');
      SceneTracker.setActiveScene({
        name: appKey
      });
      runnables[appKey].run(appParameters);
    }
```

## 3、ReactDOM.render
ReactDOM.render是React的最基本方法用于将模板转为HTML语言，并插入指定的DOM节点。


> ReactDOM.render(template,targetDOM),该方法接收两个参数:
- 第一个是创建的模板
- 第二个参数是插入该模板的目标位置

```
ReactDOM.render(
  <Page />,
  document.getElementById('app')
);
```
## 4、RN组件的生命周期

像 Android和iOS 开发一样，React Native（RN） 中的组件也有生命周期（Lifecycle）；  
当应用启动，React Native在内存中维护着一个虚拟DOM，组件的生命周期就是指组件初始化并挂载到虚拟DOM为起始，到组件从虚拟DOM卸载为终结。生命周期的方法就是组件在虚拟DOM中不同状态的描述。
![image](http://upload-images.jianshu.io/upload_images/1417629-4ee80c3ad65de2a3.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
组件的生命周期分为三个阶段：
- 挂载（mounting
- 更新（updating）
- 卸载（Unmounting

#### 组件挂载
挂载指的是组件的实例被创建并插入到DOM中，挂载会调用如下方法。
挂载阶段调用方法：

```
constructor(props) {
  super(props);
  this.state = {
    text: '构造方法'
  };
}
```
constructor是RN组件的构造方法，它在RN组件被加载前先被调用。当我们的组件继承自React.Component时，需要在构造方法中最先调用super(props)。如果不需要初始化state，则不需要实现构造方法。

```
componentWillMount()
```
componentWillMount方法在挂载前被立即调用。它在render方法前被执行，因此，在componentWillMount方法中设置state并不会导致重新被渲染。它只会被执行一次

```
render()
```
渲染，render方法中不应该修改组件的props和state；render方法在更新阶段也会被调用，前提是shouldComponentUpdate方法返回true。

```
componentDidMount()
```
componentDidMount方法在组件被挂载后立即调用，在render方法后被执行  

可以在这个方法中获取其中的元素或者子组件，需要注意的是，子组件的componentDidMount方法会在父组件的componentDidMount方法之前调用

如果需要从网络加载数据显示到界面上，最好在这里进行网络请求。在componentDidMount方法中设置state将会被重新渲染。

#### 组件更新
改变props或者state时可以导致更新，当一个组件被重新渲染时，会调用如下方法。

```
componentWillReceiveProps(nextProps)
```
componentWillReceiveProps方法会在挂载的组件接收到新的props时被调用，它接收一个Object类型参数nextProps，表示新的props。通常在这个方法中接收新的props值，并根据props的变化，通过调用 this.setState() 来更新组件state，this.setState()不会触发 render方法的调用。

在挂载的过程中，初始的props并不会触发调用componentWillReceiveProps方法，这个方法只会在组件中的props更新时被调用，另外，调用this.setState()也不会触发调用componentWillReceiveProps方法。


```
shouldComponentUpdate(nextProps, nextState)
```
当组件接收到新的props和state时，shouldComponentUpdate方法被调用，它接收两个Object参数，nextProps是新的props，nextState是新的state。   

shouldComponentUpdate方法默认返回true，用来保证数据变化时，组件能够重新渲染。你也可以重载这个方法，通过检查变化前后props和state，来决定组件是否需要重新渲染。如果返回false，则组件不会被重新渲染，也不会调用本方法后面的componentWillUpdate和componentDidUpdate方法。


```
componentWillUpdate(nextProps, nextState)
```
如果组件props或者state改变，并且此前的shouldComponentUpdate方法返回为 true，则会调用该方法。componentWillUpdate方法会在组件重新渲染前被调用，因此，可以在这个方法中为重新渲染做一些准备工作。需要注意的是，在这个方法中，不能使用 this.setState 来更改state，如果想要根据props来更改state，需要在componentWillReceiveProps方法中去实现，而不是在这里


```
componentDidUpdate(prevProps, prevState)
```
组件重新渲染完成后会调用componentDidUpdate方法。两个参数分别是渲染前的props和渲染前的state。这个方法也适合写网络请求，比如可以将当前的props和prevProps进行对比，发生变化则请求网络。

#### 组件卸载
卸载就是从DOM中删除组件，会调用如下方法

```
componentWillUnmount()
```
componentWillUnmount方法在组件卸载和销毁之前被立即调用。可以在这个方法中执行必要的清理工作，比如，关掉计时器、取消网络请求、清除组件装载中创建的DOM元素等等。

# 三、React Native如何把React转化为元素的API
> React Native会在一开始生成OC模块表，然后把这个模块表传入JS中，JS参照模块表，就能间接调用OC的代码。

 相当于买了一个机器人（OC），对应一份说明书（模块表），用户（JS）参照说明书去执行机器人的操作

# 四、React Native如何如何做到JS和OC交互
> iOS原生API有个[JavaScriptCore](https://www.jianshu.com/p/bd7dfd8c9917)框架，通过它就能实现JS和OC交互

1. 首先写好前端jsx代码
2. 把jsx代码解析成JavaScript代码
3. OC读取JS文件
4. 把JavaScript读取出来，利用JavaScriptCore执行
5. javaScript代码返回一个数组，数组中会描述OC对象，OC对象的属性，OC对象所需要执行的方法，这样就能让这个对象设置属性，并且调用方法

![image](http://cc.cocimg.com/api/uploads/20170505/1493948661418415.png)

# 五、RN启动流程（iOS）

流程图：
![image](http://cc.cocimg.com/api/uploads/20170505/1493948699270531.png)
#### 1、创建RCTRootView
> 设置窗口根控制器的View,把RN的View添加到窗口上显示 

我们使用RCTRootView将React Natvie视图封装到原生组件中。RCTRootView是一个UIView容器，承载着React Native应用。同时它也提供了一个联通原生端和被托管端的接口

```
RCTRootView *rootView = [[RCTRootView alloc] initWithBridge:bridge
                                      moduleName:@"Wubacst"
                                     initialProperties:props];
```

```
import { AppRegistry } from "react-native";
import App from "./src/index";

AppRegistry.registerComponent("Wubacst", () => App);
```
## 2、创建RCTBridge
> 桥接对象,管理JS和OC交互，做中转左右

1、RCTBridgeModule  
在React Native中，如果实现一个原生模块，需要实现RCTBridgeModule”协议

2、RCT_EXPORT_MODULE()  
如果我们实现了RCTBridgeModule协议，我们的类需要包含RCT_EXPORT_MODULE()宏。这个宏也可以添加一个参数用来指定在Javascript中访问这个模块的名字。如果你不指定，默认就会使用这个Objective-C类的名字

3、RCT_EXPORT_METHOD()  
与此同时我们需要声明RCT_EXPORT_METHOD()宏来实现要给Javascript导出的方法，否则React Native不会导出任何方法。

native方法注册示例：
```
@implementation LoadPageManager
RCT_EXPORT_MODULE()

// RN跳转Native //
RCT_EXPORT_METHOD(showCSTNativePage:(NSDictionary*)commonParames
                  disappearCallBack:(RCTResponseSenderBlock) disappearCallBack)
{
    NSLog(@"start load");
    NSString* pagePath = [commonParames objectForKey:@"pagePath"];
    NSDictionary* aParams = [commonParames objectForKey:@"params"];
    NSString* aPostMessageEvent = [commonParames objectForKey:@"postMessageEvent"];
    NSInteger isCloseParent = [(NSNumber*)[commonParames objectForKey:@"isDestoryBeforePage"] intValue];
    NSString* aAppearEvent = [commonParames objectForKey:@"appearEvent"];
    
    NSMutableDictionary* aTmpDic = [NSMutableDictionary dictionaryWithDictionary:commonParames];
    // 以下代码不要删除 //
    NSMutableDictionary* aDic;
    if (aParams){
        aDic = [NSMutableDictionary dictionaryWithDictionary:aParams];
    }

    if (aPostMessageEvent){
        [aDic setValue:aPostMessageEvent forKey:PostEvent];
    }
   
    if (aAppearEvent){
        [aDic setValue:aAppearEvent forKey:AppearEvent];
    }
    // 以下代码不要删除 //
    
    if (disappearCallBack){
        [aTmpDic setValue:disappearCallBack forKey:DisappearCallBack];
    }
    
    
    if (isCloseParent){
        [self RNBackAction];
    }
    // 转一下子为了和之前Hybrid 一致 //
    pagePath = [pagePath stringByReplacingOccurrencesOfString:@"_" withString:@"/"];
    aDic = [self adpatToHybridParmers:[NSMutableDictionary dictionaryWithDictionary:aTmpDic]]; // 后期重做考虑去掉 //
    // 转一下子为了和之前Hybrid 一致 //
    
    dispatch_async(dispatch_get_main_queue(), ^{
       [[CHRDispatcher sharedDispatcher] invokeForPath:pagePath andParams: aDic];
    });
}
```
js调用示例：

```
import { NativeModules, Platform } from "react-native";
import checker from "../lib/checker";
let pageManager = NativeModules.LoadPageManager;
function nativeBridge(type, params, disappearCallBack) {
    pageManager[type](params, disappearCallBack);
}
//RN跳转native页面
function jumpNativePage(
    { pagePath, isDestoryBeforePage = 0, jumpParameter, appearEvent, postMessageEvent, ...other },
    disappearCallBack = (error, nativeData) => {} //回调函数
) {
    const error = checker(
        arguments,
        [
            {
                pagePath: "s|r",
                isDestoryBeforePage: "b",
                jumpParameter: "o",
                appearEvent: "s",
                postMessageEvent: "s",
                ...other
            },
            "f"
        ],
        "jumpNativePage"
    );
    // 线上环境报错不调起 Native 的 API
    if (error === "error") {
        return error;
    }
    let param = {
        pagePath,
        jumpParameter,
        appearEvent,
        postMessageEvent, //事件名称
        isDestoryBeforePage, //0:不关闭，1关闭
        ...other
    };
    nativeBridge("showCSTNativePage", param, disappearCallBack);
}
```
3、创建RCTBatchedBridge

> 批量桥接对象，JS和OC交互具体实现都在这个类中

4、执行[RCTBatchedBridge loadSource]
> 加载JS源码

5、执行[RCTBatchedBridge initModulesWithDispatchGroup
> 创建OC模块表

6、执行[RCTJSCExecutor injectJSONText]
> 往JS中插入OC模块表

7、执行完JS代码，回调OC，调用OC中的组件

8、完成UI渲染

# 六、RN UI控件渲染流程
1、RCTRootView runApplication:bridge
> 通知JS运行App

2、RCTBatchedBridge _processResponse:json error:error
> 处理执行完JS代码(runApplication)返回的相应，包含需要添加多少子控件的信息。

3、RCTBatchedBridge batchDidComplete
> RCTUIManager调用处理完成的方法，就会开始去加载rootView的子控件。

4、RCTUIManager createView:viewName:rootTag:props
> 通过JS执行OC代码，让UI管理者创建子控件View  

通过RCT_EXPORT_METHOD宏定义createView这个方法

```
RCT_EXPORT_METHOD(createView:(nonnull NSNumber *)reactTag
                  viewName:(NSString *)viewName
                  rootTag:(nonnull NSNumber *)rootTag
                  props:(NSDictionary *)props)
```
RCT_EXPORT_METHOD宏:会在JS中生成对应的OC方法，这样JS就能直接调用

注意每创建一个UIView,就会创建一个RCTShadowView，与UIView一一对应

RCTShadowView:保存对应UIView的布局和子控件,管理UIView的加载

5、[RCTUIManager _layoutAndMount]
> 布局RCTRootView和增加子控件

6、[RCTUIManager setChildren:reactTags:] 
> 给RCTRootView对应的RCTRootShadowView设置子控件

注意：此方法也是JS调用OC方法

7、[RCTRootShadowView insertReactSubview:view atIndex:index++] 
> 遍历子控件数组，给RCTRootShadowView插入所有子控件

8、[RCTShadowView processUpdatedProperties:parentProperties:] 
> 处理保存在RCTShadowView中属性，就会去布局RCTShadowView对应UIView的所有子控件

9、[RCTView didUpdateReactSubviews]
> 给原生View添加子控件

10、完成UI渲染

![image](http://cc.cocimg.com/api/uploads/20170505/1493948976223744.png)


# 七、RN通信机制
React Native用iOS自带的JavaScriptCore作为JS的解析引擎，但并没有用到JavaScriptCore提供的一些可以让JS与OC互调的特性，而是自己实现了一套机制，这套机制可以通用于所有JS引擎上，在没有JavaScriptCore的情况下也可以用webview代替，实际上项目里就已经有了用webview作为解析引擎的实现，应该是用于兼容iOS7以下没有JavascriptCore的版本



通信过程
> 所谓的通信其实就是js和oc两者如何相互调用传参等

- 程序一开始native会调用js的RCTDeviceEventEmitter.emit方法
- 分别发送'appStateDidChange' 和'networkStatusDidChange'两个事件

```
iOS端：
if (![connectionType isEqualToString:self->_connectionType] ||
      ![effectiveConnectionType isEqualToString:self->_effectiveConnectionType] ||
      ![status isEqualToString:self->_statusDeprecated]) {
    self->_connectionType = connectionType;
    self->_effectiveConnectionType = effectiveConnectionType;
    self->_statusDeprecated = status;
    [self sendEventWithName:@"networkStatusDidChange" body:@{@"connectionType": connectionType,
                                                             @"effectiveConnectionType": effectiveConnectionType,
                                                             @"network_info": status}];
  }
  
  if (![newState isEqualToString:_lastKnownState]) {
    _lastKnownState = newState;
    [self sendEventWithName:@"appStateDidChange"
                       body:@{@"app_state": _lastKnownState}];
  }
  //以上这些方法都是RN提供给iOS的在RCTAppState.m中
```

```
RN端：
if (type === 'change') {
          this._eventHandlers[type].set(handler, this.addListener('appStateDidChange', function (appStateData) {
            handler(appStateData.app_state);
          }));
        } else if (type === 'memoryWarning') {
          this._eventHandlers[type].set(handler, this.addListener('memoryWarning', handler));
        }
```

- 接着调用js的AppRegistry.runApplication方法启动js应用
- 然后js层就可以通过native提供的方法来 RCTUIManager.createView来创建视图了

## 2、RN端发消息到iOS的过程  
- 首先在iOS里面新建一个类LoadPageManager
- LoadPageManager会实现RCTBridgeModule协议
- 在LoadPageManager累的实现中添加宏定义：RCT_EXPORT_MODULE() 
- RCT_EXPORT_MODULE()如果你不传入参数，那么你在iOS中导出的模块名就是类名，你也可以插入参数作为自定义模块名


```
iOS端代码
@implementation LoadPageManager
//可以指定一个参数来访问这个模块,不指定就是这个类的名字(ExampleInterface)
RCT_EXPORT_MODULE()
@end
```
接下来就可以实现协议的代理方法了，协议方法的实现需要在RCT_EXPORT_METHOD这个宏里面；下面以一个页面跳转的方法示例：

```
@implementation LoadPageManager
RCT_EXPORT_MODULE()

// RN跳转Native //
// showCSTNativePage和前端交互的具体方法宏定义  //
// 在方法宏定义里面约定和前端交互的参数传递格式 //
RCT_EXPORT_METHOD(showCSTNativePage:(NSDictionary*)commonParames
                  disappearCallBack:(RCTResponseSenderBlock) disappearCallBack)
{
    NSLog(@"start load");
    //接收解析前端传参
    NSString* pagePath = [commonParames objectForKey:@"pagePath"];
    NSDictionary* aParams = [commonParames objectForKey:@"params"];
    NSString* aPostMessageEvent = [commonParames objectForKey:@"postMessageEvent"];
    NSInteger isCloseParent = [(NSNumber*)[commonParames objectForKey:@"isDestoryBeforePage"] intValue];
    NSString* aAppearEvent = [commonParames objectForKey:@"appearEvent"];
    
    NSMutableDictionary* aTmpDic = [NSMutableDictionary dictionaryWithDictionary:commonParames];
    // 以下代码不要删除 //
    NSMutableDictionary* aDic;
    if (aParams){
        aDic = [NSMutableDictionary dictionaryWithDictionary:aParams];
    }

    if (aPostMessageEvent){
        [aDic setValue:aPostMessageEvent forKey:PostEvent];
    }
   
    if (aAppearEvent){
        [aDic setValue:aAppearEvent forKey:AppearEvent];
    }
    // 以下代码不要删除 //
    //回调函数，页面返回时执行
    if (disappearCallBack){
        [aTmpDic setValue:disappearCallBack forKey:DisappearCallBack];
    }
    
    
    if (isCloseParent){
        [self RNBackAction];
    }
    // 转一下子为了和之前Hybrid 一致 //
    pagePath = [pagePath stringByReplacingOccurrencesOfString:@"_" withString:@"/"];
    aDic = [self adpatToHybridParmers:[NSMutableDictionary dictionaryWithDictionary:aTmpDic]]; // 后期重做考虑去掉 //
    // 转一下子为了和之前Hybrid 一致 //
    
    dispatch_async(dispatch_get_main_queue(), ^{
       [[CHRDispatcher sharedDispatcher] invokeForPath:pagePath andParams: aDic];
    });
}
```


RCTBatchedBridge
> 为了桥接js跟native，native层引入了RCTBridge这个类负责双方的通信，不过真正起作用的是RCTBatchedBridge这个类，这个类应该算是比较重要的一个类了

通信在前端的实现：  
> RN层Libraries/BatchedBridge包下面有3个JS文件：BatchedBridge.js、MessageQueue.js、NativeModules.js，它们封装了通信桥在RN层的实现。 

![image](https://note.youdao.com/yws/public/resource/888aadc0af7f68a91144fa1405fc7e7d/xmlnote/13B698EF6E4B4512BC5D43EF56B94654/7379)

##### BatchedBridge.js

```
'use strict';

const MessageQueue = require('MessageQueue');

// MessageQueue can install a global handler to catch all exceptions where JS users can register their own behavior
// This handler makes all exceptions to be handled inside MessageQueue rather than by the VM at its origin
// This makes stacktraces to be placed at MessageQueue rather than at where they were launched
// The parameter __fbUninstallRNGlobalErrorHandler is passed to MessageQueue to prevent the handler from being installed
//
// __fbUninstallRNGlobalErrorHandler is conditionally set by the Inspector while the VM is paused for initialization
// If the Inspector isn't present it defaults to undefined and the global error handler is installed
// The Inspector can still call MessageQueue#uninstallGlobalErrorHandler to uninstalled on attach

const BatchedBridge = new MessageQueue(
  // $FlowFixMe
  typeof __fbUninstallRNGlobalErrorHandler !== 'undefined' &&
    __fbUninstallRNGlobalErrorHandler === true, // eslint-disable-line no-undef
);

// Wire up the batched bridge on the global object so that we can call into it.
// Ideally, this would be the inverse relationship. I.e. the native environment
// provides this global directly with its script embedded. Then this module
// would export it. A possible fix would be to trim the dependencies in
// MessageQueue to its minimal features and embed that in the native runtime.

Object.defineProperty(global, '__fbBatchedBridge', {
  configurable: true,
  value: BatchedBridge,
});

module.exports = BatchedBridge;
```
可以看到在BatchedBridge中创建了MessageQueue对象

##### MessageQueue.js
MessageQueue的构造方法

```
class MessageQueue {
  //Js模块注册添加到_lazyCallableModules
  _lazyCallableModules: {[key: string]: (void) => Object};
  //队列，分别存放要调用的模块数组、方法数组、参数数组与数组大小
  _queue: [number[], number[], any[], number];
  //回调函数数组，与_quueue一一对应，每个_queue中调用的方法，如果有回调方法，那么就在这个数组对应的下标上。
  _successCallbacks: (?Function)[];
  _failureCallbacks: (?Function)[];
  //调用函数ID，自动增加。
  _callID: number;
  _inCall: number;
  _lastFlush: number;
  _eventLoopStartTime: number;

  _debugInfo: {[number]: [number, number
  //Module Table，用于Native Module
  _remoteModuleTable: {[number]: string};
  //Method Table，用于Native Module
  _remoteMethodTable: {[number]: string[]};

  __spy: ?(data: SpyData) => void;

  __guard: (() => void) => void;

  constructor(shouldUninstallGlobalErrorHandler: boolean = false) {
    this._lazyCallableModules = {};
    this._queue = [[], [], [], 0];
    this._successCallbacks = [];
    this._failureCallbacks = [];
    this._callID = 0;
    this._lastFlush = 0;
    this._eventLoopStartTime = new Date().getTime();
    if (shouldUninstallGlobalErrorHandler) {
      this.uninstallGlobalErrorHandler();
    } else {
      this.installGlobalErrorHandler();
    }

    if (__DEV__) {
      this._debugInfo = {};
      this._remoteModuleTable = {};
      this._remoteMethodTable = {};
    }
    
    //绑定函数，也就是创建一个函数，使这个函数不论怎么调用都有同样的this值
    (this: any).callFunctionReturnFlushedQueue = this.callFunctionReturnFlushedQueue.bind(
      this,
    );
    (this: any).callFunctionReturnResultAndFlushedQueue = this.callFunctionReturnResultAndFlushedQueue.bind(
      this,
    );
    (this: any).flushedQueue = this.flushedQueue.bind(this);
    (this: any).invokeCallbackAndReturnFlushedQueue = this.invokeCallbackAndReturnFlushedQueue.bind(
      this,
    );
  }
}
```
##### NativeModules.js
> NativeModules描述了Module的结构，用于解析并生成Module。

Module的结构定义如下所示

```
type ModuleConfig = [
  string, /* name */
  ?Object, /* constants */
  Array<string>, /* functions */
  Array<number>, /* promise method IDs */
  Array<number>, /* sync method IDs */
];
```



> RN通过NativeModules来实现传输和接受消息
> 首先前端页面要引入NativeModules模块

```
import { NativeModules, Platform } from "react-native";
```
然后在我们需要使用的时候获取导出的模块，我们再用模块调用iOS的导出的函数名就可以了

```
let pageManager = NativeModules.LoadPageManager;
function nativeBridge(type, params, disappearCallBack) {
    pageManager[type](params, disappearCallBack);
}
//RN跳转native页面
function jumpNativePage(
    { pagePath, isDestoryBeforePage = 0, jumpParameter, appearEvent, postMessageEvent, ...other },
    disappearCallBack = (error, nativeData) => {} //回调函数
) {
    const error = checker(
        arguments,
        [
            {
                pagePath: "s|r",
                isDestoryBeforePage: "b",
                jumpParameter: "o",
                appearEvent: "s",
                postMessageEvent: "s",
                ...other
            },
            "f"
        ],
        "jumpNativePage"
    );
    // 线上环境报错不调起 Native 的 API
    if (error === "error") {
        return error;
    }
    let param = {
        pagePath,
        jumpParameter,
        appearEvent,
        postMessageEvent, //事件名称
        isDestoryBeforePage, //0:不关闭，1关闭
        ...other
    };
    nativeBridge("showCSTNativePage", param, disappearCallBack);
}
//showCSTNativePage约定此方法接收两个参数，一个是页面跳转需要的所有必填参数，是一个json对象，一个是页面返回时的回调函数，是Function类型
```
整个调用过程
![image](http://upload-images.jianshu.io/upload_images/458529-55bd1d087f5b19af.png?imageMogr2/auto-orient/strip%7CimageView2/2/w/1240)
## 3、native给RN主动发送事件
[官网](https://reactnative.cn/docs/0.51/native-modules-ios.html#content)解释：  
最好的方法是继承RCTEventEmitter，实现suppportEvents方法并调用self sendEventWithName:。

JavaScript代码可以创建一个包含你的模块的NativeEventEmitter实例来订阅这些事件

iOS端导出方法

```
@implementation NativeNotification
RCT_EXPORT_MODULE();

- (NSArray<NSString*>*) supportedEvents
{
    return @[AppearEvent];
}
```
RN端调用

```
import { NativeEventEmitter, NativeModules } from "react-native";
//NativeNotification是native监听事件的方法定义宏
//addListener是native监听事件的模块定义宏
const { NativeNotification } = NativeModules;
const eventManagerEmitter = new NativeEventEmitter(NativeNotification);

//在组件中使用
  componentWillMount() {
    this.listener = eventManagerEmitter.addListener('viewAppear', (nativeData)=>{
    //nativeData native端返回数据
    console.log(JSON.stringify(nativeData))
    });  //对应了原生端的名字
  }
  //记得remove
  componentWillUnmount() {
    this.listener && this.listener.remove();  
    
```

总结：  
其实就是iOS端设置了桥接内容，将收到的通知内容传递给RN，然后RN监听有没有对应Name的事件名，有的话就处理