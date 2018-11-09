自从React Native出世，虽然官方一直尽可能的优化其性能，为了能让其媲美原生App的速度，但是现实感觉有点不尽人意。接下来介绍下实践中遇到的一些性能问题以及优化方案。
# 一、StackNavigator页面切换动画优化

> 场景：在navigation还没出来时，导航路由使用NavigatorIOS来实现，页面切换是很流畅的，但是用了StackNavigator navigation发现页面切换会使JS线程出现严重的掉帧（卡顿现象）；

原因：  
NavigatorIOS的切换动画是跑在UI主线程上，而不是JS线程上的，所以不受JS线程上的掉帧影响；但是NavigatorIOS是但平台专用，在我们业务开发中是不能使用的；而react-navigation是跨平台的，也是官方推荐使用的；

问题：  
react-navigation在从A页面push到B页面时，如果B页面render存在大量可操作的视图结构，或者componentDidMount方法里面做了耗时的操作，会发现丢帧的现在，也就是页面页面会卡顿2s左右，从用户体验上来讲是非常不友好的。
> react-navigation动画是由JS线程控制的

> 想象一下“从右边推入”这个场景的切换：每一帧中，新的场景从右向左移动，从屏幕右边缘开始（不妨认为是320单位宽的的x轴偏移），最终移动到x轴偏移为0的屏幕位置。切换过程中的每一帧，JavaScript线程都需要发送一个新的x轴偏移量给主线程。

> 由于JS就是一单线程的，他会优先处理自页面的dom构建，以及componentDidMount中的事件处理，这是JS线程就无法处理转场动画了，知道处理完自页面释放之后在来处理动画，这就导致切换页面出现卡顿现象

解决方案：  
使用API InteractionManager，它的作用就是可以使本来JS的一些操作在动画完成之后执行，这样就可确保动画的流程性

InteractionManager简介  
### 1、可以提升用户体验和交互效果的模块InteractionMnager(交互管理器)


### 2、基本内容
> 使用InteractionManager可以让一些耗时的任务在交互操作或者动画完成之后进行执行，这样使用可以保证我们的JavaScript的动画效果可以平滑流畅的执行。可以大大提升用户体验。

在应用开发中我们可以如下进行执行任务

```
InteractionManager.runAfterInteractions(() => {
 
  //执行耗时的同步任务
 
});
```
该模块和其他相关的调度方法对比:

- requestAnimationFrame():执行控制动画效果的代码
- setImmediate/setTimeout():设置延迟执行任务的时间，该可能会影响到正在执行的动画
- runAfterInteractions():延迟执行任务，该不会影响到正在执行的动画效果

触摸系统中的单点或者多点触控都是交互动作，耗时任务会在这些触摸交互动作执行完成之后或者取消以后回调runAfterInteractions()方法进行执行。

runAfterInteractions任务也可以接收一个普通的回调函数或者一个带有gen方法并且返回一个Promise的PromiseTask对象。如果参数是PromiseTask对象，那么任务是异步执行的，也会阻塞。该会等着当前任务执行完毕以后才能执行下一个任务。

默认情况下，队列任务会一次性在setImmediate方法中批量执行。如果你通过setDeadline方法设置一个时间值，那么任务会在延迟该设定值时间进行执行。这时候会调用setTimeout方法进行挂起任务并且阻塞其他任务的执行。这样可以给触摸交互等操作留出时间更好的相应用户操作。

### 3、方法与属性
1.  runAfterInteractions(task)  静态方法,在用户交互和动画结束以后执行任务
2.  createInteractionHandle() 静态方法，创建一个句柄(处理器)，通知管理器，某个动画或者交互开始了
3.  clearInteractionHandle(handler:Handle)  静态方法，进行清除句柄，通知管理器，某个动画或者交互结束了。
4.  setDeadline(deadline:number) 静态方法, 设置延迟时间，该会调用setTimeout方法挂起并且阻塞所有没有完成的任务，然后在eventLoopRunningTime到设定的延迟时间后，然后执行setImmediate方法进行批量执行任务
5.  Events:CallExpression
6.  addListener:CallExpression

### 4、具体使用
开发中关于卡顿的解决：  
先在render中渲染一个空视图，等转场动画完成以后，再去渲染世纪的视图

