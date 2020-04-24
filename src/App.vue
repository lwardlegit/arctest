<template>
  <div id="app">
    <web-map id="mapComponent" ref="map" :coords="coords"/>

    <div class="center">
      <b-button class="btn-block" @click="updateCenter()" variant="primary">My Location</b-button>

      <p>after placing nodes on the map, select an analysis tool from the dropdown menu</p>

      <b-dropdown>
        <b-dropdown-item @click="getAvg('rain')">rainfall</b-dropdown-item>
        <b-dropdown-item @click="getAvg('wind')">wind speed</b-dropdown-item>
        <b-dropdown-item @click="getAvg('elevation')">elevation</b-dropdown-item>
      </b-dropdown>

    </div>
  </div>
</template>

<script>
import WebMap from "./components/webmap";

export default {
  name: "App",
  components: {
    WebMap
  },
  data: () => ({
    coords: {
      latitude: -118,
      longitude: 34
    },
  }),
  methods: {
    getAvg(param){
      this.$refs.map.calcAvg(param)
    },

    updateCenter() {
      this.getLocation();
    },
    getLocation() {
      if (navigator.geolocation) {
        navigator.geolocation.getCurrentPosition(position => {
          this.coords = {
            latitude: position.coords.latitude,
            longitude: position.coords.longitude
          };
        });
      } else {
        console.log("Geolocation not supported by your browser.");
      }
    }
  }
};
</script>
<style lang="scss" src="bootstrap-scss/bootstrap.scss"></style>
<style lang="scss">
#app {
  display: flex;
  min-height: 100vh;
  width: 65%;
  margin: 2em auto;
  flex-direction: column;
  > *:first-child {
    flex-grow: 1;
  }
  #mapComponent{
    border: 1px solid black;
    margin: 3em;
  }
}
</style>
