<script setup></script>

<template>
    <div class="left_icon" href="archive.com">
        <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-history" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
        <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
        <path d="M12 8l0 4l2 2"></path>
        <path d="M3.05 11a9 9 0 1 1 .5 4m-.5 5v-5h5"></path>
        </svg>
  </div>
</template>

<style>
.left_icon {
  position: absolute;
  top: 0;
  left: 0;
  width: 10vw;
  height: 10vw;
  z-index: 1;
  display: flex;
  align-items: center;
  justify-content: center;
}

.icon {
  transform: scale(1.5,1.5);
  cursor: pointer;
}
</style>