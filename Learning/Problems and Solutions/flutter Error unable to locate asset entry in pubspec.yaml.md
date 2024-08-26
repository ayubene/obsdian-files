---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

flutter Error: unable to locate asset entry in pubspec.yaml: "assets/fonts/Lato-Regular.ttf"

[参考链接](https://stackoverflow.com/questions/59432187/error-unable-to-locate-asset-entry-in-pubspec-yaml-assets-fonts-lato-regular)

在pubspec.yaml中添加font的时候出现这个问题
发现是因为我放的文件夹不对，需要放在根目录下（但是我不知道为什么android studio里没有显示一些文件夹）
本来放在这里 一直不对
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240714002410231-1184729479.png)
后来在文件夹找了一下放到lib里 新建了assets文件夹
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240714002519183-2134723886.png)

可以使用[google_fonts ](https://pub.dev/packages/google_fonts),这样可以不用自己配置