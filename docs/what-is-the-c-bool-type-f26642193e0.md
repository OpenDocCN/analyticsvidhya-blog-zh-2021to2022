# 什么是 c++ bool 类型？

> 原文：<https://medium.com/analytics-vidhya/what-is-the-c-bool-type-f26642193e0?source=collection_archive---------16----------------------->

`bool`类型是 c++ ***中的一种基础数据类型*** ，一种基础数据类型有一个 cpu 硬件映射。

`bool`类型用于布尔值。一个 ***布尔值*** *是*一个关系运算的结果，比如比较是否相等`==`，或者一个逻辑运算的结果，比如 or `||`。

布尔运算的结果可以是真或假，因此 C++引入了两个关键字:`true`和`false`。C++中的`true`和`false`，是布尔文字*。*

*C++标准规定，`bool`、**、*类型必须与*、**、*、*类型具有相同的大小、对齐和位表示，如同无符号整数类型。通常，C++实现将`bool`类型定义为 1 字节大小，但这并不是标准的强制要求。*

```
*#include<iostream>int main(void ){
  std::cout << sizeof(bool ) << " byte ." << std::endl ;
  /*Print the size of the bool type , on 
    this machine . Output : 
    1 byte .*/ }*
```

*当执行 ***流控制语句*** 时，如`if`或`while`，被求值的条件表达式有一个结果类型`bool`。唯一的例外是 switch 语句，它有一个结果整型。*

*整数类型如`int`，浮点类型如`float`，指针，枚举，都可以 ***转换成布尔值*** ，其中`0`或空指针为假，其他都为真。*

```
*#include<iostream>int main(void ){
  enum state{halt , run };
  /*Define the enum state , which 
    has two state , halt which is 0 , 
    and run which is 1 .*/int var_i = 1 ;
  int *ptr_i = nullptr;if(!ptr_i && run )
    /*if ptr_i , is the null pointer ,
      , ptr_i evaluates to false ,
      and not ptr_i evaluates to
      true . 
      run has a value of 1 , as such 
      it evaluates to true . 
      Hence if ptr_is the
      null pointer and the state
      is run , ptr_i is initialized
      with the address of var_i .*/
    ptr_i = &var_i ;while(ptr_i && *ptr_i ){
    /*ptr_i evaluates to false ,
      if ptr_i is the null pointer,
      and to true otherwise , *ptr_i
      evaluates to true if different from
      0 , and to false otherwise .
      The while loop will continue , as long
      as ptr_i is not null , and *ptr_i is not
      0 .*/
    std::cout << *ptr_i << std::endl ;
    *ptr_i = *ptr_i - 1 ; }
  /*Output :
    1 .*/ }*
```

*当表达式的类型为整数类型时，可以将`bool`类型 ***转换为整数*** 类型，其中 true 提升为`1`，false 提升为`0`。*

```
*#include<iostream>int main(void ){switch(true ){
    /*In a switch statement , the conditional
      expression is of an integer type , hence
      true is converted to 1 .*/
    case 0 :
      std::cout << false << " : " << std::boolalpha << false ;
      break;
    case 1 :
      /*When converted to an integer type , true evaluates
        to 1 , and false , to 0 .
        case 1 , is the true case , hence it is the one ,
        that is executed .*/
      std::cout << true << " : "<<std::boolalpha << true <<std::endl;
      /*cout by default prints the integer value
        of a boolean type , so first it will
        print the integer value of true which is 1.
        When passed the option boolalpha , it will
        print the alpha value of the boolean type ,
        hence next it prints true .
        Output :
        1 : true .*/
      break; } }*
```

****一个类类型:*** `class`，`struct`，`union`，如果提供了运算符转换函数，可以求值为布尔型，如果提供了逻辑和关系运算符函数，可以执行关系和逻辑运算。*

