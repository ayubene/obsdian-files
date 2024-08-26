---
tags:
  - Blog
  - Learning
  - FrontEnd
  - Flutter
---
#Done 
[ 《Flutter实战·第二版》调试Flutter应用](https://book.flutterchina.club/chapter2/flutter_app_debug.html#_2-7-1-%E6%97%A5%E5%BF%97%E4%B8%8E%E6%96%AD%E7%82%B9)
[ 《Flutter实战·第二版》Flutter异常捕获]（https://book.flutterchina.club/chapter2/thread_model_and_error_report.html#_2-8-1-dart%E5%8D%95%E7%BA%BF%E7%A8%8B%E6%A8%A1%E5%9E%8B）
###  Dart单线程模型
![](https://img2024.cnblogs.com/blog/3028374/202407/3028374-20240713194425328-1367600981.png)
Dart 在单线程中是以消息循环机制来运行的，其中包含两个任务队列，一个是“微任务队列” microtask queue，另一个叫做“事件队列” event queue。从图中可以发现，微任务队列的执行优先级高于事件队列。
外部事件任务都在事件队列中，如IO、计时器、点击、以及绘制事件等，而微任务通常来源于Dart内部，并且微任务非常少，我们可以通过Future.microtask(…)方法向微任务队列插入一个任务。
在事件循环中，当某个任务发生异常并没有被捕获时，程序并不会退出，而直接导致的结果是当前任务的后续代码就不会被执行了，也就是说一个任务中的异常是不会影响其他任务执行的。

### Flutter异常捕获
#### Flutter框架异常捕获
例子，当我们布局发生越界或不合规范时，Flutter就会自动弹出一个错误界面，这是因为Flutter已经在执行build方法时添加了异常捕获，最终的源码如下：
```
@override
void performRebuild() {
 ...
  try {
    //执行build方法  
    built = build();
  } catch (e, stack) {
    // 有异常时则弹出错误提示  
    built = ErrorWidget.builder(_debugReportException('building $this', e, stack));
  } 
  ...
}     
```
如果我们想自己上报异常，只需要提供一个自定义的错误处理回调即可，如：
```
void main() {
  FlutterError.onError = (FlutterErrorDetails details) {
    reportError(details);
  };
 ...
}
```
#### 其他异常捕获与日志收集
在Flutter中，还有一些Flutter没有为我们捕获的异常，如调用空对象方法异常、Future中的异常。
Dart中有一个runZoned(...) 方法，可以给执行对象指定一个Zone。Zone表示一个代码执行的环境范围，如Zone中可以捕获日志输出、Timer创建、微任务调度的行为，同时Zone也可以捕获所有未处理的异常。
#### 最终的错误上报代码
```
void collectLog(String line){
    ... //收集日志
}
void reportErrorAndLog(FlutterErrorDetails details){
    ... //上报错误和日志逻辑
}

FlutterErrorDetails makeDetails(Object obj, StackTrace stack){
    ...// 构建错误信息
}

void main() {
  var onError = FlutterError.onError; //先将 onerror 保存起来
  FlutterError.onError = (FlutterErrorDetails details) {
    onError?.call(details); //调用默认的onError
    reportErrorAndLog(details); //上报
  };
  runZoned(
  () => runApp(MyApp()),
  zoneSpecification: ZoneSpecification(
    // 拦截print
    print: (Zone self, ZoneDelegate parent, Zone zone, String line) {
      collectLog(line);
      parent.print(zone, "Interceptor: $line");
    },
    // 拦截未处理的异步错误
    handleUncaughtError: (Zone self, ZoneDelegate parent, Zone zone,
                          Object error, StackTrace stackTrace) {
      reportErrorAndLog(details);
      parent.print(zone, '${error.toString()} $stackTrace');
    },
  ),
 );
}
```