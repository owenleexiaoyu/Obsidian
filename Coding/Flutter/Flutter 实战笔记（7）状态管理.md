 `StatefulWidget` 的状态该被谁管理（Widget 本身？父 Widget？都会？还是另一个对象？）应取决于实际情况。
 
 下面是管理状态的三种最常见的方法：
 
 - Widget 管理自己的状态
 - Widget 管理子 Widget 的状态
 - 混合管理（父 Widget 和子 Widget 都管理状态）

如何决定使用哪种管理方法？官方有如下一些原则供参考：

- 如果状态是用户数据，如复选框的选中状态、滑块的位置，则gai'zhuang'tai