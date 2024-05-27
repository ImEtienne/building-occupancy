<template>
  <div class="floor-detail">
    <h2>Détails de l'étage : {{ floor.name }}</h2>
    <div class="room-details">
      <div v-for="(room, index) in floor.rooms" :key="index" class="room-detail">
        <p>Pièce n°{{ index }} : {{ room.name }} - <span :class="statusClass(room.status)">{{ roomStatus(room.status) }}</span></p>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'FloorOccupancy',
  props: {
    floor: Object
  },
  methods: {
    roomStatus(status) {
      if (status === true) return "TRUE";
      if (status === false) return "FALSE";
      return "UNDEFINED";
    },
    statusClass(status) {
      if (status === true) return 'status-true';
      if (status === false) return 'status-false';
      return 'status-undefined';
    }
  }
}
</script>

<style>
.floor-detail {
  background-color: #fff;
  border: 1px solid #ddd;
  padding: 20px;
  border-radius: 10px;
}

.room-details {
  margin-top: 20px;
  max-height: 400px;
  overflow-y: auto;
}

.room-detail {
  margin: 10px 0;
  padding: 10px;
  border: 1px solid #ddd;
  border-radius: 5px;
  display: flex;
  justify-content: space-between;
  background-color: #f9f9f9;
}

.room-detail p {
  margin: 0;
  flex: 1;
  display: flex;
  justify-content: space-between;
}

.status-true {
  color: green;
}

.status-false {
  color: red;
}

.status-undefined {
  color: gray;
}
</style>
