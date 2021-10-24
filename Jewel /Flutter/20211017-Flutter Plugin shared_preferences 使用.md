在 Flutter 中如果想保存一些简单的数据，我们需要使用 [shared_preferences](https://pub.dev/packages/shared_preferences) 插件，它可以用来持久化 key-value 格式的数据。

`shared_preferences` 插件在 Android 上使用 SharedPreferences，iOS 上使用 NSUserDefaults，数据会异步地存到设备磁盘中。

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

我们写一个类似于 Flutter 官方模版的计数器示例，按下 FloatingButton 时，count 会增加 1，和模板不同的是，我们的示例中，count 会被保存到设备的磁盘中，下次打开这个计数器，依然会显示之前的 count。

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

### 源码解读

接下来来看看这个插件的相关源码，理解插件内部的工作机制。

先来看 `SharedPreferences.getInstance()` 方法：

```dart
/// Loads and parses the [SharedPreferences] for this app from disk.
///
/// Because this is reading from disk, it shouldn't be awaited in
/// performance-sensitive blocks.
static Future<SharedPreferences> getInstance() async {
  if (_completer == null) {
    final completer = Completer<SharedPreferences>();
    try {
      final Map<String, Object> preferencesMap =
          await _getSharedPreferencesMap();
      completer.complete(SharedPreferences._(preferencesMap));
    } on Exception catch (e) {
      // If there's an error, explicitly return the future with an error.
      // then set the completer to null so we can retry.
      completer.completeError(e);
      final Future<SharedPreferences> sharedPrefsFuture = completer.future;
      _completer = null;
      return sharedPrefsFuture;
    }
    _completer = completer;
  }
  return _completer!.future;
}
```

getInstance 方法中，先判断 _completer 是否为空，不为空时，直接返回它的 future；为空时，会调用 `_getSharedPreferencesMap` 方法，获取到 SP 中存储的所有 key-value，保存到 `_preferenceCache` 中，读取时，直接从内存 cache 中读取即可，速度更快，并将结果设置给 _completer，方便下次直接获取实例。

再来看看 `_getSharedPreferencesMap` 方法：

```dart
static Future<Map<String, Object>> _getSharedPreferencesMap() async {
  final Map<String, Object> fromSystem = await _store.getAll();  // 1
  assert(fromSystem != null);
  // 2 Strip the flutter. prefix from the returned preferences.
  final Map<String, Object> preferencesMap = <String, Object>{};
  for (String key in fromSystem.keys) {
    assert(key.startsWith(_prefix));
    preferencesMap[key.substring(_prefix.length)] = fromSystem[key]!;
  }
  return preferencesMap;
}
```

注释 1 的位置调用了 `_store.getAll()` 从设备中获取到所有的 key-value，
