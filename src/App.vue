<script setup>
import panel from './components/panel.vue'
import one_frame from './components/1-frame.vue'
import { reactive, ref, onMounted } from 'vue'

//TODO: selectedindex:= -1->何も選択していない , 0- ->その番号を選択で一致させる
let selectedindex = 0
let isselected = false

//TODO:もっといい命名を
const count = ref(15)

//TODO:もっといい命名を
const start = {
  x: 0,
  y: 0
}

//TODO:これを用いて途中で画面サイズを変更した際のバグと操作がズレるバグを直す
const panel_size = ref((window.innerWidth*0.11).toFixed())

const answer = [["あ","い","う","あ","あ"],
                ["あ","_","え","_","あ"],
                ["あ","あ","お","あ","あ"],
                ["あ","_","あ","_","あ"],
                ["あ","あ","あ","あ","あ"]]

//TODO:hiddenフラグを追加してクリア時に枠を消す
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
    x: 3,
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
    y: 3,
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
    x: 3,
    y: 3,
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

//TODO:もっといい命名を行い、コメントを足す
//画面上でマウスを操作した際の動作
function mousemove(event)
{
  //選択中なら選択中のパネルを動かす
  if(isselected){
    //propsとして渡している(x,y)をマウスに合わせて移動
    //パネルとカーソルの間の差分を埋め合わせて常にパネルの同じ部分をつかめるようにする
    panel_data[selectedindex].x = event.x/this.panel_size - panel_data[selectedindex].x_offset
    panel_data[selectedindex].y = event.y/this.panel_size - panel_data[selectedindex].y_offset
  }
}

//TODO:もっといい命名を行い、コメントを足す
function clicked(event, index) {
  if(isselected || panel_data[index].status=="hit" || panel_data[index].status=="gameclear" || panel_data[index].status=="gameover") return
  console.log(index)
  selectedindex = index
  //selectedindex を 選択中にする
  selectedindex = index
  isselected = true

  //パネルとカーソルの間の差分
  
  panel_data[selectedindex].x_offset = (event.x/this.panel_size - panel_data[selectedindex].x)
  panel_data[selectedindex].y_offset = (event.y/this.panel_size - panel_data[selectedindex].y)
  
  panel_data[selectedindex].selected = true

  start.x = panel_data[selectedindex].x
  start.y = panel_data[selectedindex].y
}


//TODO:もっといい命名を行い、コメントを足す
function check_answer(){
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) if(answer[i][j]!="_"){
    if(answer[i][j]==panel_data[5*i+j].letter){
      answer[i][j] = "_"
      panel_data[5*i+j].status = "hit"
    }
  }
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) if(answer[i][j]!="_"){
    const is_blow = false;
    if(j%2==0) for(let k=0; k<5; k++) if(!this.is_blow && answer[k][j]==panel_data[5*i+j].letter){
      this.is_blow = true
      break
    }
    if(i%2==0) for(let k=0; k<5; k++) if(!this.is_blow && answer[i][k]==panel_data[5*i+j].letter){
      this.is_blow = true
      break
    }
    if(this.is_blow) panel_data[5*i+j].status = "blow" 
    else panel_data[5*i+j].status = "miss"
    this.is_blow = false
  }
}

//TODO:もっといい命名を行い、コメントを足す
function is_clear()
{
  let flag = true;
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) {
    if(answer[i][j]!='_') flag = false;
  }
  if(flag) for(let i=0; i<5; i++) for(let j=0; j<5; j++) {
    if(i%2==0 || j%2==0) panel_data[5*i+j].status = "gameclear"
  }
  return flag
}

//TODO:もっといい命名を行い、コメントを足す
function is_game_over()
{
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) {
    if(i%2==0 || j%2==0) panel_data[5*i+j].status = "gameover"
  }
}

//TODO:もっといい命名を行い、コメントを足す
function uped(event, index) {
  if(isselected==false) return
  if(panel_data[index].status == "hit")
  {
    console.log("!")
    this.otherclicked(event)
    return 
  }
  console.log(index)
  this.end_panel = index
  console.log("change" + selectedindex + " " + this.end_panel)
  if(this.end_panel==selectedindex){
    this.otherclicked(event)
    console.log("same panel")
    return
  }
  count.value--;
  //選択中を外す
  isselected = false
  panel_data[selectedindex].selected = false
  //選択中のインデックスと座標を用いて操作を実行

  panel_data[selectedindex].x = event.x/this.panel_size - panel_data[selectedindex].x_offset 
  panel_data[selectedindex].y = event.y/this.panel_size - panel_data[selectedindex].y_offset 

  panel_data[selectedindex].x = panel_data[selectedindex].x.toFixed()
  panel_data[selectedindex].y = panel_data[selectedindex].y.toFixed()

  console.log("changed!!")
  panel_data[index].x = start.x
  panel_data[index].y = start.y

  const temp = panel_data[index]
  panel_data[index] = panel_data[selectedindex]
  panel_data[selectedindex] = temp

  this.check_answer();

  if(!this.is_clear() && count.value==0)this.is_game_over();
}

