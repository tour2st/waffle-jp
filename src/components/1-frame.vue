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
  <span class="one_frame" :style="{top:position.y*11+'vw', left:position.x*11+'vw'}">
  </span>
</template>

<style scoped>
.one_frame {
    position: absolute;
    user-select: none;
    width: 100px;
    height: 100px;

    /*パネルサイズ*/
    width: 10vw;
    height: 10vw;

    border-style: solid;
    border-radius: 2vw;
    border-width: 0.1vw;
    border-color: #aaa;
}
</style>