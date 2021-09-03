Flutter 是 Google 推出并开源的移动应用开发框架，主打 **跨平台、高保真、高性能**，使用 Dart 开发，一套代码可以同时运行在 iOS、Android、PC、Web 等多个平台。Flutter 提供了丰富的组件、接口，开发者可以很快地为 Flutter 添加 Native 扩展。

下面来看看 Flutter 的特点：

## 跨平台自绘引擎

Flutter 使用自己的高性能渲染引擎来绘制 Widget，这样不仅可以保证在 Android 和 iOS 上 UI 的一致性，而且也可以避免对原生控件依赖带来的限制和高昂的维护成本。

Flutter 使用 Skia 作为其 2D 渲染引擎，Skia 是 Google 的一个 2D 图形处理函数库，在字型、坐标转换以及点阵图等方面都有高效且简洁的表现。Skia 是跨平台的，并提供了友好的 API，目前 Google Chrome 浏览器和 Android 均采用 Skia 作为其绘图引擎。

## 高性能

Flutter 的高性能主要靠两点来保证：

-   Dart 语言开发：Dart 在 JIT（即时编译）模式下，速度和 JS 基本持平，但是 Dart 支持AOT，在 AOT 模式下，性能远超 JS。
-   Flutter 使用自己的渲染引擎绘制 UI，布局数据等由 Dart 语言直接控制，所以在布局过程中，不需要像 RN 那样要在 JS 和 Native 之间通信，这在一些拖动、滑动场景下具有明显优势。

## 采用 Dart 语言开发

### JIT 和 AOT

目前程序主要有两种运行方式：静态编译和动态解释。

静态编译：程序在执行前全部被翻译为机器码，通常将这种类型称为 AOT（Ahead of time，提前编译），典型代表是 C、C++

动态解释：解释执行是一句一句边翻译便运行，通常将这种类型称为 JIT（Just in time，即时编译），代表非常多，比如 JS、Python 等。事实上，所有的脚本语言都支持JIT模式。

需要注意的是，JIT 和 AOT 指的是程序运行方式，和语言不是强关联的，有些语言既可以以 JIT 方式运行也可以以 AOT 方式运行，如 Java、Python，他们可以在第一次执行是编译成中间字节码，然后在之后的执行中直接执行字节码。虽然中间字节码并非机器码，在运行时还需将字节码转化为机器码，但通常我们区分是否为 AOT 的标准是看代码在执行前是否需要编译，只要需要编译，无论其产物是字节码还是机器码，都属于 AOT。

### Flutter 为什么选择 Dart 语言？

-   开发效率高
    -   基于JIT的快速开发周期：Flutter在开发阶段采用JIT模式，这样避免了每次改动都要进行编译，极大节省了开发时间
    -   基于AOT的发布包：Flutter在发布时，可以通过AOT生成高效的ARM代码以保证应用性能，JS不具备这个能力。
-   高性能

Flutter旨在提供流畅、高保真的UI体验，需要在每个动画帧中运行大量代码，这意味着需要一种能提供高性能的语言，不会出现丢帧的周期性暂停，Dart支持AOT，这点上做的比JS好

-   快速内存分配

Flutter框架使用函数式流，很大程度上依赖于底层的内存分配器。拥有一个能够有效处理琐碎任务的内存分配器十分重要，Dart也满足这个条件。

-   类型安全

Dart是类型安全的语言，支持静态类型检测，所以可以在编译前发现一些类型的错误，排除潜在问题。

-   Dart团队就在身边

据说这两个团队坐的很近。而由于有Dart团队的积极投入，Flutter团队可以获得更多、更方便的支持。

Flutter的框架结构

如图：

![](https://bytedance.feishu.cn/space/api/box/stream/download/asynccode/?code=YzI0ZjlkNjFiODFmMDhlMWRkY2RiODU4Zjg2YWY2YjdfRUc3OG5nTDBpcEhCMGVyVnpFMExqVUdLZEd4QVBveWZfVG9rZW46Ym94Y25iRUZNclRnWFBTam9GeDl0TFJTeGhjXzE2MzA2MzEzNzI6MTYzMDYzNDk3Ml9WNA)

Flutter Framework

这是一个纯Dart实现的SDK，实现了一套基础库，自底向上，来介绍一下：

-   底下两层（Foundation 和 Animation、Painting、Gestures）是Dart UI层，对应Flutter中的dart:ui包，是Flutter引擎暴露的底层UI库，提供动画、手势及绘制能力
-   Rendering层，这一层是一个抽象的布局层，依赖于dart UI层，Rendering层会构建一个UI树，当UI树有变化时，会计算出有变化的部分，然后更新UI树，最终将UI树绘制到屏幕上，这个过程类似于React中的虚拟DOM。它是Flutter UI框架最核心的部分，它除了确定每个UI元素的位置、大小外还要进行坐标变换、绘制（调用底层 dart:ui）
-   Widgets 层是Flutter提供的一套基础组件库，在基础组件库之上，还提供了Material和Cupertino两种视觉风格的组件库。平常Flutter开发的大多数场景，只是在和这两层打交道。

Flutter Engine

这是一个纯C++实现的SDK，其中包括了 Skia 引擎，Dart运行时，文字排版引擎等。在代码调用 dart:ui 库时，最终会走到 Engine层，然后实现真正的绘制逻辑。

如何学习Flutter

资源

-   **官网**：阅读Flutter官网的资源是快速入门的最佳方式，同时官网也是了解最新Flutter发展动态的地方

-   **源码及注释**：源码注释应作为学习Flutter的第一文档，Flutter SDK的源码是开源的，并且注释非常详细，也有很多示例，实际上，Flutter官方的SDK文档就是通过注释生成的。源码结合注释可以帮你解决大多数问题。

-   **Github**：如果遇到的问题在StackOverflow上也没有找到答案，可以去github flutter 项目下提issue

-   **Gallery源码**：Gallery是Flutter官方示例APP，里面有丰富的示例，读者可以在网上下载安装。Gallery的源码在Flutter源码“examples”目录下。

社区

-   StackOverflow
-   Flutter中文网社区：https://flutterchina.club，上面提供了Flutter官网的文档翻译、开源项目、及案例
-   博客

总结

有了资料和社区后，还需要多动手、多实践。