<template>
  <div class="building">
    <h2>Pourcentage d'occupation</h2>
    <div class="floors">
      <div v-for="(floor, index) in floors" :key="index" class="floor-summary" @click="selectFloor(index)">
        <h3>Étage n°{{ index }} : {{ floor.name }}</h3>
        <p>{{ getOccupancyPercentage(floor.rooms) }} % d'occupation</p>
      </div>
    </div>
    <div v-if="selectedFloor !== null" class="floor-details">
      <FloorOccupancy :floor="floors[selectedFloor]" />
    </div>
  </div>
</template>

<script>
import FloorOccupancy from './Floor.vue';

export default {
  name: 'BuildingOccupancy',
  components: {
    FloorOccupancy
  },
  data() {
    return {
      buildingName: '',
      floors: [],
      selectedFloor: null,
    };
  },
  created() {
    this.loadBuildingData();
  },
  methods: {
    async loadBuildingData() {
      try {
        const geogResp = await fetch('/gegraphicContext_space.json');
        const controlResp = await fetch('/ROOM_control_endpoin_list.json');
        const geogData = await geogResp.json();
        const controlData = await controlResp.json();

        this.processData(geogData, controlData);
      } catch (error) {
        console.error("Erreur lors du chargement des données", error);
      }
    },
    processData(geogData, controlData) {
      const building = geogData.children[0];
      const controls = controlData[0].endpoints; 
      const floors = building.children.map(floor => {
        return {
          name: floor.name,
          rooms: floor.children.map(room => {
            const roomName = room.name.toLowerCase().replace(/\s/g, ''); 
            const control = controls.find(c => c.name.toLowerCase().replace(/\s/g, '') === roomName);
            console.log(`Room: ${room.name}, Control:`, control); 
            return {
              name: room.name,
              status: control ? control.currentValue : 'undefined'
            };
          })
        };
      });

      this.buildingName = building.name;
      this.floors = floors;
    },
    getOccupancyPercentage(rooms) {
      const occupiedRooms = rooms.filter(room => room.status === true).length;
      return ((occupiedRooms / rooms.length) * 100).toFixed(2);
    },
    selectFloor(index) {
      this.selectedFloor = index;
    }
  }
}
</script>

<style>
.building {
  display: flex;
  justify-content: space-between;
  align-items: flex-start;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 10px;
  background-color: #f9f9f9;
  gap: 20px;
}

.floors {
  width: 40%;
}

.floor-summary {
  background-color: #fff;
  border: 1px solid #ddd;
  padding: 10px;
  margin: 10px 0;
  border-radius: 5px;
  cursor: pointer;
  transition: background-color 0.3s, box-shadow 0.3s;
}

.floor-summary:hover {
  background-color: #f0f0f0;
  box-shadow: 0 0 10px rgba(0, 0, 0, 0.1);
}

.floor-summary h3 {
  margin: 0;
  color: #333;
}

.floor-summary p {
  margin: 5px 0 0;
  color: #666;
}

.floor-details {
  width: 55%;
  max-height: 500px;
  overflow-y: auto;
  background-color: #fff;
  padding: 20px;
  border: 1px solid #ddd;
  border-radius: 10px;
}
</style>
