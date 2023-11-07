#### jmeter

##### 安装

1、安装：官网下载安装包，解压（后续步骤同java jdk安装）；
2、设置环境：设置JMETER_HOME、path。cmd输入jmeter -v提示安装成功；
3、打开：cmd 输入jmeter 或者打开安装目录bin下找到jmeter.bat打开工具；
4、设置工具：properties设置language=zh_CN，或者工具里面options-choose language；



##### 接口请求

1、testplan-右键添加-线程（用户）-线程组，修改名称。一个线程组代表一定的用户；
2、右键线程组-添加-取样器-http请求，填写接口请求数据；
3、右键http请求-添加-监听器-查看结果树；
4、运行http请求，在结果树查看结果，可在结果树的http请求上方切换json格式查看接口数据；
5、异常请求：复制上述正常http请求，粘贴命名为异常请求，验证接口异常情况；
6、运行所有接口：选择结果树，点击上方启动按钮，运行所有接口；



##### 请求组

1、线程组右键-添加-配置元件-http请求默认值，填写协议、ip和端口号，作用于当前线程组的所有组件，若挪到http请求下方，则只作用于http请求（父组件），作用域下同；
2、线程组右键-添加-配置元件-http信息头管理，填写Conten_Type=application/json，统一设置信息头；
3、线程组右键-取样器-http请求，只用填写post/get和路径，其他沿用上述两个配置；



##### 接口关联，将获取的token用于其他接口

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



##### 传参类型

###### 一、参数

1、意义：实质上就是拼接在url参数后面的参数；

###### 二、消息体数据

1、意义：json

###### 三、文件上传

1、意义：有文件（文档、图片、视频等）上传的接口；

2、文件上传，比参数方式多写文件位置参数；



##### 随机数/参数加密

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



##### 断言

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



##### 缓存/Cookie鉴权

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



##### 接口说明

1、获取 access_token，应用与其他接口；

2、接口类型：获取token，用户的增、删、改、查，用户名不能重复；有cookie应用；



##### postman组件（从上往下）

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



##### 接口关联

###### 一、理论：在登录接口获取token，复制下来放到查询接口

1、方式一：将token拼接到地址里

如：http://localhost:8000/api/auth/me?token=tokenvalue

2、方式二：将token放到headers里

如：`Authorization:Bearer tokenvalue`，其中Bearer是token的一种形式，一定要加；

###### 二、在获取token的接口获取参数，设置全局变量；

1、json方式

1）在登录接口的后置项 Tests写脚本，在postman的底部Console面板查看打印的测试语句，设置token为全局变量，代码如下：

*console.**log**('test'); //测试语句*
*console.**log**(responseBody); //响应数据*
*var jsobj **=** JSON.**parse**(responseBody); //响应数据转json*
*console.**log**(jsobj.access_token); //获取指定token值pm.globals.**set**('token',jsobj.access_token); //将获取的token值设为全局变量*

2）将登录设置的全局变量应用到查询接口

http://localhost:8000/api/auth/me?token={{token}}

或 `Authorization:Bearer {{token}}`

2、正则表达式方式

1）在登录接口的后置项 Tests写脚本，在postman的底部Console面板查看打印的测试语句，设置token为全局变量，代码如下：

console.**log**(responseBody); //响应数据
var datas = responseBody.match(new RegExp('"access_token":"(.*?)"')); //匹配正则表达式
console.**log**(datas[1]);
pm.globals.set('tokens',datas[1]); //将获取的token值设为全局变量

2）将登录设置的全局变量应用到查询接口

http://localhost:8000/api/auth/me?token={{tokens}}

或 `Authorization:Bearer {{token}}`



##### 环境变量和全局变量

1、设置环境变量，变量名为ip，设置一个localhost，一个127.0.0.1（理论上是设置一个测试环境一个预发环境）;

![微信图片_20230926141820](E:\my_workplace\ajax-js\jmeter\微信图片_20230926141820.png)

