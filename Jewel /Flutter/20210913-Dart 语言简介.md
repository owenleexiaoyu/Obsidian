> 这篇笔记只是总结《Flutter 实战》第 1.4 章的内容，所以并不包含所有的 Dart 语言特性，只有一些最常使用的。关于 Dart 语言，后续会有一个专门的系列笔记来记录。

## 变量声明

### var

用于声明一个变量，可以接收任意类型的变量，编译器会智能推断出变量类型。Dart 是静态语言，变量一旦赋值，类型就不可改变了。

```dart
var t;
t = "Hello world";
t = 100; // 报错，t 的类型已经是 String 类型了，用 int 的 100 给其赋值会失败
```

### dynamic 和 Object

`Object` 是 Dart 所有类（包括 `Function` 和 `Null`）的父类，所以任何类型的数据都可以赋值给 Object 声明的变量。`dynamic` 和 var 一样是关键词，用来声明变量，不同的是，dynamic 声明的变量后期可以改变赋值类型。

```dart
dynamic t = "hello world";
Object a = "hello object";
// 以下的赋值语句没有问题
t = 100;
a = 200;
```

dynamic 和 Object 不同的是，dynamic 声明的对象编译器会提供所有可能的组合，而 Object 声明的对象只能使用 Object 的属性和方法，否则编译器会报错。

```dart
main() {
	dynamic a = "hi";
	Object b = "hello";
	print(a.length); // 不报错
	print(b.length); // 报错，提示 length 方法没有在 Object 类中声明
}
```

使用 dynamic 要格外注意它所赋值的数据的类型，以防出现运行时错误。

### final 和 const

如果不打算更改一个变量，那么使用 `final` 或 `const`，而不是 var。一个 final 变量只能被赋值一次，const 变量在声明时就必须赋值。两者的区别是：const 变量是一个编译时常量，final 变量在第一次使用时被初始化。被 final 或 const 修饰的变量，变量类型可以省略。

```dart
final str;
str = "hello";
// 等价于 final String str = "hello";

const str1 = "world";
// 等价于 const String str1 = "world";
```

## 函数

Dart 是一门真正面向对象的语言，所以即使是函数也是对象，并且有一个类型 `Function`。这意味着函数可以赋值给变量或者作为参数传递给其他函数，这是函数式编程的典型特征。

1. 函数声明

```dart
int add(int a, int b) {
	return a + b;
}
```

Dart 函数声明如果没有显示声明返回值类型会默认当作 `dynamic` 处理，注意，函数返回值没有类型推断。

2. 对于只包含一个表达式的函数，可以使用 `=>` 代替 `return`。

上面的例子可以写成：

```dart
int add(int a, int b) => a + b;
```

3. 函数作为变量

```dart
var say = (str) {
	print(str);
}
say("hello");
```

4. 函数作为参数传递

```dart
void execute(var callback) {
	callback();
}

execute(() => print("callback"));
```

5. 可选的位置参数

用 `[]` 包装一组函数的参数，这些参数是可选的位置参数，放在参数列表的最后面：

```dart
String say(String from, String msg, [String device]) {
  var result = '$from says $msg';
  if (device != null) {
    result = '$result with a $device';
  }
  return result;
}
```

不带可选参数调用 say 函数：

```dart
say('Bob', 'Howdy'); //结果是： Bob says Howdy
```

用带可选参数调用 say 函数：

```dart
say('Bob', 'Howdy', 'smoke signal'); //结果是：Bob says Howdy with a smoke signal
```

6. 可选的命名参数

用 `{}` 包装一组函数的参数，这些参数是可选的命名参数，放在参数列表的最后面：

```dart
void enableFlags({bool bold, bool hidden}) {
    // ... 
}
```

调用函数时，可以使用指定命名参数，如：`paramName: value`

```dart
enableFlags(bold: true, hidden: false);
```

可选命名参数在 Flutter 中使用非常多。

**注意：不能同时使用可选的位置参数和可选的命名参数。**

## 异步支持

Dart 中有很多返回 `Future` 或 `Stream` 对象的函数，这些函数被称为 `异步函数`：它们会在设置好一些耗时操作（比如 IO 操作）之后返回，而不是等到这个操作完成。

### Future

`Future` 和 JS 中的 Promise  非常相似，表示一个异步操作的最终完成（或失败）的结果。它是用来处理异步操作的，异步处理成功了就执行成功的操作，异步处理失败了就捕获错误或停止后续操作。一个 Future 只会对应一个结果，要么成功，要么失败。

> Future 所有 API 的返回值还是一个 Future 对象，可以很方便地链式调用。

#### Future.then

异步任务成功，会执行 `Future.then` 中的代码，可以接受到异步的结果。

```dart
Future.delayed(new Duration(seconds: 2), () {
	return "hello dart";
}).then((data) {
	print(data);
});
```

#### Future.catchError

异步任务发生错误，可以在 `catchError` 中捕获错误。

```dart
Future.delayed(new Duration(seconds: 2), () {
	// return "hello dart";
	throw AssertionError("Error");
}).then((data) {
	// 执行成功会走到这里
	print(data);
}).catchError((e) {
	// 执行失败会走到这里
	print(e);
});
```


