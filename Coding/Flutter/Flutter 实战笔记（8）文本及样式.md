## Text 组件

`Text` 用于显示文本，包含了一些控制文本显示样式的属性。

我们来看一些常见的属性：

- `textAlign`：文本的对齐方式，可以选择左对齐、右对齐、或者居中。有以下值可选：
	- TextAlign.left：左对齐
	- TextAlign.right：右对齐
	- TextAlign.center：居中
	- TextAlign.justify：两端对齐，不足一行时，字符间距会被拉伸来填满一行
	- TextAlign.start：和文字方向有关，ltr 下左对齐，rtl 下右对齐
	- TextAlign.end：和文字方向有关，ltr 下右对齐，rtl 下左对齐

> 注意：对齐的参考系是 Text 本身，所以当文本内容不足一行（小于 Text 组件宽度）时，是没有效果的。

- `maxLines`：指定文本显示的最大行数，默认情况下，文本是自动折行的。
- 
