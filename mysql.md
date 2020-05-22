查询空或非空：is null ,is not null
> SELECT id,ordersn FROM tale_name WHERE ordersn IS NOT NULL

------------

查询某个字段包含某几个字符
> SELECT * FROM tale_name where title like "%移%值%"

------------

查询出现2（多）次以上的字段
group保留下来的字段是mysql自动保留的，无法选择
> SELECT id,title,count(title) AS con FROM tale_name GROUP BY title HAVING con>2

------------

count和sum的区别
sum用来求某个字段的参数之和，比如订单某个状态的价格之和
> SELECT SUM(price) sum  FROM tale_name WHERE `status`>0

count用来统计查询到的数据条数之和，比如已支付的订单数量
> SELECT COUNT(*) FROM table_name WHERE `status`>0

------------
连表查询，只取其中一个
> SELECT o.* order o LEFT JOIN (SELECT * FROM refund GROUP BY orderid) r ON o.id=r.orderid WHERE o.status>0 