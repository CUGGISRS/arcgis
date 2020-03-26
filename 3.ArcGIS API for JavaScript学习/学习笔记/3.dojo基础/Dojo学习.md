## Dojo学习

---

### Dojo架构

> ##### 语言库
>
> ##### 特殊环境库
>
> ##### 应用支撑库
>
> ##### 工具包库

### Dojo Toolkit特性	

#### -Base

> Base包提供**Dojo Toolkit的基础**，包括一些功能
>
> > 比如DOM使用函数
> >
> > CSS3基于选择器的DOM查询
> >
> > 事件处理
> >
> > 基本的动画
> >
> > 以及Dojo基于类的面向对象特性

#### -Core

> 包含Base中没有包含的**附加特性**
>
> 提供一些实际有用的组件
>
> > 包括
> >
> > > 高级动画拖放
> > >
> > > I/O
> > >
> > > 数据管理
> > >
> > > 浏览器历史管理

#### -Dijit

> 包含Dojo小部件和组件的扩展UI库
>
> 这些小部件包括
>
> > 对话框
> >
> > 日历
> >
> > 调色板
> >
> > 工具提示和树
> >
> > 也包括一些表单控件等

#### -DojoX

> DojoX 包含工具箱的各个子项目。
>
> 位于DojoX中的大多数是实验特性，但是也有一些稳定组件和特性



### Dojo对标签内容操作

##### dojo.ready

> 页面运行成功之后加载代码
>
> > ``dojo.ready(function(){``
> >
> > ``})``

##### dojo.byId

> 通过id属性选择一个DOM节点
>
> > 是dicument.getElementById函数的一个别名
>
> > `dojo.byId("demo");`
> >
> > > 获取id为demo的元素

##### dojo.query

> 一次可以获取多个元素
>
> > 可以接收一个字符串参数
> >
> > 使用一个CSS3选择器引用想要选择的元素。
>
> > dojo.query(".class")
> >
> > > 获取页面中某一个class的所有元素
> > >
> > > > 返回值为NodeList，可以通过list来操作每一个元素

##### dojo.body

> 返回document的body元素

##### dojo.create

> `dojo.create(str,attrs,refNode,pos)`
>
> > str：元素类型：button，div等
> >
> > attrs：属性：size，color
> >
> > refNode：添加到哪里
> >
> > pos：添加到的位置
> >
> > > [null]：新创建的元素将作为refNode的子元素
> > >
> > > [before]：新创建的元素将作为refNode的同辈元素，且位于refnode的前边
> > >
> > > [after]：先创建的元素将作为refNode的同辈元素，且位于refNode的后边
> > >
> > > [only]：新创建的元素将取代父元素内所有的子元素，添加到refNode内部
> > >
> > > [replace]：新创建的元素将直接替换父元素
> > >
> > > [first]：新创建的元素将作为refNode的子元素，并添加到所有子元素的最前边
> > >
> > > [last]：新创建的元素将作为redNode的子元素，并添加到所有子元素的最后边

##### dojo.destroy

> 从父元素中删除该元素，并删除掉该元素中的所有子元素
>
> > `var node = dojo.byId("node");`
> >
> > dojo.empty(node);
> >
> > > 删除节点的所有子节点
> >
> > dojo.destroy(node)
> >
> > > 删除节点，及所有子节点

##### 案例

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <title></title>    
    <script type="text/javascript" src="dojo/dojo/dojo.js"></script>
    <link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css"/>
    <script type="text/javascript" >
        dojo.ready(function () {
           // alert("你好");
           // var div1 = dojo.byId("div1");
           // div1.innerHTML = "我是DIV1";
            var div2 = dojo.query("#div1")[0];
            div2.innerHTML = "我是query";
            
            dojo.create("div", { innerHTML: "<p>hi</p>" }, dojo.body());
            //dojo.empty(div2);
            dojo.destroy(div2);

        });
    </script>
</head>
<body>
    <div id="div1"></div>
