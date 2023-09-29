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

.gameover {
  background-color: #333;
  box-shadow: 0 0.5vw #111;
  border-color: #111;
  color: #fff;
  transition: 1s;
}

.gameclear {
  background-color: #6b5;
  box-shadow: 0 0.5vw #593;
  border-color: #6b5;
  color: #fff;
  animation-name: jump;
  animation-fill-mode:forwards;/*2で解説*/
  animation-duration:0.5s;/*3で解説*/
  animation-iteration-count:1;/*4で解説*/
  animation-timing-function:ease-out;/*5で解説*/
  animation-delay: 0s;/*6で解説*/
}

@keyframes jump{
  0% {
    background-color: #6b5;
    box-shadow: 0 0.5vw #593;
    border-color: #6b5;
  }
  25% {
    background-color: #0b7;
    box-shadow: 0 0.5vw #095;
    border-color: #0b7;
  }
  50% {
    background-color: #c83;
    box-shadow: 0 0.5vw #a61;
    border-color: #c83;
    transform: translate(0, -1vw)
  }
  75% {
    background-color: #cc0;
    box-shadow: 0 0.5vw #aa0;
    border-color: #cc0;
  }
  100% {
    background-color: #6b5;
    box-shadow: 0 0.5vw #593;
    border-color: #6b5;
  }
}

/*文字を選択禁止&文字のマウスイベントを止める*/
.letter {
  user-select: none;
  pointer-events: none;
}
</style>
