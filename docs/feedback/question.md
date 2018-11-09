### React Native 开发问题记录

### 模拟器

- 设备反向代理，Android系统版本问题。下面报错adb reverse 无法运行，更换Android版本，低于5.0的设备不支持adb reverse。 ![image](https://note.youdao.com/yws/public/resource/d14bf14d5e3193a49878c8b072b95e3a/xmlnote/3B8DE68237CC419AAB28B1D414F17B3C/1797?ynotemdtimestamp=1539846973446)

  **番外——adb(Android debug bridge)**

  Android允许我们通过ADB，把Android上的某个端口映射到电脑（adb forward），或者把电脑的某个端口映射到Android系统（adb reverse）。假设电脑上开启的服务，监听的端口为8000。Android手机通过USB连接电脑后，执行 adb reversetcp:8000 tcp:8000，然后在手机中访问127.0.0.1:8000，就可以访问到电脑上启动的服务了。

### IOS与Android差异

**标题栏样式**

1. 阴影差异

- ios

  默认样式

  ![image](https://note.youdao.com/yws/public/resource/d14bf14d5e3193a49878c8b072b95e3a/xmlnote/D73F92211B3443AB8205AC589ADD9628/1784?ynotemdtimestamp=1539846973446)

- android

  默认样式

  ![image](https://note.youdao.com/yws/public/resource/d14bf14d5e3193a49878c8b072b95e3a/xmlnote/E7B970A8339E44578B02ABF7839E24E7/1697?ynotemdtimestamp=1539846973446)

  去阴影

  ![image](https://note.youdao.com/yws/public/resource/d14bf14d5e3193a49878c8b072b95e3a/xmlnote/E9FC422269BB4A12A0AEF656416801B0/1714?ynotemdtimestamp=1539846973446)

  ```
    headerStyle:{
              elevation: 0,//android端去阴影
      }
  ```

  去阴影加下边框

  ![image](https://note.youdao.com/yws/public/resource/d14bf14d5e3193a49878c8b072b95e3a/xmlnote/D28D83D8855047CD9224D682E3E5370F/1716?ynotemdtimestamp=1539846973446)

```
     headerStyle:{
            elevation: 0,//android端去阴影
            borderBottomWidth:1,
            borderBottomColor: "#f5f5f5",
}
```

1. 标题位置差异

- ios

  默认居中显示

  ![image](https://note.youdao.com/yws/public/resource/d14bf14d5e3193a49878c8b072b95e3a/xmlnote/D73F92211B3443AB8205AC589ADD9628/1784?ynotemdtimestamp=1539846973446)

- android

  默认左对齐显示

  ![image](https://note.youdao.com/yws/public/resource/d14bf14d5e3193a49878c8b072b95e3a/xmlnote/D28D83D8855047CD9224D682E3E5370F/1716?ynotemdtimestamp=1539846973446)

  调整居中

  ![image](https://note.youdao.com/yws/public/resource/d14bf14d5e3193a49878c8b072b95e3a/xmlnote/400673F03E2F402EB8694AEE1AC15E29/1850?ynotemdtimestamp=1539846973446)

```
    headerTitleStyle: {
        //标题居中显示
        flex:1,
        textAlign:"center"
    },
    /**如果标题栏左侧还有返回按钮，发现标题偏右依然不居中，则简单的处理方式是:
    *在右边再添加一个等宽高的空 View
    */
    headerRight:(
        <View style={{width:100}}/>
    ),
```

1. TextInput 标签样式差异

android特有属性underlineColorAndroid，用来定义输入提示下划线的颜色。如果将它的颜色设为与 TextInput 组件的背景色一样或者设置为透明，即可以隐藏输入提示下划线。另外android下，需将paddingVertical设置为0解决TextInput文字不垂直居中的问题。

默认样式

![image](https://note.youdao.com/yws/public/resource/d14bf14d5e3193a49878c8b072b95e3a/xmlnote/A58AB016CED34AEAB1F2992E705421D6/1868?ynotemdtimestamp=1539846973446)