---
title: Ubuntu开发环境配置
date: 2020-06-25 08:50:28
index_img: /resource/img/ubuntu.jpg
category: Linux
tags: Ubuntu
---

# 安装前准备

[镜像下载地址](https://ubuntu.com/download/desktop)

>下载后制作成启动盘就可以安装系统了

# 挂载磁盘

>


# 常用软件安装

### JetBrains 系列

- [Android Studio]()
- [CLion]()
- [DataGrip]()
- [Intelli IDEA]()
- [GoLand]()
- [Pycharm]()
- [WebStorm]()

### 浏览器

- [Chrome]()
- [Chromium]()
- [Firefox]()

### 开发工具
- [Redis Desktop Manager]()
- [Sublime Text]()
- [Sunlogin Client](https://sunlogin.oray.com/)
- [Teamview]()
- [Xmind ZEN]()
- [WPS](https://www.wps.cn/product/wpslinux)
- [DingTalk](https://github.com/nashaofu/dingtalk)

### 其他软件

- [electron-ssr](https://github.com/kinget007/electron-ssr)
- [FileZilla]()
- [lantern](https://github.com/getlantern/lantern)
- [Remmina]()
- [VirtualBox]()
- [网易云音乐]()
- [shougou输入法]()
- [SimpleScreenRecorder]()


### 社交软件
- [QQ](https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu)
- [Wechat](https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu)
- [TIM](https://gitee.com/wszqkzqk/deepin-wine-for-ubuntu)  

解决非中文系统中文乱码问题
```jshelllanguage
在/opt/deepinwine/tools/run.sh 和 run_v2.sh 中将 WINE_CMD 那一行修改为 WINE_CMD="LC_ALL=zh_CN.UTF-8 deepin-wine"
```
# 环境配置
- JDK
- Golang
- Docker
- Nodejs