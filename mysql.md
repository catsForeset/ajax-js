查询空或非空：is null ,is not null
> SELECT id,ordersn FROM tale_name WHERE ordersn IS NOT NULL

------------
查询某个字段包含某几个字符
关键字 LIKE
> SELECT * FROM tale_name WHERE title LIKE "%移%值%"

关键字 LOCATE
> SELECT * FROM `goods` WHERE LOCATE('葡萄酒',title)>0;

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
连表查询（一对多），重复id只其中一个
> SELECT o.* order o LEFT JOIN (SELECT * FROM refund GROUP BY orderid) r ON o.id=r.orderid WHERE o.status>0 

------------
数据库时间戳，unix_timestamp()
SELECT * FROM order WHERE (createtime+24*60*60)>unix_timestamp() ORDER BY createtime DESC LIMIT 10 -- 查询24小时之内下单的订单

字段加前缀
select t.*，concat('#',t.column_A) as column_A t from table_name where t.id = 'xx';

将多个字段连成一个字段
select concat (id, name, score) as info from tt2;

将查询数据按逗号分列
select substring_index(g.cates,',',1);

(select substring_index( substring_index(g.cates,',',1),',',-1)) cates1;
(select substring_index( substring_index(g.cates,',',2),',',-1)) cates2;

内查询 union all
