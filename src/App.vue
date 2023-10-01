<script setup>
import panel from './components/panel.vue'
import archive from './icons/archive.vue'
import one_frame from './components/1-frame.vue'
import statistics from './icons/statistics.vue'
import { reactive, ref, onMounted } from 'vue'

//selectingIndex:= [-1] -> 何も選択していない , [0, 1, 2 ... -> その番号を選択中
let selectingIndex = -1

//残り操作回数(初期設定:15)
const swapsRemain = ref(15)

//パネル１つ当たりのサイズ
const panelSize = ref((window.innerWidth*0.11).toFixed())

/*答えの文字配列

"_":すでに一致しているorパネルが存在しない
"[あ-ん]":その文字が答え
*/
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
const onePanels = reactive([{x:0, y:0, index: 0},{x:1, y:0, index: 1},{x:2, y:0, index: 2}, {x:3, y:0, index: 3},{x:4, y:0, index: 4},
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

    console.log()

    panel_data[selectingIndex].x = event.x/(window.innerWidth*0.11).toFixed() - panel_data[selectingIndex].x_offset
    panel_data[selectingIndex].y = event.y/(window.innerWidth*0.11).toFixed() - panel_data[selectingIndex].y_offset
  }
}

//パネル上でマウスを押下した場合の挙動
function startOnMouseDown(event, index) {
  
  //すでに選択中or干渉できない状態の場合何もしない
  if(selectingIndex>-1 || panel_data[index].status=="hit" || panel_data[index].status=="gameclear" || panel_data[index].status=="gameover") return

  //selectingIndex を 選択中にする
  selectingIndex = index
  panel_data[selectingIndex].selected = true
  
  //パネルとカーソルの間の差分更新
  panel_data[selectingIndex].x_offset = (event.x/(window.innerWidth*0.11).toFixed() - panel_data[selectingIndex].x)
  panel_data[selectingIndex].y_offset = (event.y/(window.innerWidth*0.11).toFixed() - panel_data[selectingIndex].y)
}


//答えに合わせてパネルのhit/blow/missを切り替え
function checkAnswer(){

  //ゲームクリアしているならゲームクリア状態に変更
  if(this.checkGameClear()){
    if(flag) for(let i=0; i<5; i++) for(let j=0; j<5; j++) {
      if(i%2==0 || j%2==0) panel_data[5*i+j].status = "gameclear"
    }
    return
  }
  //ゲームオーバーならゲームオーバー状態に変更
  if(this.checkGameOver())
  {
    for(let i=0; i<5; i++) for(let j=0; j<5; j++) {
      console.log(i,j)
      if(i%2==0 || j%2==0) panel_data[5*i+j].status = "gameover"
    }
    return
  }

  //答えと一致するならhitに変更
  for(let i=0; i<5; i++) for(let j=0; j<5; j++){
    if(answerLetter[i][j]!="_" && answerLetter[i][j]==panel_data[5*i+j].letter){
      answerLetter[i][j] = "_"
      panel_data[5*i+j].status = "hit"
    }
  }

  //各行と列に答えのパネルがあるならblowに変更
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) if(answerLetter[i][j]!="_"){
    //blowかどうか
    let is_blow = false;
    
    //行を見る
    if(j%2==0) for(let ki=0; ki<5; ki++) if(!is_blow && answerLetter[ki][j]==panel_data[5*i+j].letter){
      is_blow = true
      console.log(panel_data[5*i+j].letter)
      break
    }
    //列を見る
    if(i%2==0) for(let kj=0; kj<5; kj++) if(!is_blow && answerLetter[i][kj]==panel_data[5*i+j].letter){
      is_blow = true
      break
    }
    //is_blowならばblowに変更
    //そうでないならばmissに変更
    if(is_blow) panel_data[5*i+j].status = "blow" 
    else panel_data[5*i+j].status = "miss"
    is_blow = false
  }
}

//TODO: check と update を分ける
function checkGameClear()
{
  let flag = true;
  for(let i=0; i<5; i++) for(let j=0; j<5; j++) {
    if(answerLetter[i][j]!='_') flag = false;
  }
  return flag
}

//checkではなく演出なので命名変更
function checkGameOver()
{
  return swapsRemain.value==0 && !this.checkGameClear();
}

