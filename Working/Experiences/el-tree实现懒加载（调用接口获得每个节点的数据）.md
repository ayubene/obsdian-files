---
tags:
  - Blog
  - Experiences
  - Working
---
#Done 

## 添加el-tree元素
+ node-key="value"要添加，如果是可勾选的情况，没有添加的话无法获取已勾选的数据
+ :load="loadChildren"是每次点击某个节点时，会执行的函数，用于获取该节点下的数据
+ lazy 标识是懒加载
+ show-checkbox 标识可勾选
+ :props="treeProps" 定义了节点属性
+ ref="tree" 用于后续获取此元素以及已勾选的数据
```
<el-tree node-key="value" :load="loadChildren" lazy show-checkbox :props="treeProps" ref="tree"></el-tree>
```
## 写对应的函数
+ treeProps定义节点属性，可能会出现label: 'id'这种情况，因为获取到的数据可能和自己定义的名称不一致
+ 如果用v-if包裹el-tree,this.$refs.tree会是undefined，需要使用this.$nextTick(()=>{})的方法
+ loadChildren函数，node包含了该节点的一些数据；自己定义的数据比如label在node.data;resolve函数主要是返回获取到的节点数据；reject我这里没用到
```
export default {
    data() {
        return {
            treeProps: {
                children: 'children',
                label: 'label',
                level: 'level',
                parentCategoryId: 'parentCategoryId',
                selected: 'selected',
                value: 'value',
              },
        }
    }
    methods: {
        getSelectedNodes() {
            this.$nextTick(() => {
                // 元素显示后执行
                const checkedNodes = this.$refs.tree.getCheckedNodes();
                console.log(checkedNodes, 'checkedNodescheckedNodescheckedNodes')
            })
        },
        loadChildren(node, resolve, reject) {
            // 如果是根节点，获取第一级的所有数据
            if (node.level === 0) {
                let tempData = []
                qygApi.categoryTree({
                  companyId: this.currentId,
                  level: node.level + 1,
                }).then(res => {
                  if (res.code == 'success') {

                    tempData = res.data
                  }

                })
                setTimeout(() => {
                  console.log(tempData, 'tempDatatempDatatempData');
                  resolve(tempData)
                }, 300);
            }
            // 如果不是根节点，获取点击节点的数据
            if (node.level >= 1) {
                let tempData = []
                qygApi.categoryTree({
                  companyId: this.currentId,
                  level: node.level + 1,
                  parentCategoryId: node.data.value,
                }).then(res => {
                  // this.nodeData = res.data
                  // console.log('this.nodeData', this.nodeData);
                  if (res.code == 'success') {

                    tempData = res.data
                  }

                })
                setTimeout(() => {
                  console.log(tempData, 'tempDatatempDatatempData');
                  resolve(tempData)
                }, 300);
          }



        },
    }
}
```
## loadChildren函数更新了一些
+ 使用了async和await（最开始用的时候没有效果，所以用了setTimeout）
+ 如果调用接口失败或无数据，一般节点的下拉三角会消失，此节点变为叶子节点；如果要实现调用接口返回失败的情况下还能继续发起调用，可以更改节点状态node.loaded = false;node.loading = false;
```
async loadChildren(node, resolve, reject) {
      console.log(node, '如果是根节点，则直接提供数据如果是根节点，则直接提供数据');
      // this.getSelectedNodes()
      if (node.level === 0) {
        // this.categoryTreeData = 
        // console.log('this.categoryTreeData', this.categoryTreeData);
        // console.log('props.currentId', props.currentId);
        let tempData = []
        await qygApi.categoryTree({
          companyId: this.currentId,
          level: node.level + 1,
          // parentCategoryId: node.parentCategoryId,
        }).then(res => {
          // this.nodeData = res.data
          // console.log('this.nodeData', this.nodeData);
          if (res.code == 'success') {

            tempData = res.data
          }

        })


        // setTimeout(() => {
          console.log(tempData, 'tempDatatempDatatempData');
          resolve(tempData)
          if (!tempData) {
            reject()
          }
        // }, 1000);




      }
      if (node.level >= 1) {
        // const tempData = this.fetchChildrenNodeData(this.currentId, node.level + 1, node.value)
        console.log(node, 'nodenodenode');

        let tempData = []
        console.log('starttime', dayjs());

        await qygApi.categoryTree({
          companyId: this.currentId,
          level: node.level + 1,
          parentCategoryId: node.data.value,
        }).then(res => {
          // this.nodeData = res.data
          // console.log('this.nodeData', this.nodeData);
          if (res.code == 'success') {

            tempData = res.data
          }

        })
        console.log('endtime', dayjs());

        // setTimeout(() => {
          console.log(tempData, 'tempDatatempDatatempData');

          if (tempData.length == 0 && node.level == 3) {
            var self = this
            console.log('加载失败加载失败加载失败');
            
            self.$message.error('加载失败，请点击重新加载')
            node.loaded = false
            node.loading = false
            reject()
          } else {
            resolve(tempData)
          }
        // }, 10);
      }



    },
```