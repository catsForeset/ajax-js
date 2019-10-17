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
 
 # js函数
 
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
 
onHide 函数，监听页面隐藏
当用户退出或者离开当前页面，等于做了页面隐藏操作，触发 onHide 函数。
用真机调试模式可测试当前页面退出小程序时的 onHide 函数。
`Page({
     onHide: function () {
         console.log("用户退出/离开当前页面")
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
   
onTabItemTap 函数，点击底部 tap 时触发
`Page({
      onTabItemTap(item) {
          console.log(item.index) //tab序号，从0开始
          console.log(item.pagePath) //页面路径
          console.log(item.text) //tab名称
       }
   })`

onPageScroll 函数，滑动页面事件
`Page({
     onPageScroll: function(res){
          // 打印出对象，含参数scrollTop,页面在垂直方向已滚动的距离（单位px）
          console.log(res);
     }
 })`

onReachBottom 函数，页面上拉触底事件的处理函数
 `Page({
      onReachBottom: function () {
            //上拉触底时输出
            console.log('onReachBottom')
      }
  })`

onPullDownRefresh() 函数，监听用户下拉刷新事件。
需要在 app.json 的 window 选项中或页面配置中开启 enablePullDownRefresh，也可设置下拉样式
`page({
     onPullDownRefresh: function () {
         //下拉刷新执行
          console.log("onPullDownRefresh")
     }
 })`

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
- 被引用的模块使用 module.exports 或 exports 暴露接口
- js 引用别的模块，在 page 之前调用被引用模块

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

# 表单
表单内常包括：button 按钮，input 输入框，textarea 多行输入框，radio 单选项，checkbox 多选项，switch 开关选择器，slider 滑块，picker 底部弹起滚动选择器，label 标签，editor 富文本编辑器

test.wxml
 `<form bindsubmit="formSubmit" bindreset="formReset">
       <view class="section section_gap">
         <view class="section_title">switch</view>
         <!-- 开关选择器 switch -->
         <switch name="switch"></switch>
       </view>
       <view class="section section_gap">
         <view class="section_title">slider</view>
         <!-- 滑块 slider -->
         <slider name="slider" show-value></slider>
       </view>
       <view class="section">
         <view class="section_title">input</view>
         <!-- 输入框 input -->
         <input name="input" placeholder="please input here" type="text" confirm-type="done"></input>
       </view>
       <view class="section section_gap">
         <view class="section_title">radio</view>
         <!-- 单选项 radio  -->
         <radio-group name="radio group">
           <label><radio value="radio1"></radio>radio1</label>
           <label><radio value="radio2"></radio>radio2</label>
         </radio-group>
       </view>
       <view class="section section_gap">
         <view class="section_title">checkbox</view>
         <!-- 多选项 checkbox -->
         <checkbox-group name="checkbox">
           <label><checkbox value="checkbox1" />checkbox1</label>
           <label><checkbox value="checkbox2" />checkbox2</label>
         </checkbox-group>
       </view>
       <view class="btn-area">
         <!-- 按钮 button -->
         <button form-type="submit">Submit</button>
         <button form-type="reset">Reset</button>
       </view>
     </form>`
   
test.js
`Page({
     formSubmit: function(e) {
         console.log(e)
         console.log('表单数据：',e.detail.value)
      }
 })`
 
多行输入框 textarea

`<view class="section">
     <textarea bindblur="bindTextAraeBlur" auto-height placeholder="自动变高"></textarea>
   </view>
   <view class="section">
     <textarea placeholder="placeholder颜色是红色的" placeholder-style="color:red"></textarea>
   </view>
   <view class="section">
     <textarea placeholder="自动聚焦" focus="true"></textarea>
   </view>`
   
底部弹起选择器 piker
test.wxml
`<view class="divLine"></view>
   <view class="section">
     <view class="section_title">普通选择器</view>
     <!-- range表示选择范围，index表示选择了range的第几个，index从0开始 -->
     <picker bindchange="bindPickerChange" value="{{index}}" range="{{array}}">
       <view class="picker">
         当前选择：{{array[index]}}
       </view>
     </picker>
   </view>
   <view class="section">
     <view class="section_title">多列选择器</view>
     <!-- 选择器的mode属性定义了选择器类型，默认是普通选择器selector -->
     <picker mode="multiSelector" bindchange="bindMultiPickerChange" value="{{multiIndex}}" range="{{multiArray}}">
       <view class="picker">
         当前选择：{{multiArray[0][multiIndex[0]]}},{{multiArray[1][multiIndex[1]]}},{{multiArray[2][multiIndex[2]]}}
       </view>
     </picker>
   </view>
   <view class="section">
     <view class="section_title">日期选择器</view>
     <!-- start 和 end 控制日期可选范围，其他日期段显示但是不可选,value/start/end 的格式为YYYY-MM-DD-->
     <picker mode="date" bindchange="bindDateChange" value="{{date}}" start="2015-01-01" end="2020-01-01">
       <view class="picker">
         当前选择：{{date}}
       </view>
     </picker>
   </view>
   <view class="section">
     <view class="section_title">时间选择器</view>
     <!-- value/start/end 的格式为hh:mm -->
     <picker mode="time" bindchange="bindTimeChange" value="{{time}}" start="00:00" end="23:59">
       <view class="picker">
         当前选择：{{time}}
       </view>
     </picker>
   </view>
   <view class="section">
     <view class="section_title">多列选择器</view>
     <!-- 自动加载全国的省市区,，选择后返回一个包含省市区的一维数组。custom-item 可为省市区的顶部添加一个自定义项 -->
     <picker mode="region" bindchange="bindRegionChange" value="{{region}}" custom-item="{{customItem}}">
       <view class="picker">
         当前选择：{{region[0]}},{{region[1]}},{{region[2]}}
       </view>
     </picker>
   </view>`
   
test.js
` Page({
     data:{
         array:[1,2,3,4,5,6,7],
         index:0,
         multiIndex:[0,0,0],
         multiArray:[['无脊柱动物','脊柱动物'],['扁性动物','线性动物','环节动物','软体动物','节肢动物'],['猪肉涤虫','吸血虫']],
         date: '2019-01-01',
         time: '09:00',
         region: ['广东省','广州市','海珠区'],
     }
  })`
 
 
# 一些视图组件
icon 图标
只需一个 icon 标签即可，type 属性控制icon类型
`<icon type="success" size="10" color="red"></icon>`

text 文本和 rich-text 富文本
test.wxml
`<text>{{text}}</text>
<rich-text nodes="{{nodes}}" bindtap="bindRichText"></rich-text>`

test.js
`Page({
     data: {
         text: 'This is text'
         nodes:[{
           name: 'div',
           attrs: {
             class: 'div-class',
             style: 'line-height:60px; color:red;'
           },
           children: [{
             type: 'text',
             text: 'Hello World!'
           }]
         }],
     }
 })`

navigator 导航
`<view class="btn-area">
     <navigator url="/pages/navigate/navigate" hover-class="navigator-hover" open-type="navigate">跳转到新页面</navigator>
     <!-- 如果一个页面在app.json设置成为导航页面，那么它有效的切换方式只有 switchTab -->
     <navigator url="/pages/index/index" hover-class="navigator-hover" open-type="switchTab">切换 Tab </navigator>
  </view>`

progress 进度条
`<!-- active 从页面加载完成后加载进度条动画 -->
 <progress percent="50" show-info activeColor="red" active="true" />
 <progress percent="40" stroke-width="12"/>`

image 图片缩放和剪裁
用 mode 属性控制图片的缩放和剪裁模式
`<image style="width:200px;height:200px;background-color:#eee" mode="scaleToFill" src="../resources/images/timg2.jpg"></image>`

map 地图
makers 是地图标志点。polyline 是地图路线。当地图视野发生变化（拖动、缩放），bindregionchange 会返回变化的相关参数。
test.wxml
`<map id="map" longitude="120.21201" latitude="30.2084" markers="{{markers}}" polyline="{{polyline}}" bindmarkertap="markertap" bindregionchange="regionchange"></map>`

test.js
`Page({
     markers: [{
       iconPath:'../resources/images/timg3.jpg',
       id:0,
       longitude: 120.21201,
       latitude: 30.2084,
       width:30,
       height:40
     }],
     polyline: [{
       points: [{
         longitude: 120.21201,
         latitude: 30.2084,
       },{
           longitude: 120.51201,
           latitude: 30.5084,
       }],
       color: "#FF0000DD",
       width: 2,
       dottedLine: true
     }]
 })`
 
 # API
 ## 基础api
 ### wx.getSystemInfo() 和 wx.getSystemInfoSync()
 获取系统信息，包括用户手机型号、屏幕长宽、微信版本、用户对微信使用手机功能的权限设置（如摄像机、通讯录等）等
 
 api.js
`onLoad: function (options) {
     wx.getSystemInfo({
       success: function(res) {
         console.log(res)
       },
     })
     try{
       const res = wx.getSystemInfoSync()
       console.log(res)
     } catch (e) {
       console.log('catch error')
     }
 }`
 
### wx.getUpdateManager()
检查小程序更新和查看更新结果

api.js
`onLoad: function (options) {
     const updateManager = wx.getUpdateManager()
     console.log(updateManager)
     updateManager.onCheckForUpdate(function (res) {
       console.log(res)
     })
 }
