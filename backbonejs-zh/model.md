MODEL
=====================

#Backbone.Model

Model一般都是JavaScript应用的核心，包含了应用与后端的数据交换，以及数据转换，数据验证，计算属性，访问权限等很多的逻辑。你可以拓展Model，Model也提供一些基础的方法来管理变化。

下面是一个粗糙的例子，不过它完整的演示了Model如何自定义方法，设置属性，绑定事件。一旦运行代码，sidebar就可以再你的浏览器后台执行，你就可以玩起来了！

```
var Sidebar = Backbone.Model.extend({
  promptColor: function() {
    var cssColor = prompt("Please enter a CSS color:");
    this.set({color: cssColor});
  }
});

window.sidebar = new Sidebar;

sidebar.on('change:color', function(model, color) {
  $('#sidebar').css({background: color});
});

sidebar.set({color: 'white'});

sidebar.promptColor();
```

##extend 

** Backbone.Model.extend(properties, [classProperties]) **


创建你自己的类，你需要扩展Backbone.Model，并且提供类属性。还可以提供一个可选属性，这个属性会被类的构造函数直接使用。

如果你处理好了扩展关系，子类就可以做很多你像要的事情。

```
var Note = Backbone.Model.extend({

  initialize: function() { ... },

  author: function() { ... },

  coordinates: function() { ... },

  allowedToEdit: function(account) {
    return true;
  }

});

var PrivateNote = Note.extend({

  allowedToEdit: function(account) {
    return account.owns(this);
  }

});
```

父类: 如果你重写了核心方法，如set，save等方法，但是又要调用它的实现，你就需要像下面一样，显示的调用这些方法。

```
var Note = Backbone.Model.extend({
  set: function(attributes, options) {
    Backbone.Model.prototype.set.apply(this, arguments);
    ...
  }
});
```

##constructor / initialize
***new Model([attributes], [options]) ***

当我们开始创建model的时候，我们可以通过传递变量的初始化值来设置model。如果你定义了initalize的方法，就可以在model创建后被调用。

```
new Book({
  title: "One Thousand and One Nights",
  author: "Scheherazade"
});
```

在特殊的情况下，你也可以重写model的构造函数来达到特别的效果。

```
var Library = Backbone.Model.extend({
  constructor: function() {
    this.books = new Books();
    Backbone.Model.apply(this, arguments);
  },
  parse: function(data, options) {
    this.books.reset(data.books);
    return data.library;
  }
});
```

如果你传入了一个{collection:...}的参数，model就会获得一个collection的变量，表明了该model属于哪个collection，提供了计算model的url。model的collection属性会在你第一次传递collection的时候自动生成。如果你把它传递给构造函数，是不会自动初始化的。这个特性有时候很有用。

如果你传入一个{parse:true}的参数，model的变量会在设置之前先parse一次。












