# 基础





# Null

`dart`在推出空安全以前可以理解`null`也属于`Object`类型，推出之后改了，继承关系如下

![image description](../../imgs/dart_empty.png)

这里特别需要注意，`Null`和`null`的区别，`Null`是一种类型，而`null`表示的是一个值。而`x is T`语法表示的是检查`x`是否是类型`T`，对于`null`这个实例来说，它并不属于。实际上观察源码可以看出`is`参数需要传入的是一个值或者说实例，不建议用于类型之间的继承关系判断，虽然编译器不会报错，但这不是正确的用法。

```dart
bool is<T>(Object? value) => value is T;
```

比如使用`Null is Object`返回的是true，但这并不正确。