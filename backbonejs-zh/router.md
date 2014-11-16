# Backbone.Router 
 
Web应用常常为重要的位置提供可链接的，可存为书签的，可分享的链接。直到最近，哈希片段(hash fragments)(如#page)被用来提供这些固定链接,但是随着History API的到来，用标准URLS(如/page) 成为可能。Backbone.Router为客户端路由(routing client-side pages)提供了方法,并且把它们与动作和事件连接。为还不支持History API的浏览器，Router有着优雅的备选方案以及向片段式的URL的透明转换。
在页面加载期间，在你的应用全部创建完所有的路由后，一定要调用```Backbone.history.start()```,或者```Backbone.history.start({pushState::true})```来确保驱动初始化URL的路由。

------------
## extend ```Backbone.Router.extend(properties, [classProperties]) ```
创建一个自定义的路由类。可以通过routes定义路由动作键值对，当匹配URL片段便执行定义的动作。注意你将想在路由定义中避免使用反斜杠(/):
```
var Workspace = Backbone.Router.extend({

routes: {
"help":                 "help",    // #help
"search/:query":        "search",  // #search/kiwis
"search/:query/p:page": "search"   // #search/kiwis/p7
},

help: function() {
...
},

search: function(query, page) {
...
}

});
```

## routes ```router.routes```
routes将带参数的URLS映射到路由到路由实例上（或者只是直接的函数定义，如果你喜欢的话），这与视图的事件键值对非常类似。路由可以包含参数，```:param```，它在斜线之间匹配URL组件。路由也支持通配符```*splat```,可以匹配多个URL组件。

举个例子，路由```"search/:query/p:page"```能匹配```#search:/obame/p2```并传入```"obame"```和```"2"```到对应的动作中去。   
```"file/*path"```能匹配```#file/nested/folder/file.txt```并传入```"nested/folder/file.txt"```到对应的动作中去。   
```"docs/:section(/:subsection)"```能匹配```#docs/faq ```和```#docs/faq/installing```,并且前者传入```"faq"```,后者传入```"faq"```和```"installing"```。
反斜杠结尾被视为URL的一部分，并且当被访问时应被当做独特的路由.```docs``` 和```docs/```将触发不同的反馈。如果你不能避免生成不同类型的URLS，你可以定义一个类似```"docs(/)"```的路由来区分不同的情况.

当访问者点击浏览器后退按钮，或者输入URL，如果某个路由已匹配，将会触发一个基于动作名称的事件，其他对象可以监听这个路由并接受到通知。在下例中，访问```#help/uploading```将触发```route:help```事件 。
```
routes: {
    "help/:page":         "help",
    "download/*path":     "download",
    "folder/:name":       "openFolder",
    "folder/:name-:mode": "openFolder"
}
router.on("route:help", function(page) {
  ...
});
````

## constructor / initialize   ```new Router([options])``` 
实例化一个路由实例,你可以传入```routes```键值对象作为参数，如果定义该参数，它们将被传入```initialize```构造函数中初始化。

## route  ```router.route(route, name, callback)```
为路由对象手动创建路由。```route```参数可以时路由字符串或正则表达式。每个被路由或正则表达式匹配的都将作为参数传入回调函数。 一旦路由匹配，```name```参数将被作为```"route::name"```事件触发。如果```callback```参数被省略，将用```router::name```替代。之后添加的路由将覆盖之前声明的路由。
```
initialize: function(options) {

    // Matches #page/10, passing "10"
    this.route("page/:number", "page", function(number){ ... });

    this.route(/^(.*?)\/open$/, "open");
    },
    
    open: function(id) { ... } })}
```

## navigatei   ```router.navigate(fragment, [options]) ```

手动到达应用程序的某个位置。如果你希望同时调用路由函数，设置```trigger```参数为```true```。为了更新路由但不在浏览器历史记录中创建一条记录,设置```replace```参数为```true```   

```
openPage: function(pageNumber) {
  this.document.pages.at(pageNumber).open();
this.navigate("page/" + pageNumber);
}

# Or ...

app.navigate("help/troubleshooting", {trigger: true});

# Or ...

app.navigate("help/troubleshooting", {trigger: true, replace: true});

```

## execute   ```router.execute(callback, args)```
只要路由匹配，这个方法都会在路由内部被调用，并且它相应的回调将会执行。通过复写它来自定义路由解析(Override it to perform custom parsing or wrapping of your routes).例如，为了在传递查询字符串到回调之前解析它们，可以这样做：
```
var Router = Backbone.Router.extend({
    execute: function(callback, args) {
        args.push(parseQueryString(args.pop()));
        if (callback) callback.apply(this, args);
    }
});
```

















