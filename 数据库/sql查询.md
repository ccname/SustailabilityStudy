```
SELECT lan, count(*) as cnt from qq_album_song_table1 GROUP BY lan having cnt>10000;


show global variables like '%timeout%';
set global net_read_timeout = 900; 
set global net_write_timeout = 1600;

select a.song_name, a.singer, b.song_name, b.singer from new_migu_playlist_song_album_song a inner join wyy_playlist_song_album_song_table b on a.song_name=b.song_name and a.singer=b.singer

select a.song_name, a.singer, b.song_name, b.singer, c.song_name, c.singer from qq_playlist_song_album_song_table1 a inner join wyy_playlist_song_album_song_table b inner join new_migu_playlist_song_album_song c on a.song_name=b.song_name and a.song_name=c.song_name and b.song_name=c.song_name and a.singer=b.singer and a.singer=c.singer and b.singer=c.singer GROUP BY a.song_name;


select a.song_name, a.singer, b.song_name, b.singer, c.song_name, c.singer from qq_playlist_song_album_song_table1 a, wyy_playlist_song_album_song_table b, new_migu_playlist_song_album_song c where a.song_name=b.song_name and a.song_name=c.song_name and b.song_name=c.song_name and a.singer=b.singer and a.singer=c.singer and b.singer=c.singer;



select * from 
(SELECT song_name,count(dissid) as appear from qq_playlist_song_table
GROUP BY song_name) A
where appear>1
ORDER BY appear desc


select * from 
(SELECT song_name, count(playlist_id) as ap from wyy_playlist_song_table
group by song_name)  a
where ap >1
ORDER BY ap desc limit 50
```
```
USE copy_demp;

CREATE TABLE tblcourse (
	CourseId VARCHAR ( 3 ) NOT NULL COMMENT '课程编号',
	CourseName VARCHAR ( 20 ) NOT NULL COMMENT '课程名称',
	TeaId VARCHAR ( 3 ) DEFAULT NULL COMMENT '授课教师编号',
	PRIMARY KEY ( CourseId ) 
) ENGINE = INNODB DEFAULT CHARSET = utf8 COMMENT '课程表';

INSERT INTO `tblcourse`
VALUES
	( '001', '企业管理', '002' );
	
INSERT INTO `tblcourse`
VALUES
	( '002', '马克思', '008' );
	
INSERT INTO `tblcourse`
VALUES
	( '003', 'UML', '006' );
INSERT INTO `tblcourse`
VALUES
	( '004', '数据库', '007' );
INSERT INTO `tblcourse`
VALUES
	( '005', '逻辑电路', '006' );
INSERT INTO `tblcourse`
VALUES
	( '006', '英语', '003' );
INSERT INTO `tblcourse`
VALUES
	( '007', '电子电路', '005' );
INSERT INTO `tblcourse`
VALUES
	( '008', '毛泽东思想概论', '004' );
INSERT INTO `tblcourse`
VALUES
	( '009', '西方哲学史', '012' );
INSERT INTO `tblcourse`
VALUES
	( '010', '线性代数', '017' );
INSERT INTO `tblcourse`
VALUES
	( '011', '计算机基础', '013' );
INSERT INTO `tblcourse`
VALUES
	( '012', 'AUTO CAD制图', '015' );
INSERT INTO `tblcourse`
VALUES
	( '013', '平面设计', '011' );
INSERT INTO `tblcourse`
VALUES
	( '014', 'Flash动漫', '001' );
INSERT INTO `tblcourse`
VALUES
	( '015', 'Java开发', '009' );
INSERT INTO `tblcourse`
VALUES
	( '016', 'C#基础', '002' );
INSERT INTO `tblcourse`
VALUES
	( '017', 'Oracl数据库原理', '010' );
	
DROP TABLE IF	EXISTS `tblscore`;

CREATE TABLE `tblscore` ( `StuId` VARCHAR ( 5 ) DEFAULT NULL COMMENT '学号', `CourseId` VARCHAR ( 3 ) DEFAULT NULL COMMENT '课程编号', `Score` FLOAT DEFAULT NULL COMMENT '成绩' ) ENGINE = INNODB DEFAULT CHARSET = utf8;
DROP TABLE
IF
	EXISTS `tblstudent`;
CREATE TABLE `tblstudent` (
	`StuId` VARCHAR ( 5 ) NOT NULL COMMENT '学号',
	`StuName` VARCHAR ( 10 ) NOT NULL COMMENT '学生姓名',
	`StuAge` INT ( 11 ) DEFAULT NULL COMMENT '学生年龄',
	`StuSex` CHAR ( 1 ) NOT NULL COMMENT '学生性别',
	PRIMARY KEY ( `StuId` ) 
) ENGINE = INNODB DEFAULT CHARSET = utf8;

DROP TABLE IF	EXISTS `tblteacher`;

CREATE TABLE `tblteacher` ( `TeaId` VARCHAR ( 3 ) NOT NULL COMMENT '教师编号', `TeaName` VARCHAR ( 10 ) NOT NULL COMMENT '教师名称', PRIMARY KEY ( `TeaId` ) ) ENGINE = INNODB DEFAULT CHARSET = utf8;

--  查询“001”课程比“002”课程成绩高的所有学生的学号；
-- select B.StuId from tblscore A, tblscore B where A.CourseId='001' and B.CourseId='002' and A.Score > B.Score  and A.StuId=B.StuId;
SELECT DISTINCT
	A.StuId 
FROM
	tblscore A,
	tblscore B 
WHERE
	A.CourseId = '001' 
	AND B.CourseId = '002' 
	AND A.Score > B.score 
	AND A.stuid = B.stuid;
	
SELECT
	StuId 
FROM
	tblstudent s1 
WHERE
	( SELECT score FROM tblscore t1 WHERE t1.StuId = s1.StuId AND t1.CourseId = '001' ) > ( SELECT score FROM tblscore t2 WHERE t2.StuId = s1.StuId AND t2.CourseId = '002' ) 
	
	--  查询平均成绩大于60分的同学的学号和平均成绩；
-- 	select StuId, avg(Score) as num from tblscore GROUP BY StuId HAVING num > 60;
SELECT
	StuId,
	avg( score ) AS num 
FROM
	tblscore 
GROUP BY
	StuId 
HAVING
	avg( score ) > 60;
	
	-- 查询学习过‘001’课程的男生和女生人数各是多少？
-- 	select B.StuSex, count(*) as num from tblscore A, tblstudent B where A.CourseId='001' and A.StuId=B.StuId GROUP BY B.StuSex;
SELECT
	A.stusex,
	count( * ) AS num 
FROM
	tblstudent A,
	tblscore B 
WHERE
	B.CourseId = '001' 
	AND A.stuid = B.stuid 
GROUP BY
	A.stusex;
	
	-- 查询姓“李”的老师的个数；
-- 	select count(*) as num from tblteacher where TeaName like '叶%'
SELECT
	count( * ) AS num 
FROM
	tblteacher 
WHERE
	teaname LIKE '裘%';
	
	-- 查询“张无忌”的所有学习课程名称和授课老师姓名；
-- 	select C.StuName, A.CourseName, B.TeaName from tblcourse A, tblteacher B, tblstudent C, tblscore D where C.StuName='张无忌' and C.StuId=D.StuId and D.CourseId=A.CourseId and A.TeaId=B.TeaId;
SELECT
	A.stuname,
	B.coursename,
	C.teaname 
FROM
	tblstudent A,
	tblcourse B,
	tblteacher C,
	tblscore D 
WHERE
	stuname = '张无忌' 
	AND A.stuid = D.stuid 
	AND B.courseid = D.courseid 
	AND B.teaid = c.teaid;
	
	-- 查询每一门课程的课程名称，授课教师姓名，课程平均成绩；
-- 	select A.CourseName, B.TeaName, avg(C.Score) as num from tblcourse A, tblteacher B, tblscore C where A.TeaId=B.TeaId and A.CourseId=C.CourseId GROUP BY C.CourseId;
SELECT
	B.coursename,
	C.teaname,
	avg( D.score ) AS num 
FROM
	tblcourse B,
	tblteacher C,
	tblscore D 
WHERE
	B.teaid = C.teaid 
	AND B.courseid = D.courseid 
GROUP BY
	D.courseid;
	
	-- 查询学过“001”并且也学过编号“002”课程的同学的学号、姓名
-- 	select A.StuId, A.StuName from tblstudent A WHERE 
-- 	A.StuId in (select StuId from tblscore B where B.CourseId='001') and A.StuId in (select StuId from tblscore B where B.CourseId='002')
SELECT
	stuid,
	stuname 
FROM
	tblstudent t1 
WHERE
	( SELECT count( * ) FROM tblscore t2 WHERE t2.stuid = t1.stuid AND t2.courseid = '001' ) > 0 
	AND ( SELECT count( * ) FROM tblscore t2 WHERE t2.stuid = t1.stuid AND t2.courseid = '002' ) > 0;
SELECT
	stuid,
	stuname 
FROM
	tblstudent t1 
WHERE
	t1.stuid IN ( SELECT stuid FROM tblscore WHERE courseid = '001' ) 
	AND t1.stuid IN ( SELECT stuid FROM tblscore WHERE courseid = '002' );
	
	-- 查询课程编号“002”的成绩比课程编号“001”课程低的所有同学的学号、姓名；
	select StuId, StuName from tblstudent t1 where (select Score from tblscore t2 where t1.StuId=t2.StuId and t2.CourseId='001') >
	(select Score from tblscore t3 where t1.StuId=t3.StuId and t3.CourseId='002');
	
SELECT
	stuid,
	stuname 
FROM
	tblstudent t1 
WHERE
	( SELECT score FROM tblscore WHERE courseid = '001' AND stuid = t1.stuid ) > ( SELECT score FROM tblscore WHERE courseid = '002' AND stuid = t1.stuid ) 
	
	-- 查询已经学完所有课程的同学的学号、姓名；
	select stuid, stuname from tblstudent t1 where (select count(*) from tblscore t2 where StuId='1002' GROUP BY StuId) = (select count(*) from tblcourse )
	
SELECT
	stuid,
	stuname 
FROM
	tblstudent t1 
WHERE
	( SELECT count( * ) FROM tblscore WHERE stuid = t1.stuid ) = ( SELECT count( * ) FROM tblcourse ) 
	
	
	-- 查询课程补考过的学生学号，课程号；[同一门课程成绩存在两次代表补考]

	select Astuid, A.Acourseid from tblstudent t1, 
	(select count(*) as num, courseid as Acourseid, stuid as Astuid from tblscore GROUP BY CourseId, StuId) as A 
	where t1.stuid=A.Astuid and A.num > 1;

SELECT
	Astuid,
	Acourseid 
FROM
	tblstudent sd,
	( SELECT count( * ) AS countNum, courseid AS Acourseid, stuid AS Astuid FROM tblscore sc GROUP BY sc.courseid, stuid ) A 
WHERE
	sd.stuid = A.Astuid 
	AND A.countNum > 1;
	
	-- 查询所有同学的学号、姓名、选课数、总成绩;
select stuid, stuname, 
(select count(distinct courseid) from tblscore where stuid=t1.stuid) as c_num,
(select sum(score) from tblscore where stuid=t1.stuid) as s_sum
from tblstudent t1;

SELECT
	stuid,
	stuname,
	( SELECT count( DISTINCT courseid ) FROM tblscore sc WHERE sc.stuid = st.stuid ) AS selcourseid,
	( SELECT sum( score ) FROM tblscore sc WHERE sc.stuid = st.stuid ) AS sumscore 
FROM
	tblstudent st 
	
	-- 创建一个查询视图，视图中包括学生学号，学生姓名，授课教师编号，教师姓名，课程编号，课程名称，成绩，查询出视图中的内容
	create view show_view as select 
	a.stuid, a.stuname, c.teaid, c.teaname, b.courseid, b.coursename, d.score from 
	tblstudent a, tblcourse b, tblteacher c, tblscore d
	where a.stuid=d.stuid and c.teaid=b.teaid and b.courseid=d.courseid;
	
	select * from show_view;
	
	
	CREATE VIEW show_view AS SELECT
	a.stuid,
	a.stuname,
	b.teaid,
	b.teaname,
	c.courseid,
	c.coursename,
	d.score 
FROM
	tblstudent a,
	tblteacher b,
	tblcourse c,
	tblscore d 
WHERE
	a.stuid = d.stuid 
	AND b.teaid = c.teaid 
	AND c.courseid = d.courseid;
	
SELECT * FROM	show_view 

-- 查出”周芷若”同学所有未选课程编号和课程名称
select courseid, coursename from tblcourse a where courseid not in 
(select d.courseid from tblcourse b, tblstudent c, tblscore d where stuname='周芷若' and d.stuid=c.stuid and b.courseid=d.courseid)

SELECT
	X.courseid AS '未选课程编号',
	X.coursename AS '未选课程名称' 
FROM
	tblcourse X 
WHERE
	courseid NOT IN (
	SELECT
		cs.courseid 
	FROM
		tblcourse cs,
		tblstudent st,
		tblscore sc 
	WHERE
		st.stuname = '周芷若' 
		AND st.stuid = sc.stuid 
		AND cs.courseid = sc.courseid 
	) 
	
-- 查出和”周芷若”同学一起上过课的所有同学学号和姓名
-- 实现思路通过名字查出学号，通过学号，查询出学生学习过的课程，
-- 使用where in进行筛选学过这些课程的学生信息，去除重复值，去除‘周芷若’
select 	distinct A.stuid, A.stuname from 	tblstudent A, tblscore B 
where B.courseid in (select distinct courseid from tblscore C where stuid=
(select stuid from tblstudent where stuname='周芷若')
and A.stuname != '周芷若');

-- 降序
select company from tblorder ORDER BY company desc;

-- 更新某列的值 把lastname为‘bob’的address改为nanjinglu,city 改为‘shanghai’ 
update person set address='nanjinglu', city='shanghai' where lastname='bob'

select ascii('a');

select concat(12,34,'ab');
```
