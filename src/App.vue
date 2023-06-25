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
      balloons: [],
      lastId: 0,
      numBalloons: 4
    }
  },
  methods: {
    removeBalloon(id){
      this.balloons = this.balloons.filter(balloon=> balloon.id !== id)
      this.generateNewBalloon()

    },
    generateNewBalloon(){
      let newX = Math.random()*(500-50)+50
      let newY = Math.random()*(500-50)+50
      this.lastId += 1
      let balloon = {
        id: this.lastId,
        x: newX,
        y: newY
      }
      this.balloons.push(balloon)
      console.log(this.balloons)
    }
  },
  beforeMount() {
    const body = document.querySelector('body');

    const bodyHeight = body.offsetHeight;
    const bodyWidth = body.offsetWidth;

    this.maxHeight = bodyHeight
    this.maxWidth = bodyWidth


  },
  mounted() {
    for(let i = 0; i<this.numBalloons; i++){
      this.generateNewBalloon()
    }
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
