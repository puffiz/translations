# Backbone.history
**History** 作为全局路由服务用于处理```hashchange``` 事件或```pushState```，匹配适合的路由，并触发回调函数。 我们不需要自己去做这些事情 — 如果使用带有键值对的路由，```Backbone.history```会被自动创建。

Backbone会自动判断浏览器对```pushState```的支持，以做内部的选择。不支持```pushState```的浏览器将会继续使用基于猫点的URL片段，如果兼容```pushState```的浏览器访问了某个 URL 猫点，将会被透明的转换为真实的URL。注意使用真实的URLs需要web服务器支持直接渲染那些页面，因此后端程序也需要做修改。例如，如果有这样一个路由```/document/100```，如果浏览器直接访问它， web 服务器必须能够处理该页面。趋于对搜索引擎爬虫的兼容，让服务器完全为该页面生成静态HTML是非常好的做法 ... 但是如果要做的是一个web应用，只需要利用Javascript和 Backbone视图将服务器返回的REST数据渲染就很好了。

## start   Backbone.history.start([options]) 
当所有的路由创建并设置完毕，调用```Backbone.history.start()``` 开始监控```hashchange```事件并分配路由。
之后再调用```Backbone.history.start()```将会报错，```Backbone.History.started```是个布尔型表明其是否已经被调用。

需要指出的是，如果想在应用中使用HTML5支持的```pushState```，只需要这样做：```Backbone.history.start({pushState : true})``` 。如果你想用```pushState```,但浏览器并不原生支持，那就用整页刷新代替吧，你可以加参数：```{hashChange: false}```

如果应用不是基于域名的根路径 /，需要告诉 History 基于什么路径： Backbone.history.start({pushState: true, root: "/public/search/"})

当执行后，如果某个路由成功匹配当前URL，Backbone.history.start() 返回true。如果没有定义的路由匹配当前URL，返回false。

如果服务器已经渲染了整个页面，但又不希望开始History时触发初始路由，传入```silent:true```即可。

因为基于散列表的历史在IE中依赖```<iframe>```，确保只在DOM加载完毕后调用```start()```
```
$(function(){
    new WorkspaceRouter();
    new HelpPaneRouter();
    Backbone.history.start({pushState: true});
});
```
