## DatePicker日历选中组件

## 1、功能说明

年、月、日多滚轮日期选择组件



## 2、组件效果

<img src="https://img.58cdn.com.cn/escstatic/fecar/pmuse/reactnative/datePicker1.png" width="375"/>



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
    TouchableHighlight
} from "react-native";
import { Radio, CheckBox, DatePicker } from "@/rn-design";
const styles = StyleSheet.create({
    button: {
        borderWidth: 1,
        borderColor: "red",
        padding: 5,
        width: 300
    },
    okText: {
        color: "#dc4931"
    },
    dismissText: {
        color: "#434345"
    },
    title: {
        fontSize: 13,
        color: "#999"
    }
});

class Demo extends Component {
    constructor(props) {
        super(props);
        this.state = {
            dateValue: undefined
        };
    }
    onChange = (dateValue) => {
        this.setState({ dateValue });
    };
    format(date) {
        let mday = date.getDate();
        let month = date.getMonth() + 1;
        month = month < 10 ? `0${month}` : month;
        mday = mday < 10 ? `0${mday}` : mday;
        return `${date.getFullYear()}-${month}-${mday}`;
    }
    show = () => {
        // console.log('show')
    };
    render() {
        return (
            <ScrollView>
                <DatePicker
                    defaultDate={new Date()}
                    value={this.state.dateValue}
                    mode="date"
                    minDate={new Date(2010, 1, 1)}
                    maxDate={new Date(2020, 12, 31)}
                    onChange={this.onChange}
                    okText={<Text style={styles.okText}>确定</Text>}
                    dismissText={<Text style={styles.dismissText}>取消</Text>}
                    title={<Text style={styles.title}>请选择时间</Text>}>
                    <TouchableHighlight
                        onPress={this.show}
                        activeOpacity={0.5}
                        style={[styles.button]}
                        underlayColor="#a9d9d4">
                        <Text>
                            {(this.state.dateValue && this.format(this.state.dateValue)) || "open"}
                        </Text>
                    </TouchableHighlight>
                </DatePicker>
            </ScrollView>
        );
    }
}

export default Demo;

```





## 4、API

| 参数          | 说明                                                         | 类型                      | 默认值            |
| ------------- | ------------------------------------------------------------ | ------------------------- | ----------------- |
| styles        | 自定义DatePicker样式                                         | Object                    | pickerStyles      |
| value         | 选中值                                                       | String                    | 当前日期          |
| mode          | 日期类型： 1、date：年月日 格式YYYY-MM-DD 2、time：时分 格式HH:mm 3、datetime：年月日时分 格式YYYY-MM-DD HH:mm | String                    | date              |
| defaultDate   | 默认选中日期                                                 | String                    | 当前日期          |
| minDate       | 日期的起始时间                                               | Date                      |                   |
| maxDate       | 日期的截止时间                                               | Date                      |                   |
| onValueChange | 改变日期选择时执行                                           | Function                  |                   |
| locale        | 时间面板选择中文还是英文 CN：表示中文后面带年月日时分的单位 US:表示英文后面不带单温 | String                    | CN                |
| okText        | picker右上角确定按钮配置                                     | StringdismissText         | 确定              |
| dismissText   | picker左上角取消按钮配置                                     | String/React.ReactElement | 取消              |
| title         | picker title配置                                             | String/React.ReactElement | 请选择            |
| itemStyle     | picker样式                                                   | object                    | { color: "#333" } |