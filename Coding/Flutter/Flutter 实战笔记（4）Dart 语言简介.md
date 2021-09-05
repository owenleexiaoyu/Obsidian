这篇笔记只是总结《Flutter 实战》第 1.4 章的内容，所以并不包含所有的 Dart 语言特性，只有一些最常使用的。关于 Dart 语言，后续会有一个专门的系列笔记来记录。

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


## 函数

## 异步支持

## Stream

## Dart 和 Java 及 JS 的对比