## API学习

-----

### 配置离线API环境

> 官方下载网址
>
> > `http://support.esrichina-bj.cn/2011/0223/960.html`
> >
> > > library：为包
> > >
> > > SDK：为文档
>
> 环境搭建步骤
>
> > 1.在工程下创建`arcgisjs`文件夹
> >
> > 2.解压`arcgis_js_v39_api.zip`
> >
> > 3.拷贝`arcgis_js_v39_api\library\3.9\3.9compact`下的内容到`arcgisjs`文件夹下
>
> 文件修改
>
> > 修改：`arcgis_js\init.js`和`arcgis_js\ js\dojo\dojo\dojo.js `两个文件，替换
> >
> > > `[HOSTNAME_AND_PATH_TO_JSAPI]`为
> > >
> > > > `IP:web服务器端口/工程名/创建的文件夹arcgis_js`
> > > >
> > > > > 例如
> > > > >
> > > > > > `localhost:8080/ArcgisForJs/arcgis_js/`

##### 地图入门代码

###### 案例

```html
<!DOCTYPE HTML PUBLIC "-//W3C//DTD HTML 4.01//EN" "http://www.w3.org/TR/html4/strict.dtd">
<html>
  <head>
     <meta http-equiv="Content-Type" content="text/html; charset=utf-8"/>
     <title>My fisrt ArcGis Map</title>
     <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/dojo/dijit/themes/tundra/tundra.css"/>
   <script type="text/javascript" src="http://localhost:4528/arcgisjs/init.js"></script>
     <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/esri/css/esri.css" />
     <script type="text/javascript">
         dojo.require("esri.map");
         var myMap;
         function init() {
             myMap = new esri.Map("arcgisDiv");
             var myTiledMapServiceLayer = new esri.layers.ArcGISTiledMapServiceLayer("http://cache1.arcgisonline.cn/arcgis/rest/services/ChinaOnlineCommunity/MapServer");
             myMap.addLayer(myTiledMapServiceLayer);
             myMap.setZoom(4);
         }
         dojo.addOnLoad(init);
     </script>
  </head>
  <body class="tundra">
     <div id="arcgisDiv" style="width:900px; height:600px; border:1px solid #000;"></div>
  </body>
</html>
```



### VS智能提示配置

> `narcgis_js_v39_sdk\arcgis_js_api\sdk\jshelp\jsapi_vsdoc12_v38.js`
>
> > 在API目录中找到提示包
>
> 将该文件拷贝到项目的`arcgisjs`目录下
>
> > 加入代码
> >
> > > <script type="text/javascript" src="arcgisjs/jsapi_vsdoc12_v38.js"></script>



### ArcGIS重要API

#### 地图类Map

> Map是承载图层的容器 
>
> 主要用于呈现地图服务、影像服务，此外还可以展示 WMS 服务等
>
> > 一个图层只有被添加到 Map中，才能被显示出来
> >
> > > 例如
> > >
> > > > `var myMap = new esri.Map("Div");`

#### 图层类型

> 图层是承载服务的载体（GraphicsLayer 除外）
>
> ArcGIS for Server 将GIS资源作为服务发布出来
>
> > 要想在浏览器看到这些服务，就必须将这些服务和图层关联起来
> >
> > 不同的服务对应不同的图层类型

##### 图层和服务的对应关系

| 图层                         | 服务                                                 |
| ---------------------------- | ---------------------------------------------------- |
| ArcGISDynamicMapServiceLayer | ArcGIS for Server 发布的 2D 动态地图服务             |
| ArcGISTiledMapServiceLayer   | ArcGIS for Server 发布的 2D 缓存地图服务             |
| ArcGISImageServiceLayer      | ArcGIS for Server 发布的影像地图服务                 |
| GraphicsLayer                | 客户端图层不对应 ArcGIS for Server 发布的服务        |
| FeatureLayer                 | ArcGIS for Server 发布的要素服务或者地图服务中的图层 |
| WMSLayer                     | 调用 OGC（Open Geospatial Consortium）矢量地图服务   |
| WMTSLayer                    | OGC（Open Geospatial Consortium）地图切片服务        |
| KMLLayer                     | Keyhole Markup Language 描述和保存地理信息文件       |
| VETiledLayer                 | 微软的 Bing 地图服务                                 |
| GeoRssLayer                  | 支持 GeoRss 服务                                     |

##### 图层和图层的关系

> 地图是有多个图层组成的，因此关系包含与被包含的关系（自己理解）

###### 案例

