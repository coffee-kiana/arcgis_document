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

      esriLoader.loadModules(["esri/map", "dojo/domReady!"], options).then(([Map]) => {
        this.map = new Map("Map", {
          basemap: "topo-vector",
          center: [19.82, 41.33],
          zoom: 3,
        });
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