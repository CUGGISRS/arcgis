## arcgis开发时遇到的问题

### 某一图层需要显示某一区域

> 使用Extent中的xmin，xmax，ymin，ymax进行修改



### Buffer无法缓冲

> `geoodesicBuffer(geometry，distance，util，unionResult)`
>
> > 投影为WGS84（wkid）与Web Mercator使用
>
> `Buffer()`
>
> > 其他投影使用



### 线线相交无法获取点

> intersect(geometry，intersector)
>
> > 面和面能返回面，面和线能返回线
> >
> > 线和线无法返回点
>
> 