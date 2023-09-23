<script setup>
import { reactive, ref } from 'vue'

defineProps({
  letter : {
    type: String,
    required: true
  },
  status : {

    type: String,
    required: true
  }
})

const offset = reactive({
  x: -50,
  y: -50,
})

const isClicked = ref(false);

function mousedown(event) {
  this.isClicked = true;
  this.offset.y = event.y - parseInt(event.target.style.top)
  this.offset.x = event.x - parseInt(event.target.style.left)
}

function move(event){
  if(!this.isClicked) return
  event.target.style.top = (event.y - this.offset.y) + 'px'
  event.target.style.left = (event.x - this.offset.x) +'px'
}
</script>

<template>
  <span style="top:0px; left:0px" class="panel" :class="status" @mousedown="mousedown($event)" @mousemove="move($event)" @mouseup="isClicked=false">
    <p class="letter">{{ letter }}</p>
  </span>
</template>

<style scoped>
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

.miss {
  /*パネルデザイン(不正解)*/
  border-radius: 2vw;
  background-color: #eee;
  box-shadow: 0 0.5vw #ccc;
}

.blow {
  /*パネルデザイン(同じ行または列)*/
  border-radius: 2vw;
  background-color: #eb3;
  box-shadow: 0 0.5vw #c91;
  color: #fff;
}

.hit {
  /*パネルデザイン(正解)*/
  border-radius: 2vw;
  background-color: #6b5;
  box-shadow: 0 0.5vw #593;
  color: #fff;
}

.letter {
  /*文字を選択禁止にする*/
  user-select: none;
  /*文字のクリックイベントを止める*/
  pointer-events: none;

}
</style>
