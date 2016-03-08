---
title: '初探React，将我们的View标签化'
layout: post
tags:
 - Web
 - F2e
 - Js
 - React
---

前言

我之前喜欢玩一款游戏：全民飞机大战，而且有点痴迷其中，如果你想站在游戏的第一阶梯，便需要不断的练技术练装备，但是腾讯的游戏一般而言是有点恶心的，他会不断的出新飞机、新装备、新宠物，所以，很多时候你一个飞机以及装备还没满级，新的装备就又出来了，并且一定是更强！
<!--more-->

于是很多人便直接抛弃当前的飞机与装备，追求更好的，这个时候如果是人民币玩家或者骨灰级大神玩家的话，基本可以很快站在世界的顶端，一者是装备好，一者是技术好，但是我不愿意投入太多钱，也不愿意投入过多精力，于是在一套极品装备满级后会积累资源，因为一代之间变化不会太大，到第二代甚至第三代才开始换飞机换装备，也基本处于了第一阶梯，一直到一次游戏大更新，直接导致我当前的飞机与装备完全二逼了，我当时一时脑热投入了所有资源去刷新的极品装备，最后闹的血本无归，于是便删除了该游戏，一年时间付诸东流！！！

再回过头来看最近两年前端的变化，单是一个前端工程化工具就变了几次，而且新出来的总是叫嚷着要替换之前的，grunt->gulp->webpack->es6

再看前端框架的一些产量：backbone、angularJS、react、canJS、vueJS......

真有点乱花渐欲迷人眼的意思，似乎前端技术也开始想要坑前端玩家，因为人家会了新技能，你就落后了，于是很多技术沉淀已经足够的大神便直接在团队使用某一技术，带领团队组员深入了解了该技术的好，并大势宣传新技术。

很多人在这种情况下就中招了！他们可能会抛弃现有技术栈，直接跟风新的技术，在现有装备都没满级的情况下又去刷新装备，如果哪天一次游戏玩法大更新，大神依旧一套极品装备在那风骚，而炮灰仓库中积累着一箩筐低等级的极品装备，却没有可用的，不可谓不悲哀！

一门技术从入门到精通，是需要时间的，在有限的时间中要学习那么多的新技术，还得落地到实际工作中，而每一次新技术的落地都是对曾经架构的否定与推翻，这个成本不可谓不高，对一些创业团队甚至是致命的。工作中也没那么多时间让你折腾新东西，所以一定是了解清楚了一门再去学习其它的，不要半途而废也不要盲目选型。

我最近回顾了这几年所学，可以说技术栈没有怎么更新，但是我对我所习的每一个技术基本上进入了深入的了解：

1. 在MVVM还不太火的时候使用了MVC框架一直到最近，对为什么要使用这种模式，这种模式的好处有了比较深入的了解，并且已经碰到了更复杂的业务逻辑

2. 当一个页面过于复杂时（比如1000多行代码的订单填写页），我能通过几年沉淀，将之拆分为多个业务组件模块，保持主控制器的业务清晰，代码量维护在500行之内，并且各子模块业务也清晰，根据model进行通信

3. 使用Grunt完成前端工程化，从构建项目，到打包压缩项目，到优化项目，总的来说无往不利

4. ......

就编程方法，思维习惯，解决问题的方法来说，与两年前有了很大的变化，而且感觉很难提高了。于是我认识到，就现有的装备下，可能已经玩到极限了，可能到了跟风的时候了，而时下热门的ReactJS似乎是一个很好的切入点，React一端代码多端运行的噱头也足够。

## 初识ReactJS

我最初接触ReactJS的时候，最火的好像是angular，React Native也没有出现，看了他的demo，对其局部刷新的实现很感兴趣。结果，翻看源码一看洋洋洒洒一万多行代码，于是马上便退却了。却不想现在火成了这般模样，身边学习的人多，用于生产的少，我想幕后必然有黑手在推动！也可以预测的是，1,2年后会有更好的框架会取代他，可能是原团队的自我推翻，也有可能是Google公司又新出了什么框架，毕竟前端最近几年才开始真正富客户端，还有很长的路要走。当然，这不是我们关心的重点，我们这里的重点是Hello world。

ReactJS的Hello World是这样写的：

{% highlight html%}
<!DOCTYPE html>
 <html>
 <head>
     <script src="build/react.js" type="text/javascript"></script>
     <script src="build/JSXTransformer.js" type="text/javascript"></script>
 </head>
 <body>
     <div id="example">
     </div>
     <script type="text/jsx">
       React.render(
         <h1>Hello, world!</h1>,
         document.getElementById('example')
       );
     </script>
 </body>
 </html>
{% endhighlight%}

{% highlight html%}
<div id="example"><h1 data-reactid=".0">Hello, world!</h1></div>
{% endhighlight %}

