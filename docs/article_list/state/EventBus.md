# 简介

在移动端开发的时候，我们经常会用到`EventBus`做事件管理，也就是说使用观察者模式，通过订阅、监听的方式来通知更新，以下模仿`EventBus`核心逻辑来实现一个简单的事件总线，真实开源库要比这个复杂，它们会优化事件调度、性能。以下实现仅仅只是主流程的简单实现。

# 案例

```dart
// 申明回调函数，接收一个动态结果值
typedef EventCallback = void Function(dynamic args);
// 初始化值
var eventBus = EventBus();

class EventBus{
	/***申明单例模式***/
    
    // 定义构造函数
    EventBus._internal(){
        // 这段代码可以简写成EventBus._internal()
    }
    static final EventBus _singleton = EventBus._internal();
    factory EventBus() => _singleton;

    /***事件***/
    // 创建一个Map对象，用于保存所有的订阅者
    final _eventMap = Map<Object,List<EventCallback>?>();
    
    // 添加订阅者
  	void add(eventName,EventCallback f){
        if(eventName == null) return;
    	_eventMap[eventName]??= <EventCallback>[];
    	_eventMap[eventName]!.add(f);
  	}
    
    // 移除订阅者
    void remove(eventName,[EventCallback? f]){
    	var list = _eventMap[eventName];
    	if(list == null) return ;
    	if(f==null){
      		_eventMap[eventName] = null;
    	}else{
      		list.remove(f);
    	}
  	}
    
    // 发布通知
  	void emit(eventName,[arg]){
    	var list = _eventMap[eventName];
    	if(list==null) return;
    	int len = list.length -1;
    	for(var i=len;i>-1;--i){
      		list[i](arg);
    	}
  	}
}
```

## 使用

```dart
// 在某个页面中开启监听
bus.on("login", (arg) {
  // do something
});

// 在其他页面发布通知
bus.emit("login", userInfo);
```

