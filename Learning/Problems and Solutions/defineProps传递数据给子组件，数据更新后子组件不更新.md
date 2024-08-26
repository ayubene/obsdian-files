---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

**非一般情况，不具有参考价值**

父组件是一个按钮，点击后会显示子组件弹窗
父组件是这样写的
```
  <OrderDetailDialog v-model="state.orderDetailsVisible" :currentClickItemId="state.currentClickItemId"  
  :orderItems="state.orderItems" :orderScoreItems="state.orderScoreItems" :orderTracks="state.orderTracks"></OrderDetailDialog>
```
子组件用defineProps获取数据后，获取到的值是父组件初始化的，而不是更新后的
后来加上v-if就好了
```
  <OrderDetailDialog v-if="state.orderDetailsVisible" v-model="state.orderDetailsVisible" :currentClickItemId="state.currentClickItemId"  
  :orderItems="state.orderItems" :orderScoreItems="state.orderScoreItems" :orderTracks="state.orderTracks"></OrderDetailDialog>
```