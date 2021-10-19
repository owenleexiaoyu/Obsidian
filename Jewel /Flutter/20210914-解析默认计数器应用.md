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

### 导入包

```dart
import 'package:flutter/material.dart';
```

这行代码是导入了 Material UI 组件库，Flutter 默认提供了一套丰富的 Material 风格的 UI 组件。

### 应用入口

```dart
void main() => runApp(MyApp());
```

Flutter 中 `main` 函数是应用程序的入口。调用了 `runApp` 函数，它的功能是启动 Flutter 应用。runApp 接受一个 `Widget` 类型的参数，示例中是一个 `MyApp` 对象，MyApp 是 Flutter 应用的根组件。

### 应用根 Widget：MyApp

```dart
class MyApp extends StatelessWidget {
  @override
  Widget build(BuildContext context) {
    return new MaterialApp(
      //应用名称  
      title: 'Flutter Demo', 
      theme: new ThemeData(
        //蓝色主题  
        primarySwatch: Colors.blue,
      ),
      //应用首页路由  
      home: new MyHomePage(title: 'Flutter Demo Home Page'),
    );
  }
}
```

- MyApp 是一个 `Widget`，继承自 `StatelessWidget`，表示一个无状态的组件。
- Flutter 中，所有的 UI 都是 Widget，包括对齐、填充、布局等。
- Widget 的主要工作是提供一个 `build` 方法来描述如何构建页面，框架会调用 Widget 的 build 方法来构建所有的 UI。
- `MaterialApp` 是 Material 库提供的 Flutter App 框架，通过它可以设置应用的 `名称、主题、语言、首页、路由列表` 等，也是一个 Widget。

### 首页：MyHomePage

```dart
class MyHomePage extends StatefulWidget {
  MyHomePage({Key key, this.title}) : super(key: key);
  final String title;
  @override
  _MyHomePageState createState() => new _MyHomePageState();
}

class _MyHomePageState extends State<MyHomePage> {
 ...
}
```

- MyHomePage 是应用的首页，继承自 `StatefulWidget`，表示一个有状态的组件。
- Flutter 中的 Widget 可以分为两类：StatelessWidget 和 StatefulWidget。StatefulWidget 可以拥有状态，这些状态在 Widget 生命周期中是可以变的，而 StatelessWidget 是不能变的。
- StatefulWidget 至少由两个类组成：
	- 一个 StatefulWidget 类，如：MyHomePage
	- 一个 `State` 类，如：`_MyHomePageState`

- 和 StatelessWidget 不同，StatefulWidget 中没有 build 方法，而是被移到对应的 State 类中。

#### 首页 State：_MyHomePageState

接下来看看 `_MyHomePageState` 类中都包含哪些东西。

1. 该组件的状态，示例中只需要维护一个点击次数，所以定义一个 `_counter` 变量。

```dart
int _counter = 0; //用于记录按钮点击的总次数
```

2. 设置状态的自增函数

```dart
void _incrementCounter() {
  setState(() {
     _counter++;
  });
}
```

当按钮点击时，会调用这个函数，这个函数先自增 `_counter`，然后调用 `setState` 方法，这个方法会通知 Flutter 框架，有状态发生了变化。Flutter 框架收到通知后，会执行 build 方法来根据新的状态重新构建页面。

3. 构建 UI 界面

构建 UI 界面的方法在 build 方法中：

```dart
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
      onPressed: _incrementCounter, // 浮动按钮点击后，会执行_incrementCounter 函数
      tooltip: 'Increment',
      child: new Icon(Icons.add),
    ),
  );
}
```

- `Scaffold` 是 Material 库提供的页面脚手架，它提供了默认的导航栏（appBar）、主屏幕（body）、浮动按钮（floatingActionButton）。
-  `Center` 组件可以将其子组件对齐到屏幕中心。
-  `Column` 组件将所有子组件纵向排列。
-  `Text` 组件用来显示文本。
-  `FloatingActionButton` 组件用来展示浮动按钮，接受一个 `onPressed` 点击事件的参数。

#### 将 build 放在 State 中的原因

为什么 `build()` 方法要放在 State（而不是 `StatefulWidget`）中 ？这主要是为了提高开发的灵活性。如果 build 方法放在 StatefulWidget 中会存在两个问题。

- 在 StatefulWidget 中访问状态不方便

状态是保存在 State 中的，在构建 StatefulWidget 的 UI 时，肯定需要访问状态，build 方法就需要一个 State 的变量，同时 State 中的状态也需要声明成 public 的，这样可能会有其他地方修改了状态，变得不可控。

- 继承 StatefulWidget 不方便