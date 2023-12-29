# MySQL学习笔记 

## 登录或退出 MySQL 服务器

```
# 登录 MySQL 
# 第一种， 直接在结尾打上密码
$ mysql -u root -p123456
# 第二种，当你旁边有人的时候，你不想让他看见你的密码, 会在下一行提示你输入密码
$ mysql -u root -p
# 下一行会出现这个效果
$ enter password : *******

# 退出 MySQL 数据库服务器
exit;

```

##  基本语法

```sql
-- 显示所有的数据库
$ show databases;

-- 创建数据库
$ create database 数据库名;

-- 切换数据库
use 数据库名;

-- 显示数据库中的所有的表
show tables;

-- 创建数据表  这里数据类型不在做过多叙述，不会直接百度
create table 表名(
	name VARCHAR(20),
    owner VARCHAR(20),
    species VARCHAR(20),
    sex CHAR(1),
    birth DATE,
    death DATE
);

-- 查看表结构
desc 表名;
-- 以 SQL 语句的形式查看该表的结构，即查看当时建表的 SQL 语句
show create table 表名;

-- 查看表中的数据  * 代表所有字段
SELECT * FROM 表名;

-- 插入数据  表名后面不加字段列表，默认就是该表的所有字段，这里以上面那个表为例
INSERT INTO 表名 VALUES ('buff', 'red', 'master', '男', '1900-11-12', null );

-- 删除数据
-- 把名字为 buff 的一行删掉
DELETE FROM 表名 WHERE name = 'buff';

-- 删除表， 工作时一般禁用删除语句
-- 第一种 drop table, 直接删除表的信息，但是无法找回数据[直接把表删了，重新建一个表]
DROP TABLE 表名;
-- 第二种 delete from  删除表中的数据，但不删除表结构，速度慢，与 where 连用，删除指定行
DELETE FROM 表名;
-- 第三种 truncate 删除表的数据，不删除表的结构，速度排名第二
truncate table 表名;
```

## 建表约束

> * 主键约束
> * 外键约束
> * 非空约束
> * 默认约束
> * 唯一约束
> * 自增长约束
> * 零填充约束

### 主键约束

```sql
-- 主键约束
-- 使某个字段不重复且不允许为空，确保表内数据的唯一性
CREATE TABLE user (
	id INT PRIMARY KEY,
    name VARCHAR(20)
);

-- 联合主键
-- 联合主键中每个字段不允许为空，并且加起来不能和自己设置的联合主键重复
CREATE TABLE user (
	id INT,
    name VARCHAR(20),
    password VARCHAR(20),
    PRIMARY KEY(id, name)
);

-- 自增约束
-- 自增约束的主键由系统自动递增分配
CREATE TABLE user (
	id INT PRIMARY KEY AUTO_INCREMENT,
    name VARCHAR(20)
);

-- 添加主键约束
-- 如果忘记设置主键，可以通过 SQL 语句设置（两种方式）
ALTER TABLE user ADD PRIMARY KEY(id);
ALTER TABLE user MODIFY id INT PRIMARY KEY;

-- 删除主键
ALTER TABLE user DROP PRIMARY KEY;
```

### 唯一约束

```sql
-- 建表时创建唯一约束
CREATE TABLE user (
	id INT,
    name VARCHAR(20),
    UNIQUE(name)
);

-- 添加唯一约束
-- 如果建表的时候没有添加唯一约束(两种方式)
ALTER TABLE user ADD UNIQUE(name);
ALTER TABLE user MODIFY name VARCHAR(20) UNIQUE;

-- 删除唯一约束
ALTER TABLE user DROP INDEX name;
```

### 默认约束

```sql
-- 建表时添加默认约束
-- 约束是某个字段的默认值
CREATE TABLE user2 (
	id INT,
    name VARCHAR(20),
    age INT DEFAULT 10
);

-- 移除非空约束
ALTER TABLE user2 MODIFY age INT;
```

### 外键约束

```sql
-- 班级
CREATE TABLE classes (
	id INT PRIMARY KEY,
    name VARCHAR(20)
);

-- 学生表
CREATE TABLE student (
	id INT PRIMARY KEY,
    name VARCHAR(20),
    -- 这里的 class_id 要和 classes 中的 id 相关联
    class_id INT,
    -- 表示 class_id 的值必须来自于 classes 中的 id 字段
    FOREIGN KEY(class_id) REFERENCES classes(id)
)

-- 1. 主表（父表）classes 中没有的数据值，在副表(字表) students, 是不可以使用的
-- 2. 主表的记录被副表引用时，主表不可以被删除
```

