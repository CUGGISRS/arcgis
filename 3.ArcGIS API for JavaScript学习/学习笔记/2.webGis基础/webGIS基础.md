## webGIS基础

------

### Web开发技术的6个阶段

> 1.静态内容阶段
>
> 2.CGI程序阶段
>
> 3.脚本语言阶段
>
> 4.瘦客户阶段
>
> 5.RIA应用阶段
>
> 6.HTML5

##### 静态内容阶段

> Web由大量HTML文档组成

##### CGI程序阶段

> 向客户端提供了一些动态的变化
>
> 通过CGI协议完成

##### 脚本语言阶段

> 服务器出现了ASP/PHP/JSP等Session的脚本技术
>
> 浏览器出现了java Applet/javascript等技术

##### 瘦客户阶段

> 服务器出现了独立于Web的应用服务器
>
> 同时出现Web MVC开发模式

##### RIA应用阶段

> 出现了多种RIA技术
>
> > 如
> >
> > > DHTML+AJAX+flex+Silverlight

##### HTML5

> HTML5+各种javascript框架



#### WebGIS技术定义

> 是基于Web的GIS技术

> WebGIS 2.0 主要是Web服务，REST与Ajax等技术
>
> > `ArcGIS API For JavaScrip`t正是一套构建WebGIS2.0 应用的API



### OWS服务体系

#### 地理数据服务(DateService)

> 提供对空间数据的服务
>
> 主要有：WFS，WCS
>
> > 地理数据服务返回的结果通常是带有空间参照系的数据

##### WFS(Web FeatureService)

> 矢量数据服务

##### WCS(WebCoverage Service)

> 栅格数据服务



#### 地理描述服务(PortrayalService)

> 提供空间数据的描述
>
> 主要有：WMS，SDL

> **WMS(Web MapService)**地图服务
>
> 其中地图可以由多个图层结合起来
>
> 每个地图用**SDL(Style Layer Descriptor)**来对地图进行描述
>
> > 地图服务的返回结果通常是矢量图形或栅格图形



#### 过程处理服务（ProcessingService）

> 提供数据的查找，索引等服务
>
> 主要有
>
> > > **Geocoder(地学编码范围)**、
> > >
> > > **Gazetteer(地名索引服务)**、
> > >
> > > **Coordinate Transfer Service(坐标转化服务)**等



#### 发布注册服务(Registry)

> 提供各种服务的注册服务，以便于服务的发现



#### 客户端应用(ClientApplication)

> 如：
>
> > 地图的显示、地图浏览以及一些增值服务



### ArcGIS Server 

#### 服务类型

###### 地图服务

> 切片地图
>
> > 为快速显示地图，预先将地图切成一定规格的图片
>
> 动态地图
>
> > 根据每个请求动态的绘制地图
>
> KML
>
> > 生成Google Earth等支持的KML格式数据
>
> OGC返回遵循OGC相关标准的地图数据（包含：WCS/WFS/ WMS/ WMTS）



### ArcGIS Api for JavaScript

##### 包含内容

###### 地图显示

###### 地图绘制

###### 地图任务

###### 使用Dojo与其他类库进行扩展



##### 常用类库

###### esri/plugins: 插件包

###### esri/process:进程包

###### esri/renderers:渲染包

###### esri/styles:样式包

###### esri/ symbols:符号包

###### esri/tasks:任务包

###### esri/toolbars:工具包



##### arcgis api for JavaScript与dojo关系

> arcgis api for JavaScript构建在dojo上，从而充分利用dojo来屏蔽浏览器之间的差异
