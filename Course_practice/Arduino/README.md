### Do one demo in processing and arduino,which can communicate with each other
> #### demo介绍  

利用Processing和arduino实现**锻炼手指抓握力**的小游戏，在上个demo的基础上，利用弯曲传感器检测手指弯曲角度，通过手指抓握来替代键盘，可帮助中风患者手部锻炼  
<!-- > #### 技术原理 

Processing和arduino利用串口通信（没写完） -->
#### 代码解析    

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

#### 参考资料  

[Serial.print()函数与Serial.write()函数的区别](https://blog.csdn.net/qq_36895854/article/details/88925939?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166731541816800192274182%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166731541816800192274182&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-88925939-null-null.142^v62^pc_search_tree,201^v3^control_1,213^v1^control&utm_term=serial.write%E5%92%8Cserial.print&spm=1018.2226.3001.4187)  

[Processing+Arduino互动编程](https://blog.csdn.net/wangpuqing1997/article/details/105201551?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166731442916800180628440%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166731442916800180628440&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-105201551-null-null.142^v62^pc_search_tree,201^v3^control_1,213^v1^control&utm_term=arduino%20processing&spm=1018.2226.3001.4187)   
