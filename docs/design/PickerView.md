## pickerView多滚轮选中组件

## 1、功能说明

页面底部弹出的半屏滚轮选择弹窗

目前仅支持到双滚轮，三滚轮的后期再扩展



## 2、组件效果

##### 单滚轮

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/pickerView1.png" width="375"/>

##### 双滚轮

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/pickerView.png" width="375"/>



## 3、数据格式

```
单滚轮数据
{
    option: [
        {
            text: "双轴",
            value: "685908"
        },
        {
            text: "三轴",
            value: "685909"
        },
        {
            text: "其他",
            value: "685932"
        }
    ],
    paramname: "zhoushu",//参数名
    title: "轴数"
}

双滚轮数据
{
    option: [
        {
            isparent: true,
            option: [
                {
                    isparent: false,
                    paramname: "shangpaiyuefen",
                    text: "1",
                    value: "515681"
                },
                {
                    isparent: false,
                    paramname: "shangpaiyuefen",
                    text: "2",
                    value: "515682"
                }
            ],
            paramname: "shangpainian",
            text: "2017年",
            value: "2017"
        },
        {
            isparent: true,
            option: [
                {
                    isparent: false,
                    paramname: "shangpaiyuefen",
                    text: "10",
                    value: "515690"
                },
                {
                    isparent: false,
                    paramname: "shangpaiyuefen",
                    text: "11",
                    value: "515691"
                },
                {
                    isparent: false,
                    paramname: "shangpaiyuefen",
                    text: "12",
                    value: "515692"
                }
            ],
            paramname: "shangpainian",
            text: "2016年",
            value: "2016"
        }
    ],
    paramname: "buytime",
    title: "出厂年限"
}
```



## 4、代码演示

```
/**
 * @description 单滚轮
 * @author gongchenghui
 * @version 2018.11.05
 */
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
import { PickerView } from "rn-design";
const styles = StyleSheet.create({
    okText: {
        color: "#dc4931"
    },
    dismissText: {
        color: "#434345"
    },
    title: {
        fontSize: 16,
        color: "#999"
    },
    button: {
        borderWidth: 1,
        borderColor: "red",
        padding: 5,
        width: 100
    }
});
const pickerData = {
    lettersort: false,
    option: [
        {
            text: "双轴",
            value: "685908"
        },
        {
            text: "三轴",
            value: "685909"
        },
        {
            text: "其他",
            value: "685932"
        }
    ],
    paramid: 12286,
    paramname: "zhoushu",
    title: "轴数",
    viewtype: "1"
};

export default class Demo extends Component {
    constructor(props) {
        super(props);
        this.state = {};
    }
    setPickerData = (value) => {
        // console.log(value);
    };
    render() {
        return (
            <View style={styles.flex}>
                <PickerView
                    data={pickerData}
                    onChange={(item) => this.setPickerData(item)}
                    okText={<Text style={styles.okText}>确定</Text>}
                    dismissText={<Text style={styles.dismissText}>取消</Text>}
                    title={<Text style={styles.title}>请选择时间</Text>}>
                    <TouchableHighlight
                        activeOpacity={0.5}
                        style={[styles.button]}
                        underlayColor="#a9d9d4">
                        <Text>show popup</Text>
                    </TouchableHighlight>
                </PickerView>
            </View>
        );
    }
}

```



## 5、API

| 参数           | 说明                                                         | 类型                      | 默认值                               |
| -------------- | ------------------------------------------------------------ | ------------------------- | ------------------------------------ |
| data           | pickerView展示数据                                           | object                    | 无                                   |
| title          | pickerView标题 如果传入的是<br />React.ReactElement一定要<br />自定义样式，否则不会继承组件设置的默认样式 | string\React.ReactElement | 请选择                               |
| okText         | pickerView确定按钮文案                                       | string\React.ReactElement | 确定                                 |
| dismissText    | pickerView关闭按钮文案                                       | string\React.ReactElement | 取消                                 |
| styles         | pickerView弹窗样式<br />（已添加弹窗样式，此参数谨慎使用）   | StyleSheet.create         | -                                    |
| itemStyle      | pickerView滚轮字体设置                                       | StyleSheet.create         | { color: "#333",<br />fontSIze: 18 } |
| onSelectChange | pickerView选中结果 回调结果示例：<br />[{"text":"双轴","value":"685908",<br />"paramname":"zhoushu"}]<br />[<br />{"isparent":true,<br />"paramname":"cjshijian",<br />"text":"2017","value":"2017"},<br />{"isparent":false,<br />"paramname":"cjshijian_month",<br />"text":"7","value":"7"}] | function                  | 无                                   |

注：组件调用时，把需要点击的DOM模块包裹在PickerView标签里面