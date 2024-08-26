---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

A value of type 'Scaffold?' can't be assigned to a variable of type 'Scaffold'. flutter

原来的代码
```
class ContextRoute extends StatelessWidget  {
  const ContextRoute();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Context测试"),
      ),
      body: Container(
        child: Builder(builder: (context) {
          // 在 widget 树中向上查找最近的父级`Scaffold`  widget
          Scaffold scaffold = context.findAncestorWidgetOfExactType<Scaffold>();
          // 直接返回 AppBar的title， 此处实际上是Text("Context测试")
          return (scaffold.appBar as AppBar).title;
        }),
      ),
    );
  }
}
```
修改后的
```
class ContextRoute extends StatelessWidget  {
  const ContextRoute();

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("Context测试"),
      ),
      body: Container(
        child: Builder(builder: (context) {
          // 在 widget 树中向上查找最近的父级`Scaffold`  widget
          Scaffold scaffold = context.findAncestorWidgetOfExactType<Scaffold>() as Scaffold;
          // 直接返回 AppBar的title， 此处实际上是Text("Context测试")
          return (scaffold.appBar as AppBar).title;
        }),
      ),
    );
  }
}
```


在Scaffold scaffold = context.findAncestorWidgetOfExactType<Scaffold>()后添加了as + 类型
[参考了这里](https://stackoverflow.com/questions/68449448/flutter-error-a-value-of-type-object-cant-be-assigned-to-a-variable-of-type)