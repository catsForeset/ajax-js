微信小程序

# 输出hello world
- 用微信开发者工具新建项目，里面有个pages目录，该目录下的文件夹包含主要开发文件，开发一般在这里进行。初始pages目录下只有index和log两个文件夹。
- 在目录下创建一个测试目录test，再文件夹中“新建page”，自动生成四个基本文件。其中 .js 是数据逻辑处理文件， .wxml 是视图文件， .wxss 是样式文件，和 .json文件。
- 加载页面入口是 .js 文件， .js 文件执行方法和处理数据后，渲染 .wxml 文件。
- .js 文件的 Page 方法里包含很多事件函数，比如加载完成事件，刷新事件，分享事件等等。在微信开发者工具创建文件的时候，自动加入了很多函数。
- .js 文件数据初始化的地方是 data:{},要输出 hello world ，只需在 data:{} 里面初始化字符串，然后在视图里输出即可。
- .wxml 输出渲染数据的方式是双大括号 {{}} 里面写入参数,如下。

test.js

`
Page({
    data: {value:"hello world"}
})
`

test.wxml

`
<view>{{value}}</view>
`

---

# 加入样式
可用style属性写入内联样式，或引用外部样式。test.wxml 会自动引用 test.wxss 。

test.wxml

`
<view class="container">
  <view class="class-array" style="">{{value}}</view>
</view>
`

test.wxss
`
/* 引用样式文件 */
@import '../index/index.wxss';
.class-array {
  color:blueviolet;
}
`

# wxml 数据渲染

  - 循环：wx:for
  - 条件：wx.if
 
test.wxml
 
 `<view class="container">
 
    <!-- 数据绑定 -->
    <view class="class-array">{{value}}</view>
    <!-- 循环 -->
    <view class="class-array" wx:for="{{array}}">{{item}}</view>
    <!-- 条件 -->
    <view wx:if="{{view == 'WEBVIEW'}}"> WEBVIEW </view>
    <view wx:elif="{{view == 'APP'}}"> APP </view>
    <view wx:else="{{view == 'MINA'}}"> MINA </view>
    
  </view>
 `
 
 test.js
 
 `Page({
    data: {
      value: 'hello world',
      array:[1,2,3,4,5,6,7],
      view: "MINA",
    },
  })`
 
 # .js函数
 
onLoad 函数，监听页面加载
`Page({
   //页面的初始数据
   data: {
     value: 'hello world',
     array:[1,2,3,4,5,6,7],
   },
   //生命周期函数--监听页面加载
   onLoad: function (options) {
     console.log(this.data) //页面加载完成后，控制台输出页面的初始数据
   }
 })`
 
onShareAppMessage 函数，用户点击右上角分享
 `Page({
       onShareAppMessage: function (res) {
           if (res.form === 'button') {
             console.log(res.target)
           }
           //返回转发页面
           return {
             title: '转发页面',
             path: '/pages/logs/logs'
           }
       }
   })`
   
onTabItemTap 函数，点击底部tap时触发
`Page({
      onTabItemTap(item) {
          console.log(item.index) //tab序号，从0开始
          console.log(item.pagePath) //页面路径
          console.log(item.text) //tab名称
       }
   })`

onPageScroll 函数，滑动页面事件
`onPageScroll: function(res){
     // 打印出对象，含参数scrollTop,页面在垂直方向已滚动的距离（单位px）
     console.log(res);
   }`

onReachBottom 函数，页面上拉触底事件的处理函数
 `onReachBottom: function () {
      //上拉触底时输出
      console.log('onReachBottom')
    }`

onPullDownRefresh() 函数，监听用户下拉刷新事件。
需要在app.json的window选项中或页面配置中开启enablePullDownRefresh，也可设置下拉样式
`onPullDownRefresh: function () {
    //下拉刷新执行
     console.log("onPullDownRefresh")
   }`


onResize() 屏幕旋转函数
需要在 app.json 的 window 段中设置 "pageOrientation": "auto" 
`Page({
   onResize(res) {
     res.size.windowWidth // 新的显示区域宽度
     res.size.windowHeight // 新的显示区域高度
   }
 })
`

自定义事件
test.wxml
`<!-- 事件 -->
   <view id="tapTest" data-hi="weChat" bindtap="tapName">点击查看</view>`

test.js
`Page({
   tapName: function(event) {
       console.log(event);
     }
 })
`

引用模块
- 被引用的模块使用module.exports 或 exports 暴露接口
- 引用别的模块，在page之前调用被引用模块

common.js
`function sayHello(name) {
   console.log('Hello ${name}')
 }
 function sayGoodbye(name) {
   console.log('Goodbye ${name}')
 }
 //返回模块通过 module.exports 或 exports 暴露接口
 module.exports.sayHello = sayHello
 exports.sayGoodbye = sayGoodbye`

test.js
`var common = require('common.js')
 Page({
   //引入模块
   // var common = require('common.js') 放在Page之前
   helloMINA: function() {
     common.sayHello('MINA')
   },
   goodbyeMINA: function() {
     common.sayGoodbye('MINA')
   }
 })`
 
test.wxml
`<view bindtap="helloMINA">点击查看载入组件</view>`


 


