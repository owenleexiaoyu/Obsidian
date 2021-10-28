## LayoutBuilder

通过 `LayoutBuilder`，可以在 **布局过程** 中获取到父组件传递的约束信息，然后可以根据约束动态构建不同的布局。

LayoutBuilder 非常实用，它主要有两个使用场景：

1. 可以使用 LayoutBuilder 来根据设备尺寸实现 **响应式布局**。
2. LayoutBuilder 可以帮助开发者高效排查问题，在遇到一个布局问题或者想调试组件树中某个节点布局时的约束就可以使用 LayoutBuilder。

下面从这两个场景我们分别来看些例子：

### 响应式布局

例如我们来实现一个响应式的 Column 组件 `ResponsiveColumn`，它的功能是：当当前可用宽度小于 200 时，将子组件显示成一列，否则显示成两列。

```dart
class ResponsiveColumn extends StatelessWidget {
  
  const ResponsiveColumn({Key? key, required this.children});

  final List<Widget> children;

  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
        builder: (BuildContext context, BoxConstraints constraints) {
      // 这里可以获取到 BoxConstraints 对象，拿到约束信息
      if (constraints.maxWidth < 200) {
        return Column(
          children: children,
          mainAxisSize: MainAxisSize.min,
        );
      } else {
        var _children = <Widget>[];
        for (int i = 0; i < children.length; i += 2) {
          if (i + 1 < children.length) {
            _children.add(Row(
              children: [
                children[i],
                children[i + 1],
              ],
              mainAxisSize: MainAxisSize.min,
            ));
          } else {
            _children.add(children[i]);
          }
        }
        return Column(
          children: _children,
          mainAxisSize: MainAxisSize.min,
        );
      }
    });
  }
}
```

来看看使用效果：

```dart
Column(
  children: [
    DescItem("ResponsiveColumn 效果，父容器没有宽度限制"),
    Container(
      color: Colors.blue.shade50,
      child: ResponsiveColumn(children: [
        Chip(label: Text("Hello Hello Hello Hello")),
        Chip(label: Text("World")),
        Chip(label: Text("OwenLee")),
      ]),
    ),
    DescItem("ResponsiveColumn 效果，父容器有 150 宽度限制"),
    Container(
      color: Colors.blue.shade50,
      width: 150,
      child: ResponsiveColumn(
          children: [
            Chip(label: Text("Hello Hello Hello Hello")),
            Chip(label: Text("World")),
            Chip(label: Text("OwenLee")),
          ]
      ),
    )
  ],
),
```

运行效果如图：

![ResponsiveColumn](https://gitee.com/owenlee233/image_store/raw/master/202110280849265.png)

### 帮助调试

为了便于排查问题，我们可以封装一个能打印父组件传递给子组件约束的组件：

```dart
/// 打印父容器传递给子组件的约束信息
class LayoutLogPrinter<T> extends StatelessWidget {
  const LayoutLogPrinter({
    Key? key,
    this.tag,
    required this.child,
  });

  final Widget child;
  final T? tag;

  @override
  Widget build(BuildContext context) {
    return LayoutBuilder(
        builder: (BuildContext context, BoxConstraints constraints) {
      debugPrint("${tag ?? key ?? child} : $constraints");
      return child;
    });
  }
}
```

看下使用效果：

```dart
Container(
  color: Colors.blue.shade50,
  width: 150,
  child: LayoutLogPrinter(
      tag: "LayoutLog",
      child: Chip(label: Text("Hello Hello Hello Hello"))),
),
```

控制台中会打印出如下信息：

```text
flutter: LayoutLog : BoxConstraints(w=150.0, 0.0<=h<=Infinity)
```

> 注意：使用 LayoutBuilder 的前提是盒模型布局，如果是 Sliver 布局，可以使用 SliverLayoutBuiler。

