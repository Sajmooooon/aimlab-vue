<template >
  <Modal @startGame="startGame" :accuracy="calcAccuracy" :points="calcPoints" v-show="gameEnd"></Modal>
  <div class="playground" @click="addClick" v-if="!gameEnd">
    <TopScoreBoard :accuracy="calcAccuracy" :points="calcPoints" :time="timer" @endGame="endGame"></TopScoreBoard>
    <Balloon v-for="balloon in balloons" :key="balloon.id"
             :balloon="balloon" @pop="removeBalloon"
    ></Balloon>
  </div>

</template>

<script>
import Balloon from "@/components/Balloon";
import TopScoreBoard from "@/components/TopScoreBoard";
import Modal from "@/components/Modal";
export default {
  name: 'App',
  components: {
    Modal,
    Balloon, TopScoreBoard
  },
  data(){
    return{
      maxWidth: 0,
      maxHeight: 0,
      balloons: [],
      lastId: 0,
      numBalloons: 5,
      timer: 30,
      hits: 0,
      clicks: 0,
      gameEnd: true
    }
  },
  methods: {
    removeBalloon(id){
      this.balloons = this.balloons.filter(balloon=> balloon.id !== id)
      this.generateNewBalloon()
      this.hits += 1
    },
    removeAllBalloons(){
      this.balloons = []
    },
    endGame(){
      this.removeAllBalloons()
      this.gameEnd = true
    },
    addClick(){
      this.clicks += 1
    },
    generateNewBalloon(){
      let generateDistance = true
      let newX = 0
      let newY = 0
      while(generateDistance){
        newX = Math.random()*((this.maxWidth-300)-(this.maxWidth/2-300))+(this.maxWidth/2-300)
        newY = Math.random()*(500-50)+50
        generateDistance = this.checkDistance(newX, newY)
      }
      this.lastId += 1
      let balloon = {
        id: this.lastId,
        x: newX,
        y: newY
      }
      this.balloons.push(balloon)
    },
    checkDistance(x,y){
      for(let ball of this.balloons){
          let distance = this.distanceBetweenPoints(x,ball.x, y, ball.y)
          if(distance<150){
            return true
          }
      }
      return false
    },
    distanceBetweenPoints(x1,x2,y1,y2){
      let a = x1 - x2;
      let b = y1 - y2;
      let c = Math.sqrt( a*a + b*b );
      return c
    },
    startGame(data){
      this.numBalloons = data.balloons
      this.timer = data.time
      this.gameEnd = false
      this.hits = 0
      this.clicks = 0
      for(let i = 0; i<this.numBalloons; i++){
        this.generateNewBalloon()
      }
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
  padding: 0;
  margin: 0;
  -webkit-user-select: none;
  -ms-user-select: none;
  user-select: none;

}
.playground{
  cursor: crosshair;
  display: flex;
  flex-direction: column;
  height: 100vh;
}
</style>
