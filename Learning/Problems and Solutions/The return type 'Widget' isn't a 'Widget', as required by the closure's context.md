---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Pns
---
#Done 

The return type 'Widget?' isn't a 'Widget', as required by the closure's context

原代码
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
修改后
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
          return (scaffold.appBar as AppBar).title!;
        }),
      ),
    );
  }
}
```
在return (scaffold.appBar as AppBar).title后加了`!`（但是不知道为什么）
[参考链接](https://stackoverflow.com/questions/68453845/the-return-type-widget-isnt-a-widget-as-required-by-the-closures-context)