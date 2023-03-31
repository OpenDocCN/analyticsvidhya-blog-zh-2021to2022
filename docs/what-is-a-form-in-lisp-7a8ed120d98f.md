# lisp 中的表单是什么？

> 原文：<https://medium.com/analytics-vidhya/what-is-a-form-in-lisp-7a8ed120d98f?source=collection_archive---------22----------------------->

一个形式就是 ***要被评估*的任何东西。**

Lisp 是由两个东西[原子和 cons](https://twiserandom.com/lisp/what-is-an-atom-a-symbol-a-cons-and-a-list-in-lisp/) 组成的 ***。***

一个`atom` ***是一个*** 而不是一个`cons`，一个`cons`拥有两个引用。

一个`***list***` ***由零个或多个`cons`的*** 组成，其中每个`cons`的第二个元素是对下一个`cons`的引用，最后一个`cons`的最后一个元素或者是对代表空列表`()`的`nil`元素的引用，或者什么都没有，或者最后一个`cons`的最后一个元素是对一个原子的引用。在后一种情况下，这个列表称为点列表。列表的元素被括在左括号`(`和右括号`)`之间

话虽如此，一个表单或 ***在 lisp 中可以被求值的*** ，要么是一个`atom`或一个`cons`，或者更一般地，改为或称一个`cons`，一个`list`。

# 评估原子形式

一个 ***原子形式可以是*** 或者是一个自评估原子，或者是一个符号。

## 自我评估原子

如果形式是一个原子，那么原子可以是一个 ***原子，它评估为自身*** ，因此评估原子形式的值就是原子本身。

例如，当评估一个数字(如`1`)时，它评估为`1`，或者当评估一个字符串(如`"Hello world "`)时，它评估为`"Hello world "`。

```
> 1
;;;evaluating 1 , yields 
;;;1 .
1> "Hello world"
;;;evaluating "Hello world" , 
;;;yields "Hello world" .
"Hello world"
```

## 标志

如果形式是原子，原子也可以是符号。 ***一个符号有*** ，除了别的以外，还有一个名字，一个函数属性，一个值属性。

如果形式只是一个符号，那么 ***所评估的是*** 它的值属性。一些符号对自身求值，例如空列表`()`的`nil`，都是对自身求值的符号。

```
>(symbolp nil )
;;;symbolp checks if the passed
;;;argument is a symbol , it 
;;;returns true , since nil 
;;;is a symbol .
T> nil
;;;nil evaluates to itself 
nil> (set 'ip "192.168.0.1" )
;;;set the value property of 
;;;the symbol ip to the string
;;;192.168.0.1 . ip is quoted
;;;using ' as not to be evaluated .> ip
;;;ip is evaluated , this returns
;;;the attribute value , which is :
"192.168.0.1"
```

# 评估列表表单

如果要评估的是非空列表，因此是包含一些元素的列表，那么这个列表称为 ***复合形式*** 。空单`( )`是一个符号，它自己评价自己。

一个列表*、*、**、*取决于它所拥有的第一个元素*、**，被称为:函数形式、lambda 形式、宏形式或特殊形式

## 什么是函数形式？

***函数执行*** 计算，如加法或正则表达式匹配。

函数形式只是一个非空列表，其中 ***的第一个元素*** 是一个符号，它具有函数属性，定义了一个函数。

如何对 ***函数形式*** 求值，首先是列表的剩余元素，称为列表的`cdr`，从左到右依次求值。一旦他们的评估完成，他们的评估被应用到函数。

***标准没有指定*** ，如果包含函数定义的符号在自变量求值之前、之后或之间求值。

```
> (+ 1 pi )
;;;This is a function form , 
;;;+ is a symbol , which has
;;;its function property defining
;;;a function . 
;;;First the remainder elements of 
;;;the list are evaluated from left 
;;;to right , one after the other . 
;;;1 evaluates to 1 .
;;;pi is a symbol , its value property 
;;;is evaluated , this returns :
;;; 3.1415926535897932385L0 .
;;;The standard does not specify , 
;;;when symbol + is evaluated . 
;;;Evaluating the symbol + , returns
;;;its function definition . 
;;;Once arguments and function definition
;;;evaluation is done , the function 
;;;is called with the evaluated arguments .
;;;The addition of 1 and  
;;; 3.1415926535897932385L0 , is 
;;;equal to 4.1415926535897932383L0 . 
4.1415926535897932383L0
```

要检查符号 ***的功能属性是否属于功能类型*** ，`functionp`都可以使用，这与检查不同，如果符号的功能属性有界，在这种情况下可以使用`fboundp`。

```
>(functionp '+ )
;;;' is used to quote a 
;;;symbol , as not to be
;;;evaluated .
;;;The symbol + is not
;;;of type function , it
;;;is a symbol which has 
;;;some properties . 
NIL>(functionp #'+ )
;;;#' returns the value
;;;of the property : function
;;; of a symbol .
;;;The function property of 
;;;+ is of a function type . 
T>(fboundp '+ )
;;;Checks if the symbol +
;;;has its property : function 
;;;bounded .
T
```

## 什么是 lambda 形式？

lambda 形式是非空列表，其中 ***的第一个元素*** 是 lambda 表达式。

λ表达式是一个函数，它没有名字，通过使用一个列表来定义，第一个元素是符号`lambda`，后面是一个包含函数参数的列表，称为λ列表，λ列表可以是空的，因为函数不接收任何参数。lambda 列表后跟零个或多个可选形式，构成了函数体。如果 lambda 函数没有主体，它只返回`nil`，否则它返回最后一个求值形式的值。

λ形式 的 ***求值，相当于使用`funcall`。这意味着，由于`funcall`是一个函数，它的所有参数首先从左到右被求值，因此 lambda 表达式首先被求值，然后是列表的剩余元素，最后`funcall`将把从 lambda 表达式得到的函数应用于求值的参数，并返回结果。***

```
> ((lambda (x ) (* x x ) ) 2 )
;;;This is a lambda form , since 
;;;the first element of the list
;;;is a lambda expression . 
;;;The lambda expression is first 
;;;evaluated to get its function 
;;;definition .
;;;The second element of the form , 
;;;is 2 , it evaluates to itself , 
;;;which is 2 .
;;;The gotten function is applied 
;;;to the argument 2 , this
;;;returns 4 .
4
```

## 什么是宏表单？

一个函数执行一个计算，比如加法，或者排序一些元素， ***而一个宏函数*** ，返回另一个要求值的形式，换句话说，它返回一个新的程序。

宏表单是一个非空列表，其第一个元素是一个符号，该符号具有其 ***功能属性，定义了*** 宏。

**到*检查如果*的功能属性**为一个符号，定义了一个宏，`macro-function`可以使用。`macro-function`如果符号的属性函数没有定义宏，则返回 nil。

```
> (defmacro sub(var )
   `(- ,var 1 ) )
