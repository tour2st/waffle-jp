<script setup>
import { onMounted, reactive, ref } from 'vue'
defineProps({
  letter : {
    type: String,
    required: true
  },
  status : {
    type: String,
    required: true
  },
  position : {
    required: true
  },
  offset : {
    required: true
  },
  isselect : {
    type: Boolean,
    required: true
  },
  size : {
    required: false
  }
})


</script>

<template>
  <span class="panel" :class="status, {'selecting': isselect}" :style="{top:position.y*11+'vw', left:position.x*11+'vw'}">
    <p class="letter">{{ letter }}</p>
  </span>
</template>

<style scoped>
/*パネルの基礎デザイン*/
.panel {
  transition-duration: 100ms;
  /*パネル内の文字を中央ぞろえ*/
  display: flex;
  align-items: center;
  justify-content: center;

  /*パネルサイズ*/
  width: 10vw;
  height: 10vw;

  /*文字サイズ*/
  font-family: sans-serif;
  font-weight: 700;
  font-size: max(4.5vw,min(30px,10vw));

  /*座標*/
  position: absolute;
  top:0px;
  left:0px;

  border-radius: 2vw;
  border-style: solid;
  border-width: 0.1vw;

  pointer-events: none;
}
/*選択中のパネル*/
.selecting {
  transition-duration: 0ms;
  transform:scale(1.2,1.2);
}

/*パネルデザイン(不正解)*/
.miss {
  background-color: #eee;
  box-shadow: 0 0.5vw #ccc;
  border-color: #eee;
}
/*パネルデザイン(同じ行または列)*/
.blow {
  background-color: #eb3;
  box-shadow: 0 0.5vw #c91;
  border-color: #eb3;
  color: #fff;
}
/*パネルデザイン(正解)*/
.hit {
  background-color: #6b5;
  box-shadow: 0 0.5vw #593;
  border-color: #6b5;
  color: #fff;
}
.nothing {
  visibility: hidden;
}
/*文字を選択禁止&文字のマウスイベントを止める*/
.letter {
  user-select: none;
  pointer-events: none;
}
</style>
