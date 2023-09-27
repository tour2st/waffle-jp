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

const panel_size = ref((window.innerWidth*0.11).toFixed())

const answer = [["あ","い","う","あ","あ"],
                ["あ","_","え","_","あ"],
                ["あ","あ","お","あ","あ"],
                ["あ","_","あ","_","あ"],
                ["あ","あ","あ","あ","あ"]]
              
const one_panels = reactive([{x:0, y:0, index: 0},{x:1, y:0, index: 1},{x:2, y:0, index: 2}, {x:3, y:0, index: 3},{x:4, y:0, index: 4},
                             {x:0, y:1, index: 5},                     {x:2, y:1, index: 7},                      {x:4, y:1, index: 9},
                             {x:0, y:2, index:10},{x:1, y:2, index:11},{x:2, y:2, index:12}, {x:3, y:2, index:13},{x:4, y:2, index:14},
                             {x:0, y:3, index:15},                     {x:2, y:3, index:17},                      {x:4, y:3, index:19},
                             {x:0, y:4, index:20},{x:1, y:4, index:21},{x:2, y:4, index:22}, {x:3, y:4, index:23},{x:4, y:4, index:24}])

const panel_data = reactive([
  {
    letter: "あ",
    x: 0,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panel_size

  },
  {
    letter: "い",
    x: 1,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panel_size

  },
  {
    letter: "う",
    x: 2,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panel_size

  },
  {
    letter: "え",
    x: 3,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panel_size

  },
  {
    letter: "お",
    x: 4,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panel_size

  },
  {
    letter: "か",
    x: 0,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panel_size

  },
  {
    letter: "き",
    x: 1,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "nothing",
    selected: false,
    size: panel_size,

  },
  {
    letter: "く",
    x: 2,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panel_size

  },
  {
    letter: "け",
    x: 1,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "nothing",
    selected: false,
    size: panel_size,

  },
  {
    letter: "こ",
    x: 4,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panel_size

  },
  {
    letter: "さ",
    x: 0,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panel_size

  },
  {
    letter: "し",
    x: 1,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panel_size

  },
  {
    letter: "す",
    x: 2,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panel_size

  },
  {
    letter: "せ",
    x: 3,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panel_size

  },
  {
    letter: "そ",
    x: 4,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panel_size

  },
  {
    letter: "た",
    x: 0,
    y: 3,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panel_size

  },
  {
    letter: "ち",
    x: 1,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "nothing",
    selected: false,
    size: panel_size,

  },
  {
    letter: "つ",
    x: 2,
    y: 3,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panel_size

  },
  {
    letter: "て",
    x: 1,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "nothing",
    selected: false,
    size: panel_size,

  },
  {
    letter: "そ",
    x: 4,
    y: 3,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panel_size

  },
  {
    letter: "な",
    x: 0,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panel_size

  },
  {
    letter: "に",
    x: 1,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panel_size

  },
  {
    letter: "ぬ",
    x: 2,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panel_size

  },
  {
    letter: "ね",
    x: 3,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panel_size

  },
  {
    letter: "の",
    x: 4,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panel_size

  },
])

//パネル上でマウスを押したときの動作
function mousedown(event, index)
{
  //selectedindex を 選択中にする
  this.selectedindex = index
  this.isselected = true

  //パネルとカーソルの間の差分
  
  this.panel_data[this.selectedindex].x_offset = (event.x/this.panel_size - this.panel_data[this.selectedindex].x)
  this.panel_data[this.selectedindex].y_offset = (event.y/this.panel_size - this.panel_data[this.selectedindex].y)
  
  this.panel_data[this.selectedindex].selected = true

  this.start.x = this.panel_data[this.selectedindex].x
  this.start.y = this.panel_data[this.selectedindex].y

}

//画面上でマウスを操作した際の動作
function mousemove(event)
{
  //選択中なら選択中のパネルを動かす
  if(this.isselected){
    //propsとして渡している(x,y)をマウスに合わせて移動
    //パネルとカーソルの間の差分を埋め合わせて常にパネルの同じ部分をつかめるようにする
    this.panel_data[this.selectedindex].x = event.x/this.panel_size - this.panel_data[this.selectedindex].x_offset
    this.panel_data[this.selectedindex].y = event.y/this.panel_size - this.panel_data[this.selectedindex].y_offset
  }
}

