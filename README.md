# 20230529
# 111-2第二個作業 5/29
## 產生多個元件(class)

## class的constructor定義內容
*將給定的**類**添加到元素。如果沒有傳入類參數，它會返回一個包含元素當前類的**字符串**。

### 執行後的圖片
![](https://hackmd.io/_uploads/rJ_GR5nH2.gif)

###### 實際的程式碼
```javascript=
class Obj {
  constructor() {
    this.p = createVector(random(width), random(height));
    this.v = createVector(random(-1, 1), random(-1, 1));
    this.size = random(5, 10);
    this.color = random(colors);
    this.stroke = random(stroke_colors);
  }

```

## class的畫圖程式碼
### 執行後的圖片![](https://hackmd.io/_uploads/HyhV0c3B2.gif)
###### 實際的程式碼
```javascript=
let points = [[-2,0],[-2.5,-1],[-3,-2],[-3.5,-4],[-4,-6],[0,-6],[2,-5],[2.5,-3.5],[3,-2],[4,0],[5,2],[2,5],[2,3],[0,2],[-0.5,0],[-2,2],[-4,2],[-2,0],[-2,0]];
var stroke_colors = "2d00f7-6a00f4-8900f2-a100f2-b100e8-bc00dd-d100b6-e500a4-f20089".split("-").map(a => "#" + a);
var fill_colors = "ffbf8cc-fde4cf-ffcfd2-f1c0e8-cfbaf0".split("-").map(a => "#" + a);

class Obj {
  constructor() {//宣告物件的基本資料
  }
  draw() {
}

var ball
var balls=[]

function setup() {
  createCanvas(windowWidth, windowHeight);
}

function draw() {
  background(220);
}
```

## class的移動內容
### 執行後的圖片
![](https://hackmd.io/_uploads/BkFieo2rh.gif)
###### 實際的程式碼
```javascript=
update(){
    this.p.x=this.p.x+this.v.x
    this.p.y=this.p.y+this.v.y
}

```

## 產生20個相同class的元件
### 執行後的圖片
![](https://hackmd.io/_uploads/S1EC3c2r3.gif)
###### 實際的程式碼
```javascript=

function setup() {
  createCanvas(windowWidth, windowHeight);

  for (let j = 0; j < 20; j++) {
    ball = new Obj({});
    balls.push(ball);
  }

}

function draw() {
  background(220);
  for (var k=0;k<balls.length;k=k+1) {
    ball = balls[k]
    ball.draw();
    ball.update();
  }
}
```

## 元件的大小，元件的左右移動，速度不一
### 執行後的圖片
![](https://hackmd.io/_uploads/SyYqfj2rn.gif)
###### 實際的程式碼
```javascript=
push();
    translate(this.p.x, this.p.y);
    scale((this.v.x<0?1:-1),-1);
    stroke(this.stroke);
    fill(this.color);
    beginShape();
    for (let i = 0; i < points.length; i++) {
      vertex(points[i][0] * this.size, points[i][1] * this.size);
    }
    endShape(CLOSE);
    
    pop();
```

## 發射子彈
### 執行後的圖片
![](https://hackmd.io/_uploads/H1a5T4JL2.gif)
###### 實際的程式碼
```javascript=
function preload(){  //最早執行的程式碼
  ohno_sound = loadSound("sound/ohno.wav")
  bullet_sound = loadSound("sound/Launching wire.wav")
 
}

var bullet  //飛彈物件
var bullets=[]

function draw() {  //每秒會執行60次次
  background(220);
    for(let bullet of bullets){    if(ball.isBallInRanger(bullet.p.x,bullet.p.y))  //判斷ball與bullet有沒有碰觸
      {
        score = score - 1     //分數扣一
        ohno_sound.play()
        balls.splice(balls.indexOf(ball),1)         //讓幽靈從幽靈倉庫內移除
        bullets.splice(bullets.indexOf(bullet),1)   //讓飛彈從飛彈倉庫內移除
      }
    }
  }


  for(let bullet of bullets){  //針對飛彈倉庫內的資料，一筆一筆的顯示出來
    bullet.draw()
    bullet.update()
  }

  for(let monster of monsters){  //針對怪物倉庫內的資料，一筆一筆的顯示出來
    if(monster.IsDead && monster.timenum>=6){
      monsters.splice(monsters.indexOf(monster),1) //讓怪物從怪物資料倉庫內移除
    }
    monster.draw()
    monster.update()
 
    for(let bullet of bullets){
      if(monster.isBallInRanger(bullet.p.x,bullet.p.y))  //判斷monster與bullet有沒有碰觸
      {
        score = score + 1     //分數加一
        // elephant_sound.play()
        // monsters.splice(monsters.indexOf(monster),1)   //讓怪物從怪物資料倉庫內移除
        monster.IsDead = true //已經被打到了，準備執行爆炸後的畫面
        bullets.splice(bullets.indexOf(bullet),1)   //讓飛彈從飛彈倉庫內移除
      }
    }

function mousePressed(){

  //新增(產生)一筆飛彈資料(還沒有顯示)
  bullet  = new Bullet({})
  bullets.push(bullet)  //把這一筆資料放入飛彈倉庫
  bullet_sound.play()
}

function keyPressed(){
  if(key==" "){
    //新增(產生)一筆飛彈資料(還沒有顯示)
    bullet  = new Bullet({})
    bullets.push(bullet)  //把這一筆資料放入飛彈倉庫
    bullet_sound.play()
  }  

}


```

## 物件消失不見
### 執行後的圖片
![](https://hackmd.io/_uploads/SyxVW1HkUh.gif)
###### 實際的程式碼
```javascript=
function draw() {  //每秒會執行60次次
  background(220);

  if(keyIsPressed){  //鍵盤是否被按下，如果有鍵盤被按下，keyPressed的值為true
    if(key=="ArrowLeft" || key=="a"){  //按下鍵盤的往左鍵
      shipP.x = shipP.x-5
    }
    if(key=="ArrowRight" || key=="d"){  //按下鍵盤的往右鍵
      shipP.x = shipP.x+5
    }
    if(key=="ArrowUp" || key=="w"){  //按下鍵盤的往上鍵
      shipP.y = shipP.y-5
    }
    if(key=="ArrowDown" || key=="s"){  //按下鍵盤的往下鍵
      shipP.y = shipP.y+5
    }    
  }
  for(let ball of balls){  //針對陣列變數，取出陣列內一個一個的物件
    ball.draw()
    ball.update()
    //+++++++++++++++由此判斷，每隻幽靈有沒有接觸每一個飛彈++++++++++++++++++++++
    for(let bullet of bullets){
      if(ball.isBallInRanger(bullet.p.x,bullet.p.y))  //判斷ball與bullet有沒有碰觸
      {
        score = score - 1     //分數扣一
        ohno_sound.play()
        balls.splice(balls.indexOf(ball),1)         //讓幽靈從幽靈倉庫內移除
        bullets.splice(bullets.indexOf(bullet),1)   //讓飛彈從飛彈倉庫內移除
      }
    }
  }

```

## 計算得分
### 執行後的圖片
![](https://hackmd.io/_uploads/rkX5kryLh.gif)
###### 實際的程式碼
```javascript=
 score = score - 1     //分數扣一
 score = score + 1     //分數加一

```

## 結束後顯示畫面
### 執行後的圖片
###### 實際的程式碼



最終實際的程式碼
```javascript=
let points = [[-2,0],[-2.5,-1],[-3,-2],[-3.5,-4],[-4,-6],[0,-6],[2,-5],[2.5,-3.5],[3,-2],[4,0],[5,2],[2,5],[2,3],[0,2],[-0.5,0],[-2,2],[-4,2],[-2,0],[-2,0]];
var stroke_colors = "2d00f7-6a00f4-8900f2-a100f2-b100e8-bc00dd-d100d1-db00b6-e500a4-f20089".split("-").map(a=>"#"+a)
var fill_colors = "ffd6ff-e7c6ff-c8b6ff-b8c0ff-bbd0ff".split("-").map(a=>"#"+a)

function preload(){  //最早執行的程式碼
  ohno_sound = loadSound("sound/ohno.wav")
  bullet_sound = loadSound("sound/Launching wire.wav")
  // bg_sound = loadSound("sound/574197.wav")

}

var ball  //代表單一個物件，利用這個變數來做正在處理的物件
var balls =[]  //陣列，放所有的物件資料，物件倉庫，裡面儲存所有的物件資料
var bullet  //飛彈物件
var bullets=[]
var monster   //怪物物件
var monsters=[]
var score = 0
var shipP   //設定砲台的位置
function setup() {  //設定幽靈物件倉庫內的資料
  createCanvas(windowWidth, windowHeight);
  shipP = createVector(width/2,height/2)  //預設砲台的位置為視窗的中間(使用向量座標)
  //產生幾個物件
  for(var j=0;j<10;j=j+1)
  {
    ball = new Obj({})  //產生一個新的物件，"暫時"放入到ball變數中
    balls.push(ball)  //把ball物件放入到balls物件倉庫(陣列)中
  }
  for(var j=0;j<20;j=j+1)
  {
    monster = new Monster({})  //產生一個新的物件，"暫時"放入到monster變數中
    monsters.push(monster)  //把monster物件放入到monsters物件倉庫(陣列)中
  }


}

function draw() {  //每秒會執行60次次
  background(220);

  if(keyIsPressed){  //鍵盤是否被按下，如果有鍵盤被按下，keyPressed的值為true
    if(key=="ArrowLeft" || key=="a"){  //按下鍵盤的往左鍵
      shipP.x = shipP.x-5
    }
    if(key=="ArrowRight" || key=="d"){  //按下鍵盤的往右鍵
      shipP.x = shipP.x+5
    }
    if(key=="ArrowUp" || key=="w"){  //按下鍵盤的往上鍵
      shipP.y = shipP.y-5
    }
    if(key=="ArrowDown" || key=="s"){  //按下鍵盤的往下鍵
      shipP.y = shipP.y+5
    }    
  }
  for(let ball of balls){  //針對陣列變數，取出陣列內一個一個的物件
    ball.draw()
    ball.update()
    //+++++++++++++++由此判斷，每隻幽靈有沒有接觸每一個飛彈++++++++++++++++++++++
    for(let bullet of bullets){
      if(ball.isBallInRanger(bullet.p.x,bullet.p.y))  //判斷ball與bullet有沒有碰觸
      {
        score = score - 1     //分數扣一
        ohno_sound.play()
        balls.splice(balls.indexOf(ball),1)         //讓幽靈從幽靈倉庫內移除
        bullets.splice(bullets.indexOf(bullet),1)   //讓飛彈從飛彈倉庫內移除
      }
    }
  }


  for(let bullet of bullets){  //針對飛彈倉庫內的資料，一筆一筆的顯示出來
    bullet.draw()
    bullet.update()
  }

  for(let monster of monsters){  //針對怪物倉庫內的資料，一筆一筆的顯示出來
    if(monster.IsDead && monster.timenum>=6){
      monsters.splice(monsters.indexOf(monster),1) //讓怪物從怪物資料倉庫內移除
    }
    monster.draw()
    monster.update()
  
    for(let bullet of bullets){
      if(monster.isBallInRanger(bullet.p.x,bullet.p.y))  //判斷monster與bullet有沒有碰觸
      {
        score = score + 1     //分數加一
        // elephant_sound.play()
        // monsters.splice(monsters.indexOf(monster),1)   //讓怪物從怪物資料倉庫內移除
        monster.IsDead = true //已經被打到了，準備執行爆炸後的畫面
        bullets.splice(bullets.indexOf(bullet),1)   //讓飛彈從飛彈倉庫內移除
      }
    }

    // ++++++++++++++++++++++++++++++++++++++++++++++++++++++++++
  }

  textSize(50)
  text(score,50,50)
  //+++++劃出中間三角形的砲台++++++++++++++
  push()
    let dx = mouseX-width/2  //滑鼠座標到中心點座標的x軸距離
    let dy = mouseY-height/2 //滑鼠座標到中心點座標的y軸距離
    let angle = atan2(dy,dx)   //利用反tan算出角度


    // translate(width/2,height/2)  //砲台的位置  
    translate(shipP.x,shipP.y) //砲台的位置 ，使用shipP的向量值
    rotate(angle)    //讓三角形翻轉一個angle角度       
    noStroke()
    fill("#ffc03a")
    ellipse(0,0,60)  //劃出中間的圓
    fill("#ff0000")
    triangle(50,0,-25,-25,-25,25)  //劃出三角形
  pop()
  //+++++++++++++++++++++++++++++++

}

function mousePressed(){
  
  bullet  = new Bullet({})
  bullets.push(bullet)  //把這一筆資料放入飛彈倉庫
  bullet_sound.play()
}

function keyPressed(){
  if(key==" "){
    //新增(產生)一筆飛彈資料(還沒有顯示)
    bullet  = new Bullet({})
    bullets.push(bullet)  //把這一筆資料放入飛彈倉庫
    bullet_sound.play()
  }  

}
