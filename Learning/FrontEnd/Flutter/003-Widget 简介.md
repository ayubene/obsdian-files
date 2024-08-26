---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Flutter
---
#Done 
[学习链接](https://book.flutterchina.club/chapter2/flutter_widget_intro.html#_2-2-1-widget-%E6%A6%82%E5%BF%B5)
### 什么是widget
描述UI元素的配置信息
Widget类本身是一个抽象类，其中最核心的就是定义了createElement()接口，在 Flutter 开发中，我们一般都不用直接继承Widget类来实现一个新组件，相反，我们通常会通过继承StatelessWidget或StatefulWidget来间接继承widget类来实现。StatelessWidget和StatefulWidget都是直接继承自Widget类

### 四棵树
根据 Widget 树生成一个 Element 树，Element 树中的节点都继承自 Element 类。
根据 Element 树生成 Render 树（渲染树），渲染树中的节点都继承自RenderObject 类。
根据渲染树生成 Layer 树，然后上屏显示，Layer 树中的节点都继承自 Layer 类。

### StatelessWidget
根据学习资料的示例，自己试着调整写了一下 main.dart 
```
import 'package:flutter/material.dart';

// void main() {
//   runApp(const MyApp());
// }

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const MyHomePage(title: 'StatelessWidget'),
    );
  }
}

class MyHomePage extends StatelessWidget {
  const MyHomePage({super.key, required this.title});

  final String title;

  @override
  Widget build(BuildContext context) {
    return Echo(text: "hello world");
  }

}


class Echo extends StatelessWidget  {
  const Echo({
    Key? key,
    required this.text,
    this.backgroundColor = Colors.grey, //默认为灰色
  }):super(key:key);

  final String text;
  final Color backgroundColor;

  @override
  Widget build(BuildContext context) {
    return
    Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text("StatelessWidget"),
      ),
      body: Center(
        child: Container(
          color: backgroundColor,
          child: Text(text),
        ),
      ),
    );
  }

}
```
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240704232640271-499788928.png)
build方法有一个context参数，它是BuildContext类的一个实例，表示当前 widget 在 widget 树中的上下文，它提供了从当前 widget 开始向上遍历 widget 树以及按照 widget 类型查找父级 widget 的方法
示例代码
```
import 'package:flutter/material.dart';

// void main() {
//   runApp(const MyApp());
// }

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const ContextRoute(),
    );
  }
}


class ContextRoute extends StatelessWidget  {
  const ContextRoute();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text("Context测试"),
      ),
      body: Container(
        child: Builder(builder: (context) {
          // 在 widget 树中向上查找最近的父级`Scaffold`  widget
          Scaffold scaffold = context.findAncestorWidgetOfExactType<Scaffold>() as Scaffold;
          // 直接返回 AppBar的title， 此处实际上是Text("Context测试")
          return (scaffold.appBar as AppBar).title!;
        }),
      ),
    );
  }
}
```
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240706152323245-1468142351.png)

### StatefulWidget

StatefulWidget也是继承自widget类，并重写了createElement()方法，不同的是返回的Element 对象并不相同；另外StatefulWidget类中添加了一个新的接口createState()。
类定义：
```
abstract class StatefulWidget extends Widget {
  const StatefulWidget({ Key key }) : super(key: key);
    
  @override
  StatefulElement createElement() => StatefulElement(this);
    
  @protected
  State createState();
}
```
StatefulElement 间接继承自Element类，与StatefulWidget相对应（作为其配置数据）。StatefulElement中可能会多次调用createState()来创建状态（State）对象。

createState() 用于创建和 StatefulWidget 相关的状态，它在StatefulWidget 的生命周期中可能会被多次调用。
State 对象和StatefulElement具有一一对应的关系

