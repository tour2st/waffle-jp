<script setup>
defineProps({
  position : {
    required: true
  },
  size : {
    required: true
  }
})
</script>

<template>
  <span class="one_frame" :style="{top:position.y*20+'%', left:position.x*20+'%'}">
  </span>
</template>

<style scoped>
.one_frame {
    position: absolute;
    user-select: none;
    /*パネルサイズ*/
    width: 19%;
    height:19%;
    margin:0.5%;

    box-sizing: border-box;
    border-style: solid;
    border-radius: 20%;
    border-width: 0.1vw;
    border-color: #aaa;
}
</style>