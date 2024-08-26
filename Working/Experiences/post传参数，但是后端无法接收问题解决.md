---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
同事教了我一下这块的基础

我传参数的时候，是使用封装好的post传，实际上对应不同的格式，有不同的封装方法

我使用的post方法，传递的是raw格式（body）
而实际上后端需要的是form-data格式（body），对应封装函数为upload
params格式则是通过在链接后?id=&name= 这种格式

可以下载安装postman使用