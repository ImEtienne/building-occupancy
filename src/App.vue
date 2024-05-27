<template>
  <div id="app">
    <div class="header">
      <h1>Statistique d'occupation du bâtiment : Usine</h1>
    </div>
    <BuildingOccupancy />
  </div>
</template>

<script>
import BuildingOccupancy from './components/Building.vue';

export default {
  name: 'App',
  components: {
    BuildingOccupancy
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;
  box-sizing: border-box;
}
#app {
  font-family: Arial, sans-serif;
  text-align: center;
  margin: 20px;
  padding: 20px;
  border-radius: 10px;
}

.header {
  background-color: #507299;
  padding: 20px;
  border-radius: 10px;
  color: white;
  margin-bottom: 20px;
  text-align: center;
}
</style>
