## 对图层的操作

![route.gif](https://p3-juejin.byteimg.com/tos-cn-i-k3u1fbpfcp/228d171ae96b4ee3a4e08d46ab6a9e2c~tplv-k3u1fbpfcp-watermark.image?)

在项目中，我们肯定会对图层中的数据进行一个增删改查的操作。现在就由我们来完成这个效果吧。

> 数据全是模拟

### 增加模拟数据和操作按钮
> 为了方便，直接在 `App.vue` 中添加
1. 修改原有结构
```diff
  <div id="app">
+   <div class="button_box">
+     <div class="button" @click="handle_render">渲染</div>
+     <div class="button" @click="handle_switch">切换</div>
+     <div class="button" @click="handle_add">增加</div>
+     <div class="button" @click="handle_remove">清除</div>
+   </div>
-   <Map />
+   <Map :point="point" :operation="operation" />
  </div>
```
2. 增加数据
```js
data() {
    return {
      operation: '', // 用于方便当前是什么操作
      point: [],  // 标记点坐标(模拟动态效果)
    };
},
methods: {
    // 渲染操作
    handle_render() {
      this.operation='render'
      this.point = [[19.82, 41.33]];
    },
    // 切换操作
    handle_switch() {
      this.operation='switch'
      this.point = [[16.37, 48.21]];
    },
    // 增加操作
    handle_add() {
      this.operation='add'
      this.point = this.point.concat([
        [14.51, 46.0],
        [12.45, 41.9],
      ]);
    },
    // 清除操作
    handle_remove() {
      this.operation='remove'
      this.point = []
    },
},
```
### 对数据更新时进行操作
> 因为这次对图层有动态增删改的操作，为了使代码结果比较的清晰，所以做了一些优化，`和原本的有很大的不同`。请自行观看本章节的源代码，这里就不进行太多的阐述了。
```diff
-  let point = [[19.82, 41.33], [16.37, 48.21]]; // 原本是在Map文件中写的固定值
+  let point = this.point; // 根据APP中传入的值继续改变
```
```js
watch: {
    // 监听 operation 是否变化，变化就会执行这个函数
    operation: function () {
    // 当前操作不是增加数据的话，则需要进行清除该图层中的标记点
      if(this.operation !== 'add'){
        this.pointLayer.clear(); // 清除该图层的标记点
      }
    // 再一次处理标记点的数据
      this.deal_with(this.point)
    }
}
```