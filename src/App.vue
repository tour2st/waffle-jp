<script setup>
import panel from './components/panel.vue'
import one_frame from './components/1-frame.vue'
import { reactive, ref } from 'vue'

const selectedindex = ref(0)
const isselected = ref(false)

const start = reactive({
  x: 0,
  y: 0
})

const start_panel = -1;
const end_panel = -1;

const one_panels = reactive([{x:0, y:0, index: 0},{x:1, y:0, index: 1},{x:2, y:0, index: 2}, {x:3, y:0, index: 3},{x:4, y:0, index: 4},
                             {x:0, y:1, index: 5},                     {x:2, y:1, index: 7},                      {x:4, y:1, index: 9},
                             {x:0, y:2, index:10},{x:1, y:2, index:11},{x:2, y:2, index:12}, {x:3, y:2, index:13},{x:4, y:2, index:14},
                             {x:0, y:3, index:15},                     {x:2, y:3, index:17},                      {x:4, y:3, index:19},
                             {x:0, y:4, index:20},{x:1, y:4, index:21},{x:2, y:4, index:22}, {x:3, y:4, index:23},{x:4, y:4, index:24}])

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
  },
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
  },
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
  },
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
  },
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
  },
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
  },
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
  },
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

  this.start.x = this.panel_data[this.selectedindex].x
  this.start.y = this.panel_data[this.selectedindex].y

}
//パネル上でマウスを離したときの動作
function mouseup(event, index)
{
  console.log("mouseuped outher roam")
  //選択中を外す
  this.isselected = false
  this.panel_data[this.selectedindex].selected = false
  //選択中のインデックスと座標を用いて操作を実行

  this.panel_data[this.selectedindex].x = event.x - this.panel_data[this.selectedindex].x_offset
  this.panel_data[this.selectedindex].y = event.y - this.panel_data[this.selectedindex].y_offset

  this.panel_data[this.selectedindex].x -= this.panel_data[this.selectedindex].x % (window.innerWidth*0.11).toFixed()
  this.panel_data[this.selectedindex].y -= this.panel_data[this.selectedindex].y % (window.innerWidth*0.11).toFixed()
  this.panel_data[this.selectedindex].x += (window.innerWidth*0.11).toFixed()/2
  this.panel_data[this.selectedindex].y += (window.innerWidth*0.11).toFixed()/2

}
//画面上でマウスを操作した際の動作
function mousemove(event)
{
  console.log(event.x/this.panel_size, event.y/this.panel_size)
  //選択中なら選択中のパネルを動かす
  if(this.isselected){
    //propsとして渡している(x,y)をマウスに合わせて移動
    //パネルとカーソルの間の差分を埋め合わせて常にパネルの同じ部分をつかめるようにする
    this.panel_data[this.selectedindex].x = event.x - this.panel_data[this.selectedindex].x_offset
    this.panel_data[this.selectedindex].y = event.y - this.panel_data[this.selectedindex].y_offset
  }
}

function clicked(event, index) {
  console.log(index)
  this.start_panel = index
}

function uped(event, index) {
  console.log(index)
  this.end_panel = index
  console.log("change" + this.start_panel + " " + this.end_panel)
}

function otherclicked(event) {
  console.log("other area is clicked")
}

const panel_size = ref((window.innerWidth*0.11).toFixed())

</script>

<template>
  <header>
  </header>
  <main>
    <!--画面全体を覆うフレーム:マウスの位置を常に検知するために使用-->
    <div class="frame" @mousemove="mousemove($event)" @click="otherclicked($event)">
      <one_frame v-for="(ops, index) in one_panels"
        :position="{x:ops.x, y:ops.y}"
        :size="panel_size"
        @mousedown="clicked($event, ops.index)"
        @mouseup="uped($event, ops.index)"
      />
      <!--各パネル:配列上のデータをpropsとして渡す
      <panel v-for="(pd, index) in panel_data" 
        :letter="pd.letter" 
        :status="pd.status"
        :position="{x:pd.x, y:pd.y}"
        :offset="{x_offset:pd.x_offset, y_offset:pd.y_offset}"
        :isselect="pd.selected"
        @mousedown="mousedown($event,index)"
        @mouseup="mouseup($event,index)"
      />-->
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