```js
// 绑定图层容器，定义一个地图
var myMap = new esri.Map("arcgisDiv");
// 定义一个图层
var myTiledMapServiceLayer = new esri.layers.ArcGISTiledMapServiceLayer("http://cache1.arcgisonline.cn/arcgis/rest/services/ChinaOnlineCommunity/MapServer");
// 将图层绑定到容器中
myMap.addLayer(myTiledMapServiceLayer);

```



#### Geometry

> 几何对象用于表示对象的显示形式

> 在 ArcGIS API for JavaScript 中 Geometry 大体上可以分为下面几类
>
> > 点、多点、线、矩形、多边和 ScreenPoint
> >
> > > ScreenPoint是以像素的方式表示的点
> >
> > > 而点、多点、线、矩形、多边形都是继承 Geometry

##### Geometry类型名

###### Geometry

> 抽象类，定义几何体的图形

###### MapPoint

> 点对象

###### MultiPoint

> 多点对象

###### Polyline

> 多义线对象，由路径（Path）组合而成

###### Envelope

> 矩形对象，长宽方向分别平行于 X、Y 轴

###### Polygon

> 多边形对象，由环（Ring）组合而成

###### ScreenPoint

> 用像素来表示点的 X,Y 坐标，相对于屏幕的左上角。 



#### Symbol

> 定义了如何在GraphicLayer上显示点，线，面和文本
>
> 符号定义了几何对象的所有非地理特征方面的外观
>
> > 颜色，边框线宽度，透明度等等
>
> ArcGIS API for JavaScript 包含了很多符号类，每个类都允许你使用唯一的方式制定一种符号
>
> > 每种符号都特定于一种类型
> >
> > > 点，线，面，文本

##### 集合类型和对应的符号

> 点 SimpleMarkerSymbol, PictureMarkerSymbol 
>
> 线 SimpleLineSymbol, CartographicLineSymbol 
>
> 面 SimpleFillSymbol, PictureFillSymbol 
>
> 文本 TextSymbol, Font 

#### Graphic

> Geometry
>
> > 定义了对象的形状，
>
> Symbol
>
> > 定义了图形是如何显示的
>
> Graphic 
>
> > 可以包含一些属性信息，并且在Javascript中还可以使用 infoTemplate
> >
> > > 一个InfoTemplate 包含标题和内容模板字符串，该内容模板字符串用于将Graphic 的属性转换成HTML 的表达式
> >
> > 定义如何对属性信息进行显示，最终的Graphic 则是被添加到GraphicsLayer 中
> >
> > GraphicsLayer 允许对Graphics 进行事件监听，对于Graphic 的描述可以用一个数学表达式来表示：
>
> > Graphic=Geometry+Attribute+Symbol+infoTemplate 
> >
> > > 形状，属性，图形，信息

#### Reader(专题渲染)

> 渲染器定义了一种或多种符号以应用于一个GraphicsLayer
>
> 每个Graphic 的符号所使用的符号取决于该 Graphic 的属性值
>
> 渲染器指定了属性值与符号之间的对应关系

#### FeatureSet

> FeauteSet 是要素类的轻量级表示，相当于地理数据库中的一个要素类，是Feature（要素）的集合
>
> FeatureSet中的每个 Feature可能包含 Geometry、属性、符号、和一个InfoTemplate
>
> 如果FeatureSet不包含 Geometry,只包含属性，那么FeatureSet可以看作一个表
>
> 其中每个Feature 是一个行对象。FeatureSet是我们在利用 ArcGISAPI for Javacript 和 ArcGIS for Server 进行据通讯的一个非常重要的对象
>
> 当使用查询，地理处理和路径分析的时候，FeatureSet常常作为这些分析的功能的输入和输出参数



### 地图操作

#### Map参数详解

> `esri.Map(divId,options)`
>
> > 构造方法在创建一个 map对象**必须传入一个 div元素作为其容器**
> >
> > 此外这个构造方法还包括一系列**可选的参数**用来描述地图的相关行为
>
> > Map可选参数
> >
> > > Extent
> > >
> > > > 设置投影范围
> > > >
> > > > 一旦这个选项的投影被设置，那么所有的图层都在定义的投影中绘制
> > >
> > > Logo
> > >
> > > > 是否显示esri的 logo
> > >
> > > wrapAround180
> > >
> > > > 是否连续移动地图
> > > >
> > > > 即通过日期变更线，好似对地图进行横向旋转360 度
> > >
> > > lods
> > >
> > > > 设置地图的初始比例级别
> > >
> > > maxScale
> > >
> > > > 设置地图的最大可视比例尺
> > >
> > > sliderStyle
> > >
> > > > 设置 slider的样式
> > > >
> > > > 值为 large或者 small

#### Map主要方法

##### toScreen/toMap

> 地图坐标屏幕的转换

##### setScale

> 设置地图到指定的比例尺 

##### setZoom

> 放缩到指定的层级 

