# 网站搭建

- 使用doscify搭建 下载 node.js
- 命令行键入 npm i docsify-cli -g 安装 docsify-cli 工具
- 在index.html文件中加入lodaSidebar: true 开启侧边栏
- 新建_sidebar.md文件
- 键入`<!-- docs/_sidebar.md -->`
- 键入`*[首页](zh-cn/)`
- 在index.html文件中加入lodaNavbar: true 开启导航栏
- 新建_navbar.md文件
- 键入`*[操作指南](guide)`

# 图片管理 
选择开源免费图片上传与管理工具Picgo
1. 安装[picgo](https://github.com/Molunerfinn/PicGo/releases)
2. 在github上新建图片仓库
3. 进入图床设置-GitHub图床
4. 获取[密钥](https://www.nexmaker.com/doc/1projectmanage/imageuploadservice.html)
5. 完成Picgo配置  
   <img src="https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/picgo_peizhi.png" width="80%">
   ![图片测试](https://cdn.jsdelivr.net/gh/zimaStrawer/doubleQ_Image/chi.png)
6. 获取图片链接，使用markdown图片上传语法即可完成导入