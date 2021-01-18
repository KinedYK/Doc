**掌中星球小程序 项目总结**

```
1. 优化小程序页面的样式。按照设计图参数调整细节处间隙，颜色，适当增加小动画。

2. 整理wxss文件。规范样式命名规则，遵循样式与结构分离原则；提取公共样式，常用的边距和色值。

3. 提取公共方法。路由获取；距离换算；保留小数位；

4. 优化埋点。在wx.onAppRoute统一回调。

5. 整理目录结构。统一命名规则，分包以 “package-”开头 ；分包内组件单独放置在内部components目录下，按需要提升到全局组件目录。

6. 输出基础组件。日历选择；发票管理；金额展示；自定义头部；搜索框；footer; nav-tabs;

7. 优化订单状态及操作。根据状态动态渲染。

8. 减小小程序体积。引入iconfont 替代 image。拆分公共功能模块单独分包。删除无用文件。提取公共代码，去除不合理的代码嵌套判断等。
```

**技术优化**

```
1.   Footer组件，采用父及position relative; 子级position fixed; 让固定定位占有内容高度。无需在引入页面添加padding来避免被footer遮挡的内容

2.   Iconfont样式 单独文件，在app.wxss中引入。

3.   自动化切换环境。wx.getAccountInfoSync().miniProgram.envVersion。兼容问题不算太成功。

4.   小程序环信接入。单独分包。自动登录，跳转到聊天页。

5.   Api错误统一处理。同一错误码。

6.   封装统一的自动上拉加载下一页。Template, 利用抽象节点实现子节点循环

7.   统一表单绑定，用Object key-value 方式setData。

8.   提交订单按钮 增加 函数节流 控制，输入搜索 增加 函数防抖 控制。

9.   页面吸顶优化，将逻辑层的监听滚动修改样式，移动到wxs中 利用 this.selectComponent() 的实例修改样式。
```

**技术难点**

```
1.   消息推送。一次性消息订阅，无法重复推送消息。

2.   兄弟组件通讯。Drop-Tabs，不提供类似vue 的 $slots 方法。

3.   自动环境识别，在审核代码时有概率网络错误。Api兼容问题。

4.   抽象节点，分页组件子节点接受父节点item参数循环展示。
```

**微信小程序 bug**

```
1. 禁止浮层后的页面滚动，在最外层<view catchtouthmove = 'true'>可解决。但是在wechatDevelope工具的模拟器上不生效，真机有效。
```

```
2. 小程序 wx.navigateTo  跳转传参数时， json解析数据报如下错误 lifeCycleMethod onLoad function SyntaxError: Unexpected end of JSON input
一般获取的json数据先转换成string字符，如：JSON.stringify(json)，然后用encodeURIComponent 编码，如encodeURIComponent(string), 就可以传参过去了，到接收页 ，先 decodeURIComponent(data),然后在 JSON.parse() 取得json数据 赋值！
```

```
3. background-clip: text; iOS无法显示文字
   参考：https://stackoverflow.com/questions/44963978/safari-on-ios-not-displaying-text-when-using-background-clip-and-text-fill-color/47819393
```

```
4. 小程序红包 paySign 生成方法，可以由前端生成也可以由后端生成。微信文档示例有问题，具体生成方法参照文档最下面
https://pay.weixin.qq.com/wiki/doc/api/tools/cash_coupon.php?chapter=18_3&index=4

paySign生成方法
  字段说明：paySign 字段是对本次发起JSAPI 的行为进行鉴权，只有通过了paySign 鉴权，才能继续对package 鉴权并调起红包的收和拆。

  生成规则：参与paySign 签名的字段包括：appId、timeStamp、nonceStr、package 。这里signType 并不参与签名。

  1 . 对所有待签名参数按照字段名的 ASCII码从小到大排序（字典序）后，使用 URL 键 值对的格式（即key1=value1&key2=value2…）拼接成字符串string1。这里需要注意的是所有参数名均为驼峰形式
  2 . 在string1最后拼接上key=商户支付密钥得到stringSignTemp字符串
  3 . 对 stringSignTemp 作MD5签名算法，字段名和字段值都采用原始值（package需要进行urlencode再进行加密），最后得到的字符串不用转成大写， 具体签名算法为 paySign =MD5(stringSignTemp)
```

