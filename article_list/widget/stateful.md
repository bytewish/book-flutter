
# 案例
```dart
class B extends StatefulWidget{
    /* 省略构造函数 */
    _BState createState() => _BState();
}

class _BState extends State<B>{
    int _count = 0;
    
    void changeSomeThing(){
        setState((){
            _count++;
        })
    }
    
    Widget build(BuildContext context){
        return Container(
        	child: Row(
            	children:[
                    Text("123"),
                    const Text("456")
                ]
            )
        );
    }
}
```

# 讲解
对于`StatefulWidget`来说，里面数据发生了变更，它只会去重新调用`B.state.build()`方法，而不会重新创建组件，所以`B.state`里面的属性都还在，也就意味着状态得到了保存。这里特别要注意，属性改变会触发Widget的重新创建，但它内部包裹的并不一定会重建，如上述代码所示：`const Text`组件会复用以前的，并不会重建，因为它并没有任何改变。


# 状态管理
对于一个`StatefulWidget`来说，状态值该的管理一般有三种策略。widget自身管理、widget管理子Widget状态、混合管理(父Widget和子Widget一起管理)。

### 自身管理

如上面代码所示

### 父Widget管理子Widget

父Widget通过`setState`触发子Widget重新刷新来控制子Widget，子Widget通过回调函数来通知父Widget做更新。

```dart
// 父Widget代码
class ParentWidget extends StatefulWidget{
    _ParentWidgetState createState() => _ParentWidgetState();
}

class _ParentWidgetState extends State<ParentWidget>{
    bool _active = false;
    
    void handleTapChanged(bool newVlue){
        setState((){
            _active = newValue;
        })
    }
    
    Widget build(BuildContext context){
        return Container(
        	child:TapBoxB(
            	active:_active,
                onChanged: handleTapChanged
            )
        );
    }
}
```

```dart
// 子Widget代码
class ChildWidget extends StatelessWidget{
    ChildWidget({Key?key,this.active:false,required this.onChanged}):super(key:key);
    
    final bool active;
    final ValueChanged<bool> onChanged;
    
    void _handleTap(){
        onChanged(!active);
    }
    
    Widget build(BuildContext context){
        return GestureDetector(
        	onTap:_handleTap,
            child: Container(
            	child:Text(active? 'Active':'Inactive')
            )
        );
    }
}
```

### 混合管理

一般来说：对于只和自己相关的数据自己控制，和父控件相关的交由父容器控制。比如以下案例：由父容器控制是否显示，子容器控制是否显示边框

```dart
// 父容器
class ParentWidgetC extends StatefulWidget{
    _ParentWidgetCState createState() => _ParentWidgetCState();
}

class _ParentWidgetCState extends State<ParenetWidgetC>{
    bool _active = false;
    
    void handleTapChanged(bool newValue){
        setState((){
            _active = newValue;
        })
    }
    
    Widget build(BuildContext context){
        return Container(
        	child: ChildWidget(
            	active: _active,
                onChanged: handleTapChanged
            )
        );
    }
}
```

```dart
// 子容器控制：因为要控制状态，所以要申明为StatefulWidget
class ChildWidget extends StatefulWidget{
    ChildWidget({Key?key,this.active:false,required this.onChanged}):super(key:key);
    
    final bool active;
    final ValueChanged<bool> onChanged;
    
    _ChildWidgetState createState() => _ChildWidgetState();
}

class _ChildWidgetState extends State<ChildWidget>{
    bool _hightlight = false;
    void handleTapDown(TapDownDetails details){
        setState((){
            _hightlight = true;
        })
    }
    void handleTapUp(TapDownDetails details){
        setState((){
            _hightlight = false;
        })
    }
    void handleTapCancel(TapDownDetails details){
        setState((){
            _hightlight = false;
        })
    }
    void hangdeTap(){
        widget.onChanged(!widget.active);
    }
    
    Widget build(BuildContext context){
        return GestureDetector(
        	onTapDown: _handleTapDown,
            onTapUp: _handleTapUp,
            onTap:_hangleTap,
            onTapCancel: _hangdleTapCancel,
            ...
        );
    }
}
```

