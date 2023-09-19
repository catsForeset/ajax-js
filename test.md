##### jmeter 使用

1、安装：官网下载安装包，解压（后续步骤同java jdk安装）；
2、设置环境：设置JMETER_HOME、path。cmd输入jmeter -v提示安装成功；
3、打开：cmd 输入jmeter 或者打开安装目录bin下找到jmeter.bat打开工具；
4、设置工具：properties设置language=zh_CN，或者工具里面options-choose language；



##### jmeter接口请求

1、testplan-右键添加-线程（用户）-线程组，修改名称。一个线程组代表一定的用户；
2、右键线程组-添加-取样器-http请求，填写接口请求数据；
3、右键http请求-添加-监听器-查看结果树；
4、运行http请求，在结果树查看结果，可在结果树的http请求上方切换json格式查看接口数据；
5、异常请求：复制上述正常http请求，粘贴命名为异常请求，验证接口异常情况；
6、运行所有接口：选择结果树，点击上方启动按钮，运行所有接口；



##### jmeter请求组

1、线程组右键-添加-配置元件-http请求默认值，填写协议、ip和端口号，作用于当前线程组的所有组件，若挪到http请求下方，则只作用于http请求（父组件），作用域下同；
2、线程组右键-添加-配置元件-http信息头管理，填写Conten_Type=application/json，统一设置信息头；
3、线程组右键-取样器-http请求，只用填写post/get和路径，其他沿用上述两个配置；



##### jmeter接口关联，将获取的token用于其他接口

###### 一、正则表达式提取器

1、查看结果树的登录接口更改数据查看为RegExp Tester，Regular expreesion-编写正则表达式-点击test，测试要获取的token值是否正确；

`"token":"(.*?)"`

2、登录接口右键-添加后置处理器-正则表达式提取器，随意添加引用名称（引用名称全局唯一），将步骤1测试成功的正则表达式填进去；

![微信图片_20230901151712](.\jmeter\微信图片_20230901151712.png)

3、线程组右键-添加取样器-调试取样器；
4、运行-查看结果树-查看；调试取样器结果；
5、查询用户接口右键添加-配置元件-http信息头管理，填写参数，获取步骤2的token；



###### 二、json提取器

1、查看结果树的登录接口更改数据查看为Json Path Tester，Regular expreesion-编写jsonpath语句-点击test，测试要获取的token值是否正确；

`$.token` 

多级数据,{}用.连接，[]用下标表示，如`$.token.data`[0] ,

2、登录接口右键-添加后置处理器-json提取器，随意添加引用名称，将步骤1测试成功的jsonpath语句填进去；

![微信图片_20230901153035](.\jmeter\微信图片_20230901153035.png)

3、线程组右键-添加取样器-调试取样器；
4、运行-查看结果树-查看；调试取样器结果；
5、查询用户接口右键添加-配置元件-http信息头管理，填写参数，获取步骤2的token；



###### 三、用json提取器获取id查询用户数据

1、登录接口右键-添加后置处理器-json提取器，填以下数据；

![json提取器获取id](.\jmeter\json提取器获取id.png)

2、put/get接口获取参数后查询

![image-20230904103004607](.\jmeter\image-20230904103004607.png)

2、post接口获取参数后查询

![post](.\jmeter\post.png)



##### jmeter传参类型

###### 一、参数

1、意义：实质上就是拼接在url参数后面的参数；

###### 二、消息体数据

1、意义：json

###### 三、文件上传

1、意义：有文件（文档、图片、视频等）上传的接口；

2、文件上传，比参数方式多写文件位置参数；



##### jmeter随机数/参数加密

###### 一、随机数

1、随机数：工具-函数助手对话框-选择Random-编辑参数-测试生成-拷贝并粘贴函数字符串

`${__Random(100000,999999,rm)}`

2、将步骤1的字符串拼接到用户注册接口的name参数中，使用户名不重复，添加成功

参数：`小花${__Random(100000,999999,rm)}`

json：`{"name":"小花${__Random(100000,999999,rm)}","password":"111111"}`

3、随机字符串__RandomString、随机时间戳同理

###### 二、参数加密

1、MD5加密：工具-函数助手对话框-选择digest-算法摘要填“md5”-填需要被加密的数据-测试生成-拷贝并粘贴函数字符串；

`${__digest(md5,111111,,,)`

2、用户登录接口，填写参数；

参数：`${__digest(md5,111111,,,)}`

json：`{"name":"小花","password":"${__digest(md5,111111,,,)}"}`

3、SHA-1、base64同理；

4、其他未自带的加密方式，用代码方式写BeanShell语言；



##### jmeter断言

###### 一、响应断言

1、用户登录接口添加-断言-响应断言；

2、在响应断言的测试模式下方输入想要断言的数据，比如输入success，如果响应数据中有这个数据（默认是包含模式），则断言成功（无任何提示），否则断言失败（有失败提示）；

###### 二、json断言

1、用户登录接口添加-断言-json断言；

2、在json断言下输入jsonpath表达式，获取想要断言的字段，写入字段的预期参数，如果接口返回的数据有该字段且参数一直，则断言成功（无任何提示），否则断言失败（有失败提示）；

![微信截图_20230907144503](.\jmeter\微信截图_20230907144503.png)

3、BenShell断言同理，需要BeanShell代码来实现；



