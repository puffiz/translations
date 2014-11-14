# Backbone.Router 
 
Web应用常常为重要的位置提供可链接的，可存为书签的，可分享的链接。直到最近，哈希碎片(hash fragments)(如#page)被用来提供这些固定链接,但是随着History API的到来，用标准URLS(如/page) 成为可能。Backbone.Router为路由客户端页面(routing client-side pages)提供了方法,并且把它们与动作和事件连接。为还不支持History API的浏览器，Router有着优雅的备选方案以及向碎片式的URL的透明转换。
在页面加载期间，在你的应用全部创建完所有的路由后，一定要调用```Backbone.history.start()```,或者```Backbone.history.start({pushState::true})```来路由初始的URL。

------------
## extend ```Backbone.Router.extend(properties, [classProperties]) ```
以创建一个自己的路由类开始。定义当特定的URL碎片匹配时被触发的动作，并且提供一个路由hash来匹配路由和动作。注意你将想在路由定义中避免使用反斜杠(/):
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

## routes ```router.toutes```




