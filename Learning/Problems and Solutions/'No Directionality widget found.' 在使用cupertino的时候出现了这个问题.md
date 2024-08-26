---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

 在使用cupertino的时候出现了这个问题，不过使用其他组件库也是类似的
原代码：
```
import 'package:flutter/cupertino.dart';

void main() => runApp(const CupertinoTestRoute());

class CupertinoTestRoute extends StatelessWidget  {
  const CupertinoTestRoute({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return CupertinoPageScaffold(
      navigationBar: const CupertinoNavigationBar(
        middle: Text("Cupertino Demo"),
      ),
      child: Center(
        child: CupertinoButton(
            color: CupertinoColors.activeBlue,
            child: const Text("Press"),
            onPressed: () {}
        ),
      ),
    );
  }
}
```
查询资料，大概有两种方法可以解决
[方法1](https://blog.csdn.net/shulianghan/article/details/115262342)：在Text内增加属性textDirection: TextDirection.ltr
```
Text("Cupertino Demo", textDirection: TextDirection.ltr),
```
[方法2](https://stackoverflow.com/questions/49687181/no-directionality-widget-found):在runApp内增加 MaterialApp 或 WidgetsApp 或 CupertinoApp
```
void main() => runApp(CupertinoApp(home: CupertinoTestRoute()));
```
修改后如下，实际上是使用第二个方法解决的，增加了CupertinoApp
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