## 数据库三大设计范式

### 1NF

只要字段还可以继续差分，就不满足第一范式。

范式设计的越详细，对某些实际操作可能会更好。但并非都有好处，需要根据项目的实际情况进行设定。

### 2NF

在满足第一范式的前提下，其他列都必须完全依赖于主键，如果出现不完全依赖，只可能发生在联合主键的情况下：

```sql
-- 订单表
CREATE TABLE myorder (
	product_id INT,
    customer_id INT,
    product_name VARCHAR(20),
    customer_name VARCHAR(20),
    PRIMARY KEY(product_id, customer_id)
);
```

实际上，在这张订单表中，`product_name` 只依赖于 `product_id`,  `customer_name` 只依赖于 `customer_id` 。也就是说，`peoduct_name` 和 `customer_id` 是没关系的，`customer_name` 和`product_id` 也是没有关系的。

这就不满足第二范式：**其他列必须完全依赖于主键！**

```sql
CREATE TABLE myorder (
	order_id INT PRIMARY KEY,
    product_id INT,
    customer_id INT
);

CREATE TABLE product (
	id INT PRIMARY KEY,
    name VARCHAR(20)
);

CREATE TABLE customer (
	id INT PRIMARY KEY,
    name VARCHAR(20)
);
```

拆分之后， `myorder`  表中的 `product_id` 和 `customer_id` 完全依赖于 `order_id`主键，而 `product` 和 `customer` 表中的其他字段又完全依赖于主键，满足第二范式的设计！

### 3NF

在满足第二范式的前提下，**除了主键列之外，其他列主键不能有传递关系。**

```sql
CREATE TABLE myorder (
	order_id INT PRIMARY key,
    product_id INT,
    customer_id INT,
    customer_phone VARCHAR(15)
);
```

表中的 `customer_phone` 有可能依赖于 `order_id` , `customer_id` 两列，也就不满足了第三范式的要求：**其他列之间不能有传递关系。**

```sql
CREATE TABLE myorder (
	order_id INT PRIMARY KEY,
    product_id INT,
    customer_id INT
);

CREATE TABLE customer (
	id INT PRIMARY KEY,
    name VARCHAR(20),
    phone VARCHAR(15)
);
```

修改后就不存在与其他列的依赖关系，其他列都只依赖于主列，满足第三范式的设计！

## 查询练习

### 准备数据

```sql
create database select_test;

use select_test;

-- 创建学生表
create table student (
    no varchar(20) primary key ,
    name varchar(20) not null ,
    sex varchar(10) not null ,
    birthday date, -- 生日
    class varchar(20) -- 所在班级
);

-- 创建教师表
create table teacher (
    no varchar(20) primary key ,
    name varchar(20) not null ,
    sex varchar(10) not null ,
    birthday date,
    profession varchar(20) not null , -- 职称
    department varchar(20) not null -- 部门
);

-- 创建课程表
create table course (
    no varchar(20) primary key ,
    name varchar(20) not null ,
    t_no varchar(20) not null , -- 教师编号
    -- 表示 t_no 来自 teacher 表的 no 字段
    foreign key (t_no) references teacher(no)
);

-- 成绩表
create table score (
    s_no varchar(20) not null , -- 学生编号
    c_no varchar(20) not null , -- 课程编号
    degree DECIMAL, -- 成绩
    -- 表示 s_no， c_no 分别来自于 student， course 表中的 no 字段值
    foreign key (s_no) references student(no),
    foreign key (c_no) references course(no),
    -- 设置 s_no c_no 为联合主键
    PRIMARY KEY (s_no,c_no)
);

-- 展示所有表
show tables;

-- 添加学生数据表
INSERT INTO student VALUES ('101','曾华','男','1977-09-01','95033');
INSERT INTO student VALUES ('102','匡明','男','1975-10-02','95031');
INSERT INTO student VALUES ('103','王丽','女','1076-01-05','95033');
INSERT INTO student VALUES ('104','李军','男','1976-02-20','95033');
INSERT INTO student VALUES ('105','王芳','女','1975-02-10','95031');
INSERT INTO student VALUES ('106','陆军','男','1974-06-03','95031');
INSERT INTO student VALUES ('107','王五','男','1976-02-20','95033');
INSERT INTO student values ('108','张全蛋','男','1975-02-10','95031');
INSERT INTO student VALUES ('109', '赵铁柱','男','1974-06-03','95031');

-- 添加教师数据表
INSERT INTO teacher VALUES ('804','李成','男','1958-12-02','副教授','计算机系');
INSERT INTO teacher VALUES('856', '张旭', '男', '1969-03-12', '讲师', '电子工程系');
INSERT INTO teacher VALUES('825', '王萍', '女', '1972-05-05', '助教', '计算机系');
INSERT INTO teacher VALUES('831', '刘冰', '女', '1977-08-14', '助教', '电子工程系');

-- 添加课程数据表
INSERT INTO course VALUES('3-105', '计算机导论', '825');
INSERT INTO course VALUES('3-245', '操作系统', '804');
INSERT INTO course VALUES('6-166', '数字电路', '856');
INSERT INTO course VALUES('9-888', '高等数学', '831');

-- 添加添加成绩表数据
INSERT INTO score VALUES('103', '3-105', '92');
INSERT INTO score VALUES('103', '3-245', '86');
INSERT INTO score VALUES('103', '6-166', '85');
INSERT INTO score VALUES('105', '3-105', '88');
INSERT INTO score VALUES('105', '3-245', '75');
INSERT INTO score VALUES('105', '6-166', '79');
INSERT INTO score VALUES('109', '3-105', '76');
INSERT INTO score VALUES('109', '3-245', '68');
INSERT INTO score VALUES('109', '6-166', '81');

-- 查询学生表所有行
select * from student;
-- 查询教师表所有行
select * from teacher;
-- 查询课程表所有行
select * from course;
-- 查询成绩表所有行
select * from score;

```

