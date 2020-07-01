java环境变量安装好后，运行也没有任何问题。
在doc窗口进行编译后运行
javac ClassName.java
java ClassName
报错"错误: 找不到或无法加载主类ClassName"
错误的解决方法：将ClassName.java的package packagename去掉，重新编译可成功。但是类会报错。
正确的解决方法：编译完成之后，要到最外层包的上一级目录下运行类，而且类前面需要带包名，以“.”或者“/”隔开。
比如ClassName.java在目录 F:\my_workplace\java\mytest\src\mytest 下
在此目录执行：javac ClassName.java
然后回到目录 F:\my_workplace\java\mytest\src 下
在此目录执行：java mytest.ClassName
运行成功！

