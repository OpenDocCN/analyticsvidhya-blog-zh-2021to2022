# 类型限定符:const，volatile，在 C++教程中

> 原文：<https://medium.com/analytics-vidhya/type-qualifiers-const-volatile-in-c-a-tutorial-d2d61a0d83a1?source=collection_archive---------28----------------------->

c++ ***中对象的声明具有以下形式*** :

```
[Storage class] [Qualifier] Type Name ;/*brackets means optional  .*/
```

对于存储类，您可以查看本[教程](https://twiserandom.com/cpp/storage-classes-in-c-a-tutorial/)，至于限定符，它们用于限定一个定义，作为常量定义:`const`，或者可变定义:`volatile`，或者两者都是`const volatile`定义。

`const`和`volatile` ***可以与*** 变量、函数参数和返回值、类类型数据成员和函数成员一起使用。

# 常数

`const`表示不可改变， ***提供定义后，*** 定义不变。常量变量必须初始化。

一个 ***带有变量*** 的例子。

```
int var_i;
/*var_i is not a constant 
  variable , its definition 
  can be changed .*/
var_i = 1;int var1_i;
/*var1_i  is not a constant 
  variable , its definition can 
  be changed .*/
var1_i = 1;const int var_ci = -1 ;
/*var_ci  is a constant 
  variable , its definition 
  cannot change . 
  var_ci can also be declared 
  int const var_ci = -1 ; */int const *ptr_ci = &var_i;
/*ptr_ci is a pointer to 
  a constant integer , 
  it is illegal to use
  *ptr_ci = 4 ; 
  to change the value of 
  the pointed to integer .*/int *const cptr_i = &var_i;
/*cptr_i is a constant pointer
  to an integer . The value 
  of cptr_i cannot be changed .
  It is illegal to do :
  cptr_i = &var1_i; .*/const int *const cptr_ci = &var_i;
/*cptr_ci is a constant pointer 
  to a constant integer . The
  value of the pointer and the 
  pointed value cannot be changed .
  It is illegal to do :
  cptr_ci = &var1_i;
  *cptr_ci = 34; .*/
```

一个 ***带函数参数的例子。***

```
const int * foo(const int *ptr_ci ){
    /*ptr_ci is a pointer to a
      constant integer , the value
      of the constant integer must
      not be changed .
      The function foo , perform
      an operation , that does not
      affect original data .*/ static int arr_i[ ] = {0 , 1 , 2 , 3 };
      /*Declare a static array arr_i.*/ return &arr_i[1];
      /*Return the address of the
      second element of the array .*/ }
```

一个 ***的例子，有类*** 的数据成员，和类成员函数。

```
class Foo{
public:
  const int var_i;
  float var_f;
  mutable double var_d;
  /*A mutable class data member ,
    can be changed , even
    if its class instance is
    declared constant .*/
  Foo( );
  void cFct( )const; };Foo::Foo():var_i(0 ) , var_f(-1.f ) , var_d(0.4 ){}void Foo::cFct( )const{
  /*A constant function can access
    all data members , but must not
    change the value of any , beside
    a data member which is declared
    mutable .*/
  var_d = var_d + var_f + var_i; }int main(void ){
  Foo foo;
  /*Create an object named foo ,
    It is illegal to do
    foo.var_i = -1 ; 
    because var_i is declared
    constant .*/ foo.var_d = foo.var_i + foo.var_f + foo.var_d ;
  /*Assign a value to var_d .*/ foo.cFct();
  /*Call the constant function cFct .*/ const Foo foo_c;
  /*Create an object named foo_c.
    foo_c is declared constant ,
    it is allowed only to change
    the value of a mutable data
    member.
    It is illegal to do :
      foo_c.var_f = 4.f ; .*/ foo_c.var_d = foo.var_i + foo.var_f + foo.var_d ;
  /*Assign a value to var_d , which is
   a mutable data member .*/
  foo_c.cFct();
  /*A constant object is only 
    allowed to call constant 
    functions .*/ }
```

# 不稳定的

一个`volatile`限定符，用来通知编译器，那一个 ***变量的定义，可以*** 异步地改变到程序中。所以在某种程度上，这与程序本身所做的改变无关，因此变量的改变可以由外部进程来完成。

当使用 volatile 限定符时，编译器 ***不做某些优化*** ，这依赖于变量定义的变化与程序本身同步的事实。当一个变量被访问时，即使程序本身没有改变它的值，变量值也要被重新读取。

***非静态的成员函数*** 可以声明为 volatile，如果一个类的实例是 volatile，那么它只能访问 volatile 成员函数。

```
class Foo{
public:
  volatile int var_vi;
  float var_f;
  Foo();
  void fct_v( )volatile;
  void fct();
};Foo::Foo( ):var_vi(1 ) , var_f(-3.f ){}void Foo::fct_v( )volatile{
  var_vi = var_vi + var_f; }void Foo::fct( ){
  var_vi = var_vi + var_f; }int main(void ){
  Foo foo;
  /*Create an instance of foo .*/
  foo.var_vi = -1;
  /*Set a value to the volatile
    data member .*/
  foo.fct_v();
  /*Call the volatile function .*/
  foo.fct();
  /*Call the non volatile function .*/ volatile Foo foo_v;
  /*Create a volatile instance of
    Foo .
    Each data member of foo_v
    is volatile . foo_v can
    only access functions declared
    as volatile .*/
  foo_v.var_f = 3.1f;
  /*Set a value to var_f which 
    is not declared volatile .*/
  foo_v.fct_v();
  /*Call the volatile function 
    fct_v .*/ }
```

`const` ***可以和*** `volatile`一起使用，表示一个变量相对于程序本身的不变性，但是这个变量从程序外部被改变的能力。

```
const volatile int var_ci = 0 ;
```

*最初发表于 2021 年 1 月 11 日*[*https://twiserandom.com*](https://twiserandom.com/cpp/type-qualifiers-const-volatile-in-cpp-a-tutorial/)*。*