### 基本查询

```sql
-- 查询 student 表的所有行
select * from student;

-- 查询 student 表中的 name, sex, class 字段的所有行
select name, sex, class from student;

-- 查询 teacher 表中不重复 department 列
select distinct department from teacher;

-- 查询 score 表中成绩在 60 - 80 之间的所有行（区间查询和运算符）
-- between xxx and xxx      查询区间 ， and 表示 “并且”
select * from score where degree between 60 and 80;
select * from score where degree >= 60 and degree <= 80;

-- 查询 score 表中成绩为 85 86 或 87 的行
-- IN ：查询规定中的多个值
select * from score where degree in (85, 86, 97);

-- 查询 student 表中 '95031' 班或性别为 ‘女’ 的 所有行
-- or ：表示或者关系
select * from student where class = '95031' or sex = '女';

-- 以 class 降序方式查询 student 表中的所有行
-- DESC 降序， 从高到低
-- ASC(默认可以不写) 升序 ， 从高到低
select * from student order by class desc ;
select * from student order by class ;

-- 以 c_no 升序， degree 降序查询 score 表中的所有行 (默认就是升序)
select * from score order by c_no , degree desc;

-- 查询 ‘95031’ 班的学生人数
-- COUNT : 统计
select count(*) from student where class = '95031';

-- 查询 score 表中最高分学生的学号和课程编号（子查询或排序查询）
select s_no, c_no from score where degree = (select max(degree) from score);

-- 排序查询
-- limit r, n  表示从第 r 行开始， 查询 n 条数据
select s_no, c_no, degree from score order by degree desc
limit 0, 1;
```

### 分组计算平均成绩

**查询每门课的平均成绩**

```sql
-- AVG : 平均值
select avg(degree) from score where c_no = '3-105';
select avg(degree) from score where c_no = '3-245';
select avg(degree) from score where c_no = '6-166';

-- GROUP BY ： 分组查询
select c_no, avg(degree) from score group by c_no;

```

### 分组条件于模糊查询

**查询 `score` 表中至少有两名学生选修，并且以 3 开头的课程的平均数**

```sql
-- 首先把 c_no avg(degree) 通过分组查询出来
select c_no, avg(degree) from score group by c_no;

-- 在上面的基础上查询出至少有 3 名学生选修的课程
select c_no as '课程编号', avg(degree) as '平均成绩', count(c_no) as '选修人数' from score group by c_no
having count(c_no) >= 2;

-- 在上面的基础上，查询出课程编号以 3 开头的
-- 这里使用 like 进行模糊查询： % 表示任意个个字符的通配符
select c_no as '课程编号', avg(degree) as '平均成绩', count(c_no) as '选修人数'
from score
group by c_no
having count(c_no) >= 2 and c_no like '3%';
```

### 多表查询 -1

查询所有学会生的 name 以及 该学生在 `score` 表中对应的 `c_no` 和 `degree`

```sql
-- from..... :  表示从 student, score 表中查询
-- where 的条件表示为， 只有在 student.no 和 score.c_no 相等是才显示出来
select name, c_no, degree from student, score
where student.no = score.s_no;
```

