<script>
import AMapLoader from "@amap/amap-jsapi-loader";
import { shallowRef } from "vue";

export default {
  setup() {
    const map = shallowRef(null);
    return {
      map,
    };
  },

  methods: {
    initMap() {
      AMapLoader.load({
        key: "21b62cdcb2d83d4ec59da1291c5f88a9", // 申请好的Web端开发者Key，首次调用 load 时必填
        version: "2.0", // 指定要加载的 JSAPI 的版本，缺省时默认为 1.4.15..
        plugins: ['AMap.Scale', 'AMap.ToolBar'], // 需要使用的的插件列表，如比例尺'AMap.Scale'等
      })
        .then((AMap) => {
          this.map = new AMap.Map("container", {
            //设置地图容器id
            viewMode: "3D", //是否为3D地图模式
            zoom: 5, //初始化地图级别
            center: [105.602725, 37.076636], //初始化地图中心点位置
          });


          this.map.addControl(new AMap.Scale())
          this.map.addControl(new AMap.ToolBar())
          
          this.map.add(new AMap.Marker({
            position:this.map.getCenter()
        }));

        })
        .catch((e) => {
          console.log(e);
        });

        AMapLoader.load({ //可多次调用load
        plugins:['AMap.MapType']
    }).then((AMap)=>{
        this.map.addControl(new AMap.MapType())
    }).catch((e)=>{
        console.error(e);
    });
    },
  },

  mounted() {
    //DOM初始化完成进行地图初始化
    this.initMap();
  },
};
</script>

<template>
  <div id="container"></div>
</template>

<style scoped>
#container {
  padding: 0px;
  margin: 0px;
  width: 100%;
  height: 100vh;
}
</style>
