<template>
  <div></div>
</template>

<script>
import { loadModules } from "esri-loader";
let E = {}; // placeholder for Esri modules
// (could have used `this`, but it didn't feel right...)
export default {
  name: "web-map",
  props: {
    coords: {
      type: Object,
      required: true
    },
  },
  data: function() {
    return {
      view: null,
      nodes: []
    };
  },
  mounted() {
    // lazy load the required ArcGIS API for JavaScript modules and CSS
    loadModules([
        "esri/Map", 
        "esri/views/MapView", 
        "esri/geometry/Point", 
        "esri/Graphic",
        "esri/layers/GraphicsLayer",
        "esri/widgets/Sketch"], {
      css: true
    }).then(([ArcGISMap, MapView, Point, Graphic, GraphicsLayer, Sketch]) => {
      E.ArcGISMap = ArcGISMap;
      E.MapView = MapView;
      E.Point = Point;
      const map = new E.ArcGISMap({
        basemap: "topo-vector"
      });

      this.view = new E.MapView({
        container: this.$el,
        map: map,
        center: [this.coords.latitude, this.coords.longitude],
        zoom: 9
      });
        var graphicsLayer = new GraphicsLayer();
        // create a new sketch widget
        const sketch = new Sketch({
          view:this.view,
          layer: graphicsLayer
        });

      var point = {
         type: "point",
         longitude: -118.80657463861,
         latitude: 34.0005930608889
       };

       var simpleMarkerSymbol = {
         type: "simple-marker",
         color: [226, 119, 40],  // orange
         outline: {
           color: [255, 255, 255], // white
           width: 1
         }
       };

       var pointGraphic = new Graphic({
         geometry: point,
         symbol: simpleMarkerSymbol
       });

        

      
      this.view.ui.add(sketch, "top-right");

      graphicsLayer.add(pointGraphic);
       map.add(graphicsLayer);


    });
   
  },
  watch: {
    coords() {
      this.showPos();
    }
  },
  beforeDestroy() {
    if (this.view) {
      this.view.container = null;
    }
  },
  methods: {
    showPos() {
      if (E.Point) {
        const point = new E.Point(this.coords.longitude, this.coords.latitude);
        this.view.goTo({ center: point });
      } else {
        console.log("Map modules are still loading...");
      }
    },

    calcAvg(param){
      console.log('calc average',param)

      switch(param) {
          case 'wind':
            console.log('wind nodes',this.nodes)
            break;
          case 'rain':
            console.log('rain nodes',this.nodes)
            break;
          case 'elevation':
            console.log('elevation nodes',this.nodes)
            break;
          default:
            // code block
        }
    },
  }
};
</script>

