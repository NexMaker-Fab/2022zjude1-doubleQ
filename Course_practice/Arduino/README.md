## Arduino  
> ### Arduino简介  
  
> ### Arduino 练习  

#### 练习简介  

使用Arduino对1602显示屏进行控制 

#### LCD1602介绍   

LCD1602液晶显示器是广泛使用的一种字符型液晶显示模块。所谓的1602是指显示的时候，有2行内容每行有16个字符，其实这类字符型产品都可以这样解读比如：lcd12864就是有128行64列。目前市面上字符液晶大多数是基于HD44780液晶芯片的，控制原理大多相同。因此基于HD44780写的液晶控制程序可以很方便适用于市面上大多数字符型液晶产品。  

<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/LCD引脚.png" width="80%">   
 
根据表中可知，15、16号引脚用于背光，三号引脚用于液晶显示偏压，因此15、16号不接线的情况下，1602仍可以正常显示文本信息。而偏压信号则可以决定液晶模块的电压控制的，可以决定液晶显示屏的通光量（明暗程度）。  

#### 接线图   
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/LCD接线.png" width="80%">    

#### 代码解析   

```C   
/*
 * LCD1602_bit4
 * LCD1602驱动显示Hello World
 */
 
int LCD1602_RS = 7;
int LCD1602_EN = 6;
int DB[4] = { 2, 3, 4, 5};

/*
 * LCD写命令
 */
void LCD_Command_Write(int command)
{
  int i, temp;
  digitalWrite( LCD1602_RS, LOW);
  digitalWrite( LCD1602_EN, LOW);

  temp = command & 0xf0;
  for (i = DB[0]; i <= 5; i++)
  {
    digitalWrite(i, temp & 0x80);
    temp <<= 1;
  }

  digitalWrite( LCD1602_EN, HIGH);
  delayMicroseconds(1);
  digitalWrite( LCD1602_EN, LOW);

  temp = (command & 0x0f) << 4;
  for (i = DB[0]; i <= 5; i++)
  {
    digitalWrite(i, temp & 0x80);
    temp <<= 1;
  }

  digitalWrite( LCD1602_EN, HIGH);
  delayMicroseconds(1);
  digitalWrite( LCD1602_EN, LOW);
}

/*
 * LCD写数据
 */
void LCD_Data_Write(int dat)
{
  int i = 0, temp;
  digitalWrite( LCD1602_RS, HIGH);
  digitalWrite( LCD1602_EN, LOW);

  temp = dat & 0xf0;
  for (i = DB[0]; i <= 5; i++)
  {
    digitalWrite(i, temp & 0x80);
    temp <<= 1;
  }

  digitalWrite( LCD1602_EN, HIGH);
  delayMicroseconds(1);
  digitalWrite( LCD1602_EN, LOW);

  temp = (dat & 0x0f) << 4;
  for (i = DB[0]; i <= 5; i++)
  {
    digitalWrite(i, temp & 0x80);
    temp <<= 1;
  }

  digitalWrite( LCD1602_EN, HIGH);
  delayMicroseconds(1);
  digitalWrite( LCD1602_EN, LOW);
}

/*
 * LCD设置光标位置
 */
void LCD_SET_XY( int x, int y )
{
  int address;
  if (y == 0)    address = 0x80 + x;
  else          address = 0xC0 + x;
  LCD_Command_Write(address);
}

/*
 * LCD写一个字符
 */
void LCD_Write_Char( int x, int y, int dat)
{
  LCD_SET_XY( x, y );
  LCD_Data_Write(dat);
}

/*
 * LCD写字符串
 */
void LCD_Write_String(int X, int Y, char *s)
{
  LCD_SET_XY( X, Y );    //设置地址
  while (*s)             //写字符串
  {
    LCD_Data_Write(*s);
    s ++;
  }
}

void setup (void)
{
  int i = 0;
  for (i = 2; i <= 7; i++)
  {
    pinMode(i, OUTPUT);
  }
  delay(100);
  LCD_Command_Write(0x28);//显示模式设置4线 2行 5x7
  delay(50);
  LCD_Command_Write(0x06);//显示光标移动设置
  delay(50);
  LCD_Command_Write(0x0c);//显示开及光标设置
  delay(50);
  LCD_Command_Write(0x80);//设置数据地址指针
  delay(50);
  LCD_Command_Write(0x01);//显示清屏
  delay(50);

}

void loop (void)
{
  LCD_Write_String(2, 0, "Hello World!");
  LCD_Write_String(6, 1, "--doubleQ");
}
```
#### 实验演示  

<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/LCD实物.png" width="80%">    

#### 参考资料  

> ### Arduino & Processing 手拉手demo 

#### demo简介  

利用Processing和arduino实现**锻炼手指抓握力**的小游戏，在上个demo的基础上，利用弯曲传感器检测手指弯曲角度，通过手指抓握来替代键盘，可帮助中风患者手部锻炼  
<!-- > #### 技术原理 

Processing和arduino利用串口通信（没写完） -->
#### 代码解析    

**processing代码**
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

**arduino代码**
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

#### 实验演示  

#### 参考资料  

[Serial.print()函数与Serial.write()函数的区别](https://blog.csdn.net/qq_36895854/article/details/88925939?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166731541816800192274182%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166731541816800192274182&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-88925939-null-null.142^v62^pc_search_tree,201^v3^control_1,213^v1^control&utm_term=serial.write%E5%92%8Cserial.print&spm=1018.2226.3001.4187)  

[Processing+Arduino互动编程](https://blog.csdn.net/wangpuqing1997/article/details/105201551?ops_request_misc=%257B%2522request%255Fid%2522%253A%2522166731442916800180628440%2522%252C%2522scm%2522%253A%252220140713.130102334..%2522%257D&request_id=166731442916800180628440&biz_id=0&utm_medium=distribute.pc_search_result.none-task-blog-2~all~sobaiduend~default-1-105201551-null-null.142^v62^pc_search_tree,201^v3^control_1,213^v1^control&utm_term=arduino%20processing&spm=1018.2226.3001.4187)   
