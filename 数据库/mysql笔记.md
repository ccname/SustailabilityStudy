+ 查询移动微分享连续参加三个任务以上的活跃人数
```
select partner_id, count(*) as num from p3_partner_task where task_id in (37, 38, 39) GROUP BY partner_id; 
```

+ 查看数据库是Innodb 还是MyIsam 
```
 show variables like '%storage_engine%';
```
+ 数据库备份 mysqldump 
```
mysqldump -uroot -p caixun > /Users/chenchen/Desktop/bf_caixun.sql
```
+ 还原
```
mysql -u root -p copy_demp < /Users/chenchen/Desktop/bf_caixun.sql 
```
+ 查看mysql的运行时长
```
mysql -uroot -p 进入mysql
show global status like 'uptime';
```
+ my.ini配置文件 mysqld 节点下添加(2013 lost connection to mysql server during query)
```
max_allowed_packet = 500M

max_allowed_packet的大小，如果是这种原因，你只要修改my.cnf，加大max_allowed_packet的值即可。
```

+ 查看文件大小是否超过 max_allowed_packet
```
mysql> show global variables like 'max_allowed_packet';

修改参数
mysql> set global max_allowed_packet=1024*1024*16;
mysql> show global variables like 'max_allowed_packet';

```
- 修改mysql数据库某一字段里面的值。
```
# mysql replace()
UPDATE tableName SET columnName = REPLACE(columnName, 'from_str' , 'to_str');
```
- mysql删除多余的重复数据，只保留id较小的一条
```
# 会报错，意思就是不能一边查寻一边删除，借用中间表解决
delete FROM qq_singer_song_comments WHERE 
commentid in (SELECT commentid FROM qq_singer_song_comments GROUP BY commentid HAVING COUNT(*) >1)
and 
comment_time not in (select min(comment_time) from qq_singer_song_comments group by commentid having count(*)>1)


# 成功删除的语句。借用了中间表
delete from qq_singer_song_comments where 
commentid in (SELECT a.commentid from
(select commentid from qq_singer_song_comments group by commentid having count(commentid) > 1) a)
and 
comment_time not in (SELECT b.comment_time from 
(select comment_time from qq_singer_song_comments group by commentid having count(commentid) > 1) b)

```

- 有时候可能数据库存的是一个汇总的字段，例如表A，有一类型名称为identifier，根据0和1代表不同含义来区分值，比如0代表私有，1代表公有，值存储在name字段里，这时候想获得这样的结果：id， 私有的名字， 共有的名字三列字段，我们可以在查询时候用 IF 语句来实现。
```
SELECT cam_id, IF(identifier = 0, name, null) as private, if(identifier = 1, name, null)as public from table_A limit 10
```

- 将同张表的同类型的字段赋值给另一字段
```
UPDATE t_user  
SET signed_time = create_time 
```
- 将同一个表中两个类型一样的字段的值互换
```
UPDATE t_user u1, t_user u2 
SET u1.signed_time = u2.create_time,  
    u2.create_time = u1.signed_time  
```

- 一个表中把一个字段的值根据指定字符切分，取某个值并复制给另一个字段
```
# 参考链接 https://blog.csdn.net/qq_25497867/article/details/71170785
UPDATE qq_playlist_song_table,(SELECT playlist_song_id d, SUBSTRING_INDEX(playlist_song_id, "_", 1) c from qq_playlist_song_table) b SET qq_playlist_song_table.playlist_id=b.c where qq_playlist_song_table.playlist_song_id =b.d;
```
