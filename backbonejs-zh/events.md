# Events
Events是一能和任对象混合的模型，给对象提供绑定和触发各种事件的能力。Event不需要在绑定之前不需要声明，还可以传参数。例如：
```
var object = {};

_.extend(object, Backbone.Events);

object.on("alert", function(msg) {
  alert("Triggered " + msg);
});

object.trigger("alert", "an event");
```
例如，创一个便利的事件分配器，它能在你的应的不同地方调度事件：  
```
var dispatcher = _.clone(Backbone.Events)
```

**On** ```object.on(event, callback, [context])``` 别名：bind

为对象绑定一个回调方法。只要事件触发，这个回调就会执行。如果在一个页面上有大量不同的事件，约定用冒号(:)来命名他们：```"poll:start"```或者```"change:selection"``` .不能事件之间用空格隔开：   

```book.on("change:title change:author", ...);```

为了提供上下文this,可以当回调方法被调用时，传递第三个可选的参数：```model.on('change', this.render, this)```

绑定成all回调方法将被任何事件触发，并且会事件名作为第一个参数传递。例如，为了从一个对象向其他对象代理所有事件（ to proxy all events from one object to another）：
```
proxy.on("all", function(eventName) {
  object.trigger(eventName);
})
```

所有的Backbone事件方法都还支持一个事件映语法，作一个可选的位置参数：
```
book.on({
  "change:title": titleView.update,
  "change:author": authorPane.update,
  "destroy": bookView.remove
});
```