React一来就搞了一个标新立异的地方：jsx（js扩展），说实话，这种做法真的有点大手笔，最初的这种声明式标签写法，在我脑中基本可以追溯到5年前的.net控件了，比如gridview与datalist组件。

在text/jsx中的代码最初不会被浏览器理会，他会被react的JSXTransformer编译为常规的JS，然后浏览器才能解析。这里与html模板会转换为js函数是一个道理，我们有一种优化方案是模板预编译，即：

在打包时候便将模板转换为js函数，免去在线解析的过程，react当然也可以这样做，这里如果要解析的话，会是这个样子：

{% highlight js%}
 React.render(
   React.createElement("h1", null, "Hello, world!"),
   document.getElementById('example')
 );
{% endhighlight %}

因为render中的代码可以很复杂，render中的代码写法就是一种语法糖，帮助我们更好的写表现层代码：render方法中可以写html与js混杂的代码：

{% highlight js%}
 var data = [1,2,3];
 React.render(
     <h1>Hello, {data.toString(',')}!</h1>,
     document.getElementById('example')
 );
{% endhighlight %}

{% highlight js %}
 var data = [1,2,3];
 React.render(
     <h1>{
         data.map(function(v, i) {
             return <div>{i}-{v}</div>
         })
     }</h1>,
     document.getElementById('example')
 );
{% endhighlight %}

所以，react提供了很多类JS的语法，JSXTransformer相当于一个语言解释器，而解析逻辑长达10000多行代码，这个可不是一般屌丝可以碰的，react从这里便走出了不平常的路，而他这样做的意义是什么，我们还不知道。

## 标签化View

react提供了一个方法，将代码组装成一个组件，然后像HTML标签一样插入网页：

{% highlight js %}
 var Pili = React.createClass({
     render: function() {
         return <h1>Hello World!</h1>;
     }
 });
 
 React.render(
     <Pili />,
     document.getElementById('example')
 );

{% endhighlight %}

所谓，声明试编程，便是将需要的属性写到标签上，以一个文本框为例：

{% highlight html %}
<input type="text" data-type="num" data-max="100" data-min="0" data-remove=true />
{% endhighlight %}

我们想要输入的是数字，有数字限制，而且在移动端输入的时候，右边会有一个X按钮清除文本，这个便是我们期望的声明式标签。

react中，标签需要和原始的类发生通信，比如属性的读取是这样的：

{% highlight js %}
 var Pili = React.createClass({
     render: function() {
         return <h1>Hello {this.props.name}!</h1>;
     }
 });
 
 React.render(
     <Pili name='霹雳布袋戏'/>,
     document.getElementById('example')
 );
 
 //Hello 霹雳布袋戏!
{% endhighlight %}

上文中Pili便是一个组件，标签使用法便是一个实例，声明式写法最终也会被编译成一般的js方法，这个不是我们现在关注的重点。

由于class与for为关键字，需要使用className与htmlFor替换

通过this.props对象可以获取组件的属性，其中一个例外为children，他表示组件的所有节点：

{% highlight js %}
 var Pili = React.createClass({
     render: function() {
         return (
             <div>
                 {
                     this.props.children.map(function (child) {
                       return <div>{child}</div>
                     })
                 }
             </div>
         );
     }
 });
 
 React.render(
     <Pili name='霹雳布袋戏'>
         <span>素还真</span>
         <span>叶小钗</span>
     </Pili>
     ,
     document.getElementById('example')
 );
{% endhighlight %}

{% highlight html %}
 <div id="Div1">
     <div data-reactid=".0">
         <div data-reactid=".0.0">
             <span data-reactid=".0.0.0">素还真</span></div>
         <div data-reactid=".0.1">
             <span data-reactid=".0.1.0">叶小钗</span></div>
     </div>
 </div>

{% endhighlight %}

PS：return的语法与js语法不太一样，不能随便加分号

如果想限制某一个属性必须是某一类型的话，便需要设置PropTypes属性：

{% highlight js %}
 var Pili = React.createClass({
     propType: {
         //name必须有，并且必须是字符串
         name:  React.PropTypes.string.isRequired
     },
     render: function() {
         return <h1>Hello {this.props.name}!</h1>;
     }
 });
{% endhighlight %}

如果想设置属性的默认值，则需要:

{% highlight js%}
 var Pili = React.createClass({
     propType: {
         //name必须有，并且必须是字符串
         name:  React.PropTypes.string.isRequired
     },
     getDefaultProps : function () {
         return {
             title : '布袋戏'
         };
     },
     render: function() {
         return <h1>Hello {this.props.name}!</h1>;
     }
 });

{% endhighlight %}

我们仍然需要dom交互，我们有时也需要获取真实的dom节点，这个时候需要这么做：

