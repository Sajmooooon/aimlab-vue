<template >
  <div class="playground" @click="addClick">
    <TopScoreBoard :accuracy="calcAccuracy" :points="calcPoints"></TopScoreBoard>
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
      numBalloons: 4,
      hits: 0,
      clicks: 0,
    }
  },
  methods: {
    removeBalloon(id){
      this.balloons = this.balloons.filter(balloon=> balloon.id !== id)
      this.generateNewBalloon()
      this.hits += 1
    },
    addClick(){
      this.clicks += 1
    },
    generateNewBalloon(){
      let newX = Math.random()*(this.maxWidth/2-50)+50
      let newY = Math.random()*(500-50)+50
      this.lastId += 1
      let balloon = {
        id: this.lastId,
        x: newX,
        y: newY
      }
      this.balloons.push(balloon)

    }
  },
  computed: {
    calcAccuracy(){
      if(this.clicks===0){
        return 100
      }
      let acc = this.hits * 100 / this.clicks
      return acc.toFixed(2)
    },
    calcPoints(){
      let positivePoints = this.hits * 10
      let negativePoints = (this.clicks-this.hits) * 5
      return positivePoints - negativePoints
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
  -webkit-user-select: none;
  -ms-user-select: none;
  user-select: none;

}
.playground{
  display: flex;
  flex-direction: column;
  height: 100vh;
}
</style>
