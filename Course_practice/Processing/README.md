## Processing
> ### Processing简介 

Processing是一种用于图形设计的编程语言，并有与之配套的集成开发环境。它专注于图形和交互，被电子艺术和视觉设计从业者使用。Processing可以和其他的开源软件或设备结合起来使用，如leap motion，arduino等，因此常运用于新媒体和互动艺术作品中。

> ### 相似工具p5.JS简介  

p5.js是一个用于创意编码的开源的JavaScript库，面向艺术家、设计师、教育工作者、初学者使用编码。p5.js具有全套绘图功能，可以将整个浏览器页面设定为画布，利用文本、输入、视频、网络摄像头和声音进行HTML5互动与编写。  

> ### p5.JS demo  

#### demo简介  

尝试使用p5js工具进行创作
这是一个尝试使用鼠标键盘和麦克风进行交互的小游戏。  

#### 代码解析  

```html
<!DOCTYPE html>
<html>
  <head>
    <meta name="viewport" width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=0>
    <style> body {padding: 100; margin: 100;
    background:paleturquoise
    } </style>
  
   
    <script src="libraries/p5.js"></script>
    <script src="libraries/p5.dom.js"></script>
    <script src="libraries/p5.sound.js"></script>
    <script src="libraries/p5.collide2d.js"></script>
    <script src="sunchange.js"></script>
  </head>
  <body>
   
  </body>
</html>  
```
配置文件，选择调用的库  

```javascript
var waves = []
var C = []
var flows = []
var wind
var box
var mic
var cloudX = [];
var cloudY = [];
var dis = 0 
var stars = []
var circles = []
```  
定义变量  

```javascript  
class flow{
    constructor(){
        this.x = random(windowWidth);
		this.y = random(480,600);
		this.r = random(20,50);
    }
    show(){
		noStroke();
		fill(random(240,255));
		ellipse(this.x, this.y, this.r /2);
	}
} 
```  
绘制水中的波纹，用圆形表现水面波光粼粼的效果  

```javascript 
class wave {
	constructor(x, y, r){
		this.x = random(windowWidth);
		this.y = random(480,600);
		this.r = random(30,100);
		this.brightness = 100; 
	}
	
	changeColor(re,gre,blu){
        this.brightne = re;
        this.brightnes = gre;
        this.brightness = blu;
        
	}
	
	contains(px, py){
		let d = dist(px, py, this.x, this.y);
		if (d < this.r){
			return true;
		} else{
			return false;
		
		}
	}
	move (){
		this.x = this.x + wind;
		this.y = this.y + wind;
	}
	
	show(){
		noStroke();
		fill(this.brightne, this.brightnes,this.brightness);
		ellipse(this.x, this.y, this.r /2);
	}
}  
```  
设定天空的颜色切换，表现白天和夜晚  

```javascript 
class cloud {
    constructor(){
      this.x = random(windowWidth, windowWidth);
      this.y = random(-height);
      this.z = random(width);
      this.pz = this.z;
    }
      update() {
          this.x = this.x + wind;
          if (this.z < 1) {
              this.z = random(-width,width);
              this.x = width;
              this.y = random(-height, height);
              this.pz = this.z;
          }
      }
  
      show () {
          fill(255);
          noStroke();
  
          var sx = map(this.x / this.z, 0, 1, 0, width);
          var sy = map(this.y / this.z, 0, 1, 0, height);
  
          var r = map(this.z, 0, width, 16, 0);
          rect(sx, sy, 100,20, 120);   
  
      } 
    }

class star{
    constructor(){
        this.x = random(0,1500)
        this.y = random(0,300)
        this.r = random(2,7)
    }
    show(){
        frameRate(5);
        fill(random(240,255));
        ellipse(this.x,this.y,this.r);
    }
}  
```  
绘制天空中的云朵和星星  

```javascript 
function setup()

createCanvas(windowWidth,windowHeight)
noStroke();

mic = new p5.AudioIn()
mic.start()  
```  
设定画布大小，使用麦克风  