{% highlight js %}
 var MyComponent = React.createClass({
   handleClick: function() {
     React.findDOMNode(this.refs.myTextInput).focus();
   },
   render: function() {
     return (
       <div>
         <input type="text" ref="myTextInput" />
         <input type="button" value="Focus the text input" onClick={this.handleClick} />
       </div>
     );
   }
 });
{% endhighlight %}

事件触发的时候通过ref属性获取当前dom元素，然后可进行操作，我们这里看看返回的dom是什么：

{% highlight html %}
<input type="text" data-reactid=".0.0">
{% endhighlight %}

看来是真实的dom结构被返回了，另外一个比较关键的事情，便是这里的dom事件支持，需要细读文档：http://facebook.github.io/react/docs/events.html#supported-events

表单元素，属于用户与组件的交互，内容不能由props获取，这个时候一般有状态机获取，所谓状态机，便是会经常变化的属性。

{% highlight js%}
 var Input = React.createClass({
   getInitialState: function() {
     return {value: 'Hello!'};
   },
   handleChange: function(event) {
     this.setState({value: event.target.value});
   },
   render: function () {
     var value = this.state.value;
     return (
       <div>
         <input type="text" value={value} onChange={this.handleChange} />
         <p>{value}</p>
       </div>
     );
   }
 });
 
 React.render(<Input/>, document.body);
{% endhighlight %}

组件有其生命周期，每个阶段会触发相关事件可被用户捕捉使用：

- Mounting：已插入真实 DOM
- Updating：正在被重新渲染
- Unmounting：已移出真实 DOM

一般来说，我们会为一个状态发生前后绑定事件，react也是如此：
{% highlight js %}
componentWillMount()
componentDidMount()
componentWillUpdate(object nextProps, object nextState)
componentDidUpdate(object prevProps, object prevState)
componentWillUnmount()
{% endhighlight %}

此外，React 还提供两种特殊状态的处理函数。

- componentWillReceiveProps(object nextProps)：已加载组件收到新的参数时调用
- shouldComponentUpdate(object nextProps, object nextState)：组件判断是否重新渲染时调用


根据之前的经验，会监控组件的生命周期的操作的时候，往往都是比较高阶的应用了，我们这里暂时不予关注。

好了，之前的例子多半来源于阮一峰老师的教程，我们这里来一个简单的验收，便实现上述只能输入数字的文本框：

{% highlight js%}
 var NumText = React.createClass({
     getInitialState: function() {
         return {value: 50};
     },
     propTypes: {
         value: React.PropTypes.number
     },
     handleChange: function (e) {
         var v = parseInt(e.target.value);
         if(v > this.props.max || v < this.props.min  ) return;
         if(isNaN(v)) v = '';
         this.setState({value: v});
     },
     render: function () {
         return (
             <input type="text" value={this.state.value} onChange={this.handleChange} />
         );
     }
 });
 
 React.render(
   <NumText min="0" max="100" />,
   document.body
 );
{% endhighlight %}

通过以上学习，我们对React有了一个初步认识，现在我们进入其todolist，看看其是如何实现的

