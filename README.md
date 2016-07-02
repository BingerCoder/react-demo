> 阅读前请看这里：
>  * 了解js及jQuery的使用
>  * 对react有一定的了解，知道jsx的语法
>  * 这里只讲述如何使用react，并不介绍react的优缺点

> 如果不满足这些，建议先了解下，然后再看这篇文章

下面会讲述5个react的实例，虽然仅有5个，但在常用的开发中，几乎会包含大部分的情况，只要熟练掌握这5个demo，相信一定会解决大部分问题。

demo中，所有样例会打包后，传递到附件，大家可以下载阅览，最好自己亲手实践下，不要直接copy代码，没有意义。

使用的react版本是0.14.7

# DEMO 1	- 最简单的react渲染

### 代码：

```html
<html>
    <head>
        <link href="css/bootstrap.min.css" rel="stylesheet">
    </head>
    
    <body>
        <h1><span class="label label-info">DEMO 1</span></h1>
        <br><br><br>
        
        
        <div class="well" id="well">
            
        </div>

        <script src="js/jquery.min.js"></script>
        <script src="js/react.js"></script>
        <script src="js/react-dom.js"></script>
        <script src="js/browser.min.js"></script>

        <script type="text/babel">
        var Text = React.createClass({
            render: function() {
                return (
                    <div className="a">
                        大家好，我是用react渲染出来的！
                    </div>
                );
            }
        });
        
        ReactDOM.render(
            <Text/>,
            document.getElementById('well')
        );
        </script>
    </body>
</html>
```
### 在浏览器中显示的效果如下  

