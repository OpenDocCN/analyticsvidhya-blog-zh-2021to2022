# 使用示例 DDL 和 DML 命令了解 SQL

> 原文：<https://medium.com/analytics-vidhya/understanding-sql-with-sample-ddl-and-dml-commands-5d07ecc68d8e?source=collection_archive---------17----------------------->

要提高 SQL 技能，一个方法就是安装一个类似 MySQL 的 SQL 包，开始用它练习。为了给大家一个积极的推动，我在这篇文章中列出了几个 SQL 查询问题。

我选择了一组 30 个 SQL 查询，你可以用它们来加快你的学习。我还给出 SQL 脚本来创建测试数据。因此，您可以使用它们来创建一个测试数据库和表。

我已经讨论了大多数 SQL 查询问题。

# 让我们开始学习 SQL 吧。
* * * * * * * * * * * * * * * * * * * * * * * *

准备示例数据以练习 SQL 技能。
= = = = = = = = = = = = = = = = = = = = = = = = =
样表→员工
Employee _ ID FIRST _ NAME LAST _ NAME 薪资加盟 _ 日期部门
001 莫妮卡·阿罗拉 100000 2014–02–20 09:00:00 HR
002 尼哈里卡·维尔马 80000 2014–06–11 09:00:00 行政
003 维沙尔·辛哈尔 30000000 →奖金
员工 _REF_ID 奖金 _ 日期奖金 _ 金额
1 2016–02–20 00:00:00 5000
2 2016–06–11 00:00:00 3000
3 2016–02–20 00:00:00 4000
1 2016–02–20 00:00:00 4500【17 要准备示例数据，可以在数据库查询执行器或 SQL 命令行中运行以下查询。 我已经用 MySQL Server 5.7 测试过了。

用于种子样本数据的 SQL 脚本。
====================
创建数据库组织；
显示数据库；
使用 ORG

创建表 Employee(
Employee _ ID INT NOT NULL 主键 AUTO_INCREMENT，
FIRST_NAME CHAR(25)，
LAST_NAME CHAR(25)，
SALARY INT(15)，
JOINING_DATE DATETIME，
DEPARTMENT CHAR(25)
)；

