---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

创建了一个vue2的项目，在运行`npm run serve`的时候报错，检查了拼写也完全没有问题
[最后找到的解决方案](https://blog.csdn.net/ch_jia_jia/article/details/109223093)
将原来的
```
<script>
import { Person } from './components/Person.vue'

export default {
  name: 'App',
  components:{
    Person
  }
}
</script>
```
修改为
```
<script>
// import { Person } from './components/Person.vue'

export default {
  name: 'App',
  components:{
    Person: ()=> import ('./components/Person.vue')
  }
}
</script>
```