### 多表查询 -2

**查询所有学生的 `no` 、课程名称 ( `course` 表中的 `name` ) 和成绩 ( `score` 表中的 `degree` ) 列。**

```sql
-- 表示获得这个字段的别名
select s_no, name as c_name, degree from score, course
where score.c_no = course.no;
```

### 三表联合查询

**查询所有学生的 `name` 、课程名 ( `course` 表中的 `name` ) 和 `degree` 。**

```sql
-- 查询所有学生的 name， 课程名和 degree
-- 不难发现只有 score 表中关联了学生的学号和课程号
-- 只要 把 score 表中的 s_no c_no  替换成 student course  表中的 name 就可以了
select student.name as s_name, course.name as c_name, degree from score, student, course
where student.no = score.s_no and score.c_no = course.no;
```

### 子查询 + 分组求平均成绩

```sql
-- 查询 95031 班 学生 每门课程的平均成绩
-- 1. 先获得 95031 班所有学生的 学号
-- 2. 在按课程号进行分组，求每门课平均成绩
select c_no, avg(degree) as '平均成绩'
from score
where s_no in (SELECT no FROM student WHERE class = '95031')
group by c_no;

```

### 子查询-1

**查询在 `3-105` 课程中，所有成绩高于 `109` 号同学的记录。**

```sql
-- 查询在 3-105 课程中，所有成绩高于 109 号同学的记录。
SELECT * FROM score
WHERE c_no = '3-105' AND degree > (SELECT degree FROM score WHERE s_no = '109' AND c_no = '3-105');
```

### 子查询-2

**查询所有成绩高于 `109` 号同学的 `3-105` 课程成绩记录。**

```sql
-- 查询所有成绩高于 109 号同学的 3-105 课程成绩记录。
SELECT * FROM score
WHERE degree > (SELECT degree FROM score WHERE s_no = '109' AND c_no = '3-105');
```

### `YEAR` 函数 与 带 in 关键字查询

```sql
-- 查询所有和 101 、108 号学生同年出生的 no 、name 、birthday 列。
SELECT no, name, birthday FROM student
where YEAR(birthday) in (SELECT YEAR(birthday) FROM student WHERE no IN('101','108'));
```

### 多层嵌套查询

**查询 `'张旭'` 教师任课的学生成绩表。**

```sql
-- 查询 '张旭' 教师任课的学生成绩表。
select * from teacher;
select * from course;

SELECT * FROM score WHERE c_no IN (
    SELECT no FROM course WHERE t_no IN (
        SELECT no FROM teacher WHERE name = '张旭'
        )
    );
```

### 多表查询

**查询某选修课程多于5个同学的教师姓名。**

```sql
-- 查询某选修课程多于5个同学的教师姓名。
-- 1. 从成绩表中 按 c_no 进行分类， 找出 选修人数大于 5 的 课程编号
-- 2. 根据课程编号在课程表中找到教师编号
-- 3. 根据教师编号在教师表中找到 教师姓名
SELECT name FROM teacher WHERE no IN (
    SELECT t_no FROM course WHERE no IN (
        SELECT c_no FROM score GROUP BY c_no HAVING COUNT(c_no) > 5
        )
    );

```

### 子查询 -3

**查询 “计算机系” 课程的成绩表。**

```sql
-- 查询 “计算机系” 课程的成绩表。
-- 1. 根据表结构 teacher 表中有哪个教师是哪个系的，因此，查询教师表中系别是计算机系的教师的 no
-- 2. 根据上一步查找 的 教师 no 在查找课程表，看看计算机系的老师都教了哪些课程
-- 3. 根据上一步的课程编号即可查到计算机系 课程的成绩表
select * from score where c_no in (
    SELECT no FROM course WHERE t_no IN (
        SELECT no FROM teacher WHERE department = '计算机系'
        )
    );
```

### UNION 和 NOTIN 的使用

**查询 `计算机系` 与 `电子工程系` 中的不同职称的教师。**

```SQL
-- NOT 代表逻辑非
-- UNION 合并两个集合
-- 要求 : 查找计算机系和电子工程系不同职称的来时信息
SELECT * FROM teacher WHERE department = '计算机系' AND profession NOT IN (
    SELECT profession FROM teacher WHERE department = '电子工程系'
    )
UNION
SELECT * FROM teacher WHERE department = '电子工程系' AND profession NOT IN (
    SELECT profession FROM teacher WHERE department = '计算机系'
    );
```

