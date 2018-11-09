# RN资源管理

### 基本功能
- 列表显示所有RN资源
- 按ID搜索资源
- 添加资源
- 修改资源

### RN资源列表
> 列表展示平台上添加的所有项目资源，可根据项目id来搜索资源

### 新增资源

![image](http://c.58corp.com/download/attachments/26152743/image2018-9-12%2016%3A7%3A4.png?version=1&modificationDate=1536739624311&api=v2)

#### 资源添加说明
- 项目名称  
项目名称必填 一般也都是唯一的  
项目名称填写时最好保持与gitlab上创建的分支名字一样，以方便后期搜索查找

- 项目负责人  
最好填写OA名，方便联系

- git地址  
git地址 必填  
要创建的RN项目在gitlab仓库中的下载地址 如：git@igit.58corp.com:cst-rn/rn-speed-project.git  
在项目创建初始化时会根据用户所填写的git地址下载项目代码到本地，在进行后续的操作

- 降级H5链接  
设计的目的如果创建的RN项目是改版之前的H5项目，说明该功能存在H5模板，如果在RN加载异常的情况下，
又填写了之前的H5模板链接，这时可以回滚到之前的H5模板；
如果此功能就只有RN版本，降级H5链接可以填写一个错误页面的模板链接，以便RN加载错误时弹出H5 的错误页面，
当然也可以不填写，就看业务方的开发需求

- 项目描述  
简单的填写一下当前项目的简介

- 启动预加载  
native端每次下载完成该项目资源包时是否强制更新，默认false  
如果当前项目有更新，并且native端也下载了最新的资源包，如果不启用预加载，native会先加载老的资源包，等下载在启动该功能时在加载最新资源包。

- 是否拆包  
目前车商通APP的RN项目有两种打包方式：  
一种是使用RN默认的打包方式，直接build把所有资源全部打包到一个jsbundle里面；  
一种是拆包方式，会把RN包拆分成common和business两个jsbundle，对于车商通APP来说，所有的RN项目都共用一个common；  
在下一章节资源构建里面会详细讲解拆包和不拆包的区别；

项目创建完成会在列表生成一条记录；  
平台也会在后台执行init.sh shell脚本下载代码：
```
cd $(pwd)/gitlib/rnsource
echo "下载项目代码"
git clone --depth 1 $1 $2
echo "代码下载完成"
cd $2
echo "下载更新submodule子模块"
git submodule init
git submodule update
echo "子模块下载更新完成"
echo "npm依赖包安装中..."
npm install;
echo "npm依赖包安装完成"
```

总体上就是在指定目录下下载项目代码，并下所有子模块，完成之后再按照项目依赖包；  
此过程因为要按照依赖包所有整个过程时间比较长； 

> 后续会在前端页面直接显示后台执行的日志，能试试观察当前执行状态

- 开发人员
添加项目权限，只有项目中添加的人员才有权限修改，同步此项目，默认选中当前系统登录者

### 资源修改
<img src="http://c.58corp.com/download/attachments/26152759/image2018-9-6%2016%3A13%3A42.png?version=1&modificationDate=1536221622115&api=v2" width="500">
<!-- ![image](http://c.58corp.com/download/attachments/26152759/image2018-9-6%2016%3A13%3A42.png?version=1&modificationDate=1536221622115&api=v2) -->

#### 资源修改说明
- 项目名称  
项目名称是数据库检索的关键字，一旦项目创建就不允许修改了；

- git地址  
所填写的git地址是clone代码的唯一途径，项目创建完成之后，代码也下载到服务器指定目录；如果随意修改git地址，就改变了该项目的实际意义，所有git地址也是不允许修改的；  
如果该项目在git远程仓库中迁移地址了，请联系管理员gongchenghui@58ganji.com来修改对应的项目地址；

### 公共资源包管理
> 公共资源包也就是RN走拆包流程时的common.jsbundle的一个项目管理

这也是一个独立的RN项目，只用来管理车商通APP里面的common.jsbundle；一般创建之后就很少会修改，common里面就只是打包了react和react-native两个资源；车商通APP第一次下载完成该包时，就会内置到APP里面，后续的拆包的RN项目就不再下载该资源了；

该项目只有有管理员权限的才能修改；