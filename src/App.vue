<script setup>
import panel from './components/panel.vue'
import one_frame from './components/1-frame.vue'
import { reactive, ref, onMounted } from 'vue'

//selectingIndex:= [-1] -> 何も選択していない , [0, 1, 2 ... -> その番号を選択中
let selectingIndex = -1

//残り操作回数(初期設定:15)
const swapsRemain = ref(15)

//パネル１つ当たりのサイズ
const panelSize = ref((window.innerWidth*0.11).toFixed())

//答えの文字配列
const answerLetter = [["あ","い","う","あ","あ"],
                      ["あ", "_","え", "_","あ"],
                      ["あ","あ","お","あ","あ"],
                      ["あ", "_","あ", "_","あ"],
                      ["あ","あ","あ","あ","あ"]]

/*フレームに渡すprops群

  x:=左からのマス目(0-indexed)
  y:=上からのマス目(0-indexed)
  index:=クリック時にイベントに渡す番号
*/
const one_panels = reactive([{x:0, y:0, index: 0},{x:1, y:0, index: 1},{x:2, y:0, index: 2}, {x:3, y:0, index: 3},{x:4, y:0, index: 4},
                             {x:0, y:1, index: 5},                     {x:2, y:1, index: 7},                      {x:4, y:1, index: 9},
                             {x:0, y:2, index:10},{x:1, y:2, index:11},{x:2, y:2, index:12}, {x:3, y:2, index:13},{x:4, y:2, index:14},
                             {x:0, y:3, index:15},                     {x:2, y:3, index:17},                      {x:4, y:3, index:19},
                             {x:0, y:4, index:20},{x:1, y:4, index:21},{x:2, y:4, index:22}, {x:3, y:4, index:23},{x:4, y:4, index:24}])


/*各パネルに渡すprops群

  letter:=表示される文字
  x:=左からのマス目(float)
  y:=上からのマス目(float)
  x_offset:=クリックしたx座標とパネルのx座標の差分
  y_offset:=クリックしたy座標とパネルのy座標の差分
  status:=パネルの状態(hit/blow/miss/gameover/gameclear)
  selected:=現在選択中かどうか
  size:=パネルのサイズ
*/
const panel_data = reactive([
  {
    letter: "あ",
    x: 0,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panelSize

  },
  {
    letter: "い",
    x: 1,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panelSize

  },
  {
    letter: "う",
    x: 2,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panelSize

  },
  {
    letter: "え",
    x: 3,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panelSize

  },
  {
    letter: "お",
    x: 4,
    y: 0,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panelSize

  },
  {
    letter: "か",
    x: 0,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panelSize

  },
  {
    letter: "き",
    x: 1,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "nothing",
    selected: false,
    size: panelSize,

  },
  {
    letter: "く",
    x: 2,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panelSize

  },
  {
    letter: "け",
    x: 3,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "nothing",
    selected: false,
    size: panelSize,

  },
  {
    letter: "こ",
    x: 4,
    y: 1,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panelSize

  },
  {
    letter: "さ",
    x: 0,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panelSize

  },
  {
    letter: "し",
    x: 1,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panelSize

  },
  {
    letter: "す",
    x: 2,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panelSize

  },
  {
    letter: "せ",
    x: 3,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panelSize

  },
  {
    letter: "そ",
    x: 4,
    y: 2,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panelSize

  },
  {
    letter: "た",
    x: 0,
    y: 3,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panelSize

  },
  {
    letter: "ち",
    x: 1,
    y: 3,
    x_offset: 0,
    y_offset: 0,
    status: "nothing",
    selected: false,
    size: panelSize,

  },
  {
    letter: "つ",
    x: 2,
    y: 3,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panelSize

  },
  {
    letter: "て",
    x: 3,
    y: 3,
    x_offset: 0,
    y_offset: 0,
    status: "nothing",
    selected: false,
    size: panelSize,

  },
  {
    letter: "そ",
    x: 4,
    y: 3,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panelSize

  },
  {
    letter: "な",
    x: 0,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panelSize

  },
  {
    letter: "に",
    x: 1,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panelSize

  },
  {
    letter: "ぬ",
    x: 2,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "blow",
    selected: false,
    size: panelSize

  },
  {
    letter: "ね",
    x: 3,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "miss",
    selected: false,
    size: panelSize

  },
  {
    letter: "の",
    x: 4,
    y: 4,
    x_offset: 0,
    y_offset: 0,
    status: "hit",
    selected: false,
    size: panelSize

  },
])


