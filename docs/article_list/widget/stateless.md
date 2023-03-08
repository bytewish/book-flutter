
# 案例
```dart
class TestPage extends StatelessWidget{
    const TestPage({Key? key}) : super(key: key);

    Widget build(BuildContext context){
        return Container("伪代码：布局")
    }
}
```
对于`StatelessWidget`组件来说是无法保存状态的，所以我们只能在构造参数里传递初始值或者引用`final`的静态值。
```dart
class TestPage extends StatelessWidget {
   final int a;
   const TestPage({Key? key,required this.a}) : super(key: key);
}

```

# 讲解
对于`StatelessWidget`的组件树来说，一旦内部的数据发生变更，整个组件都需要重新创建并且渲染。