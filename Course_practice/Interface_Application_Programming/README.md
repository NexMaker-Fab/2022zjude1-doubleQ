## Processing
### Learn the basic knowledge about processing and explore new tool similar with processing


### Do one demo in processing which can use mouse or keyboard to interactive
> #### demo简介  

利用Processing实现**锻炼手眼协调能力**的小游戏，从屏幕四面八方随机出现红蓝两种颜色的小球，玩家通过键盘改变中心大球的颜色，若大小球接触时颜色一致，加一分，反之减两分，由于每个小球有不一样的初速度与加速度，因此玩家需要进行一定程度的预判。  
> #### 技术原理 

Processing类的用法（没写完）
> #### processing代码解析    

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

> #### 参考资料
[Processing-对象（class）]("https://blog.csdn.net/liuxiao723846/article/details/82051791")  

[二锅头【Processing】雷霆战机]("https://www.bilibili.com/video/BV11Z4y1H7hw/?spm_id_from=333.999.0.0&vd_source=63a4e1f90cdfa681f6b6cfeefbbcc3eb")   

[Processing（1.5 一些有趣的库）]("https://zhuanlan.zhihu.com/p/349092863") 


### Do one demo in processing and arduino,which can communicate with each other
> #### demo介绍  

利用Processing和arduino实现**锻炼手指抓握力**的小游戏，在上个demo的基础上，利用弯曲传感器检测手指弯曲角度，通过手指抓握来替代键盘，可帮助中风患者手部锻炼  
> #### 技术原理 

Processing和arduino利用串口通信（没写完）
> #### 代码解析    

**processing代码解析**
```javascript  
import processing.serial.*;
Serial myport;
```  
加载processing.serial库  

```javascript  
myport = new Serial(this,"COM6",9600); 
```  
在void setup()中设定与arduino串口通信端口号与波特率    

```javascript  
  if (myport.available() > 0)//如果串口有数据
  { 
    int val = myport.read();//读取串口数据  
    if(val>60){status=0;}
    if(val<60){status=1;}
  }  
```    
在void draw()中使用myport.read()接收来自arduino发送至串口的数据 

**arduino代码解析**
```javascript  
int flexSensorPin = A0;//定义弯曲传感器引脚为A0
int flexSensorReading;//定义弯曲传感器直接输出的度数
int flexAngle;//定义转换后的角度
```  
一些初始定义   

```javascript  
void flex_detect()
{
    flexSensorReading = analogRead(flexSensorPin);
    flexAngle = map(flexSensorReading,620,860,0,90);
    Serial.write(flexAngle); 
    delay(500);
}
```
在void loop()中读取A0引脚的模拟量，将直接读数映射至0至90°，然后使用Serial.write将数据发送至串口  

> #### 参考资料
[Serial.print()函数与Serial.write()函数的区别]("https://blog.csdn.net/qq_36895854/article/details/88925939?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166731541816800192274182%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166731541816800192274182&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-88925939-null-null.142^v62^pc_search_tree,201^v3^control_1,213^v1^control&utm_term=serial.write%E5%92%8Cserial.print&spm=1018.2226.3001.4187")  

[Processing+Arduino互动编程]("https://blog.csdn.net/wangpuqing1997/article/details/105201551?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166731442916800180628440%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166731442916800180628440&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-105201551-null-null.142^v62^pc_search_tree,201^v3^control_1,213^v1^control&utm_term=arduino%20processing&spm=1018.2226.3001.4187")   

### Try to communicate with Kinect,Leapmotion or IOT platform


### Interpret and implement design and programming protocols to create a Graphic User Interface (GUI)