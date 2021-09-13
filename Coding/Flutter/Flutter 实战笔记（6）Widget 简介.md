## Widget 的概念

Flutter 中几乎所有的对象都是一个 Widget，不仅包含 UI 控件（如 Text、Button），还包含一些功能性的组件，如手势检测的 `GestureDetector`、表示 App 主题的 `Theme`，动画等。

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

- Widget 继承自 `DiagnosticableTree`，DiagnosticableTree（诊断树）的作用是提供调试信息。
- `Key key`：key 的主要作用是决定下一次 build 的时候是否复用旧的 Widget，判断条件在 `canUpdate`。
- `createElement`：Flutter 在构建 UI 树时，会调用这个方法生成对应节点的 Element 对象。
- `debugFillProperties` ：复写父类的方法，主要是设置诊断树的一些特性。
- `canUpdate`：用于在 Widget 树重新 build 时判断能否复用旧的 Widget。是否用新的 Widget 对象去更新旧 UI 树上所对应 Element 对象的配置，从代码中看到，当 newWidget 和 oldWidget 的 runtimeType、key 相同时，就会用 newWidget 去更新 Element 对象的配置，否则会创建新的 Element。


## StatelessWidget

`StatelessWidget` 继承自 Widget，重写了 createElement 方法。

```dart
@override
StatelessElement createElement() => new StatelessElement(this);
```

`StatelessElement` 间接继承自 Element，与 StatelessWidget 对应。

通常在 build 方法中嵌套其他 Widget 来构建 UI。

### BuildContext

build 方法中有个 context 参数，类型是 `BuildContext`，表示当前 Widget 在 Widget 树中的上下文，每个 Widget 都会对应一个 BuildContext 对象。BuildContext 是对 Widget 树操作的一个句柄。例如可以从当前 Widget 向上遍历 Widget 树，或者按照 Widget 类型查找父级 Widget。

## StatefulWidget

`StatefulWidget` 同样继承自 Widget，并重写了 createElement 方法。返回的是 `StatefulElement`。另外 StatefulWidget 还添加了一个新的接口 `createState`，在创建 StatefulWidget 时，需要重写这个方法。这个方法用于创建和 StatefulWidget 有关的状态，在 StatefulWidget 生命周期中可能会被多次调用，例如，当一个 StatefulWidget 同时插入到Widget 树的多个位置时，Flutter 就会调用该方法为每个位置生成一个单独的 State 示例，是一个 StatefulElement 对应一个 State。

```dart
abstract class StatefulWidget extends Widget {
  const StatefulWidget({ Key key }) : super(key: key);
    
  @override
  StatefulElement createElement() => new StatefulElement(this);
    
  @protected
  State createState();
}
```

## State

`State` 表示 StatefulWidget 中需要维护的状态，State 中保存的信息可以：
1. 在 Widget 构建时被同步读取
2. 在 Widget 生命周期中被修改，修改后调用 `setState` 方法，通知 Flutter Framework 状态发生改变，Flutter Framework 会重新调用 build 方法构建 Widget 树，更新 UI。

State 中有两个常用属性：
1. `widget`：表示与这个 State 关联的 Widget 实例。
2. `context`：BuildContext 实例

### State 生命周期

下面这张图是 State 的声明周期图：

![State lifecycle](https://gitee.com/owenlee233/image_store/raw/master/202109132353287.png)

State 生命周期包含如下回调：

- `initState`：当 Widget 第一次插入到 Widget 树时被调用。对于每个 State 对象，这个方法生命周期内只会调用一次（类似于 Android Activity onCreate）。所以通常在这个回调中做一些时机很早的一次性操作，如状态初始化、订阅子树的事件通知等。不能在这个回调中调用 `BuildContext.dependOnInheritedWidgetOfExactType`（该方法用于在 Widget 树上获取离当前 Widget 最近的父级 InheritFromWidget），原因是初始化完成后，Widget 树中的 InheritFromWidget 可能会发生变化，应该在 build 方法或 `didChangeDependencies` 中调用它。
- `didChangeDependencies`：当 State 对象的依赖发生变化时会被调用。例如：build 中包含了一个 InheritedWidget，并且发生了变化，那么 InheritedWidget 的子 Widget 的 didChangeDependencies 都会被调用。典型场景是当系统语言或主题改变时，Flutter 会通知 Widget 调用这个方法。
- `build`：这个回调是用于构建 Widget 子树的，会在以下场景被调用
	- 在调用 initState 之后
	- 在调用 didUpdateWidget 之后
	- 在调用 setState 之后
	- 在调用 didChangeDependencies 之后
	- 在 State 对象从树中一个位置移除后（会调用 deactivate）又重新插入到树的其他位置后

- `reassemble`：这个回调是给调试用的，在热重载之后被调用，在 release 模式下永远不会被调用。
- `didUpdateWidget`：在  Widget 重新构建时，Flutter 会调用 `Widget.canUpdate` 来判断是否要更新，如果返回 true，则会调用这个回调。
- `deactivate`：当 State 对象从树中被移除是，会调用这个回调。在一些场景下，Flutter 会将 State 对象重新插入到树中。如果移除后没有重新插入到树中，紧接着就会回调 `dispose` 方法。
- `dispose`：当 State 对象从树中被永久移除时调用，通常在这里释放资源。

> **注意**：在继承`StatefulWidget`重写其方法时，对于包含`@mustCallSuper`标注的父类方法，都要在子类方法中先调用父类方法。


## 在 Widget 树中获取 State

因为 StatefulWidget 的具体逻辑都在  State 中，所以很多时候需要获取 StatefulWidget 对应的 State 对象来调用一些方法，比如 `Scoffold` 组件的 `ScoffoldState` 中定义了打开 SnackBar 的方法。

### 通过 BuildContext

`context` 对象有个 `findAncestorStateOfType` 方法，可以从当前节点沿着 Widget 树往上查找指定类型的 StatefulWidget 对应的 State 对象。



### 通过 GlobalKey