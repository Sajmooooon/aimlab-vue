<template>
  <Balloon v-for="balloon in balloons" :key="balloon.id"
           :x="balloon.x" :y="balloon.y"
  ></Balloon>
</template>

<script>
import Balloon from "@/components/Balloon";
export default {
  name: 'App',
  components: {
    Balloon
  },
  data(){
    return{
      maxWidth: 0,
      maxHeight: 0,
      balloons: [
        {id: 0, x: 0, y: 0},
        {id: 1, x: 50, y: 150},
        {id: 2, x: 150, y: 50},
        {id: 3, x: 300, y: 400},
      ],
    }
  },
  beforeMount() {
    const body = document.querySelector('body');

    const bodyHeight = body.offsetHeight;
    const bodyWidth = body.offsetWidth;

    this.maxHeight = bodyHeight
    this.maxWidth = bodyWidth
  }
}
</script>

<style>
body{
  background-image: url("assets/bg.png");
  background-repeat: no-repeat;
  background-position: center center;
  background-size: cover;
  background-attachment: fixed;
  height: 100vh;
  cursor: crosshair;

}
</style>
