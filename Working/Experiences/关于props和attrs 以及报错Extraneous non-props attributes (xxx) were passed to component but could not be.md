---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
这个也是 之前学过，今天用到了才真正理解

父组件传递数据给子组件时：
比如：
```
<PrintDialog v-if="state.printDialogVisible" v-model="state.printDialogVisible" :currentClickItem="state.currentClickItem"
  :printData="state.printData" :currentClickItemId="state.currentClickItemId"></PrintDialog>
```

其中 currentClickItem，printData， currentClickItemId都是传过去的数据

如果子组件的defineProps为以下：
```
const props = defineProps({
    // currentClickItem: {
    //     type: Object,
    //     default: {}
    // },
    printData: {
        type: Object,
        default: {}
    },
    currentClickItemId:{
        type: Number,
        default: 0
    },
})
```
那么printData和currentClickItemId可以`const { printData,currentClickItemId } = props`后直接使用，或者用`props.printData`这样使用，而currentClickItem未被定义，则是被存储在`$attrs`中
此时如果有1个根元素，比如：则currentClickItem会直接绑定到唯一的根元素<span>上
```HTML
<template>
    <span>{{ currentClickItemId }}</span>
</template>
```
此时如果有多个根元素，比如：则currentClickItem不知道绑定到哪里，会报错Extraneous non-props attributes (xxx) were passed to component but could not be
此报错解决方案：
[参考此博客1](https://blog.csdn.net/DM_Cxx/article/details/131133960)
[参考此博客2](https://segmentfault.com/a/1190000042029039)
```HTML
<template>
    <span>{{ currentClickItemId }}</span> //可以显示
    <div class="ReceiptWrap">{{ currentClickItem }}</div> //不会显示
</template>
```
只要修改为以下即可
```HTML
<template>
    <span>{{ currentClickItemId }}</span> //可以显示
    <div class="ReceiptWrap" v-bind="$attrs">{{ $attrs.currentClickItem}}</div> //可以显示
</template>
```