<template>
  <div class="map" id="Map"></div>
</template>

<script>
import * as esriLoader from "esri-loader"; // 引入 esri-loader
export default {
  name: "Map",
  data() {
    return {
      map: null,
    };
  },
  created() {
    this.init(); // 初始化地图
  },
  methods: {
    init() {
      // arcgis的配置
      const options = {
        url: "https://js.arcgis.com/3.39/",
      };

      esriLoader
        .loadModules(
          [
            "esri/map",
            "esri/geometry/Point",
            "esri/graphic",
            "dojo/_base/array",
            "dojo/domReady!",
          ],
          options
        )
        .then(([Map, Point, Graphic, arrayUtils]) => {
          let self = this; // 存储当前的 this 指向

          // arcgis 配置
          this.map = new Map("Map", {
            basemap: "topo-vector",
            center: [19.82, 41.33],
            zoom: 3,
          });

          // 当地图加载完成后，执行 mapLoaded 函数
          this.map.on("load", mapLoaded);

          function mapLoaded() {
            // 标记点的经纬度坐标
            let point = [
              [19.82, 41.33],
              [16.37, 48.21],
            ];
            // 标记点的图片(svg)
            let iconpath =
              "M528 32C325.056 32 160 196.8 160 399.36c0 75.2 22.656 147.584 65.024 208.48 2.112 3.648 4.256 7.168 6.784 10.592l268.608 353.472c7.296 8.096 17.088 12.576 27.584 12.576 10.368 0 20.224-4.512 28.768-14.08l267.36-352c2.624-3.52 4.896-7.36 6.112-9.6A364.864 364.864 0 0 0 896 399.36C896 196.8 730.912 32 528 32z m0 498.72a131.52 131.52 0 0 1-131.456-131.232 131.488 131.488 0 0 1 262.88 0 131.52 131.52 0 0 1-131.424 131.2z";
            let fill_color = "#80B7F9";

            arrayUtils.forEach(point, function (point) {
              var graphic = new Graphic(
                new Point(point),
                createSymbol(iconpath, fill_color)
              );
              self.map.graphics.add(graphic);
            });
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