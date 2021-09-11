## Widget 的概念

Flutter 中几乎所有的对象都是一个 Widget，不仅包含 UI 控件（如 Text、Button），还包含一些功能性的组件，如手势检测的 `GestureDetector`、表示 App 主题的 `Theme` 等。

## Widget 和 Element

- `Widget` 是描述一个 UI 元素的配置数据，并不是最终绘制在屏幕上的显示元素。`Element` 才是。
- Widget 是 Element 的配置数据，Widget 树是一个配置树，真正的 UI 渲染树是由 Element 组成。不过大体上可以认为 Widget 树就是指 UI 控件树或 UI 渲染树。
- 一个 Widget 对象可以对应多个 Element 对象，根据相同一份配置（Widget），可以创建多个实例（Element）

## Widget 主要接口

```dart
@immutable
abstract class Widget extends DiagnosticableTree {
  const Widget({ this.key });
  final Key key;
    
  @protected
  Element createElement();

  @override
  String toStringShort() {
    return key == null ? '$runtimeType' : '$runtimeType-$key';
  }

  @override
  void debugFillProperties(DiagnosticPropertiesBuilder properties) {
    super.debugFillProperties(properties);
    properties.defaultDiagnosticsTreeStyle = DiagnosticsTreeStyle.dense;
  }
  
  static bool canUpdate(Widget oldWidget, Widget newWidget) {
    return oldWidget.runtimeType == newWidget.runtimeType
        && oldWidget.key == newWidget.key;
  }
}
```

- Widget 继承自 DiagnosticableTree，DiagnosticableTree（诊断树）的作用是提供调试信息。
- `Key key`：key 的主要作用是决定下一次 build 的时候是否复用旧的 Widget，判断条件在 `canUpdate`。
- `debugFillProperties` ：复写父类的方法，主要是设置诊断树的一些特性。
- `canUpdate`：用于判断是否用新