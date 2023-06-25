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
      balloons: []
    }
  },
  methods: {
    removeBalloon(id){
      this.balloons = this.balloons.filter(balloon=> balloon.id !== id)
      fetch("http://localhost:3000/balloons/"+id,{
        method: "DELETE"}).then(() => this.$emit('delete',id))
          .catch(err => console.log(err.message))
    },
  },
  beforeMount() {
    const body = document.querySelector('body');

    const bodyHeight = body.offsetHeight;
    const bodyWidth = body.offsetWidth;

    this.maxHeight = bodyHeight
    this.maxWidth = bodyWidth
  },
  mounted() {
    fetch("http://localhost:3000/balloons")
        .then(data=>data.json())
        .then(balloons=>this.balloons=balloons)
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
