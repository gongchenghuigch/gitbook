# 跳转Native页面

> iOS version >= 3.5.1

从RN页面调起想要跳转的native页面

## 协议引入

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

## 协议设计

```
jumpNativePage(
    {
        pagePath,
        isDestoryBeforePage = 0,
        jumpParameter,
        appearEvent,
        postMessageEvent = "",
        ...other
    },
    disappearCallBack //回调函数
)
```

## 引用

```

WBCSTAPP.jumpNativePage(
    {
        pagePath: "jumppage/content_nativepage_crmPage",
        appearEvent: "viewData",
        postMessageEvent: "homeShareClick",
        isDestoryBeforePage: 0,
        jumpParameter: {
            condition: { synStatus: 1 },
            subTabIndex: 1
        }
    },
    () => {
        //回调函数 可以直接匿名函数 也可以定义好方法传进来
    }
);
```

参数

| 参数                | 类型               | 默认值 | 描述                                                         |
| ------------------- | ------------------ | ------ | ------------------------------------------------------------ |
| pagePath            | String             | 无     | 同之前Hybrid跳转方式一样,跳转到navtive 界面，iOS端需要做一层解析，下述表格列出了所有需要跳转的native页面对应的pagePath，以及跳转对应页面需要的参数传值 |
| jumpParameter       | Object/ Dictionary | 无     | 跳转页面需要携带的参数，下属表格列出了具体native页面需要的参数 |
| isDestoryBeforePage | number             | 0      | 上级页面是否需要关闭 0:不关闭 1：关闭                        |
| appearEvent         | String             | 无     | 在native端viewWillAppear的时候发送消息给Native端，RN 端需要通过data.viewAppear，来获取值是否和RN之前传的进行比较如果一致执行，不一致的不执行，viewAppear参数格式为：当前类名称+调起模块名称（rn 为bundleid）+ viewAppear<br />备注：从A页面跳转B页面，需要B页面携带数据给A页面可以设置appearEvent，在B页面即将离开时发消息给A页面，A页面设置监听事件：<br />eventManagerEmitter.addListener("viewAppear", (nativeData) =><br />//nativeData native端返回数据<br />console.log(JSON.stringify(nativeData))<br />);<br />注意：appearEvent监听事件固定写死“viewAppear”，在组件卸载时一定要移除监听事件<br />componentWillUnmount() {<br />eventManagerEmitter.removeAllListerers(this);<br />} |
| postMessageEvent    | String             | 无     | 与RN 端通讯时候发送事件的事件名字<br />与appearEvent用法一样，<br />区别是：postMessageEvent是实时传递消息的，不是即将离开页面时发消息，事件监听的就是填写的事件名称，<br />一般一个页面里面appearEvent和postMessageEvent是不会共存的 |
| disappearCallBack   | Function           | 无     | 回调方法<br />可以直接匿名函数 也可以定义好方法传进来        |

车商通APP跳转native页面：

| pagePath                                       | 跳转页面                            | jumpParameter                                                |
| :--------------------------------------------- | ----------------------------------- | ------------------------------------------------------------ |
| `pagePath`                                     | 跳转页面                            | jumpParameter                                                |
| `content_nativepage_cluePage`                  | 收车页面                            | `subTabIndex:1`                                              |
| `content_nativepage_cluePage`                  | 卖车页面                            | `subTabIndex:0`                                              |
| `content_nativepage_crmPage`                   | Crm客户列表页面                     | `subTabIndex: 1`                                             |
| `content_nativepage_crmPage`                   | Crm来电列表                         | `subTabIndex: 0`                                             |
| `content_nativepage_marketingPage`             | 营销管理页面                        |                                                              |
| `content_nativepage_stockManagerPage`          | 库存管理类别页面                    |                                                              |
| jumppage/<br />mkt_nativepage_priorityPushPage | 营销管理-优先推送                   | {   condition:{            refreshStatus:”1”,”2”//1全部，2已刷新priPushStatus:”1”,”2”//1全部，2已推送jZPushStatus: ”1”,”2”//1全部，2已精准zDPushStatus: ”1”,”2”//1全部，2已置顶}sign:1//商业操作标识 1：来电通 2：置顶 3：精准 4：优先推送 5：刷新6：精选 7：优先刷新cateId:29cateName:”二手车”title:”优先推送”infoIds:”31685385082934,31685385026934”//多条帖子id,以英文逗号分隔  }这写参数都要添加到jumpParameter里面 |
| jumppage/<br />mkt_nativepage_jZPushPage       | 营销管理-精选                       |                                                              |
| jumppage/<br />mkt_nativepage_zDPushPage       | 营销管理-置顶                       |                                                              |
| jumppage/<br />mkt_nativepage_pRPushPage       | 营销管理-优先刷新                   |                                                              |
| `jumppage/stockcarhome_nativepage`             | 跳转二手车库存管理首页              |                                                              |
| `jumppage/stockcarlist_nativepage`             | 跳转二手车库存管理列表页-带筛选条件 | {condition:{synStatus:1,2,3,4 //不限，未同步，同步成功，同步失败}} |
| `jumppage/allfunction_nativepage`              | 全部功能页面                        |                                                              |
| `jumppage/coupon_nativepage`                   | 优惠券首页面                        |                                                              |
| `jumppage/message_nativepage`                  | 未读消息界面                        |                                                              |
| `jumppage/preview_nativepage`                  | 图片预览界面                        | {    "data": [        "图片1地址",        "图片2地址",        "图片3地址"    ],    "position": 1   //需要展示的图片所处位置} |
| `jumppage/active_nativepage`                   | 活动中心                            |                                                              |
| `jumppage/visitor_nativepage`                  | 优选访客                            |                                                              |
| `jumppage/jqjs_nativepage`                     | 急求急售                            |                                                              |
| `jumppage/publish_nativepage`                  | 发布类型选择弹框                    |                                                              |
| `jumppage/mktselect_nativepage`                | 营销管理车辆类别大类页              | {           “sign”:1      //商业操作标识1：来电通2：置顶3：精准4：优先推送5：刷新 6：精选         “functionType”:1          "isDestoryBeforePage": 0,   } |
| `jumppage/addcustomer_nativepage`              | 添加客户页面                        |                                                              |
| `jumppage/homepage_nativepage`                 | cst首页                             |                                                              |

 
 

