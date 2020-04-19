<template>
    <div id="app">
        <web-map v-bind:centerX="lat" v-bind:centerY="long" ref="map"/>

        <div class="center">
          <b-button class="btn-block" @click="getLocation" variant="primary">My Location</b-button>
        </div>
    </div>

    
</template>

<script>
import WebMap from './components/webmap.vue';

export default {
    name: 'App',
    components: { WebMap }, 
    data(){
      return{
        lat: null,
        long: null,
      }
    },
    methods:{

      showPos(pos){

        this.lat = pos.coords.latitude
        this.long = pos.coords.longitude

        
        console.log('new location',this.lat,this.long, this.$refs)   
      },


      getLocation(){
        if (navigator.geolocation) {
              navigator.geolocation.getCurrentPosition(this.showPos);
              this.$refs.map.somethingElse();
          } else { 
            console.log("Geolocation is not supported by this browser.");
          }
      },

    
      
    },
};
</script>

<style>
html,body {
  margin: 0;
  padding: 0;
  height: 100%;
  width: 100%;
}
#app {
  display: flex;
  flex-direction: column;
  padding: 0;
  margin: 0 auto;
  margin-top: 5em;
  width: 50%;
  height: 60%;
}
.center{
  margin-top: 2em;
}
</style>