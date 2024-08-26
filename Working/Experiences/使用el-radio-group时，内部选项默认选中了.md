---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
[参考文章链接](https://blog.csdn.net/nsnydnz/article/details/120787958)
原来写的是：
```
<el-col :span="24">
  <el-form-item label="状态" prop="transportType">
    <el-radio-group v-model="state.editData.transportType">
      <el-radio :value="1">预售配送</el-radio>
      <el-radio :value="2">团购自提</el-radio>
    </el-radio-group>
  </el-form-item>
</el-col>
```

改成了
```
<el-col :span="24">
  <el-form-item label="状态" prop="transportType">
    <el-radio-group v-model="state.editData.transportType">
      <el-radio :label="1">预售配送</el-radio>
      <el-radio :label="2">团购自提</el-radio>
    </el-radio-group>
  </el-form-item>
</el-col>
```