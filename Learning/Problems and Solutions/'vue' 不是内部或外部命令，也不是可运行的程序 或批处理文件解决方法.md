---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

查看信息
```
npm config list
```
![](https://img2024.cnblogs.com/blog/3028374/202403/3028374-20240319204320057-1762386662.png)
根据prefix路径，查看是否有'vue.cmd'文件
[参考了这篇 感觉很整齐](https://blog.csdn.net/weixin_44566194/article/details/129962361)
如果没有就安装vue
```
npm install -g vue
```
再安装脚手架vue-cli
```
// 安装
npm install -g @vue/cli
// 或者 
cnpm install -g @vue/cli
// 或者
yarn global add @vue/cli
```
然后配置环境变量，在系统变量中的Path增加prefix路径
![](https://img2024.cnblogs.com/blog/3028374/202403/3028374-20240319204805220-1203325179.png)

然后，关掉原来的终端，再打开一个（不知道是不是这个原因，我在原来终端输入`vue - V`还是报错，但是打开新的就好了）
输入
```
vue - V
```