# RN资源同步

在资源构建完成之后，就需要进行提测上线的环节了；整个流程分三部分：

- 测试资源同步
- 沙箱资源同步
- 线上资源同步

### 测试资源同步
在资源构建完成之后会在提测列表中生成一条记录，在点击提测是平台会把资源上传到服务器对应目录下面，并把版本号和静态资源版本号➕10。
<img src="http://c.58corp.com/download/attachments/26152743/image2018-9-10%2014%3A14%3A23.png?version=1&modificationDate=1536560063315&api=v2">

dev环境上传路径拼接规则：

- 不拆包整包上传路径
>//j1.58cdn.com.cn/escstatic/rn/apptest/bundleId(如：10010)/platform(ios/android)/bundleId_version_platform_index.tgz

- 公共资源包common上传路径
>//j1.58cdn.com.cn/escstatic/rn/apptest/bundleId(如：10010)/platform(ios/android)/bundleId_version_platform_common.tgz

- 拆包业务bundle上传路径
>//j1.58cdn.com.cn/escstatic/rn/apptest/bundleId(如：10010)/platform(ios/android)/bundleId_version_platform_business.tgz

- 拆包业务静态资源包上传路径
>//j1.58cdn.com.cn/escstatic/rn/apptest/bundleId(如：10010)/platform(ios/android)/bundleId_version_platform_assets.tgz


后期规划： 
``` 
1、目前项目每提测一次，该项目对应的版本号和静态资源版本号都会对应加10，
一旦版本号变更，客户端都会重新下载资源包；


2、对应拆包项目，车商通APP会内置一个common.bundle，
平台上也不会允许修改common项目，common的版本号就不会变更，APP启动时就不会下载；

3、对于拆包项目的静态资源包
就是打包时RN未能识别的非js后缀的文件，一般大部分都是本地使用的图片，
对于正常业务来说一般很少修改图片，但是每次提测是也都会把静态资源的版本号加10，这样APP也会每次都重新下载，
这样对于流量和时间都是多余的消耗，平台后期会在提测之前做一个diff对比，如果前端未修改静态资源，则不变更版本号。
```

### 沙箱资源同步
在提测完成之后会在沙箱列表生成一条记录，点击提测沙箱可把资源上传到沙箱环境。

上传路径和dev环境一样就是把链接中的apptest改成sandbox

### 线上资源同步
线上资源同步步骤和前面的一致，多了一步保存当前上线版本记录的操作，在项目回滚会详细讲解。
