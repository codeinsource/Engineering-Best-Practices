## Performance
性能
Writing performant code is absolutely critical. Poorly written JavaScript can significantly slow down and even crash the browser. On mobile devices, it can prematurely drain batteries and contribute to data overages. Performance at the browser level is a major part of user experience which is part of the 10up mission statement.
编写代码是绝对关键的。编写的javascript很糟糕，可以显著降低浏览器的速度甚至崩溃。在移动设备上，它可以过早地耗尽电池，并有助于数据的产生。浏览器级别的性能是用户体验的主要部分，它是在任务语句的一部分。
### Only Load Libraries You Need
你需要加载的库
JavaScript libraries should only be loaded on the page when needed. jquery-1.11.1.min.js is 96 KB. This isn't a huge deal on desktop but can add up quickly on mobile when we start adding a bunch of libraries. Loading a large number of libraries also increases the chance of conflicts.
当needed.为96 kb时，javascript库只应该加载到页面上。这不是一个巨大的交易桌面，但可以在移动时快速增加，当我们开始添加一组库。加载大量的库也会增加冲突的机会。
### Use jQuery Wisely
明智地使用jquery
[jQuery](http://jquery.com/) is a JavaScript framework that allows us to easily accomplish complex tasks such as AJAX and animations. jQuery is great for certain situations but overkill for others. For example, let's say we want to hide an element:
jquery是一个javascript框架，允许我们轻松完成复杂任务，比如ajax和动画。jquery对于某些情况很好，但是对于其他情况来说却是多余的。例如，假设我们想隐藏一个元素：

```javascript
document.getElementById( 'element' ).style.display = 'none';
```

vs.

```javascript
jQuery( '#element' ).hide();
```

The non-jQuery version is [much faster](https://jsperf.com/hide-with-and-without-jquery) and is still only one line of code.
非jquery版本是快得多而且仍然只是一行代码。
### Try to Pass an HTMLElement or HTMLCollection to jQuery Instead of a Selection String
尝试将一个HTMLElement或HTMLCollection传递给jquery而不是选择字符串
When we create a new jQuery object by passing it a selection string, jQuery uses its selection engine to select those element(s) in the DOM:
当我们通过传递一个选择字符串创建一个新的jquery对象时，jquery使用它的选择引擎来选择的中的元素：
```javascript
jQuery( '#menu' );
```

We can pass our own [HTMLCollection](https://developer.mozilla.org/en-US/docs/Web/API/HTMLCollection) or [HTMLElement](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement) to jQuery to create the same object. Since jQuery does a lot of magic behind the scenes on each selection, [this will be faster](https://jsperf.com/wrap-an-element-or-html-collection-in-jquery):
我们可以通过自己的n.HTMLCollection或n.HTMLElement对于jquery创建相同的对象。因为jquery在每个选择中都有很多魔力，这会更快：
```javascript
jQuery( document.getElementById( 'menu' ) );
```

### Cache DOM Selections

It's a common JavaScript mistake to reselect something unnecessarily. For example, every time a menu button is clicked, we do not need to reselect the menu. Rather, we select the menu once and cache its selector. This applies whether you are using jQuery or not. For example:
这是一个常见的javascript错误，以避免不必要的事情。例如，每次单击菜单按钮时，我们都不必重新打开菜单。相反，我们选择了一次菜单并缓存它的选择器。这适用于您是否使用jquery。例如：
non-jQuery Uncached:
非jquery Uncached：
```javascript
var hideButton = document.getElementsByClassName( 'hide-button' )[0];
hideButton.onclick = function() {
    var menu = document.getElementById( 'menu' );
    menu.style.display = 'none';
}
```

non-jQuery Cached:
非jquery缓存：
```javascript
var menu = document.getElementById( 'menu' );
var hideButton = document.getElementsByClassName( 'hide-button' )[0];
hideButton.onclick = function() {
    menu.style.display = 'none';
}
```

jQuery Uncached:

```javascript
var $hideButton = jQuery( '.hide-button' );
$hideButton.on( 'click', function() {
    var $menu = jQuery( '#menu' );
    $menu.hide();
});
```

jQuery Cached:
jquery缓存：
```javascript
var $menu = jQuery( '#menu' );
var $hideButton = jQuery( '.hide-button' );
$hideButton.on( 'click', function() {
	$menu.hide();
});
```
Notice how in cached versions we are pulling the menu selection out of the event handler so it only happens once. Non-jQuery cached is not surprisingly the [fastest way to handle this situation](https://jsperf.com/dom-selection-caching).
注意如何在缓存版本中，我们将菜单选择从事件处理程序中拉出来，这样它就只能发生一次。非jquery缓存并不奇怪最快的方法来处理这种情况。
#### Event Delegation
项目代表
Event delegation is the act of adding one event listener to a parent node to listen for events bubbling up from its children. This is much more performant than adding one event listener for each child element. Here is an example:
事件授权是将一个事件侦听器添加到父节点以侦听来自其子节点冒泡事件的行为。这比为每个子元素添加一个事件侦听器要多得多。以下是一个例子：
Without jQuery:
没有jquery：
```javascript
document.getElementById( 'menu' ).addEventListener( 'click', function( event ) {
    var currentTarget = event.currentTarget;
    var target = event.target;

    if ( currentTarget && target ) {
      if ( 'LI' === target.nodeName ) {
        // Do stuff with target!
      } else {
        while ( currentTarget.contains( target ) ) {
          // Do stuff with a parent.
          target = target.parentNode;
        }
      }
    }
});
```

With jQuery:
使用jquery：
```javascript
jQuery( '#menu' ).on( 'click', 'li', function() {
    // Do stuff!
});
```

The non-jQuery method is as usual [more performant](https://jsperf.com/jquery-vs-non-jquery-event-delegation). You may be wondering why we don't just add one listener to ```<body>``` for all our events. Well, we want the event to *bubble up the DOM as little as possible* for [performance reasons](https://jsperf.com/event-delegation-distance). This would also be pretty messy code to write.
非jquery方法通常是一样的更多的。您可能会想知道为什么我们不只是添加一个侦听器<body>为了我们所有的活动。好吧，我们想要这个活动尽可能少地泡上dom为性能原因。这也将是非常杂乱的代码编写。
<h2 id="design-patterns">Design Patterns {% include Util/top %}</h2>
设计模式
Standardizing the way we structure our JavaScript allows us to collaborate more effectively with one another. Using intelligent design patterns improves maintainability, code readability, and even helps to prevent bugs.
规范我们构建javascript的方式，使我们能够更有效地相互协作。使用智能设计模式提高了维护性、代码可读性，甚至有助于防止bug。
### Don't Pollute the Window Object

Adding methods or properties to the ```window``` object or the global namespace should be done carefully. ```window``` object pollution can result in collisions with other scripts. We should wrap our scripts in closures and expose methods and properties to ```window``` decisively.
将方法或属性添加到window对象或全局命名空间应该小心地完成。window对象污染会导致与其他脚本发生冲突。我们应该将脚本封装在闭包中，并将方法和属性公开到window果断地。
When a script is not wrapped in a closure, the current context or ```this``` is actually ```window```:
当脚本未封装在闭包中时，当前上下文或this实际上window：
```javascript
window.console.log( this === window ); // true
for ( var i = 0; i < 9; i++ ) {
    // Do stuff
}
var result = true;
window.console.log( window.result === result ); // true
window.console.log( window.i === i ); // true
```

When we put our code inside a closure, our variables are private to that closure unless we expose them:
当我们将代码放到闭包中时，我们的变量是私有的，除非我们暴露它们：
```javascript
( function() {
    for ( var i = 0; i < 9; i++ ) {
        // Do stuff
    }

    window.result = true;
})();

window.console.log( typeof window.result !== 'undefined' ); // true
window.console.log( typeof window.i !== 'undefined' ); // false
```

Notice how ```i``` was not exposed to the ```window``` object.
注意如何i未暴露于window对象。
### Use Modern Functions, Methods, and Properties
使用现代功能、方法和属性
It's important we use language features that are intended to be used. This means not using deprecated functions, methods, or properties. Whether we are using plain JavaScript or a library such as jQuery or Underscore, we should not use deprecated features. Using deprecated features can have negative effects on performance, security, maintainability, and compatibility.
我们使用那些想要使用的语言特性是很重要的。这意味着不使用不推荐的函数、方法或属性。无论我们使用了普通javascript还是jquery或下划线库，我们都不应该使用不推荐的功能。使用不推荐的特性会对性能、安全性、可维护性和兼容性产生负面影响。
For example, in jQuery ```jQuery.live()``` is a deprecated method:
例如，在jquery中jQuery.live()是一种不赞成的方法：
```javascript
jQuery( '.menu' ).live( 'click', function() {
    // Clicked!
});
```

We should use ```jQuery.on()``` instead:
我们应该用jQuery.on()相反：
```javascript
jQuery( '.menu' ).on( 'click', function() {
    // Clicked!
});
```

Another example in JavaScript is ```escape()``` and ```unescape()```. These functions were deprecated. Instead we should use ```encodeURI()```, ```encodeURIComponent()```, ```decodeURI()```, and ```decodeURIComponent()```.
javascript中的另一个示例是escape()以及unescape()。这些函数被否决了。相反，我们应该使用encodeURI()，encodeURIComponent()，decodeURI()，和decodeURIComponent()。

<h2 id="code-style">Code Style & Documentation {% include Util/top %}</h2>
代码样式&文档
We conform to the [WordPress JavaScript coding standards](http://make.wordpress.org/core/handbook/coding-standards/javascript/).
我们遵守wordpress javascript编码标准。
We conform to the [WordPress JavaScript Documentation Standards](https://make.wordpress.org/core/handbook/best-practices/inline-documentation-standards/javascript/).
我们遵守wordpress javascript文档标准。
<h2 id="unit-and-integration-testing">Unit and Integration Testing {% include Util/top %}</h2>
  单元与集成测试
At 10up, we generally employ unit and integration tests only when building applications that are meant to be distributed. Writing tests for client themes usually does not offer a huge amount of value (there are of course exceptions to this). When we do write tests, we use [Mocha](https://mochajs.org).
在10up中，我们通常只在构建要分发的应用程序时使用单元和集成测试。对于客户端主题编写测试通常不会提供大量的值(当然，这当然是例外)。当我们编写测试时，我们使用摩卡。
<h2 id="libraries">Libraries {% include Util/top %}</h2>
图书馆
There are many JavaScript libraries available today. Many of them directly compete with each other. We try to stay consistent with what WordPress uses. The following is a list of primary libraries used by 10up.
今天有许多javascript库可用。他们中的许多人直接竞争。我们尽量保持与wordpress使用的一致。下面是的使用的主库列表。
### DOM Manipulation
dom操纵
[jQuery](https://jquery.com/) - Our and WordPress's library of choice for DOM manipulation.
jquery-我们和wordpress的选择库用于Dom操作。
### Utility
效用
[Underscore](http://underscorejs.org) - Provides a number of useful utility functions such as ```clone()```, ```each()```, and ```extend()```. WordPress core uses this library quite a bit.
下划线-提供一些有用的实用工具函数，如clone()，each()，和extend()。wordpress核心使用这个库相当多。
### Frameworks
框架
[Backbone](http://backbonejs.org) - Provides a framework for building complex JavaScript applications. Backbone is based on the usage of models, views, and collections. WordPress core relies heavily on Backbone especially in the media library. Backbone requires Underscore and a DOM manipulation library (jQuery)
骨干-提供构建复杂javascript应用程序的框架。主干是基于模型、视图和集合的使用。wordpress核心依赖于骨干，尤其是媒体库。主干需要下划线和一个Dom操作库(Jquery)
