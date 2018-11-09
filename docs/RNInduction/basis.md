## 一、RN优劣势
React Native的设计理念： 既拥有Native的用户体验、又保留React的开发效率

#### 优势：

它对比原生开发更为灵活，对比H5体验更为高效。

替代传统的WebView，打开效率更高，和原生之间的交互更方便。

多个版本迭代后的今天，它已经拥有了丰富第三方插件支持。

更方便的热更新。

#### 劣势：

尽管是跨平台，但是不同平台Api的特性与显示并不一定一致。

调试’相对‘麻烦。

Android上的兼容性问题。

#### 风险：

尽管Facebook有3款App(Groups、Ads Manager、F8)使用了React Native，随着React Native大规模应用，Appstore的政策是否有变不得而知

## 二、搭建开发环境

#### Node环境

React Native需要NodeJS 4.0或更高版本

#### React Native的命令行工具（react-native-cli）

React Native的命令行工具用于执行创建、初始化、更新项目、运行打包服务（packager）等任务。

> npm install -g react-native-cli

#### 新建项目
> react-native init MyAppTest<br>
> cd MyAppTest<br>
> react-native run-ios<br>

如果使用Mac开发，直接下载Xcode，react-naitve run-ios时会自动遍历你电脑上是否安装Xcode，如果安装直接启动Xcode的模拟器，window上需要安装Android模拟器才能运行