##### CSV参数化-测正反例

1、新建CSV线程组，将登录接口已经请求头等控件复制到此线程组；登录接口添加-配置元件-CSV 数据文件设置，删除其他断言；

2、添加CSV文件数据，填参数，保存为.csv文件。name和password是请求参数，code是响应参数之一，登录成功则code为111，登录失败则code为222；

`name,password,code
小花,111111,111
,111111,222
小花,,222`

3、CSV 数据文件设置-添加文件路径及其他参数，一旦引入csv文件成功，里面的参数name、password、code则成为全局变量，获取方式同全局变量参数，$.参数名；

![微信图片_20230908091921](.\jmeter\微信图片_20230908091921.png)

4、登录接口，参数改成上述变量，获取方式为 $.参数名；

```
{"name":"$.{name}","password":"$.{password}"}
```

5、线程组添加-逻辑控制器-循环控制器，将登录接口拉到循环控制器下方，设置循环次数为3（csv文件写了3组数据）；

6、登录接口添加json断言；

![微信图片_20230908094722](.\jmeter\微信图片_20230908094722.png)

7、运行登录接口，接口运行成功，断言成功则无断言数据，断言失败在结果树的登录接口下方有断言数据；



##### jmeter缓存/Cookie鉴权

###### 一、Http Cookie管理器实现Cookie关联的原理

1、当Jmeter第一次请求服务器的时候，如果说服务器有通过响应头的Set-Cookie有返回Cookie，那么Http Cookie管理器就会自动的保存这些Cookie的值；

2、然后当Jmeter第2-N次请求服务器的时候，那么Http Cookie管理器就会自动的把保存的这些Cookie运用于这些请求，从而实现Cookie关联；

3、若一组接口互相有cookie关联，使用Http Cookie管理器，如果不清楚接口间是否有cookie关联，线程组建议加上Http Cookie管理器，防止报错；



##### 用命令行运行测试计划

1、jmeter 页面保存上述用户登录测试计划，复制并重命名为test.jmx（D:\apache-ant-1.9.16\ant\test.jmx）；

2、生成日志文件：在test.jmx 所在目录cmd 输入以下命令：

`jmeter -n -t test.jmx` 生成文件jmeter.log，没多大用处

3、生成jtl测试报告：

`jmeter -n -t test.jmx -l result.jtl`   生成文件result.jtl，没多大用处

4、生成html测试报告，在当前目录添加文件夹results

`jmeter -n -t test.jmx -l result.jtl -e -o results`  在results文件夹生成多个文件，打开index.html，可查看到测试报告；

若报错则在jmeter.properties设置：`jmeter.save.saveservice.output_format=csv`



##### ant插件生成测试报告

1、下载ant插件：官网（https://ant.apache.org/）下载，解压，添加环境变量path D:\apache-ant-1.9.16\bin，cmd 命令行 ant -version，有输出ant版本则安装成功；（设置build.xml：cmd 输入ant -v，提示build.xml有问题）

2、在test.jmx所在目录配置build.xml文件，build.xml文件见jmeter文档；

3、jmeter.properties设置：

`jmeter.save.saveservice.output_format=xml`

4、执行：确保有到test.jmx和build.xml在同一个目录下，当前目录打开cmd，运行：

`ant` 

5、ant运行成功后生成html、jtl、report三个文件夹，html里面打开html文档，是总体测试报告，report报告是详细的报告；

6、如果没有找到文件jmeter.results.shanhe.me.xsl，可以查看附件jmeter文档，或去下载：https://github.com/baozhida/jmeter.bak/blob/master/jmeter.results.shanhe.me.xsl，放到D:\apache-jmeter-5.6.2\extras里面；



#### postman

##### 测试理论

1、参数传递正确性、功能正确性；

2、异常情况的容错能力；

3、接口权限、兼容性；

4、正例：正常入参，返回成功

5、反例：鉴权、参数、错误码（有多少错误码就有多少测试用例）、黑名单、接口限制次数、分页、兼容性等；



##### 测试执行

###### postman组件（从上往下）

一、参数

Params:get请求传参
Authorization:鉴权
Headers:请求头
	accept:客户端接收的数据类型
	content-type:客户端发送给服务器的数据类型
	User-agent:客户端的类型
	xmlhttprequest:异步请求
	......
Body: post请求传参
	none:没有参数
	form-data:文件上传 (包含键值对和文件上传)
	x-www-from-urlencodeed:表单请求(键值对)
	raw:使用原始数据格式请求 (JSON，XML，HTML，Text，Javascript)
	binary:二进制文件上传。
Pre-request Script: 请求之前的脚本。
Tests:请求之后的脚本。
Settings:设置
Cookies:postman用于自动管理Cookie的功能

二、响应

Body: 返回的值
Pretty:以不同的格式查看返回结果
Raw:以文本格式查看返回结果
Preview:以网页格式查看返回结果Cookies:响应的CookieHeaders: 响应头TestResults:断言的结果

三、调试









Locust 使用
1、安装python；
2、cmd 安装：pip3 install Locust（详细步骤：https://docs.locust.io/en/stable/installation.html#installation）；
3、参照官网写一个demo locu.py，cmd 运行：locust -f locu.py；
4、根据运行的提示，访问：http://localhost:8089/；