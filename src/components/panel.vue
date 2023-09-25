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
  }
})


</script>

<template>
  <span class="panel" :class="status, {'selecting': isselect}" :style="{top:position.y+'px', left:position.x+'px'}">
    <p class="letter">{{ letter }}</p>
  </span>
</template>

<style scoped>
/*パネルの基礎デザイン*/
.panel {
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
}
/*選択中のパネル*/
.selecting {
  transform:scale(1.2,1.2);
}

/*パネルデザイン(不正解)*/
.miss {
  border-radius: 2vw;
  background-color: #eee;
  box-shadow: 0 0.5vw #ccc;
}
/*パネルデザイン(同じ行または列)*/
.blow {
  border-radius: 2vw;
  background-color: #eb3;
  box-shadow: 0 0.5vw #c91;
  color: #fff;
}
/*パネルデザイン(正解)*/
.hit {
  border-radius: 2vw;
  background-color: #6b5;
  box-shadow: 0 0.5vw #593;
  color: #fff;
}
/*文字を選択禁止&文字のマウスイベントを止める*/
.letter {
  user-select: none;
  pointer-events: none;
}
</style>
