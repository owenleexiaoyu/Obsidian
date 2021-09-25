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

### Image 组件参数解析

Image 主要有如下参数：

- `【double?】width`、`【double?】height`：用于设置图片的宽高，当不指定宽高时，图片会根据当前父容器的限制，尽可能展示其原始大小，如果只指定宽高其中一个，另一个属性默认会按比例缩放，但可以通过 `fit` 属性进行修改。
- `【BoxFit?】fit`：用于在图片的显示控件和图片本身大小不同时，指定图片的适应模式，有如下值：
  - BoxFit.fill
  - BoxFit.cover
  - BoxFit.contain
  - BoxFit.fitWidth
  - BoxFit.fitHeight
  - BoxFit.none
  - BoxFit.scaleDown