function clicked(event, index) {
  if(this.isselected || this.panel_data[index].status=="hit") return
  console.log(index)
  this.start_panel = index
  //selectedindex を 選択中にする
  this.selectedindex = index
  this.isselected = true

  //パネルとカーソルの間の差分
  
  this.panel_data[this.selectedindex].x_offset = (event.x/this.panel_size - this.panel_data[this.selectedindex].x)
  this.panel_data[this.selectedindex].y_offset = (event.y/this.panel_size - this.panel_data[this.selectedindex].y)
  
  this.panel_data[this.selectedindex].selected = true

  this.start.x = this.panel_data[this.selectedindex].x
  this.start.y = this.panel_data[this.selectedindex].y
}

function uped(event, index) {
  if(this.isselected==false) return
  if(this.panel_data[index].status == "hit")
  {
    console.log("!")
    this.otherclicked(event)
    return 
  }
  console.log(index)
  this.end_panel = index
  console.log("change" + this.start_panel + " " + this.end_panel)
  //選択中を外す
  this.isselected = false
  this.panel_data[this.selectedindex].selected = false
  //選択中のインデックスと座標を用いて操作を実行

  this.panel_data[this.selectedindex].x = event.x/this.panel_size - this.panel_data[this.selectedindex].x_offset 
  this.panel_data[this.selectedindex].y = event.y/this.panel_size - this.panel_data[this.selectedindex].y_offset 

  this.panel_data[this.selectedindex].x = this.panel_data[this.selectedindex].x.toFixed()
  this.panel_data[this.selectedindex].y = this.panel_data[this.selectedindex].y.toFixed()

  console.log("changed!!")
  this.panel_data[index].x = this.start.x
  this.panel_data[index].y = this.start.y

  const temp = this.panel_data[index]
  this.panel_data[index] = this.panel_data[this.selectedindex]
  this.panel_data[this.selectedindex] = temp

  this.check_answer();
}

function otherclicked(event) {
  console.log("other area is clicked")
  console.log(this.start_panel)
  //選択中を外す
  this.isselected = false
  this.panel_data[this.selectedindex].selected = false
  //選択中のインデックスと座標を用いて操作を実行

  this.panel_data[this.selectedindex].x = this.start.x
  this.panel_data[this.selectedindex].y = this.start.y

}

function check_answer(){
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) if(this.answer[i][j]!="_"){
    if(this.answer[i][j]==this.panel_data[5*i+j].letter){
      console.log(i, j)
      console.log(this.panel_data[5*i+j].status)
      this.answer[i][j] = "_"
      this.panel_data[5*i+j].status = "hit"
    }
  }
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) if(this.answer[i][j]!="_"){
    console.log(i, j)
    const is_blow = false;
    for(let k=0; k<5; k++) if(!this.is_blow && this.answer[k][j]==this.panel_data[5*i+j].letter){
      this.is_blow = true
      break
    }
    for(let k=0; k<5; k++) if(!this.is_blow && this.answer[i][k]==this.panel_data[5*i+j].letter){
      this.is_blow = true
      break
    }
    if(this.is_blow) this.panel_data[5*i+j].status = "blow" 
    else this.panel_data[5*i+j].status = "miss"
    console.log(this.panel_data[5*i+j].status);
    this.is_blow = false
  }
}
</script>

<template>
  <header>
  </header>
  <main>
    <!--画面全体を覆うフレーム:マウスの位置を常に検知するために使用-->
    <div class="frame" @mousemove="mousemove($event)" @mouseup="otherclicked($event)">
      <one_frame v-for="(ops, index) in one_panels"
        :position="{x:ops.x, y:ops.y}"
        :size="panel_size"
        @mousedown="clicked($event, ops.index)"
        @mouseup.stop="uped($event, ops.index)"
      />
      <!--各パネル:配列上のデータをpropsとして渡す-->
      <panel v-for="(pd, index) in panel_data" 
        :letter="pd.letter" 
        :status="pd.status"
        :position="{x:pd.x, y:pd.y}"
        :offset="{x_offset:pd.x_offset, y_offset:pd.y_offset}"
        :isselect="pd.selected"
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
