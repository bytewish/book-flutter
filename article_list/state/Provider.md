
# 简介



Flutter官方在`Google IO`大会上推荐的状态管理方式之一。原来google官方发布的`Flutter-provide`基本不会被用到了。

# 原理

`Provider`内部的`DlegateWidget`是一个`StatefulWidget`，所以具有状态更新的生命周期。其中的状态共享使用的是`InheritedProvider`类，这个类是基于`InheritedWidget`实现的。



# 注意事项

> 1. 不要将所有状态放在全局，不要的资源及时释放
> 2. 控制刷新范围：尽量使用Consumer和MultiProvider组合以减小刷新范围