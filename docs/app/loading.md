# 页面loading

> iOS version >= 3.5.1

RN页面loading



## 协议引入

```
import WBCSTAPP from "@/rn-app";//引入对象名称可以自定义
```





## 协议设计

```
loading(
    {
        text = "加载中",
        state
    }
)
```





## 调用

```
WBCSTAPP.loading(
    {
        text: "加载中",
        state: "show"
    }
);
```





## 参数

| 参数  | 类型   | 默认值 | 描述                                 |
| ----- | ------ | ------ | ------------------------------------ |
| text  | String | 加载中 | loading文案                          |
| state | String | 无     | show展示loading dismiss：关闭loading |
