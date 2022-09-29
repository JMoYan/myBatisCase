# MySQL数据库

## 1.数据库语法

### 1.1.创建数据库

~~~sql

~~~

## 2.数据表语法

### 2.1.数据表字段操作

#### 2.1.1.增加字段

~~~sql
-- 增加一列
alter table 表名 add 字段名 字段类型(长度)
-- 增加一列(放在最前面)
alter table 表名 add 字段名 字段类型(长度) first
-- 增加一列(放在sex后面)
alter table 表名 add 字段名 字段类型(长度) after sex
~~~

#### 2.1.2.删除字段

~~~sql
-- 删除一列
alter table 表名 drop 字段名
~~~

#### 2.1.3.修改字段

~~~sql
-- 修改一列
-- modify 修改字段的类型 不会修改字段的名字
alter table 表名 modify 字段名 新的字段类型(字段长度)
-- change 可以同时修改 字段名称 和 字段类型
alter table 表名 change 字段名 新的字段名 新的字段类型(字段长度)
~~~

### 2.2.数据表数据操作

#### 2.2.1.创建数据表

~~~sql
create table 表名(
    -- 多个字段以逗号分隔
	字段名 字段类型(整型长度),
)
~~~

#### 2.2.2.删除数据表

~~~sql
drop table 表名
~~~

#### 2.2.3.查看表结构

~~~sql
desc 表名
~~~

#### 2.2.4.查看建表语句

~~~~sql
show create table 表名
~~~~

#### 2.2.5.查看表中数据

~~~sql
-- * 的意思是通配符，代指所有字段
select * from 表名
~~~

#### 2.2.6.增加数据

~~~sql
-- 字段类型 和 数量都要对应
insert into 表名 values(字段1,字段2，...)
-- 如果插入数据的个数与数量不对应就要加上对应的字段名
insert into 表名(字段名称1,字段名称2,...) values(字段1,字段2，...)
-- 同时插入多条数据
insert into 表名 values (字段1,字段2...),(字段1,字段2...),(字段1,字段2...)
~~~

#### 2.2.7.修改数据

~~~sql
-- 如果不加where就会把表里这个字段所有的数据都改为 新的数据
update 表名 set 字段名="新的数据" where 限制条件
~~~

#### 2.2.8.删除数据

~~~sql
-- 如果不加限制条件就会把这个表所有数据删掉
delete from 表名 where 限制条件
~~~

## 3.表的完整性约束

> 为防止不符合规范的数据存入数据库，在用户对数据进行插入、修改、删除等操作时，MySQL提供了一种机制来保证数据库中数据的==准确性==和==一致性==，这种机制就是==完整性约束==。
>
> MySQL主要支持以下几种完整约束，其中==Check==约束时MySQL8中提供的支持
>
> | 约束条件       | 约束描述                                     |
> | -------------- | -------------------------------------------- |
> | PRIMARY KEY    | 主键约束，约束字段的值可唯一的表示对应的记录 |
> | NOT NULL       | 非空约束，约束字段的值不能为空               |
> | UNIQUE         | 唯一约束，约束字段的值时唯一的               |
> | CHECK          | 检查约束，限制某个字段的取值范围             |
> | DEFAULT        | 默认值约束，约束字段的默认值                 |
> | AUTO_INCREMENT | 自增约束，约束字段的值自动递增               |
> | FOREIGN KEY    | 外键约束，约束表与表之间的关系               |

### 3.1.表字段的约束

#### 3.1.1.主键约束

~~~sql
-- 设置学号字段为主键
create table Student(
    snum int(8) primary key
)
~~~

#### 3.1.2.非空约束

~~~sql
-- 设置姓名字段非空
create table Student(
	sname varchar(4) not null
)
~~~

#### 3.1.3.唯一约束

~~~sql
-- 设置qq号字段唯一
create table Student(
	sqq int(10) unique
)
~~~

#### 3.1.4.检查约束

~~~sql
-- 设置性别字段只能输入 男 或 女
create table Student(
	ssex varchar(1) check(ssex="男" || sex="女")
)
-- 设置年龄字段只能输入 18 - 50 岁的
create table Student(
	sage varchar(3) check(sage >=18 and sage<=50)
)
~~~

#### 3.1.5.默认值约束

~~~sql
-- 设置性别字段的默认值为 '男'
create table Student(
	ssex varchar(1) default '男'
)
~~~

#### 3.1.6.自增约束

~~~sql
-- 设置学号字符为 自增
create table Student(
    -- 自增约束必须同时与主键约束同时存在
	snum int(8) primary key auto_increment
)
~~~

#### 3.1.7.外键约束

~~~sql
-- 创建主表(班级表)
create table Stucla(
	sno int(10) primary key auto_increment,
    sroom varchar(10)
)
-- 创建从表(学生表)并把主表(班级表)的sno设置为外键
create table Student(
    snum int(10) primary key auto_increment,
	scla int(4)
    -- 外键只能作为表约束 无法作为列约束
    constraint fk_slc_snum foreign key (scla) references Stucla (sno)
)
~~~

##### 3.1.7.1外键策略

~~~sql
-- 策略可以混合使用不一定只能使用一种策略
-- 策略1: not action 不允许操作
create table Stucla(
	sno int(10) primary key auto_increment,
    sroom varchar(10)
)
create table Student(
    snum int(10) primary key auto_increment,
	scla int(4)
    -- 外键后面默认时 not action 无论是更细还是删除都不会改变
    constraint fk_slc_snum foreign key (scla) references Stucla (sno)
)

-- 策略2: cascade 级联操作
create table Stucla(
	sno int(10) primary key auto_increment,
    sroom varchar(10)
)
create table Student(
    snum int(10) primary key auto_increment,
	scla int(4)
    -- 外键后面 on 操作的名字 cascade 就会在操作主表的时候 从表同时也会进行相同的操作
    constraint fk_slc_snum foreign key (scla) references Stucla (sno) on update cascade on delete cascade
)

-- 策略3: set null 置空操作
create table Stucla(
	sno int(10) primary key auto_increment,
    sroom varchar(10)
)
create table Student(
    snum int(10) primary key auto_increment,
	scla int(4)
    -- 外键后面 on 操作的名字 set null 就会在操作主表的时候 从表直接置空
   constraint fk_slc_snum foreign key (sclas) references Stucla (sno) on update set null on delete set null
)
~~~



### 3.2.表的约束

~~~sql
-- 创建一个表，学号字段为主键并且自增，姓名字段只能为'男'或'女',年龄字段只能输入>=18且<=50的,并且邮箱地址唯一,使用表约束
create table Student(
	snum int(8) auto_increment,
    sname varchar(4),
    ssex varchar(1),
    sage varchar(3),
    semail varchar(20),
    -- 表约束里无法使用的约束(auto_increament,not null,default)
    constraint pk_snum primary key(snum),
    constraint ck_ssex check(ssex='男' || ssex='女'),
    constraint ck_sage check(sage>=18 || sage<= 50),
    constraint un_email unique (seamil)
)
~~~

