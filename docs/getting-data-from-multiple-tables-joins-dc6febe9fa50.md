# 从多个表中获取数据(连接)

> 原文：<https://medium.com/analytics-vidhya/getting-data-from-multiple-tables-joins-dc6febe9fa50?source=collection_archive---------15----------------------->

加入:

*   JOIN 是 MYSQL 中的一个子句，它帮助从多个表中检索数据
*   联接是在称为键的特定列上完成的

左连接:

*   使用左边的表，并用相应的变量连接右边的表
*   连接表时大约使用 90%的时间
*   右表中与左表不对应的任何变量都不会被合并

```
mysql> select <table1>.a, <table1>.b, <table2>.c from <table1> left join <table2> on <table1>.<key1> = <table2><key2>;
```

*   a、b、c 是要从每个表中显示的列
*   键 1 和 2 是组合表的公共变量

内部联接:

*   仅用匹配的变量组合两个表
*   连接表时使用了将近 10%的时间

```
mysql> select a,b,c from <table1> inner join <table2> on <table1>.<key1> = <table2>.<key2>;
```

右连接:

*   使用右边的表，并用相应的变量连接左边的表
*   很少使用

```
mysql> select a,b,c from <table1> right join <table2> on <table1>.<key1> = <table2>.<key2>;
```

笛卡尔/交叉连接:

*   组合两个表中的所有内容
*   很少使用

```
mysql> select a,b,c from <table1> cross join <table2>;
```