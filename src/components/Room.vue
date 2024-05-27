<template>
  <div :class="['room', statusClass]">
    {{ room.name }}: <span :class="statusClass">{{ roomStatus }}</span>
  </div>
</template>

<script>
export default {
  name: 'RoomOccupancy',
  props: {
      room: {
        type: Object,
        required: true,
        default: () =>({})
    }
  },
  computed: {
      roomStatus() {
        if (this.room && 'status' in this.room) {
            if (this.room.status === true) return "TRUE";
            if (this.room.status === false) return "FALSE";
        }
        return "UNDEFINED";
    },
      statusClass() {
          if (this.room && 'status' in this.room) {
              if (this.room.status === true) return 'status-true';
              if (this.room.status === false) return 'status-false';
          }
      return 'status-undefined';
    }
  }
}
</script>

<style>
.room {
  margin: 5px 0;
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
