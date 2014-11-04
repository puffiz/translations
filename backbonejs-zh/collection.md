#Backbone.Collection

集合是模型的有序组合，我们可以在集合上绑定“change”事件，当集合中的模型发生变化时会触发这个事件，集合还可以监听“add”和“remove”事件，以及从服务器更新集合数据的“fetch”事件，同时集合还可以使用Underscore.js的方法。

方便起见，集合中的模型触发的任意事件都可以在集合直接触发。所以我们可以监听集合中任意模型的特定属性变化，例如： documents.on("change:selected", ...)


###extend 

Backbone.Collection.extend(properties, [classProperties]) 

通过扩展 Backbone.Collection 创建一个 Collection 类。实例属性参数 properties 以及 类属性参数 classProperties 会被直接注册到集合的构造函数。

###model

collection.model 

重载这个属性来指定集合的模型类。 如果它已被定义,可以传入原始属性对象（和数组）来 add，create，以及 reset， 传入的属性会被自动转换为适合的模型类型。

```
var Library = Backbone.Collection.extend({
  model: Book
});
```

通过不同的构造函数返回不同的模型的方式来重载属性, 一个集合可以包含多种形态的模型。

```
var Library = Backbone.Collection.extend({

  model: function(attrs, options) {
    if (condition) {
      return new PublicDocument(attrs, options);
    } else {
      return new PrivateDocument(attrs, options);
    }
  }

});
```

###constructor / initialize

new Backbone.Collection([models], [options]) 

当创建集合时，你可以选择传入初始的 模型 数组。 集合的 comparator 函数也可以作为选项传入. 如果定义了 initialize 函数，它会在集合创建时被调用. 有很多提供的选项是直接被注册到集合里的: model 和 comparator。

```
var tabs = new TabSet([tab1, tab2, tab3]);
var spaces = new Backbone.Collection([], {
  model: Space
});
```

###models

collection.models 

用来访问集合中模型的模型数组。 通常我们使用 get，at，或 Underscore methods 访问模型对象， 但偶尔也需要直接访问。

###toJSON

collection.toJSON([options])

返回一个集合中包含的每个模型(使用 toJSON)属性散列的数组。 它可用于集合的序列化和持久化。 本方法名称容易引起混淆，因为它与 JavaScript's JSON API 命名相同。

```
var collection = new Backbone.Collection([
  {name: "Tim", age: 5},
  {name: "Ida", age: 26},
  {name: "Rob", age: 55}
]);

alert(JSON.stringify(collection));
```

###sync

collection.sync(method, collection, [options]) 

使用 Backbone.sync 来持久化一个集合的状态到服务器. 它能够被自定义的行为重载。