在移动开发中，经常会出现跨页面通知，常用的有`EventBus`、`Provider`、`BLoc`、`Redux`

# EventBus

`EventBus`是统一定义一个全局事件总线，并使用单例模式，维护一条事件总线的写法。



# Provider

`Provider`是一个简单、轻量级的状态管理库，是基于`InheritedWidget`和`ChangeNotifier`来实现Widget数共享数据的一种实现方法，适用于小型应用。里面没有太多架构代码，单纯的只是基于方法的回调。



# Bloc

`Bloc`是一种基于流的状态管理库，基本思想是将业务逻辑和界面分开，通过将数据源和UI组件之间的交互抽象成为输入和输出，将状态变更和UI更新解耦。所以`Bloc`一般是由两部分组成，一个是数据流的处理器：负责将输入数据转换为输出数据；另一个是界面组件，负责接收输入数据和显示；相较于复杂项目，它还需要和其他库进行配合，比如`RxDart`、`StreamBuilder`等



# Redux

`Redux`是一种单项数据流的状态管理，最初是为`React`应用程序设计的。核心思想是将应用程序的状态都存储在一个全局的`Store`中，内部的状态变更只允许通过分发`action`的方式来修改，当`action`分发到`Store`时，`Redux`的`Reducer`函数会根据`action`类型结合当前状态返回一个新的状态，这个新的状态会存储到`Store`中，并且会通知所有订阅者，`Redux`同样可以和其他库或者框架集成，比如`flutter_redux`、`redux_thunk`、`redux_observable`





# 建议

一般对于小型项目来说，`Provider`和`Bloc`就已经足够了，对于大型的复杂的，可以考虑使用`redux`