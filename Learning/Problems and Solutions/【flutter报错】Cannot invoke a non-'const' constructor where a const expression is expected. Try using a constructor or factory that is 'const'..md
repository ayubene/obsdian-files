---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

[参考来自](https://stackoverflow.com/questions/62983305/error-cannot-invoke-a-non-const-constructor-where-a-const-expression-is-expec)
这段话解决了问题
Unfortunately not all widgets has a const constructor, Container and Column are two examples of that. You won't be able to construct those widgets as child of a const constructor.

出现这个报错的时候可以看看是否在附近使用了const 去掉就好了