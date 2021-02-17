<template>
  <div id="app">
    <!-- Карточка товара -->
    <VehicleCard 
      :key="unitCode"
      v-bind:vehicles="vehicles"
      v-bind:vehiclesProp="vehiclesProp"
      v-bind:vehiclesColors="vehiclesColors"
      v-bind:vehiclesOrderParameters="vehiclesOrderParameters"
      v-bind:unitCode="unitCode"
    />
    
  </div>
</template>

<script>
import VehicleCard from './components/VehicleCard'

export default {
  data() {
    return {
      vehicles: {},
      vehiclesProp: [],
      vehiclesOrderParameters: [],
      vehiclesColorParameters: {},
      vehiclesColors: [],
      unitCode: ""
    }
  },
  name: 'App',
  components: {
    VehicleCard
  },
  mounted() {
    // Получение JSON
      fetch('https://raw.githubusercontent.com/maxxeefy/masterhost_json/main/master.json')
        .then((response) => response.json())
        .then(json => {
          this.vehicles = json
          this.vehiclesProp = this.vehicles.properties
          this.vehiclesOrderParameters = this.vehicles.parameters
          for (let i=0; i < this.vehicles.parameters.length; i++) {
            if(this.vehicles.parameters[i].code == "color")
              this.vehiclesColorParameters = this.vehicles.parameters[i]
              this.vehiclesColors = this.vehiclesColorParameters.value
          }
          this.unitCode = this.vehicles.unitCode
        })
  }
}
</script>

<style>
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 60px;
}
@media screen and (min-width: 980px) {
  body {
    max-width: 980px;
    margin: 0 auto;
  }

}
@media screen and (max-device-width: 320px) {
  body {
    min-width: 320px;
  }
}
</style>
