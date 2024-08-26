---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

想在pubspec.yaml 文件中添加字体：报错了
```
  fonts:
    - family: MiaoZi
      fonts:
        - asset: assets/fonts/MiaoZi-YunYingTi-2.ttf
          weight: 500
```


看了[这篇文章](https://stackoverflow.com/questions/61144571/a-dependency-specification-must-be-a-string-or-a-mapping)解决了

我原来是加在
```
dependencies:
  flutter:
    sdk: flutter
  # 新添加的依赖
  fonts:
    - family: MiaoZi
      fonts:
        - asset: assets/fonts/MiaoZi-YunYingTi-2.ttf
          weight: 500
```
其实应该加在最后
```
name: flutter01
publish_to: 'none' # Remove this line if you wish to publish to pub.dev

version: 1.0.0+1

environment:
  sdk: '>=3.4.3 <4.0.0'

dependencies:
  flutter:
    sdk: flutter
  english_words: ^4.0.0
  cupertino_icons: ^1.0.6

dev_dependencies:
  flutter_test:
    sdk: flutter

flutter:
  uses-material-design: true
  fonts:
    - family: MiaoZi
      fonts:
        - asset: assets/fonts/MiaoZi-YunYingTi-2.ttf
          weight: 500


```