```javascript 
function draw() {

 wind = 0.5+mic.getLevel()*200;

 
	
var box = map(mouseY, 0, height, 500, 0);


background(box, 73, 100, 200);
translate(100 , 100 );
	for (var i = 0; i < C.length; i++) {
		C[i].update();
		C[i].show();
	}
//drop

//SUN
fill(255, 180);
ellipse(1000, mouseY, 100, 100);

fill(255, 10);
ellipse(1000, mouseY, 120, 120);
ellipse(1000, mouseY, 140, 140);
ellipse(1000, mouseY, 160, 160);
   //sea
//fill(200,200,220)
var topcolor = color(1, 73, 141);
var bottomcolor = color(144, 200, 226);
	for (var i = 10; i > 0; --i) {
		var size = map(i, 100, 0, 5, 400);
		fill(lerpColor(topcolor, bottomcolor, 1 - i / 10));
rect(-100,450,windowWidth,400);
    }  
```  
绘制太阳和海水，设定风力的大小  

```javascript 
//wave

for (var i = 0; i < 20; i++) {
  waves[i] = new wave();
}
for (let i = 0; i < waves.length; i++){
    if (waves[i].contains(mouseX, mouseY)) {
        waves[i].changeColor(255,255,255);
    } else {
        waves[i].changeColor(173,225,250);
    }
    
    waves[i].move();
    waves[i].show();
}
if(mouseY>600){
for (let i = 0; i < 40; i++) {
    
    stars[i].show();
}
}
```  
设置黑夜和白天随着鼠标的位置上下移动而切换的效果，设定鼠标到达600px后显示星星  

```javascript 
//boats
if(dis>1000){
    dis = -600
}

fill(255,255,255)
triangle(850-dis, 475, 935-dis, 350, 935-dis, 475);
triangle(945-dis, 475, 945-dis, 380, 1000-dis, 475);
fill(0,0,0)
beginShape();
vertex(850-dis,475);
vertex(880-dis,520);
vertex(970-dis,520);
vertex(1000-dis,475);
endShape(CLOSE); 
fill(100,100,100)
noStroke();
rect(850-dis,475,150,15);


for (var i = 0; i < 10; i++) {
    flows[i] = new flow();
  }
for (var i = 0; i < 10; i++) {
    
    flows[i].show();
}
```
绘制小船，利用collide2d库，在画布上绘制平面几何形态的小帆船  

```javascript
//clouds
for(var i = 0; i < 10; i ++){
  clouds(cloudX[i], cloudY[i]);
  cloudX[i] = cloudX[i] - wind;
      //clouds have jitter movement in y-axis
  cloudY[i] = cloudY[i] 

//
  if (cloudX[i] < -width-50) {
    cloudX[i] = width-50;
      }
}
//drops
for (var i=0; i<circles.length; i++) {
    circles[i].move();
    circles[i].show();
  }
}
```
设定云随着风力大小而移动  

```javascript  
function keyPressed(){
    dis += 50
}
function Circle() {
    this.x = mouseX;
    this.y = mouseY;
    this.r = random(5,7);
    this.R = random(8,20);
    this.n = random(10,50);

    this.speed = random(100, 150);
   
    this.expandSeed = random(100);

    this.show = function() {
        noStroke();
        fill(164,240,255);
        for(var i = 0; i < this.n; i++) {
            var x_ = this.x + this.R * cos( i * TWO_PI / this.n);
            var y_ = this.y + this.R * sin( i * TWO_PI / this.n);
            ellipse(x_, y_,  this.r);
        }
    }
    this.move = function() {        
        this.R += sin(this.expandSeed + frameCount / 10);    
    }
}

 function mousePressed(){
     if(mouseY>460){
    circles.push(new Circle());
     }
  }
```  
利用键盘和鼠标交互，完成对于小船的控制，按下任意键小船前进50px，点击鼠标出现海水波纹   