##### setLevel

> 放缩到指定的层级

##### setExtend

> 设置地图显示范围，常用于进行地图的平移操作 

##### disablePan

> 禁止使用鼠标平移地图 

##### removeAllLayers

> 移除所有图层

##### addLayer

> 添加图层 

##### getBasemap

> 获取底图 

##### getLayer

> 根据 id 获取图层 

##### getLayersVisibleAtScaleRange

> 获取某一比例尺下的可见图层（图层数组） 

##### getScale

> 获取当前的比例尺 

##### hidePanArrows

> 隐藏移动时候的鼠标箭头 

##### hideZoomSlider 

> 隐藏放大滑块 

##### panRight 

> 向右平移 

##### panUp 

> 向北平移 

##### removeAllLayers 

> 移除所有图层 

##### removeLayer 

> 移除指定图层 

##### reorderLayer 

> 改变图层的顺序 

##### reposition 

> 复位地图，该方法在地图的 DIV 被复位的时候要用到 

##### setTimeExtent

> 设置地图的时间范围

##### setTimeSlider 

> 设置和地图关联的时间滑块 

##### setZoom 

> 设置放大级别 

##### showPanArrows 

> 显示平移箭头 

##### showZoomSlider 

> 显示放大滑块



#### Map主要属性

##### autoResize 

> 如果浏览器窗口或 ContentPane 填充的地图控件的小部件的大小调整了，地图是否自动调整大小。 

##### attribution 

> 地图属性 

##### fadeOnZoom 

> 在地图进行缩放时，是否启用淡入淡出的效果 

##### extent 

> 地图外包矩形的范围，即四个角点坐标范围 

##### force3DTransforms 

> 是否启用 CSS3 转换 

##### infoWindow 

> 在地图上显示消息框 

##### isClickRecenter 

> 按住 Shift 键，在地图上单击鼠标左键，是否将该点设为地图中心

##### isDoubleClickZoom 

> 双击鼠标左键，是否进行放大地图操作 

##### isPan 

> 设置地图是否可以用鼠标移动 

##### spatialReference 

> 获取地图的空间参考信息 

##### isKeyboardNavigation 

> 是否用键盘上的“+”和“-”导航地图  

##### isRubberBandZoom 

> 是否启用橡皮筋缩放模式 

##### isScrollWheelZoom 

> 是否允许滚轮进行缩放操作 

##### isShiftDoubleClickZoom 

> 按住 Shift 键，在地图上双击鼠标左键，是否将该点设为地图中心,并同时进行缩放操作 

##### geographicExtent 

> 地图的地理坐标范围(只支持 Web 墨卡托) 

##### layerIds 

> 地图已加载的图层 ID 列表 

##### loaded 

> 地图控件是否已加载完成 

##### graphics 

> 获取地图的 GraphicsLayer 

##### position 

> 地图左上角坐标 

root 

> 容纳图层、消息框等的容器的 DOM 节点 

showAttribution 

> 是否允许显示地图属性 

snappingManager 

> 捕捉管理器 

isZoomSlider 

> 设置或者获取地图的放大滑块状态（true 和 false） 

layerIds 

> 获取地图的图层的 ID(数组) 

navigationMode 

> 设置或者获取地图的导航模式 

timeExtent 

> 地图的时间范围 



#### Map主要事件

##### onExtentChange 

> 地图范围改变事件。 

##### onBasemapChange 

> 地图的底图发生变化 

##### onLoad 

> 当第一个图层或者底图被添加到 Map 中的时候发生 

##### onClick 

> 在地图上发生单击的时候发生 

##### onLayerAdd 

> 当图层添加的时候发生 

##### onLayersAddResult 

> 当所有图层都添加结束后发生，使用 map.addLayers 方法之后

##### onLayersRemoved 

> 当所有图层都移除后发生 

##### onLoad 

> 当第一个图层或者底图加载成功后发生 

##### onMouseDown 

> 当鼠标在地图上单击的时候发生 

##### onMouseMove 

> 当鼠标在地图上移动的时候发生（在这个事件中经常用来获取 X,Y坐标）. 

##### onMouseOut 

> 当鼠标移出地图的时候发生 

#### 案例