2、在所有接口用{{}}代替不同环境的地址；

如：http://{{ip}}:8000/api/auth/login

3、选择不同的环境；

![微信图片编辑_20230926142341](E:\my_workplace\ajax-js\jmeter\微信图片编辑_20230926142341.jpg)



##### 动态参数

###### 一、系统自带

1、{{$timestamp}} 动态时间戳

2、{{$randomInt}} 动态0-1000整型数据

3、{{$guid}} 动态guid

4、创建用户接口请求参数：

*{"name":"aaaaa{{$randomInt}}","email":"aaa@mm.com","password":"111111","created_at":{{$timestamp}}}*

###### 二、自定义动态参数

从其他接口获取响应参数作为请求参数



##### *断言，Test标签

###### 一、参数

Status code: Code is 200
Response body: Contains string
Response body: JSON value check
Response body: ls equal to a string
Response headers: Content-Type header check
Response time is less than 200ms
Stalus code: Successful POST request
Status code: Code name has string
检查返回的状态码是否为200（常用）
检查返回的数据中包括有指定的字符串（常用）
检查json中的其中一个字段的值（常用）
检查返回的值等于一个指定的字符串（常用）
检查是否包含有content-type响应头
检查请求的时间少于200MS
检查返回的状态码是否在数组中
检查状态信息是指定的字符串。

###### 二、断言响应码200

1、点击 Status code: Code is 200 ，自动出现相应的代码，其余方式类似：

*pm.**test**("Status code is 200", **function** () {*
  *pm.response.to.have.status(200);*
*});*

可修改文字如下：

*pm.**test**("断言响应码为200", **function** () {*
  *pm.response.to.have.status(200);*
*});*

2、send 执行接口，在Test Results 面板里可查看到断言结果，通过则为PASS，不通过则为FAIL：

![微信图片_20230927141211](E:\my_workplace\ajax-js\jmeter\微信图片_20230927141211.png)

###### 三、断言响应字符串

*pm.**test**("包含字段access_token", **function** () {*
  *pm.expect(pm.response.text()).to.include("access_token");*
*});*

###### 四、断言JSON某字段的值

*pm.**test**("检查json中某字段的值", **function** () {*
  *var jsonData **=** pm.response.json();*
  *pm.expect(jsonData.expires_in).to.eql(3600);*
*});*

###### 五、断言是否返回指定字符串

创建用户接口断言成功，其成功返回结果是如下指定的json字符串，当接口返回参数比较少且变化不大时，用这种方式

*pm.**test**("Body is correct", **function** () {*
  *pm.response.to.have.body('{"code":111,"msg":"success"}');*
*});*

###### 六、检查是否有响应头Content-Type

*pm.**test**("检查是否有响应头Content-Type", **function** () {*
  *pm.response.to.have.header("Content-Type");*
*});*

###### 七、响应时间是否小于指定值

*pm.**test**("响应时间是否小于200ms", **function** () {*
  *pm.expect(pm.response.responseTime).to.be.below(200);*
*});*

###### 八、检查返回的状态码是否在数组中

*pm.**test**("状态码是否在数组[201, 202]中", **function** () {*
  *pm.expect(pm.response.code).to.be.oneOf([201, 202]);*
*});*

###### 九、检查状态信息是指定的字符串

*pm.**test**("Status code name has string", **function** () {*
  *pm.response.to.have.status("ok");*
*});*



##### 批量运行

1、项目-Run collection-选择要批量运行的接口，填写执行次数，填写间隔时间，运行

![微信截图_20230927162801](E:\my_workplace\ajax-js\jmeter\微信截图_20230927162801.png)

2、文件上传接口批量运行时会报错，要把上传的文件放在postman默认上传文件夹并且允许操作该文件夹，才能批量上传成功；默认文件夹在设置-settings-general-location里查看，打开Read files outside working directory允许操作文件夹；



##### CSV文件断言请求参数

1、创建一个csv或json文件，用excel或notpad打开，写一个正例两个反例（有文档）；

