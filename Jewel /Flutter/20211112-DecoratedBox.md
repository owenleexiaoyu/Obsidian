## 装饰容器 DecoratedBox

`DecoratedBox` 可以在子组件绘制前（或后）绘制一些装饰，比如背景、边框、渐变、圆角等。

来看看 DecoratedBox 的定义：

```dart
const DecoratedBox({
  Key? key,
  required this.decoration,
  this.position = DecorationPosition.background,
  Widget? child,
})
```

- `【Decoration】decoration`：表示要绘制的装饰，Decoration是个抽象类，定义了一个接口 `createBoxPainter`，子类实现它来创建一个画笔，用于装饰绘制，通常使用 `BoxDecoration`。
- `【DecorationPosition】position`：决定是绘制前景还是背景，有如下两个枚举值：
  - DecorationPosition.background：默认值，在子组件绘制之前绘制，背景
  - DecorationPosition.foreground：在子组件绘制之后绘制，前景

## BoxDecoration

