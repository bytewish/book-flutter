# Widget简介

在Flutter中(非Dart)几乎所有的对象都是widget。这里的widget和前端开发中的`控件`是不同的，它的意义更加广泛，不仅包含`UI元素`，还包括一些功能性的组件，比如手势检测的`GestureDetector`，主题参数配置相关的`Theme参数`等。这个概念主要来源于类是否继承自`widget`。  


# StatelessWidget
无状态的Widget，重建无法保留以前状态。


# StatefulWidget
状态：指的是`State`内部的属性，具有状态的Widget

# InheritedWidget
可以共享数据的widget。