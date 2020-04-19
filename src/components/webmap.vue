<template>
  <div></div>
</template>

<script>
import { loadModules } from 'esri-loader';

export default {
  name: 'web-map',
  props:['centerX', 'centerY'],
  data: function(){
    return{
      X: this.centerX,
      Y: this.centerY,
  
    }
  },
  mounted() {
    console.log('new data',this.X,this.Y)

    // lazy load the required ArcGIS API for JavaScript modules and CSS
    loadModules(['esri/Map', 'esri/views/MapView'], { css: true })
    .then(([ArcGISMap, MapView]) => {
      const map = new ArcGISMap({
        basemap: 'topo-vector'
      });

      this.view = new MapView({
        container: this.$el,
        map: map,
        center: [-118,34],   ///USE PROPS HERE FOR NEW CENTER
        zoom: 8
      });
    });
  },
  beforeDestroy() {
    if (this.view) {
      // destroy the map view
      this.view.container = null;
    }
  },
  methods:{
    somethingElse(){
   
      console.log('map x',this.view)
      console.log('props',this.X,this.Y)
      this.view.goTo({center:[70,70]})
    
    },
  }

};

</script>

<style scoped>
div {
  padding: 0;
  margin: 0;
  width: 100%;
  height: 100%;
  border: 1px solid black;
}
</style>