//マウスボタンを上げたときの挙動
function endOnMouseUp(event, index) {
  //選択していないなら何もしない
  if(selectingIndex==-1) return

  //正解のパネルと入れ替えようとした場合何もしない
  if(panel_data[index].status == "hit")
  {
    console.log("!")
    this.doNothing(event)
    return 
  }
  //視点と終点が同じパネルだった場合何もしない
  if(index==selectingIndex){
    this.doNothing(event)
    console.log("same panel")
    return
  }

  //異なるパネルであった場合
  //交換残り回数を減らす
  swapsRemain.value--;
    
  //選択中を外す
  panel_data[selectingIndex].selected = false

  //selectingIndexとindexで位置を変更
  panel_data[selectingIndex].x = index%5
  panel_data[selectingIndex].y = (index-index%5)/5

  panel_data[index].x = selectingIndex%5
  panel_data[index].y = (selectingIndex-selectingIndex%5)/5

  //格納位置を入れ替え
  const temp = panel_data[index]
  panel_data[index] = panel_data[selectingIndex]
  panel_data[selectingIndex] = temp

  //選択中のインデックスを直す
  selectingIndex = -1

  //答え合わせ
  this.checkAnswer();
}

//同じパネルを入れ替えor外を選択した場合に何もせず選択状態をデフォルトにする
function doNothing(event) {
  if(selectingIndex == -1) return
  //選択中を外す
  panel_data[selectingIndex].selected = false

  //選択中のインデックスと座標を用いて操作を実行
  panel_data[selectingIndex].x = selectingIndex%5
  panel_data[selectingIndex].y = (selectingIndex-selectingIndex%5)/5

  //選択中のインデックスを直す
  selectingIndex = -1
}
</script>

<template>
  <!--googlefont読み込み-->
    <head>
      <link rel="preconnect" href="https://fonts.googleapis.com">
      <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
      <link href="https://fonts.googleapis.com/css2?family=M+PLUS+Rounded+1c:wght@700&family=Onest:wght@900&display=swap" rel="stylesheet">
    </head>
    <body>
    <!--画面全体を覆うトラッカー:マウスの位置を画面全体で検知-->
    <div class="mouse-tracker" @mousemove="updateOnMouseMove($event)" @mouseup="doNothing($event)">
      <!--コンテナ:横幅を制限することで画面サイズに対応-->
      <div class="container">
        <div class="title">waffle JP</div>
        <archive/>
        <statistics/>
        <!--問題ID:-->
        <div class="problemID">daily waffle #001</div>

        <!--ゲーム盤面-->
        <div class="game-board">
          <!--フレーム:入力がどのパネルで行われたかを検知しindexを渡す-->
          <one_frame v-for="(ops, index) in onePanels"
            :position="{x:ops.x, y:ops.y}"
            :size="panelSize.value"
            @mousedown="startOnMouseDown($event, ops.index)"
            @mouseup.stop="endOnMouseUp($event, ops.index)"
          />
          <!--各パネル:パネルの表示-->
          <panel v-for="(pd, index) in panel_data" 
            :letter="pd.letter" 
            :status="pd.status"
            :position="{x:pd.x, y:pd.y}"
            :offset="{x_offset:pd.x_offset, y_offset:pd.y_offset}"
            :isselect="pd.selected"
            :size="panelSize.value"
          />
        </div>
        <!--残り操作回数カウンター-->
        <div class="swap-remain-counter">{{swapsRemain}} swaps remaining</div>
      </div>
    </div>
  </body>
</template>


<style scoped>
.mouse-tracker {
  position: absolute;
  top: 0;
  left: 0;
  width:100%;
  height:100%;
  
  background-color: #eee;
}
.container {
  position: absolute;
  top: 0;

  width: min(70vw, 70vh);
  height: min(90vw, 100vh);

  background-color: white;
}
.title {
  font-size: 5vw;
}
.problemID {
  margin: 5vw;
  font-size: 3vw;
}
.swap-remain-counter {
  margin: 3vw;
  font-size: 3vw;
}
.game-board {
  position: relative;
  width: 54.1vw;
  height: 54.6vw;
}
</style>