2、登录接口断言如下

*pm.**test**("包含字段access_token", **function** () {*
  *pm.expect(pm.response.text()).to.include("access_token");*
*});*

3、修改登录接口的参数为全局参数

*{"email":"{{email}}","password":"{{password}}"}*

4、项目-Run collection-选择要批量运行的接口，填写执行次数，填写间隔时间，选择csv或json文件；

![微信图片_20231009112645](E:\my_workplace\ajax-js\jmeter\微信图片_20231009112645.png)

5、如果正常运行的话，三组断言，通过一个失败两个；



##### CSV文件断言返回数据

1、在csv文件加接口返回参数"error";

2、登录接口断言处取全局变量，验证返回参数包含的字符；

pm.test("Body matches string", function () {
  pm.expect(pm.response.text()).to.include(data.error)
});

3、项目-Run collection-选择要批量运行的接口，填写执行次数，填写间隔时间，选择csv文件；

4、csv文件有三组数据，都pass就执行成功；



##### 根据返回结果提取数据

1、原则：若返回成功则提取token，失败则不提取；

2、登录接口的断言如下：

if(responseBody.access_token != -1){
  var datas = responseBody.match(new RegExp('"access_token":"(.?)"')); //匹配正则表达式
  pm.globals.set('tokens',datas[1]); 
}

3、postman右上角Environment quick look，将全局变量tokens删除；

4、运行登录接口（获取tokens并设置全局变量），再运行查询接口/查看全局变量看是否成功获取tokens，成功获取则正常；



##### 必须带请求头的接口

1、有些接口要求必须带请求头，在postman可以设置；

2、在请求的url下方的Headers里面填入请求头键值对即可；



##### cookie鉴权

1、postman会自动处理cookie；

2、postman请求地址下方的cookie里面可以查看和编辑cookie；

3、删除cookie后重新请求新cookie，获取最新数据；



##### mock测试

1、作用：当前后端分离，前端可以调用mock数据来代替还未完成的后端接口数据；当调用第三方接口时，第三方接口一般只返回正常数据，可以用mock数据来模拟错误数据；展示demo时，可以用mock数据来实现简单的动态流程；

2、postman创建一个new collection，点击右边三个点选择mock collection，创建mock 服务器，创建完成之后，在postman左边栏出现mock servers 选项；

![微信图片_20231019154918](E:\my_workplace\ajax-js\jmeter\微信图片_20231019154918.png)

3、删除上述testmock，在mock servers再创建如下：

![微信图片_20231019160927](E:\my_workplace\ajax-js\jmeter\微信图片_20231019160927.png)

![微信图片_20231019161024](E:\my_workplace\ajax-js\jmeter\微信图片_20231019161024.png)

4、上述操作后，在左边栏的collection里面自动生成一个testmock，里面包含一个test接口，请求接口，返回test接口编辑的参数；

![微信截图_20231019161412](E:\my_workplace\ajax-js\jmeter\微信截图_20231019161412.png)

5、在浏览器访问test参数，可用postman生成的地址和接口地址拼起来请求；如：https://933904ae-a9ea-4da7-a4d9-4d97a8c3f52c.mock.pstmn.io/test

6、改写test接口参数：test接口右键，选择add example，编辑返回结果，同理也可以编辑请求参数，如下：

![微信截图_20231019162150](E:\my_workplace\ajax-js\jmeter\微信截图_20231019162150.png)

7、接口请求结果更新为上述编辑结果，则操作成功；

8、postman的mockserver每天有访问次数限制，可打通flask 的mock server 来无限访问；



##### 加密解密

1、接口需要参数base64加密后再大写；

