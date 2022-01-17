## 给地图添加标记点

### svg 可单个可多个

1. 首先，需要在 [加载地图] 的基础上再引入以下资源：
```diff
"esri/map",
+ "esri/geometry/Point",
+ "esri/graphic",
+ "dojo/_base/array",
"dojo/domReady!",
```

2. 标记点的经纬度坐标
```js
// 可以为单个或者多个
let point = [
  [19.82, 41.33],
  [16.37, 48.21],
];
```

3. 标记点图片和标记点的填充颜色
```js
// iconpath: svg 的 path 路径
let iconpath =
              "M819.6 606.7l-185.8-52.5-189 52.6-103.5-42.9-28.7 69.3L440 686l193.6-53.9 190.9 54 136.7-57.7-29.2-69.1zM819.6 820.9l-185.8-52.5-189 52.6-103.5-42.8-28.7 69.2L440 900.2l193.6-53.9 190.9 54 136.7-57.7-29.2-69.1zM633.6 203.7l190.9 54 136.7-57.8-29.2-69.1-112.4 47.5-185.8-52.5-189 52.6-103.5-42.9-28.7 69.3L440 257.6zM819.6 392.5L633.8 340l-189 52.6-103.5-42.9-28.7 69.3L440 471.8l193.6-53.9 190.9 54 136.7-57.8-29.2-69zM66.8 139.1h94.9v176.4H66.8v75h94.9v186.6H66.8v75h94.9v201.7H64.5v75h172.2V64.1H66.8z";
// fill_color: 填充颜色
let fill_color = "#80B7F9";
```

4. 循环 `标记点的经纬度坐标`, 创建坐标点
```js
// new Graphic: 创建新的图形对象
// new Point: 根据经纬度创建点
arrayUtils.forEach(point, function (point) {
  var graphic = new Graphic(
    new Point(point),
    createSymbol(iconpath, fill_color)
  );
  self.map.graphics.add(graphic);
});
```

5. 在图层上绘制点或者多点
```js
function createSymbol(path, color) {
  // new esri.symbol.SimpleMarkerSymbol: 在图层上绘制点
  var markerSymbol = new esri.symbol.SimpleMarkerSymbol();
  markerSymbol.setPath(path); // 设置点的样式
  markerSymbol.setColor(new dojo.Color(color)); // 设置点的填充颜色
  markerSymbol.setOutline(null); // 设置点的轮廓
  return markerSymbol;
}
```