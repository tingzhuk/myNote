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

