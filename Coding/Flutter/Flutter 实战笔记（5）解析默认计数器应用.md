用 Android Studio 创建 Flutter 应用，会默认创建一个计数器模板应用，如图所示：

![Flutter Demo](https://gitee.com/owenlee233/image_store/raw/master/202109042245817.png)

每点击一次右下角的 + 号，屏幕中间的数字就会加 1。

下面来解析一下这个 Demo 的源码。源码在 lib/main.dart 文件中，不同版本可能有差异。

```dart
import 'package:flutter/material.dart';

void main() => runApp(new MyApp());

class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      title: 'Flutter Demo',
      theme: new ThemeData(
        primarySwatch: Colors.blue,
      ),
      home: new MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}

class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;

  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
  int _counter = 0;

  void _incrementCounter() {
    setState(() {
      _counter++;
    });
  }

  @override
  Widget build(BuildContext context) {
    return new Scaffold(
      appBar: new AppBar(
        title: new Text(widget.title),
      ),
      body: new Center(
        child: new Column(
          mainAxisAlignment: MainAxisAlignment.center,
          children: <Widget>[
            new Text(
              'You have pushed the button this many times:',
            ),
            new Text(
              '$_counter',
              style: Theme.of(context).textTheme.headline4,
            ),
          ],
        ),
      ),
      floatingActionButton: new FloatingActionButton(
        onPressed: _incrementCounter,
        tooltip: 'Increment',
        child: new Icon(Icons.add),
      ), // This trailing comma makes auto-formatting nicer for build methods.
    );
  }
}
```


## 分析

1. 导入包

```dart
import 'package:flutter/material.dart';
```

这行代码是导入了 Material UI 组件库，Flutter 默认提供了一套丰富的 Material 风格的 UI 组件。

2. 应用入口

```dart
void main() => runApp(MyApp());
```

Flutter 中 `main` 函数是应用程序的入口。调用了 `runApp` 函数，它的功能是启动 Flutter 应用。runApp 接受一个 `Widget` 类型的参数，示例中是一个 `MyApp` 对象，MyApp 是 Flutter 应用的根组件。

3. 