2、写前置脚本：postman  Pre-request 处理参数如下：
var us = CryptoJS.enc.Utf8.parse("admin") ;
var pw = CryptoJS.enc.Utf8.parse("123");
//对转换后的值做Base64加密
var bs64_us = CryptoJS.enc.Base64.stringify(us);
var bs64_pw = CryptoJS.enc.Base64.stringify(pw) ;
//设置为全局变量
pm.globals.set("bs64_us",bs64_us.toString().toUpperCase());
pm.globals.set("bs64_pw",bs64_pw.toString().toUpperCase());



使用newman

1、安装nodejs，官网一键安装，查看版本

node -v
npm -v

2、安装newman，用以下命令安装：

npm install -g newman

安装完毕后，查看是否安装成功

newman -v

3、postman调试好接口，导出用例：用例右边操作按钮，选择export，默认导出，命名为test_postman_collection.json

4、导出postman环境变量；

5、导出postman全局变量；

6、cmd命令行执行以上脚本；



#### pytest

##### 安装

1、安装python，cmd输入python成功，quit()退出；

2、cmd安装pytest：pip install pytest，安装完成后检查：pytest --version；



##### 创建项目

1、创建一个python项目，在项目根目录创建一个文件requirements.txt，写入以下内容：
pytest
pytest-html
pytest-xdist
pytest-ordering
pytest-rerunfailures
allure-pytest
pyyaml
requests

2、项目的命令行运行以下语句，加入插件：pip install -r requirements.txt，若不成功，多试几次，有时候外网网速慢，会有中断的情况。成功后再python的虚拟环境venv里面可以看到对应的插件；



##### 添加实例

1、规则：模块名必须以test-开头或以-test 结尾，测试类必须以Test开头且不能有init方法，测试类必须以test-开头(-代替底杠)；

2、项目根目录创建一个Python Package，命名testcases，在包里创建一个用例test_print，用来测试输出一些参数；

```python
class TestPrint:
    def test_one(self):
        print("this is a test string")
```

##### 执行实例

###### 方式一：命令行

pythoncharm（工具）里面，命令行输入并执行：pytest；添加其他参数可以执行不同效果；
pytest -v  输出详细信息，包括包名类名等；
-s 打印信息；
-vs 输出详细信息和打印信息；
-n=2，多线程，可填其他数字，多线程比单线程用时短，可以用多个方法，sleep来验证（先安装pytest-xdist）
-x ，出现一个用例失败则停止测试
--returns=2,失败重跑（先安装插件pytest-rerunfailures）
--maxfail=2，初心几个失败则停止测试
--html，生成html的测试报告（先安装插件pytest-html），会在当前根目录生成一个html文件，可以指定文件夹，如pytest -vs --html ./reports/result.html
-k，指定执行哪个测试用例，如：pytest -vs -k "test_one",pytest -vs -k "test_one or two"

###### 方式二：main()方法

1、项目根目录下创建一个文件run.py

```python
import pytest

if __name__ == '__main__':
    pytest.main(["-vs"])
```

2、在工具里面，上述文件右键-run运行，最终执行的是指定文件testcases\test_print.py；



###### 方式三：全局文件

1、项目根目录创建文件pytest.ini，名字不能更改，此文件可以更改测试用例规则、命令；

pytest.ini

```python
[pytest]
#参数
addopts = -vs -m "smoke"
#标记
markers=
    smoke:冒烟测试
```

test_print.py

```python
@pytest.mark.smoke
def test_smoke(self):
    print("smoke")
```

##### 跳过实例

1、无理由跳过

pytest.ini

```python
[pytest]
#参数
addopts = -vs
```

test_print.py

```python
@pytest.mark.skip(reason = "无理由跳过")
def test_one(self):
    print("test one")
```

2、根据条件跳过

```python
workage = 10
@pytest.mark.skipif(workage<=10,reason="根据条件跳过")
def test_two(self):
    # time.sleep(3)
    print("test two")
```

##### 前置件、后置件

1、在当前类写前后置件，只作用于当前类

test_print.py

```python
def setup_class(self):
    print("每个类之前执行一次")

def teardown_class(self):
    print("每个类之后执行一次")

def setup_method(self):
    print("每个用例之前执行一次")

def teardown_method(self):
    print("每个用例之后执行一次")
```

