---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Flutter
---
#Done 
[《Flutter实战·第二版》](https://book.flutterchina.club/chapter3/img_and_icon.html#_3-3-1-%E5%9B%BE%E7%89%87)
层叠布局和 Web 中的绝对定位、Android 中的 Frame 布局是相似的，子组件可以根据距父容器四个角的位置来确定自身的位置。层叠布局允许子组件按照代码中声明的顺序堆叠起来。Flutter中使用`Stack`和`Positioned`这两个组件来配合实现绝对定位。`Stack`允许子组件堆叠，而`Positioned`用于根据`Stack`的四个角来确定子组件的位置。

### Stack
- `alignment`：此参数决定如何去对齐没有定位（没有使用`Positioned`）或部分定位的子组件
- `textDirection`：和`Row`、`Wrap`的`textDirection`功能一样，都用于确定`alignment`对齐的参考系，即：`textDirection`的值为`TextDirection.ltr`，则`alignment`的`start`代表左，`end`代表右，即`从左往右`的顺序；`textDirection`的值为`TextDirection.rtl`，则alignment的`start`代表右，`end`代表左，即`从右往左`的顺序。
- `fit`：此参数用于确定**没有定位**的子组件如何去适应`Stack`的大小。`StackFit.loose`表示使用子组件的大小，`StackFit.expand`表示扩伸到`Stack`的大小。
- `clipBehavior`：此属性决定对超出`Stack`显示空间的部分如何剪裁，Clip枚举类中定义了剪裁的方式，Clip.hardEdge 表示直接剪裁，不应用抗锯齿

### Positioned
`left`、`top` 、`right`、 `bottom`分别代表离`Stack`左、上、右、底四边的距离。`width`和`height`用于指定需要定位元素的宽度和高度。注意，`Positioned`的`width`、`height` 和其他地方的意义稍微有点区别，此处用于配合`left`、`top` 、`right`、 `bottom`来定位组件，举个例子，在水平方向时，你只能指定`left`、`right`、`width`三个属性中的两个，如指定`left`和`width`后，`right`会自动算出(`left`+`width`)，如果同时指定三个属性则会报错，垂直方向同理。

### 示例
由于第一个子文本组件`Text("Hello world")`没有指定定位，并且`alignment`值为`Alignment.center`，所以它会居中显示。第二个子文本组件`Text("I am Jack")`只指定了水平方向的定位(`left`)，所以属于部分定位，即垂直方向上没有定位，那么它在垂直方向的对齐方式则会按照`alignment`指定的对齐方式对齐，即垂直方向居中。对于第三个子文本组件`Text("Your friend")`，和第二个`Text`原理一样，只不过是水平方向没有定位，则水平方向居中。
```
import 'package:flutter/material.dart';  
import 'package:english_words/english_words.dart';  
  
// import 'package:flutter/cupertino.dart';  
// void main() {  
//   runApp(const MyApp());  
// }  
  
import 'dart:async' show Future;  
import 'package:flutter/services.dart' show rootBundle;  
  
  
  
void main() => runApp(const MyApp());  
// void main() => runApp(CupertinoApp(home: CupertinoTestRoute()));  
  
class MyApp extends StatelessWidget {  
  const MyApp({super.key});  
  
  // This widget is the root of your application.  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      title: 'Flutter Demo',  
      initialRoute:"/", //名为"/"的路由作为应用的home(首页)  
      theme: ThemeData(  
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),  
        useMaterial3: true,  
      ),  
      routes:{  
        "/":(context) => LoginRoute(), //注册首页路由  
      },  
    );  
  }  
}  
  
class LoginRoute extends StatefulWidget{  
  LoginRoute({Key? key}) : super(key: key);  
  
  @override  
  _LoginRoute createState() => _LoginRoute();  
}  
  
class _LoginRoute extends State<LoginRoute> with SingleTickerProviderStateMixin  {  
  
  // 两个controller：获取文本内容，监听文本变化  
  TextEditingController _unameController = TextEditingController();  
  TextEditingController _pwdController = TextEditingController();  
  
  TextEditingController _selectionController =  TextEditingController();  
  
  // 焦点  
  FocusNode focusNodeUname = FocusNode();  
  FocusNode focusNodePwd = FocusNode();  
  FocusNode focusNodeSel = FocusNode();  
  FocusScopeNode? focusScopeNode;  
  GlobalKey _formKey = GlobalKey<FormState>();  
  AnimationController? _animationController;  
  
  @override  
  void initState() {  
    //监听输入改变  
    _pwdController.addListener((){  
      print("***************"+_pwdController.text+"***************");  
    });  
    // 获得焦点时focusNodeUname.hasFocus值为true，失去焦点时为false  
    focusNodeUname.addListener((){  
      print('焦点变化！！！！！！！！！！！！！！！！！');  
      print(focusNodeUname.hasFocus);  
    });  
  
    //动画执行时间3秒  
    _animationController = AnimationController(  
      vsync: this, //注意State类需要混入SingleTickerProviderStateMixin（提供动画帧计时/触发器）  
      duration: Duration(seconds: 3),  
    );  
    _animationController?.forward();  
    _animationController?.addListener(() => setState(() => {}));  
    super.initState();  
  }  
  
  
  
  @override  
  void dispose() {  
    _animationController?.dispose();  
    super.dispose();  
  }  
  
  
  
  @override  
  Widget build(BuildContext context) {  
    return Scaffold(  
      appBar: AppBar(  
        title: Text("初始页面"),  
      ),  
      body: ConstrainedBox(  
        constraints: BoxConstraints.expand(),  
        child: Stack(  
          alignment:Alignment.center , //指定未定位或部分定位widget的对齐方式  
          children: <Widget>[  
            Container(  
              child: Text("Hello world",style: TextStyle(color: Colors.white)),  
              color: Colors.red,  
            ),  
            Positioned(  
              left: 18.0,  
              child: Text("I am Jack"),  
            ),  
            Positioned(  
              top: 18.0,  
              child: Text("Your friend"),  
            )  
          ],  
        ),  
      )  
    );  
  
  
  }  
}
```
给`Stack`指定一个`fit`属性，由于第二个子文本组件没有定位，所以`fit`属性会对它起作用，就会占满`Stack`。由于`Stack`子元素是堆叠的，所以第一个子文本组件被第二个遮住了，而第三个在最上层，所以可以正常显示。
```
import 'package:flutter/material.dart';  
import 'package:english_words/english_words.dart';  
  
// import 'package:flutter/cupertino.dart';  
// void main() {  
//   runApp(const MyApp());  
// }  
  
import 'dart:async' show Future;  
import 'package:flutter/services.dart' show rootBundle;  
  
  
  
void main() => runApp(const MyApp());  
// void main() => runApp(CupertinoApp(home: CupertinoTestRoute()));  
  
class MyApp extends StatelessWidget {  
  const MyApp({super.key});  
  
  // This widget is the root of your application.  
  @override  
  Widget build(BuildContext context) {  
    return MaterialApp(  
      title: 'Flutter Demo',  
      initialRoute:"/", //名为"/"的路由作为应用的home(首页)  
      theme: ThemeData(  
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),  
        useMaterial3: true,  
      ),  
      routes:{  
        "/":(context) => LoginRoute(), //注册首页路由  
      },  
    );  
  }  
}  
  
class LoginRoute extends StatefulWidget{  
  LoginRoute({Key? key}) : super(key: key);  
  
  @override  
  _LoginRoute createState() => _LoginRoute();  
}  
  
class _LoginRoute extends State<LoginRoute> with SingleTickerProviderStateMixin  {  
  
  // 两个controller：获取文本内容，监听文本变化  
  TextEditingController _unameController = TextEditingController();  
  TextEditingController _pwdController = TextEditingController();  
  
  TextEditingController _selectionController =  TextEditingController();  
  
  // 焦点  
  FocusNode focusNodeUname = FocusNode();  
  FocusNode focusNodePwd = FocusNode();  
  FocusNode focusNodeSel = FocusNode();  
  FocusScopeNode? focusScopeNode;  
  GlobalKey _formKey = GlobalKey<FormState>();  
  AnimationController? _animationController;  
  
  @override  
  void initState() {  
    //监听输入改变  
    _pwdController.addListener((){  
      print("***************"+_pwdController.text+"***************");  
    });  
    // 获得焦点时focusNodeUname.hasFocus值为true，失去焦点时为false  
    focusNodeUname.addListener((){  
      print('焦点变化！！！！！！！！！！！！！！！！！');  
      print(focusNodeUname.hasFocus);  
    });  
  
    //动画执行时间3秒  
    _animationController = AnimationController(  
      vsync: this, //注意State类需要混入SingleTickerProviderStateMixin（提供动画帧计时/触发器）  
      duration: Duration(seconds: 3),  
    );  
    _animationController?.forward();  
    _animationController?.addListener(() => setState(() => {}));  
    super.initState();  
  }  
  
  
  
  @override  
  void dispose() {  
    _animationController?.dispose();  
    super.dispose();  
  }  
  
  
  
  @override  
  Widget build(BuildContext context) {  
    return Scaffold(  
      appBar: AppBar(  
        title: Text("初始页面"),  
      ),  
      body: ConstrainedBox(  
        constraints: BoxConstraints.expand(),  
        child: Stack(  
          alignment:Alignment.center , //指定未定位或部分定位widget的对齐方式  
          fit: StackFit.expand,  
          children: <Widget>[  
            Positioned(  
              left: 18.0,  
              child: Text("I am Jack"),  
            ),  
            Container(  
              child: Text("Hello world",style: TextStyle(color: Colors.white)),  
              color: Colors.red,  
            ),  
            Positioned(  
              top: 18.0,  
              child: Text("Your friend"),  
            )  
          ],  
        ),  
      )  
    );  
  
  }  
}
```