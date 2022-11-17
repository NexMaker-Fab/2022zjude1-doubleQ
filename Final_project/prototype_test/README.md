 
> ### 立体书结构探索
<iframe src="//player.bilibili.com/player.html?aid=901915605&bvid=BV1EP4y1S7LF&cid=873138420&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="false"> </iframe>

<iframe src="//player.bilibili.com/player.html?aid=816920838&bvid=BV1wG4y1h7Ua&cid=873137062&page=1" scrolling="no" border="0" frameborder="no" framespacing="0" allowfullscreen="true"> </iframe>  

<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/zhi1.jpg" width="80%">  
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/zhi2.jpg" width="80%">  

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
采用立创EDA，绘制相关原理图与PCB，进行电路板打样与焊接，如图所示： 
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/Schematic.png" width="100%"> 
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/PCB_PCB2.png" width="100%">    
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/shiwutu.jpg" width="100%">    

**PCB渲染**

在立创商城中找到想要的元器件编号，编辑下符号名称即可保存为自己的元件库，选择里面自带的3d模型封装，即可以实现step文件带元器件导出，再导入至Fusion360，与其他模组进行装配
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/pcb_fusion.jpg" width="100%">  

导出step文件，进入Keyshot渲染，渲染如图所示：  
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/Frame 2.png" width="100%">  

**演示视频**   
 

**参考资料** 
 
[如何将立创EDA中的元器件的原理图/封装和3D模型导入AD的库中_hzy_best的博客-CSDN博客_立创原理图怎么导入ad](https://blog.csdn.net/hzy_best/article/details/123767574)
  
[基于立创EDA Pro和KeyShot进行简单PCB渲染](https://www.bilibili.com/video/BV1gW4y1e79R/?zw&vd_source=63a4e1f90cdfa681f6b6cfeefbbcc3eb)
