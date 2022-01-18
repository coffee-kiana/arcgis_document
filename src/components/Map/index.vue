<template>
  <div class="map" id="Map"></div>
</template>

<script>
import * as esriLoader from "esri-loader";
export default {
  name: "Map",
  props: ["point", "operation"],
  data() {
    return {
      map: null,
      pointLayer: null,
      Graphic: null,
      Point: null,
      SimpleMarkerSymbol: null,
      Color: null,
      arrayUtils: null,
      options: {
        url: "https://js.arcgis.com/3.39/",
      },
      infoTemplate: null,
      fill_color: "#80B7F9",
      iconpath:
        "M528 32C325.056 32 160 196.8 160 399.36c0 75.2 22.656 147.584 65.024 208.48 2.112 3.648 4.256 7.168 6.784 10.592l268.608 353.472c7.296 8.096 17.088 12.576 27.584 12.576 10.368 0 20.224-4.512 28.768-14.08l267.36-352c2.624-3.52 4.896-7.36 6.112-9.6A364.864 364.864 0 0 0 896 399.36C896 196.8 730.912 32 528 32z m0 498.72a131.52 131.52 0 0 1-131.456-131.232 131.488 131.488 0 0 1 262.88 0 131.52 131.52 0 0 1-131.424 131.2z",
    };
  },
  created() {
    this.init();
  },
  methods: {
    init() {
      esriLoader
        .loadModules(
          [
            "esri/map",
            "esri/geometry/Point",
            "esri/graphic",
            "dojo/_base/array",
            "esri/layers/GraphicsLayer",
            "esri/InfoTemplate",
            "dojo/domReady!",
          ],
          this.options
        )
        .then(
          ([Map, Point, Graphic, arrayUtils, GraphicsLayer, InfoTemplate]) => {
            this.Point = Point;
            this.Graphic = Graphic;
            this.arrayUtils = arrayUtils;
            this.GraphicsLayer = GraphicsLayer;
            this.InfoTemplate = InfoTemplate;
            this.SimpleMarkerSymbol = esri.symbol.SimpleMarkerSymbol;
            this.Color = dojo.Color;

            this.map = new Map("Map", {
              basemap: "topo-vector",
              center: [19.82, 41.33],
              zoom: 3,
            });

            // 创建图层
            this.pointLayer = new GraphicsLayer({
              id: "pointLayer",
            });

            this.infoTemplate = new InfoTemplate("title", `content`);
            this.pointLayer.setInfoTemplate(this.infoTemplate);

            this.map.on("load", this.mapLoaded);
          }
        );
    },
    mapLoaded() {
      this.deal_with(this.point);
      this.map.addLayer(this.pointLayer, 1);
    },
    // 处理标记点数据
    deal_with(points) {
      let self = this;
      this.arrayUtils.forEach(points, function (point, index) {
        self.add_point(point)
      });
    },
    add_point(point) {
      this.pointLayer.add(
        new this.Graphic(
          new this.Point({
            latitude: point[1],
            longitude: point[0],
          }),
          this.createSymbol()
        )
      );
    },
    createSymbol() {
      let markerSymbol = new this.SimpleMarkerSymbol();
      markerSymbol.setPath(this.iconpath);
      markerSymbol.setColor(new this.Color(this.fill_color));
      markerSymbol.setOutline(null);
      return markerSymbol;
    },
  },
  watch: {
    operation: function () {
      if(this.operation !== 'add'){
        this.pointLayer.clear(); // 清除该图层的标记点
      }
      this.deal_with(this.point)
    }
  }
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