</body>
</html>
```



### dojo面向对象

#### 类的声明-定义

> 语法
>
> > `dojo.declare(className,superclass, props);`
> >
> > > className：定义类的名称
> > >
> > > superclass：父类
> > >
> > > props：该类所有的字段以及方法
> > >
> > > > props中可以定义一个特殊的函数`constructor`，该类在实例化的时候会自动被调用到，相当于**构造函数**

###### 案例

```javascript
dojo.declare('People',null,{  
    name:‘小明',  
    constructor:function(name){  
        this.name=name;  
    }  
});  
var p=new People('Jack'); 
```

#### 继承

###### 案例

```javascript
// 定义“人”这个类
dojo.declare('People',null,{  
    name:'unknown name',  
    action:function(){  
        //do nothing  
    },  
    constructor:function(name){  
        this.name=name;  
    }  
}); 
// 定义学生这个类
dojo.declare('Student',People,{  
    school:'',  
    action:function(){  
        //I am studing  
    },  
    constructor:function(name,school){  
        this.school=school;  
    }  
});  

var s=new Student('Jack','Harvard');  
s.name    // Jack  
s.school 　// Harvard  
s.action 　// I am studing
```

#### 静态域

###### 案例

> 通过模拟的方式来达到静态域的效果

```javascript
dojo.declare("Foo", null, {  
    staticFields: { num: 0 },  
    add:function(){  
        this.staticFields.num++;  
    }  
});  
var f1=new Foo();  
var f2=new Foo();  
f1.add();  
f2.add();  
console.log(f1.staticFields.num ) //2 
```

#### 调用父类的方法

###### 案例：

```javascript
dojo.declare("Foo", null, {  
    constructor:function(){ console.log('foo')  }  
});  
dojo.declare("Bar", Foo, {  
    constructor:function(){ console.log('bar')  }  
});  
var b=new Bar; // 自动调用,打印foo bar  
```



#### 定义扩展(extend)

> 可以用它来添加重名的属性
>
> 不过这样会有一定的风险替换掉原先已经定义的属性

###### 案例

```javascript
dojo.declare('A',null,{  
    func1:function(){ console.log('fun1')}  
});  
A.extend({  
    func1:function(){ console.log('fun2')},  
    func2:function(){ console.log('fun3')}  
});  
var a=new A;  
a.func1();      //fun2  
a.func2();    
```



#### Define函数

> 作用是定义一个模块
>
> 这个模块可以被require引用，引用了之后就可以使用define里面的东西

###### 案例

```javascript
define([ "dojo/dom"], function(dom) {  
    return {  
        setRed: function(id){  
            dom.byId(id).style.color = "red";  
        }  
    };  
});  
```



### dojo常用函数

#### Require函数

> Require函数的主要是引入组件和模块的作用

###### 案例

```javascript
// 老方式
dojo. require("dijit.form.Button");
// 新方式
require([“dijit/form/Button”, 
         “dojox/layout/ContentPane”, 
         ...
        ], 
         function(Button, ContentPane, ...){　
         }
);
```

###### Require调用Define定义的模块

```javascript
<script>  
    require(  
        [ "dojo/ready", "test/util" ],  
        function(ready, util) {  
            ready(function() {  
                var id = "selected_text";  
                util.setRed(id);  
            });  
        });  
