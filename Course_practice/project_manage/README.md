# 网站搭建

1. 使用doscify搭建 下载 node.js
2. 命令行键入 npm i docsify-cli -g 安装 docsify-cli 工具
3. 在index.html文件中加入lodaSidebar: true 开启侧边栏
4. 新建_sidebar.md文件
5. 键入`<!-- docs/_sidebar.md -->`
6. 键入`*[首页](zh-cn/)`
7. 在index.html文件中加入lodaNavbar: true 开启导航栏
8. 新建_navbar.md文件
9. 键入`*[操作指南](guide)`


# 图片管理 
选择开源免费图片上传与管理工具Picgo
1. 安装[picgo](https://github.com/Molunerfinn/PicGo/releases)
2. 在github上新建图片仓库
3. 进入图床设置-GitHub图床
4. 获取[密钥](https://www.nexmaker.com/doc/1projectmanage/imageuploadservice.html)
5. 设定自定义域名**使用cdn加速**`https://cdn.jsdelivr.net/gh/用户名/图床仓库名`，完成Picgo设置
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/picgo_peizhi.png" width="70%">  

6. 获取图片链接，使用HTML标签`<img src="图片链接" width="60%">` 实现图片导入
<img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/pic_link.png" width="70%">