//画面上でマウスが移動した場合の挙動
function updateOnMouseMove(event)
{
  //選択中なら選択中のパネルを動かす
  if(selectingIndex>-1){
    //propsとして渡している(x,y)をマウスに合わせて変更
    //offsetを埋め合わせて常にパネルの同じ部分をつかめるようにする
    panel_data[selectingIndex].x = event.x/this.panelSize - panel_data[selectingIndex].x_offset
    panel_data[selectingIndex].y = event.y/this.panelSize - panel_data[selectingIndex].y_offset
  }
}

//パネル上でマウスを押下した場合の挙動
function startOnMouseDown(event, index) {
  if(selectingIndex>-1 || panel_data[index].status=="hit" || panel_data[index].status=="gameclear" || panel_data[index].status=="gameover") return
  console.log(index)
  selectingIndex = index
  //selectingIndex を 選択中にする
  selectingIndex = index

  //パネルとカーソルの間の差分
  
  panel_data[selectingIndex].x_offset = (event.x/this.panelSize - panel_data[selectingIndex].x)
  panel_data[selectingIndex].y_offset = (event.y/this.panelSize - panel_data[selectingIndex].y)
  
  panel_data[selectingIndex].selected = true
}


//TODO:もっといい命名を行い、コメントを足す
function checkAnswer(){
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) if(answerLetter[i][j]!="_"){
    if(answerLetter[i][j]==panel_data[5*i+j].letter){
      answerLetter[i][j] = "_"
      panel_data[5*i+j].status = "hit"
    }
  }
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) if(answerLetter[i][j]!="_"){
    const is_blow = false;
    if(j%2==0) for(let k=0; k<5; k++) if(!this.is_blow && answerLetter[k][j]==panel_data[5*i+j].letter){
      this.is_blow = true
      break
    }
    if(i%2==0) for(let k=0; k<5; k++) if(!this.is_blow && answerLetter[i][k]==panel_data[5*i+j].letter){
      this.is_blow = true
      break
    }
    if(this.is_blow) panel_data[5*i+j].status = "blow" 
    else panel_data[5*i+j].status = "miss"
    this.is_blow = false
  }
}

//TODO:もっといい命名を行い、コメントを足す
function checkGameClear()
{
  let flag = true;
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) {
    if(answerLetter[i][j]!='_') flag = false;
  }
  if(flag) for(let i=0; i<5; i++) for(let j=0; j<5; j++) {
    if(i%2==0 || j%2==0) panel_data[5*i+j].status = "gameclear"
  }
  return flag
}

//TODO:もっといい命名を行い、コメントを足す
function checkGameOver()
{
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) {
    if(i%2==0 || j%2==0) panel_data[5*i+j].status = "gameover"
  }
}

//TODO:もっといい命名を行い、コメントを足す
function endOnMouseUp(event, index) {
  if(selectingIndex==-1) return
  if(panel_data[index].status == "hit")
  {
    console.log("!")
    this.otherarea(event)
    return 
  }
  if(index==selectingIndex){
    this.otherarea(event)
    console.log("same panel")
    return
  }
  swapsRemain.value--;
  //選択中を外す
  panel_data[selectingIndex].selected = false
  //選択中のインデックスと座標を用いて操作を実行

  panel_data[selectingIndex].x = event.x/this.panelSize - panel_data[selectingIndex].x_offset 
  panel_data[selectingIndex].y = event.y/this.panelSize - panel_data[selectingIndex].y_offset 

  panel_data[selectingIndex].x = index%5
  panel_data[selectingIndex].y = (index-index%5)/5

  console.log("changed!!")
  panel_data[index].x = selectingIndex%5
  panel_data[index].y = (selectingIndex-selectingIndex%5)/5

  const temp = panel_data[index]
  panel_data[index] = panel_data[selectingIndex]
  panel_data[selectingIndex] = temp

  selectingIndex = -1

  this.checkAnswer();

  if(!this.checkGameClear() && swapsRemain.value==0)this.checkGameOver();
}

//TODO:もっといい命名を行い、コメントを足す
//選択していないときの例外処理追加
function otherarea(event) {
  console.log("other area is startOnMouseDown")
  console.log(selectingIndex, selectingIndex)
  //選択中を外す
  panel_data[selectingIndex].selected = false
  //選択中のインデックスと座標を用いて操作を実行
  panel_data[selectingIndex].x = selectingIndex%5
  panel_data[selectingIndex].y = (selectingIndex-selectingIndex%5)/5
  selectingIndex = -1
}

//TODO:変数参照のバグを直して、マウント時にパネルの状態を変更する関数を生成
onMounted(() => {
  console.log("mounted");
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
    <div class="frame" @mousemove="updateOnMouseMove($event)" @mouseup="otherarea($event)">
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
            :size="panelSize"
            @mousedown="startOnMouseDown($event, ops.index)"
            @mouseup.stop="endOnMouseUp($event, ops.index)"
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
        <div class="counter">{{swapsRemain}} swaps remaining</div>
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
