<template>
  <div id="app">
    <div class="button_box">
      <div class="button" @click="handle_render">渲染</div>
      <div class="button" @click="handle_switch">切换</div>
      <div class="button" @click="handle_add">增加</div>
      <div class="button" @click="handle_remove">清除</div>
    </div>
    <Map :point="point" :operation="operation" />
  </div>
</template>

<script>
import Map from "./components/Map";

export default {
  name: "App",
  components: {
    Map,
  },
  data() {
    return {
      operation: '',
      point: [],
    };
  },
  methods: {
    handle_render() {
      this.operation='render'
      this.point = [[19.82, 41.33]];
    },
    handle_switch() {
      this.operation='switch'
      this.point = [[16.37, 48.21]];
    },
    handle_add() {
      this.operation='add'
      this.point = this.point.concat([
        [14.51, 46.0],
        [12.45, 41.9],
      ]);
    },
    handle_remove() {
      this.operation='remove'
      this.point = []
    },
  },
};
</script>

<style>
* {
  margin: 0;
  padding: 0;
}
html,
body,
#app {
  width: 100%;
  height: 100%;
}
#app {
  font-family: "Avenir", Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
}
.button_box {
  position: absolute;
  right: 30px;
  top: 30px;
  width: 60px;
  z-index: 1;
}
.button {
  width: 60px;
  height: 60px;
  border: 1px solid #ddd;
  border-radius: 9px;
  cursor: pointer;
  line-height: 60px;
  font-size: 14px;
  background-color: aliceblue;
  margin: 12px 0;
}
</style>
