`Transform` 可以在子组件绘制时对它应用一些矩阵变换来实现一些特效，比如缩放、旋转、平移等。`Matrix4` 是个 4D 矩阵，可以用来实现各种矩阵操作。

## 平移

`Transform.translate` 接收一个 `offset` 参数，可以让子组件在绘制时沿 `x`、`y` 轴平移指定的距离。

```dart
DecoratedBox(
  decoration:BoxDecoration(color: Colors.red),
  //默认原点为左上角，左移20像素，向上平移5像素  
  child: Transform.translate(
    offset: Offset(-20.0, -5.0),
    child: Text("Hello world"),
  ),
)
```

效果如图，子组件 Text 向左移动了 20 像素，向上移动了 5 像素：

![translate](https://gitee.com/owenlee233/image_store/raw/master/202111140001645.png)

## 旋转

`Transform.rotate` 可以对子组件进行旋转变换，`angle` 字段指定旋转的角度。

```dart
DecoratedBox(
  decoration:BoxDecoration(color: Colors.red),
  child: Transform.rotate(
    //旋转90度
    angle:math.pi/2 ,
    child: Text("Hello world"),
  ),
)
```

效果如图：

![rotate](https://gitee.com/owenlee233/image_store/raw/master/202111140004801.png)

>  注意：
>
> Transform 的变换是在绘制阶段，而不是在布局阶段，所以这些变换不会影响子组件的宽高大小和在屏幕上的位置，因为这些是在布局阶段就确定下来了。

来看个例子，给上面旋转的 Transform 右边再放置一个组件。

```dart
Row(
  children: [
    Container(
      decoration: BoxDecoration(
        color: Colors.red,
      ),
      margin: EdgeInsets.symmetric(vertical: 50),
      child: Transform.rotate(
        angle: pi / 2,
        child: Text("Hello world"),
      ),
    ),
    Text(
      "你好世界",
      style: TextStyle(color: Colors.green),
    )
  ],
),
```

效果如图：

![rotate](https://gitee.com/owenlee233/image_store/raw/master/202111140034878.png)

### RotateBox

`RotateBox` 组件和`Transform.rotate` 功能相似，也可以对子组件进行旋转变换，但是 RotatedBox 的变换是在布局阶段，会影响在子组件的位置和大小。把上面例子中的 Transform.rotate 换成 RotateBox：

```dart
Row(
  children: [
    Container(decoration: BoxDecoration(
      color: Colors.red,
    ),
      child: RotatedBox(
        quarterTurns: 1, // 旋转 90 度，1/4圈
        child: Text("Hello world"),
      ),),
    Text("你好世界", style: TextStyle(
        color: Colors.green
    ),)
  ],
),
```

效果如图：

![RotateBox](https://gitee.com/owenlee233/image_store/raw/master/202111140038726.png)

## 缩放

`Transform.scale` 可以对子组件进行缩小或放大，`scale` 字段用来表示缩放比例。

```dart
DecoratedBox(
  decoration:BoxDecoration(color: Colors.red),
  child: Transform.scale(
    scale: 1.5, //放大到1.5倍
    child: Text("Hello world")
  )
);
```

效果如图：

![scale](https://gitee.com/owenlee233/image_store/raw/master/202111140039571.png)

## Transform 的特点

1. 如上面所说，不影响子组件的大小和位置
2. 因为矩阵变换是在绘制阶段，所以在某些场景下，在 UI 需要变化时，可以直接通过矩阵变换来达到视觉上的 UI 改变，而不需要重新触发 build 流程，这样会节省掉重新布局的开销，所以性能会比较好。
   - [[20211019-Flutter 流式布局]] 中 的 Flow 组件内部就是用矩阵变换来更新 UI
   - Flutter 中的动画也大量使用了 Transform 来提高性能