### State
#### 简介
+ 一个 StatefulWidget 类会对应一个 State 类，State表示与其对应的 StatefulWidget 要维护的状态
+ State 中的保存的状态信息可以
+ 当State被改变时，可以手动调用其setState()方法通知Flutter 框架状态发生改变,重新调用其build方法重新构建 widget 树，更新UI
+ State的两个常用属性：widget、context；widget是与该 State 实例关联的 widget 实例、context作用同StatelessWidget 的BuildContext
#### State的生命周期
```
import 'package:flutter/material.dart';
import 'dart:io';
import 'dart:developer' as developer;

void main() => runApp(const StateLifecycleTest());


class StateLifecycleTest extends StatelessWidget {
  const StateLifecycleTest({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const CounterWidget(),
    );
    // return CounterWidget();
  }
}

class CounterWidget extends StatefulWidget {
  const CounterWidget({Key? key, this.initValue = 0});

  final int initValue;

  @override
  _CounterWidgetState createState() => _CounterWidgetState();
}


class _CounterWidgetState extends State<CounterWidget> {
  int _counter = 0;

  @override
  void initState() {
    super.initState();
    //初始化状态
    _counter = widget.initValue;
    print("initState");
  }

  @override
  Widget build(BuildContext context) {
    print("build");
    return Scaffold(
      body: Center(
        child: TextButton(
          child: Text('$_counter'),
          //点击后计数器自增
          onPressed: () => setState(
                () => ++_counter,
          ),
        ),
      ),
    );
  }

  @override
  void didUpdateWidget(CounterWidget oldWidget) {
    super.didUpdateWidget(oldWidget);
    print("didUpdateWidget ");
  }

  @override
  void deactivate() {
    super.deactivate();
    print("deactivate");
  }

  @override
  void dispose() {
    super.dispose();
    print("dispose");
  }

  @override
  void reassemble() {
    super.reassemble();
    print("reassemble");
  }

  @override
  void didChangeDependencies() {
    super.didChangeDependencies();
    print("didChangeDependencies");
  }
}


class ContextRoute extends StatelessWidget  {
  const ContextRoute();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        backgroundColor: Theme.of(context).colorScheme.inversePrimary,
        title: Text("Context测试"),
      ),
      body: Container(
        child: Builder(builder: (context) {
          // 在 widget 树中向上查找最近的父级`Scaffold`  widget
          Scaffold scaffold = context.findAncestorWidgetOfExactType<Scaffold>() as Scaffold;
          // 直接返回 AppBar的title， 此处实际上是Text("Context测试")
          return (scaffold.appBar as AppBar).title!;
        }),
      ),
    );
  }
}
```
运行以上代码，输出：
```
I/flutter ( 5053): initState
I/flutter ( 5053): didChangeDependencies
I/flutter ( 5053): build
```
点击热重载，输出
```
I/flutter ( 5053): reassemble
I/flutter ( 5053): build
```
在 widget 树中移除CounterWidget，`home: const CounterWidget(),`修改为`home: const Text('hjklh')`
```
I/flutter ( 5053): reassemble
I/flutter ( 5053): deactivate
I/flutter ( 5053): dispose
```
改回`home: const CounterWidget(),`
```
I/flutter ( 5053): initState
I/flutter ( 5053): didChangeDependencies
I/flutter ( 5053): build
```

总结：
+ widget第一次插入widget树时调用initState(),仅调用1次
+ 当State对象的依赖发生变化时调用didChangeDependencies()，组件第一次被创建后挂载的时候（包括重创建）对应的didChangeDependencies()也会被调用
+ 构建 widget 子树时调用build()
+ reassemble()：此回调是专门为了开发调试而提供的，在热重载(hot reload)时会被调用，此回调在Release模式下永远不会被调用
+ didUpdateWidget ()：在 widget 重新构建时，Flutter 框架会调用widget.canUpdate来检测 widget 树中同一位置的新旧节点，然后决定是否需要更新，如果widget.canUpdate返回true则会调用此回调。正如之前所述，widget.canUpdate会在新旧 widget 的 key 和 runtimeType 同时相等时会返回true，也就是说在在新旧 widget 的key和runtimeType同时相等时didUpdateWidget()就会被调用。
+ deactivate()：当 State 对象从树中被移除、位置移动时，会调用此回调
+ dispose()：当 State 对象从树中被永久移除时调用；通常在此回调中释放资源

