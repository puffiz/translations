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
-------------------------   

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
----------------------   

**off**  ```object.off([event], [callback], [context])``` 别名：unbind

去除先前绑定在对象的回调函数.如果没有指上下，所有不同上下文的所有不同版本的回调都会删除。如果没有指定回调，那这个事件的所回调都会被删除。如果没有指定事件，那么所事件的回调都会被删除。
```
// Removes just the `onChange` callback.
object.off("change", onChange);

// Removes all "change" callbacks.
object.off("change");

// Removes the `onChange` callback for all events.
object.off(null, onChange);

// Removes all callbacks for `context` for all events.
object.off(null, null, context);

// Removes all callbacks on `object`.
object.off();
```
注意```model.off```这个调用。例如，它将会马上删除这个model上的所有事件，包括Backone内部用来统计的事。

----------------------------- 

**trigger**  ```object.trigger(event, [*args])```
一个给定事件或者用空格隔的一系列事件的Trigger回调。第二个参数会在事件的回调间传递。

-----------------------

**once**   ```object.once(event, callback, [context]) ```
跟```on```类似，但是被绑定的回调在删除前只能被调用一次
Handy for saying "the next time that X happens, do this".

--------------------

**listenTo** ```object.listenTo(other, event, callback) ```
告诉对象去监听另一个对象的一个特的事件。这样做而不是用***other.on(event, callback, object)***的好处是，**listen To**允许对象保持追事件，并且可在之后立被移。他的回调常常对象作上下文调用。
```
view.listenTo(model, 'change', view.render);
```

--------------------

**stopListening**  ```object.stopListening([other], [event], [callback]) ```
告诉对象停止监听事件。或者不带参数调用**stopListening**去移除对象的所有的**已注册**的回调。或者更加严格告诉对象仅仅移除正在监听的事件或某个特定的事或某个特定的回调。

```
view.stopListening();

view.stopListening(model);
```

-----------------

**listenToOnce** ```object.listenToOnce(other, event, callback) ```
类似listen To,但是被绑定的回调在删除前只能被调用一次.

-------------------

**Catalog of Events **

下面完整的Backbone内置带参数的事件列表。你也可以在Model,Collection,View上触发自己的事件，只要你觉得合适。Backbone对象本省也夹杂在事件中，可以被用来发任何全局事件如果需要的话。

- **"add"** (model, collection, options) ———当一个model被添加到一个collection
- **"remove"** (model, collection, options) ————当一个model移除出一个collection
- **"reset"**(collection, options) ———— 当collection的所有内容都被替换
- **"sort"**(collection, options) ————当collection被重新排序
- **"change"**(model, options) ————当一个model的属性改变了
- **"change:[attribute]"** (model, value, options) ————当一个特定的属性更新了
- **"destory"**(model, collection, options) ————当一个model被销毁
- **"request"** (model_or_collection, xhr, options) ————当一个model或collectio向服务器发送了一个请求
- **"sync"** (model_or_collection, resp, options) ————当一个model或collection成功与服务器同步
- **"error"** (model_or_collection, resp, options) ————当model或collectio向服务器请求失败
- **"invalid"** (model, error, options) ————当一个model在客户端验证失败
- **"route:[name]"** (params) ———— 当一个特定的路由匹配到时被这个路由触发
- **route**(route, params) ————当任意路由匹配被这个路由触发
- **"route"** (router, route, params)————当任意路由匹配被历史记录(history)触发
- **"all"** ————这个特殊事件被任意已触发的事件触发，并且传递这个事件名作为第一参数。


通常来说，当调用能发出事件(如model.set, collection.add等)的方法时，如果你愿意防事件被触发，你可传递```{silent: true}```作为一个选项。注意这不常用，也许这不是一个好办法。为监听的事件回调传递一个特定的标记(Passing through a specific flag in the options for your event callback to look at)，然后选择去忽略它，常常会更好。
























。