````html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/dojo/dijit/themes/tundra/tundra.css" />
    <script type="text/javascript" src="http://localhost:4528/arcgisjs/init.js"></script>
    <script type="text/javascript" src="arcgisjs/jsapi_vsdoc12_v38.js"></script>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/esri/css/esri.css" />
    <script>
        import xxx form "xxxx"
        dojo.require("esri.map");
        var myMap;
        function init() {
            // myMap = new esri.Map("map1",{logo:true});
            myMap = new esri.Map("arcgisDiv");
            //  var myTiledMapServiceLayer = new esri.layers.ArcGISTiledMapServiceLayer("http://localhost:6080/arcgis/rest/services/sd/MapServer");
            var myTiledMapServiceLayer = new esri.layers.ArcGISTiledMapServiceLayer("http://cache1.arcgisonline.cn/arcgis/rest/services/ChinaOnlineCommunity/MapServer");
            myMap.addLayer(myTiledMapServiceLayer);
            myMap.setZoom(4);
            var div1 = dojo.byId("div1");
            var div2 = dojo.byId("div2");
            // 使用dojo.connect来绑定值，绑定事件
            dojo.connect(myMap, "onMouseMove", function (e) {
                var mp = e.mapPoint;
                var sp = e.screenPoint;
                div1.innerHTML = mp.x + "/" + sp.x;
                div2.innerHTML = mp.y + "/" + sp.y;
            });
            
        }

        dojo.addOnLoad(init)
    </script>
</head>
<body>
    <div id="arcgisDiv" style=" width:900px; height:500px"></div>
    <div id="div1"></div>
    <div id="div2"></div>
</body>
</html>

````



### 地图常用工具

##### 放大

###### 案例

```js
// 注册工具包
dojo.require("esri.toolbars.navigation");
// 定义工具包实体类，调用activate方法，添加参数
navToolbar.activate(esri.toolbars.Navigation.ZOOM_IN)
```

##### 缩小

> navToolbar.activate(esri.toolbars.Navigation.ZOOM_OUT);

##### 左移

> myMap.panLeft()

##### 右移

> myMap.panRight()

##### 上移

> myMap.panUp()

##### 下移

> myMap.panDown()

##### 全屏

> navToolbar.zoomToFullExtent()

##### 拖动

> navToolbar.activate(esri.toolbars.Navigation.PAN)

##### 鹰眼

###### 案例

```js
function OverviewMap() { 
 var over = 
 { 
 map: Map, 
 attachTo: "bottom-right", 
 color: "#D84E13", 
 expandFactor:2, 
 baseLayer:new esri.layers.ArcGISTiledMapServiceLayer(MapServer) 
 }; 
 
 var MapViewer = new esri.dijit.OverviewMap(over, dojo.byId("OverViewDiv")); 
 MapViewer.startup(); 
 
 } 
```

##### 案例

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/dojo/dijit/themes/tundra/tundra.css" />
    <script type="text/javascript" src="http://localhost:4528/arcgisjs/init.js"></script>
    <script type="text/javascript" src="arcgisjs/jsapi_vsdoc12_v38.js"></script>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/esri/css/esri.css" />
    <script>
        var map;
         // 注册工具包
        dojo.require("esri.toolbars.navigation");
        require([
            "esri/map", 
            "esri/dijit/OverviewMap",
            "dojo/parser",
            "dijit/layout/BorderContainer", 
            "dijit/layout/ContentPane", 
            "dojo/domReady!"
        ], function (
          Map, OverviewMap,parser
        ) {
            parser.parse();
            myMap = new esri.Map("map1", { logo: true });
            // 定义工具包实体类，调用activate方法，添加参数
            navToolbar = new esri.toolbars.Navigation(myMap);
            var layer1 = new esri.layers.ArcGISTiledMapServiceLayer("http://cache1.arcgisonline.cn/arcgis/rest/services/ChinaOnlineCommunity/MapServer");
            myMap.addLayer(layer1);
            myMap.setZoom(4);
            var div1 = dojo.byId("div1");
            var div2 = dojo.byId("div2");
            var button1 = dojo.byId("Button1");
            var button2 = dojo.byId("Button2");
            var button3 = dojo.byId("Button3");
            var button4 = dojo.byId("Button4");
            var button5 = dojo.byId("Button5");
            var button6 = dojo.byId("Button6");
            var button7 = dojo.byId("Button7");
            var button8 = dojo.byId("Button8");
            var button9 = dojo.byId("Button9");
            dojo.connect(myMap, "onMouseMove", function (e) {
                var mp = e.mapPoint;
                var sp = e.screenPoint;
                div1.innerHTML = mp.x + "/" + sp.x;
                div2.innerHTML = mp.y + "/" + sp.y;
            });
            dojo.connect(button1, "click", function (evt) {
                navToolbar.activate(esri.toolbars.Navigation.ZOOM_IN);
            });
            dojo.connect(button2, "click", function (evt) {
                navToolbar.activate(esri.toolbars.Navigation.ZOOM_OUT);
            });
            dojo.connect(button3, "click", function (evt) {
                myMap.panUp();
            });
            dojo.connect(button4, "click", function (evt) {
                myMap.panDown();
            });
            dojo.connect(button5, "click", function (evt) {
                myMap.panLeft();
            });
            dojo.connect(button6, "click", function (evt) {
                myMap.panRight();
            });
            dojo.connect(button7, "click", function (evt) {
                navToolbar.zoomToFullExtent();
            });
            dojo.connect(button8, "click", function (evt) {
                navToolbar.activate(esri.toolbars.Navigation.PAN);
            });

            dojo.connect(button9, "click", function (evt) {

                var overviewMapDijit = new OverviewMap({
                    map: myMap,
                    visible: true
                });
                overviewMapDijit.startup();
            });
        });
    </script>
