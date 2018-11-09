# RN资源构建

基本功能

build打包RN项目，然后在按需打成压缩包

#### 流程
>build -> 更新下载代码 -> 安装依赖包 -> 打包RN项目合成jsbundle -> 按需把资源打成tgz压缩包 -> 上传到服务器指定目录下

整个构建流程分成项目拆包流程和不拆包流程，下面分别介绍一下：

### 项目拆包流程
> 新建资源时勾选了项目拆包

点击build按钮如果项目拆包会执行以下流程
```
# !/bin/bash
# 功能：自动打包构建Android和iOS bundle包 并根据需求打包成压缩包
# 参数：$2 应用脚本是的传参 此处是 RN资源的项目名称 如：rn-common-bundle
echo $(pwd)
cd $(pwd)/gitlib/rnsource/$2
# 创建备份文件夹
if [ ! -d backup ];then
    mkdir backup
fi
# 备份package.json文件到backup
cp -f package.json ./backup
git pull origin master
# 不存在node_modules 一定是第一次构建 直接npm install安装依赖包
if [ ! -d node_modules ];then
    echo "初始未成功需要重新安装依赖包"
    npm install
else
    # 存在node_modules 此时需要对比package有改变则需安装依赖没有直接跳过
    if [ "`diff -w ./backup/package.json ./package.json`" ];then
        #echo `diff -w ./backup/package.json ./package.json`
        # 此时需要备份最新文件
        echo "有更新需要重新安装依赖包"
        cp -f package.json ./backup
        npm install
    fi
fi
git submodule foreach 'git pull origin master' 
# 创建打包所需的目录
echo "------>创建文件夹"
if [ ! -d dist ];then
    mkdir -p dist/ios
    mkdir -p dist/android
fi
# # 拆分打包jsbundle
npm run buildios
# npm run buildandroid
#common项目只打包common.bundle
# if判断条件后面必须添加;,[]方括号两边添加空格
if [ "$2" = "rn-common-bundle" ];then
    echo "----->开始压缩"
    # android和iOS拆分出来的common包是一样的
    cd dist/ios 
    tar -zcvf ./ios_common.tgz ./common.bundle
    cd ./../
    # tar -zcvf ./android_common.tgz ./common.bundle
else
    # 判断业务bundle是否含有静态资源 
    # 如果没有 直接压缩业务business bundle
    if [ -d $(pwd)/dist/ios/assets ];then
    cd dist/ios
    # 如果有静态资源把静态资源和业务bundle分别压缩
    tar -zcvf ./ios_assets.tgz ./assets
    tar -zcvf ./ios_business.tgz ./index.jsbundle
    cd ./../
    else
    cd dist/ios
    tar -zcvf ./ios_business.tgz ./index.jsbundle
    cd ./../
    fi
    
    # Android后期在处理
    # if [ -d $(pwd)/dist/android/assets ];then
    # tar -zcvf $(pwd)/dist/android/android_business.tgz dist/android/index.jsbundle dist/android/assets
    # else
    # tar -zcvf $(pwd)/dist/android/android_business.tgz dist/android/index.jsbundle
    # fi
fi

```

- 进入项目根目录备份package.json文件  
备份的目的是与项目更新后的最新文件对比，看是否有改动，选择性是否按照依赖包

- 更新当前项目代码  
git pull origin master

- 对比package.json文件  
如果项目使用了新的第三方依赖包，就需要平台再次npm install安装新的依赖包，如果没有更改直接跳过此步骤，大大节省build的时间消耗

- 创建打包目录  
首先判断使用有指定的打包目录，如果没有需要创建打包目录，在项目根目录下创建  
dist/ios  
dist/android

- 执行项目打包命令  
iOS打包命令如下：
>node node_modules/rocket-bundler/src/cli.js bundle   
--dev false   
--platform ios   
--entry-file index.js   
--bundle-output dist/ios/index.jsbundle   
--base-file base.js   
--base-output dist/ios/common.bundle   
--assets-dest dist/ios

此处拆包用的是rocket-bundler工具；  
<img src="http://c.58corp.com/download/attachments/26152743/image2018-9-6%2017:30:52.png?version=1&modificationDate=1536226252641&api=v2" width="300"  />

原理：
>bundle 文件中每一行都是一个Module.js对应的数据结构；打包过程中，在分析依赖时，引入base.js先进行基础依赖遍历，并对Module元素标记base: true，然后再对入口文件进行依赖分析，这种先后顺序能保证基础模块 id 在前，业务模块 id 在后；在打包输出时，将标记base: true的Module打包到common.bundle，否则打包到business bundle


输出成果：  
1. 抽离react和react-native打包成common.bundle
2. 减小线上下发业务bundle体积，节省用户流量
3. 预加载common.bundle，提升打开页面速度

base.js如下：

```
import React, { Component } from "react";
import {} from "react-native";
```

- 打压缩包  
目的：  
由于RN资源build之后会把jsbundle和静态资源分离，而静态资源又是一个文件夹，需要平台把这些资源打成一个压缩包，以便上传。  
打包原则：  
对于common项目只打包common.bundle  
```
tar -zcvf ./ios_common.tgz ./common.bundle
```
对于普通的业务项目把业务bundle和静态资源各种大压缩包，在平台上独立管理静态资源和业务bundle的资源版本号
```
tar -zcvf ./ios_assets.tgz ./assets
tar -zcvf ./ios_business.tgz ./index.jsbundle
```

以上就是拆包项目资源构建的整个流程，构建完成该项目本地打包目录如下：  

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/rnImg/mulu.jpg" width="300">


### 项目不拆包流程
>新建项目资源是未勾选拆包

RN项目整包编译会走一下流程
```
cd $(pwd)/gitlib/rnsource/$2
# 创建备份文件夹
if [ ! -d backup ];then
    mkdir backup
fi
# 备份package.json文件到backup
cp -f package.json ./backup
git pull origin master
# 不存在node_modules 一定是第一次构建 直接npm install安装依赖包
if [ ! -d node_modules ];then
    npm install
else
    # 存在node_modules 此时需要对比package有改变则需安装依赖没有直接跳过
    if [ "`diff -w ./backup/package.json ./package.json`" ];then
        #echo `diff -w ./backup/package.json ./package.json`
        # 此时需要备份最新文件
        cp -f package.json ./backup
        npm install
    fi
fi
# git pull origin master
# npm install
git submodule foreach 'git pull origin master' 
npm run build:android
npm run build:ios
tar -zcvf $(pwd)/ios_index.tgz -C $(pwd)/ios/bundle/ .
tar -zcvf $(pwd)/android_index.tgz -C $(pwd)/android/bundle/ .
```

流程大体上与拆包项目一致；构建完成的的目录结构如下：

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/rnImg/mulu1.jpg" width="300">
<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/rnImg/mulu2.jpg" width="300">