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

