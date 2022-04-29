<template>
  <div id="card"></div>
</template>

<script>
export default {
  name: "NotificationBar"
}
</script>

<style scoped>
#card {
  width: 50%;
  height: 50px;
  position: absolute;
  bottom: 10px;
  left: 50%;
  transform: translateX(-50%);
  background-color: black;
}
</style>