</script>  
```

#### 处理ajax，异步请求

###### 格式

> Request(URL,Option)
>
> > Option参数通常可以省略
> >
> > > 常用配置参数
> > >
> > > > method
> > > >
> > > > > 用于本请求的HTTP方法，默认是GET
> > > >
> > > > query
> > > >
> > > > > 参数，形如{key:
> > > > > 'value'}
> > > >
> > > > data
> > > >
> > > > > 字符串或对象
> > > >
> > > > handleAs
> > > >
> > > > > 表示如何处理服务器端响应的字符串，默认"text"，其他可能的值包括'json', 'javascript',以及'xml'
> > > >
> > > > headers
> > > >
> > > > > 形如{'Header-Name':
> > > > > 'value'}的对象，包含请求所需要的各种头部属性
> > > >
> > > > timeout
> > > >
> > > > > 表示等待多少毫秒算超时

###### 案例

```javascript
Require(“dojo/request”,Function(request){
Request(url,options).then(
Function(data)
{
处理成功返回的数据
},function(err)
{
处理请求的失败
},function(evt)
{
处理progress事件
});
});
```





### dojo配置

##### dojoConfig

> 用于设置一些在Dojo运行时的选项和默认的行为方式
>
> > 首先要定义dojoConfig设置一些属性
> >
> > 然后加载dojo.js
> >
> > > 如果这个过程反过来，那dojoConfig的配置则无效

###### has()属性

> has()
>
> >  用来设置一些Dojo支持的系统特性。
> >  has: {
> >
> >  "dojo-firebug": true,//加载Dojo版的Firebug调试环境，
> >
> > //如果浏览器没有自带调试工具，可以用：   
> >
> > "dojo-debug-messages": true	//显示调试信息，针对于一些废弃的或测试中的功能特性在运行时的信息
> >
> > }

###### Loader Configuration属性

> 加载时一些常用选项 

###### packages

> 提供包名及其路径
>
> > packages: [{
> >
> > ​    name: "myapp",
> >
> > ​    location: "/js/myapp"
> >
> > }]

###### aliases

> 设置别名
>
> > aliases: [
> >
> > ​    // [alias name, true name]
> >
> > ​    ["cookie", "dojo/cookie"]
> >
> > ]

###### async

> 是否异步加载
>
> > `async:true/false/legacyAsync`

###### parseOnLoad

> 是否在DOM和所有初始化完成后由dojo.parser解析页面
>
> > `parseOnLoad:true/false `

###### locale

> 本地化与国际化
>
> > `locale: location.search.match(/locale=([\w\-]+)/) ? RegExp.$1 : "en-us"`

###### 案例

```html
<script>
    dojoConfig= {
        has: {
            "dojo-firebug": true
        },
        parseOnLoad: false,
        foo: "bar",
        async: true,        
        aliases:[ 
            ["ready", "dojo/domReady"],
            ["registry","dijit/registry"],
            ["dialog","dijit/Dialog"], 
            ["parser","dojo/parser"] 
        ], 
        ackages: [{
            name: "js", 
            location: "/js"
        }]，
        locale: location.search.match(/locale=([\w\-]+)/) ? RegExp.$1 : "en-us"
    };
</script>
```

##### ContentPanes

> 所有小部件的基石，其他任何小部件都可以用他作为内容或者子小部件的载体

###### 案例：

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <!-- 加载页面的解释， -->
    <script>dojoConfig = { parseOnLoad: true }</script>
    <!-- CSS包的引如方式1 -->
    <link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css" />
    <!-- CSS包的引如方式2 -->
    <style type="text/css">
        @import "dojo/dojox/layout/resources/FloatingPane.css";
        @import "dojo/dojox/layout/resources/ResizeHandle.css";
    </style>
    <!-- 引入dojo.js -->
    <script src="dojo/dojo/dojo.js"></script>

    <script>
        // require加载dojo/ready，dijit/layout/ContentPane组件，并执行函数
        require(["dojo/ready", "dijit/layout/ContentPane"], function (ready, ContentPane) {
            ready(function () {
                new ContentPane({
                    content: "<p>I am initial content</p>",
                    style: "height:125px"
                }).placeAt("targetID2");
            });
        });
    </script>
</head>
<body class="claro">
    <div id="targetID2"  style="background-color:red">
        A contentPane will appear here:
    </div>
</body>
</html>
```

##### FloatingPanes面板

> 浮动面板，可以模拟Windows窗口的效果在页面上随意拖动

###### 案例

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <script>dojoConfig = { parseOnLoad: true }</script>
    <link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css" />
    <style type="text/css">
        @import "dojo/dojox/layout/resources/FloatingPane.css";
        @import "dojo/dojox/layout/resources/ResizeHandle.css";
    </style>
    <script src="dojo/dojo/dojo.js"></script>

    <script>dijit.form.Button
        // 引入dojox.layout.FloatingPane与dijit.form.Button
        dojo.require("dojox.layout.FloatingPane");
        dojo.require("dijit.form.Button");
        var pFloatingPane;
        dojo.ready(function () {
            pFloatingPane = new dojox.layout.FloatingPane({
                title: "A floating pane",
                resizable: true, able: true,
                style: "position:absolute;top:0;left:0;width:100px;height:100px;visibility:hidden;",
                id: "pFloatingPane"
            }, dojo.byId("pFloatingPane"));

            pFloatingPane.startup();
        });
    </script>