;;;Define the symbol sub ,
;;;function property with
;;;a macro definition . 
SUB>> (macro-function '+ )
;;;The property function of the
;;;symbol + , is not a macro
;;; definition , hence 
;;; macro-function returns NIL .
NIL> (macro-function 'sub )
;;;The symbol sub function property 
;;;is a macro definition , hence
;;;macro-function does not return
;;;nil . 
#<FUNCTION SUB (SYSTEM::<MACRO-FORM> SYSTEM::<ENV-ARG>)
  (DECLARE (CONS SYSTEM::<MACRO-FORM>)) (DECLARE (IGNORE SYSTEM::<ENV-ARG>))
  (IF (NOT (SYSTEM::LIST-LENGTH-IN-BOUNDS-P SYSTEM::<MACRO-FORM> 2 2 NIL))
   (SYSTEM::MACRO-CALL-ERROR SYSTEM::<MACRO-FORM>)
   (LET* ((VAR (CADR SYSTEM::<MACRO-FORM>))) (BLOCK SUB `(- ,VAR 1))))>
```

一个 ***宏表单被*** 评估，通过首先评估宏，并扩展它，替换宏表单。参数作为扩展形式的一部分进行评估。

```
> (defmacro sub(var )
    `(- ,var 1 ) )
;;;Defines the function property
;;;of the symbol sub , as being 
;;;a macro defintion .> (sub pi)
;;;sub function property , is a macro
;;;definition , hence what happens first,
;;;is that sub is expanded , and the form
;;;is replaced , by sub expansion .
;;;pi as a symbol , is not evaluated , 
;;;first , it is only evaluated ,
;;;when evaluating the replacement 
;;;form , which is now :
;;;(- pi 1 )
;;;This is a function form , 
;;;the cdr of the list , 
;;;which is its remainder , 
;;;is evaluated . 
;;; pi is evaluated to get 
;;;3.1415926535897932385L0
;;;1 is evaluated to 1 .
;;;The standard does not specify ,
;;;when the symbol - is evaluated , 
;;;to get its function definition .
;;;It can be , before , after , or
;;;in between evaluating the arguments.
;;;Once the elements of the list 
;;;are evaluated , the function 
;;;- is applied to the gotten 
;;;arguments , and the returned 
;;;value is 2.1415926535897932385L0 
2.1415926535897932385L0
```

## 什么是特殊形态？

特殊表单，是一个表单，其中 ***的第一个元素*** 是一个特殊运算符。特殊操作员包括:

```
block                            
catch                         
eval-when                 
flet                             
function                  
go                     
if                           
labels                                      
let          
let*
load-time-value
locally
macrolet
multiple-value-call 
multiple-value-prog1 
progn
progv
quote
return-from 
setq
symbol-macrolet 
tagbody
the
throw
unwind-protect
```

一个特殊的表单，可能有一个特殊的语法，或者 ***一个特殊的求值规则*** ，例如它可能不会求值它所有的参数。

```
> (if t 1 pi )
;;;In this example , pi is 
;;;never evaluated , since 
;;;the condition is true , 
;;;hence what is evaluated 
;;;is 1 , which evaluates ,
;;;to itself .
1
```

*原载于 2021 年 1 月 22 日 https://twiserandom.com*[](https://twiserandom.com/lisp/what-is-a-form-in-lisp/)**。**