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

`BoxDecoration` 是 Decoration 的子类，实现了常用装饰元素的绘制。

```dart
const BoxDecoration({
  Color? color,                            // 颜色
  DecorationImage? image,                  // 图片
  BoxBorder? border,                       // 边框
  BorderRadiusGeometry? borderRadius,      // 圆角
  List<BoxShadow>? boxShadow,              // 阴影，可以有多个
  Gradient? gradient,                      // 渐变
  BlendMode? backgroundBlendMode,          // 背景混合模式
  BoxShape shape = BoxShape.rectangle,     // 形状
})
```

下面针对各个属性依次举个简单的示例：

- color

```dart
DecoratedBox(decoration: BoxDecoration(
  color: Colors.red
),
child: SizedBox(
  width: 100,
  height: 40,
  child: Center(
    child: Text("Button",style: TextStyle(color: Colors.white),),
  ),
),),
```

效果如图：

![color](https://gitee.com/owenlee233/image_store/raw/master/202111130005921.png)

- image

```dart
DecoratedBox(decoration: BoxDecoration(
    image: DecorationImage(
      image: AssetImage("images/chandler2.jpg"),
      fit: BoxFit.cover
    )
),
  child: SizedBox(
    width: 100,
    height: 40,
    child: Center(
      child: Text("Button",style: TextStyle(color: Colors.white),),
    ),
  ),),
```

效果如图：

![image](https://gitee.com/owenlee233/image_store/raw/master/202111130008852.png)

- border

```dart
DecoratedBox(decoration: BoxDecoration(
    border: Border.all()
),
  child: SizedBox(
    width: 100,
    height: 40,
    child: Center(
      child: Text("Button"),
    ),
  ),),
```

效果如图：

![border](https://gitee.com/owenlee233/image_store/raw/master/202111130009927.png)

- borderRadius

```dart
DecoratedBox(decoration: BoxDecoration(
    color: Colors.red,
  borderRadius: BorderRadius.circular(5)
),
  child: SizedBox(
    width: 100,
    height: 40,
    child: Center(
      child: Text("Button", style: TextStyle(color: Colors.white),),
    ),
  ),),
```

![borderRadius](https://gitee.com/owenlee233/image_store/raw/master/202111130011284.png)

- boxShadow

```dart
DecoratedBox(decoration: BoxDecoration(
    color: Colors.red,
    boxShadow: [
      BoxShadow(color: Colors.red, blurRadius: 10)
    ]
),
  child: SizedBox(
    width: 100,
    height: 40,
    child: Center(
      child: Text("Button", style: TextStyle(color: Colors.white),),
    ),
  ),),
```

效果如图：

![boxShadow](https://gitee.com/owenlee233/image_store/raw/master/202111130021295.png)

- gradient

`Gradient` 是个抽象类，Flutter 提供了三种子类：`LinearGradient`、`RadialGradient`、`SweepGradient`。

```dart
DecoratedBox(decoration: BoxDecoration(
    gradient: LinearGradient(
      colors: [
        Colors.red,
        Colors.yellow
      ]
    )
),
  child: SizedBox(
    width: 100,
    height: 40,
    child: Center(
      child: Text("Button", style: TextStyle(color: Colors.white),),
    ),
  ),),
```

效果如图：

![gradient](https://gitee.com/owenlee233/image_store/raw/master/202111130017419.png)

- shape

BoxShape 是个枚举类，有两个值：BoxShape.rectangle（矩形）和 BoxShape.circle（圆形）。

```
DecoratedBox(decoration: BoxDecoration(
    shape: BoxShape.circle,
  color: Colors.red
),
  child: SizedBox(
    width: 100,
    height: 100,
    child: Center(
      child: Text("Button", style: TextStyle(color: Colors.white),),
    ),
  ),),
```

效果如图：

![shape](https://gitee.com/owenlee233/image_store/raw/master/202111130016409.png)

BoxDecoration 中的属性可以组合起来使用来实现想要的装饰效果。

