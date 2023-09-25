<script setup>
import panel from './components/panel.vue'
import { reactive, ref } from 'vue'

var selectedindex = ref(0)
var isselected = ref(false)

const panel_data = reactive([
  {
    letter: "あ",
    x: 0,
    y: 100,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false
  },
  {
    letter: "い",
    x: 0,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false
  },
  {
    letter: "う",
    x: 0,
    y: 200,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false
  }
])

//パネル上でマウスを押したときの動作
function mousedown(event, index)
{
  //selectedindex を 選択中にする
  this.selectedindex = index
  this.isselected = true

  //パネルとカーソルの間の差分
  this.panel_data[this.selectedindex].x_offset = event.x - this.panel_data[this.selectedindex].x
  this.panel_data[this.selectedindex].y_offset = event.y - this.panel_data[this.selectedindex].y

  this.panel_data[this.selectedindex].selected = true
}
//パネル上でマウスを離したときの動作
function mouseup(event, index)
{
  //選択中を外す
  this.isselected = false
  this.panel_data[this.selectedindex].selected = false
  //選択中のインデックスと座標を用いて操作を実行
}
//画面上でマウスを操作した際の動作
function mousemove(event)
{
  //選択中なら選択中のパネルを動かす
  if(this.isselected){
    //propsとして渡している(x,y)をマウスに合わせて移動
    //パネルとカーソルの間の差分を埋め合わせて常にパネルの同じ部分をつかめるようにする
    this.panel_data[this.selectedindex].x = event.x - this.panel_data[this.selectedindex].x_offset
    this.panel_data[this.selectedindex].y = event.y - this.panel_data[this.selectedindex].y_offset
  }
}
</script>

<template>
  <header>
  </header>
  <main>
    <!--画面全体を覆うフレーム:マウスの位置を常に検知するために使用-->
    <div class="frame" @mousemove="mousemove($event)">
      <!--各パネル:配列上のデータをpropsとして渡す-->
      <panel v-for="(pd, index) in panel_data" 
        :letter="pd.letter" 
        :status="pd.status"
        :position="{x:pd.x, y:pd.y}"
        :offset="{x_offset:pd.x_offset, y_offset:pd.y_offset}"
        :isselect="pd.selected"
        @mousedown="mousedown($event,index)"
        @mouseup="mouseup($event,index)"
      />
    </div>
  </main>
</template>

<style scoped>
.frame {
  position: absolute;
  top: 0;
  left: 0;
  width:100%;
  height:100%;
  background-color: aquamarine;
}

</style>
