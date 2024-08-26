---
tags:
  - Blog
  - Experiences
  - Working
---

#Done 
在工作中，写el-tabe的时候，看同事们的代码会写：
```
const handleOpenOrderDetailDialog = (currentClickItem = {}) => {}
```
其中currentClickItem 会获取到这行的所有信息，一直不知道是为什么，今天查了一下
AI提供给我这段代码
```
<template>  
  <el-table  
    :data="tableData"  
    @row-click="handleRowClick"  
  >  
    <!-- ... 其他列定义 ... -->  
  </el-table>  
</template>  
  
<script>  
export default {  
  data() {  
    return {  
      tableData: [  
        // ... 你的数据 ...  
      ],  
    };  
  },  
  methods: {  
    handleRowClick(row, column, event) {  
      console.log(row); // 这里会打印出当前行的所有信息  
    },  
  },  
};  
</script>
```
也就是说，currentClickItem 就是row这个参数，包含了行的信息
而`currentClickItem = {}`是给这个参数给了一个默认值