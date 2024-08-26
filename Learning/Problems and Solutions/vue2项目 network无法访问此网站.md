---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

vue2项目启动后，local可以访问，但是network不能访问
防火墙等等都检查了
查到原因如下：本来写的是这样，实际上端口号需要保持一致
```
devServer: {
    disableHostCheck: true,
    open: true,
    host: '0.0.0.0',
    port: 8002,
    https: false,
    hotOnly: false,
    public:'http://XXX.xxx.xx.xxx:8080',
}
```
修改为
```
devServer: {
    disableHostCheck: true,
    open: true,
    host: '0.0.0.0',
    port: 8002,
    https: false,
    hotOnly: false,
    public:'http://XXX.xxx.xx.xxx:8002',
}
```