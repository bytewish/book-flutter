# InheritedWidget

在Flutter的开发过程当中，一个Widget的状态通常是保存在自己的内部的，这意味着它们对状态的修改都只局限于在自己内部，也就是说每个`Widget`中维护的只是一个复本。所以无法做到组件之间的访问、共享状态。比如现在开发一个电商首页的购物车模块，我们对购物车内的每条商品的展示进行了封装，比如叫`GoodsItemWidget`，我们期望使用这个组件进行增加、删除的时候，底部总加购数量会跟着改变，这时候使用`setState`就不那么容易做到了。更别说跨页面消息传递了，比如用户登录之后刷新购物车、消息等功能了。
而`InheritedWidget`的出现就是为了解决这个问题，它允许在Widget树中共享数据，也就是可以多个Widget之间共享数据。它设计的准则是当`InheritedWidget`中的数据发生变化的时候，会通知其他的依赖项。

## demo

创建`InheritedWidget`的实现类

```dart
class CountState extends InheritedWidget {

  final int count;
  final Widget child;
  final VoidCallback addCounter;  //VoidCallback等同于 void Function()
  final VoidCallback removeCounter;

  const CountState({
    Key? key,required this.count,required this.child,required this.addCounter,required this.removeCounter}) : super(key: key, child: child);

  // 通用获取实例声明
  static CountState of(BuildContext context) {
    final CountState? result = context.dependOnInheritedWidgetOfExactType<CountState>();
    return result!;
  }

  // 用来控制是否需要通知
  @override
  bool updateShouldNotify(CountState oldWidget) {
    return count != oldWidget.count;
  }
}

```

创建页面

```dart
class RootWidget extends StatefulWidget{}

class _RootWidgetState extends State<RootWidget>{
    int count = 0;
	
    void addCounter() {
    	setState(() {
      		count++;
    	});
  	}

  	void removeCounter() {
    	setState(() {
      		count--;
    	});
  	}
    
    Widget build(BuildContext context){
        return CountState(
        	count: count,
            addCounter: addCounter,
            removeCounter: removeCounter,
            child: ChildWidget()
        );
    }
}

// 具体UI界面:省略各种样式配置等
class ChildWidget extends StatelessWidget{
    Widget build(BuildContext context){
        final counterState = Counter.of(context);
        return Scaffold(
        	column(
            	children:<Widget>[
                    Text('${counterState.count}'),
                    Row(
                    	FloatingActionButton(onPressed:counterState.addCounter);
                        FloatingActionButton(onPressed:counterState.removeCounter);
                    )
                ]
            )
        );
    }
}
```

