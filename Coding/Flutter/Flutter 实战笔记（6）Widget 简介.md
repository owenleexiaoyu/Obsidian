## Widget 的概念

Flutter 中几乎所有的对象都是一个 Widget，不仅包含 UI 控件（如 Text、Button），还包含一些功能性的组件，如手势检测的 `GestureDetector`、表示 App 主题的 `Theme` 等。

## Widget 和 Element

- `Widget` 是描述一个 UI 元素的配置数据，并不是最终绘制在屏幕上的显示元素。`Element` 才是。
- Widget 是 Element 的配置数据，Widget 树是一个配置树，真正的  UI 渲染树是由 Element 组成。不过并且一个 Widget 可以对应多个 Element。