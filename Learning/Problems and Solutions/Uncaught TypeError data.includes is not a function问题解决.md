---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

Uncaught TypeError: data.includes is not a function问题解决

查询了很多帖子，基本上这个报错的原因是
element plus 的el-table报错，v-model传值错误：
+ 传了空值
+ 传了非数组

我是查了很久很久才查到是哪个el-table出了问题
因为是父组件嵌套子组件，子组件先不显示，最后发现是子组件里的el-table，我传了对象，所以报错了

另外也发现自己对生命周期的理解不透彻，我本来以为是点击显示子组件时，才会挂载；而实际上页面一打开，子组件就挂载了