#### 实验演示  

#### 参考资料  

[openprocessing](https://openprocessing.org/)  

> ### Processing demo    

#### demo简介  

利用Processing实现**锻炼手眼协调能力**的小游戏，从屏幕四面八方随机出现红蓝两种颜色的小球，玩家通过键盘改变中心大球的颜色，若大小球接触时颜色一致，加一分，反之减两分，由于每个小球有不一样的初速度与加速度，因此玩家需要进行一定程度的预判。  
<!-- > #### 技术原理 

Processing类的用法（没写完） -->
#### processing代码解析    

**小球功能实现**
```javascript
Enemy [] enemys = new Enemy[10];
```
声明小球的对象数组，数量为10个

```javascript
for(int i = 0;i<enemys.length;i++)
{
    int ballc = int(random(0,1)<0.5 ? 1 : 0);//初始化小球颜色，随机为1或0
    float cx = random(-2000,2000);//初始化小球x坐标
    float cy = (random(0,1) < 0.5 ? 1 : -1)* sqrt(abs(random(1000001,4000000)-cx*cx));//初始化小球y坐标，使其初始位置在一圆环上
    enemys[i] = new Enemy(ballc,cx,cy);//將颜色，x坐标，y坐标三个属性赋予每一个小球
} 
```
在void setup()中给10个小球初始化状态

```javascript
class Enemy//定义Enemy类
{
  float c,x,y;//定义局域变量 颜色，坐标
  float velocity=random(1,2);//定义局域变量随机速度
  float accelerate=random(0.01,0.015);//定义局域变量随机加速度
  
  Enemy(int c,float x,float y)//构造函数
  {
    this.c=c;
    this.x=x;
    this.y=y;
  }
  void display()//小球出现
  {     
    if(c==1)
    {fill(22,184,243);}
    if(c==0)
    {fill(255,0,0);}
    noStroke();
    ellipse(x,y,30*(voice+2),30*(voice+2)); 
  }
  void update()//小球更新
  {
    x += -velocity*x/sqrt(x*x+y*y);
    y += -velocity*y/sqrt(x*x+y*y);  //根据速度实时更新位置 
    velocity += accelerate; //根据加速度实时更新速度 
  }
  void countbest()
  {
    bestScore = bestScore > (rad-180) ? bestScore:( rad-180) ;//得出最高分数
  }
  boolean checksame()//判断颜色是否相同
  {
    if(x*x+y*y<rad*rad && c==status)
    {
      return true;
    } return false;     
  }
  boolean checkwrong()//判断颜色是否相异
  {
    if(x*x+y*y<rad*rad && c!=status)
    {
      return true;
    } return false;     
  }  
  boolean reach()//判断是否大小球是否遭遇
  {
    if(x*x+y*y<rad*rad)
    {
      return true;
    } return false;
  }  
} 
``` 
构造Enemy类，定义小球一系列方法

```javascript
  for(int i = 0;i<enemys.length;i++)
  {
    enemys[i].display();
    enemys[i].update();
    enemys[i].countbest();    
    if(enemys[i].checksame() )
    {rad+=1;}  //如果颜色一致，大球半径+1
    if(enemys[i].checkwrong() )
    {rad-=2;}  //如果颜色相异，大球半径-2      
    if(enemys[i].reach() )
    {
      int ballc = int(random(0,1)<0.5 ? 1 : 0);
      float cx = random(-2000,2000);
      float cy = (random(0,1) < 0.5 ? 1 : -1)* sqrt(abs(random(1000001,4000000)-cx*cx));
      enemys[i] = new Enemy(ballc,cx,cy);     
    }  //如果小球接触到大球，则这个小球完成了他的使命，重新给予小球一个初始状态，让其重新出发    
  }
```
在void draw()中给给每个小球应用之前定义的方法  

**游戏框架实现**
```javascript
int status = 1;//大球的颜色状态
int rad = 200;//大球的初始半径
int bestScore = 0;//最好成绩
boolean gameOver = false;//游戏结束判定
boolean gameBegin = false;//游戏开始判定
```
在void setup()中进行初始定义  

```javascript
  translate(width/2,height/2);//把原点移动到中心   
   
  if(gameBegin == false)//如果还没开始
  {
    showBegin();
  }
  if(keyPressed && key == 'w' && gameBegin ==false)//如果开始界面按w
  {
    gameBegin = true;       
  }
  if(keyPressed && key == 'w'&& gameOver)///如果死亡界面按w
  {
    restart();
  }
  if(rad-180<=0)//如果死亡
  {
    showOver();
  }
  if(gameOver || gameBegin==false)
  {
    return;
  } 
  ```  
在void draw()中判断游戏运行状态，初始gameBegin = false，表示为游戏未开始，运行showBegin()函数，显示游戏欢迎界面；当游戏未开始时键入w，则gameBegin = true，表示游戏开始；当分数为0，游戏结束，运行showOver()函数，将gameOver置为true并显示游戏结束界面，当游戏结束时键入w，运行restart()函数，将gameOver重置回false，以便下一次游戏循环

 ```javascript
void showBegin()
{
  background(22,184,243);   
  textAlign(CENTER); 
  fill(255);
  textSize(50);
  text("Welcome to finger game",0,0);
  textSize(20);
  text("Press w to start",0,100);
}

void restart()
{
  gameOver = false;
  rad = 200;
  for(int i = 0;i<enemys.length;i++)
  {
    int ballc = int(random(0,1)<0.5 ? 1 : 0);
    float cx = random(-2000,2000);
    float cy = (random(0,1) < 0.5 ? 1 : -1)* sqrt(abs(random(1000001,4000000)-cx*cx));
    enemys[i] = new Enemy(ballc,cx,cy);
  }
}

void showOver()
{
    gameOver = true; 
    background(255,0,0);   
    textAlign(CENTER); 
    fill(255);
    textSize(50);
    text(bestScore,0,0);
}
 ```
创建各种游戏状态函数，以便调用  

**音乐功能实现** 

```javascript
import ddf.minim.*;
Minim minim;
AudioPlayer bgm;
float voice;//用来储存音乐的音量  
```
导入ddf.minim库  
打开Processing，选择 【Tools】-【Add Tool】，打开Contribution Manager  

<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/processingku1.png" width="70%">  

然后选择Libraries，搜索minim。选择Minim库，再选择Install。等待安装完成即可  

<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/processingku2.png" width="70%">   

```javascript
minim = new Minim(this); 
bgm = minim.loadFile("bgm.mp3", 1024); //jingle.mp3：此处加载的音乐名称需要与data文件中一致，1024:提取音乐频率,数值为2的幂次方                           
bgm.loop();                             
```
在void setup()中进行一些初始定义 

```javascript  
void display()
{     
    if(c==1)
    {fill(22,184,243);}
    if(c==0)
    {fill(255,0,0);}
    noStroke();
    ellipse(x,y,30*(voice+2),30*(voice+2)); 
}
```
在class Enemy()中将小球半径与音量大小关联，使游戏更加生动，巧妙绝伦  
   
#### 参考资料  

[Processing-对象（class）](https://blog.csdn.net/liuxiao723846/article/details/82051791)  

[二锅头【Processing】雷霆战机](https://www.bilibili.com/video/BV11Z4y1H7hw/?spm_id_from=333.999.0.0&vd_source=63a4e1f90cdfa681f6b6cfeefbbcc3eb)   

[Processing（1.5 一些有趣的库）](https://zhuanlan.zhihu.com/p/349092863) 

#### 实验演示 


<!-- ### Try to communicate with Kinect,Leapmotion or IOT platform -->


<!-- ### Interpret and implement design and programming protocols to create a Graphic User Interface (GUI) -->