`

### wx.setEnableDebug() 和 console
打开、关闭调试和打印日志或报错

api.js
`onLoad: function (options) {
 wx.setEnableDebug({
   //打开/关闭调试
   enableDebug: true,//false
 })
 console.info('info log')
 //输出错误日志，相当于一条 error 报错
 console.error('error log')
 console.info('warn log')`


### setTimeout() 和 setInterval()
定时器

api.js
`onLoad: function (options) {
     var that = this
     //interval 每隔一个时间周期执行一次回调函数
     var interval = setInterval(function() {
       //在定时器使用 this ，会返回 windows 对象
       that.setData({
         timeOut: that.data.timeOut + 1
       })
     },1000)
     //timeout 设定一个定时器，到期执行回调函数
     var timeout = setTimeout(function() {
       that.setData({
         timeOut: 10000
       })
       clearInterval(interval)
     },10000)
     //取消定时器
     // clearTimeout(timeout)
 }`


### 路由
路由，对应视图 navigator 组件的 open-type 属性

api.js
`pages({
   bindRoute: function() {
     //跳转到 tabBar 页面，并关闭其他所有非 tabBar 页面
     // wx.switchTab({
     //   url: '/pages/index/index',
     // })
     //关闭所有页面，打开到应用内的某个页面
     // wx.reLaunch({
     //   url: '/pages/index/index',
     // })
     //关闭当前页面，跳转到应用内的某个页面。不允许跳转到 tabbar
     // wx.redirectTo({
     //   url: '/pages/navigate/navigate',
     // })
     // 保留当前页面，跳转到应用内的某个页面。但是不能跳到 tabbar 页面。
     wx.navigateTo({
       url: '/pages/navigate/navigate',
     })
   }
 })`

api.wxml
`<view bindtap="bindRoute">点击跳转</view>`

navigate.js
`bindBack: function() {
   //返回上级页面
   wx.navigateBack({
     delta: 3 //返回的页面数
   })
 }`
 
navigate.wxml
`<view bindtap="bindBack">返回上级页面</view>`

### 界面
### wx.showToast() 和 wx.hideToast()
显示和关闭消息提示框

api.js
`onLoad: function (options) {
     wx.showToast({
       title: '成功',
       icon: 'success',
       duration: 2000
     })
     setTimeout(function () {
       wx.hideToast()
     }, 1000)
 }`
 
### wx.showLoading() 和 wx.hideLoading()
显示和关闭 loading 加载框

api.js
` onLoad: function (options) {
  wx.showLoading({
     title: '加载中',
   })
   setTimeout(function () {
     wx.hideLoading()
   },2000)
}`

### wx.showActionSheet()
操作菜单,从底部弹出选项。返回选择的数组的 index .

api.js
`onLoad: function (options) {
     wx.showActionSheet({
       itemList: ['A', 'B', 'C'],
       success (res) {
         console.log(res)
       },
       fail (res) {
         console.log(res.errMsg)
       }
     })
 }`
### wx.showModal() 
模拟对话框,即让用户有“是”和“否”选择的弹框。不同的选择对应不同的回调函数
api.js
`onLoad: function (options) {
     //真机模式调试之后，is not a function 报错消失
     wx.showModal({
       title: '提示',
       content: '这是一个模拟对话框',
       success (res) {
         if(res.confirm) {
           console.log('用户点击确定')
         } else if (res.cancel) {
           console.log('用户点击取消')
         }
       }
     })
 }`

### 顶部导航栏 navigation 和下拉刷新
app.json 的 windows 参数设置全局的导航栏。页面导航栏控制当前页面导航栏的显示。

api.js   
`//导航栏加载动画,控制页面加载时导航栏的 loading 标识显示时长
onLoad: function (options) {
     wx.showNavigationBarLoading()
     setTimeout(function () {
       wx.hideNavigationBarLoading({
         success (res) {
           console.log(res)
         },
         complete (res) {
           console.log(res)
         },
         fail (res) {
           console.log(res)
         }
       })
     },2000)
     //设置导航栏标题
     wx.setNavigationBarTitle({
       title: '动态标题',
     })
     //设置导航栏颜色
     wx.setNavigationBarColor({
       frontColor: '#ffffff',
       backgroundColor: '#0000ff',
       animation: {
         duration: 400,
         timingFunc: 'easeIn'
       }
     })
     //隐藏返回首页按钮
     //真机调试模式，在非主页的页面，导航栏左边会有返回主页按钮
     //在ide调试 hideHomeButton 报错，真机模式调试则不会报错
     wx.hideHomeButton({
       success (res) {
         console.log(res)
       },
       fail (res) {
         console.log(res)
       }
      })
 }`

### wx.setBackgroundTextStyle()

api.js
`onLoad: function (options) {
 //设置下拉背景字体、loading 样式
     //在ide环境下会报错，真机的环境下不会报错
     wx.setBackgroundTextStyle({
       textStyle: 'dark',
       complete (res) {
         console.log(res)
       }
     })
 }`

### 底部导航栏tabBar


## 动画
### 动画使用
官方定义：创建一个动画实例 animation。调用实例的方法来描述动画。最后通过动画实例的 export 方法导出动画数据传递给组件的 animation 属性。
用代码来拆分一下这个定义，就能理解动画的用法。

api.js
`Pages({
   start: function () {
     //创建一个动画实例 animation。
     var animation = wx.createAnimation({
       duration: 4000,
       timingFunction: 'ease',
       delay: 1000
     })
     //调用实例的方法 animation.opacity() 和 animation.translate() 来描述动画。step() 表示一组动画的完成，完成一组动画内所有动画，下一组动画才会开始。
     animation.opacity(0.2).translate(100,-100).step()
     this.setData({
       //通过动画实例的 export 方法导出动画数据 animate
       animate: animation.export()
     })
   }
 })`

api.wxml
`<view class="container">
    <!-- 动画数据传递给组件的 animation 属性 -->
   <view class="animation-view" animation="{{animate}}">
     <text>animation</text>
   </view>
 <button bindtap="start">开始</button></view>`
 
api.xwss
`.animation-view{
   background-color: pink;
   width: 50%;
 }`

 
## 动画方法
动画实例的方法主要有：设置透明度、宽高度，平移、旋转、缩放，等等。

api.js
`Pages({
   start: function () {
     var animation = wx.createAnimation({
       duration: 1000,
       timingFunction: 'ease',
       delay: 1000
     })
     //透明度为0.5
     animation.opacity(0.5).step()
     //宽高度变化
     animation.height(100).width(100).step()
     //向x轴平移和y轴平移
     animation.translate(100,-100).step()
     //顺时针旋转并缩放为原来的两倍
     animation.rotate(90).scale(2).step()
     this.setData({
       animate: animation.export()
     })
   }
 })`
 
 
## 网络请求

### wx.request()
request 请求类似于 ajax 请求，只支持 https 请求，并且请求的域名必须设置成小程序后台的 request 参数。微信开发者工具设置不校验 https 则可请求成功。

api.js
`Pages({
 bindRequest: function() {
     wx.request({
       url: 'http://www.mydomain.com/test.php',
       data: {
         x: 'data x',
         y: 'data y'
       },
       header: {
       },
       success (res) {
         console.log(res)
         console.log(res.data)
       }
     })
   }
 })`

api.wxml
`<button bindtap="bindRequest">请求数据</button>`

### wx.requestTask()
同 wx.request() 类似，拥有实例方法比如 requestTask.abort()。
api.js
`Pages({
 bindRequest: function() {
     const requestTask = wx.request({
       url: 'http://www.mydomain.com/test.php',
       data: {
         x: 'data x',
         y: 'data y'
       },
       header: {
         'content-type': 'application/json'
       },
       success(res) {
         console.log(res)
         console.log(res.data)
       }
     })
     requestTask.abort()//取消请求任务
   }
 })`
 
### wx.uploadFile() 和 wx.downloadFile()
上传和下载请求


 
 
    