[开发环境搭建详见官网](https://reactnative.cn/docs/0.36/getting-started.html/)


## 三、常用组件
 [详细总结](https://blog.csdn.net/gongch0604/article/details/79972777)
<font color="red">View组件<br></font>
View是一个支持Flexbox布局、样式、一些触摸处理、和一些无障碍功能的容器，并且它可以放到其它的视图里，也可以有任意多个任意类型的子视图。
View的设计初衷是和StyleSheet搭配使用，这样可以使代码更清晰并且获得更高的性能。尽管内联样式也同样可以使用<br>
```
<View style={styles.container}>  
    <View style={[styles.flex, styles.center]}>  
        <Text style={styles.white}>酒店</Text>  
    </View>  
</View> 
```
View就相当于一个容器，能设置背景、边框等样式<br>
View组件里面是不能直接写文字的:如<br>
```
<View>当前可用积分</View>
```
在RN的低版本里面会直接抛异常，在高版本里面虽说能正常运行，但是也不会生效<br>
[详细请参考官网](https://reactnative.cn/docs/0.51/view.html#content)<br>
注意：<br>
如果要使用onPress点击事件，请直接使用Touchable系列组件替代View，然后添加onPress函数，View自身是没有onPress属性的


<font color="red">Text组件<br></font>
一个用于显示文本的React组件，并且它也支持嵌套、样式，以及触摸处理 如：<br>  
```
<View style={styles.container}>  
    <Text style={styles.font}>  
        <Text style={styles.red}>网易</Text>  
        <Text style={styles.white}>新闻</Text>  
        <Text>有态度</Text>  
    </Text>  
</View>
```
常用特性 [详细请参考官网](https://reactnative.cn/docs/0.51/text.html#content)
```
onPress：手指触摸事件 点击事件  
numberOfLines ：显示多少行 
```
常用样式设置
```
color：字体颜色  
fontSize：字体大小  
fontWeight：字体加粗enum('normal', 'bold', '100', '200', '300', '400', '500', '600', '700', '800', '900')  
指定字体的粗细。大多数字体都支持'normal'和'bold'值。并非所有字体都支持所有的数字值。如果     某个值不支持，则会自动选择最接近的值。  
textAlign：对齐方式  
lineHeight number  
```


<font color="red">Touchable类组件<br></font>
该组件用于封装视图，使其可以正确响应触摸操作<br>
常用设置
```
TouchableOpacity：透明触摸，点击时，组件会出现透明过度效果。  
TouchableHighlight：高亮触摸，点击时组件会出现高亮效果。只能进行一层嵌套，不能多层嵌套  
TouchableWithoutFeedback：无反馈触摸，点击时候，组件无视觉变化
```
TouchableOpacity示例：
```
<TouchableOpacity  
    activeOpacity={0.8}  
    onPress={this.onButtonClick}  
    style={styles.signBtn}>  
    <Text style={styles.signBtnText}>立即签到</Text>  
</TouchableOpacity>  
```
activeOpacity指定封装的视图在被触摸操作激活时以多少不透明度显示（通常在0到1之间）

<font color="red">TextInput组件<br></font>
TextInput是一个允许用户在应用中通过键盘输入文本的基本组件。本组件的属性提供了多种特性的配置，譬如自动完成、自动大小写、占位文字，以及多种不同的键盘类型（如纯数字键盘）等等<br>
通用属性设置(属性很多就不一一列举，详细可查看官网)
```
（1）autoCapitalize：首字母自动大写。可选值有：none、sentences、words、characters。  
  ● none：不自动变为大写  
  ● sentences：将每句话的首字母自动改成大写  
  ● words：将每个单词的首字母自动改成大写  
  ● characters：将每个英文字母自动改为大写 
  
（2）placeholder：占位符，在输入前显示的文本内容。 
（3）value：用来设置 TextInput 组件内字符串的值。
（4）multiline：如果为 true，表示多行输入
（5）editable：默认为 true。如果设置为 false 表示不可编辑。  
（6）maxLength：能够输入的最长字符数
（7）defaultValue：用来定义 TextInput 组件中的字符串默认值
```
组件的方法属性
```
（1）onChange：当文本发生变化时，调用该函数
（2）onChangeText：当文本发生变化时，调用该函数。
    onChangeText 回调函数与上面的 onChange 类似，但它的好处是直接可以接收用户输入的字符串。
（3）onEndEditing：当结束编辑时，调用该函数。
（4）onBlur：失去焦点时触发（在 onEndEditing 之后）
（5）onFocus：获得焦点时触发
```
TextInput用法示例
```
<View style={styles.textareaContent}>  
    <Text style={styles.tcTitle}>反馈内容：</Text>  
    <TextInput  
    multiline={true}//多行输入  
    style={styles.tcInput}  
    autoCapitalize="none"//不自动切花任何大小写iOS上默认会首字母大写，需要手动设置一下  
    placeholder="请填写您的宝贵意见，让58不断进步，谢谢！"  
    placeholderTextColor="#CCC"  
    onChangeText={(textarea) => this.setState({ textarea })}  
    />  
</View>  
```
[关于软键盘Keyboard的讲解](https://blog.csdn.net/gongch0604/article/details/79989692)


<font color="red">Image组件<br></font>
[Image组件的详细总结](https://blog.csdn.net/gongch0604/article/details/79961244)   
1、组件的基本用法   

1.1 从当前项目中加载图片

要往App中添加一个静态图片，只需把图片文件放在代码文件夹中某处，然后像下面这样去引用它：  
<Image source={require('./my-icon.png')} /> 
```
{/*从项目中加载图片*/} 
<TouchableOpacity
      style={styles.redeem}
      onPress={() => navigate("Redeem", { title: "积分兑换" })}>
      <Image  source={require("./../../img/task/jifen-icon.png")} style={styles.imageStyle} />
       <Text style={styles.redeemText}>积分兑换</Text>
</TouchableOpacity>
```
不支持变量拼接的写法 如：  
```
// 错误  
let icon = this.props.active ? 'my-icon-active' : 'my-icon-inactive';  
<Image source={require('./' + icon + '.png')} />  
  
// 正确  
let icon = this.props.active ? require('./my-icon-active.png') : require('./my-icon-inactive.png');  
<Image source={icon} /> 
```
1.2 加载使用APP中的图片  
注意：这一做法并没有任何安全检查。你需要自己确保图片在应用中确实存在，而且还需要指定尺寸。
```
{/*从资源包中加载图片*/}  
 <Text>2.从APP中加载图片</Text>  
 <Image source={{uri:'icon_a'}} style={styles.imageStyle} /> 
```
1.3 加载来自网络的图片

客户端的很多图片资源基本上都是实时通过网络进行获取的，这边一定需要指定图片的尺寸大小，实现如下
```
{/*从网络中加载图片*/}  
 <Text>3.从网络中加载图片</Text>  
 <Image source={{uri: 'https://upload.wikimedia.org/wikipedia/commons/d/de/Bananavarieties.jpg'}}  
        style={styles.imageStyle} /> 
```
1.4 设置图片为背景  
直接在Image组件里面嵌套写的组件 实现如下
```
<Image
    style={styles.newTaskHeader}
    source={require("./../../img/task/xsrw-bg.png")}>  
    <Text style={styles.ntTitle}>积分兑换优惠券啦～</Text>
    <Text style={styles.ntSubTitle}>完成任务获取积分，多重优惠券好礼不停送！</Text>  
</Image>
```
在rn版本0.46版本的时候添加了ImageBackground控件，在0.46版本以后使用Image的时候不能在嵌套使用，ImageBackground就是解决这个问题的。ImageBackground的使用和Image一样
```
<ImageBackground
    style={styles.newTaskHeader}
    source={require("./../../img/task/xsrw-bg.png")}>  
    <Text style={styles.ntTitle}>积分兑换优惠券啦～</Text>
    <Text style={styles.ntSubTitle}>完成任务获取积分，多重优惠券好礼不停送！</Text>  
</ImageBackground>
```
开发时遇到的问题：  
在iOS上Image里面嵌套的Text组件总是会出现白色背景 如下图  
![image](https://note.youdao.com/yws/public/resource/15552c4941240eaf77eaacdc3af00b84/xmlnote/EC352037F20B4C73B1E931911496E5EA/4579)  
解决方法：  
给Image添加透明背景backgroundColor: "transparent" 解决完显示如下  
![image](https://note.youdao.com/yws/public/resource/15552c4941240eaf77eaacdc3af00b84/xmlnote/F7CB140CAB2E45D09A9F82887D5D5775/4587)

2、组件的常见属性  

2.1 常用样式
```
  FlexBox         支持弹性盒子风格
  Transforms      支持属性动画
  backgroundColor 背景颜色
  borderColor     边框颜色
  borderWidth     边框宽度
  borderRadius    边框圆角
  overflow 设置图片尺寸超过容器可以设置显示或者隐藏('visible','hidden')

  tintColor       颜色设置

  opacity 设置不透明度0.0(透明)-1.0(完全不透明)
```
2.2 常用属性方法
```
resizeMode

   缩放比例,可选参数('cover', 'contain', 'stretch') 该当图片的尺寸超过布局的尺寸的时候，会根据设置Mode进行缩放或者裁剪图片
   'cover':图片居中显示，没有被拉伸，超出部分被截断
   'contain':容器完全容纳图片，图片等比例进拉伸
   'stretch':图片被拉伸适应容器大小，有可能会发生变形
```
[详细讲解](https://blog.csdn.net/gongch0604/article/details/79961244)    [官网](https://reactnative.cn/docs/0.51/image.html#content)


<font color="red">ScrollView组件<br></font>
能够调用移动平台的ScrollView（滚动视图）的组件 当内容显示不全时可以通过滚动显示  
常用属性
```
(1) View props… :继承View的所有属性 
(2) horizontal(bool):表示ScrollView是横向滑动还是纵向滑动.默认false表示纵向滑动
(3) scrollTo: (y: number | { x?: number, y?: number, animated?: boolean }, x: number, animated: boolean) 
滚动到指定的x, y偏移处。第三个参数为是否启用平滑滚动动画。
(4) scrollToEnd: 滚动到视图底部（水平方向的视图则滚动到最右边）scrollToEnd({animated: true})则启用平滑滚动动画
```
[详见官网](https://reactnative.cn/docs/0.22/scrollview.html#content)


<font color="red">ListView组件<br></font>
一个核心组件，用于高效地显示一个可以垂直滚动的变化的数据列表  
在RN 0.43版本以后，新出了一个FlatList的长列表组件，在数据量比较大的情况下，性能要比List View好很多，并且在高版本的RN中ListView已经逐渐废弃，不再维护，这里就不在讲解了    
[详见官网](https://reactnative.cn/docs/0.36/listview.html#content)

<font color="red">FlatList组件<br></font>
常用属性
```
ItemSeparatorComponent：分割线组件， 
ListFooterComponent：结尾组件 
ListHeaderComponent：头组件 
data：列表数据 data属性目前只支持普通数组
horizontal：设置为true则变为水平列表（默认false）。 
numColumns：列数 组件内元素必须是等高的,当horizontal为true时不支持此参数 
renderItem：渲染每个组件 
keyExtractor：给定的item生成一个不重复的Key
ListEmptyComponent：数据为空时的布局
initialNumToRender：指定一开始渲染的元素数量，最好刚刚够填满一个屏幕，这样保证了用最短的时间给用户呈现可见的内容。注意这第一批次渲染的元素不会在滑动过程中被卸载，这样是为了保证用户执行返回顶部的操作时，不需要重新渲染首批元素
```
用法示例
```
<FlatList
    data={this.state.taskList}
    //使用 ref 可以获取到相应的组件
    ref={(flatList) => (this._flatList = flatList)}  
    //header头部组件
    ListHeaderComponent={this._header} 
    //空数据视图,可以是ReactComponent,也可以是一个render函数，或者渲染好的element。
    keyExtractor={(item, index) => "index" + index + item}
    ListEmptyComponent={this._renderEmptyView}
    renderItem={this._renderItem}
/>

 _renderItem = ({ item, index }) => {
        return (
            <View style={styles.list} key={index}>
                <View style={styles.content}>
                    <Text style={styles.contentTitle}>{item.title}</Text>
                    <Text style={styles.contentTip}>
                        任务奖励<Text style={{ color: "#FF552E" }}>+{item.integral}积分</Text>
                    </Text>
                </View>
                <View style={styles.listBtn}>
                    <Text style={styles.text13FFF}>去完成</Text>
                </View>
            </View>
        );
    };
```


<font color="red">SectionList组件<br></font>
也是0.43版本推出的,高性能的分组列表组件
常用属性
```
ItemSeparatorComponent：分割线组件， 
ListFooterComponent：结尾组件 
ListHeaderComponent：头组件 
sections：数据源 
数据格式[ // 不同section渲染相同类型的子组件
    {data: [...], key: ...},
    {data: [...], key: ...},
    {data: [...], key: ...},
  ]
renderItem：渲染每个组件 
keyExtractor：给定的item生成一个不重复的Key
ListEmptyComponent：数据为空时的布局
initialNumToRender：指定一开始渲染的元素数量，最好刚刚够填满一个屏幕，这样保证了用最短的时间给用户呈现可见的内容。注意这第一批次渲染的元素不会在滑动过程中被卸载，这样是为了保证用户执行返回顶部的操作时，不需要重新渲染首批元素
renderSectionHeader：每个section的组头
```
用法示例
```
<SectionList
    sections={section}
    //区域header头部组件
    renderSectionHeader={this._sectionHeader}
    keyExtractor={(item, index) => "index" + index + item}
    renderItem={this._renderItem}
/>
//其中renderSectionHeader中传入的数据格式和renderItem是有区别的
_sectionHeader = (info) => {
        //传入参数info的示例
    {
	"section": {
		"title": "general",
		"data": [ {
			"integral": 50,
			"status": 1,
			"taskId": "2",
			"title": "查看未读消息 ",
			"taskTag": 1
		}, {
			"integral": 50,
			"status": 1,
			"taskId": "1",
			"title": "查看未接电话",
			"taskTag": 1
		}]
	}
}
```
## 四、style样式及Flex布局
React-Native 编写的应用的样式不是靠css来实现的，而是依赖javascript来为你的应用来添加样式

1、声明样式  
导入必要文件
>import React, { StyleSheet } from "react-native";  

然后调用React-Native的一个构造方法，传入一个对象生成style，和React的React.createCladd()语法是一样的，传入对象的key就相当于类名，每个类也是一个对象，可以配置各种样式参数，总体来说和CSS的写法差不多，差别上把CSS的命名又“-”连字符改成驼峰写法，然后长度不用加单位“px”，字符串比如色值需要加引号写成字符串。
```
const styles = StyleSheet.create({  
    base: {  
        width: 38,  
        height: 38,  
    },  
    background: {  
        backgroundColor: '#222222',  
    },  
    active: {  
        borderWidth: 2,  
        borderColor: ‘#ff00ff',  
    },  
});  
```

2、样式使用  
所有的核心组件都支持样式属性  
```
<View style={styles.base}></View>  
```
当你需要设置多个属性类的时候，可以传入一个数组
```
<View style={[styles.base,styles.backgroundColor]}></View>  
```
也可以在组件中render样式，然而这种做法不推荐，其实就像一般html页面中行内样式不推荐一样
```
<View  
  style={{width:this.state.width, height:this.state.width*this.state.aspectRatio}}  
/> 
<View  
  style={[styles.base,{width:this.state.width, height:this.state.width*this.state.aspectRatio}]}  
/> 
```

3、flexBox布局  

3.1 什么是FlexBox布局?  
弹性盒模型（The Flexible Box Module）,又叫Flexbox，意为“弹性布局”，旨在通过弹性的方式来对齐和分布容器中内容的空间，使其能适应不同屏幕，为盒装模型提供最大的灵活性。

Flex布局主要思想是：让容器有能力让其子项目能够改变其宽度、高度（甚至是顺序），以最佳方式填充可用空间；

3.2 Flex布局基于flex-flow流

容器默认存在两根轴：水平的主轴（main axis）和垂直的交叉轴（cross axis）。主轴的开始位置（与边框的交叉点）叫做main start，结束位置叫做main end；交叉轴的开始位置叫做cross start，结束位置叫做cross end。

项目默认沿主轴排列，单个项目占据的主轴空间叫做main size，占据的交叉轴空间叫做cross size

3.3 在React Native中，有4个容器属性，2个项目属性
```
容器属性：flexDirection   flexWrap   justifyContent  alignItems

项目属性：flex  alignSelf
```
3.3.1 flexDirection
>flexDirection容器属性: row | row-reverse | column | column-reverse

该属性决定主轴的方向（即项目的排列方向）。
```
row：主轴为水平方向，起点在左端。

row-reverse：主轴为水平方向，起点在右端。

column(默认值)：主轴为垂直方向，起点在上沿。

column-reverse：主轴为垂直方向，起点在下沿。
```
3.3.2 flexWrap
>flexWrap容器属性: nowrap | wrap | wrap-reverse

默认情况下，项目都排在一条线（又称"轴线"）上。flex-wrap属性定义，如果一条轴线排不下，如何换行。
```
nowrap(默认值)：不换行

wrap：换行，第一行在上方

wrap-reverse：换行，第一行在下方。（和wrap相反）
```
3.3.3 justifyContent
>justifyContent容器属性:flex-start | flex-end | center | space-between | space-around

定义了伸缩项目在主轴线的对齐方式
```
flex-start(默认值)：伸缩项目向一行的起始位置靠齐。

flex-end：伸缩项目向一行的结束位置靠齐。

center：伸缩项目向一行的中间位置靠齐。

space-between：两端对齐，项目之间的间隔都相等。

space-around：伸缩项目会平均地分布在行里，两端保留一半的空间
```
3.3.4 alignItems
>alignItems容器属性:flex-start | flex-end | center | baseline | stretch

定义项目在交叉轴上如何对齐，可以把其想像成侧轴（垂直于主轴）的“对齐方式”。
```
flex-start：交叉轴的起点对齐。

flex-end：交叉轴的终点对齐 。

center：交叉轴的中点对齐。

baseline：项目的第一行文字的基线对齐。

stretch（默认值）：如果项目未设置高度或设为auto，将占满整个容器的高度。
```
3.3.5 flex项目属性
>flex项目属性:flex-grow | flex-shrink | flex-basis

复合属性。设置或检索伸缩盒对象的子元素如何分配空间，其中第二个和第三个参数（flex-shrink、flex-basis）是可选参数。默认值为“0 1 auto”。

3.3.6 alignSelf项目属性
>alignSelf项目属性:auto | flex-start | flex-end | center | baseline | stretch

align-self属性允许单个项目有与其他项目不一样的对齐方式，可覆盖align-items属性。

默认值为auto，表示继承父元素的align-items属性，如果没有父元素，则等同于stretch。

我这归纳了一些平时RN开发常用的样式 [详见](https://blog.csdn.net/gongch0604/article/details/79962070)


## 五、导航路由（难点、重点）
在 React Native 中，官方已经推荐使用 react-navigation 来实现各个界面的跳转和不同板块的切换。  
react-navigation 主要包括三个组件：
>StackNavigator 导航组件  
>TabNavigator 切换组件  
>DrawerNavigator 抽屉组件

[详细参数配置及用法详解](https://blog.csdn.net/gongch0604/article/details/79957358)


1、StackNavigator  
 组件采用堆栈式的页面导航来实现各个界面跳转。它的构造函数：
```
StackNavigator(RouteConfigs, StackNavigatorConfig)
```
StackNavigator参数：
```
navigationOptions：配置StackNavigator的一些属性。  
  
    title：标题，如果设置了这个导航栏和标签栏的title就会变成一样的，不推荐使用  
    header：可以设置一些导航的属性，如果隐藏顶部导航栏只要将这个属性设置为null  
    headerTitle：设置导航栏标题，推荐  
    headerBackTitle：设置跳转页面左侧返回箭头后面的文字，默认是上一个页面的标题。可以自定义，也可以设置为null  
    headerTruncatedBackTitle：设置当上个页面标题不符合返回箭头后的文字时，默认改成"返回"  
    headerRight：设置导航条右侧。可以是按钮或者其他视图控件  
    headerLeft：设置导航条左侧。可以是按钮或者其他视图控件  
    headerStyle：设置导航条的样式。背景色，宽高等  
    headerTitleStyle：设置导航栏文字样式  
    headerBackTitleStyle：设置导航栏‘返回’文字样式  
    headerTintColor：设置导航栏颜色  
    headerPressColorAndroid：安卓独有的设置颜色纹理，需要安卓版本大于5.0  
    gesturesEnabled：是否支持滑动返回手势，iOS默认支持，安卓默认关闭  
   
  
screen：对应界面名称，需要填入import之后的页面  
  
mode：定义跳转风格  
  
   card：使用iOS和安卓默认的风格  
  
   modal：iOS独有的使屏幕从底部画出。类似iOS的present效果  
  
headerMode：返回上级页面时动画效果  
  
   float：iOS默认的效果  
  
   screen：滑动过程中，整个页面都会返回  
  
   none：无动画  
  
cardStyle：自定义设置跳转效果  
  
   transitionConfig： 自定义设置滑动返回的配置  
  
   onTransitionStart：当转换动画即将开始时被调用的功能  
  
   onTransitionEnd：当转换动画完成，将被调用的功能  
  
path：路由中设置的路径的覆盖映射配置  
  
initialRouteName：设置默认的页面组件，必须是上面已注册的页面组件  
  
initialRouteParams：初始路由参数 
```
RouteConfigs  
可以只配置RouteConfigs参数（本次分享只介绍第一个参数的配置和用法）  
RouteConfigs参数表示各个页面路由配置，React开发中的 Router路由配置 ，它是让导航器知道需要导航的路由对应的页面

React路由配置
```
ReactDom.render(
     <Router history={hashHistory}>
        <Route path='/' component={Page}></Route>
        <Route path='/NewTask' component={NewTask} />
        <Route path='/Redeem' component={Redeem} />
        <Route path='/Rule' component={Rule} />
    </Router> 
    ,document.getElementById("app"));
```
RN导航路由配置
```
//功能：任务首页->积分兑换->兑换规则
const MyNavigator = StackNavigator({
    Home: { 
        screen: Task,
        //加载首屏需要在navigationOptions里面配置首页导航信息
        navigationOptions: ({ navigation }) => ({
                headerTitle: "首页",
                headerBackTitle: null
            })
    },  
    Redeem: {  
        screen: Redeem
    },
    Rule: {
        screen: Rule
    }
})
AppRegistry.registerComponent("MyNavigator", () => App);

'注意：导航配置时要在路由的上一个页面设置headerBackTitle为null，不然路由跳转到的目的页面左侧返回按钮后面会默认带上上一个页面的标题'
```
```
页面的配置选项 navigationOptions 通常还可以在对应页面中去静态配置，比如在 Redeem 页面中（注意：如果默认是首页的话就要在声明static navigationOptions了）
class Redeem extends Component {
    static navigationOptions = ({ navigation }) => ({
        headerTitle: `${navigation.state.params.title}`,
        headerRight: (
            <View style={{ flexDirection: "row" }}>
                <Text
                    style={{ color: "#333", marginRight: 13 }}
                    onPress={() =>navigation.state.params ? navigation.state.params.jumpToRule() : null
                    }>
                    兑换规则
                </Text>
            </View>
        )
    });
    .....
}
一般子页面navigationOptions参数最好都在子页面里面去配置，不要在首页到导航参数里面配置，方便调用本类方法
```
路由跳转：  
已经配置好导航器以及对应的路由页面了，但是要完成页面之间的跳转，还需要 navigation   
navigation  
在导航器中的每一个页面，都有 navigation 属性，该属性有以下几个属性/方法：
```
navigate - 跳转到其他页面
调用这个方法可以跳转到导航器中的其他页面，此方法有三个参数
-routeName 导航器中配置的路由名称
-params 传递参数到下一个页面
-action action

state - 当前页面导航器的状态
state 里面包含有传递过来的参数 params 、 key 、路由名称 routeName 
{ 
  params: { param: 'i am the param' },
  key: 'id-1500546317301-1',
  routeName: 'Redeem' 
}

setParams - 更改路由的参数
更改当前页面路由的参数，比如可以用来更新头部的按钮或者标题

goBack - 返回
回退，可以不传，也可以传参数，还可以传 null 
this.props.navigation.goBack();       // 回退到上一个页面
this.props.navigation.goBack(null);   // 回退到任意一个页面
this.props.navigation.goBack('Home'); // 回退到Home页面
dispatch - 发送一个action
```
跳转  
首先在render里面声明navigate
```
const { navigate } = this.props.navigation;
```
然后通过点击事件调用navigate跳转
```
<TouchableOpacity
    style={styles.redeem}
    onPress={() => navigate("Redeem", { title: "积分兑换" })}>
    <Text style={styles.redeemText}>积分兑换</Text>
</TouchableOpacity>
```
到此就完成了一个页面到另一个页面的路由跳转以及页面的导航标题参数配置

导航右侧按钮配置
```
headerRight: (
    <View style={{ flexDirection: "row" }}>
        <Text
        style={{ color: "#333", marginRight: 13 }}
        onPress={() =>navigation.state.params ? navigation.state.params.jumpToRule() : null
        }>
            兑换规则
        </Text>
    </View>
)
```
使用react-navigation时,单页面设置navigationOptions中,进行Static中调用本类中的方法,不能像以下设置
onPress = {()=>this.jumpToRule()}  
解决方法：  
首先声明一个jumpToRule=()=>{}方法   
然后调用navigation中的setParams方法把jumpToRule方法作为一个参数set到页面路由中  
setParams要在声明周期的componentDidMount里面去执行
```
componentDidMount() {
    this.props.navigation.setParams({
        jumpToRule: this.jumpToRule
    });
}
```

## 六、屏幕适配
[详见](https://blog.csdn.net/gongch0604/article/details/79971588)



结语：这是在近半月开发RN的过程中总结的一些知识以及遇到的一些问题，目前运用的也不是太熟练，有兴趣的同学大街可以一块学习探讨，共同进步

知识点：  
点击事件
> 1. 按钮按下事件：onPress          - 按下并松开按钮，会触发该事件(相当于PC的onclick)  
> 2. 按钮长按事件：onLongPress  - 按住按钮不松开，会触发该事件(长按事件)
> 3. 按钮按下事件：onPressIn       - 按下按钮不松开，会触发该事件(相当于PC的onkeydown)  
> 4. 按钮松开事件：onPressOut    - 按下按钮后松开，或按下按钮后移动手指到按钮区域外，都会触发该事件(相当于PC的onkeyup) 

[RN触摸事件详解](https://blog.csdn.net/jw_45840/article/details/52336675)

disabled bool  
如果设为true，则禁止此组件的一切交互。  
> 当我们没有对Touchable组件设置onLongPress属性而设置了onPress属性的时候，我们长按按钮之后会回调onPress方法。另外，我们也可以通过delayLongPress方法来这设置从onPressIn被回调开始，到onLongPress被调用的延迟。

[React Native触摸事件详解](http://www.360doc.com/content/16/0711/23/34978982_574835465.shtml)