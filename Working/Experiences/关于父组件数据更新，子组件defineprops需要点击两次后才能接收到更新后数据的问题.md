---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
代码大概是：
父组件内有一个表格，点击订单号后，弹窗展示订单明细
最开始是这样写的：
```
// 父组件中
<OrderDetailDialog v-if="state.orderDetailsVisible" v-model="state.orderDetailsVisible" :currentClickItemId="state.currentClickItemId"  
  :orderItems="state.orderItems"></OrderDetailDialog>

// 子组件用defineProps接收数据

const props = defineProps({
  currentClickItemId:{
    type: Number,
    default: 0
  },
  orderItems:{
    type: Array,
    default: []
  },
})

// 子组件接收到的数据在template中使用
<template>
  <Dialog width="40%" :title="state.title">
    <!-- id -->
    currentClickItemId: {{ currentClickItemId }}
    orderDetails.id: {{ orderDetails.id }}
     <!-- 购买商品 -->
     <div class="commment">
        <span>购买商品</span>
        <div class="detail-content">
            <el-table :data="orderItems" border style="width: 100%">
                <el-table-column prop="itemName" label="商品名称" />
                // 其他column...
            </el-table>
        </div>
     </div>
  </Dialog>
</template>
```

问题在于，每次点击订单号，弹窗展示的数据，是上一次点击得到的结果，
比如有订单号1，订单号2，先点击1，此时不会展示任何数据；再点击1，会正常展示1的数据；再点击2，展示的是1的数据，再点击2，才会正确展示2的数据

我怀疑是因为订单数据量很大，所以页面渲染的速度快于接口返回数据的速度？

不是很确定真正的原因，但是这样解决了：
```
// 父组件
<OrderDetailDialog v-if="state.orderDetailsVisible  && state.currentClickItemId == state.orderDetails.id" v-model="state.orderDetailsVisible" :currentClickItemId="state.currentClickItemId"  
  :orderItems="state.orderItems"></OrderDetailDialog>
```

currentClickItemId 是当前点击的订单号，orderDetails是订单数据，id也是订单号；当二者相等时，才挂载这个页面
我观察到弹出弹窗的时候会小小刷新/闪一下，之前不会

另为了加深知识点印象，我再写一下：
v-if会挂载、销毁页面；v-show则只是控制页面是否展示