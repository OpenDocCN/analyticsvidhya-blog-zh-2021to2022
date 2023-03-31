# SAS 宏|高效使用 SAS 宏的快速说明

> 原文：<https://medium.com/analytics-vidhya/sas-macros-quick-notes-on-sas-macros-for-efficient-usage-ade670ee7b1?source=collection_archive---------27----------------------->

# 介绍

宏变量的名称以&号为前缀，而宏的名称以百分号(%)为前缀。宏变量类似于标准数据变量，只是它只有一个值，不属于数据集，并且它的值总是一个字符。这个值可以是一个变量名，一个数字，或者任何你想在程序中替换的文本。

如果您反复编写相同或相似的 SAS 语句，您应该考虑使用宏。宏允许您打包一段无错误的代码，并在单个 SAS 程序或多个 SAS 程序中重复使用。你可以把宏想象成一种三明治。%MACRO 和%MEND 语句就像两片面包。在这些切片之间，您可以放置任何您想要的语句。

SAS 宏是一种基于字符串的语言。

# 2.定义宏变量

%LET 用于定义宏变量。

```
%let macro_var = risk_score;
```

代码中使用&符号引用宏变量。

```
proc means data=file1; var &macro_var.; run;
```

# 3.定义宏编程

%MACRO 用于定义一个宏程序。

```
%macro macro_print; proc print data=file1 (obs=10); run; %mend;
```

代码中使用百分号(%)引用宏程序

```
%macro_print;
```

# 4.用宏变量定义宏编程

```
%macro macro_means (var); proc means data=file1; var &var; run; %mend;
```

引用宏程序时，宏变量在宏中传递。

```
%macro_means(risk_score); %macro_means(risk_score_2);
```

# 5.用具有默认值的宏变量定义宏编程

宏变量是在宏名之后定义的。定义默认值，以便在没有传递值的情况下，选择默认值。

```
%macro macro_means_def (var=risk_score); proc means data=file1; var &var; run; %mend;
```

如果没有传递任何值，则宏变量会选择默认值“risk_score”。

```
%macro_means_def(); %macro_means_def(var=risk_score_2);
```

# 6.自动作为宏变量

SYSDATE 是调用 SAS 的日期。SYSDATE9 是以 DDMMMYYYY 格式调用 SAS 的日期。SYSDAY 是一周中调用 SAS 的那一天。SYSTIME 是调用 SAS 的时间。

```
%put &sysdate; %put &sysdate9; %put &sysday; %put &systime;
```

以下 SAS 宏根据提交的 SAS 语句而变化。SYSLAST 是最近创建的 SAS 数据集的名称。如果某个数据步骤或过程步骤没有正确执行，SYSERR 包含错误代码。

```
%put &syslast; %put &syserr;
```

要验证宏变量的值或在 SAS 日志中写入您自己的消息，可以使用“put”语句。

# 7.STR 函数

它用于在编译期间屏蔽标记，以便宏处理器不会将它们解释为宏级语法。特殊记号和助记符包括:+ — * / IN < LT LE > GT GE = EQ NE ~ NOT ^和| OR

选项 1 —完整语法被屏蔽

```
%let var_means1 = %str(proc means data=file1; var risk_score; run;); &var_means1;
```

选项 2-只屏蔽分号

```
%let var_means2 = proc means data=file1 %str(;) var risk_score %str(;) run %str(;); &var_means2;
```

# 8.在宏前后添加文本

程序员可以将文本直接放在宏变量之前，只要宏变量直接放在与符号(&)之后

```
%macro text_before (var); proc means data=file1; var risk_score&var.; run; %mend; %text_before(); %text_before(_2); %text_before(_3);
```

程序员可以将文本直接放在宏变量之后，只要宏变量直接放在句点()之前的&符号之后。)

```
%macro text_after (var); proc means data=file1; var &var. &var._2 &var._3; run; %mend; %text_after(risk_score);
```

# 9.宏中包含引号

引号是宏变量的一部分。以下代码的输出是— 2020 年为“新冠肺炎”年

```
%let var1 = "COVID-19"; %let var2 = 2020 as &var1 year ; %put &var2;
```

以下代码的输出是— 2021 年是僵尸年

```
%let var1 = ZOMBIE; %let var2 = 2021 as &var1 year ; %put &var2;
```

# 10.双引号与单引号

宏处理器解析双引号内而不是单引号内的宏变量引用。下面代码的输出是——“新年快乐”

```
%let var3 = HAPPY NEW YEAR; %put "&var3";
```

以下代码的输出是—“& var 3”

```
%put '&var3';
```

# 11.在数据步骤中定义宏变量

调用 SYMPUT 用于将数据集变量作为一个值赋给宏变量。它还可以用于在一个数据步骤中创建一系列宏变量。当用于给宏变量赋值时，该函数自动将数值转换为字符值。

```
data _null_; call symput ('yyyy',2021); run; %put &yyyy;
```

# 12.在过程步骤中定义宏变量

程序员可以在 proc sql 的执行步骤中创建或更新宏变量。INTO 子句用于定义宏变量。

```
proc sql noprint; select avg(ext_quality_score) into: avg_score from file2; quit; %put &avg_score;
```

# 13.在 proc sql 中创建多个宏变量

使用 INTO 子句可以在 proc sql 步骤中创建多个宏变量。每个宏变量前面都应该有冒号(:)

```
proc sql noprint; select min(ext_quality_score), avg(ext_quality_score), max(ext_quality_score) into:min_score, :avg_score, :max_score from file2; quit;
```

以下代码的输出是 min_score = 0.00，avg_score = 0.62，max_score = 1.00。

```
%put &min_score; %put &avg_score; %put &max_score;
```

# 14.分解倍数

宏处理器从添加多个&符号的位置开始，从左到右扫描和解析标记，直到无法解析更多的触发器。这也称为间接引用。

```
%let teacher3 = MR D K BOSE; %let n = 3;
```

以下代码的输出是—D . K . BOSE 先生

```
%put &teacher3;
```

以下代码的输出是—D . K . BOSE 先生

```
%put &&teacher&n;
```

# 15.与宏相关的选项

这些选项用于调试 SAS 宏。SYMBOLGEN 在 SAS 日志中打印宏变量的值。MPRINT 在执行宏时将文本发送给编译器，并打印在 SAS 日志中。MLOGIC 打印指示宏执行期间所采取的宏操作的消息。

```
options symbolgen mprint mlogic;
```

# 16.位置参数

程序员需要按照定义宏变量的顺序来定义宏变量的值。

```
%macro macro_means_pos (d, v); proc means data=&d; var &v; run; %mend; %macro_means_pos(file2, ext_quality_score);
```

# 17.参数关键字

程序员必须指定宏变量，后跟等号。宏变量的值的顺序可以是随机的。

```
%macro macro_means_key (d=, v=); proc means data=&d; var &v; run; %mend;
```

下面两条语句给出了相同的输出。

```
%macro_means_key(d=file2, v=ext_quality_score); %macro_means_key(v=ext_quality_score, d=file2);
```

程序员也可以使用混合参数。

# 18.Eval 和 Sysevalf

对具有整数值的宏变量执行数学运算。使用了 EVAL 函数。宏变量' c '的值是 11。

```
%let a = 5; %let b = 6; %let c = %eval(&a+&b); %put &c;
```

对具有浮点值的 SAS 宏变量执行数学运算。使用 SYSEVALF 函数。宏变量' c '的值是 12。

```
%let a = 5.5; %let b = 6.5; %let c = %sysevalf(&a+&b); %put &c;
```