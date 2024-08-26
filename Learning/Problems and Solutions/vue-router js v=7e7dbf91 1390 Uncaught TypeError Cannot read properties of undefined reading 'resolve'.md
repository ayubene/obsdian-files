---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

vue-router.js?v=7e7dbf91:1390 Uncaught TypeError: Cannot read properties of undefined (reading 'resolve')

这个我查了半天十分奇怪的报错
是因为我没有在main.tsmain.ts引入并使用路由器
```
// 引入createApp创建应用
import { createApp } from "vue";
//  引入APP根组件
import App from './App.vue'

// 引入pinia
import { createPinia } from "pinia";
// 引入路由器
import router from "./router";
// 创建一个应用
const app = createApp(App)
// 创建pinia
const pinia = createPinia()
// 使用路由器
app.use(router)

// 安装pinia
app.use(pinia)
// 挂载应用到app容器
app.mount('#app')


```