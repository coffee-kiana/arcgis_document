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
              "M819.6 606.7l-185.8-52.5-189 52.6-103.5-42.9-28.7 69.3L440 686l193.6-53.9 190.9 54 136.7-57.7-29.2-69.1zM819.6 820.9l-185.8-52.5-189 52.6-103.5-42.8-28.7 69.2L440 900.2l193.6-53.9 190.9 54 136.7-57.7-29.2-69.1zM633.6 203.7l190.9 54 136.7-57.8-29.2-69.1-112.4 47.5-185.8-52.5-189 52.6-103.5-42.9-28.7 69.3L440 257.6zM819.6 392.5L633.8 340l-189 52.6-103.5-42.9-28.7 69.3L440 471.8l193.6-53.9 190.9 54 136.7-57.8-29.2-69zM66.8 139.1h94.9v176.4H66.8v75h94.9v186.6H66.8v75h94.9v201.7H64.5v75h172.2V64.1H66.8z";
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