![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240706162254253-1269710233.png)

### 在 widget 树中获取State对象
#### 通过context获取
有几种写法：
```
// 1
// 查找父级最近的Scaffold对应的ScaffoldState对象
ScaffoldState _state = context.findAncestorStateOfType<ScaffoldState>()!;
// 2
// Scaffold也提供了一个of方法，我们其实是可以直接调用它的
ScaffoldState _state = Scaffold.of(context);
// 3
ScaffoldMessenger.of(context).showSnackBar(
  SnackBar(content: Text("我是SnackBar")),
);
```

整体的代码：
```
import 'package:flutter/material.dart';
import 'dart:io';
import 'dart:developer' as developer;
// void main() {
//   runApp(const MyApp());
// }

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const GetStateObjectRoute(),
    );
  }
}


class GetStateObjectRoute extends StatefulWidget {
  const GetStateObjectRoute({Key? key}) : super(key: key);

  @override
  State<GetStateObjectRoute> createState() => _GetStateObjectRouteState();
}

class _GetStateObjectRouteState extends State<GetStateObjectRoute> {
  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("子树中获取State对象"),
      ),
      body: Center(
        child: Column(
          children: [
            Builder(builder: (context) {
              return ElevatedButton(
                onPressed: () {
                  // 查找父级最近的Scaffold对应的ScaffoldState对象
                  ScaffoldState _state = context.findAncestorStateOfType<ScaffoldState>()!;
                  // 打开抽屉菜单
                  _state.openDrawer();
                },
                child: Text('打开抽屉菜单1'),
              );
            }),
            Builder(builder: (context) {
              return ElevatedButton(
                onPressed: () {
                  // 查找父级最近的Scaffold对应的ScaffoldState对象
                  // 如果 StatefulWidget 的状态是希望暴露出的，应当在 StatefulWidget 中提供一个of 静态方法来获取其 State 对象，开发者便可直接通过该方法来获取；如果 State不希望暴露，则不提供of方法
                  // Scaffold也提供了一个of方法，我们其实是可以直接调用它的
                  ScaffoldState _state = Scaffold.of(context);
                  // 打开抽屉菜单
                  _state.openDrawer();
                },
                child: Text('打开抽屉菜单2'),
              );
            }),
            Builder(builder: (context) {
              return ElevatedButton(
                onPressed: () {
                  ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(content: Text("我是SnackBar")),
                  );
                },
                child: Text('显示SnackBar'),
              );
            }),
          ],
        ),
      ),
      drawer: Drawer(),
    );
  }
}
```

#### 通过GlobalKey获取
用GlobalKey改写了上面的代码
核心点在：
+ 定义一个静态变量 static GlobalKey<ScaffoldState> _globalKey= GlobalKey();
+ 给Scaffold设置key为_globalKey
+ 使用_globalKey _globalKey.currentState.openDrawer()

