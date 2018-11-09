## Swiper轮播图组件

## 1、功能说明

banner图标或者文字轮播



## 2、组件效果

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/swiper.gif" width="375"/>





## 3、代码演示

```
import React, { Component } from "react";
import {
    StyleSheet,
    Text,
    ScrollView,
    Image,
    Alert,
    TextInput,
    View,
    TouchableOpacity,
    TouchableHighlight,
    Dimensions
} from "react-native";
import { Radio, CheckBox, DatePicker, Swiper } from "@/rn-design";
import PropTypes from "prop-types";
const styles = StyleSheet.create({
    flex: {
        flex: 1,
        marginTop: 65
    },
    image: {
        width: Dimensions.get("window").width,
        height: 80
    },
    item: {
        height: 100,
        justifyContent: "center",
        alignItems: "center"
    },
    one: {
        backgroundColor: "#0ca"
    },
    two: {
        backgroundColor: "#0cc"
    },
    three: {
        backgroundColor: "#0ac"
    },
    paginationStyle: {
        flexDirection: "row",
        alignItems: "center",
        justifyContent: "center",
        bottom: 10
    }
});

class Demo extends Component {
    constructor(props) {
        super(props);
        this.state = {
            images: [
                {
                    uri: "http://img.58cdn.com.cn/images/car/activitypic/reactnative/pic1.jpg"
                },
                {
                    uri: "http://img.58cdn.com.cn/images/car/activitypic/reactnative/pic2.jpg"
                }
            ]
        };
    }
    static defaultProps = {
        count: ["one", "two", "three"]
    };

    render() {
        //图片轮播
        const images = this.state.images.map((source, index) => (
            <View key={index} style={styles.image}>
                <Image style={styles.image} source={source} resizeMode={"stretch"} />
            </View>
        ));
        //普通轮播
        const items = this.props.count.map((name, index) => (
            <View key={index} style={[styles.item, styles[name]]}>
                <Text>{name}</Text>
            </View>
        ));
        return (
            <ScrollView>
                <View style={styles.flex}>
                    <Swiper
                        width={Dimensions.get("window").width}
                        height={80}
                        autoplay={true}
                        horizontal={true}
                        loop={true}
                        index={1}
                        paginationStyle={styles.paginationStyle}
                        showsPagination={true}
                        dotColor="#000"
                        dotStyle={{ width: 5, height: 5, backgroundColor: "red" }}
                        autoplayTimeout={3}>
                        {images}
                    </Swiper>
                </View>
            </ScrollView>
        );
    }
}

export default Demo;

```



## 4、API

| 参数            | 说明                                                         | 类型   | 默认值        |
| --------------- | ------------------------------------------------------------ | ------ | ------------- |
| width           | 宽度，默认flex:1全屏如果不写宽高可能会导致内容闪屏一下       | number | -             |
| height          | 高度，默认flex:1全屏                                         | number | -             |
| index           | 初始进入页面标识为0的页面                                    | number | 0             |
| style           | 设置轮播样式                                                 | style  | -             |
| horizontal      | 设置轮播方向，默认水平方向                                   | bool   | true          |
| loop            | 是否循环播放                                                 | bool   | true          |
| autoPlay        | 是否自动轮播                                                 | bool   | false         |
| autoplayTimeout | 设置轮播间隔                                                 | number | 2.5           |
| onIndexChanged  | 当用户拖拽时更新索引调用                                     | func   | (index)=>null |
| showsPagination | 是否显示小圆点                                               | bool   | true          |
| paginationStyle | 设置轮播原点的整体样式{flexDirection: "row",alignItems: "center",justifyContent: "center",bottom: 10} | style  | -             |
| dotColor        | 设置未选中小圆点颜色                                         | string | #fff          |
| dotStyle        | 设置未选中小圆点样式{ width: 10, height: 2, backgroundColor: "red" }注意：如果dotColor和dotStyle同时存在，dotColor会覆盖dotStyle里面的backgroundColor | style  | -             |
| activeDotColor  | 设置选中小圆点颜色                                           | string | #ff552e       |
| activeDotStyle  | 设置选中小圆点样式（同上）                                   | style  | -             |