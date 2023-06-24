<template>
  <img src="../assets/balloon.png" alt="balloon" class="balloon" ref="balloon">
</template>

<script>
export default {
  name: "Balloon",
  props: ['x','y'],
  mounted() {
    const img = this.$refs.balloon
    img.style.left = this.x + 'px';
    img.style.top = this.y + 'px'
  }
}
</script>

<style scoped>
.balloon{
  height: 104px;
  width: 106px;
  position: absolute;
  filter: drop-shadow(0 0 0.75rem gray);}
</style>