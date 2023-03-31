# C++中的作用域，教程？

> 原文：<https://medium.com/analytics-vidhya/scope-in-c-a-tutorial-6e8c9d9db447?source=collection_archive---------19----------------------->

# 什么是范围？

作用域 ***是一个标识符被声明为*** 的地方，它可以被认为包含了一个标识符列表。在`C++`中有多个作用域，因此有多个地方可以声明标识符。

一个 ***标识符在同一个作用域内*** 只能定义一次。`C++`中的标识符只是一个名称，例如，可以给定一个名称空间、一个类、一个函数、一个变量...在不同的作用域中，可以定义同名的标识符。

***有六个范围*** ，在`C++`。它们是，文件作用域也称为全局作用域；命名空间范围、类范围、块范围，也称为局部范围；函数范围和原型范围。所以在这些作用域中，可以声明标识符。范围可以相互嵌套。

***名称查找*** ，是在不同的可访问范围内搜索一个标识符的行为。

# 文件范围或全局范围

文件作用域，或全局作用域，也称为 ***翻译单元作用域*** 。翻译单元是在执行了预处理器指令(如`#include`或`#define`)后得到的源代码。

在翻译单元作用域或文件作用域 ***中声明的标识符在任何*** *其他*作用域之外声明。

```
/* file :  myMath.h */int add(int a , int b ){
  return a + b ;}float add(float a, float b){
  return a + b ;}
/* add , is an identifier declared in 
   the file scope , for a function , 
   which is overloaded .*//* file :  myProg.cpp */#include "myMath.h" 
/*Includes the file 
  myMath.h .
  After preprocessing , 
  a single translation unit ,
  or source code is created . 
  The global identifiers are : 
  add , var_i , var1_i , 
  var_f , var1_f , main .*/int var_i  = 0;
int var1_i = 1;float var_f = 1.0f;
float var1_f = 3.0f;int main(void ){
  int var1_i = 3;
  int result_i = add(var_i , var1_i );
  /*add var_i which is equal to 0 , with
    var1_i , which is equal to 3  ,
    the result 3 , is stored , in result_i .*/float var1_f = -1.0f;
  float result_f = add(var_f , ::var1_f );
  /*add var_f which has a value of 1.0f ,
    to the variable  qualified 
    by the global scope var1_f .
    ::var1_f has a value of 3.0f . 
    The result is 4.0f , and is stored in 
    var1_f .*/ }
```

在全局作用域中声明的标识符 ***是可见的，可以从声明后的点开始从全局作用域或任何嵌套作用域中访问*** 。

例如，在前面的例子中，`var_i`在`void )`后的花括号所限定的范围内是可见的，因此在调用函数`add`时使用了它的值。

通常，父作用域中声明的标识符在嵌套作用域中是可见的。在名称查找中，首先搜索嵌套范围，同时也搜索父范围。 ***名称查找从使用标识符的作用域开始*** 。

所以在前面的例子中，`var1_i`的值是`3`，因为`var1_i`被用在`void )` 后的花括号所限定的范围内，在这里执行对`add`函数的调用，因此名字查找在这个范围内开始。

可以使用范围解析运算符`::`指定 ***在哪里执行名称查找*** 。当标识符由作用域限定时，仅在限定的作用域中搜索该标识符。

因此，在前面的例子中，`::var1_f`用全局作用域限定，在全局作用域中搜索标识符 var 1 _ f。`::var1_f`的值为`3.0f`。

# 命名空间范围

一个 ***命名空间被用来*** 创建作用域。作用域包含标识符声明，可以防止名称冲突。可以在不同的作用域中定义同名的标识符。范围定义可以扩展。

```
int xid_i = 1 ;namespace OTHER_ID{
  int xid_i = 5 ;}namespace OTHER_ID{
  int zid_i = 5 ;}float tax(float money ){
  return money * 0.15f ;}namespace irregular{
  float tax(float money ){
    return money * 0.20f ;}}namespace discounted{
  float tax(float money ){
    return money * 0.10f ;}}namespace {
  char d_c = '1';
  char l_c = 'u'; }int main(void ){char var_c = ::d_c;
  /*The identifier d_c is
    preceded with the global 
    scope operator :: .
    d_c is declared in an unnamed 
    namespace . Identifiers in an
    unnamed namespace belong as
    accessible to the global scope . 
    Hence the value , of the 
    variable var_c , is the 
    character '1' .*/var_c  = l_c ; 
  /*The identifier l_c is used in
    the current scope , a lookup
    for the identifier l_c takes
    place in the current scope .
    It is not found , hence the enclosing
    scope , which is the global scope ,
    is searched . l_c is declared in
    an unnamed namespace , as such it
    belongs to the file scope as
    accessible , hence the value of
    the variable var_c is
    the character 'u' .*/tax(20 );
  /*tax is called without using the
    scope operator . Hence  the scope
    where the tax identifier is used ,
    is first searched , and after that
    other accessible scopes . 
    The tax function is defined in the global, 
    scope , hence the returned value is 
    3.0f .*/irregular::tax(20 );
  /*tax is qualified , using the scope 
    operator :: by the namespace irregular , 
    the tax identifier is searched in ,
    the namespace irregular . The 
    tax function defined in irregular , 
    is called . The result is 4.0f .*/using irregular::tax ; 
  tax(20 );
  /*The using declaration , 
    using irregular::tax is used .
    The using declaration , adds the 
    tax identifier , to the current 
    scope . An identifier  with the same 
    name ,  must not exist in  the current
    scope , or else this will 
    cause an error .
    tax(20 ) is called , and the 
    scope operator  :: is not used , 
    hence the current scope , is 
    searched for the tax identifier . 
    The tax identifier is found in the 
    current scope, this is the identifier 
    added from the namespace irregular , 
    hence the tax function from irregular , 
    is called . The result is 4.0f .*/using namespace discounted;
  tax(20 );
  /*The using directive , using namespace , 
    does not add the identifiers 
    in the namespace discounted , 
    to the current scope . They 
    are only  made accessible . When
    calling tax , the current scope 
    is searched for the identifier tax .
    The identifier tax is added , 
    by the using declaration , 
    using irregular::tax , precedingly , 
    to the current scope , hence what is
    called is the  tax function from , 
    the irregular namespace , which 
    returns 4.0f , and not 2.0f .*/using namespace OTHER_ID;
  int varID_i = xid_i ;
  /*This statement will cause , 
    a compiler error to occur . 
    The using directive , using namespace 
    , makes accessible all the identifiers 
    in the namespace OTHER_ID , in the 
    current scope .  The identifiers , 
    from the global scope , are also accessible
    from the current scope . Hence ambiguity ,
    to which identifier is meant arise .*/ }
```

