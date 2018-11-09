# 多选选组件

#### 实现效果

单列布局  
<img src="http://c.58corp.com/download/attachments/22414141/image2018-7-30%2010%3A9%3A55.png?version=1&modificationDate=1532916595000&api=v2" width="250">
<img src="http://c.58corp.com/download/attachments/22414141/image2018-7-30%2010%3A10%3A28.png?version=1&modificationDate=1532916628000&api=v2" width="250">
<img src="http://c.58corp.com/download/attachments/22414141/image2018-7-30%2010%3A10%3A48.png?version=1&modificationDate=1532916648000&api=v2" width="250">

多列布局


<img src="http://c.58corp.com/download/attachments/22414141/image2018-7-30%2010%3A11%3A57.png?version=1&modificationDate=1532916717000&api=v2">
<img src="http://c.58corp.com/download/attachments/22414141/image2018-7-30%2010%3A12%3A35.png?version=1&modificationDate=1532916755000&api=v2">
<img src="http://c.58corp.com/download/attachments/22414141/image2018-7-30%2010%3A12%3A53.png?version=1&modificationDate=1532916773000&api=v2">
<img src="http://c.58corp.com/download/attachments/22414141/image2018-7-30%2010%3A16%3A33.png?version=1&modificationDate=1532916993000&api=v2">

#### API

pram | description | type 
---|--- | --- 
dataOption | 组件传值数据示例：[{ value: 1, text: "测试一号", checked: false, disabled: false },<br> { value: 2, text: "测试二号", checked: false, disabled: false },<br>{ value: 3, text: "测试三号", checked: true, disabled: false },<br>{ value: 4, text: "测试四号", checked: false, disabled: false },<br>{ value: 5, text: "测试五号", checked: false, disabled: false },<br>{ value: 6, text: "测试六号", checked: false, disabled: false }] | Array 
options | 参数属性<br>{id:"对应数组对象CheckBox的value值，如：value", 必填<br>value:"对应数组对象CheckBox的text值，如：text", 必填<br>checked:"默认是否选中，也是勾选时选中与否的校验", 必填<br>disabled:"默认置灰不能勾选的选项，如：disabled" 非必填} | Object 
onValueChange | 获取设置选中值<br>onValueChange={(data) => this.setState({ checkedArr: data })}<br>返回选中的值是一个数组 | function 
initStyle | 自定义行内样式（包括背景颜色，行高等） | Styles 
txtColor | 定义CheckBox text颜色 | String 
activeTxtColor | 定义CheckBox text选中时的颜色 | String  
noneColor | 定义disabled时的文案样式 | String 
isPeer | 布局方式 1：一行一列 2：一行两列 ... 根据自己的页面自己定义每行几列<br>布局方式 1：一行一列 2：一行两列 ... 根据自己的页面自己定义每行几列 | String 
selAll | 是否显示全选按钮 默认false不展示 不配置此属性也是不展示的	 | Boolean 


#### Demo
```
import React, { Component, PropTypes } from "react";
import {
    StyleSheet,
    Text,
    ScrollView,
    Image,
    Alert,
    TextInput,
    View,
    TouchableOpacity
} from "react-native";
import { Radio, Toast, CheckBox } from "./../../rn-design";
const styles = StyleSheet.create({
    flex: {
        flex: 1,
        marginTop: 65
    },
    listItem: {
        height: 40,
        marginLeft: 10,
        marginRight: 10,
        borderBottomWidth: 1,
        borderBottomColor: "#ddd",
        justifyContent: "center"
    },
    listItemFont: {
        fontSize: 16
    },
    initStyle: {
        backgroundColor: "#fff",
        paddingHorizontal: 15,
        height: 50
    },
    labelStyle: {
        fontSize: 14
    },
    input: {
        borderWidth: 1,
        borderColor: "#ccc",
        borderRadius: 4,
        height: 30
    }
});

class Home extends Component {
    constructor(props) {
        super(props);
        this.state = {
            text: "",
            textarea: "",
            checkedArr: [],
            checkData: [
                { value: 1, text: "测试一号", checked: false, disabled: false },
                { value: 2, text: "测试二号", checked: false, disabled: false },
                { value: 3, text: "测试三号", checked: true, disabled: false },
                { value: 4, text: "测试四号", checked: false, disabled: false },
                { value: 5, text: "测试五号", checked: false, disabled: false },
                { value: 6, text: "测试六号", checked: false, disabled: false }
            ]
        };
    }
    getFormData = () => {
        let radioId = this.state.initId;
        console.log(JSON.stringify(this.state.checkedArr));
        Toast.show("test", 2000);
    };
    render() {
        const { navigate } = this.props.navigation;
        return (
            <ScrollView>
                    <TouchableOpacity onPress={this.getFormData.bind(this)}>
                        <Text>获取选中值</Text>
                    </TouchableOpacity>
                    <CheckBox
                        dataOption={this.state.checkData}
                        options={{
                            id: "value",
                            value: "text",
                            checked: "checked",
                            disabled: "disabled"
                        }}
                        onValueChange={(arr) => this.setState({ checkedArr: arr })}
                        innerStyle={styles.initStyle}
                        isPeer={3}
                        selAll={true}
                    />
                </View>
            </ScrollView>
        );
    }
}

export default Home;

```

