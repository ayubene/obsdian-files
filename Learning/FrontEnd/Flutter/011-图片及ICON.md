---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Flutter
---
#Done 
[《Flutter实战·第二版》](https://book.flutterchina.club/chapter3/img_and_icon.html#_3-3-1-%E5%9B%BE%E7%89%87)
### 图片
Image组件来加载并显示图片，Image的数据源可以是asset、文件、内存以及网络。
#### ImageProvider
ImageProvider 是一个抽象类，主要定义了图片数据获取的接口load()，从不同的数据源获取图片需要实现不同的ImageProvider ，如AssetImage是实现了从Asset中加载图片的 ImageProvider，而NetworkImage 实现了从网络加载图片的 ImageProvider

#### Image
Image widget 有一个必选的image参数，它对应一个 ImageProvider
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240714110609660-1484754324.png)

```
// 从asset中加载图片
Image(
    image: AssetImage("lib/assets/images/my_icon.png"),
    width: 100.0
),
Image.asset("lib/assets/images/my_icon.png",
  width: 100.0,
),
// 从网络加载图片
Image(
  image: NetworkImage(
      "https://i.ibb.co/x5fLLFg/20221030161312.jpg"),
  width: 100.0,
),
Image.network(
  "https://i.ibb.co/x5fLLFg/20221030161312.jpg",
  width: 100.0,
)
```
+ 一些参数
  - width、height：用于设置图片的宽、高，当不指定宽高时，图片会根据当前父容器的限制，尽可能的显示其原始大小，如果只设置width、height的其中一个，那么另一个属性默认会按比例缩放，但可以通过下面介绍的fit属性来指定适应规则。
  - fit：该属性用于在图片的显示空间和图片本身大小不同时指定图片的适应模式；
    - fill：会拉伸填充满显示空间，图片本身长宽比会发生变化，图片会变形。
    - cover：会按图片的长宽比放大后居中填满显示空间，图片不会变形，超出显示空间部分会被剪裁。
    - contain: 这是图片的默认适应规则，图片会在保证图片本身长宽比不变的情况下缩放以适应当前显示空间，图片不会变形。
    - fitWidth：图片的宽度会缩放到显示空间的宽度，高度会按比例缩放，然后居中显示，图片不会变形，超出显示空间部分会被剪裁。
    - fitHeight：图片的高度会缩放到显示空间的高度，宽度会按比例缩放，然后居中显示，图片不会变形，超出显示空间部分会被剪裁。
    - none：图片没有适应策略，会在显示空间内显示图片，如果图片比显示空间大，则显示空间只会显示图片中间部分。
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240714112858003-1789002990.png)

```
Container(
  child: Row(
    children: [
      Container(
        child: Image(
          image: AssetImage("lib/assets/images/my_icon.png"),
          fit: BoxFit.fill,
        ),
        width: 100,
        height: 120,
      ),
      Text('BoxFit.fill')
    ],
  ),
),
Container(
  child: Row(
    children: [
      Container(
        child: Image(
          image: AssetImage("lib/assets/images/my_icon.png"),
          fit: BoxFit.cover,
        ),
        width: 100,
        height: 120,
      ),
      Text('BoxFit.cover')
    ],
  ),
),
Container(
  child: Row(
    children: [
      Container(
        child: Image(
          image: AssetImage("lib/assets/images/my_icon.png"),
          fit: BoxFit.contain,
        ),
        width: 100,
        height: 120,
      ),
      Text('BoxFit.contain')
    ],
  ),
),
Container(
  child: Row(
    children: [
      Container(
        child: Image(
          image: AssetImage("lib/assets/images/my_icon.png"),
          fit: BoxFit.fitWidth,
        ),
        width: 100,
        height: 120,
      ),
      Text('BoxFit.fitWidth')
    ],
  ),
),
Container(
  child: Row(
    children: [
      Container(
        child: Image(
          image: AssetImage("lib/assets/images/my_icon.png"),
          fit: BoxFit.fitHeight,
        ),
        width: 100,
        height: 120,
      ),
      Text('BoxFit.fitHeight')
    ],
  ),
),
Container(
  child: Row(
    children: [
      Container(
        child: Image(
          image: AssetImage("lib/assets/images/my_icon.png"),
          fit: BoxFit.none,
        ),
        width: 100,
        height: 120,
      ),
      Text('BoxFit.none')
    ],
  ),
),
```
  - color和 colorBlendMode：在图片绘制时可以对每一个像素进行颜色混合处理，color指定混合色，而colorBlendMode指定混合模式
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240714113312427-2086447716.png)

```
Container(
  child: Row(
    children: [
      Container(
        child: Image(
          image: AssetImage("lib/assets/images/my_icon.png"),
          fit: BoxFit.contain,
          color: Colors.blue,
          colorBlendMode: BlendMode.difference,
        ),
        width: 100,
        height: 120,
      ),
      Text('color: Colors.blue,colorBlendMode: BlendMode.difference,',
        overflow: TextOverflow.ellipsis,
      )
    ],
  ),
),
```
  - repeat：当图片本身大小小于显示空间时，指定图片的重复规则
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240714172011434-1589628814.png)

```
Container(
  child: Row(
    children: [
      Container(
        child: Image(
          image: AssetImage("lib/assets/images/my_icon.png"),
          fit: BoxFit.contain,
          repeat: ImageRepeat.repeatY,
          // color: Colors.blue,
          // colorBlendMode: BlendMode.difference,
        ),
        width: 100,
        height: 500,
      ),
      Text('repeat: ImageRepeat.repeatY',
        overflow: TextOverflow.ellipsis,
      )
    ],
  ),
),
```

### ICON
iconfont和图片相比有如下优势：
体积小：可以减小安装包大小。
矢量的：iconfont都是矢量图标，放大不会影响其清晰度。
可以应用文本样式：可以像文本一样改变字体图标的颜色、大小对齐等。
可以通过TextSpan和文本混用。

#### 使用Material Design字体图标
在pubspec.yaml文件中的配置如下
```
flutter:
  uses-material-design: true

```
使用（两种写法）
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240714173649894-1370808636.png)

```
Text(
  "\uE03e\uE237\uE287",
  style: TextStyle(
    fontFamily: "MaterialIcons",
    fontSize: 60,
    color: Colors.green,
  ),
),
Row(
  mainAxisAlignment: MainAxisAlignment.center,
  children: <Widget>[
    Icon(Icons.accessible,color:Colors.green,size:60),
    Icon(Icons.error,color:Colors.green),
    Icon(Icons.fingerprint,color:Colors.green),
  ],
)
```

#### 使用自定义字体图标
原书中说可以在iconfont.cn找到很多素材，实际上我找到的都是svg或png，书中例子则是ttf，因此没进行下去