代码如下：

```
//页面引入InteractionManager
import {
    AppRegistry,
    StyleSheet,
    Text,
    ScrollView,
    Image,
    Alert,
    TouchableOpacity,
    InteractionManager,
    View,
    ImageBackground,
    DeviceEventEmitte
} from "react-native";
class Redeem extends Component {
    constructor(props) {
        super(props);
        this.state = {
            renderPlaceholderOnly: true//交互管理器延时控制标识
        };
        
    }
    /**
     * 页面初始化先渲染空视图减少页面转场时间
     */
    _renderPlaceholderView = () => {
        return (
            <View
                style={{
                    flex: 1,
                    justifyContent: "center",
                    alignItems: "center"
                }}>
                <Image
                    style={{ width: 150, height: 150, backgroundColor: "#f5f5f5" }}
                    source={require("./../../img/task/loading.gif")}
                />
            </View>
        );
    };
    render() {
        //首先渲染空视图
        if (this.state.renderPlaceholderOnly) {
            return this._renderPlaceholderView();
        }
        return(
            //页面dom
            ...
        )
    }
    componentDidMount() {
        //转场动画完成之后改变标记值，重新渲染dom
        InteractionManager.runAfterInteractions(() => {
            this.setState({ renderPlaceholderOnly: false });
        });
    }
}

```

# 二、事件监听

> 场景：在页面跳转是如从A页面路由跳转到B页面，需要在B页面更新了数据，返回A页面是需要更新A页面数据

问题：  
由于react路由是基于hash来实现的，从A页面路由跳转到B页面，会在URL上拼接页面对应的hash值，从B页面在返回A页面时，去除URL上的hash值，相当于改变了URL，此时A页面会重新加载，从而达到刷新页面的效果；  

但是react-navigation的路由跳转是基于栈来实现的，A路由跳转B，就是一个压栈的过程，就是把页面B push到栈顶，从B返回到A是就是pop弹栈，此时对于页面A来说是无感知的，就是把B页面从栈中pop出去，此时对于开发者来说就无法感知B页面的返回操作了；

解决方案：

使用RN神器发送和接收事件DeviceEventEmitter

具体使用如下：

```
页面A：

//首 先导入DeviceEventEmitter组件
import {
    StyleSheet,
    Text,
    ScrollView,
    Image,
    Alert,
    TouchableOpacity,
    TouchableWithoutFeedback,
    TouchableHighlight,
    InteractionManager,
    View,
    FlatList,
    SectionList,
    DeviceEventEmitter
} from "react-native";
//要在A页面写一个接收消息的方法
//在didmount方法中写好监听/接收方法，当有消息发送时，这就会接收到，并执行相应方法
componentDidMount() {
        //收到监听 事件监听
        //从新手任务页面或者积分兑换页面返回需要监听 然后更新任务首页总积分
        //原因：新手任务页面和积分兑换页面都会设计积分的变更，但是返回任务首页时不会刷新页面，
        //所以需要监听返回状态，在主动调用接口
        this.listener = DeviceEventEmitter.addListener("left", (e) => {
            //e就是从页面B发送过来的数据
            if (e) {
                this.getAllTntegral();
            }
            Toast.info(e);
        });
        
    }
}
//当然，别忘了要卸载
componentWillUnmount() {
        // 移除监听
        this.listener.remove();
}
```


```
页面B
//首 先导入DeviceEventEmitter组件
import {
    StyleSheet,
    Text,
    ScrollView,
    Image,
    Alert,
    TouchableOpacity,
    View,
    ImageBackground,
    InteractionManager,
    FlatList,
    DeviceEventEmitter
} from "react-native";
//可根据自己的业务场景来实现消息发送的时机，本人用法是在B页面返回时，给A页面发消息通知
class NewTask extends Component {
    static navigationOptions = ({ navigation }) => ({
        headerTitle: `${navigation.state.params.title}`,
        headerLeft: (
            <TouchableOpacity
                style={styles.action}
                onPress={() => {
                    //给监听事件赋值
                    DeviceEventEmitter.emit("left", "back");
                    navigation.goBack();
                }}>
                <View style={styles.headerLeft} />
            </TouchableOpacity>
        )
        // headerBackTitle: `${navigation.state.params.title}`
    });
}
```


