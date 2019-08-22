查询空或非空：is null ,is not null
SELECT id,ordersn FROM `tale_name` where ordersn IS NOT NULL

查询某个字段包含某几个字符
SELECT * FROM `tale_name` where title like "%移%值%"

查询出现2（多）次以上的字段
group保留下来的字段是mysql自动保留的，无法选择
SELECT id,title,count(title) AS con FROM `tale_name` GROUP BY title HAVING con>2