</head>
<body class="claro">
    <div id="pFloatingPane">This is the content of the pane!</div>
    <div data-dojo-type="dijit.form.Button" data-dojo-props="label:'Show me', onClick:function(){pFloatingPane.show();}"></div>
    <br /><br /><br /><br />
</body>
</html>
```



### Dojo堆叠容器和BorderContainer

##### BorderContainer

> 是一个布局容器，主要分为5个区域，上下左右中
>
> 每个BorderContainer都有重量不同的方式布局
>
> > 通过design属性来控制
> >
> > > headline
> > >
> > > sidebar

###### 案例

```html
<!DOCTYPE html>
<html >
<head>
    <!-- 引入dojo样式 -->
	<link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css">
	<style type="text/css">
html, body {
    width: 100%;
    height: 100%;
    margin: 0;
    overflow:hidden;
}

#borderContainerTwo {
    width: 100%;
    height: 100%;
}
	</style>
	<!-- 写入dojoConfig -->
    <script>dojoConfig = { parseOnLoad: true }</script>
	<script src='dojo/dojo/dojo.js'></script>
	
	<script>
        // 引入需要的组件
	    //require(["dojo/parser", "dijit/layout/ContentPane", "dijit/layout/BorderContainer", "dijit/layout/TabContainer", "dijit/layout/AccordionContainer", "dijit/layout/AccordionPane"]);
	</script>
</head>
<body class="claro">
    <div data-dojo-type="dijit/layout/BorderContainer" data-dojo-props="gutters:true, liveSplitters:false" id="borderContainerTwo">
    <div data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'top', splitter:false">
      头部
    </div>
    <!-- 折叠的url组件 -->
    <div data-dojo-type="dijit/layout/AccordionContainer" data-dojo-props="minSize:20, region:'right', splitter:true" style="width: 300px;" id="leftAccordion">
        <div data-dojo-type="dijit/layout/AccordionPane" title="项目一">
        </div>
        <div data-dojo-type="dijit/layout/AccordionPane" title="项目二">
        </div>
        <div data-dojo-type="dijit/layout/AccordionPane" title="项目三" selected="true">
        </div>
        <div data-dojo-type="dijit/layout/AccordionPane" title="项目四">
        </div>
    </div><!-- end AccordionContainer -->
    <div data-dojo-type="dijit/layout/TabContainer" data-dojo-props="region:'center', tabStrip:true">
        <div data-dojo-type="dijit/layout/ContentPane" title="My first tab" selected="true">
       查询
        </div>
        <div data-dojo-type="dijit/layout/ContentPane" title="My second tab">
      图表
        </div>
        <div data-dojo-type="dijit/layout/ContentPane" data-dojo-props="closable:true" title="My last tab">
      数据
        </div>
    </div>
   <div data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'bottom'" style="background-color:red">底部</div>
</div><!-- end BorderContainer -->
</body>
</html>
```





### dojo小部件

> 自定义小部件
>
> 内部小部件

##### _Widget接口

> 该接口用于定义一些设置方法
>
> 通过这些方法可以利用小部件管理器等类来统一管理小部件

##### _BaseWidget基类

> 小部件基类

##### 行为型小部件

> 这类小部件内部不创建DOM元素
>
> 直接使用DOM树创建自己的DOM树
>
> 

###### 案例

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <title></title>
    <link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css"/>
    <script src='dojo/dojo/dojo.js'></script>
    <script>
        require([
     "dojo/_base/declare", "dojo/parser", "dojo/ready",
     "dijit/_WidgetBase",
        ], function (declare, parser, ready, _WidgetBase) {

            declare("MyFirstBehavioralWidget", [_WidgetBase], {
                // put methods, attributes, etc. here
            });
            ready(function () {
                // Call the parser manually so it runs after our widget is defined, and page has finished loading
                parser.parse();
            });
        });
    </script>
</head>
<body>
    <span data-dojo-type="MyFirstBehavioralWidget">最简单的行为性小部件</span>
</body>
</html>

```

