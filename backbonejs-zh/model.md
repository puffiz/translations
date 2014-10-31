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