2、创建公共类包common、公共类common_uitl.py、新类test_method.py，新类和引用common类的前后置项；

common_uitl.py

```python
class CommonUtil:
    def setup_class(self):
        print("每个类之前执行一次")

    def teardown_class(self):
        print("每个类之后执行一次")

    def setup_method(self):
        print("每个用例之前执行一次")

    def teardown_method(self):
        print("每个用例之后执行一次")
```

test_print.py

```python
import pytest

from common.common_util import CommonUtil

class TestPrint(CommonUtil):

    def test_one(self):
        print("test one")
```

test_method.py

```python
from common.common_util import CommonUtil

class TestMethod(CommonUtil):
    def test(self):
        print("hello")
```

##### fixture部分前后置项

1、指定某个用例使用某个前置项

test_method.py

```python
class TestPrint:

    # 添加前置项标识，此功能用作前置项，scope="function"表示作用域是方法，此外还有class、package、session等
    @pytest.fixture(scope="function")
    def select_sql(self):
        print("查询数据库")

    def test_fixture(self,select_sql):
        print("fixture")
```

2、自动执行

test_method.py

```python
# autouse = True 自动执行（默认False），不需要引用select_sql，本类的所有方法都会执行此前项
@pytest.fixture(scope="function",autouse=True)
def select_sql(self):
    print("查询数据库")

def test_fixture(self):
    print("fixture")

# @pytest.mark.skip(reason="无理由跳过")
def test_one(self):
    # time.sleep(3)
    # raise Exception()
    print("test one")
```

3、后置项yield关键字

test_method.py

```python
@pytest.fixture(scope="function")
def select_sql(self):
    print("查询数据库")
    yield
    print("关闭后置项")

def test_fixture(self,select_sql):
    print("fixture")
```

##### fixture进阶功能

###### 一、标记前后置项

###### 二、传参：

```python
@pytest.fixture(scope="function")
def select_sql(self):
    # yield 传参同retrun
    # yield "success"
    return "success"

def test_fixture(self,select_sql):
    print("fixture")
    print(select_sql)
```

或者

```python
import pytest

def read_yaml():
    return [1,2,3]

@pytest.fixture(scope="function",autouse=False,params=read_yaml())
def select_sql(request):
    print("查询数据库")
    # 返回参数
    yield request.param
    print("关闭数据库")

class TestFixture:
    def test_fix(self,select_sql):
        print("fix")
        print(select_sql)

```

ids，要与params配合使用，给参数加上id

```python
@pytest.fixture(scope="function",autouse=False,params=read_yaml(),ids=['no1','no2','no3'])
```

name，标记fixture名

```python
@pytest.fixture(scope="function",autouse=False,params=read_yaml(),ids=['no1','no2','no3'],name="s_sql")
def select_sql(request):
    print("查询数据库")
    # 返回参数
    yield request.param
    print("关闭数据库")

class TestFixture:
    #起了别名，fixture的名称就会失效
    # def test_fix(self, select_sql):
    def test_fix(self,s_sql):
        print("fix")
        print(s_sql)
```

###### 三、作用于类

```python
import pytest

@pytest.fixture(scope="class")
def select_sql():
    print("查询数据库")
    yield "success"
    print("关闭数据库")

# 作用于此注解下的类
@pytest.mark.usefixtures("select_sql")
class TestFixture:
    def test_fix(self):
        print("fix")

# 无usefixtures注解，则不作用于此类
class TestPrint:

    def test_fixture(self):
        print("fixture")
```

###### 四、作用package/session



五、conftest，存放固件



#### Locust 使用

1、安装python；
2、cmd 安装：pip3 install Locust（详细步骤：https://docs.locust.io/en/stable/installation.html#installation）；
3、参照官网写一个demo locu.py，cmd 运行：locust -f locu.py；
4、根据运行的提示，访问：http://localhost:8089/；