</head>
<body>

     <div id="map1" 
           data-dojo-type="dijit/layout/ContentPane" 
           data-dojo-props="region:'center'" 
           style="padding:0">
      </div>
    <div id="div1"></div>
    <div id="div2">
        &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

    </div>
    <div>
        <table style="width: 100%;">
            <tr>
                <td>
                    <input id="Button1" type="button" value="放大" /><input id="Button2" type="button" value="缩小" /><input id="Button3" type="button" value="上移" /><input id="Button4" type="button" value="下移" /><input id="Button5" type="button" value="左移" /><input id="Button6" type="button" value="右移" /><input id="Button7" type="button" value="全屏" /><input id="Button8" type="button" value="拖动" /><input id="Button9" type="button" value="鹰眼" /></td>
                <td>&nbsp;</td>
                <td>&nbsp;</td>
            </tr>
        </table>
    </div>
</body>
</html>
```



### 图层控制

> 图层控制需要使用动态图层

###### 案例

> 控制图层是否显示
>
> > 扩展
> >
> > > dojo树,当有多层图层时可以使用，进行控制

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <!-- 引入包 -->
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/dojo/dijit/themes/tundra/tundra.css" />
    <script type="text/javascript" src="http://localhost:4528/arcgisjs/init.js"></script>
    <script type="text/javascript" src="arcgisjs/jsapi_vsdoc12_v38.js"></script>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/esri/css/esri.css" />
    <script>
        // 注册map类
        dojo.require("esri.map");
        // 定义map对象和图层对象
        var myMap, layer1;
        // 初始化函数
        function init() {
            myMap = new esri.Map("map1", { logo: true });
            // 图层控制需要使用动态图层
            layer1 = new esri.layers.ArcGISDynamicMapServiceLayer("http://sampleserver6.arcgisonline.com/arcgis/rest/services/Census/MapServer");
            dojo.connect(layer1, "onLoad", function (layers) {
                var html = "";
                //获取图层集合对象
                var infos = layers.layerInfos;
                // alert(infos.length);
                // 遍历图层
                for (var i = 0; i < infos.length; i++) {
                    var info = infos[i];
                    html = html + "<div><input id='" + info.id + "' name='layerList' class='listCss' type='checkbox' value='checkbox' onclick='setLayerVisibility()' " + (info.defaultVisibility ? "checked" : "") + " />" + info.name + "</div>"
                }
                dojo.byId("toc").innerHTML = html;
            });
            myMap.addLayer(layer1);
            myMap.setZoom(4);

        }
        function setLayerVisibility() {
            //用dojo.query获取css为listCss的元素数组
            //用dojo.query获取css为listCss的元素数组
            var inputs = dojo.query(".listCss");
            visible = [];
            // 对checkbox数组进行变量把选中的id添加到visible
            // 选择图层则显示，不选择则不显示图层
            for (var i = 0; i < inputs.length; i++) {
                if (inputs[i].checked) {
                    visible.push(inputs[i].id);
                } else {
                    visible.push(-1);
                }
            }
            //设置可视图层
            layer1.setVisibleLayers(visible);
        }
        dojo.addOnLoad(init)
    </script>
</head>
<body>
    <table>
        <tr>
            <td style=" width:800px;height:400px">
                <div id="map1"
                    data-dojo-type="dijit/layout/ContentPane"
                    data-dojo-props="region:'center'"
                    style="padding: 0">
                </div>
                <div id="div1"></div>
                <div id="div2">
                    &nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;&nbsp;

                </div>
                
            </td>
            <td>
                <div id="toc"></div>
            </td>
        </tr>
    </table>

</body>
</html>

```



### 常用控件

##### Scalebar(比例尺)

> Scalebar用于在地图上或者一个指定的 HTML 节点中**显示地图的比例尺信息**

##### Bookmark(书签)

> 可以快速定位到地图中的某一位置
>
> 书签控件用于管理用户创建的地图书签提供新建书签、定位到书签和删除书签的功能

