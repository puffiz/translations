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
###Underscore 方法 (28) 
Backbone.Collection 有28个 Underscore.js 的迭代方法。这边没有文档，不过你可以参考Underscore.js的文档获取更多细节。

forEach (each)
```
books.each(function(book) {
  book.publish();
});
```
map (collect)
```
var titles = books.map(function(book) {
  return book.get("title");
});
```
reduce (foldl, inject)
reduceRight (foldr)
find (detect)
filter (select)
```
var publishedBooks = books.filter(function(book) {
  return book.get("published") === true;
});
```
reject
every (all)
some (any)
contains (include)
invoke
max
min
sortBy
```
var alphabetical = books.sortBy(function(book) {
  return book.author.get("name").toLowerCase();
});
```
groupBy
shuffle
toArray
size
first (head, take)
initial
rest (tail)
last
without
indexOf
lastIndexOf
isEmpty
chain







###add
```
collection.add(models, [options]) 
```

添加一个或者多个model中的数组到collection，并触发一个“add”事件。

如果一个模型属性被定义了,你也可以传递一个新的属性对象,并让他们作为模型实例存在。返回值为新添加的(或者复制的,以前存在的)模型。

传递{at: index}可以指定model在collection中的位置。

如果添加的model在collection中已经存在，Backbone会无视这个操作，除非你有传递 {merge: true}，这样就会将已经存在的model和被添加的model 的属性合并起来，同时还会触发一个“change”事件。

```
var ships = new Backbone.Collection;

ships.on("add", function(ship) {
  alert("Ahoy " + ship.get("name") + "!");
});

ships.add([
  {name: "Flying Dutchman"},
  {name: "Black Pearl"}
]);
```

*多次添加相同的model（model的id相同）到collection是没有意义的*


###remove
```
collection.remove(models, [options]) 
```
从collection中移除一个或多个model，并将他们当作返回值。触发“remove”事件，可以使用“slient”禁止触发事件。

###reset
```
collection.reset([models], [options]) 
```
同时添加或者移除collection中的多个model是很好，但是有时候你只是单纯的想更新collection中的model。这时就可以使用reset方法直接替换collection的models。reset方法会触发“reset”事件，并返回新的models。

下面是一个Rails应用的例子，在初始化页面的时候reset一个collection：

```
<script>
  var accounts = new Backbone.Collection;
  accounts.reset(<%= @accounts.to_json %>);
</script>
```
调用 collection.reset() 不带任何models作为参数的话，会清空collection。

###set
```
collection.set(models, [options]) 
```
The set method performs a "smart" update of the collection with the passed list of models. If a model in the list isn't yet in the collection it will be added; if the model is already in the collection its attributes will be merged; and if the collection contains any models that aren't present in the list, they'll be removed. All of the appropriate "add", "remove", and "change" events are fired as this happens. Returns the touched models in the collection. If you'd like to customize the behavior, you can disable it with options: {add: false}, {remove: false}, or {merge: false}.

var vanHalen = new Backbone.Collection([eddie, alex, stone, roth]);

vanHalen.set([eddie, alex, stone, hagar]);

// Fires a "remove" event for roth, and an "add" event for "hagar".
// Updates any of stone, alex, and eddie's attributes that may have
// changed over the years.
getcollection.get(id) 
Get a model from a collection, specified by an id, a cid, or by passing in a model.

var book = library.get(110);
atcollection.at(index) 
Get a model from a collection, specified by index. Useful if your collection is sorted, and if your collection isn't sorted, at will still retrieve models in insertion order.

pushcollection.push(model, [options]) 
Add a model at the end of a collection. Takes the same options as add.

popcollection.pop([options]) 
Remove and return the last model from a collection. Takes the same options as remove.

unshiftcollection.unshift(model, [options]) 
Add a model at the beginning of a collection. Takes the same options as add.

shiftcollection.shift([options]) 
Remove and return the first model from a collection. Takes the same options as remove.

slicecollection.slice(begin, end) 
Return a shallow copy of this collection's models, using the same options as native Array#slice.

lengthcollection.length 
Like an array, a Collection maintains a length property, counting the number of models it contains.

comparatorcollection.comparator 
By default there is no comparator for a collection. If you define a comparator, it will be used to maintain the collection in sorted order. This means that as models are added, they are inserted at the correct index in collection.models. A comparator can be defined as a sortBy (pass a function that takes a single argument), as a sort (pass a comparator function that expects two arguments), or as a string indicating the attribute to sort by.

"sortBy" comparator functions take a model and return a numeric or string value by which the model should be ordered relative to others. "sort" comparator functions take two models, and return -1 if the first model should come before the second, 0 if they are of the same rank and 1 if the first model should come after. Note that Backbone depends on the arity of your comparator function to determine between the two styles, so be careful if your comparator function is bound.

