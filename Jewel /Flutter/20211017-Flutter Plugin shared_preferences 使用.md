在 Flutter 中如果想保存一些简单的数据，我们需要使用 [shared_preferences](https://pub.dev/packages/shared_preferences) 插件，它可以用来持久化 key-value 格式的数据。

shared_preferences 插件在 Android 上使用 SharedPreferences，iOS 上使用 NSUserDefaults，数据会异步地存到设备磁盘中。

### 使用方式

1. 在 pubspec.yaml 中添加 `shared_preferences` 的依赖。

```yaml
flutter:
	shared_preferences: ^2.0.8
```

2. 获取 SharedPreferences 实例，注意 getInstance() 返回的是 `Future<SharedPreferences>` 类型。如果在一个 Widget 中需要多次获取实例来调用时，可以在 initState 方法中获取到存到变量中。

```dart
SharedPreferences prefs = await SharedPreferences.getInstance();
```

3. 调用 SharedPreferences 的读写方法