![这里写图片描述](http://img.blog.csdn.net/20160701174206306)

### 讲解：

* 页面中，只有`<div id='well'>`这里的内容是使用react渲染出来的，代码中这里是空的，依赖下面的js进行渲染
* 首先看下 `vat Text = ` 这块，这里是声明一个模块，名字随意起，我们把第一个字母大写，用来区分html中原生的标签
```html
React.createClass({
    render: function() {
        return (
            <div className="a">
                大家好，我是用react渲染出来的！
            </div>
        );
    }
});
```
这块是创建一个模块，使用`React.createClass`即可创建。
* 其中参数有很多，但都可以省略，唯有`render`不可以省略，因为这是用来表述这个插件被加载后，显示的是什么样子，它的返回结果，就是加载在页面上最终的样式。
```html
ReactDOM.render(
   <Text/>,
    document.getElementById('well')
);
```
这段代码是用来渲染react组件额，第一个参数是组件，第二个参数是要渲染的位置。
* 使用`<Text/>`的 方式就可以实例化组件，或者写成`<Text></Text>`，要注意下，react中标签的闭合非常严格，任何标签的关闭与打开必须一一对应，否则会报错。    
* 到目前为止，就完成了一次渲染，将Text组件render函数返回的内容，填充到了id=well的div中。

---

# DEMO 2	- 带有参数的react

往往在使用中，文本的内容并不是写死的，而是需要被我们指定，这样组件才能更通用。下面介绍下，如何向react中传递参数。

###代码：
```html
<html>
    <head>
        <link href="css/bootstrap.min.css" rel="stylesheet">
    </head>
    
    <body>
        <h1><span class="label label-info">DEMO 2</span></h1>
        <br><br><br>
        
        
        <div class="well" id="well">
            
        </div>

        <script src="js/jquery.min.js"></script>
        <script src="js/react.js"></script>
        <script src="js/react-dom.js"></script>
        <script src="js/browser.min.js"></script>

        <script type="text/babel">
        var Text = React.createClass({
            render: function() {
                return (
                    <div className="a">
                        大家好，我是用{this.props.name}渲染出来的！age={this.props.age}
                    </div>
                );
            }
        });
        
        ReactDOM.render(
            <Text name="react" age={181}></Text>,
            document.getElementById('well')
        );

        </script>
    </body>
</html>
```

### 在浏览器中显示的效果如下  

![这里写图片描述](http://img.blog.csdn.net/20160701175602910)   

### 讲解
* 首先，这个大体上跟第一个demo类似，唯有实例化Text时，多了参数。
* 当我们传递参数时，写了两种方式，一种是 `name="react"`另一种是`age={181}`，这两种写法是有区别的，并不仅仅因为一个是`str`，一个是`int`。如果是`str`这种类型，写成 `name="xxx"`或者`name={"xxx"}`都是可以的，加了{}的意思就是js中的变量，更加精确了。而后者`age={181}`是不可以去掉{}的，这样会引起异常，所以这里要注意下，并且建议任何类型都加上{}来确保统一。
* 当在Text初始化时添加了参数，在组件内部，都收集在this.props中，使用时只要{this.props.name}既可以获取name对应的值，如果取得key并不存在，这里不会报错，只是取到的值是空的。当然可以在`getDefaultProps`中定义默认的props值，即使在没有传递参数的情况下，也能取到默认值。
* props中的参数，在初始化传递后，便不能再修改。

---

# DEMO 3		- state，react的核心

state算是react的核心了，任何页面的变化，刷新都是state的变化引起。在react中，只要调用了setState都会引起render的重新执行。下面介绍下如何通过键盘事件触发状态变化。

### 代码：
```html
<html>
    <head>
        <link href="css/bootstrap.min.css" rel="stylesheet">
    </head>
    
    <body>
        <h1><span class="label label-info">DEMO 3</span></h1>
        <br><br><br>
        
        
        <div class="well" id="well">
            
        </div>

        <script src="js/jquery.min.js"></script>
        <script src="js/react.js"></script>
        <script src="js/react-dom.js"></script>
        <script src="js/browser.min.js"></script>

        <script type="text/babel">
        var Text = React.createClass({
            getInitialState: function() {
                return {name: "react"};
            },
            keyUp: function(e){
                this.setState({name: e.target.value});
                console.log(this.state.name);
            },
            render: function() {
                return (
                    <div className="a">
                        大家好，我是用{this.state.name}渲染出来的！
                        <input onKeyUp={this.keyUp} />
                    </div>
                );
            }
        });
        
        ReactDOM.render(
            <Text></Text>,
            document.getElementById('well')
        );

        </script>
    </body>
</html>
```

### 在浏览器中显示的效果如下  

![这里写图片描述](http://img.blog.csdn.net/20160702090408416)

### 讲解：

* 这次组件中多了两个函数，`getInitialState`和`keyUp`，其中getInitialState是react中的初始化状态的函数，类似上一节中`getDefaultProps`的作用。keyUp是我自定义的函数，用来响应键盘事件的。
* 我们先看render函数，文字渲染中加了{this.state.name}，这个是react内部的状态，可以理解是存储数据的k-v结构，这里的v支持的对象较多。{this.state.name}就是引用状态中的name属性，与props的区别在于，如果state中不存在这个属性，是会报错的，所以我们要在getInitialState中初始化这个状态的初始值。
* render中还多了一个<input onKeyUp={this.keyUp} />，onKeyUp是注册键盘键弹起的事件，当按键按下后弹起，就会触发onKeyUp事件，然后通过绑定的this.keyUp，将事件传递给了自己定义的keyUp函数中。
* keyUp函数中，使用了this.setState({name: e.target.value})，setState是react中内部的函数，专门用来更新状态的，这里是讲状态中name的值变更为引起事件的value值。
* 在react中，每次状态的变化，都会引起render函数的重新渲染，这是它自己的机制，我们无需人为处理，当键盘输入内容时，会触发状态变化，导致render重新渲染，渲染的过程会从state中取出变量，所以我们就看到了页面的内容发生了变化。
* 我们在setState下面加了一个console，通过控制台可以发现，每次打印的值并不是当前输入的值，而是上一次输入的值，这是怎么回事呢？在setState中，这是一个异步处理的函数，并不是同步的，console在setState后立刻执行了，所以这时候状态还没有真正变更完，所以这里取到的状态仍旧是更新前的。这里要特殊注意下。如果需要在更新状态后，再执行操作怎么办呢，setState还有第二个参数，接受一个callback，我们尝试将keyUp中代码改成这样

```html
this.setState({name: e.target.value}, function(){
    console.log(this.state.name);
})
```
* 这时候log打印出来的只就是我们期望的内容，当每次状态更新成功后，都会调用传进去的callback函数。
* react中渲染dom有自己的优化方式，首先它在内存中构建一套虚拟的dom，每次更新前将虚拟dom与浏览器中dom对比，只讲有变化的部分进行更新，这样大大的提高了性能。或者我们可以重写函数来控制是否刷新，当然这种方式我们并不提倡。

---

# DEMO 4		-	网络请求触发状态变化

上一节讲到状态变化触发render的重新渲染，这里将常用的网络请求引入，结合到状态变化中。

### 代码：
```html
<html>
    <head>
        <link href="css/bootstrap.min.css" rel="stylesheet">
        <style>
            .div {
                height: 100px;
            }
        </style>
    </head>
    
    <body>
        <h1><span class="label label-info">DEMO 4</span></h1>
        <br><br><br>
        
        
        <div class="well div" id="well">
            
        </div>

        <script src="js/jquery.min.js"></script>
        <script src="js/react.js"></script>
        <script src="js/react-dom.js"></script>
        <script src="js/browser.min.js"></script>

        <script type="text/babel">
        var Text = React.createClass({
            getInitialState: function() {
                return {cur_time: 0};
            },
            request: function() {
                $.ajax({
                    type: "GET",
                    url: "http://xxx",
                    success: function(data){
                        this.setState({cur_time: data.timestamp});
                    }.bind(this),
                    complete: function(){
                        this.setState({cur_time: Date.parse(new Date()) / 1000});
                    }.bind(this)
                });
            },
            componentDidMount: function(){
                setInterval(this.request, 1000);
            },
            render: function() {
                return (
                    <div className="col-xs-12">
                        当前时间{this.state.cur_time}
                        <div className={ this.state.cur_time % 2 == 0?"hidden": "col-xs-6 alert alert-success"}>
                            <span>最后一位奇数</span>
                        </div>
                        <div className={ this.state.cur_time % 2 != 0?"hidden": "col-xs-6 alert alert-danger"}>
                            <span>最后一位偶数</span>
                        </div>
                    </div>
                );
            }
        });
        
        ReactDOM.render(
            <Text></Text>,
            document.getElementById('well')
        );

        </script>
    </body>
</html>
```

### 在浏览器中显示的效果如下

![这里写图片描述](http://img.blog.csdn.net/20160702100128997)
![这里写图片描述](http://img.blog.csdn.net/20160702100143399)  

###讲解：

* 这个例子中，页面每秒会请求一次网络，将请求到的数据中时间戳更新到状态中。但是这个例子中实际上并没有使用服务返回的时间戳，因为我在公司使用测试网的接口拿数据，但是开放给大家要换一个公网能拿到时间戳的api，简单的找了下没找到，也不想用公司的接口试，所以算是mock了下~，在complete中每次都取浏览器时间来更新。
* 仍旧是先看代码，相比于上一个例子，这里多了两个函数`request`和`componentDidMount`，其中`request`是请求网络的函数，`componentDidMount`是react内部的函数，也是react生命周期的一部分，它会在render第一次渲染前执行，而且只会执行一次。
* 先看request，一个普通的ajax请求，在success回调中，假设服务器返回的json为{"timestamp": 1467425645}，那data.timestamp就取得是1467425645，然后将值赋给state中的cur_time属性。这时状态发生了变化，render函数会重新渲染。当然例子中success不会被访问到，因为那个url根本不存在，所以我在complete回调中来写了一个状态变化，模拟success成功。
* 为什么success回调函数最后会加一个bind(this)？因为这个函数已经不是react内部的函数了，它是一个外部函数，它里面的this并不是react组件中的this，所以要将外部函数绑定到react中，并能使用react内部的方法，例如setState，就要在函数最后bind(this)，这样就完成了绑定。
* 再看下`componentDidMount`函数，这个函数在render渲染前会执行，里面的代码也很简单，增加了一个定时器，1秒钟执行一次request。
* 这里应该在加一个回调，就是定时器在初始化时创建，却没有对应的销毁，所以在组件销毁的时候，应该在这个生命周期中销毁定时器。

---

# DEMO 5		-	组件的嵌套使用

在封装react时，我们往往按照最小单位封装，例如封装一个通用的div，一个通用的span，或者一个通用的table等，所以各自组件对应的方法都会随着组件封装起来，例如div有自己的方法可以更改背景色，span可以有自己的方法更改字体大小，或者table有自己的方法来更新table的内容等~ 这里我们用一个div相互嵌套的例子来查看父子组件如何相互嵌套及调用各自的方法。在下面的例子中，父组件与子组件都有一个方法，来改变自身的背景色，我们实现父子组件相互调用对方的方法，来改变对方的背景色。

### 代码：
```html
<html>
    <head>
        <link href="css/bootstrap.min.css" rel="stylesheet">
        <style>
            .child {
                border-color: black;
                border: 1px solid;
                height: 200px;
            }
            .parent {
                height: 400px;
                border: 3px solid;
            }
        </style>
    </head>
    
    <body>
        <h1><span class="label label-info">DEMO 5</span></h1>
        <br><br><br>
        
        
        <div id="well">
            
        </div>

        <script src="js/jquery.min.js"></script>
        <script src="js/react.js"></script>
        <script src="js/react-dom.js"></script>
        <script src="js/browser.min.js"></script>

        <script type="text/babel">
        var Child = React.createClass({
            getInitialState: function() {
                return {color: ""};
            },
            changeColor: function(e) {
                this.setState({color: e.target.getAttribute("data-color")});
            },
            render: function() {
                return (
                    <div style={{backgroundColor: this.state.color}} className="col-xs-5 col-xs-offset-1 child">
                        <br/>
                        <ul className="list-inline">
                            <li><a href="#" data-color="#286090" className="btn btn-primary" onClick={this.props.parentChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#31b0d5" className="btn btn-info" onClick={this.props.parentChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#c9302c" className="btn btn-danger" onClick={this.props.parentChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#ec971f" className="btn btn-warning" onClick={this.props.parentChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#e6e6e6" className="btn btn-default" onClick={this.props.parentChangeColor}>&nbsp;</a></li>
                        </ul>
                    </div>
                );
            }
        });

        var Parent = React.createClass({
            getInitialState: function() {
                return {color: ""};
            },
            changeColor: function(e) {
                this.setState({color: e.target.getAttribute("data-color")});
            },
            child1ChangeColor: function(e) {
                this.refs["child1"].changeColor(e);
            },
            child2ChangeColor: function(e) {
                this.refs["child2"].changeColor(e);
            },
            render: function() {
                return (
                    <div style={{backgroundColor: this.state.color}} className="col-xs-10 col-xs-offset-1 parent">
                        <br/>
                        <ul className="list-inline">
                            <li>对应第一个child</li>
                            <li><a href="#" data-color="#286090" className="btn btn-primary" onClick={this.child1ChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#31b0d5" className="btn btn-info" onClick={this.child1ChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#c9302c" className="btn btn-danger" onClick={this.child1ChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#ec971f" className="btn btn-warning" onClick={this.child1ChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#e6e6e6" className="btn btn-default" onClick={this.child1ChangeColor}>&nbsp;</a></li>
                        </ul>
                        <ul className="list-inline">
                            <li>对应第二个child</li>
                            <li><a href="#" data-color="#286090" className="btn btn-primary" onClick={this.child2ChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#31b0d5" className="btn btn-info" onClick={this.child2ChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#c9302c" className="btn btn-danger" onClick={this.child2ChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#ec971f" className="btn btn-warning" onClick={this.child2ChangeColor}>&nbsp;</a></li>
                            <li><a href="#" data-color="#e6e6e6" className="btn btn-default" onClick={this.child2ChangeColor}>&nbsp;</a></li>
                        </ul>
                        <hr/>

                        <Child ref="child1" parentChangeColor={this.changeColor} />
                        <Child ref="child2" parentChangeColor={this.changeColor} />
                    </div>
                );
            }
        });

        
        ReactDOM.render(
            <Parent/>,
            document.getElementById('well')
        );

        </script>
    </body>
</html>
```

### 在浏览器中显示的效果如下

![这里写图片描述](http://img.blog.csdn.net/20160702104655592)

### 讲解：

* 首先说下，刚打开页面并不是这样的，背景都是白色的。这里的截图是点击各个按钮后变色的样子。
* 在这个例子中，蓝色的div是一个父组件，它里面包含了两个子组件，分别是红色和橙色，这两个子组件实际上是一模一样的。我们先看下父组件如何调用子组件。
* 代码中，子组件里面定义了changeColor函数，用来接收onClick事件，并将点击的按钮的data-color属性值作为色值，更改到state中的color属性中，然后触发render来更新背景色。在父组件调用子组件时，我们写了<Child ref="child1" parentChangeColor={this.changeColor} />，里面的ref="child1"就是react中提供的一个属性标签，它与普通的props不同，这里写上ref="xxx"后，在父组件中，使用this.refs["child1"]就可以引用对应的子组件，当然这里的ref的值是可以随意定义，只要不重复就好。这样就可以实现组组件引用子组件，然后直接调用里面的方法就好，例如`child1ChangeColor`中就有`this.refs["child1"].changeColor(e);`的使用。连起来说下逻辑，在点击父组件中第一列中的按钮后，触发onClick事件，然后onClick事件后，传递到child1ChangeColor后，将事件传递进入，然后再次传递给子组件的changeColor中，因为子组件的changeColor是更改子组件自身的state，所以这时候子组件再次渲染，于是改变了颜色。这就是父组件调用子组件的逻辑。
* 再说下子组件何如调用父组件的方法，父组件自身也有一个changeColor函数，用来改变自身的背景色。当父组件调用子组件时，<Child ref="child1" parentChangeColor={this.changeColor} />，通过props，也就是第二个例子中讲的那样，通过参数的方式传递给子组件，这样子组件中就可以使用this.props.parentChangeColor，来把子组件的onClick事件传递给父组件的changeColor方法中，来改变父组件的背景色。这就是子组件调用父组件函数的方法。
* 还有一种情况，就是一个父组件下有多个子组件，但是子组件中并没有直接的关系，这时候如果一个子组件调用另一个子组件的方法，就得通过他们共同的父组件来作为中转，在父组件中增加函数来作为中转的函数，来实现子组件间的调用。

## 好啦，以上就是这次分享给大家的react的5个DEMO，虽然只有5个demo，但在常用的情况下应该可以满足大部分的需求，只要非常了解这5个demo就好了。更多的细节大家可以去react官网看下文档，例如事件，生命周期等，都有较详细的说明。英文文档大家不用担心看不懂，在react文档地址后面，加一个zh-CH就会变成中文啦~ 例如这个  https://facebook.github.io/react/docs/getting-started.html 可改成 https://facebook.github.io/react/docs/getting-started-zh-CN.html ，这样就可以看了。

附件地址在github上~ 

