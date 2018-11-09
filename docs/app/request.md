# 网络请求

RN页面网络请求封装

纯前端封装 不涉及和native交互



## 方法引入

```
import WBCST from "@/rn-app";//引入对象名称可以自定义
```



## 接口

```
getFetch(url,params) //GET请求
postFetch(url,params) //POST请求
```



## 调用

```
//GET请求
WBCST.getFetch("https://cheshangtongapi.58.com/task/getList", {
    taskType: 1
}).then((response) => {
    if (response && response.respCode == 0 && response.respData.length > 0) {
        this.handleTaskListData(response.respData);
    } else {
        this.handleTaskListData();
        WBCST.toast({text:"当前网络不可用，请检查网络设置！",state:"error"});
    }
});
//POST请求
说明：
参数直接以json传入，不用在声明new FormData()，接口封装是都统一处理了
WBCST.postFetch("https://cheshangtongapi.58.com/task/getIntegral", {
    taskId: taskId
}).then((response) => {
    if (response && response.respCode == 0) {
        WBCST.toast({ text: "领取成功", state: "prompt" });
        this.getTaskList();
    } else {
        WBCST.toast({ text: "领取失败", state: "error" });
    }
});
```



## 参数

| 参数   | 类型   | 默认值 | 描述              |
| ------ | ------ | ------ | ----------------- |
| url    | String | 无     | 接口请求的URL     |
| params | Object | 无     | GET请求需要的参数 |