###### 案例

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/dojo/dijit/themes/tundra/tundra.css" />
    <script type="text/javascript" src="http://localhost:4528/arcgisjs/init.js"></script>
    <script type="text/javascript" src="arcgisjs/jsapi_vsdoc12_v38.js"></script>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/esri/css/esri.css" />
    <script>
        dojo.require("esri.map");
        // 引入Scalebar这个类
        dojo.require("esri.dijit.Scalebar");
        dojo.require("esri.dijit.Bookmarks");
        dojo.require("esri.dijit.BookmarkItem");
        var myMap,Books;
        function init() {
            myMap = new esri.Map("map1");
            var imageryPrime = new esri.layers.ArcGISTiledMapServiceLayer("http://server.arcgisonline.com/ArcGIS/rest/services/ESRI_Imagery_World_2D/MapServer");
            myMap.addLayer(imageryPrime);
            var portlandLandBase = new esri.layers.ArcGISDynamicMapServiceLayer("http://jh-53a435fbc0e8/ArcGIS/rest/services/USA/MapServer");
            //设置要显示的图层
            portlandLandBase.setVisibleLayers([2, 1, 0]);
            //设置图层透明度
            portlandLandBase.setOpacity(0.8);
            myMap.addLayer(portlandLandBase);
         
            var div1 = dojo.byId("div1");
            var div2 = dojo.byId("div2");
            dojo.connect(myMap, "onMouseMove", function (e) {
                var mp = e.mapPoint;
                var sp = e.screenPoint;
                div1.innerHTML = mp.x + "/" + sp.x;
                div2.innerHTML = mp.y + "/" + sp.y;
            });
            var scalebar = new esri.dijit.Scalebar({
                // 给属性绑定map
                map: myMap,
                // "dual" displays both miles and kilmometers
                // "english" is the default, which displays miles
                // use "metric" for kilometers
                scalebarUnit: "dual"
            }, dojo.byId("scalebardiv"));

              // 使用Bookmaeks申明一个Books对象
              Books = new esri.dijit.Bookmarks({
                map: myMap,
                editable: "true"
            }, dojo.byId("bookmarks"));

            dojo.connect(button1, "click", addBook);

        }
        function addBook() {

            var bookextent = myMap.extent;

            var bookmarkItem = new esri.dijit.BookmarkItem({
                "extent": bookextent,
                "name": "北京"
            });
            Books.addBookmark(bookmarkItem);
        }
        

        dojo.addOnLoad(init)
    </script>
</head>
<body><div id="scalebardiv"></div>
    <div id="map1" style=" width:900px; height:500px"></div>
    <div id="div1"></div>
    <div id="div2"></div>
    <table>
        <tr>
            <td><div id="bookmarks"></div></td>
            <td></td>
            <td><button id="button1">添加标签</button></td>
        </tr>
    </table>
</body>
</html>

