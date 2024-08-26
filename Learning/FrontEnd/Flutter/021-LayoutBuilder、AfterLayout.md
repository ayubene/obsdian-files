---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Flutter
---
#Done 
[《Flutter实战·第二版》](https://book.flutterchina.club/chapter3/img_and_icon.html#_3-3-1-%E5%9B%BE%E7%89%87)

### LayoutBuilder
通过 LayoutBuilder，我们可以在**布局过程**中拿到父组件传递的约束信息，然后我们可以根据约束信息动态的构建不同的布局。
实现一个响应式的 Column 组件 ResponsiveColumn，它的功能是当当前可用的宽度小于 200 时，将子组件显示为一列，否则显示为两列。
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
  
class ResponsiveColumn extends StatelessWidget {  
  const ResponsiveColumn({Key? key, required this.children}) : super(key: key);  
  
  final List<Widget> children;  
  
  @override  
  Widget build(BuildContext context) {  
    // 通过 LayoutBuilder 拿到父组件传递的约束，然后判断 maxWidth 是否小于200  
    return LayoutBuilder(  
      builder: (BuildContext context, BoxConstraints constraints) {  
        if (constraints.maxWidth < 200) {  
          // 最大宽度小于200，显示单列  
          return Column(children: children, mainAxisSize: MainAxisSize.min);  
        } else {  
          // 大于200，显示双列  
          var _children = <Widget>[];  
          for (var i = 0; i < children.length; i += 2) {  
            if (i + 1 < children.length) {  
              _children.add(Row(  
                children: [children[i], children[i + 1]],  
                mainAxisSize: MainAxisSize.min,  
              ));  
            } else {  
              _children.add(children[i]);  
            }  
          }  
          return Column(children: _children, mainAxisSize: MainAxisSize.min);  
        }  
      },  
    );  
  }  
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
    var _children = List.filled(6, Text("A"));  
    return Scaffold(  
      appBar: AppBar(  
        title: Text("初始页面"),  
      ),  
      body: Column(  
        children: [  
          Text('限制宽度为190，小于 200,显示单个'),  
          // 限制宽度为190，小于 200          SizedBox(width: 190, child: ResponsiveColumn(children: _children)),  
          Text('限制宽度为300，大于 200,显示2个'),  
          // 限制宽度为300，大于 200          SizedBox(width: 300, child: ResponsiveColumn(children: _children)),  
        ],  
      )  
    );  
  
  }  
}
```
### 打印布局时的约束信息

为了便于排错，我们封装一个能打印父组件传递给子组件约束的组件：
```
  
class LayoutLogPrint<T> extends StatelessWidget {  
  const LayoutLogPrint({  
    Key? key,  
    this.tag,  
    required this.child,  
  }) : super(key: key);  
  
  final Widget child;  
  final T? tag; //指定日志tag  
  
