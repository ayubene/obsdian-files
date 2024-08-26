---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

Vue跨域问题：...has been blocked by CORS policy: Response to preflight request doesn't pass access control check: No 'Access-Control-Allow-Origin' header is present on the requested resource.

[参考文章1](https://segmentfault.com/a/1190000041961895)
[参考文章2](https://blog.csdn.net/imqdcn/article/details/133351735)

是在自己本地（http://localhost:5171/）、调后端本地的接口(http://193.0.1.2:20011/ 随便编的，但应该类似)
最开始是这样写的：
```
// 这个是get的封装
export function get(url, data) {

  return request(url, data, 'get', {
    headers: {
      "Content-Type": 'application/x-www-form-urlencoded'
    }
  })
}

// 这里使用封装调用接口http://193.0.1.2:20011/aaa/bbb
import { get } from '@/apis'
export function XXXXXXCheck(data) {
  return get(`http://193.0.1.2:20011/aaa/bbb`, data)
}
```
由于URL有三个基本组成部分：协议+域名或ip+端口，三个必须完全相同称之为同源，不同源的称之为跨域,这个情况是明显的跨域，直接调用是有问题的
所以需要设置代理
一般在配置文件里设置，搜到的一般是vue.config.js，但我用的是vite.config.js,所以这样写
```
import { fileURLToPath, URL } from 'node:url'

import { defineConfig } from 'vite'
import vue from '@vitejs/plugin-vue'

// https://vitejs.dev/config/
export default defineConfig({
  plugins: [vue()],
  resolve: {
    extensions: ['.js', '.vue', '.json','.scss'],
    alias: {
      '@': fileURLToPath(new URL('./src', import.meta.url))
    }
  },
  // server: {
  //   host: "0.0.0.0",
  //   hmr: true,
  //   port:8086
  // }

  //这个是新增的本地服务器与proxy代理设置
  server: {
    open: false,
    port: 5171,
    https: false,
    hotOnly: false,
    proxy: {
      "/sltest": {
        target: "http://193.0.25.103:20033/",
        changeOrigin: true, //是否跨域
        rewrite: (path) => path.replace(/^\/sltest/, ""), //如果后端接口有sltest前缀就不需要替换
        // ws: true,                       //是否代理 websockets
        // secure: true, //是否https接口
      },
    },
  },
})

```
在使用的时候是这样：这个test是自己定义的
```
export function XXXXXXCheck(data) {
  return get(`test/aaa/bbb`, data)
}
```