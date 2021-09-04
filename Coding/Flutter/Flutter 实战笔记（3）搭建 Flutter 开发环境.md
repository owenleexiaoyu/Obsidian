## 安装 Flutter

由于 Flutter 会同时构建 Android 和 iOS 两个平台的发布包，所以 Flutter 同时依赖 Android 和 iOS SDK，在安装 Flutter 时，也需要安装相应平台的构建工具和 SDK。

### 使用镜像

在国内访问 Flutter 有时可能会受到限制，官方为中国开发者搭建了临时镜像，可以将如下环境变量加到用户环境变量中（临时镜像并不保证一直可用，可以在 https://flutter.cn/ 上查看最新的镜像）：

```
export PUB_HOSTED_URL=https://pub.flutter-io.cn
export FLUTTER_STORAGE_BASE_URL=https://storage.flutter-io.cn
```

### 在 macOS 上搭建 Flutter 开发环境

Window 和 Linux 搭建开发环境可直接参考 [官方文档 Install 章节](https://flutter.dev/docs/get-started/install)。这篇笔记就不写了。

在 macOS 上可以同时进行 Android 和 iOS 设备的测试。

#### 系统要求

操作系统：macOS（64bit）

磁盘空间：2.8GB（不包含 IDE 和工具的空间）

工具：Flutter 使用 git 来安装和升级。

#### 获取Flutter SDK

1. 从 [官方 SDK 下载地址]( https://flutter.dev/docs/development/tools/sdk/releases) 下载 SDK，解压缩到目标文件夹，例如我的是 `~/Workspace/FlutterSDK`，解压完成后，Flutter SDK 的路径为 `~/Workspace/FlutterSDK/flutter`。

2. 配置环境变量：

打开或创建 `~/.bash_profile` 文件（如果使用 zsh 的话，打开或创建 `~/.zshenv`）。

添加如下路径到 PATH：

```
// FLUTTER_INSTALL_PATH 是 Flutter SDK 解压后存放的目录
export PATH=[FLUTTER_INSTALL_PATH]/flutter/bin:$PATH
```

运行 `source ~/.bash_profile` （或 `source ~/.zshenv`）命令刷新当前终端窗口。

3. 验证 flutter/bin 是否已经在 PATH 中，成功的话，可以在其中看到 flutter/bin 的路径。

```
echo $PATH
```

#### 运行 `flutter doctor`

运行以下命令，查看是否有任何需要安装的依赖项来完成设置（对于详细输出，添加 -v 标志）:

```
flutter doctor
```

这个命令会输出目前已经安装了哪些工具，还有哪些需要的工具没有安装，可以根据提示进行安装，安装成功后，再次运行 flutter doctor 命令。

### IDE 配置和使用

官方推荐使用 Android Studio 或 VS Code 来开发 Flutter。官方提供了这两个 IDE 的插件，通过 IDE 插件可获得代码补全，语法高亮，Widget 编辑辅助、运行和调试支持等功能，提升开发效率。

#### Android Studio 配置和使用

也可以使用 IDEA。

首先需要安装 Flutter 和 Dart 插件

-   Flutter 插件：支持 Flutter 开发工作流（运行、调试、热重载等）
-   Dart 插件：提供代码分析（输入代码时进行验证、代码补全等）

创建 Flutter 应用

1.  选择 File - New Flutter Project
2.  选择 Flutter application，点击 Next
3.  输入项目名称，Flutter SDK 地址，点击 Next
4.  点击 Finish

上述步骤创建了一个 Flutter 项目，其中包含一个使用 Material 组件的演示应用。

代码位于：lib/main.dart

运行应用程序

5.  定位到 Android Studio 工具栏

![](https://bytedance.feishu.cn/space/api/box/stream/download/asynccode/?code=Y2E0ZmUyODhjNmExYmM3YzVlZTUyMmExYzI0MGY3ODlfbXJsdTJONnZLWUN4TERvN3JNNGU3UkNKbFhrd3lSQlRfVG9rZW46Ym94Y256WEdGQ0lNaEFob25wb3UwSU1qYlljXzE2MzA2ODg2OTg6MTYzMDY5MjI5OF9WNA)

6.  在 target selector中，选择一个运行该应用的android 设备，如果没有，连接一个android真机或者创建一个android模拟器。
7.  在工具栏中点击 run
8.  如果一切正常，可以在设备上看到启动的应用程序

![](https://bytedance.feishu.cn/space/api/box/stream/download/asynccode/?code=Yzk5Y2Q1MWFiMTQ3NWRjY2E1NjhhY2FjMjI0MmYyODJfc2tINWxiU3g1OHRLR2ZMNDlicmsweGlLMWZFZ0prTHNfVG9rZW46Ym94Y242QldlS3piS3B2RU5NZzBKeGdnSlVnXzE2MzA2ODg2OTg6MTYzMDY5MjI5OF9WNA)

踩坑记录 1

> zsh: command not found: flutter

> zsh: command not found: vim

> 之前在电脑上配置过flutter的path，长这样：

> PATH=/Users/bytedance/Workspace/FlutterSDK/flutter/bin:$PATH:$JAVA_HOME/bin:/Users/bytedance/Library/Android/sdk/platform-tools:/Users/bytedance/Library/Android/sdk/tools:.

> 然后这次配的时候，手贱把 flutter/bin:$PATH:$JAVA_HOME 中的**:$PATH** 删掉了，这样很多bash的命令都用不了，vim、open等

> 解决方案：

> [mac 配置bash时导致基本命令失效的解决办法_|刘钊|的博客-CSDN博客](https://blog.csdn.net/weixin_40200876/article/details/87938005)

> 先输入命令让vim 或者open命令能用，再打开 ~/.bash_profile 文件，将那个 **:$PATH** 加回来

踩坑记录 2

> 编辑完 ~/.bash_profile 文件后，输入 source ~/.bash_profile。输入flutter doctor 还是不起作用。

> 原因：使用的是 zsh 的原因

> 解决方案：在 zsh 中输入 source ~/.bash_profile 然后保存，再在命令行中输入 source ~/.bash_profile 就可以了。

> 或者可以编辑 .zprofile 文件

https://stackoverflow.com/questions/58400500/zsh-command-not-found-flutter

踩坑记录 3

> 输入 flutter doctor 命令，出现这样的报错

> Exception: Flutter failed to create a directory at "/Users/bytedance/.config/flutter". The flutter tool cannot access the file or directory.

> Please ensure that the SDK and/or project is installed in a location that has read/write permissions for the current user.

> 原因：/Users/bytedance/.config/ 这个文件夹，没有读写权限

> 解决方案：[flutter upgrade - failed to create a directory at "/Users/.../.config/flutter". - Flutter Daemon has](https://stackoverflow.com/questions/66601502/flutter-upgrade-failed-to-create-a-directory-at-users-config-flutter)

> 命令行中输入 sudo chown bytedance /Users/bytedance/.config （bytedance是我电脑上的用户名，不同电脑上不同），需要输入密码，成功后，再次输入 flutter doctor 命令就好了。