Note how even though all of the chapters in this example are added backwards, they come out in the proper order:

var Chapter  = Backbone.Model;
var chapters = new Backbone.Collection;

chapters.comparator = 'page';

chapters.add(new Chapter({page: 9, title: "The End"}));
chapters.add(new Chapter({page: 5, title: "The Middle"}));
chapters.add(new Chapter({page: 1, title: "The Beginning"}));

alert(chapters.pluck('title'));
Collections with a comparator will not automatically re-sort if you later change model attributes, so you may wish to call sort after changing model attributes that would affect the order.

sortcollection.sort([options]) 
Force a collection to re-sort itself. You don't need to call this under normal circumstances, as a collection with a comparator will sort itself whenever a model is added. To disable sorting when adding a model, pass {sort: false} to add. Calling sort triggers a "sort" event on the collection.

pluckcollection.pluck(attribute) 
Pluck an attribute from each model in the collection. Equivalent to calling map and returning a single attribute from the iterator.

var stooges = new Backbone.Collection([
  {name: "Curly"},
  {name: "Larry"},
  {name: "Moe"}
]);

var names = stooges.pluck("name");

alert(JSON.stringify(names));
wherecollection.where(attributes) 
Return an array of all the models in a collection that match the passed attributes. Useful for simple cases of filter.

var friends = new Backbone.Collection([
  {name: "Athos",      job: "Musketeer"},
  {name: "Porthos",    job: "Musketeer"},
  {name: "Aramis",     job: "Musketeer"},
  {name: "d'Artagnan", job: "Guard"},
]);

var musketeers = friends.where({job: "Musketeer"});

alert(musketeers.length);
findWherecollection.findWhere(attributes) 
Just like where, but directly returns only the first model in the collection that matches the passed attributes.

urlcollection.url or collection.url() 
Set the url property (or function) on a collection to reference its location on the server. Models within the collection will use url to construct URLs of their own.

var Notes = Backbone.Collection.extend({
  url: '/notes'
});

// Or, something more sophisticated:

var Notes = Backbone.Collection.extend({
  url: function() {
    return this.document.url() + '/notes';
  }
});
parsecollection.parse(response, options) 
parse is called by Backbone whenever a collection's models are returned by the server, in fetch. The function is passed the raw response object, and should return the array of model attributes to be added to the collection. The default implementation is a no-op, simply passing through the JSON response. Override this if you need to work with a preexisting API, or better namespace your responses.

var Tweets = Backbone.Collection.extend({
  // The Twitter Search API returns tweets under "results".
  parse: function(response) {
    return response.results;
  }
});
clonecollection.clone() 
Returns a new instance of the collection with an identical list of models.

fetchcollection.fetch([options]) 
Fetch the default set of models for this collection from the server, setting them on the collection when they arrive. The options hash takes success and error callbacks which will both be passed (collection, response, options) as arguments. When the model data returns from the server, it uses set to (intelligently) merge the fetched models, unless you pass {reset: true}, in which case the collection will be (efficiently) reset. Delegates to Backbone.sync under the covers for custom persistence strategies and returns a jqXHR. The server handler for fetch requests should return a JSON array of models.

Backbone.sync = function(method, model) {
  alert(method + ": " + model.url);
};

var accounts = new Backbone.Collection;
accounts.url = '/accounts';

accounts.fetch();
The behavior of fetch can be customized by using the available set options. For example, to fetch a collection, getting an "add" event for every new model, and a "change" event for every changed existing model, without removing anything: collection.fetch({remove: false})

jQuery.ajax options can also be passed directly as fetch options, so to fetch a specific page of a paginated collection: Documents.fetch({data: {page: 3}})

Note that fetch should not be used to populate collections on page load — all models needed at load time should already be bootstrapped in to place. fetch is intended for lazily-loading models for interfaces that are not needed immediately: for example, documents with collections of notes that may be toggled open and closed.

createcollection.create(attributes, [options]) 
Convenience to create a new instance of a model within a collection. Equivalent to instantiating a model with a hash of attributes, saving the model to the server, and adding the model to the set after being successfully created. Returns the new model. If client-side validation failed, the model will be unsaved, with validation errors. In order for this to work, you should set the model property of the collection. The create method can accept either an attributes hash or an existing, unsaved model object.

Creating a model will cause an immediate "add" event to be triggered on the collection, a "request" event as the new model is sent to the server, as well as a "sync" event, once the server has responded with the successful creation of the model. Pass {wait: true} if you'd like to wait for the server before adding the new model to the collection.

var Library = Backbone.Collection.extend({
  model: Book
});

var nypl = new Library;

var othello = nypl.create({
  title: "Othello",
  author: "William Shakespeare"
});

