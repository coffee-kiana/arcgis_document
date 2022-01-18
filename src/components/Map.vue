<template>
  <div class="map" id="Map"></div>
</template>

<script>
import * as esriLoader from "esri-loader"; // 引入 esri-loader
export default {
  name: "Map",
  created() {
    this.init(); // 初始化地图
  },
  methods: {
    init() {
      // arcgis的配置
      const options = {
        url: "https://js.arcgis.com/3.39/",
      };

      esriLoader.loadModules(["esri/map","esri/geometry/Point","esri/symbols/TextSymbol","esri/symbols/Font","esri/graphic", "dojo/domReady!"], options).then(([Map,Point,TextSymbol,Font,Graphic]) => {
        let self = this; // 存储当前的 this 指向
        this.map = new Map("Map", {
          basemap: "topo-vector",
          center: [120.12, 30.16],
          zoom: 9,
        });
        // 当地图加载完成后，执行 mapLoaded 函数
        this.map.on("load", mapLoaded);
        
        function mapLoaded() {
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
            // 标记点的图片(svg)
            let iconpath =
              "M528 32C325.056 32 160 196.8 160 399.36c0 75.2 22.656 147.584 65.024 208.48 2.112 3.648 4.256 7.168 6.784 10.592l268.608 353.472c7.296 8.096 17.088 12.576 27.584 12.576 10.368 0 20.224-4.512 28.768-14.08l267.36-352c2.624-3.52 4.896-7.36 6.112-9.6A364.864 364.864 0 0 0 896 399.36C896 196.8 730.912 32 528 32z m0 498.72a131.52 131.52 0 0 1-131.456-131.232 131.488 131.488 0 0 1 262.88 0 131.52 131.52 0 0 1-131.424 131.2z";
            let fill_color = "#80B7F9";
            //遍历点
            point.forEach((ele,index)=>{
              //设置文本标记的文本内容、颜色、偏移量、字体
              let textSymbol = new TextSymbol(pointLabel[index].toString())
                  .setColor("#0b539d")
                  .setOffset(0, 15)
                  .setFont(new Font("15px", Font.STYLE_NORMAL, Font.VARIANT_NORMAL, Font.WEIGHT_BOLD, "Arial"));
              //创建点
              let pointObj=new Point(ele);
              var graphic = new Graphic(pointObj,createSymbol(iconpath, fill_color));
              let graphicText = new Graphic(pointObj, textSymbol);
              //将点标记和文本标记添加到地图
              self.map.graphics.add(graphicText);
              self.map.graphics.add(graphic);
              
            })
          }
          function createSymbol(path, color) {
            var markerSymbol = new esri.symbol.SimpleMarkerSymbol();
            markerSymbol.setPath(path);
            markerSymbol.setColor(new dojo.Color(color));
            markerSymbol.setOutline(null);
            return markerSymbol;
          }
      });
    },
  },
};
</script>


<style scoped>
/* 3.39版本的地图样式 */
@import url(https://js.arcgis.com/3.39/esri/css/esri.css);
.map {
  width: 100%;
  height: 100%;
}
</style>