---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
花了一下午时间 基本功还是比较差

[参考博客1](https://blog.csdn.net/interestANd/article/details/121382272)
首先，要修改表头颜色，需要el-table的属性：header-cell-style
可以这样写：
`header-cell-style="{background:'#409EFF',color:'#409EFF'}"`
而我有两个点需要考虑
1）只有部分表头需要修改颜色
2）同事封装的组件，对背景颜色使用了!important

解决1）
写函数:header-cell-style="cellStyle"
这里又有几个基本功不够导致的坑：
+ ({row,column,rowIndex, columnIndex}) 写这个的时候要带上{}，否则row,column,rowIndex, columnIndex这些参数是undefined
+ return {background: 'red !important'}; 返回值需要是对象，不能直接返回数组
```
const cellStyle = ({row,column,rowIndex, columnIndex})=>{
    if(rowIndex===1 && (columnIndex >= 0 && columnIndex <= 7)){
        return {background: 'red !important'};
    }
}
```

解决2）
我尝试了在style中写选择器，但是都没有生效
最后还是使用header-cell-style="cellStyle"，return {background: 'red !important'}; 在return的样式中加上!important 最后实现了

![](https://img2024.cnblogs.com/blog/3028374/202405/3028374-20240508173719448-1407901869.png)
