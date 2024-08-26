---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Flutter
---
#Done 
[《Flutter实战·第二版》](https://book.flutterchina.club/chapter3/img_and_icon.html#_3-3-1-%E5%9B%BE%E7%89%87)
弹性布局允许子组件按照一定比例来分配父容器空间
### Flex
`Flex`组件可以沿着水平或垂直方向排列子组件，如果你知道主轴方向，使用`Row`或`Column`会方便一些，**因为`Row`和`Column`都继承自`Flex`**，参数基本相同，所以能使用Flex的地方基本上都可以使用`Row`或`Column`。`Flex`本身功能是很强大的，它也可以和`Expanded`组件配合实现弹性布局
### Expanded
Expanded 只能作为 Flex 的孩子（否则会报错）
`flex`参数为弹性系数，如果为 0 或`null`，则`child`是没有弹性的，即不会被扩伸占用的空间。如果大于0，所有的`Expanded`按照其 flex 的比例来分割主轴的全部空闲空间
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
        children: <Widget>[  
          //Flex的两个子widget按1：2来占据水平空间  
          Text("Flex的两个子widget按1：2来占据水平空间"),  
          Flex(  
            direction: Axis.horizontal,  
            children: <Widget>[  
              Expanded(  
                flex: 1,  
                child: Container(  
                  height: 30.0,  
                  color: Colors.red,  
                ),  
              ),  
              Expanded(  
                flex: 2,  
                child: Container(  
                  height: 30.0,  
                  color: Colors.green,  
                ),  
              ),  
            ],  
          ),  
          Text("Flex的三个子widget，在垂直方向按2：1：1来占用100像素的空间"),  
          Padding(  
            padding: const EdgeInsets.only(top: 20.0),  
            child: SizedBox(  
              height: 100.0,  
              //Flex的三个子widget，在垂直方向按2：1：1来占用100像素的空间  
              child: Flex(  
                direction: Axis.vertical,  
                children: <Widget>[  
                  Expanded(  
                    flex: 2,  
                    child: Container(  
                      height: 30.0,  
                      color: Colors.red,  
                    ),  
                  ),  
                  Spacer(  
                    flex: 1,  
                  ),  
                  Expanded(  
                    flex: 1,  
                    child: Container(  
                      height: 30.0,  
                      color: Colors.green,  
                    ),  
                  ),  
                ],  
              ),  
            ),  
          ),  
        ],  
      )  
    );  
  
  
  }  
}
```