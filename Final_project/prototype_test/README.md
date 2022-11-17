## 原型实验  
  
> ### 立体书结构探索
<iframe src="//player.bilibili.com/player.html?aid=901915605&bvid=BV1EP4y1S7LF&cid=873138420&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="false"> </iframe>

<iframe src="//player.bilibili.com/player.html?aid=816920838&bvid=BV1wG4y1h7Ua&cid=873137062&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>  

<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/zhi1.jpg" width="80%">  
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/zhi2.jpg" width="80%">  
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/zhi3.jpg" width="80%"> 

> ### 手持识别器制作  

为了在游戏过程中实现良好的交互效果，我们设计了一种手持识别器，使得玩家用其识别场景内的谜题，识别器播报语音（比如“这里有一条河挡住了去路，该怎么办呢？”）大大提升用户使用时的流畅性与生动性，增强游戏沉浸感。手持识别器硬件上由DFPlayer Mini、23mm圆形扬声器、ESP12F、RC522射频识别器组成。其功能框架如图所示  

<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/功能框架.png" width="100%">   
    

**主控芯片**  

本设计使用ESP12F作为主控芯片。  

**识别模块**  
使用RFID(Radio Frequency Identification)无接触自动识别技术，利用射频信号及其空间耦合传输特性，实现对静止的或移动中的待识别物品的自动识别。在本设计中使用RC522射频识别器识别故事书中的NFC电子标签，RC522将读取到的NFC电子标签UID通过SPI通信传输给主控芯片，Esp12F将UID转化为4位16进制数组，在主循环程序中依次将读取到的UID与预存的电子标签进行匹配，从而实现与故事书的无接触识别。  

**语音模块**    
DFPlayer Mini是一款小巧且价格低廉的MP3模块，在本设计中选用直径23mm的圆形扬声器，接驳到DFPlayer Mini的SPK引脚，将需要呈现的语音文件以序号命名到TF卡中，DFPlayer Mini通过串口通信将音频数据传输给主控芯片，当匹配到特定UID时，播放特定序号音频。  

**实物制作**  
采用立创EDA，绘制相关原理图PCB，进行电路板打样与焊接，实物如图所示：  

<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/Frame 2.png  " width="100%">    

**演示视频** 