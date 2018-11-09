# 对话框dialog

> iOS version >= 3.5.1

RN页面对话框



## 协议引入

```
import WBCSTAPP from "@/rn-app";//引入对象名称可以自定义
```



## 协议设计

```
dialog(
    {
        title = "提示",
        content,
        btns = [{ color: "#000000", text: "确定" }, { color: "#000000", text: "取消" }],
        ...other
    },
    callback
)
```



## 调用

```
WBCSTAPP.dialog(
    {
        title: "温馨提示",
        content: "请上传证件照，否则发帖需要扣费10元",
        btns: [{ color: "#ff552e", text: "确定" }, { color: "#000000", text: "取消" }]
    },
    (error,nativeData)=>{
        //弹窗回调nativeData=index 返回按钮位置
    }
);
```



## 参数

| 参数     | 类型     | 默认值                                                       | 描述                                                         |
| -------- | -------- | ------------------------------------------------------------ | ------------------------------------------------------------ |
| title    | String   | `提示`                                                       | 弹窗标题                                                     |
| content  | String   | 无                                                           | 弹窗提示文案                                                 |
| btns     | Array    | [{ color: "#000000", text: "确定" }, { color: "#000000", text: "取消" }] | Dialog的buttons数据                                          |
| callback | function | 无                                                           | 回调函数Rn传给Native回调：native 端执行回调callBack(error, index));//index 返回按钮点击的位置，第几个按钮点击 (整型) |
