# 引用css文件
```html
<link href="../data/css/base.css" type="text/css" rel="stylesheet">
```
 

如果引用不成功，大概率是路径出问题。在浏览器里查看元素，Console下面会打印出错误原因，比如：net::ERR_ABORTED 404 (Not Found)。

------------


查看元素的css及其设置文件
在浏览器里查看元素，Elements的Styles下面有元素的css设置，可以在此直接编辑。
每个元素样式的右上角是这个css样式所在的文件名，鼠标停靠到文件名上，可以看到完整的文件路径。

------------
# 元素选择
选择元素名
```css
body{
color:red;
text-align:center;
}
h1{
}
```
选择类名，可多个元素用同一个类名，通过类名选择元素是最常用的方法。
```css
.classname{
}
```
指定的元素选择类名。不同的元素有相同的类名，可以指定某一元素使用类名，其他不同元素不受影响。
p.classname{
}
选择id，一个id名只能对应一个元素，id相同的元素会报错。
```css
#idname{
}
```
选择元素的子元素，通过类名或id名选择元素，并选择其子元素。
```css
.classname>div{
/*选择classname 下的所有div元素*/
}
#idname>div{
/*选择idname 下的所有div元素*/
}
```
选择多个元素，用英文逗号隔开元素名。