##### 非行为型小布件

> 最低要求式创建一个DOM树，小部件的DOM树保存到domNode属性中

###### 案例

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <title></title>
    <link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css"/>
    <script src='dojo/dojo/dojo.js'></script>
    <script>
        // _WidgetBase为部件的基类
        require([
    "dojo/_base/declare", "dojo/dom-construct", "dojo/parser", "dojo/ready",
    "dijit/_WidgetBase",
            // domConstruct:用于创建dom元素
        ], function (declare, domConstruct, parser, ready, _WidgetBase) {
            // 创建小部件，继承了_WidgeBas基类
            declare("Counter", [_WidgetBase], {
                // counter
                _i: 0,

                // 创建小部件
                buildRendering: function () {
                  // 创建一个button按钮，其内容为this._i
                    this.domNode = domConstruct.create("button", { innerHTML: this._i });
                },
				// 创建绑定方法
                postCreate: function () {
                    // 当domeNode被单击的时候，执行increment方法
                    this.connect(this.domNode, "onclick", "increment");
                },

                increment: function () {
                    this.domNode.innerHTML = ++this._i;
                }
            });

            ready(function () {
                // Call the parser manually so it runs after our widget is defined, and page has finished loading
                parser.parse();
            });
        });

    </script>
</head>
<body>
    <span data-dojo-type="Counter">最简单的非行为性小部件</span>
</body>
</html>

```

##### 模块化小部件

> 使用`_TemplatedMixin`实现小部件定义
>
> 与小部件行为的实现分离开，实现模块化小部件

###### 案例

```html
<!DOCTYPE html>
<html >
<head>

	<link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css">
	
	<script>dojoConfig = { parseOnLoad: false }</script>
	<script src='dojo/dojo/dojo.js'></script>
	
	<script>
	    require([
            "dojo/_base/declare", "dojo/parser",
            "dijit/_WidgetBase", "dijit/_TemplatedMixin", "dojo/domReady!"
	    ], function (declare, parser, _WidgetBase, _TemplatedMixin) {
            // 创建了MyButton部件，继承_WidgetBase基类，以及_TemplatedMixin模板
	        var MyButton = declare("MyButton", [_WidgetBase, _TemplatedMixin], {
	            templateString:
                    "<button data-dojo-attach-point='containerNode' data-dojo-attach-event='onclick: onClick'></button>",
	            onClick: function (evt) {
	                alert("Awesome!!");
	            }
	        });
	        parser.parse();
	    });
	</script>
</head>
<body class="claro">
    <button data-dojo-type="MyButton">press me</button>
</body>
</html>
```



### dojo可移动小部件

> ndojo/dnd/Moveable 移动组件
>
> ndojo/dom 文档组件
>
> ndojo/dom-style 样式组件

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
<meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
    <title></title>
    <link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css">
	<style type="text/css">
#dndOne {
  width: 100px;
  height: 100px;
  border: 1px solid #000;
  background: red;
}

#dndArea {
  height: 200px;
  border: 1px solid #000;
}
	    .auto-style1 {
            width: 140px;
        }
	    #close {
            height: 13px;
        }
	</style>
    <script>dojoConfig = { async: true, parseOnLoad: false }</script>
	<script src='dojo/dojo/dojo.js'></script>
    <script>
        // 引入移动组件包dojo/dnd/Moveable，文档管理：dojo/dom，绑定："dojo/on
        // 文档样式：dojo/dom-style，运行模式：dojo/domReady!
        require(["dojo/dnd/Moveable", "dojo/dom", "dojo/on", "dojo/dom-style", "dojo/domReady!"], function (Moveable, dom, on, domStyle) {
            // 绑定doIt
            on(dom.byId("doIt"), "click", function () {
                var dnd = new Moveable(dom.byId("dndOne"));
            });
            on(dom.byId("close"), "click", function () {
                domStyle.set(dom.byId("dndOne"), "display", "none");
            });
            on(dom.byId("btxs"), "click", function () {
                domStyle.set(dom.byId("dndOne"), "display", "block");
            });
        })
    </script>
</head>
<body>
    <body class="claro">
    <div id="dndArea">
  <div id="dndOne" style="width:200px; height:200px">   	
		<table style="background-color:black; align-content:flex-start;width:100%; height:30px">
			<tr style="vertical-align: top; color:white">
				<td class="auto-style1" >标题</td>
				<td><button id="close" style="height:30px" type="button">关闭</button></td>
			</tr>
		</table>
		<div   style="height:150px" data-dojo-attach-point="containerNode">
		</div>
  </div>
</div>
<p><button id="doIt" type="button">拖动</button></p>
    <p><button id="btxs" type="button">显示</button></p>
</body>

</html>

```





