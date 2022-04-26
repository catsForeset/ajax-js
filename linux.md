

sed -i "/logdb/d" config.php  删除匹配到 logdb 的行

echo "\$config['logdb']['master']['username'] = 'root';" >> config.php  追加行，$符号前要加转义符\