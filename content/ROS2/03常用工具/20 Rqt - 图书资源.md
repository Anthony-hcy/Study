---
title: "Rqt - 图书资源"
source: "https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/3.6_Rqt/"
author:
published:
created: 2026-08-13
description: "图书资源·古月居"
tags:
  - "clippings"
---
## RQT：模块化可视化工具

<iframe src="https://player.bilibili.com/player.html?aid=597395989&amp;bvid=BV16B4y1Q7jQ&amp;cid=746075589&amp;page=20&autoplay=0" width="800px" height="460px" frameborder="no" framespacing="0" allowfullscreen="true"></iframe>

ROS中的Rviz功能已经很强大了，不过有些场景下，我们可能更需要一些简单的模块化的可视化工具，比如只显示一个摄像头的图像，使用Rviz的话，难免会觉得操作有点麻烦。

此时，我们就会用到ROS提供的另外一种模块化可视化工具—— **rqt** 。

## rqt介绍

正如RQT的命名，它和Rviz一样，也是基于QT可视化工具开发而来，在使用前，我们需要通过这样一句指令进行安装，然后就可以通过rqt这个命令启动使用了。

```js
$ sudo apt install ros-humble-rqt
$ rqt
```

![image-20220528153119321](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.6_Rqt/image-20220528153119321.png)

类似这个界面一样，里边可以加载很多小模块，每个模块都可以实现一个具体的小功能，一些常用的功能如下：

## 日志显示

![image-20220528153332794](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.6_Rqt/image-20220528153332794.png)

## 图像显示

![image-20220528153354370](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.6_Rqt/image-20220528153354370.png)

## 发布话题数据/调用服务请求

![image-20220528153406339](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.6_Rqt/image-20220528153406339.png)

## 绘制数据曲线

![image-20220528153414784](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.6_Rqt/image-20220528153414784.png)

## 数据包管理

![image-20220528153423185](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.6_Rqt/image-20220528153423185.png)

## 节点可视化

![image-20220528153433016](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/3.6_Rqt/image-20220528153433016.png)

## 参考资料

[https://docs.ros.org/en/humble/Concepts/About-RQt.html](https://docs.ros.org/en/humble/Concepts/About-RQt.html)

[https://docs.ros.org/en/humble/Tutorials/Rqt-Console/Using-Rqt-Console.html](https://docs.ros.org/en/humble/Tutorials/Rqt-Console/Using-Rqt-Console.html)

[![图片1](https://book.guyuehome.com/ROS2/3.%E5%B8%B8%E7%94%A8%E5%B7%A5%E5%85%B7/image/footer.png)](https://www.guyuehome.com/)