此段参考：阮一峰老师的入门教程，[http://www.ruanyifeng.com/blog/2015/03/react.html](http://www.ruanyifeng.com/blog/2015/03/react.html)

## TodoMVC

入口文件

TodoMVC为MVC框架经典的demo，难度适中，而又可以展示MVC的思想，我们来看看React此处的入口代码：

{% highlight html%}
 <!doctype html>
 <html lang="en" data-framework="react">
 <head>
     <meta charset="utf-8">
     <title>React • TodoMVC</title>
     <link rel="stylesheet" href="node_modules/todomvc-common/base.css">
     <link rel="stylesheet" href="node_modules/todomvc-app-css/index.css">
 </head>
 <body>
     <section class="todoapp">
     </section>
     <script src="node_modules/react/dist/react-with-addons.js"></script>
     <script src="node_modules/react/dist/JSXTransformer.js"></script>
     <script src="node_modules/director/build/director.js"></script>
     <script src="js/utils.js"></script>
     <script src="js/todoModel.js"></script>
 
     <script type="text/jsx" src="js/todoItem.jsx"></script>
     <script type="text/jsx" src="js/footer.jsx"></script>
     <script type="text/jsx" src="js/app.jsx"></script>
 </body>
 </html>

{% endhighlight %}

页面很干净，除了react基本js与其模板解析文件外，还多了一个director.js，因为react本身不提供路由功能，所以路由的工作便需要插件，director便是路由插件，这个不是我们今天学习的重点，然后是两个js文件：

{% highlight js %}
 var app = app || {};
 
 (function () {
   'use strict';
 
   app.Utils = {
     uuid: function () {
       /*jshint bitwise:false */
       var i, random;
       var uuid = '';
 
       for (i = 0; i < 32; i++) {
         random = Math.random() * 16 | 0;
         if (i === 8 || i === 12 || i === 16 || i === 20) {
           uuid += '-';
         }
         uuid += (i === 12 ? 4 : (i === 16 ? (random & 3 | 8) : random))
                     .toString(16);
       }
 
       return uuid;
     },
 
     pluralize: function (count, word) {
       return count === 1 ? word : word + 's';
     },
 
     store: function (namespace, data) {
       if (data) {
         return localStorage.setItem(namespace, JSON.stringify(data));
       }
 
       var store = localStorage.getItem(namespace);
       return (store && JSON.parse(store)) || [];
     },
 
     extend: function () {
       var newObj = {};
       for (var i = 0; i < arguments.length; i++) {
         var obj = arguments[i];
         for (var key in obj) {
           if (obj.hasOwnProperty(key)) {
             newObj[key] = obj[key];
           }
         }
       }
       return newObj;
     }
   };
 })();
{% endhighlight %}

{% highlight js%}
 var app = app || {};
 
 (function () {
   'use strict';
 
   var Utils = app.Utils;
   // Generic "model" object. You can use whatever
   // framework you want. For this application it
   // may not even be worth separating this logic
   // out, but we do this to demonstrate one way to
   // separate out parts of your application.
   app.TodoModel = function (key) {
     this.key = key;
     this.todos = Utils.store(key);
     this.onChanges = [];
   };
 
   app.TodoModel.prototype.subscribe = function (onChange) {
     this.onChanges.push(onChange);
   };
 
   app.TodoModel.prototype.inform = function () {
     Utils.store(this.key, this.todos);
     this.onChanges.forEach(function (cb) { cb(); });
   };
 
   app.TodoModel.prototype.addTodo = function (title) {
     this.todos = this.todos.concat({
       id: Utils.uuid(),
       title: title,
       completed: false
     });
 
     this.inform();
   };
 
   app.TodoModel.prototype.toggleAll = function (checked) {
     // Note: it's usually better to use immutable data structures since they're
     // easier to reason about and React works very well with them. That's why
     // we use map() and filter() everywhere instead of mutating the array or
     // todo items themselves.
     this.todos = this.todos.map(function (todo) {
       return Utils.extend({}, todo, { completed: checked });
     });
 
     this.inform();
   };
 
   app.TodoModel.prototype.toggle = function (todoToToggle) {
     this.todos = this.todos.map(function (todo) {
       return todo !== todoToToggle ?
                 todo :
                 Utils.extend({}, todo, { completed: !todo.completed });
     });
 
     this.inform();
   };
 
   app.TodoModel.prototype.destroy = function (todo) {
     this.todos = this.todos.filter(function (candidate) {
       return candidate !== todo;
     });
 
     this.inform();
   };
 
   app.TodoModel.prototype.save = function (todoToSave, text) {
     this.todos = this.todos.map(function (todo) {
       return todo !== todoToSave ? todo : Utils.extend({}, todo, { title: text });
     });
 
     this.inform();
   };
 
   app.TodoModel.prototype.clearCompleted = function () {
     this.todos = this.todos.filter(function (todo) {
       return !todo.completed;
     });
 
     this.inform();
   };
 
 })();

{% endhighlight %}

utils为简单的工具类，不予理睬；无论什么时候数据层一定是MVC的重点，这里稍微给予一点关注：

1. model层实现了一个简单的事件订阅通知系统

2. 从类实现来说，他仅有三个属性，key（存储与localstorage的命名空间），todos（真实的数据对象），changes（事件集合）

3. 与backbone的model不同，backbone的数据操作占了其实现大部分篇幅，backbone的TodoMVC会完整定义Model的增删差改依次触发的事件，所以Model定义结束，程序就有了完整的脉络，而我们看react这里有点“弱化”数据处理的感觉

4. 总的来说，整个Model的方法皆在操作todos数据，subscribe用于注册事件，每次操作皆会通知changes函数响应，并且存储到localstorage，从重构的角度来说inform其实只应该完成通知的工作，存储的事情不应该做，但是这与我们今天所学没有什么管理，不予理睬，接下来我们进入View层的代码。
组件化编程

React号称组件化编程，我们从标签化、声明式编程的角度来一起看看他第一个View TodoItem的实现：
{% highlight  js %}
 var app = app || {};
 (function () {
     'use strict';
     var ESCAPE_KEY = 27;
     var ENTER_KEY = 13;
     app.TodoItem = React.createClass({
         handleSubmit: function (event) {
             var val = this.state.editText.trim();
             if (val) {
                 this.props.onSave(val);
                 this.setState({editText: val});
             } else {
                 this.props.onDestroy();
             }
         },
 
         handleEdit: function () {
             this.props.onEdit();
             this.setState({editText: this.props.todo.title});
         },
 
         handleKeyDown: function (event) {
             if (event.which === ESCAPE_KEY) {
                 this.setState({editText: this.props.todo.title});
                 this.props.onCancel(event);
             } else if (event.which === ENTER_KEY) {
                 this.handleSubmit(event);
             }
         },
 
         handleChange: function (event) {
             this.setState({editText: event.target.value});
         },
 
         getInitialState: function () {
             return {editText: this.props.todo.title};
         },
 
         /**
          * This is a completely optional performance enhancement that you can
          * implement on any React component. If you were to delete this method
          * the app would still work correctly (and still be very performant!), we
          * just use it as an example of how little code it takes to get an order
          * of magnitude performance improvement.
          */
         shouldComponentUpdate: function (nextProps, nextState) {
             return (
                 nextProps.todo !== this.props.todo ||
                 nextProps.editing !== this.props.editing ||
                 nextState.editText !== this.state.editText
             );
         },
 
         /**
          * Safely manipulate the DOM after updating the state when invoking
          * `this.props.onEdit()` in the `handleEdit` method above.
          * For more info refer to notes at https://facebook.github.io/react/docs/component-api.html#setstate
          * and https://facebook.github.io/react/docs/component-specs.html#updating-componentdidupdate
          */
         componentDidUpdate: function (prevProps) {
             if (!prevProps.editing && this.props.editing) {
                 var node = React.findDOMNode(this.refs.editField);
                 node.focus();
                 node.setSelectionRange(node.value.length, node.value.length);
             }
         },
 
         render: function () {
             return (
                 <li className={React.addons.classSet({
                     completed: this.props.todo.completed,
                     editing: this.props.editing
                 })}>
                     <div className="view">
                         <input
                             className="toggle"
                             type="checkbox"
                             checked={this.props.todo.completed}
                             onChange={this.props.onToggle}
                         />
                         <label onDoubleClick={this.handleEdit}>
                             {this.props.todo.title}
                         </label>
                         <button className="destroy" onClick={this.props.onDestroy} />
                     </div>
                     <input
                         ref="editField"
                         className="edit"
                         value={this.state.editText}
                         onBlur={this.handleSubmit}
                         onChange={this.handleChange}
                         onKeyDown={this.handleKeyDown}
                     />
                 </li>
             );
         }
     });
 })();
{% endhighlight %}

根据我们之前的知识，这里是创建了一个自定义标签，而标签返回的内容是：

{% highlight js%}
render: function () {
    return (
        <li className={React.addons.classSet({
            completed: this.props.todo.completed,
            editing: this.props.editing
        })}>
            <div className="view">
                <input
                    className="toggle"
                    type="checkbox"
                    checked={this.props.todo.completed}
                    onChange={this.props.onToggle}
                />
                <label onDoubleClick={this.handleEdit}>
                    {this.props.todo.title}
                </label>
                <button className="destroy" onClick={this.props.onDestroy} />
            </div>
            <input
                ref="editField"
                className="edit"
                value={this.state.editText}
                onBlur={this.handleSubmit}
                onChange={this.handleChange}
                onKeyDown={this.handleKeyDown}
            />
        </li>
    );
}
{% endhighlight %}

要展示这个View需要依赖其属性与状态：

{% highlight js%}
getInitialState: function () {
    return {editText: this.props.todo.title};
},
{% endhighlight %}

这里没有属性的描写，而他本身也仅仅是标签组件，更多的信息我们需要去看调用方，该组件显示的是body部分，TodoMVC还有footer部分的操作工具条，这里的实现便比较简单了：
 
{% highlight js %}
 var app = app || {};
 
 (function () {
     'use strict';
 
     app.TodoFooter = React.createClass({
         render: function () {
             var activeTodoWord = app.Utils.pluralize(this.props.count, 'item');
             var clearButton = null;
 
             if (this.props.completedCount > 0) {
                 clearButton = (
                     <button
                         className="clear-completed"
                         onClick={this.props.onClearCompleted}>
                         Clear completed
                     </button>
                 );
             }
 
             // React idiom for shortcutting to `classSet` since it'll be used often
             var cx = React.addons.classSet;
             var nowShowing = this.props.nowShowing;
             return (
                 <footer className="footer">
                     <span className="todo-count">
                         <strong>{this.props.count}</strong> {activeTodoWord} left
                     </span>
                     <ul className="filters">
                         <li>
                             <a
                                 href="#/"
                                 className={cx({selected: nowShowing === app.ALL_TODOS})}>
                                     All
                             </a>
                         </li>
                         {' '}
                         <li>
                             <a
                                 href="#/active"
                                 className={cx({selected: nowShowing === app.ACTIVE_TODOS})}>
                                     Active
                             </a>
                         </li>
                         {' '}
                         <li>
                             <a
                                 href="#/completed"
                                 className={cx({selected: nowShowing === app.COMPLETED_TODOS})}>
                                     Completed
                             </a>
                         </li>
                     </ul>
                     {clearButton}
                 </footer>
             );
         }
     });
 })();
{% endhighlight %}

我们现在将关注点放在其所有标签的调用方，app.jsx（TodoApp），因为我没看见这个TodoMVC的控制器在哪，也就是我没有看见控制逻辑的js文件在哪，所以控制流程的代码只能在这里了：
{% highlight js%}
 var app = app || {};
 (function () {
     'use strict';
     app.ALL_TODOS = 'all';
     app.ACTIVE_TODOS = 'active';
     app.COMPLETED_TODOS = 'completed';
     var TodoFooter = app.TodoFooter;
     var TodoItem = app.TodoItem;
 
     var ENTER_KEY = 13;
 
     var TodoApp = React.createClass({
         getInitialState: function () {
             return {
                 nowShowing: app.ALL_TODOS,
                 editing: null
             };
         },
 
         componentDidMount: function () {
             var setState = this.setState;
             var router = Router({
                 '/': setState.bind(this, {nowShowing: app.ALL_TODOS}),
                 '/active': setState.bind(this, {nowShowing: app.ACTIVE_TODOS}),
                 '/completed': setState.bind(this, {nowShowing: app.COMPLETED_TODOS})
             });
             router.init('/');
         },
 
         handleNewTodoKeyDown: function (event) {
             if (event.keyCode !== ENTER_KEY) {
                 return;
             }
 
             event.preventDefault();
 
             var val = React.findDOMNode(this.refs.newField).value.trim();
 
             if (val) {
                 this.props.model.addTodo(val);
                 React.findDOMNode(this.refs.newField).value = '';
             }
         },
 
         toggleAll: function (event) {
             var checked = event.target.checked;
             this.props.model.toggleAll(checked);
         },
 
         toggle: function (todoToToggle) {
             this.props.model.toggle(todoToToggle);
         },
 
         destroy: function (todo) {
             this.props.model.destroy(todo);
         },
 
         edit: function (todo) {
             this.setState({editing: todo.id});
         },
 
         save: function (todoToSave, text) {
             this.props.model.save(todoToSave, text);
             this.setState({editing: null});
         },
 
         cancel: function () {
             this.setState({editing: null});
         },
 
         clearCompleted: function () {
             this.props.model.clearCompleted();
         },
 
         render: function () {
             var footer;
             var main;
             var todos = this.props.model.todos;
 
             var shownTodos = todos.filter(function (todo) {
                 switch (this.state.nowShowing) {
                 case app.ACTIVE_TODOS:
                     return !todo.completed;
                 case app.COMPLETED_TODOS:
                     return todo.completed;
                 default:
                     return true;
                 }
             }, this);
 
             var todoItems = shownTodos.map(function (todo) {
                 return (
                     <TodoItem
                         key={todo.id}
                         todo={todo}
                         onToggle={this.toggle.bind(this, todo)}
                         onDestroy={this.destroy.bind(this, todo)}
                         onEdit={this.edit.bind(this, todo)}
                         editing={this.state.editing === todo.id}
                         onSave={this.save.bind(this, todo)}
                         onCancel={this.cancel}
                     />
                 );
             }, this);
 
             var activeTodoCount = todos.reduce(function (accum, todo) {
                 return todo.completed ? accum : accum + 1;
             }, 0);
 
             var completedCount = todos.length - activeTodoCount;
 
             if (activeTodoCount || completedCount) {
                 footer =
                     <TodoFooter
                         count={activeTodoCount}
                         completedCount={completedCount}
                         nowShowing={this.state.nowShowing}
                         onClearCompleted={this.clearCompleted}
                     />;
             }
 
             if (todos.length) {
                 main = (
                     <section className="main">
                         <input
                             className="toggle-all"
                             type="checkbox"
                             onChange={this.toggleAll}
                             checked={activeTodoCount === 0}
                         />
                         <ul className="todo-list">
                             {todoItems}
                         </ul>
                     </section>
                 );
             }
 
             return (
                 <div>
                     <header className="header">
                         <h1>todos</h1>
                         <input
                             ref="newField"
                             className="new-todo"
                             placeholder="What needs to be done?"
                             onKeyDown={this.handleNewTodoKeyDown}
                             autoFocus={true}
                         />
                     </header>
                     {main}
                     {footer}
                 </div>
             );
         }
     });
 
     var model = new app.TodoModel('react-todos');
 
     function render() {
         React.render(
             <TodoApp model={model}/>,
             document.getElementsByClassName('todoapp')[0]
         );
     }
 
     model.subscribe(render);
     render();
 })();
{% endhighlight %}

这里同样是创建了一个标签，然后最后一段代码有所不同：

{% highlight js %}
 var model = new app.TodoModel('react-todos');
 
 function render() {
     React.render(
         <TodoApp model={model}/>,
         document.getElementsByClassName('todoapp')[0]
     );
 }
 
 model.subscribe(render);
 render();
{% endhighlight %}

1. 这里创建了一个Model的实例，我们知道创建的时候，todos便由localstorage获取了数据（如果有的话）

2. 这里了定义了一个方法，以todoapp为容器，装载标签

3. 为model订阅render方法，意思是每次model有变化都将重新渲染页面，这里的代码比较关键，按照代码所示，每次数据变化都应该执行render方法，如果list数量比较多的话，每次接重新渲染岂不是浪费性能，但真实使用过程中，可以看到React竟然是局部刷新的，他这个机制非常牛逼啊！

4. 最后执行了render方法，开始了TodoApp标签的渲染，我们这里再将TodoApp的渲染逻辑贴出来

{% highlight js %}
 render: function () {
         var footer;
         var main;
         var todos = this.props.model.todos;
 
         var shownTodos = todos.filter(function (todo) {
             switch (this.state.nowShowing) {
             case app.ACTIVE_TODOS:
                 return !todo.completed;
             case app.COMPLETED_TODOS:
                 return todo.completed;
             default:
                 return true;
             }
         }, this);
 
         var todoItems = shownTodos.map(function (todo) {
             return (
                 <TodoItem
                     key={todo.id}
                     todo={todo}
                     onToggle={this.toggle.bind(this, todo)}
                     onDestroy={this.destroy.bind(this, todo)}
                     onEdit={this.edit.bind(this, todo)}
                     editing={this.state.editing === todo.id}
                     onSave={this.save.bind(this, todo)}
                     onCancel={this.cancel}
                 />
             );
         }, this);
 
         var activeTodoCount = todos.reduce(function (accum, todo) {
             return todo.completed ? accum : accum + 1;
         }, 0);
 
         var completedCount = todos.length - activeTodoCount;
 
         if (activeTodoCount || completedCount) {
             footer =
                 <TodoFooter
                     count={activeTodoCount}
                     completedCount={completedCount}
                     nowShowing={this.state.nowShowing}
                     onClearCompleted={this.clearCompleted}
                 />;
         }
 
         if (todos.length) {
             main = (
                 <section className="main">
                     <input
                         className="toggle-all"
                         type="checkbox"
                         onChange={this.toggleAll}
                         checked={activeTodoCount === 0}
                     />
                     <ul className="todo-list">
                         {todoItems}
                     </ul>
                 </section>
             );
         }
 
         return (
             <div>
                 <header className="header">
                     <h1>todos</h1>
                     <input
                         ref="newField"
                         className="new-todo"
                         placeholder="What needs to be done?"
                         onKeyDown={this.handleNewTodoKeyDown}
                         autoFocus={true}
                     />
                 </header>
                 {main}
                 {footer}
             </div>
         );
     }
{% endhighlight%}

说句实话，这段代码不知为什么有一些令人感到难受......

#### 1. 他首先获取了注入的model实例，获取其所需的数据todos，注入点在：

{% highlight html %}
<TodoApp model={model}/>
{% endhighlight%}

#### 2. 然后他由自身状态机，获取真实要显示的项目，其实这里如果不考虑路由的变化，完全显示即可

{% highlight js %}
 getInitialState: function () {
     return {
         nowShowing: app.ALL_TODOS,
         editing: null
     };
 },
{% endhighlight%}

#### 3. 数据获取成功后，便使用该数据组装为一个个独立的TodoItem标签：

{% highlight js %}
 var todoItems = shownTodos.map(function (todo) {
     return (
         <TodoItem
             key={todo.id}
             todo={todo}
             onToggle={this.toggle.bind(this, todo)}
             onDestroy={this.destroy.bind(this, todo)}
             onEdit={this.edit.bind(this, todo)}
             editing={this.state.editing === todo.id}
             onSave={this.save.bind(this, todo)}
             onCancel={this.cancel}
         />
     );
 }, this);
{% endhighlight%}

标签具有很多事件，这里要注意一下各个事件这里事件绑定与控制器上绑定有何不同

#### 4. 然后其做了一些工作处理底部工具条或者头部全部选中的工作

#### 5. 最后开始渲染整个标签：

{% highlight js %}
 return (
     <div>
         <header className="header">
             <h1>todos</h1>
             <input
                 ref="newField"
                 className="new-todo"
                 placeholder="What needs to be done?"
                 onKeyDown={this.handleNewTodoKeyDown}
                 autoFocus={true}
             />
         </header>
         {main}
         {footer}
     </div>
 );
{% endhighlight%}

该标签事实上为3个模块组成的了：header部分、body部分、footer部分，模块与模块之间的通信依赖便是model数据了，因为这里最终的渲染皆在app的render处，而render处渲染所有标签全部共同依赖于一个model，就算这里依赖于多个model，只要是统一在render处做展示即可。
流程分析

我们前面理清了整个脉络，接下来我们理一理几个关键脉络：

#### 1. 新增

TodoApp为其头部input标签绑定了一个onKeyDown事件，事件代理到了handleNewTodoKeyDown：

{% highlight js %}
 handleNewTodoKeyDown: function (event) {
     if (event.keyCode !== ENTER_KEY) {
         return;
     }
 
     event.preventDefault();
 
     var val = React.findDOMNode(this.refs.newField).value.trim();
 
     if (val) {
         this.props.model.addTodo(val);
         React.findDOMNode(this.refs.newField).value = '';
     }
 },
{% endhighlight%}


因为用户输入的数据不能由属性或者状态值获取，这里使用了dom操作的方法获取输入数据，这里的钩子是ref，事件触发了model新增一条记录，并且将文本框置为空，现在我们进入model新增的逻辑：

{% highlight js %}
 app.TodoModel.prototype.addTodo = function (title) {
   this.todos = this.todos.concat({
     id: Utils.uuid(),
     title: title,
     completed: false
   });
 
   this.inform();
 };
{% endhighlight%}

model以最简的方式构造了一个数据对象，改变了todos的值，然后通知model发生了变化，而我们都知道informa程序干了两件事：
 {% highlight js %}
 app.TodoModel.prototype.inform = function () {
   Utils.store(this.key, this.todos);
   this.onChanges.forEach(function (cb) { cb(); });
 };
{% endhighlight%}
存储localstorage、触发订阅model变化的回调，也就是：
{% highlight js %}
 function render() {
     React.render(
         <TodoApp model={model}/>,
         document.getElementsByClassName('todoapp')[0]
     );
 }
 
 model.subscribe(render);
{% endhighlight%}

于是整个标签可耻的重新渲染了，我们再来看看编辑是怎么回事：

#### 2. 编辑

这个编辑便与TodoApp没有什么关系了：
{% highlight html %}
 <label onDoubleClick={this.handleEdit}>
     {this.props.todo.title}
 </label>
{% endhighlight%}
当双击标签项时，触发了代理的处理程序：
{% highlight js %}
 handleEdit: function () {
     this.props.onEdit();
     this.setState({editText: this.props.todo.title});
 },
{% endhighlight%}
这里他做了两个事情：

onEdit，为父标签注入的方法，他这里执行函数作用域是指向this.props的，所以外层定义时指定了作用域：
复制代码
{% highlight js %}
 return (
     <TodoItem
         key={todo.id}
         todo={todo}
         onToggle={this.toggle.bind(this, todo)}
         onDestroy={this.destroy.bind(this, todo)}
         onEdit={this.edit.bind(this, todo)}
         editing={this.state.editing === todo.id}
         onSave={this.save.bind(this, todo)}
         onCancel={this.cancel}
     />
 );
{% endhighlight%}

其次，他改变了自身状态机，而状态机或者属性的变化皆会引起标签重新渲染，然后当触发keydown事件后，完成的逻辑便与上面一致了

## 思考

经过之前的学习，我们对React有了一个大概的了解，是时候搬出React设计的初衷了：

- Just the ui
- virtual dom
- data flow

后面两个概念还没强烈的感触，这里仅仅对Just the ui有一些认识，似乎React仅仅提供了MVC中View的实现，但是这个View又强大到可以抛弃C了，可以看到上述代码控制器被无限的弱化了，而我觉得React其实真实想提供的可能是一种开发方式的思路，React便是如何帮你实现这种思路的方案：

模块化编程、组件化编程、标签化编程，可能是React真正想表达的思想

我们在组织负责业务逻辑时，也会分模块、分UI，但是我们一般是采用控制器调用组件的方式使用，React这里不同的一点是使用标签分模块，孰优孰劣要真实开发过生产项目的朋友才能认识，真实的应用路由的功能必不可少，应该有不少插件会主动抱大腿，但使用灵活性仍然得项目实践验证。

react本身很干净，不包括模块加载的机制，真正发布生产前需要通过webpack打包处理，但是对于复杂项目来说，按需加载是必不可少的，这块不知道如何

而我的关注点仍然落在了样式上，之前做组件或者做页面时，有一个优化方案，是将对应的样式作为一个View的依赖项加载，一个View保持最小的html&css&js量加载，而react对样式与动画一块的支持如何，也需要生产验证；复杂的项目开发，Model的设计一定是至关重要的，也许借鉴Backbone Model的实现+React的View处理，会是一个不错的选择

最后，因为现在没有生产项目能让我使用React试水，过多的话基本就是意淫了，根据我之前MVC的使用经验，感觉灵活性上估计React仍然有一段路要走，但是其模块化编程的思路倒是对我的项目有莫大的指导作用，对于这门技术的深入，经过今天的学习，我打算再观望一下，不知道angularJS怎么样，我也许该对这门MVVM的框架展开调研

> 本文转载自 [http://www.cnblogs.com/yexiaochai/p/4853398.html](http://www.cnblogs.com/yexiaochai/p/4853398.html)