```

##### InfoWindow

> InfoWindow 控件是一个带有小尾巴的窗口，小尾巴指向一个位置或感兴趣的要素，其本质上就是一个**HTML弹出框**
>
> InfoWindow 经常包括 Graphic 的属性信息。如果 Graphic 定义了InfoTemplate，则点击 Graphic 显示InfoTemplate 所定义的，每个地图仅有一个 InfoWindow，无构造函数。
>
> 构造方法：无，通过 Map.infoWindow 获取或设置

###### 案例1

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <!--The viewport meta tag is used to improve the presentation and behavior of the samples 
      on iOS devices-->
    <meta name="viewport" content="initial-scale=1, maximum-scale=1,user-scalable=no">
    <title>Formatter Function</title>

 <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/dojo/dijit/themes/tundra/tundra.css" />
    <script type="text/javascript" src="http://localhost:4528/arcgisjs/init.js"></script>
    <script type="text/javascript" src="arcgisjs/jsapi_vsdoc12_v38.js"></script>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/esri/css/esri.css" />
    <style>
      html, body { height: 100%; width: 100%; margin: 0; padding: 0; }
    </style>

    <script>
        // infotemplate formatting functions need to be in the global scope to work
        var map, compare, compare2;

        // 引用样式
        require([
          "esri/map",
          "esri/InfoTemplate",
          "esri/layers/FeatureLayer",
          "esri/renderers/SimpleRenderer",
          "esri/symbols/SimpleFillSymbol",
          "esri/symbols/SimpleLineSymbol",
          "dojo/dom",
          "dojo/number",
          "dojo/on",
          "dojo/parser",
          "esri/Color",
          "dijit/layout/BorderContainer",
          "dijit/layout/ContentPane",
          "dojox/layout/ExpandoPane",
          "dojo/domReady!"
        ],
          function (
            Map, InfoTemplate, FeatureLayer, SimpleRenderer, SimpleFillSymbol,
            SimpleLineSymbol, dom, number, on, parser, Color
        ) {

              parser.parse();
            
              map = new esri.Map("mapDiv");
              // 定义一个图层
              var imageryPrime = new esri.layers.ArcGISTiledMapServiceLayer("http://server.arcgisonline.com/ArcGIS/rest/services/ESRI_Imagery_World_2D/MapServer");
              // 将图层添加到地图上
              map.addLayer(imageryPrime);
              // 声明信息模板
              var infoTemplate = new InfoTemplate();
              infoTemplate.setTitle("Population in ${NAME}");
              infoTemplate.setContent("<b>2007 :D: </b>${POP2007:compare}<br/>" +
                                      "<b>2007 density: </b>${POP07_SQMI:compare}<br/><br/>" +
                                      "<b>2000: </b>${POP2000:NumberFormat}<br/>" +
                                      "<b>2000 density: </b>${POP00_SQMI:NumberFormat}");
              // 关联一个数据服务层
              var counties = new FeatureLayer("http://sampleserver1.arcgisonline.com/ArcGIS/rest/services/Demographics/ESRI_Census_USA/MapServer/3", {
                  // 设置模板类型
                  mode: FeatureLayer.MODE_SNAPSHOT,
                  // 模板
                  infoTemplate: infoTemplate,
                  // 要显示的字段
                  outFields: [
                    "NAME", "POP2000", "POP2007", "POP00_SQMI",
                    "POP07_SQMI"
                  ]
              });
              // 筛选出一个区域
              counties.setDefinitionExpression("STATE_NAME = 'Michigan'");

              //apply a renderer
              // 定义一个样式，给筛选出的区域赋予颜色
              var symbol = new SimpleFillSymbol(SimpleFillSymbol.STYLE_SOLID,
                new SimpleLineSymbol(SimpleLineSymbol.STYLE_SOLID,
                  new Color([255, 255, 255, 0.35]), 1),
                new Color([109, 146, 155, 0.35]));
              counties.setRenderer(new SimpleRenderer(symbol));

              map.addLayer(counties);
          });
    </script>
  </head>
  <body class="soria">
    <div data-dojo-type="dijit/layout/BorderContainer"
         data-dojo-props="design:'headline', gutters:true" 
         style="width: 100%; height: 100%; margin: 0;">

      <div id="mapDiv" data-dojo-type="dijit/layout/ContentPane" data-dojo-props="region:'center'"></div>
    </div>
  </body>

</html>
```

###### 案例2

> 单击查询的方式获取地图信息

```html
<!DOCTYPE html>
<html>
  <head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8">
    <!--The viewport meta tag is used to improve the presentation and behavior of the samples 
      on iOS devices-->
    <meta name="viewport" content="initial-scale=1, maximum-scale=1,user-scalable=no">
    <title>QueryTask with geometry, results as an InfoWindow</title>

    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/dojo/dijit/themes/tundra/tundra.css" />
    <script type="text/javascript" src="http://localhost:4528/arcgisjs/init.js"></script>
    <script type="text/javascript" src="arcgisjs/jsapi_vsdoc12_v38.js"></script>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/esri/css/esri.css" />
    <script>
        dojo.require("esri.map");
        // 引入查询类
        dojo.require("esri.tasks.query");

        var map, queryTask, query;
        var symbol, infoTemplate;

        function init() {
            //create map
            map = new esri.Map("mapDiv");

            var layer = new esri.layers.ArcGISDynamicMapServiceLayer("http://sampleserver1.arcgisonline.com/ArcGIS/rest/services/Specialty/ESRI_StateCityHighway_USA/MapServer");
            map.addLayer(layer);


            dojo.connect(map, "onClick", executeQueryTask);//给地图绑定单击事件

            // 查询信息
            queryTask = new esri.tasks.QueryTask("http://sampleserver1.arcgisonline.com/ArcGIS/rest/services/Specialty/ESRI_StateCityHighway_USA/MapServer/1");
            // 声明一个查询任务对象
            query = new esri.tasks.Query();
            // 定义查询任务返回的为一个空间集合对象
            query.returnGeometry = true;
            // 返回内容的字段信息
            query.outFields = ["STATE_NAME", "STATE_FIPS", "STATE_ABBR", "HYPERLINK", "AREA"];
            // 定义一个模板
            infoTemplate = new esri.InfoTemplate("${STATE_NAME}", "State Fips: ${STATE_FIPS}<br />Abbreviation: ${STATE_ABBR}<br />Area: ${AREA}");
            // 定义一个样式
            symbol = new esri.symbol.SimpleFillSymbol(esri.symbol.SimpleFillSymbol.STYLE_SOLID, new esri.symbol.SimpleLineSymbol(esri.symbol.SimpleLineSymbol.STYLE_DASHDOT, new dojo.Color([255, 0, 0]), 2), new dojo.Color([255, 255, 0, 0.5]));
        }

        function executeQueryTask(evt) {

            query.geometry = evt.mapPoint;//根据点击地图的坐标到地图数据中查询
            
            // 根据坐标点执行查询
            queryTask.execute(query, showResults);
        }

        // 查询的回调函数，遍历信息
        function showResults(featureSet) {
            map.graphics.clear();

            dojo.forEach(featureSet.features, function (feature) {
                var graphic = feature;
                graphic.setSymbol(symbol);

                graphic.setInfoTemplate(infoTemplate);

                map.graphics.add(graphic);

            });
        }
        dojo.ready(init);
    </script>
  </head>
  <body class="claro">
    Click on a State to get more info.  When State is highlighted, click State again to get infoWindow.
    <div id="mapDiv" style="width:600px; height:600px; border:1px solid #000;"></div>
  </body>
</html>
```

