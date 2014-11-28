# Backbone.sync
**Backbone.sync** 是 Backbone 每次向服务器读取或保存模型时都要调用执行的函数。默认情况下，它使用```jQuery.aja```方法发送RESTful json请求并返回jqXR。如果想采用不同的持久化方案，比如 WebSockets, XML, 或 Local Storage，我们可以重载该函数。

**Backbone.sync** 的语法为 sync(method, model, [options])。

method – CRUD 方法 ("create", "read", "update", 或 "delete")
model – 要被保存的模型（或要被读取的集合）
options – 成功和失败的回调函数，以及所有 jQuery 请求支持的选项

默认情况下，当 **Backbone.sync** 发送请求以保存模型时，其属性会被序列化为JSON，并以 application/json 的内容类型发送。当接收到来自服务器的JSON响应后，对经过服务器改变的模型进行拆解，然后在客户端更新。当 "read" 请求从服务器端响应一个集合（Collection#fetch）时，便拆解模型属性对象的数组。

无论一个模型或者一个集合何时开始**同步** ,一个```"request"```事件被放出。如果请求成功完成，你将得到一个```sync```事件，反之则是```error```事件。 

默认 sync 映射 REST 风格的 CRUD 类似下面这样：

create → POST   /collection
read → GET   /collection[/id]
update → PUT   /collection/id
delete → DELETE   /collection/id

举个例子，一个处理来自```Backbone```的```"update"```请求的Rails程序可能像这样：(在实际编码中，不要盲目地用```update_attributes```,要经常将允许修改的参数列入白名单)
```
def update
  account = Account.find params[:id]
  account.update_attributes params
  render :json => account
end
```
另外一个集成3.1版本前的Rails程序的贴士就是禁用默认的```to_json```命名空间:
```ActiveRecord::Base.include_root_in_json = falsto_json```

## ajax   Backbone.ajax = function(request) { ... };  }
如果你想用自定义的AJAX方法，或者你的目的是不支持Jquery。ajax API,你需要通过这样设置来调整:
```Backbone.ajax```

## emulateHTTP   Backbone.emulateHTTP = true 
老的服务器不支持 Backbone 默认的 REST/HTTP，此时可以开启Backbone.emulateHTTP 。设置该选项将通过 POST 方法伪造 PUT 和 DELETE 请求，此时该请求会向服务器传入名为 _method 的参数。 设置该选项同时也会向服务器发送 X-HTTP-Method-Override 头。

```
Backbone.emulateHTTP = true;

model.save();  // POST 到 "/collection/id", 附带 "_method=PUT" + header.
```

## emulateJSON   Backbone.emulateJSON = true 
同样老的服务器也不支持处理application/json 编码的请求，设置 Backbone.emulateJSON = true; 然后JSON模型会被序列化为model参数，请求会按照 application/x-www-form-urlencoded 的内容类型发送，就像提交表单一样。

