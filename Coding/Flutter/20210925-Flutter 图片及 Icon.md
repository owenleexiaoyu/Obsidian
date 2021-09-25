### 图片

Flutter 中使用 `Image` 组件来加载 asset、文件、内存、以及网络中的图片。

#### ImageProvider

`ImageProvider` 是一个抽象类，主要定义了获取图片数据的接口 `load`，从不同的数据源获取图片要实现不同的 ImageProvider。

| ImageProvider 子类 | 作用                |
| ------------------ | ------------------- |
| AssetImage         | 从 asset 中加载图片 |
| NetworkImage       | 加载网络图片        |

#### Image

`Image` 组件有个必选的参数 `image`，对应一个 ImageProvider。

**从 asset 中加载图片**

1. 在 `pubspec.yaml` 中的 `asset` 节点下添加图片的路径

```yaml
  assets:
    - images/avatar.png
```

2. 使用 `Image` 组件加载图片

```dart
// 直接使用，传入 AssetImage
Image(
  image: AssetImage("images/avatar.png"),
  width: 100.0
);
// 使用构造函数 Image.asset()
Image.asset("images/avatar.png",
  width: 100.0,
)
```

**从网络中加载图片**

```dart
// 直接使用，传入 NetworkImage
Image(
  image: NetworkImage(
      "https://avatars2.githubusercontent.com/u/20411648?s=460&v=4"),
  width: 100.0,
)
// 使用构造函数 Image.network()
Image.network(
  "https://avatars2.githubusercontent.com/u/20411648?s=460&v=4",
  width: 100.0,
)
```

#### Image 组件参数解析

Image 主要有如下参数：

- `【double?】width`、`【double?】height`：用于设置图片的宽高，当不指定宽高时，图片会根据当前父容器的限制，尽可能展示其原始大小，如果只指定宽高其中一个，另一个属性默认会按比例缩放，但可以通过 `fit` 属性进行修改。
- `【BoxFit?】fit`：用于在图片的显示控件和图片本身大小不同时，指定图片的适应模式，有如下值：
  - BoxFit.fill：会拉伸充满显示空间，图片本身长宽比会发生变化，图片会变形。
  - BoxFit.cover：会按图片长宽比放大后居中填满显示空间，图片不会变形，超出部分被裁剪。
  - BoxFit.contain：默认 fit 规则，在保证图片本身长宽比不变的情况下，缩放以适应当前显示空间，图片不会变形。
  - BoxFit.fitWidth：图片宽度缩放到显示空间的宽度，高度按比例缩放，然后居中展示，图片不会变形，超出部分被裁剪。
  - BoxFit.fitHeight：图片高度缩放到显示空间的高度，宽度按比例缩放，然后居中展示，图片不会变形，超出部分被裁剪。
  - BoxFit.none：没有适应规则，在显示空间内展示图片，如果图片比显示空间大，则只会显示图片的中间部分，超出部分被裁剪。
  - BoxFit.scaleDown：当图片需要缩放时，和 contain 效果一致，不需要缩放时，和 none 效果一致。

```dart
Column(
  children: BoxFit.values.map((e) {
    return Padding(
      padding: const EdgeInsets.symmetric(vertical: 4.0),
      child: Row(
        children: [
          Container(
            width: 150,
            height: 100,
            child: Image(
              image: img,
              fit: e,
            ),
            decoration: BoxDecoration(color: Colors.yellow),
          ),
          SizedBox(width: 10,),
          Text(e.toString())
        ],
      ),
    );
  }).toList(),
),
```

![BoxFit](https://gitee.com/owenlee233/image_store/raw/master/202109251348330.png)

> BoxFit.scaleDown 的不同效果展示：
>
> ![BoxFit.scaleDown](https://gitee.com/owenlee233/image_store/raw/master/202109251358697.png)

- `【Color?】color` 、`【BlendMode?】colorBlendMode`：绘制图片时，对每个像素进行颜色混合，color 指定要混合的颜色，colorBlendMode 指定混合模式。
- `【ImageRepeat?】repeat`：当图片（可能是缩放后）没有填充满小于显示空间时，指定图片的重复规则，有如下几种值：
  - ImageRepeat.noRepeat：不重复
  - ImageRepeat.repeat：X 轴、Y 轴方向都重复
  - ImageRepeat.repeatX：仅在 X 轴方向重复
  - ImageRepeat.repeatY：仅在 Y 轴方向重复

### Icon

Flutter 中，可以使用 iconfont，即「字体图标」，它是将图标做成字体文件，然后通过指定不同的字符来展示不同的图片。

使用