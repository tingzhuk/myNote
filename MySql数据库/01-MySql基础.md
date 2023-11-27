# MySql基础

DQL: 数据查询语言（凡带有select 关键字的都是查询语言）

​	select .....

DML: 数据操作语言（凡是对**表**中的数据进行增删改的都是数据操作语言）

​	`insert` `delete`  `update`

​	`insert` 增

​	`delete` 删

​	`update` 改

​	这个主要是表中的数据

DDL：数据定义语言（凡是带有 `create  drop  alter `都是DDL） DDL主要操作的是表结构，不是表中的数据

​	`create` 新建，等同于增

​	`alter` 修改

​	`drop` 删除

​	这个增删改和 DML 不同 ， 这个主要操作表结构

TCL: 事务控制语言。包括事务提交：commit     事务回滚：  rollback

DCL: 是数据控制语言。列如：授权 grant, 撤销权限 revoke ......

![image-20230917231407594](https://boimage.oss-cn-beijing.aliyuncs.com/img_for_typora/image-20230917231407594.png)

## DQL

```sql
// 查询表中的所有数据
select * from 表名;

//不看表中的数据，只看表的结构
desc 表名;

//查看 mysql 的版本号
select version();

//查看当前用的哪个数据库
select detabase();
```

#### 简单查询

````sql
//查询一个字段
select 字段名 from 表明;

//查询两个字段或多个字段， 使用逗号隔开
select 字段1, 字段2....  from  表名;

//查询所有字段  1. 把所有字段都写上  2. *
select 所有字段 from 表名;
//这个效率低,   这个自己写着玩可以，别写到 java 程序中
select * from 表名;

//给查询的列起别名
/*
1. 使用 as 关键字 起别名     注意：只是将显示的查询结果显示成别名, 原表名不变
2. as 关键字可以省略 用空格替代。  假设起的别名有空格怎么办？  用单引号括起来   '别名'  双引号也可以    注意： 在所有的数据库中 字符串统一用单引号括起来， 双引号在 oracle 数据库中用不了。 再次强调：数据库中的字符串都是单引号括起来的，这是标准的，双引号不标准
*/
select 字段 as 别名 from 表名;

//计算员工年薪？  sal * 12    结论：字段可以使用数学表达式
select ename, sal*12 as '年薪' from emp;
````

#### 条件查询

```sql
/*
什么是条件查询？
	不是将表中所有数据查出来，是查询出来符合的条件
	语法格式：
		select
			....
		from
			...
		where
			条件;
			
			
	= 等于
	<> 或 ！= 不等于
	<= 小于等于
	> 大于
	>= 大于等于
	between ... and ... 两个值之间   等于 >= and  <=
	is null 为 null (is not null  不为空)
	and 并且
	or 或者
	in 包含， 相当于多个 or (not in 不在这个范围中)
	not not 可以去非， 主要用在 is 或 in 中
	like like 称为模糊查询  支持% 或下划线匹配
	% 匹配任意字符
	_ 一个下划线值匹配一个字符
*/
```



​	