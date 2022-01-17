## 创建并且创建地图

![微信图片_20220117174657.png](https://p9-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/93f5f8367b7543f8b2ee650d47d7cb62~tplv-k3u1fbpfcp-watermark.image?)

1. 首先，引入 `esri-loader` 包，如下：
```js
import * as esriLoader from "esri-loader";
```

2. 在 `methods` 里面进行 `arcgis` 的配置:
```js
    // url：arcgis 的地图资源，3.39为 arcgis 的版本号
    const options = {
        url: "https://js.arcgis.com/3.39/",
      };
    
    // 引入需要的资源文件，需要按照顺序引入，否则可能会报错（根据官网的顺序）
    esriLoader.loadModules(["esri/map", "dojo/domReady!"], options).then(([Map]) => {
        this.map = new Map("Map", {
          basemap: "topo-vector", // arcgis 的地图样式
          center: [19.82, 41.33], // arcgis 的中心点坐标
          zoom: 3, // arcgis 的放大标准
        });
    });
```