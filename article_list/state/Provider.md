
# 简介



Flutter官方在`Google IO`大会上推荐的状态管理方式之一。原来google官方发布的`Flutter-provide`基本不会被用到了。

# 原理

`Provider`内部的`DlegateWidget`是一个`StatefulWidget`，所以具有状态更新的生命周期。其中的状态共享使用的是`InheritedProvider`类，这个类是基于`InheritedWidget`实现的。



# 使用

导包

```yaml
dependencies:
  provider: 6.0.5
```

编写需要监听的泛型

```dart
class CountModel with ChangeNotifier{

  int count = 0;

  int add(){
    count++;
    // 通知数据发生变更
    notifyListeners();
    return count;
  }

}
```

编写状态监听

```dart
class ProviderWidget extends StatelessWidget {
  const ProviderWidget({Key? key}) : super(key: key);

  @override
  Widget build(BuildContext context) {
    return ChangeNotifierProvider<CountModel>(
      // 创建可供监测的对象
      create: (context){return CountModel();},
      child: Scaffold(
        body: Column(
          children: [
            // 监听对象变更
            Consumer<CountModel>(builder: (context,countModel,child){
              return Text('${countModel.count}');
            })
          ],
        ),
        // 监听对象变更
        floatingActionButton: Consumer<CountModel>(builder: (context,countModel,child){
          return FloatingActionButton(onPressed: (){
            countModel.add();
          });
        }),
      ),
    );
  }
}
```



# 注意事项

> 1. 不要将所有状态放在全局，不要的资源及时释放
> 2. 控制刷新范围：尽量使用Consumer和MultiProvider组合以减小刷新范围