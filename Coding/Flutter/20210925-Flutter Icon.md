### Icon

Flutter 中，可以使用 iconfont，即「字体图标」，它是将图标做成字体文件，然后通过指定不同的字符来展示不同的图片。

和图片相比，使用 iconfont 有如下优势：

-  体积小：可以减少安装包大小
- 矢量的：iconfont 都是矢量图标，放大不会影响清晰度
- 可以应用文本样式：可以像文本一样改变字体图标的颜色、大小、对齐等
- 可以通过 TextSpan 和文本混用

#### 使用 Material Design 字体图标

Flutter 默认包含了一套 Material Design 的字体图标，在 `pubspec.yaml` 中配置如下：

```yaml
flutter:
  uses-material-design: true
```

Material Design 所有字体图标可在其 [官网](https://fonts.google.com/icons?selected=Material+Icons) 查看。

每个字体图标都是一个字符，可以直接使用 `Text` 组件传入这个字符，即可显示：

```dart
Text(
  "\uE914 \uE000 \uE90D",
  style: TextStyle(
      fontFamily: "MaterialIcons",
      fontSize: 24,
      color: Colors.blue),
)
```

效果如下：

![MaterialIcons](https://gitee.com/owenlee233/image_store/raw/master/202109251647442.png)

不过这种方式对开发很不友好，需要记住每个图标对应的字符。所以 Flutter 封装了 `Icon` 和  `IconData` 来专门显示字体图标。IconData 中包含了 iconfont 的字符码点信息和字体信息。 `Icons` 类中包含了所有 Material Design 图标的 `IconData` 静态变量定义。可以直接通过 icon 的名字来使用它。

```dart
Row(
  children: <Widget>[
    Icon(Icons.accessible,color: Colors.green),
    Icon(Icons.error,color: Colors.green),
    Icon(Icons.fingerprint,color: Colors.green),
  ],
)
```

效果如下：

![iconfont](https://gitee.com/owenlee233/image_store/raw/master/202109251654415.png)

#### Icon 中的属性解析

Icon 中常见的属性有如下几个：

- `【IconData】icon`：图标数据

- `【Color?】color`：图标的颜色
- `【double?】size`：图标的大小

#### 使用自定义字体图标

我们也可以使用自定义字体图标，iconfont.cn 上有很多字体图标素材，可以将字体图标打包下载后，将 `ttf` 格式的字体文件导入工程中。字体图标下载方式：[IconFont字体图标的下载与使用](https://blog.csdn.net/wingchiehpih/article/details/105875227)

假设需要一个图书和微信的图标，我们的步骤如下：

1. 打包下载后把 ttf 格式的字体文件放在项目 `fonts` 目录下
2. 和使用其他字体一样，在 `pubspec.yaml` 中配置字体

```yaml
fonts:
  - family: "MyIcon"  #指定一个字体名
    fonts:
      - asset: fonts/iconfont.ttf
```

3. 为了使用方便，定义一个 `MyIcons` 类，功能和 Icons 类一样，将字体文件中的所有图标都定义为静态变量。

```dart
class MyIcons{
  // book 图标
  static const IconData book = const IconData(
      0xe614, 
      fontFamily: 'MyIcon', 
      matchTextDirection: true
  );
  // 微信图标
  static const IconData wechat = const IconData(
      0xec7d,  
      fontFamily: 'MyIcon', 
      matchTextDirection: true
  );
}
```

4. 用 `Icon` 展示图标

```dart
Row(
  children: <Widget>[
    Icon(MyIcons.book,color: Colors.purple),
    Icon(MyIcons.wechat,color: Colors.green),
  ],
)
```

效果如图：

![myicon](https://gitee.com/owenlee233/image_store/raw/master/202109251706821.png)

