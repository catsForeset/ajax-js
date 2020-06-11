ISO-8601时间
格式：2020-06-09t01:19:21.9885078+08:00
生成：date(DATE_ISO8601[, int $timestamp ])
转换为时间戳：strtotime("2020-06-09t01:19:21.9885078+08:00")

判断一个字符串是否为json格式
将改字符串json_decode(),若得到的结果是对象is_object()或者数组is_array()，则该字符串是json格式，否则不是json格式。