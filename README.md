## 给标记点添加弹框
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/d18db8cc58b541a793bf7c8ebc2d6ea9~tplv-k3u1fbpfcp-watermark.image?)

在大部分的业务场景中，我们都需要对地图上的标记点增加一个点击事件，以此来查看当前标记点的信息。   

### 首先添加一个 点图层
> 这一块和上一节的 `给地图增加标记点` 的代码有所不同，`给地图增加标记点` 我们是直接把点的数据添加到底图中去的，这次我们新建一个图层，把点数据添加到这个新的图层中去。

1. 引入 `esri/layers/GraphicsLayer`:
```diff
    "esri/map",
    "esri/geometry/Point",
    "esri/graphic",
    "dojo/_base/array",
+   "esri/layers/GraphicsLayer",
    "dojo/domReady!",
```

2. 新建一个图形层，并且给他一个id，名为 `"pointLayer"`，后期我们可以通过这个 `id` 来找到这个图形层。
```js
let pointLayer = new GraphicsLayer({
    id: "pointLayer",
});
```

3. 此时，我们把点的数据添加到这个新的图形层上：
```js
arrayUtils.forEach(point, function (point, index) {
    pointLayer.add(
      new Graphic(
        new Point(point),
        createSymbol(iconpath, fill_color)
      )
    );
});
```
这一块和 `给地图增加标记点` 中有所不同，我们使用 `diff` 把两者的代码做个比较：
```diff
arrayUtils.forEach(point, function (point) {
- var graphic = new Graphic(
+ pointLayer.add(
+   new Graphic(
        new Point(point),
        createSymbol(iconpath, fill_color)
+   );
+ );
- self.map.graphics.add(graphic);
});
```

4. 当我们把点添加到 `pointLayer` 的图层上是，页面上啥也没有，这是因为，我们还有没有把这个 `pointLayer` 添加到地图中去。
```js
// map.addLayer(图层, 索引)
self.map.addLayer(pointLayer, 1);
```
5. 完成上面的步骤，就可以在地图上看到熟悉的那两个点了。
![image.png](https://p1-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e6500c97fa904213a227f9c3a8a1cd16~tplv-k3u1fbpfcp-watermark.image?)

### 给图层中的标记点添加点击事件
```js
// 我们点击自己新加的图形层之后，不点击标记点不触发事件，只有点击了该图层上的标记点之后，才会触发这个点击事件。这一块我也不知道是为什么，只能做到一个实现的功能。
pointLayer.on("click", function (e) {
    console.log(e); // 这个 e 代表的是这个标记点
});
```
我们可以在这个点击事件中实现自己想要实现的功能。   

下面我们就来实现一个 `**点击标记点出现信息窗口**` 的示例吧。

### 点击标记点出现信息窗口

1. 引入 `esri/InfoTemplate`
```diff
    "esri/map",
    "esri/geometry/Point",
    "esri/graphic",
    "dojo/_base/array",
    "esri/layers/GraphicsLayer",
+   "esri/InfoTemplate",
    "dojo/domReady!",
```
2. 新建一个信息弹框并且添加弹框到当前图层的点击事件中
```js
// new InfoTemplate(弹框的名字，弹框里面的内容)
let infoTemplate =  new InfoTemplate("title", `content`);
pointLayer.setInfoTemplate(infoTemplate);
```
此时，当你点击标记点时，就会出现一个信息弹框了，如下图：
![image.png](https://p6-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/e0af33b8d42449909e9f7a296606ff97~tplv-k3u1fbpfcp-watermark.image?)

3. 我们还可以在点击事件中，修改弹框的内容
```diff
pointLayer.on("click", function (e) {
-    console.log(e);
+    infoTemplate.setContent('我是弹框的内容呀！')
});
```

这样一个小功能就实现了。