  @override  
  Widget build(BuildContext context) {  
    return LayoutBuilder(builder: (_, constraints) {  
      // assert在编译release版本时会被去除  
      assert(() {  
        print('${tag ?? key ?? child}: $constraints');  
        return true;  
      }());  
      return child;  
    });  
  }  
}
```
这样，我们就可以使用 LayoutLogPrint 组件树中任意位置的约束信息
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
  
class ResponsiveColumn extends StatelessWidget {  
  const ResponsiveColumn({Key? key, required this.children}) : super(key: key);  
  
  final List<Widget> children;  
  
  @override  
  Widget build(BuildContext context) {  
    // 通过 LayoutBuilder 拿到父组件传递的约束，然后判断 maxWidth 是否小于200  
    return LayoutBuilder(  
      builder: (BuildContext context, BoxConstraints constraints) {  
        if (constraints.maxWidth < 200) {  
          // 最大宽度小于200，显示单列  
          return Column(children: children, mainAxisSize: MainAxisSize.min);  
        } else {  
          // 大于200，显示双列  
          var _children = <Widget>[];  
          for (var i = 0; i < children.length; i += 2) {  
            if (i + 1 < children.length) {  
              _children.add(Row(  
                children: [children[i], children[i + 1]],  
                mainAxisSize: MainAxisSize.min,  
              ));  
            } else {  
              _children.add(children[i]);  
            }  
          }  
          return Column(children: _children, mainAxisSize: MainAxisSize.min);  
        }  
      },  
    );  
  }  
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
    var _children = List.filled(6, Text("A"));  
    return Scaffold(  
      appBar: AppBar(  
        title: Text("初始页面"),  
      ),  
      body: Column(  
        children: [  
          Text('限制宽度为190，小于 200,显示单个'),  
          // 限制宽度为190，小于 200          SizedBox(width: 190, child: ResponsiveColumn(children: _children)),  
          Text('限制宽度为300，大于 200,显示2个'),  
          // 限制宽度为300，大于 200  
          LayoutLogPrint(child:SizedBox(width: 300, child: ResponsiveColumn(children: _children)))  
        ],  
      )  
    );  
  
  }  
}  
  
class LayoutLogPrint<T> extends StatelessWidget {  
  const LayoutLogPrint({  
    Key? key,  
    this.tag,  
    required this.child,  
  }) : super(key: key);  
  
  final Widget child;  
  final T? tag; //指定日志tag  
  
  @override  
  Widget build(BuildContext context) {  
    return LayoutBuilder(builder: (_, constraints) {  
      // assert在编译release版本时会被去除  
      assert(() {  
        print('${tag ?? key ?? child}: $constraints');  
        return true;  
      }());  
      return child;  
    });  
  }  
}
```
以上输出
```
I/flutter ( 6554): SizedBox(width: 300.0): BoxConstraints(0.0<=w<=411.4, 0.0<=h<=Infinity)
```

### AfterLayout

#### 获取组件大小和相对于屏幕的坐标
当布局完成时，每个组件的大小和位置才能确定
`context.size` 可以获取当前上下文 RenderObject 的大小，对于Builder、StatelessWidget 以及 StatefulWidget 这样没有对应 RenderObject 的组件（这些组件只是用于组合和代理组件，本身并没有布局和绘制逻辑），获取的是子代中第一个拥有 RenderObject 组件的 RenderObject 对象
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
  
class ResponsiveColumn extends StatelessWidget {  
  const ResponsiveColumn({Key? key, required this.children}) : super(key: key);  
  
  final List<Widget> children;  
  
  @override  
  Widget build(BuildContext context) {  
    // 通过 LayoutBuilder 拿到父组件传递的约束，然后判断 maxWidth 是否小于200  
    return LayoutBuilder(  
      builder: (BuildContext context, BoxConstraints constraints) {  
        if (constraints.maxWidth < 200) {  
          // 最大宽度小于200，显示单列  
          return Column(children: children, mainAxisSize: MainAxisSize.min);  
        } else {  
          // 大于200，显示双列  
          var _children = <Widget>[];  
          for (var i = 0; i < children.length; i += 2) {  
            if (i + 1 < children.length) {  
              _children.add(Row(  
                children: [children[i], children[i + 1]],  
                mainAxisSize: MainAxisSize.min,  
              ));  
            } else {  
              _children.add(children[i]);  
            }  
          }  
          return Column(children: _children, mainAxisSize: MainAxisSize.min);  
        }  
      },  
    );  
  }  
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
    var _children = List.filled(6, Text("A"));  
    return Scaffold(  
      appBar: AppBar(  
        title: Text("初始页面"),  
      ),  
      body: GestureDetector(  
        child: Text('flutter@wendux'),  
        onTap: () => print(context.size), //打印 text 的大小  
      )  
    );  
  
  }  
}
```
点击后得到输出
```
I/flutter ( 6554): Size(411.4, 890.3)
```

#### 获取组件相对于某个父组件的坐标
RenderAfterLayout 类继承自 RenderBox，RenderBox 有一个 localToGlobal 方法，它可以将坐标转化为相对与指定的祖先节点的坐标
