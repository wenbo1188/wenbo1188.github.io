---
layout: post
title: "Python 异常处理"
date: 2017-05-27
categories: python
---

### Python 异常处理 ###
+ 异常  
  
异常是一种打破正常代码块控制流来进行错误或者其他类型的异常处理的方式。在错误被检测到的地方，就会抛出一个异常。它可能被周围的程序块处理或者由直接或间接调用该代码块的代码处理。  

当python解释器检测到一个运行时错误(比如除零错误)时，它会抛出一个异常。python程序也可以显示地通过 _raise_ 语句抛出一个异常。一般通过 _try_ ... _except_ 语句来进行可能出现异常的代码编写，_finally_ 语句用来作为最终清理语句，无论是否发生异常都会被执行。  

python使用“终止”模型的错误处理方式：异常处理可以发现发生了什么问题并且在当前代码的上一层继续运行，但不能修复错误和进行错误操作的重试(除非从代码的顶层进行重新进入)  

当异常未被处理时，解释器会终止程序执行或者返回它的交互主循环。无论哪种情况下，它都会打印栈顶信息，除非这种异常是 _SystemExit_  
  
异常被识别为类的实例。 _except_ 语句取决于实例所属的类: 它会引用实例所属的类或其基类。实例可以被handler接收并且可以携带关于异常情况的额外信息。  
  
+ try语句  
  
```python  
try_stmt  ::=  try1_stmt | try2_stmt
try1_stmt ::=  "try" ":" suite
               ("except" [expression ["as" identifier]] ":" suite)+
               ["else" ":" suite]
               ["finally" ":" suite]
try2_stmt ::=  "try" ":" suite
               "finally" ":" suite
```  
_except_ 语句指出一个或多个异常handler，当 _try_ 语句中没有异常发生时，没有异常handler被执行，当异常发生时，就开始了相应handler的查找。查找在 _except_ 语句中进行，直到找到一个相匹配的handler。一个不含表达式的 _except_ 语句必须放在最后，它可以匹配任何异常；而一个含表达式的 _except_ 语句，它的表达式的值将被计算。如果一个 _object_ 属于异常 _object_ 的类或基类，那么这个 _object_ 就和这个异常匹配。  

如果没有匹配的异常，搜索将在周围代码和调用栈中继续；如果在handler的expression中抛出了异常，原有的搜索handler取消并开始对新的异常handler进行搜索(就好像异常是在 _try_ 语句中抛出的一样)  

当异常匹配一个 _except_ 语句之后，这个异常通过 _as_ 关键词来指定给特定的变量并且如果存在 _except_ 的suite的话，suite将会被执行。  

可选的 _else_ 语句在 _try_ 语句后被执行。_else_ 语句中的异常不会被接下来的 _except_ 语句处理。  

_finally_ 语句作为一个清理的handler。如果在 _except_ 或 _else_ 语句中出现异常并且没有被处理的话，这个异常将被临时存储。 _finally_ 语句被执行，该临时存储的异常将会在 _finally_ 语句执行之后再次被抛出。如果 _finally_ 语句中也抛出了异常，则存储的异常将会被替换为最新抛出的那个异常。如果 _finally_ 语句执行了一个 _return_ 或者 _break_ 语句，那么存储的异常会被丢弃。  
  
```python  
>>> def f():
...     try:
...         1/0
...     finally:
...         return 42
>>> f()
42
```  
在 _try_ ... _finally_ 语句中return，break，或者continue被执行时， _finally_ 语句在结束时也会被执行。  

最后的返回值是取决于最后的 _return_ 语句，由于 _finally_ 语句永远会被执行因此 _finally_ 语句中的 _return_ 会被最后执行。  
  
```python  
>>> def foo():
...     try:
...         return 'try'
...     finally:
...         return 'finally'
...
>>> foo()
'finally'
```  

