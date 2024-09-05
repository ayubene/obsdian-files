---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 
## 首先给表格加上ref标签
```
// 创建一个 ref 来引用 Table
const tableRef = ref(null);

<el-table ref="tableRef" :data="state.mainDataList"></el-table>
```
## 跳转时记录index
```
const findIndex = (id) => {
  // state.mainDataList.forEach(item => {
  //   if (item.id == id) {
  //     state.elementId = id
  //   }
  // })
  var i = 0
  for (i = 0; i < state.mainDataList.length; i++) {
    if (state.mainDataList[i].id == id) {
      state.elementId = i
    }
  }
  console.log(state.elementId, 'state.elementId');
  console.log(id, 'id');

}
```
## 回到页面时，跳转定位到对应行
```
// 实现：跳转的明细页，再返回时，还是定位到原来的行
const scrollToSelectedRow = () => {
  if (state.elementId) {
    const rowIndex = state.elementId
    if (rowIndex >= 0) {
      // 获取当前行的 DOM 元素
      const rowElement = tableRef.value.$el.querySelector(`table tbody tr:nth-child(${rowIndex})`);
      if (rowElement) {
        // 获取表格的滚动容器
        const scrollContainer = tableRef.value.$el.querySelector('.el-table__body-wrapper');
        console.log('scrollContainer', scrollContainer);

        if (scrollContainer) {
          // 使用 scrollIntoView 定位到指定行
          rowElement.scrollIntoView();
          // 手动滚动到指定位置
          scrollContainer.scrollTop = rowElement.offsetTop - scrollContainer.clientHeight / 2 + rowElement.clientHeight / 2;
          console.log('rowElement.offsetTop', rowElement.offsetTop);
          console.log('scrollContainer.clientHeight', scrollContainer.clientHeight);
          console.log('rowElement.clientHeight', rowElement.clientHeight);

        }
      }
    }
  }
}
```