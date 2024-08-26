---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Flutter
---
#Done 
[《Flutter实战·第二版》](https://book.flutterchina.club/chapter3/img_and_icon.html#_3-3-1-%E5%9B%BE%E7%89%87)


所谓线性布局，即指沿水平或垂直方向排列子组件。Flutter 中通过`Row`和`Column`来实现线性布局,`Row`和`Column`都继承自`Flex`

对于线性布局，有主轴和纵轴之分，如果布局是沿水平方向，那么主轴就是指水平方向，而纵轴即垂直方向；如果布局沿垂直方向，那么主轴就是指垂直方向，而纵轴就是水平方向。在线性布局中，有两个定义对齐方式的枚举类`MainAxisAlignment`和`CrossAxisAlignment`，分别代表主轴对齐和纵轴对齐。

### Row
- `textDirection`：表示水平方向子组件的布局顺序(是从左往右还是从右往左)，默认为系统当前Locale环境的文本方向(如中文、英语都是从左往右，而阿拉伯语是从右往左)。
- `mainAxisSize`：表示`Row`在主轴(水平)方向占用的空间，默认是`MainAxisSize.max`，表示尽可能多的占用水平方向的空间，此时无论子 widgets 实际占用多少水平空间，`Row`的宽度始终等于水平方向的最大宽度；而`MainAxisSize.min`表示尽可能少的占用水平空间，当子组件没有占满水平剩余空间，则`Row`的实际宽度等于所有子组件占用的水平空间；
- `mainAxisAlignment`：表示子组件在`Row`所占用的水平空间内对齐方式，如果`mainAxisSize`值为`MainAxisSize.min`，则此属性无意义，因为子组件的宽度等于`Row`的宽度。只有当`mainAxisSize`的值为`MainAxisSize.max`时，此属性才有意义，`MainAxisAlignment.start`表示沿`textDirection`的初始方向对齐，如`textDirection`取值为`TextDirection.ltr`时，则`MainAxisAlignment.start`表示左对齐，`textDirection`取值为`TextDirection.rtl`时表示从右对齐。而`MainAxisAlignment.end`和`MainAxisAlignment.start`正好相反；`MainAxisAlignment.center`表示居中对齐。读者可以这么理解：`textDirection`是`mainAxisAlignment`的参考系。
- `verticalDirection`：表示`Row`纵轴（垂直）的对齐方向，默认是`VerticalDirection.down`，表示从上到下。
- `crossAxisAlignment`：表示子组件在纵轴方向的对齐方式，`Row`的高度等于子组件中最高的子元素高度，它的取值和`MainAxisAlignment`一样(包含`start`、`end`、 `center`三个值)，不同的是`crossAxisAlignment`的参考系是`verticalDirection`，即`verticalDirection`值为`VerticalDirection.down`时`crossAxisAlignment.start`指顶部对齐，`verticalDirection`值为`VerticalDirection.up`时，`crossAxisAlignment.start`指底部对齐；而`crossAxisAlignment.end`和`crossAxisAlignment.start`正好相反；
- `children` ：子组件数组
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
    // 背景颜色为红色的盒子，不指定它的宽度和高度  
    Widget redBox = DecoratedBox(  
      decoration: BoxDecoration(color: Colors.red),  
    );  
    return Scaffold(  
      appBar: AppBar(  
        title: Text("初始页面"),  
      ),  
      body: Column(  
        //测试Row对齐方式，排除Column默认居中对齐的干扰  
        crossAxisAlignment: CrossAxisAlignment.start,  
        children: <Widget>[  
          Row(  
            mainAxisAlignment: MainAxisAlignment.center,  
            children: <Widget>[  
              Text(" 居中 "),  
              Text(" 布局 "),  
            ],  
          ),  
          Row(  
            mainAxisSize: MainAxisSize.min,  
            mainAxisAlignment: MainAxisAlignment.center,  
            children: <Widget>[  
              Text(" 居中 "),  
              Text(" 无效 "),  
            ],  
          ),  
          Row(  
            mainAxisAlignment: MainAxisAlignment.end,  
            textDirection: TextDirection.rtl,  
            children: <Widget>[  
              Text(" 布局从右到左 "),  
              Text(" rtl此处左对齐 "),  
            ],  
          ),  
          Row(  
            crossAxisAlignment: CrossAxisAlignment.start,  
            verticalDirection: VerticalDirection.down,  
            children: <Widget>[  
              Text(" 纵向布局VerticalDirection.down ",),  
              Text(" start从上到下 ", style: TextStyle(fontSize: 30.0)),  
            ],  
          ),  
        ],  
      ),  
    );  
  
  
  }  
}
```
### Column
参数和`Row`一样，不同的是布局方向为垂直，主轴纵轴正好相反
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
    // 背景颜色为红色的盒子，不指定它的宽度和高度  
    Widget redBox = DecoratedBox(  
      decoration: BoxDecoration(color: Colors.red),  
    );  
    return Scaffold(  
      appBar: AppBar(  
        title: Text("初始页面"),  
      ),  
      body: Column(  
        // Column会在垂直方向占用尽可能多的空间，此例中会占满整个屏幕高度。  
        // 指定了 crossAxisAlignment 属性为CrossAxisAlignment.center，那么子项在Column纵轴方向（此时为水平方向）会居中对齐  
        crossAxisAlignment: CrossAxisAlignment.center,  
        verticalDirection: VerticalDirection.up,  
        children: <Widget>[  
          Text("hi"),  
          Text("world"),  
        ],  
      )  
    );  
  
  
  }  
}
```

### 特殊情况
如果`Row`里面嵌套`Row`，或者`Column`里面再嵌套`Column`，那么只有最外面的`Row`或`Column`会占用尽可能大的空间，里面`Row`或`Column`所占用的空间为实际大小
如果要让里面的`Column`占满外部`Column`，可以使用`Expanded` 组件
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
    // 背景颜色为红色的盒子，不指定它的宽度和高度  
    Widget redBox = DecoratedBox(  
      decoration: BoxDecoration(color: Colors.red),  
    );  
    return Scaffold(  
      appBar: AppBar(  
        title: Text("初始页面"),  
      ),  
      body: Container(  
        color: Colors.green,  
        child: Padding(  
          padding: const EdgeInsets.all(16.0),  
          child: Column(  
            crossAxisAlignment: CrossAxisAlignment.start,  
            mainAxisSize: MainAxisSize.max, //有效，外层Colum高度为整个屏幕  
            children: <Widget>[  
              Expanded(  
                  child: Container(  
                    color: Colors.red,  
                    child: Column(  
                      mainAxisSize: MainAxisSize.max,//无效，内层Colum高度为实际高度  
                      children: <Widget>[  
                        Text("hello world "),  
                        Text("I am Jack "),  
                      ],  
                    ),  
                  )  
              )  
            ],  
          ),  
        ),  
      )  
    );  
  
  
  }  
}
```