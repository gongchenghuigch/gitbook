## Radio单选框

## 1、功能说明

用于在多个备选项中选中单个状态。

Radio 所有选项默认可见，方便用户在比较中选择，因此选项不宜过多



## 2、组件效果

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/radio1.png" width="375" />

## 3、代码演示

```
import React, { Component } from "react";
import { Alert, View, Text, StyleSheet, TouchableOpacity, Image, ScrollView } from "react-native";
import { Radio } from "@/rn-design";
const RadioButton = Radio.Button;
const styles = StyleSheet.create({
    content: {
        flexDirection: "row",
        alignItems: "center",
        paddingHorizontal: 15,
        backgroundColor: "#fff",
        justifyContent: "space-between"
    },
    radioContent: {
        flexDirection: "row",
        alignItems: "center"
    },
    innerStyle: {
        height: 50
    }
});

export default class DemoTest extends Component {
    constructor(props) {
        super(props);
        this.state = {
            radioData: [
                { content: "商家评分", value: "1", disabled: false },
                { content: "个人评分", value: "2", disabled: false }
            ],
            selected: 2
        };
    }

    componentDidMount() {}

    render() {
        // console.log(RadioButton);
        return (
            <View>
                <View style={styles.content}>
                    <View>
                        <Text>测试Radio</Text>
                    </View>
                    <Radio
                        style={styles.radioContent}
                        dataOption={this.state.radioData}
                        options={{ value: "value", text: "content", disabled: "disabled" }}
                        selectedValue={this.state.selected}
                        disabledAll={false}
                        innerStyle={styles.innerStyle}
                        onChange={(item) => {
                            console.log(item);
                        }}
                        // txtColor="#ff552e"
                    />
                </View>
                {/* <RadioButton /> */}
            </View>
        );
    }
}

```



## 4、API

| 参数          | 说明                                                         | 类型     | 默认值 |
| ------------- | ------------------------------------------------------------ | -------- | ------ |
| style         | 当前Radio group的父级样式                                    | object   | 无     |
| dataOption    | Radio渲染当前Radio的数据                                     | array    | 无     |
| options       | 匹配Radio组件渲染时的text和value值（必填）<br />{<br /> value:"对应dataOption中确定当前唯一标识的key",<br />text:"对应dataOption中要展示的文案",<br />disabled:"对应dataOption中当前选中是否禁用的标识，如果dataOption中没有改属性，默认全部是false，radio无禁用状态"<br />} | object   | 无     |
| selectedValue | 用于设置当前选中的值，也可以设置默认值                       | any      | 无     |
| disabledAll   | 是否禁用全部Radio                                            | boolean  | false  |
| seledImg      | Radio选中图标 传值示例：{uri:"...."} 不支持本地图片          | object   | -      |
| selImg        | Radio未选中图标 传值示例：{uri:"...."} 不支持本地图片        | object   | -      |
| innerStyle    | 单个Radio样式模块                                            | Object   | -      |
| txtColor      | Radio文案颜色                                                | string   | -      |
| onChange      | 选项变化时的回调函数                                         | function | 无     |





## RadioButton

## 1、组件效果

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/radio.png" width="375" />



## 2、代码演示

```
import React, { Component } from "react";
import { Alert, View, Text, StyleSheet, TouchableOpacity, Image, ScrollView } from "react-native";
import { Radio } from "@/rn-design";
const RadioButton = Radio.Button;
const styles = StyleSheet.create({
    content: {
        // flexDirection: "row",
        // alignItems: "center",
        paddingHorizontal: 15,
        backgroundColor: "#fff"
        // justifyContent: "space-between"
    },
    radioContent: {
        flexDirection: "row",
        alignItems: "center"
    },
    innerStyle: {
        height: 50
    }
});

export default class DemoTest extends Component {
    constructor(props) {
        super(props);
        this.state = {
            radioBtn: [
                { text: "不限", value: "0" },
                { text: "1万公里内", value: "0_1" },
                { text: "3万公里内", value: "0_3" },
                { text: "5万公里内", value: "0_5" },
                { text: "8万公里内", value: "0_8" }
            ],
            selected: "0_1"
        };
    }

    componentDidMount() {}

    render() {
        return (
            <View>
                <View style={styles.content}>
                    <View style={{ height: 45, justifyContent: "center" }}>
                        <Text>测试Radio</Text>
                    </View>
                    <RadioButton
                        dataOption={this.state.radioBtn}
                        options={{ value: "value", text: "text", disabled: "disabled" }}
                        selectedValue={this.state.selected}
                        onChange={(item) => {
                            console.log(item);
                        }}
                        size={[78, 37]}
                        // seledImg={require("./../img/filter/9@3x.png")}
                    />
                </View>
            </View>
        );
    }
}

```



## 3、API

| 参数                  | 说明                                                         | 类型     | 默认值    |
| --------------------- | ------------------------------------------------------------ | -------- | --------- |
| dataOption            | Radio渲染当前Radio的数据                                     | array    | 无        |
| options               | 匹配Radio组件渲染时的text和value值（必填）<br />{<br /> value:"对应dataOption中确定当前唯一标识的key",<br />text:"对应dataOption中要展示的文案",<br />disabled:"对应dataOption中当前选中是否禁用的标识，如果dataOption中没有改属性，默认全部是false，radio无禁用状态"<br />} | object   | 无        |
| selectedValue         | 用于设置当前选中的值，也可以设置默认值                       | any      | 无        |
| disabledAll           | 是否禁用全部Radio                                            | boolean  | false     |
| seledImg              | Radio选中图标 传值示例：{uri:"...."} 不支持本地图片          | object   | -         |
| onChange              | 选项变化时的回调函数                                         | function | 无        |
| txtColor              | Radio字体颜色                                                | string   | "#555555" |
| activeTxtColor        | Radio选中字体颜色                                            | string   | "#FF552E" |
| backgroundColor       | Radio按钮块背景颜色                                          | string   | "#F6F6F6" |
| activeBackgroundColor | Radio按钮块选中背景颜色                                      | string   | "#FFEFEB" |
| size                  | Radio按钮块大小 如[80,38]                                    | int[]    | [78,37]   |