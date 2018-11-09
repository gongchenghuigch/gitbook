# 扫码真机预览

## 一、设计流程

#### 1、入口

![preview](http://c.58corp.com/download/attachments/26953326/image2018-10-18%2011%3A36%3A49.png?version=1&modificationDate=1539833809000&api=v2)

> 入口展示原则：
>
> 只是进行了一次项目提测；也就是至少测试环境版本号不为空才展示查看入口



#### 2、二维码预览页面

![preview](http://c.58corp.com/download/attachments/26953326/image2018-10-18%2011%3A38%3A3.png?version=1&modificationDate=1539833883000&api=v2)

![preview](http://c.58corp.com/download/attachments/26953326/image2018-10-18%2011%3A39%3A39.png?version=1&modificationDate=1539833979000&api=v2)

![preview](http://c.58corp.com/download/attachments/26953326/image2018-10-18%2011%3A40%3A19.png?version=1&modificationDate=1539834019000&api=v2)

![preview](http://c.58corp.com/download/attachments/26953326/image2018-10-18%2011%3A41%3A7.png?version=1&modificationDate=1539834067000&api=v2)



#### 3、流程图

![preview](http://c.58corp.com/download/attachments/26953326/preview.png?version=1&modificationDate=1539834147000&api=v2)



#### 4、功能简介

- 点击查看入口请求当前项目所有历史版本，根据需求分组成测试环境和线上环境列表
- 展示项目预览弹窗
- 默认显示测试包的最近一次的版本
- 二维码生成链接H5落地页和当前项目选择的参数
- 切换环境时项目版本列表对应换成当前环境的版本列表，并且项目版本默认选中当前环境下的最新一次的版本号
- 如果项目还未上线预览环境只显示测试包，版本号也只有测试环境的所有历史版本
- 切换预览环境和项目版本是，重新拼接URL，二维码重新绘制
- 使用微信扫一扫直接跳转H5落地页
- 落地页处理逻辑：未安装车商通APP跳转应用商店引导下载安装，已安装直接调起APP
- 客户端接收传入的参数拼接当前要预览项目的下载地址，直接下载当前项目，并新建一个RN容器打开当前项目



## 二、参数传入

| 参数       | 说明                   |
| ---------- | ---------------------- |
| `bundleId` | 项目ID                 |
| version    | 预览项目版本           |
| srcVersion | 预览项目静态资源版本   |
| unpacking  | 当前项目是整包还是分包 |
| condition  | 当前环境               |
|            |                        |



## 三、客户端下载地址拼接规则

#### 1、整包unpacking false

项目下载地址：

```
//platfrom iOS端：ios  Android端：android
"https://j1.58cdn.com.cn/escstatic/rn/"+condition+"/"+bundleId+"/"+platfrom+"/"+bundleId_version_platfrom_index.tgz
```



#### 2、分包unpacking true

项目下载地址：

```
//platfrom iOS端：ios  Android端：android
//业务包下载地址
"https://j1.58cdn.com.cn/escstatic/rn/"+condition+"/"+bundleId+"/"+platfrom+"/"+bundleId_version_platfrom_business.tgz
//静态资源包下载地址
"https://j1.58cdn.com.cn/escstatic/rn/"+condition+"/"+bundleId+"/"+platfrom+"/"+bundleId_srcVersion_platfrom_assets.tgz
```