除了使用 catchError 来捕获异常外，`then` 方法还有一个可选参数 `onError`，也可以用来捕获异常。

```dart
Future.delayed(new Duration(seconds: 2), () {
	// return "hello dart";
	throw AssertionError("Error");
}).then((data) {
	// 执行成功会走到这里
	print(data);
}, onError((e) {
	// 执行失败会走到这里
	print(e);
});
```


#### Future.whenComplete

无论成功还是失败，都会调用到 `whenComplete`，可以在这里做一些异步任务结束（不管成功还是失败）时要做的事情。

```dart
Future.delayed(new Duration(seconds: 2), () {
	// return "hello dart";
	throw AssertionError("Error");
}).then((data) {
	// 执行成功会走到这里
	print(data);
}).catchError((e) {
	// 执行失败会走到这里
	print(e);
}).whenComplete(() {
	// 无论成功还是失败都会走到这里
	print("future complete");
});

// 这段代码输出结果
// Error
// future complete
```


#### Future.wait

对于需要**等待多个异步任务都执行结束后才做逻辑**的场景。例如，一个界面，需要先请求两个网络接口，将两个请求的结果做处理后再展示在界面上。可以使用 `Future.wait`。它接受一个 Future 数组，只有数组中所有 Future 都执行成功，才会触发 then 的回调，只要有一个 Future 失败，就会触发错误回调（catchError 或 onError）。


```dart
Future.wait([
  // 2秒后返回结果  
  Future.delayed(new Duration(seconds: 2), () {
    return "hello";
  }),
  // 4秒后返回结果  
  Future.delayed(new Duration(seconds: 4), () {
    return " world";
  })
]).then((results){
  print(results[0]+results[1]);
}).catchError((e){
  print(e);
});
```


### async 和 await

代码中有大量异步逻辑，并且出现异步任务依赖其他异步任务的结果时，就会出现 Future.then 回调中套回调的情况，这种就被称为 `回调地狱`。例如：有个场景是，用户先登录，登陆成功后获得 userId，然后通过 userId 去请求用户数据，获取到用户数据后将其缓存在文件中。代码如下：

首先定义三个 Future 任务：

```dart
Future<String> login(String username, String password) {
	// 用户登录
	// ...
}

Future<String> getUserInfo(String userId) {
	// 获取用户信息
	// /...
}

Future saveUserInfo(UserInfo userInfo) {
	// 缓存用户信息
}
```

接下来，用代码来完成上面的需求：

```dart
login("owen", "123456").then((id) {
	getUserInfo(id).then((userInfo) {
		saveUserInfo(userInfo).then(() {
			// 执行其他操作
		});
	});
});
```

可以看到，这种回调套回调的代码可读性是非常差的，出错率也会变高，变得很难维护。所以我们需要想办法消除这种回调地狱。

#### 使用 Future 消除回调地狱

上面提到，Future 所有 API 的返回值仍然是个 Future 对象，可以方便地链式调用，我们可以利用这一点来避免嵌套。

```dart
login("owen", "123456").then((id) {
	return getUserInfo(id);
}).then((userInfo) {
	return saveUserInfo(userInfo);
}).then(() {
	// 执行其他操作
}).catchError((e) {
	// 错误处理
	print(e);
});
```

#### 使用 async/await 消除回调地狱

async/await 可以让异步任务的代码看起来和同步代码一样。

```dart
task() async {
	try {
		String userId = await login("owen", "123456");
		UserInfo userInfo = await getUserInfo(id);
		await saveUserInfo(userInfo);
		// 执行接下来操作
	} catch(e) {
		// 错误处理
		print(e);
	}
}
```

- `async` 表示函数是异步的，会返回一个 Future 对象，可以用 then 方法添加回调。
- `await` 后面是一个 Future，表示等待该异步任务完成，异步完成后才会往下走；await 必须出现在 async 函数内部。

> Dart 中的 `async/await` 的作用和 JS 中的 `async/await` 是一模一样的。无论是在 JS 还是在 Dart 中，async/await 都只是一个语法糖，编译器或解释器会将其转化为一个 Promise（Future）的调用链。

## Stream

`Stream` 也是用于接收异步事件数据，和 Future 不同的是，它可以接受多个异步操作的结果（成功或失败）。也就是说，在执行异步任务时，可以多次触发成功或失败事件来传递结果或错误异常。Stream 常用于会多次读取数据的异步人物场景，如网络内容下载、文件读写等。

```dart
Stream.fromFutures([
	// 1s后返回结果
	Future.delayed(new Duration(seconds: 1), () {
		return "hello 1";
	}),
	// 抛出一个异常
	Future.delayed(new Duration(seconds: 2), () {
		throw AssertionError("Error");
	}),
	// 3s后返回结果
	Future.delayed(new Duration(seconds: 3), () {
		return "hello 3";
	})
]).listen((data) {
	print(data);
}, onError: (e) {
	print(e.message);
}, onDone: () {
	
}); 
```

上面的代码会输出：

```dart
hello 1
Error
hello 3
```
