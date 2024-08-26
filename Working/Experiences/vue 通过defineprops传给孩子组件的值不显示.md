---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
[参考这篇](https://blog.csdn.net/CYL_2021/article/details/130323719)
原来是因为子组件先mount

加载数据渲染过程

父组件beforeCreate
父组件created
父组件beforeMount
子组件beforeCreate
子组件created
子组件beforeMount
子组件mounted
父组件mounted