# 轻提示toast

> iOS version >= 3.5.1

RN页面toast消息提示



## 协议引入

```
import WBCSTAPP from "@/rn-app";//引入对象名称可以自定义
```



## 协议设计

```
toast(
    {
        text = "",
        state,
        time = 2
    }
)
```





## 调用

```
WBCSTAPP.toast(
    {
        text: "发布成功",
        state: "prompt",
        time: 2
    }
);
```



## 参数

| 参数  | 类型   | 默认值 | 描述                      |
| ----- | ------ | ------ | ------------------------- |
| text  | String | `""`   | 提示文案                  |
| state | String | 无     | 提示等级error/warn/prompt |
| time  | float  | 2      | toast弹窗持续时间         |

