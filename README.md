
![image.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/ba9a727c231046e895deb139e2e81389~tplv-k3u1fbpfcp-watermark.image?)
**文本标记不会显示标记，只有文本，标记需要另外添加**

文本通过循环增加多个

1.  首先，需要在 [添加标记点](https://github.com/xuan-zai/arcgis_document/tree/%E7%BB%99%E5%9C%B0%E5%9B%BE%E5%A2%9E%E5%8A%A0%E6%A0%87%E8%AE%B0%E7%82%B9) 的基础上再引入以下资源：

```diff
"esri/map",
"esri/geometry/Point",
"esri/graphic",
+"esri/symbols/TextSymbol",
+"esri/symbols/Font",
"dojo/domReady!"
```

2.  标点的经纬度和标记的文本内容

```
// 标记点的经纬度坐标
let point = [
  [120.444444, 30.184722],
  [120.403055, 30.278333],
  [120.168888, 29.951944],
  [120.244722, 30.19]
];
//标记点的文字标记
var pointLabel = [
  ["first"],
  ["secode"],
  ["there"],
  ["four"]
];
```

3.循环 `标记点的经纬度坐标`和`文本内容`, 创建坐标点和文本
```
point.forEach((ele,index)=>{
  //创建文本标记 设置文本内容、颜色、偏移量、字体
  let textSymbol = new TextSymbol(pointLabel[index].toString()).setColor("#0b539d")
      .setOffset(0,15)
      .setFont(new Font("15px", Font.STYLE_NORMAL, Font.VARIANT_NORMAL, Font.WEIGHT_BOLD, "Arial"));
  //创建点
  let pointObj=new Point(ele);
  var graphic = new Graphic(pointObj,createSymbol(iconpath, fill_color));
  //创建文本图形
  let graphicText = new Graphic(pointObj, textSymbol);
  //将文本标记和点标记图形添加到地图中
  self.map.graphics.add(graphicText);
  self.map.graphics.add(graphic);
})
```