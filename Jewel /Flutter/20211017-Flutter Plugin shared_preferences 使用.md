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

```dart
/// 读方法
// 返回所有的key
Set<String> getKeys();
// 返回一个key的值，不确定类型
Object? get(String key);
bool? getBool(String key);
int? getInt(String key);
double? getDouble(String key);
String? getString(String key);
List<String>? getStringList(String key);

/// 写方法
Future<bool> setBool(String key, bool value);
Future<bool> setInt(String key, int value);
Future<bool> setDouble(String key, double value);
Future<bool> setString(String key, String value);
Future<bool> setStringList(String key, List<String> value);
Future<bool> remove(String key);
Future<bool> clear();
```

### 示例

我们写一个 Flutter 官方模版的计数器示例，和模板不同的是，计数值 count 会被保存到设备的磁盘中，下次打开这个计数器，依然会显示之前的 count。

```dart
import 'package:flutter/material.dart';
import 'package:shared_preferences/shared_preferences.dart';

class SpPluginLibPage extends StatefulWidget {
  @override
  _SpPluginLibPageState createState() => _SpPluginLibPageState();
}
class _SpPluginLibPageState extends State<SpPluginLibPage> {
  static const String KEY_COUNT = "sp_key_count";
  /// 计数值
  int count = 0;
  Future<SharedPreferences> spFuture = SharedPreferences.getInstance();

  @override
  void initState() {
    super.initState();
    readCount();
  }

  @override
  Widget build(BuildContext context) {
    return Scaffold(
      appBar: AppBar(
        title: Text("SharedPreferences Demo"),
      ),
      body: Padding(
        padding: EdgeInsets.all(20),
        child: Center(
          child: Text(
            "你一共点击了 $count 次"
          ),
        ),
      ),
      floatingActionButton: FloatingActionButton(
        onPressed: () {
          storeCount(count + 1);
        },
        child: Icon(Icons.add),
      ),
    );
  }

  void readCount() async {
    SharedPreferences sp = await spFuture;
    setState(() {
      count = sp.getInt(KEY_COUNT) ?? 0;
    });
  }

  void storeCount(int count) async {
    SharedPreferences sp = await spFuture;
    await sp.setInt(KEY_COUNT, count);
    setState(() {
      this.count = count;
    });
  }
}
```