//TODO:もっといい命名を行い、コメントを足す
function otherclicked(event) {
  console.log("other area is clicked")
  console.log(selectedindex, selectedindex)
  //選択中を外す
  isselected = false
  panel_data[selectedindex].selected = false
  //選択中のインデックスと座標を用いて操作を実行

  panel_data[selectedindex].x = start.x
  panel_data[selectedindex].y = start.y
}

//TODO:変数参照のバグを直して、マウント時にパネルの状態を変更する関数を生成
onMounted(() => {
  console.log("mounted")
})
</script>

<template>
  <!--コメントを足す　&& アイコン部分は子コンポーネントに分割-->
    <head>
      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=M+PLUS+Rounded+1c:wght@700&family=Onest:wght@900&display=swap" rel="stylesheet">
    </head>
  <header>
    <div class="title">waffle JP</div>
  </header>
  <main>
    <!--画面全体を覆うフレーム:マウスの位置を常に検知するために使用-->
    <div class="frame" @mousemove="mousemove($event)" @mouseup="otherclicked($event)">
      <div class="container">
        <div class="left_icon" href="archive.com">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-history" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M12 8l0 4l2 2"></path>
            <path d="M3.05 11a9 9 0 1 1 .5 4m-.5 5v-5h5"></path>
          </svg>
        </div>
        <div class="right_icon" href="archive.com">
          <svg xmlns="http://www.w3.org/2000/svg" class="icon icon-tabler icon-tabler-chart-bar" width="24" height="24" viewBox="0 0 24 24" stroke-width="2" stroke="currentColor" fill="none" stroke-linecap="round" stroke-linejoin="round">
            <path stroke="none" d="M0 0h24v24H0z" fill="none"></path>
            <path d="M3 12m0 1a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v6a1 1 0 0 1 -1 1h-4a1 1 0 0 1 -1 -1z"></path>
            <path d="M9 8m0 1a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v10a1 1 0 0 1 -1 1h-4a1 1 0 0 1 -1 -1z"></path>
            <path d="M15 4m0 1a1 1 0 0 1 1 -1h4a1 1 0 0 1 1 1v14a1 1 0 0 1 -1 1h-4a1 1 0 0 1 -1 -1z"></path>
            <path d="M4 20l14 0"></path>
          </svg>
        </div>
        <div class="problem">daily waffle #001</div>
        <div class="panels">
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
        <div class="counter">{{count}} swaps remaining</div>
      </div>
    </div>
  </main>
</template>


<style scoped>
/*base.cssを活用して記述量を全体的に減らす*/
/*もっと分かりやすいクラス名を使う*/
* {
  font-family: 'M PLUS Rounded 1c', sans-serif;
}

.icon {
  transform: scale(1.5,1.5);
  cursor: pointer;
}
.title {
  position: absolute;
  top: 0;
  left:50%;
  transform: translate(-50%,0);
  font-size: 5vw;
  z-index: 1;
}

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

.right_icon {
  position: absolute;
  top: 0;
  left: 100%;
  width: 10vw;
  height: 10vw;
  z-index: 1;
  transform: translate(-100%, 0);
  display: flex;
  align-items: center;
  justify-content: center;
}
.frame {
  position: absolute;
  top: 0;
  left: 0;
  width:100%;
  height:100%;
  background-color: #eee;
  justify-content: center;
  display: flex;
  flex-flow: column;
  align-items: center;
}

.panels {
  position: relative;
  width: 54.1vw;
  height: 54.6vw;
}

.problem {
  margin: 5vw;
  font-size: 3vw;
}

.counter {
  margin: 3vw;
  font-size: 3vw;
}

.container {
  background-color: white;
  display: flex;
  flex-flow: column;
  align-items: center;
  justify-content: center;
  
  position: absolute;
  top: 0px;

  width: 70vw;
  height: 90vw;
}
</style>