# 类别范围

标识符声明在一个类中，有 ***类作用域*** 。作用域操作符`::`可以用来访问作用域的成员。当与类一起使用时，它可用于访问或初始化静态变量。它也可以用来访问一个类的方法，以便提供一个定义。

```
#include<iostream>class Foo{
  /*Declare The class
    Foo .*/
public :
  int fct();
  /*Declare Foo's public
    method fct .*/
private :
  static int var;
  /*Declare Foo's private
    static variable var .*/};int Foo::var = -1;
/*Initialize Foo private
  static variable var .*/int Foo::fct(){
  /*Define Foo function fct .*/
  return var;}int main(void ){
  Foo foo;
  std::cout << foo.fct() << std::endl;
  /*Output :
   -1 . */}
```

# 本地范围

一个局部作用域，以花括号`{}`开始和结束。 ***局部范围*** 可以嵌套。定义函数时，函数参数是紧接在其参数列表之后的局部范围的一部分。for 循环中的声明也是紧跟在 for 循环之后的局部范围的一部分。

```
#include<iostream>int add(int x , int y );
/*x , y belong to the function ,
  prototype scope .*/int main(){
  std::cout << add(1 , 3 ) << std::endl;
  std::cout << add(3 , 1 ) << std::endl ;
/*Output :
20
2 .*/}int add(int x , int y ){
  int z = 10;
  /*x , y , z belong to the 
    local scope created after
    the function parameters 
    list .*/
  if(x < y ){
  /*The curly brackets create a 
    nested local scope , 
    z belongs to the newly 
    create nested local
    scope .*/
     int z = 20 ; 
     return z ;
     /*Returns 20 .*/}
  else {
    int i = 0 ;
    /*i Belongs to another newly 
      create nested local
     scope.*/
    for(int i = 2 ; i < 4 ; i ++ )
    /*Creates another local scope , 
      i has a starting value of 2 .*/ 
      return i; /*returns 2 */ }}
```

# 功能范围

***标签声明在一个函数内*** *，*有一个函数作用域。这些是仅有的具有功能范围的实体。

```
#include<iostream>
void square_cube( int n ){
  int i = 0 ;
 loop:{
    if( i == n )
      return ;
    else if(i % 2  == 0 )
      goto powerTwo;
    else
      goto powerThree;powerTwo :
    std::cout << i * i << std::endl;
    i++;
    goto loop;
  powerThree:
    std::cout << i * i * i << std::endl;
    i++;
    goto loop ;}}int main(void ){
  square_cube(5 ) ;}
/*output 
0
1
4
27
16*/
```

# 原型范围

原型声明 中声明 ***的参数有原型作用域。***

```
bool fit(float weight);
/*weight , has prototype scope .*/
```

# 名称查找是如何发生的？

除非使用范围操作符`::`限定，否则在使用范围内，通过 ***可访问范围*** 搜索标识符的起始点，如前几节所述。如果合格，则在合格范围中搜索标识符。

***除了*** 之外，对于不合格的函数，如果在使用该函数的作用域或其他可访问的作用域中找不到该函数的名称，则进行参数相关的名称查找，也称为 Koenig 查找。

基本上，在 ***类型的参数*** 的范围内搜索函数声明。

```
#include<iostream>namespace nmspcFoo{
  class ClassFoo{}; void printScopeName(ClassFoo arg ){
    std::cout << "nmspcFoo" << std::endl; }}int main(void ){
  nmspcFoo::ClassFoo fooVar ;
  printScopeName(fooVar ) ;}
/*Output 
nmspcFoo .*/
```

*原载于 2021 年 1 月 5 日 https://twiserandom.com**的* [*。*](https://twiserandom.com/cpp/scope-in-cpp-a-tutorial/)