## Processing
### Learn the basic knowledge about processing and explore new tool similar with processing


### Do one demo in processing which can use mouse or keyboard to interactive
> #### demo简介  
利用Processing实现**锻炼手眼协调能力**的小游戏
> #### 游戏介绍  
从屏幕四面八方随机出现红蓝两种颜色的小球，玩家通过键盘改变中心大球的颜色，若大小球接触时颜色一致，加一分，反之减两分，由于每个小球有不一样的初速度与加速度，因此玩家需要进行一定程度的预判。
> #### 代码解析  
**小球功能实现**
```javascript
Enemy [] enemys = new Enemy[10];
```
定义小球的对象数组，数量为10个

```javascript
for(int i = 0;i<enemys.length;i++)
{
    int ballc = int(random(0,1)<0.5 ? 1 : 0);初始化小球颜色，随机为1或0
    float cx = random(-2000,2000);初始化小球x坐标
    float cy = (random(0,1) < 0.5 ? 1 : -1)* sqrt(abs(random(1000001,4000000)-cx*cx));初始化小球y坐标，使其初始位置在一圆环上
    enemys[i] = new Enemy(ballc,cx,cy);赋予每一个小球，颜色，x坐标，y坐标属性
} 
```
给10个小球初始化状态

### Do one demo in processing and arduino ,which can communicate with each other


### Try to communicate with Kinect,Leapmotion or IOT platform


### Interpret and implement design and programming protocols to create a Graphic User Interface (GUI)