#### Navigation导航

> 构造方法：esri.toolbars.Navigation(map)
>
> 创建导航对象，传入一个 map对象作为参数，无可选参数
>
> > 前移，左移，上移，下移等操作

#### Draw绘图

> 在地图上进行绘图操作，主要是借助于 Toolbar 上的 Draw（绘图）工具，绘图工具支持几何对象的创建
>
> new Draw(map, options?)

###### 案例

```html
<!DOCTYPE html>
<html xmlns="http://www.w3.org/1999/xhtml">
<head>
    <meta http-equiv="Content-Type" content="text/html; charset=utf-8" />
    <title></title>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/dojo/dijit/themes/tundra/tundra.css" />
    <script type="text/javascript" src="http://localhost:4528/arcgisjs/init.js"></script>
    <script type="text/javascript" src="http://localhost:4528/arcgisjs/jsapi_vsdoc12_v38.js"></script>
    <link rel="stylesheet" type="text/css" href="http://localhost:4528/arcgisjs/js/esri/css/esri.css" />
    <script>
        dojo.require("esri.map");
        dojo.require("esri.toolbars.navigation");//引入导航组件
        dojo.require("esri.toolbars.draw");
        dojo.require("dijit.registry");
        var myMap;
        function init() {
            myMap = new esri.Map("map1", { logo: true });
            var layer1 = new esri.layers.ArcGISTiledMapServiceLayer("http://cache1.arcgisonline.cn/arcgis/rest/services/ChinaOnlineCommunity/MapServer");
            myMap.addLayer(layer1);
            myMap.setZoom(4);
            myMap.disableKeyboardNavigation();
            var div1 = dojo.byId("div1");
            var div2 = dojo.byId("div2");
            dojo.connect(myMap, "onMouseMove", function (e) {
                var mp = e.mapPoint;
                var sp = e.screenPoint;
                div1.innerHTML = mp.x + "/" + sp.x;
                div2.innerHTML = mp.y + "/" + sp.y;
            });
            var navToolbar = new esri.toolbars.Navigation(myMap);//导航对象
            var zoomprev = dojo.byId("zoomprev");
            var zoomnext = dojo.byId("zoomnext");
            dojo.connect(zoomprev, "click", function (ex) {

                navToolbar.zoomToPrevExtent();
            })
            dojo.connect(zoomnext, "click", function (ex) {

                navToolbar.zoomToNextExtent();
            })       
        }

        dojo.addOnLoad(init)
    </script>
</head>
<body>
    <div id="map1" style=" width:900px; height:500px"></div>
    <div id="div1"></div>
    <div id="div2"></div>
    <div id="div3"><button id="zoomprev">上一视图</button><button id="zoomnext">下一视图</button></div>
</body>
</html>

```



#### Legend图例

> Legend 控件用于动态显示全部或者部分图层的标签和符号信息。
>
> new Legend(params, srcNodeRef) 
>
> > params：绑定参数
> >
> > srcNodeRef：绑定div元素



### IdentifyTask

> IdentifyTask 是在某个地图服务中进行空间查询
>
> IdentifyTask 以IdentifyParameters 对象作为参数，能查询同一个地图服务的一个或者多个图层
>
> IdentifyTask 仅仅用于空间信息查询

##### IdentifyTask构造函数

> new IdentifyTask(url,options)
>
> > Url:服务地址
> >
> > options:可选参数
> >
> > > gdbVersion：指定要显示geodatabase版本

##### IdentifyTask 方法

> execute(identifyParameters, callback?, errback?)
>
> > identifyParameters
> >
> > > 指定的标准用来识别特性。空间信息化标准
> >
> > Callback 函数调用方法时完成，回调函数
> >
> > Errback：错误返回函数

