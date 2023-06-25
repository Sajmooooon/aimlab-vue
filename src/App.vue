<template>
  <div class="playground">
    <TopScoreBoard></TopScoreBoard>
    <Balloon v-for="balloon in balloons" :key="balloon.id"
             :balloon="balloon" @pop="removeBalloon"
    ></Balloon>
  </div>

</template>

<script>
import Balloon from "@/components/Balloon";
import TopScoreBoard from "@/components/TopScoreBoard";
export default {
  name: 'App',
  components: {
    Balloon, TopScoreBoard
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
  methods: {
    removeBalloon(id){
      this.balloons = this.balloons.filter(balloon=> balloon.id !== id)
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
.playground{
  display: flex;
  flex-direction: column;
}
</style>