```
*#include<iostream>class Foo{
  /*Class Foo Definition .*/
  public :
    int var_i;
    bool operator==(const Foo foo );
    bool operator!=(const Foo foo );
    bool operator&&(const Foo foo );
    operator bool( ); };bool Foo::operator==(const Foo foo ){
  /*Definition of the operator == 
    function , that compares two 
    instances of Foo
    for equality .*/
  return var_i == foo.var_i ;}bool Foo::operator!=(const Foo foo ){
  /*Definition of the operator != 
    function , which compares two 
    instances of Foo for 
    difference .*/
  return var_i != foo.var_i ;}bool Foo::operator&&(const Foo foo ){
  /*Definition of operator && 
    function , which implements 
    the and logical operation 
    between two instance of 
    Foo .*/
  switch(var_i - foo.var_i ){
    /*subtract var_i from foo.var_i
      and return false , if the 
      result of the subtraction 
      is 0 , 1 otherwise .*/
    case 0 :
      return false;
    default:
      return true; } }Foo::operator bool(){
  /*Definition of the operator
    conversion to a bool data 
    type function , which 
    the result of its evaluation
    is a bool type .*/
  switch(var_i ) {
      /*If var_i is equal to 0 , 
        return false , otherwise
        return true .*/
      case 0 :
        return false ;
      default :
        return true; } }int main(void ){
  /*The main function .*/Foo var1_foo{-1 };
  Foo var2_foo{0 };
  /*Define two objects var1_foo ,
    and var2_foo , initializing 
    each to -1 and 0 respectively .*/if(var1_foo == var2_foo )
    /*Compare var1_foo and var2_foo for equality .
      var1_foo operator function equal is called ,
      being passed var2_foo as an argument .
      Their var_i data members are not equal , 
      hence the operator function == returns
      false , the body of the if statement 
      is not executed .*/
    std::cout << "var1_foo is equal  var2_foo ." << std::endl ;if(var1_foo != var2_foo )
    /*Compare var1_foo and var2_foo for difference .
      The operator function != of var1_foo is called
      , being passed var2_foo as an argument . 
      Their var_i data members are different , 
      hence the result of the operator function 
      != is true , hence the body of the if 
      statement is executed .*/
    std::cout << "var1_foo is different from  var2_foo ." << std::endl ;
    /*Output :
      var1_foo is different from  var2_foo .*/if(var1_foo )
    /*var1_foo is evaluated as a boolean , 
      since the destination type for an
      if statement is a boolean type .
      var1_foo is converted to a boolean , 
      by calling its operator conversion 
      function bool , which switches 
      the var_i data member , and return 
      true if its different from 0 , 
      false otherwise .*/
    std::cout << "var1_foo is true ." << std::endl ;
    /*Output :
      var1_foo is true .*/if(!var2_foo )
    /*var2_foo is evaluated as a boolean , 
      hence it is converted to a boolean ,   
      since the destination type of an
      if statement is the bool type .
      var2_foo conversion operator function  
      bool is called , and it returns a 
      bool , which is true if the 
      var_i data member is different from 
      0 , and false otherwise .*/
    std::cout << "var2_foo is false ." << std::endl ;
    /*Output :
      var2_foo is false .*/if(var2_foo && var1_foo )
    /*var2_foo operator function && is 
      called , it is passed var1_foo 
      as an argument .
      The operator function && subtract
      var2_foo var_i data member from 
      var1_foo var_i data member : 
      0 - (-1 ) is equal
      to 1 , hence the operator function
      && returns a true , as such the 
      body of the if statement is executed .*/
    std::cout<< "var1_foo && var2_foo is true ." <<std::endl; 
    /*Output :
      var1_foo && var2_foo is true .*/ }*
```

*`bool`类型可以用作谓词函数 的 ***返回类型，这些函数必须返回`true`或`false`。****

```
*bool isCppStandard_Compliant(){
  return true ;}*
```

**原载于 2021 年 1 月 20 日 https://twiserandom.com**[*。*](https://twiserandom.com/cpp/what-is-the-cpp-bool-type/)**