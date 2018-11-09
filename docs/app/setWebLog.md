# 埋点协议

> iOS version >= 3.5.1

RN页面统计埋点



## 协议引入

```
import WBCSTAPP from "@/rn-app";//引入对象名称可以自定义
```



## 协议设计

```
setWebLog(
    {
        eventId,
        attributes
    }

```



## 调用

```
WBCSTAPP.setWebLog(
    {
        eventId: "eventId",
        attributes: {
            key:value
        }
    }
);
```



## 参数

| 参数       | 类型                | 默认值 | 描述         |
| ---------- | ------------------- | ------ | ------------ |
| eventId    | String              | 无     | 埋点的key    |
| attributes | Object/NSDictionary | 无     | 埋点的上传值 |