### dojo模型与代码分离式小部件

##### 案例

###### dojo/test/031001.html

````html
<div>
    姓名：<input id="Text1" type="text" />
    <button data-dojo-attach-point='containerNode' data-dojo-attach-event='onclick: onClick'>确定</button>
</div>
````

###### dojo/test/js0310.js

```js
// 调用新建的模型 : dojo/text!./031001.html
define(["dojo/_base/declare", "dijit/_WidgetBase", "dijit/_TemplatedMixin", "dojo/text!./031001.html", "dojo/dom"], function (declare, _WidgetBase, _TemplatedMixin, temstring, dom) {
    return declare([_WidgetBase, _TemplatedMixin], {
        templateString:
           temstring,
        onClick: function (evt) {
            var t = dom.byId("Text1");
            alert(t.value);
        }
    });
});
```

###### 0310.html

```html
<!DOCTYPE html>
<html>
<head>

    <link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css">

    <script>dojoConfig = { parseOnLoad: false }</script>
    <script src='dojo/dojo/dojo.js'></script>

    <script>
        require([
            "test/js0310", "dojo/parser", "dojo/_base/window",
            "dojo/domReady!"
        ], function (testst, parser, win) {
            parser.parse();
            var mm = new testst();
            mm.placeAt(win.body());
            // mm.onClick();
        });
    </script>
</head>
<body class="claro">

</body>
</html>
```



### dojo动画控制部件

> 动画类库dojo/_base/fx

##### dojo/_base/fx主要方法

> FadeIn()：淡入
>
> > 格式
> >
> > > FadeIn(dom节点对象)
>
> FadeOut()：淡出
>
> > 格式
> >
> > > FadeOut(dom节点对象)
>
> AnimateProperty()
>
> > 属性
> >
> > > Node: dom节点的ID
> > >
> > > Properties属性
> > >
> > > Duration: 动画时间
> > >
> > > Rate：时间类型
> > >
> > > Easing: 指定动画缓和曲线函数
> > >
> > > 事件处理函数：如OnEnd
>
> > 主要方法
> >
> > > Play()动画播放
> > >
> > > Pause()动画暂停
> > >
> > > Status()返回动画当前状态
> > >
> > > nStop(gotoEnd)停止播放
> > >
> > > > gotoEnd为true时候，当前位置位置为1%

###### 案例

```html
<!DOCTYPE html>
<html >
<head>

	<link rel="stylesheet" href="dojo/dijit/themes/claro/claro.css">
	<style type="text/css">
#statusCode {
    padding: 5px;
    border: 1px solid #000;
    background: red;
    text-align: center;
    width: 100px;
}
	</style>
	<script>dojoConfig = { parseOnLoad: true }</script>
	<script src='dojo/dojo/dojo.js'></script>
	
	<script>
	    require(["dojo/dom", "dojo/_base/fx"], function (dom, fx) {
	        statusOk = function () {
	            fx.animateProperty({
	                node: dom.byId("statusCode"), duration: 500,
	                properties: {
	                    backgroundColor: { start: "red", end: "green" },
	                    color: { start: "black", end: "white" },
	                },
	                onEnd: function () {
	                    dom.byId("statusCode").innerHTML = "Granted";
	                }
	            }).play();
	        };
	    });
	</script>
</head>
<body class="claro">
    <p><button onclick="statusOk();">确定</button></p>
<div id="statusCode">颜色变化</div>
</body>
</html>
```

