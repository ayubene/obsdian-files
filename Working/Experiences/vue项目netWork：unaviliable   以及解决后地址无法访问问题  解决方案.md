---
tags:
  - Blog
  - Working
  - Experiences
---
#Done 
[参考了这篇博客](https://blog.csdn.net/episode33/article/details/131083135)

### 问题1：netWork：unaviliable 
主要是：这几个：
host: "0.0.0.0",
public: "172.201.10.26:1888", // 此处是自己电脑IP地址！
port: 1888,
```
devServer: {
host: "0.0.0.0",
public: "172.201.10.26:1888", // 此处是自己电脑IP地址！
port: 1888,
https: false,
disableHostCheck: true,
open: false, // 配置自动启动浏览器
}
```
### 地址无法访问
我这边地址无法访问的原因：
以上的devServer里的port和我在public里写的不一致，导致一直无法访问··