# 跳转H5页面

>iOS version >= 3.5.1

从RN页面调起想要跳转的H5页面

### 协议引入
```
import WBCSTAPP from "@/rn-app";//引入对象名称可以自定义
//如果需要native事件监听
//NativeModules native注册的方法都在NativeModules里面
//JavaScript代码可以创建一个包含你的模块的NativeEventEmitter实例来订阅这些事件
//即使没有被JavaScript调用，原生模块也可以给JavaScript发送事件通知
import { NativeEventEmitter, NativeModules } from "react-native";
//NativeNotification是native监听事件的方法定义宏
//addListener是native监听事件的模块定义宏
const { NativeNotification } = NativeModules;
const eventManagerEmitter = new NativeEventEmitter(NativeNotification);
```

### 协议设计
```
loadPage(
    {
        url,
        isDestoryBeforePage = 0,
        jumpParameter,
        appearEvent,
        title,
        ...other
    },
    disappearCallBack //回调函数
)
```

### 协议调用
```
WBCSTAPP.loadPage(
    {
        url: "https://carprice.m.58.com/carsample/index.html#/priceCollect",
        appearEvent: "viewData",
        isDestoryBeforePage: 0,
        title: "金牌估价师",
        jumpParameter: {
            //跳转目的页面需要携带参数可在这里设置
        }
    },
    () => {
        //回调函数 可以直接匿名函数 也可以定义好方法传进来
    }
);
```

### 参数说明
```
param:{
    pagePath(String):跳转的web的url 
    jumpParameter(Object/ Dictionary):跳转页面需要携带的参数，具体参数根据自己业务需求设置
    isDestoryBeforePage(number): 上级页面是否需要关闭 0:不关闭 1：关闭 默认0
    title(String):要跳转的目的页面标题
    appearEvent(String):在native端viewWillAppear的时候发送消息给Native端，RN 端需要通过

    data.viewAppear，来获取值是否和RN之前传的进行比较如果一致执行，不一致的不执行，

    viewAppear参数格式为：当前类名称+调起模块名称（rn 为bundleid）+ viewAppear

    备注：从A页面跳转B页面，需要B页面携带数据给A页面可以设置appearEvent，在B页面即将离开时发消息给A页面，

    A页面设置监听事件：

    eventManagerEmitter.addListener("viewAppear", (nativeData) =>
    //nativeData native端返回数据
    console.log(JSON.stringify(nativeData))
    );
    注意：appearEvent监听事件固定写死“viewAppear”，在组件卸载时一定要移除监听事件

    componentWillUnmount() {
        eventManagerEmitter.removeAllListerers(this);
    }
}
disappearCallBack:回调方法
可以直接匿名函数 也可以定义好方法传进来
```


| 详细参数            | 类型               | 默认值 | 描述                                                         |
| ------------------- | ------------------ | ------ | ------------------------------------------------------------ |
| pagePath            | String             | 无     | 跳转的web的url                                               |
| jumpParameter       | Object/ Dictionary | 无     | 跳转页面需要携带的参数，具体参数根据自己业务需求设置         |
| isDestoryBeforePage | int                | 0      | 上级页面是否需要关闭 0:不关闭 1：关闭                        |
| `title`             | String             | 无     | 要跳转的目的页面标题                                         |
| appearEvent         | String             | 无     | 在native端viewWillAppear的时候发送消息给Native端，RN 端需要通过data.viewAppear，来获取值是否和RN之前传的进行比较如果一致执行，不一致的不执行，viewAppear参数格式为：当前类名称+调起模块名称（rn 为bundleid）+ viewAppear备注：从A页面跳转B页面，需要B页面携带数据给A页面可以设置appearEvent，在B页面即将离开时发消息给A页面，A页面设置监听事件：eventManagerEmitter<br />.addListener("viewAppear", (nativeData) =><br />//nativeData native端返回数据<br />)<br />注意：appearEvent监听事件固定写死“viewAppear”，在组件卸载时一定要移除监听事件<br />componentWillUnmount() {<br />  eventManagerEmitter.removeAllListerers(this);<br />} |
| disappearCallBack   | funtion            | 无     | 回调方法`可以直接匿名函数 也可以定义好方法传进来`            |