但是改写SnackBar失败了，参考了[这篇](https://stackoverflow.com/questions/66764441/a-new-way-to-get-showsnackbar)
但是还是没成功
```
import 'package:flutter/material.dart';
import 'dart:io';
import 'dart:developer' as developer;
// void main() {
//   runApp(const MyApp());
// }

void main() => runApp(const MyApp());

class MyApp extends StatelessWidget {
  const MyApp({super.key});

  // This widget is the root of your application.
  @override
  Widget build(BuildContext context) {
    return MaterialApp(
      title: 'Flutter Demo',
      theme: ThemeData(
        colorScheme: ColorScheme.fromSeed(seedColor: Colors.blue),
        useMaterial3: true,
      ),
      home: const GetStateObjectRoute(),
    );
  }
}


class GetStateObjectRoute extends StatefulWidget {
  const GetStateObjectRoute({Key? key}) : super(key: key);

  @override
  State<GetStateObjectRoute> createState() => _GetStateObjectRouteState();
}

class _GetStateObjectRouteState extends State<GetStateObjectRoute> {
  static GlobalKey<ScaffoldState> _globalKey= GlobalKey();
  static GlobalKey<ScaffoldMessengerState> _globalKey2= GlobalKey<ScaffoldMessengerState>();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      key: _globalKey, //设置key
      appBar: AppBar(
        title: Text("子树中获取State对象"),
      ),
      body: Center(
        child: Column(
          children: [
            Builder(builder: (context) {
              return ElevatedButton(
                onPressed: () {
                  // // 查找父级最近的Scaffold对应的ScaffoldState对象
                  // ScaffoldState _state = context.findAncestorStateOfType<ScaffoldState>()!;
                  // // 打开抽屉菜单
                  // _state.openDrawer();
                  _globalKey.currentState?.openDrawer();
                },
                child: Text('打开抽屉菜单3'),
              );
            }),
            Builder(builder: (context) {
              return ElevatedButton(
                onPressed: () {
                  // 查找父级最近的Scaffold对应的ScaffoldState对象
                  _globalKey.currentState?.openDrawer();
                },
                child: Text('打开抽屉菜单4'),
              );
            }),
            Builder(builder: (context) {
              return ElevatedButton(
                onPressed: () {
                  // _globalKey2.currentState?.showSnackBar(
                  //   SnackBar(content: Text('我是SnackBar22')),
                  // );
                  ScaffoldMessenger.of(context).showSnackBar(
                    SnackBar(content: Text('DDDDD')),
                  );
                },
                child: Text('显示SnackBar2'),
              );
            }),
          ],
        ),
      ),
      drawer: Drawer(),
    );
  }
}
```
### 通过 RenderObject 自定义 Widget
StatelessWidget 和 StatefulWidget 都是用于组合其他组件的， Text 、Column、Align等通过定义 RenderObject 来实现；也就是 StatelessWidget 和 StatefulWidget 将通过 RenderObject 实现的组件组合起来
复制了一个示例代码
```
class CustomWidget extends LeafRenderObjectWidget{
  @override
  RenderObject createRenderObject(BuildContext context) {
    // 创建 RenderObject
    return RenderCustomObject();
  }
  @override
  void updateRenderObject(BuildContext context, RenderCustomObject  renderObject) {
    // 更新 RenderObject
    super.updateRenderObject(context, renderObject);
  }
}

class RenderCustomObject extends RenderBox{

  @override
  void performLayout() {
    // 实现布局逻辑
  }

  @override
  void paint(PaintingContext context, Offset offset) {
    // 实现绘制
  }
}
```
如果组件不会包含子组件，则我们可以直接继承自 LeafRenderObjectWidget
如果自定义的 widget 可以包含子组件，则可以根据子组件的数量来选择继承SingleChildRenderObjectWidget 或 MultiChildRenderObjectWidget

### Flutter SDK内置组件库介绍
flutter提供基础组件库、Material 风格（ Android 默认的视觉风格）组件库和、Cupertino 风格（iOS视觉风格）组件库
```
import 'package:flutter/widgets.dart';
import 'package:flutter/material.dart';
import 'package:flutter/cupertino.dart';
```
基础组件：Text、Row、Column、Stack、Container
Material 组件：Scaffold、AppBar、TextButton
Cupertino 组件：

```
  import 'package:flutter/cupertino.dart';
  void main() => runApp(CupertinoApp(home: CupertinoTestRoute()));
  
  
  
  class CupertinoTestRoute extends StatelessWidget  {
    const CupertinoTestRoute({Key? key}) : super(key: key);
  
    @override
    Widget build(BuildContext context) {
      return CupertinoPageScaffold(
        navigationBar: const CupertinoNavigationBar(
          middle: Text("Cupertino Demo"),
          // middle: Text("Cupertino Demo", textDirection: TextDirection.ltr),
        ),
        child: Center(
          child: CupertinoButton(
              color: CupertinoColors.activeBlue,
              child: const Text("Press", textDirection: TextDirection.ltr),
              // child: const Text("Press", textDirection: TextDirection.ltr),
              onPressed: () {}
          ),
        ),
      );
    }
  }
```