插入到员工
(员工 ID，名字，姓氏，薪水，加入日期，部门)值
(001，'莫尼卡'，'阿罗拉'，100000，' 14–02–20 09 . 00 . 00 '，' HR '，
(002，'尼哈里卡'，'维尔马'，80000，' 14–06–11 09 . 00 . 00 '，'管理')，
(003，'维沙尔'，'辛哈尔)

创建表 Bonus (
Employee_REF_ID INT，
BONUS_AMOUNT INT(10)，
BONUS_DATE DATETIME，
外键(Employee_REF_ID)
在删除级联上引用 Employee(Employee _ ID)

)；

插入奖金值
(Employee_REF_ID，BONUS_AMOUNT，Bonus _ DATE)
(001，5000，' 16–02–20 ')
(002，3000，' 16–06–11 ')
(003，4000，' 16–02–20 ')
(001，4500，' 16–02–20 ')
(002
创建表标题(
Employee_REF_ID INT，
Employee_TITLE CHAR(25)，
AFFECTED_FROM DATETIME，
外键(Employee_REF_ID)
在删除级联上引用 Employee(Employee _ ID)
)；

插入到标题
(Employee_REF_ID，Employee_TITLE，AFFECTED_FROM)值
(001，'经理'，
(002，'主管'，【2016–06–11 00:00:00 ')，
(008，'主管'，【2016–06–11 00:00 ')，
(005，'经理'，

一旦上面的 SQL 运行，您将看到类似下面的结果。

Q-1。编写一个 SQL 查询，使用别名<employee_name>从 Employee 表中提取“FIRST_NAME”。
Ans。</employee_name>

所需的查询是:

从员工中选择名字作为员工名；

Q-2。编写一个 SQL 查询，从 EMPLOYEE 表中获取大写的“FIRST_NAME”。
Ans。

所需的查询是:

从员工中选择 upper(FIRST_NAME );

Q-3。编写一个 SQL 查询，从 EMPLOYEE 表中获取 DEPARTMENT 的唯一值。
Ans。

所需的查询是:

从员工中选择不同的部门；

Q-4。编写一个 SQL 查询来打印 EMPLOYEE 表中 FIRST_NAME 的前三个字符。
Ans。

所需的查询是:

从雇员中选择子串(名字，1，3)。

Q-5。编写一个 SQL 查询，从 EMPLOYEE 表中查找名字列“Amitabh”中字母表(“a”)的位置。
Ans。

所需的查询是:

Select INSTR(FIRST_NAME，BINARY'a') from EMPLOYEE，其中 FIRST _ NAME = ' Amitabh
注意事项。

默认情况下，INSTR 方法区分大小写。
使用二元运算符将使 INSTR 作为区分大小写的函数工作。

Q-6。编写一个 SQL 查询，在删除右侧的空格后打印 EMPLOYEE 表中的 FIRST_NAME。
Ans。

所需的查询是:

从员工中选择 RTRIM(名字);

Q-7。编写一个 SQL 查询来打印 EMPLOYEE 表中的 DEPARTMENT，删除左边的空格。
Ans。

所需的查询是:

从员工中选择 LTRIM(部门)；

Q-8。编写一个 SQL 查询，从 EMPLOYEE 表中获取 DEPARTMENT 的唯一值，并打印其长度。
Ans。

所需的查询是:

从员工中选择不同的长度(部门)；

Q-9。编写一个 SQL 查询，在用“A”替换“A”后，打印 EMPLOYEE 表中的 FIRST_NAME。
Ans。

所需的查询是:

Select REPLACE(名字，' A '，' A ')from EMPLOYEE；

Q-10。编写一个 SQL 查询，将 EMPLOYEE 表中的 FIRST_NAME 和 LAST_NAME 打印到一列 COMPLETE_NAME 中。应该用空格符将它们分开。
Ans。

所需的查询是:

Select CONCAT(FIRST_NAME，' '，LAST_NAME)作为员工的“完整姓名”。

Q-11。编写一个 SQL 查询，按名字升序打印 EMPLOYEE 表中所有雇员的详细信息。
Ans。

所需的查询是:

select * from EMPLOYEE order by FIRST _ NAME ASC；

Q-12。编写一个 SQL 查询，按名字升序和部门降序打印 EMPLOYEE 表中所有雇员的详细信息。
俺们。

所需的查询是:

Select * from 员工 order by FIRST_NAME asc，部门 desc。

Q-13。编写一个 SQL 查询来打印雇员表中名字为“Vipul”和“Satish”的雇员的详细信息。
Ans。

所需的查询是:

select * from EMPLOYEE where FIRST _ NAME in(' Vipul '，' Satish ')；

Q-14。编写一个 SQL 查询来打印雇员的详细信息，不包括雇员表中的名字、“Vipul”和“Satish”。
俺们。

所需的查询是:

select * from FIRST _ NAME 不在其中的员工(' Vipul '，' Satish ')；

Q-15。编写一个 SQL 查询来打印部门名称为“Admin”的雇员的详细信息。
Ans。

所需的查询是:

select * from EMPLOYEE where DEPARTMENT like ' Admin % '；

Q-16。编写一个 SQL 查询来打印名字中包含“a”的雇员的详细信息。
Ans。

所需的查询是:

select * from EMPLOYEE where FIRST _ NAME like“% a %”；

Q-17。编写一个 SQL 查询来打印名字以“a”结尾的雇员的详细信息。
Ans。

所需的查询是:

select * from EMPLOYEE where FIRST _ NAME like“% a”；

Q-18。编写一个 SQL 查询来打印名字以“h”结尾并包含六个字母的雇员的详细信息。
Ans。

所需的查询是:

select * from EMPLOYEE where FIRST _ NAME like ' _ _ _ _ _ h '；

Q-19。编写一个 SQL 查询来打印工资在 100000 到 500000 之间的雇员的详细信息。
Ans。

所需的查询是:

Select * from 工资在 100000 到 500000 之间的员工；

Q-20。编写一个 SQL 查询来打印 2014 年 2 月加入的员工的详细信息。
Ans。

所需的查询是:

Select * from 雇员，其中年(加入日期)= 2014，月(加入日期)= 2；

Q-21。编写一个 SQL 查询来获取在部门“Admin”中工作的员工数。
Ans。

所需的查询是:

SELECT COUNT(*)FROM EMPLOYEE WHERE DEPARTMENT = ' Admin '；

Q-22。编写一个 SQL 查询来获取薪水> = 50000 且回答为<= 100000.
的雇员姓名。

所需的查询是:

SELECT CONCAT(FIRST_NAME，' '，LAST_NAME)作为 EMPLOYEE_Name，Salary
FROM EMPLOYEE
WHERE EMPLOYEE _ ID IN
(SELECT EMPLOYEE _ ID FROM EMPLOYEE
WHERE Salary 介于 EN 50000 和 100000 之间)；
Q-23。编写一个 SQL 查询，以降序获取每个部门的雇员人数。
Ans。

所需的查询是:

SELECT 部门，count(EMPLOYEE_ID)员工数量
从员工
分组按部门
排序按员工数量 desc；
Q-24。编写一个 SQL 查询来打印同时也是经理的雇员的详细信息。
俺们。

所需的查询是:

SELECT DISTINCT W.FIRST_NAME，T . EMPLOYEE _ TITLE
FROM EMPLOYEE W
INNER JOIN TITLE T
ON W . EMPLOYEE _ ID = T . EMPLOYEE _ REF _ ID
AND T . EMPLOYEE _ TITLE in(' Manager ')；

Q-25。编写一个 SQL 查询来获取在表的某些字段中有匹配数据的重复记录。
Ans。

所需的查询是:

SELECT EMPLOYEE_TITLE，AFFECTED_FROM，COUNT(*)
FROM TITLE
GROUP BY EMPLOYEE _ TITLE，AFFECTED_FROM
有 COUNT(*)>1；
Q-26。编写一个 SQL 查询，只显示表中的奇数行。
Ans。

所需的查询是:

SELECT * FROM EMPLOYEE WHERE MOD(EMPLOYEE _ ID，2)<>0；

Q-27。编写一个 SQL 查询，只显示表中的偶数行。
Ans。

所需的查询是:

SELECT * FROM EMPLOYEE WHERE MOD(EMPLOYEE _ ID，2)= 0；

Q-28。编写一个 SQL 查询来显示当前日期和时间。
Ans。

以下 MySQL 查询返回当前日期:

SELECT CURDATE()；
以下 MySQL 查询返回当前日期和时间:

立即选择()；
以下 SQL Server 查询返回当前日期和时间:

Q-29。编写一个 SQL 查询来显示一个表的前 n 条(比如 10 条)记录。
答案。

以下 MySQL 查询将使用 LIMIT 方法返回前 n 条记录:

SELECT * FROM 员工 ORDER BY 薪金 DESC 限制 10；
以下 SQL Server 查询将使用 top 命令返回前 n 条记录:

Q-30。编写一个 SQL 查询，从一个表中获取三个最大工资。
答案。

所需的查询是:

SELECT distinct Salary from EMPLOYEE a WHERE 3 > =(SELECT count(distinct Salary from EMPLOYEE b，WHERE a . Salary < = b . Salary)order by a . Salary desc；

祝你学习愉快。