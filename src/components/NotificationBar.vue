<template>
  <transition enter-active-class="animate__animated animate__slideInUp" leave-active-class="animate__animated animate__slideOutDown">
    <div v-show="visible" id="card" :style="{'borderColor': bg_color}">
      <div id="card-inner">
        <button @click="visible = !visible;" id="close-btn">X</button>
       <p style="font-size: 20px; font-family: Arial, serif;">{{text}}</p>
      </div>
    </div>
  </transition>

</template>

<script>
export default {
  name: "NotificationBar",
  data: function() {
    return{
      visible: false,
      text: "Default Text",
      bg_color: "transparent",
    }
  },
  methods: {
    notify(text, color="transparent") {
      this.text = text;
      this.bg_color = color;
      if(!this.visible) this.visible = true;
    }
  }
}
</script>

<style scoped>
#card {
  width: 50%;
  height: 50px;
  position: absolute;
  bottom: 10px;
  left: 25%;
  border-radius: 20px;
  background-color: white;
  border: transparent 5px solid;
}
#card-inner {
  width: 100%;
  height: 100%;
  display: flex;
  justify-content: left;
  align-items: center;
}
#close-btn {
  border: none;
  margin-right: 10px;
  width: 50px;
  height: 50px;
  color: white;
  background-color: lightgray;
  border-top-left-radius: 10px;
  border-bottom-left-radius: 10px;
  font-size: 1.75em;
}
#close-btn:hover {
  background-color: lightcoral;
}
#close-btn:active {
  background-color: indianred;
}
</style>