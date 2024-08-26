---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
[参考链接](https://www.cnblogs.com/luoyihao/p/14297957.html)
二编：发到线上才发现，this.是vue2用的，本地是生效的，发到线上就有问题了；另发现我这边无法输入的原因是我数据没有写成reactive....
原代码
```
<Input v-model="state.taskData.jobMainId" placeholder="请输入任务id"/>

```

修改为
```
<Input v-model="state.taskData.jobMainId" placeholder="请输入任务id" @input="change($event)"/>

const change = (e) =>{
  this.$forceUpdate()
}
```